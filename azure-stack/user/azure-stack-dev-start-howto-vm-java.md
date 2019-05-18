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
ms.openlocfilehash: 1738d106a0688518f7a739d3fb02ec1b16c2b8b9
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838366"
---
# <a name="deploy-a-java-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Java web uygulaması dağıtma

Bir sanal makine (VM) Azure Stack'te Python web uygulamanızı barındırmak için oluşturabilirsiniz. Bu makalede, yükleme ve Azure stack'teki Linux VM'de bir Apache Tomcat sunucusunu yapılandırın. Bir Java Web uygulaması kaynak (WAR) dosyası sonra sunucuya yükleyin. Bir WAR dosyası Java arşiv (JAR) dosyaları, sınıflar, metin, resimler, XML ve HTML gibi Java kaynak ve bir web uygulaması sunmak için kullanılan diğer kaynaklar içeren sıkıştırılmış bir koleksiyonunu dağıtmak için kullanılır.

## <a name="create-a-vm"></a>VM oluşturma

1. ' Ndaki yönergeleri takip ederek, Azure Stack sanal ayarlama [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ bölmesinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), Web sayfalarını sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür. |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanın. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), makinenizde bir grafik kullanıcı arabirimini kullanarak bir Uzak Masaüstü bağlantısı sağlar.   |
    | 8080 | Özel | Apache Tomcat hizmeti için varsayılan bağlantı noktası. Bir üretim sunucusu için 80 ve 443 üzerinden trafiğiniz yol. |

## <a name="install-java"></a>Java'yı yükleme

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTY SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

2. Sanal makinenizin üzerinde bash isteminde aşağıdaki komutu çalıştırın:

    ```bash  
        sudo apt-get install default-jdk
    ```

3. Yüklemenizi doğrulayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutu çalıştırın:

    ```bash  
        java -version
    ```

## <a name="install-and-configure-tomcat"></a>Yükleme ve Tomcat yapılandırma

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTY SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. Aşağıdakileri yaparak Tomcat kullanıcı oluşturun:

    a. Aşağıdaki komutu çalıştırarak yeni Tomcat grubu oluşturun:

    ```bash  
        sudo groupadd tomcat
    ```
     
    b. Yeni Tomcat kullanıcı oluşturun. Giriş dizini ile Tomcat grubuna bu kullanıcı ekleme */opt/tomcat*. Bu dizine Tomcat dağıttığınız:

    ```bash  
        sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
    ```

1. Tomcat, aşağıdakileri yaparak yükleyin:

    a. Tomcat 8'den en son sürümü için hedefi URL'sini alma [Tomcat 8 indirme sayfası](http://tomcat.apache.org/download-80.cgi).

    b. Bağlantıyı kullanarak en son sürümü indirmek için cURL kullanın. Aşağıdaki komutları çalıştırın:

    ```bash  
        cd /tmp 
        curl -O <URL for the tar for the latest version of Tomcat 8>
    ```

    c. Yükleme için Tomcat */opt/tomcat* dizin. Klasör oluşturun ve ardından arşiv açın:

    ```bash  
        sudo mkdir /opt/tomcat
        sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
        sudo chown -R tomcat webapps/ work/ temp/ logs/
    ```

1. Aşağıdaki komutları çalıştırarak izinleri tomcat güncelleştirin:

    ```bash  
        sudo chgrp -R tomcat /opt/tomcat
        sudo chmod -R g+r conf
        sudo chmod g+x conf
    ```

1. Oluşturma bir *systemd* Tomcat hizmet olarak çalıştırabilmeniz için dosya, hizmet.

   a. Tomcat, Java yüklediğiniz bilmesi gerekir. Bu yol, yaygın olarak adlandırılır *JAVA_HOME*. Konumun çalıştırarak bulabilirsiniz:

    ```bash  
        sudo update-java-alternatives -l
    ```

    Bu, aşağıdaki gibi oluşturur:

    ```Text  
        Output
        java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
    ```

    Oluşturulabilir *JAVA_HOME* çıkış yolu alma ve ekleyerek değişken değeri */jre*. Örneğin, önceki örnekte kullanarak */usr/lib/jvm/java-1.8.0-openjdk-amd64/jre*.

    b. Systemd hizmeti dosyası oluşturmak için değer sunucunuzdan kullanın:

    ```bash  
        sudo nano /etc/systemd/system/tomcat.service
    ```

    c. Aşağıdaki içeriği hizmet dosyanıza yapıştırın. Değerini değiştirmek *JAVA_HOME*, gerekirse, sisteminizde bulunan değer ile eşleşmelidir. CATALINA_OPTS içinde belirtilen bellek ayırma ayarlarını değiştirmek isteyebilirsiniz:

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

    d. Dosyayı kaydedin ve kapatın.

    e. Bu hizmet dosyanızı bilebilmesi systemd arka plan programı yeniden yükleyin:

    ```bash  
        sudo systemctl daemon-reload
    ```

    f. Tomcat hizmetini başlatın: 

    ```bash  
        sudo systemctl start tomcat
    ```

    g. Girerek hatasız başlatıldığını doğrulayın:

    ```bash  
        sudo systemctl status tomcat
    ```

1. Tomcat sunucusunun doğrulayın. Tomcat, geleneksel isteklerini kabul etmek için bağlantı noktası 8080 kullanır. Trafik, bağlantı noktası için aşağıdaki komutu çalıştırarak izin ver:

    ```bash  
        sudo ufw allow 8080
    ```

    Henüz eklediyseniz *gelen bağlantı noktası kuralları* Azure Stack VM'niz için bunları şimdi ekleyin. Daha fazla bilgi için [VM oluşturma](#create-a-vm).

1. Azure Stack aynı ağda bir tarayıcı açın ve sunucunuzun açın *yourmachine.local.cloudapp.azurestack.external:8080*.

    ![Bir Azure Stack VM üzerinde Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat.png)

    Apache Tomcat sayfanın sunucunuzdaki yükler. Ardından, sunucu, sunucu durumunu, Yöneticisi uygulama ve ana bilgisayar yöneticisi erişmesine olanak sağlamak için yapılandırın.

1. Hizmet dosya sunucunuzu yeniden başlattığınızda, Tomcat otomatik olarak başlatılmasını etkinleştirin:

    ```bash  
        sudo systemctl enable tomcat
    ```

1. Kendiniz izin verecek şekilde erişmek için bir web yönetimi arabirimi, Tomcat sunucusunun yapılandırın. 

   a. Düzen *tomcat users.xml* dosya ve oturum açın, böylece rol ve kullanıcı tanımlayın. Erişmek için kullanıcı tanımlı `manager-gui` ve `admin-gui`.

    ```bash  
        sudo nano /opt/tomcat/conf/tomcat-users.xml
    ```

   b. Aşağıdaki öğeleri ekleyin `<tomcat-users>` bölümü:

    ```XML  
        <role rolename="tomcat"/>
        <user username="<username>" password="<password>" roles="tomcat,manager-gui,admin-gui"/>
    ```

    Örneğin, son dosyanız aşağıdakine benzer:

    ```XML  
        <tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
        <role rolename="tomcat"/>
        <user username="tomcatuser" password="changemepassword" roles="tomcat,manager-gui,admin-gui"/>
        </tomcat-users>
    ```

    c. Dosyayı kaydedin ve kapatın.

1. Tomcat erişimi kısıtlayan *Manager* ve *ana bilgisayar yöneticisi* sunucudan gelen bağlantılar için uygulamalar. Azure Stack'te bir VM üzerinde Tomcat yüklemekte için bu kısıtlamayı kaldırmak isteyebilirsiniz. Bu uygulamalar üzerinde IP adresi sınırlamaları uygun düzenleyerek değiştirme *context.xml* dosyaları.

    a. Güncelleştirme *context.xml* Manager uygulamasında:

    ```bash  
        sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
    ```

    b. IP adresi kısıtlaması herhangi bir gelen bağlantılara izin verin veya Tomcat için bağlanmak için kullandığınız makinenin IP adresini ekleyin açıklama satırı yapın.

    ```XML  
    <Context antiResourceLocking="false" privileged="true" >
        <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
                allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
    </Context>
    ```

    c. Dosyayı kaydedin ve kapatın.

    d. Güncelleştirme *context.xml* benzer bir güncelleştirme ile ana bilgisayar yöneticisi uygulama:

    ```bash  
        sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
    ```

    e. Dosyayı kaydedin ve kapatın.

1. Sunucu değişikliklerle güncelleştirmek için Tomcat hizmetini yeniden başlatın:

    ```bash  
        sudo systemctl restart tomcat
    ```

1. Azure Stack aynı ağda bir tarayıcı açın ve sunucunuzun açın: *yourmachine.local.cloudapp.azurestack.external:8080*.

    a. Tomcat sunucusunun durumunu gözden geçirin ve erişime sahip olduğunu doğrulamak için **sunucu durumu**.

    b. Tomcat kimlik bilgilerinizle oturum açın.

    ![Bir Azure Stack VM üzerinde Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat-management-app.png)

## <a name="create-an-app"></a>Uygulama oluşturma

Tomcat için dağıtılacak War dosyası oluşturmanız gerekir. Yalnızca ortamınızın denetlemek istiyorsanız, WAR örnek bulabilirsiniz, [Apache Tomcat site](https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/).

Azure'da Java uygulamaları geliştirme hakkında yönergeler için bkz. [azure'da Java uygulamaları oluşturup dağıtmayı](https://azure.microsoft.com/develop/java/).

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTY SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. Sunucu uygulama paketinizle birlikte güncelleştirmek için Tomcat hizmeti durdurun:

    ```bash  
        sudo systemctl stop tomcat
    ```

1. Webapps klasörüne yazmak için FTP kullanıcı Tomcat grubuna ekleyin. FTP kullanıcı Azure Stack'te VM'nizi oluşturduğunuzda, tanımladığınız bir kullanıcıdır.

    ```bash  
        sudo usermod -a -G tomcat <VM-user>
    ```

1. Webapps klasörüne temizleyin ve ardından, yeni veya güncelleştirilmiş WAR yüklemek için FileZilla ile sanal makinenize bağlanın. Yönergeler için [Connect SFTP FileZilla ile ile](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-sftp-with-filezilla).

    a. NET *TOMCAT_HOME/webapps*.

    b. Eklemek için WAR *TOMCAT_HOME/webapps* (örneğin, */opt / / webapps/tomcat*).

1.  Tomcat, otomatik olarak genişletir ve uygulamayı dağıtır. Daha önce oluşturduğunuz DNS adını kullanarak görüntüleyebilirsiniz. Örneğin:

    ```HTTP  
       http://yourmachine.local.cloudapp.azurestack.external:8080/sample

## Next steps

- Learn more about how to [develop for Azure Stack](azure-stack-dev-start.md).
- Learn about [common deployments for Azure Stack as IaaS](azure-stack-dev-start-deploy-app.md).
- To learn the Java programming language and find additional resources for Java, see [Java.com](https://www.java.com).
