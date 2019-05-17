---
title: Azure Stack'te bir sanal makineye bir Java WAR dağıtma | Microsoft Docs
description: Java War dosyası, bir sanal makineye Azure Stack'te dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: dbf6083ff81d045d92d488eda5cfab757093bb7e
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782908"
---
# <a name="how-to-deploy-a-java-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Java web uygulaması dağıtma

Azure stack'teki Python Web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, sunucu, Python web uygulamanızı barındırmak için sunucu yapılandırma ve ardından uygulamanızı dağıtmak ayarında izleyeceğiniz adımlar bakar.

Bu makalede, yükleme ve Azure stack'teki Linux VM'de bir Apache Tomcat sunucusunu yapılandırma ve sonra bir Java Web uygulaması (WAR) kaynak dosyası Server'a yükleme yol gösterir. Bir WAR dosyası, bir koleksiyon sınıfları, metin, resimler, XML ve HTML gibi Java kaynak ve bir web uygulaması teslim etmek için kullanılan diğer kaynakları içeren sıkıştırılmış bir dosya JAR dosyalarını dağıtmak için kullanılır.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure Stack'te ayarlamak, bir VM oluşturun. Bağlantısındaki [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ dikey penceresinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), web sayfaları sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür.  |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanır. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı için makinenizi sağlar.   |
    | 8080 | Özel | Apache Tomcat hizmeti için varsayılan bağlantı noktası 8080'dir. Bir üretim sunucusu için 80 ve 443, trafiği yönlendirmek isteyebilirsiniz. |

## <a name="install-java"></a>Java'yı yükleme

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).
2. Sanal makinenizin üzerinde bash isteminde, aşağıdaki komutları yazın:

    ```bash  
        sudo apt-get install default-jdk
    ```

3. Yüklemenizi doğrulayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
        java -version
    ```

## <a name="install-and-configure-tomcat"></a>Yükleme ve Tomcat yapılandırma

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).

2. Tomcat kullanıcı oluşturun.
    - İlk olarak, yeni Tomcat grubu oluşturun:
        ```bash  
            sudo groupadd tomcat
        ```
     
    - İkinci olarak, Tomcat kullanıcı oluşturun. Giriş dizini ile tomcat grubuna bu kullanıcı ekleme `/opt/tomcat`. Bu dizine Tomcat dağıtacaksınız:
        ```bash  
            sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
        ```

3. Tomcat yükleyin.
    - İlk olarak, URL hedefi Tomcat 8'in en son sürümü için Tomcat 8 indirme sayfasından alın:  [http://tomcat.apache.org/download-80.cgi](http://tomcat.apache.org/download-80.cgi)
    - Bağlantıyı kullanarak en son sürümü indirmek için ikinci, kullanıcı curl.

        ```bash  
            cd /tmp 
            curl -O <URL for the tar for the latest version of Tomcat 8>
        ```

    - Üçüncü olarak, Tomcat için yükleme `/opt/tomcat` dizin. Bir klasör oluşturun.  Arşiv açın:

        ```bash  
            sudo mkdir /opt/tomcat
            sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
            sudo chown -R tomcat webapps/ work/ temp/ logs/
        ```

4. Tomcat izinlerini güncelleştirin.

    ```bash  
        sudo chgrp -R tomcat /opt/tomcat
        sudo chmod -R g+r conf
        sudo chmod g+x conf
    ```

5. Oluşturma bir `systemd` hizmet dosyası. Böylece, Tomcat hizmet olarak çalıştırabilirsiniz.

    - Tomcat, Java yüklediğiniz bilmesi gerekir. Bu yol, yaygın olarak adlandırılır **JAVA_HOME**. Konumun çalıştırarak bulabilirsiniz:

        ```bash  
            sudo update-java-alternatives -l
        ```

        Bu, aşağıdaki gibi oluşturur:

        ```Text  
            Output
            java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
        ```

        **JAVA_HOME** değişken değeri oluşturulmasını çıkış yolu alma ve ekleyerek `/jre`. Örneğin, yukarıdaki örneği kullanarak: `/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre`

    - Systemd hizmeti dosyası oluşturmak için sunucunuzdan değeri kullanın.

        ```bash  
            sudo nano /etc/systemd/system/tomcat.service
        ```

    - Aşağıdaki içeriği hizmet dosyanıza yapıştırın. Değerini değiştirmek **JAVA_HOME** değerle eşleşecek şekilde gerekli sisteminizde bulunan durumunda. CATALINA_OPTS içinde belirtilen bellek ayırma ayarlarını değiştirmek isteyebilirsiniz:

        ```Text  
            [Unit]
            Description=Apache Tomcat Web Application Container
            After=network.target
            
            [Service]
            Type=forking
            
            Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
            Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
            Environment=CATALINA_HOME=/opt/tomcat
            Environment=CATALINA_BASE=/opt/tomcat
            Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
            Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
            
            ExecStart=/opt/tomcat/bin/startup.sh
            ExecStop=/opt/tomcat/bin/shutdown.sh
        
            User=tomcat
            Group=tomcat
            UMask=0007
            RestartSec=10
            Restart=always
            
            [Install]
            WantedBy=multi-user.target
        ```

    - Dosyayı kaydedin ve kapatın.
    - Bu hizmet dosyanızı bilebilmesi systemd arka plan programı yeniden yükleyin:

        ```bash  
            sudo systemctl daemon-reload
        ```

    - Tomcat hizmetini başlatın 

        ```bash  
            sudo systemctl start tomcat
        ```

    - Yazarak hatasız başlatıldığını doğrulayın:

        ```bash  
            ssudo systemctl status tomcat
        ```

6. Tomcat sunucusunun doğrulayın. Tomcat, geleneksel isteklerini kabul etmek için bağlantı noktası 8080 kullanır. Trafik, bağlantı noktası için aşağıdaki komutu çalıştırarak izin ver:

    ```bash  
        sudo ufw allow 8080
    ```

    Henüz eklediyseniz **gelen bağlantı noktası kuralları** Azure Stack VM'niz için daha sonra bu şimdi ekleyin. Daha fazla bilgi için [VM oluşturma](#create-a-vm).

7. Azure Stack aynı ağda bir tarayıcı açın ve sunucunuzun açın: `yourmachine.local.cloudapp.azurestack.external:8080`

    ![Bir Azure Stack VM üzerinde Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat.png)

    Apache Tomcat sayfanın sunucunuzdaki yükler. Ardından, sunucu, sunucu durumunu, Yöneticisi uygulama ve ana bilgisayar yöneticisi erişmesine olanak sağlamak için yapılandırır.

8. Hizmet dosya sunucunuzu yeniden başlattığınızda, Tomcat otomatik olarak başlatılmasını etkinleştirin:

    ```bash  
        sudo systemctl enable tomcat
    ```

9. Web yönetimi arabirimi erişmesine olanak Tomcat sunucusunu yapılandırın. Düzen `tomcat-users.xml` ve oturum açın, böylece rol ve kullanıcı tanımlayın. Erişmek için kullanıcı tanımlı `manager-gui` ve `admin-gui`.

    ```bash  
        sudo nano /opt/tomcat/conf/tomcat-users.xml
    ```

    - Aşağıdaki öğeyi ekleyin `<tomcat-users>` bölümü:

    ```XML  
        <role rolename="tomcat"/>
        <user username="<username>" password="<password>" roles="tomcat,manager-gui,admin-gui"/>
    ```

    - Örneğin, son dosyanız aşağıdakine benzer:

    ```XML  
        <tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
         <role rolename="tomcat"/>
        <user username="tomcatuser" password="changemepassword" roles="tomcat,manager-gui,admin-gui"/>
        </tomcat-users>
    ```

    - Dosyayı kaydedin ve kapatın.


10. Tomcat erişimi kısıtlayan **Manager** ve **ana bilgisayar yöneticisi** sunucudan gelen bağlantılar için uygulamalar. Azure Stack'te bir VM üzerinde Tomcat yükleme olduğundan, bu kısıtlamayı kaldırmak isteyeceksiniz. Bu uygulamalar üzerinde IP adresi sınırlamaları uygun düzenleyerek değiştirme `context.xml` dosyaları.

    - Güncelleştirme `context.xml` Yöneticisi uygulama:

        ```bash  
            sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
        ```

    - IP adresi kısıtlaması herhangi bir gelen bağlantılara izin verin veya Tomcat için bağlanmak için kullandığınız makinenin IP adresini ekleyin açıklama satırı yapın.

        ```XML  
        <Context antiResourceLocking="false" privileged="true" >
          <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
                 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
        </Context>
        ```

    - Dosyayı kaydedin ve kapatın.

    - Güncelleştirme `context.xml` benzer bir güncelleştirme ile ana bilgisayar yöneticisi uygulama:

        ```bash  
            sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
        ```

    - Dosyayı kaydedin ve kapatın.

11. Sunucuyu değişikliklerle güncelleştirmek için Tomcat hizmetini yeniden başlatın:

    ```bash  
        sudo systemctl restart tomcat
    ```

12. Azure Stack aynı ağda bir tarayıcı açın ve sunucunuzun açın: `yourmachine.local.cloudapp.azurestack.external:8080`.

    - Tomcat sunucusunun durumunu gözden geçirin ve erişiminiz doğrulamak için sunucu durumu seçin.
    - Tomcat kimlik bilgilerinizle oturum açın.

![Bir Azure Stack VM üzerinde Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat-management-app.png)

## <a name="create-an-app"></a>Uygulama oluşturma

Tomcat için dağıtılacak War dosyası oluşturmanız gerekir. Yalnızca ortamınızın denetlemek istiyorsanız, War Apache TomCat sitedeki bir örnek bulabilirsiniz: [örnek uygulaması](https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/).

Azure'da Java uygulamaları geliştirme hakkında yönergeler için bkz. [azure'da Java uygulamaları oluşturup dağıtmayı](https://azure.microsoft.com/develop/java/).

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).
1. Uygulama paketinizle birlikte sunucuyu güncelleştirmek için Tomcat hizmetini durdurun:

    ```bash  
        sudo systemctl stop tomcat
    ```

2.  Böylece webapps klasörüne yazabildiğinizi FTP kullanıcı tomcat grubuna ekleyin. Kullanıcı, FTP olduğu, VM'niz Azure Stack'te oluştururken tanımlayın.

    ```bash  
        sudo usermod -a -G tomcat <VM-user>
    ```

3. Webapps klasörüne temizleyin ve ardından, yeni veya güncelleştirilmiş WAR yüklemek için FileZilla ile sanal makinenize bağlanın. FileZila kullanma ile ilgili yönergeler için bkz: [Connect SFTP FileZilla ile ile](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-sftp-with-filezilla).
    - NET `TOMCAT_HOME/webapps`.
    - Eklemek için WAR ` TOMCAT_HOME/webapps`, örneğin `/opt/tomcat/webapps/`.

4.  Tomcat, otomatik olarak genişletir ve uygulamayı dağıtır. Daha önce oluşturduğunuz DNS adı ile görüntüleyebilirsiniz. Örneğin:

    ```HTTP  
       http://yourmachine.local.cloudapp.azurestack.external:8080/sample

## Next steps

- Learn more about how to [Develop for Azure Stack](azure-stack-dev-start.md)
- Learn about [common deployments for Azure Stack as IaaS](azure-stack-dev-start-deploy-app.md).
- To learn the Java programming language and find additional resources for Java, see [Java.com](https://www.java.com).