---
title: Azure Stack bir sanal makineye Python web uygulaması dağıtma | Microsoft Docs
description: Azure Stack bir sanal makineye Python web uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 9fdb5767e225f9d5d8372058984c9128609339f1
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824237"
---
# <a name="deploy-a-python-web-app-to-a-vm-in-azure-stack"></a>Azure Stack bir sanal makineye Python web uygulaması dağıtma

Azure Stack ' de Python web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, bir sunucu ayarlar, sunucuyu Python web uygulamanızı barındıracak şekilde yapılandırır ve ardından uygulamayı Azure Stack dağıtmanız gerekir.

Bu makalede, NGINX sunucusundaki bir sanal ortamda Flask çalıştıran Python 3. x kullanılmaktadır.

## <a name="create-a-vm"></a>VM oluşturma

1. [Azure Stack ' de bir Web uygulaması barındırmak için bir Linux sanal makinesi dağıtma](azure-stack-dev-start-howto-deploy-linux.md)bölümündeki yönergeleri izleyerek sanal makineyi Azure Stack ' de ayarlayın.

2. VM ağı bölmesinde, aşağıdaki bağlantı noktalarına erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), sunuculardan Web sayfalarını teslim etmek için kullanılan protokoldür. İstemciler bir DNS adı veya IP adresi ile HTTP aracılığıyla bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), bir güvenlik sertifikası gerektiren ve şifreli bilgi iletimi sağlayan güvenli bir HTTP sürümüdür. |
    | 22 | SSH | Secure Shell (SSH), güvenli iletişimler için şifreli bir ağ protokolüdür. Bu bağlantıyı, sanal makineyi yapılandırmak ve uygulamayı dağıtmak için bir SSH istemcisiyle birlikte kullanırsınız. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), uzak masaüstü bağlantısının makinenizde bir grafik kullanıcı arabirimi kullanmasına izin verir.   |
    | 5000, 8000 | Özel | Geliştirme aşamasında Flask Web çerçevesi tarafından kullanılan bağlantı noktaları. Bir üretim sunucusu için, trafiğinizi 80 ve 443 ile yönlendirmenize sahip olursunuz. |

## <a name="install-python"></a>Python'ı yükleme

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTy Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).
2. SANAL makinenizin Bash isteminde aşağıdaki komutu girin:

    ```bash  
    sudo apt-get -y install python3 python3-venv python3-dev
    ```

3. Yüklemenizi doğrulayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
        python -version
    ```

3. Hafif bir Web sunucusu olan [NGINX 'ı yükler](https://www.nginx.com/resources/wiki/). SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install nginx git
    ```

4. Yaygın olarak dağıtılmış bir sürüm denetimi ve kaynak kodu yönetimi (SCM) sistemi olan [Git 'ı yükler](https://git-scm.com). SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. VM 'de git deponuzu ayarlayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash  
       git clone https://github.com/mattbriggs/flask-hello-world.git
    
       cd flask-hello-world
    ```

2. Sanal bir ortam oluşturun ve tüm paket bağımlılıklarıyla doldurun. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash  
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    
    export FLASK_APP=application.py
    # export FLASK_DEBUG=1 
    flask run -h 0.0.0.0
    ```

3. Yeni sunucunuza gidin. Çalışan Web uygulamanızı görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:5000
    ```

## <a name="update-your-server"></a>Sunucunuzu güncelleştirme

1. SSH oturumunuzda sanal makinenize bağlanın. CTRL + C yazarak sunucuyu durdurun.

2. Aşağıdaki komutları girin:

    ```bash  
    deactivate
    open the git repo
    git pull
    ```

3. Sanal ortamı etkinleştirin ve uygulamayı başlatın:

    ```bash  
    source venv/bin/activate
    export FLASK_APP=application.py
    flask run -h 0.0.0.0
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için geliştirme](azure-stack-dev-start.md)hakkında daha fazla bilgi edinin.
- [Azure Stack için genel dağıtımlar IaaS olarak](azure-stack-dev-start-deploy-app.md)hakkında bilgi edinin.
- Python programlama dilini öğrenmek ve Python için ek kaynaklar bulmak için bkz. [Python.org](https://www.python.org).
