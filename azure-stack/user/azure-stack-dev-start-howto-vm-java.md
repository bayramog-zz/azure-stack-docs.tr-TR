---
title: Azure Stack bir Java WAR 'i sanal makineye dağıtma | Microsoft Docs
description: Azure Stack içindeki bir Java WAR 'i sanal makineye dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 2c9c43439872a19ba590fb22059969f9a7c742f0
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824455"
---
# <a name="deploy-a-java-web-app-to-a-vm-in-azure-stack"></a>Azure Stack bir Java Web uygulamasını bir VM 'ye dağıtma

Python web uygulamanızı Azure Stack barındırmak için bir sanal makine (VM) oluşturabilirsiniz. Bu makalede, Azure Stack bir Linux sanal makinesine Apache Tomcat sunucusu yükleyip yapılandırırsınız. Daha sonra bir Java Web uygulaması kaynağı (WAR) dosyasını sunucuya yüklersiniz. Bir WAR dosyası, bir Java Arşivi (JAR) dosyaları koleksiyonunu, sınıflar, metin, resimler, XML ve HTML gibi Java kaynaklarını içeren sıkıştırılmış dosyaları ve bir Web uygulaması teslim etmek için kullanılan diğer kaynakları dağıtmak için kullanılır.

## <a name="create-a-vm"></a>VM oluşturma

1. [Azure Stack ' de bir Web uygulaması barındırmak için bir Linux sanal makinesi dağıtma](azure-stack-dev-start-howto-deploy-linux.md)bölümündeki yönergeleri izleyerek sanal makineyi Azure Stack ' de ayarlayın.

2. VM ağı bölmesinde, aşağıdaki bağlantı noktalarına erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), sunuculardan Web sayfalarını teslim etmek için kullanılan protokoldür. İstemciler bir DNS adı veya IP adresi ile HTTP aracılığıyla bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), bir güvenlik sertifikası gerektiren ve şifreli bilgi iletimi sağlayan güvenli bir HTTP sürümüdür. |
    | 22 | SSH | Secure Shell (SSH), güvenli iletişimler için şifreli bir ağ protokolüdür. Bu bağlantıyı, sanal makineyi yapılandırmak ve uygulamayı dağıtmak için bir SSH istemcisiyle birlikte kullanırsınız. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), uzak masaüstü bağlantısının makinenizde bir grafik kullanıcı arabirimi kullanmasına izin verir.   |
    | 8080 | Özel | Apache Tomcat hizmeti için varsayılan bağlantı noktası. Bir üretim sunucusu için, trafiğinizi 80 ve 443 ile yönlendirmenize sahip olursunuz. |

## <a name="install-java"></a>Java 'Yı yükler

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTY Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

2. SANAL makinenizin Bash isteminde aşağıdaki komutu çalıştırın:

    ```bash  
        sudo apt-get install default-jdk
    ```

3. Yüklemenizi doğrulayın. SSH oturumunuzda sanal makinenize hala bağlı, aşağıdaki komutu çalıştırın:

    ```bash  
        java -version
    ```

## <a name="install-and-configure-tomcat"></a>Tomcat 'i yükleyip yapılandırma

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTY Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. Aşağıdakileri yaparak bir Tomcat kullanıcısı oluşturun:

    a. Aşağıdaki komutu çalıştırarak yeni bir Tomcat grubu oluşturun:

    ```bash  
        sudo groupadd tomcat
    ```
     
    b. Yeni bir Tomcat kullanıcısı oluşturun. Bu kullanıcıyı, */seçenek/Tomcat*giriş diziniyle birlikte Tomcat grubuna ekleyin. Tomcat 'i bu dizine dağıtırsınız:

    ```bash  
        sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
    ```

1. Aşağıdakileri yaparak Tomcat 'i yüklersiniz:

    a. [Tomcat 8 indirme sayfasından](http://tomcat.apache.org/download-80.cgi)en son Tomcat 8 sürümü IÇIN tar URL 'sini alın.

    b. Bağlantıyı kullanarak en son sürümü indirmek için kıvrımlı kullanın. Aşağıdaki komutları çalıştırın:

    ```bash  
        cd /tmp 
        curl -O <URL for the tar for the latest version of Tomcat 8>
    ```

    c. */Seçenek/Tomcat* dizinine Tomcat 'i yükler. Klasörünü oluşturun ve ardından arşivi açın:

    ```bash  
        sudo mkdir /opt/tomcat
        sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
        sudo chown -R tomcat webapps/ work/ temp/ logs/
    ```

1. Aşağıdaki komutları çalıştırarak Tomcat izinlerini güncelleştirin:

    ```bash  
        sudo chgrp -R tomcat /opt/tomcat
        sudo chmod -R g+r conf
        sudo chmod g+x conf
    ```

1. Bir hizmet olarak Tomcat çalıştırabilmeniz için bir *systemd* hizmet dosyası oluşturun.

   a. Tomcat 'in Java 'Yı nerede yüklediklerinizi bilmelidir. Bu yol genellikle *JAVA_HOME*olarak adlandırılır. Şunu çalıştırarak konumu bulun:

    ```bash  
        sudo update-java-alternatives -l
    ```

    Bu, aşağıdakine benzer bir şey üretir:

    ```Text  
        Output
        java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
    ```

    *JAVA_HOME* değişken değerini, çıktısından yolu alarak ve */JRE*ekleyerek oluşturabilirsiniz. Örneğin, önceki örneği kullanarak */usr/lib/jvm/Java-1.8.0-OpenJDK-AMD64/JRE*.

    b. Systemd hizmet dosyasını oluşturmak için, sunucunuzdaki değeri kullanın:

    ```bash  
        sudo nano /etc/systemd/system/tomcat.service
    ```

    c. Aşağıdaki içerikleri hizmet dosyanıza yapıştırın. Gerekirse, *JAVA_HOME*değerini, sisteminizde bulduğunuz değerle eşleşecek şekilde değiştirin. Ayrıca, CATALINA_OPTS içinde belirtilen bellek ayırma ayarlarını da değiştirmek isteyebilirsiniz:

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

    e. Hizmet dosyanızı bilmesi için systemd arka plan programını yeniden yükleyin:

    ```bash  
        sudo systemctl daemon-reload
    ```

    f. Tomcat hizmetini başlatın: 

    ```bash  
        sudo systemctl start tomcat
    ```

    g. Şunu girerek hata olmadan başlatıldığını doğrulayın:

    ```bash  
        sudo systemctl status tomcat
    ```

1. Tomcat sunucusunu doğrulayın. Tomcat geleneksel istekleri kabul etmek için 8080 bağlantı noktasını kullanır. Aşağıdaki komutu çalıştırarak bu bağlantı noktasına giden trafiğe izin verin:

    ```bash  
        sudo ufw allow 8080
    ```

    Azure Stack VM 'niz için *gelen bağlantı noktası kurallarını* eklemediyseniz, şimdi ekleyin. Daha fazla bilgi için bkz. [VM oluşturma](#create-a-vm).

1. Azure Stack aynı ağda bir tarayıcı açın ve ardından sunucunuzu ( *yourmachine. Local. cloudapp. azurestack. external: 8080*) açın.

    ![Azure Stack VM 'de Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat.png)

    Sunucunuzdaki Apache Tomcat sayfası yüklenir. Daha sonra, sunucuyu sunucu durumuna, yönetici uygulamasına ve konak yöneticisine erişmenize izin verecek şekilde yapılandırırsınız.

1. Sunucunuzu yeniden başlattığınızda Tomcat 'in otomatik olarak başlaması için hizmet dosyasını etkinleştirin:

    ```bash  
        sudo systemctl enable tomcat
    ```

1. Web yönetimi arabirimine erişim sağlamak için, Tomcat sunucusunu yapılandırın. 

   a. *Tomcat-Users. xml* dosyasını düzenleyin ve oturum açabilmeniz için bir rol ve Kullanıcı tanımlayın. `manager-gui` Ve '`admin-gui`a erişmek için kullanıcıyı tanımlayın.

    ```bash  
        sudo nano /opt/tomcat/conf/tomcat-users.xml
    ```

   b. Aşağıdaki öğeleri `<tomcat-users>` bölümüne ekleyin:

    ```XML  
        <role rolename="tomcat"/>
        <user username="<username>" password="<password>" roles="tomcat,manager-gui,admin-gui"/>
    ```

    Örneğin, son dosyanız şöyle görünebilir:

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

1. Tomcat, sunucudan gelen bağlantılarla *yönetici* ve *ana bilgisayar yöneticisi* uygulamalarına erişimi kısıtlar. Tomcat 'i Azure Stack bir sanal makineye yükletireceğiz, bu kısıtlamayı kaldırmak isteyeceksiniz. Uygun *Context. xml* dosyalarını düzenleyerek, bu uygulamalardaki IP adresi kısıtlamalarını değiştirin.

    a. Manager uygulamasında *Context. xml* ' i güncelleştirin:

    ```bash  
        sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
    ```

    b. Her yerden bağlantılara izin vermek için IP adresi kısıtlamasını açıklama veya Tomcat 'e bağlanmak için kullandığınız makinenin IP adresini ekleyin.

    ```XML  
    <Context antiResourceLocking="false" privileged="true" >
        <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
                allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
    </Context>
    ```

    c. Dosyayı kaydedin ve kapatın.

    d. *Context. xml* adlı konak Yöneticisi uygulamasını benzer bir güncelleştirmeyle güncelleştirin:

    ```bash  
        sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
    ```

    e. Dosyayı kaydedin ve kapatın.

1. Sunucuyu değişikliklerle güncelleştirmek için Tomcat hizmetini yeniden başlatın:

    ```bash  
        sudo systemctl restart tomcat
    ```

1. Azure Stack aynı ağda bir tarayıcı açın ve ardından sunucunuzu açın: *yourmachine. Local. cloudapp. azurestack. external: 8080*.

    a. Tomcat sunucusunun durumunu gözden geçirmek ve erişiminizin olduğunu doğrulamak için **sunucu durumu**' nu seçin.

    b. Tomcat kimlik bilgilerinizle oturum açın.

    ![Azure Stack VM 'de Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat-management-app.png)

## <a name="create-an-app"></a>Uygulama oluşturma

Tomcat 'e dağıtmak için bir WAR oluşturmanız gerekir. Yalnızca ortamınızı denetlemek istiyorsanız [Apache Tomcat sitesinde](https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/)bir örnek War bulabilirsiniz.

Azure 'da Java uygulamaları geliştirme hakkında yönergeler için bkz. [Azure 'Da Java uygulamaları oluşturma ve dağıtma](https://azure.microsoft.com/develop/java/).

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTY Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. Sunucuyu uygulama paketinizdeki güncelleştirmek için Tomcat hizmetini durdurun:

    ```bash  
        sudo systemctl stop tomcat
    ```

1. Webapps klasörüne yazabilmesi için FTP kullanıcısını Tomcat grubuna ekleyin. FTP kullanıcılarınız, Azure Stack ' de sanal makineyi oluşturduğunuzda tanımladığınız Kullanıcı.

    ```bash  
        sudo usermod -a -G tomcat <VM-user>
    ```

1. Webapps klasörünü temizlemek ve sonra yeni veya güncelleştirilmiş WAR ' ı yüklemek için, FileZilla ile sanal makinenize bağlanın. Yönergeler için bkz. [FileZilla Ile SFTP Ile bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-sftp-with-filezilla).

    a. Clear *TOMCAT_HOME/webapps*.

    b. WAR 'nizi *TOMCAT_HOME/webapps* 'e ekleyin (örneğin, */seçenek/Tomcat/webapps/* ).

1.  Tomcat, uygulamayı otomatik olarak genişletir ve dağıtır. Daha önce oluşturduğunuz DNS adını kullanarak görüntüleyebilirsiniz. Örneğin:

    ```HTTP  
       http://yourmachine.local.cloudapp.azurestack.external:8080/sample
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için geliştirme](azure-stack-dev-start.md)hakkında daha fazla bilgi edinin.
- [Azure Stack için genel dağıtımlar IaaS olarak](azure-stack-dev-start-deploy-app.md)hakkında bilgi edinin.
- Java programlama dilini öğrenmek ve Java için ek kaynaklar bulmak için bkz. [Java.com](https://www.java.com).
