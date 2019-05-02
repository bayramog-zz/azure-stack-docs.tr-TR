---
title: Azure Stack'te bir sanal makine bir Python web uygulaması dağıtma | Microsoft Docs
description: Azure Stack'te bir sanal makineye bir Python web uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: b1a588dd084962c095a534f6569333b34e694bc6
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64481846"
---
# <a name="how-to-deploy-a-python-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Python web uygulaması dağıtma

Azure stack'teki Python Web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, sunucu, Python web uygulamanızı barındırmak için sunucu yapılandırma ve ardından uygulamanızı dağıtmak ayarında izleyeceğiniz adımlar bakar.

Python bir yorumlanan, üst düzey, genel amaçlı programlama dilidir. Tülin van Rossum tarafından oluşturulan ve 1991 ilk yayımlanan özellikle önemli boşluk kullanarak Python kodunun okunabilirliğini vurgular bir tasarım felsefesi sahiptir. Bu, küçük ve büyük ölçekleri üzerinde NET programlamaya olanak veren yapıları sağlar. Python programlama dili öğrenin ve Python için ek kaynakları bulmak için bkz: [Python.org](https://www.python.org).

Bu makalede Python kullanacak Flask Ngnix sunucuda sanal bir ortamda çalışan 3.x.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure Stack'te ayarlamak, bir VM oluşturun. Bağlantısındaki [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ dikey penceresinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Bağlantı noktası | Protokol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), dağıtılmış, işbirliğine dayalı, Hiper medyayı bilgi sistemlerine yönelik bir uygulama protokolüdür. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal Makinenizin bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), Köprü Metni Aktarım Protokolü (HTTP) bir uzantısıdır. Bir bilgisayar ağ üzerinden güvenli iletişim için kullanılır. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal makinenizin bağlanır. |
    | 22 | SSH | Güvenli Kabuk (SSH) ağ hizmetleri güvenli bir şekilde güvenli olmayan bir ağ üzerinden işletim bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanır. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı için makinenizi sağlar.   |
    | 5000, 8000 | Özel | Bağlantı noktaları 5000 8000 geliştirme Flask web çerçevesi tarafından kullanılır. Bir üretim sunucusu için 80 ve 443, trafiği yönlendirmek isteyebilirsiniz. |

## <a name="install-python"></a>Python'ı yükleme

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).
2. Sanal makinenizin üzerinde bash isteminde, aşağıdaki komutları yazın:

    ```bash  
    sudo apt-get -y install python3 python3-venv python3-dev
    ```

3. Yüklemenizi doğrulayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
        python -version
    ```


3. Ngınx yükleyin. [Ngınx](https://www.nginx.com/resources/wiki/) de ters bir proxy olarak kullanılabilir, Yük Dengeleyiciyi, posta proxy ve önbellek HTTP web sunucusunun adıdır. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       sudo apt-get -y install nginx git
    ```

4. Git'i yükleyin. [Git](https://git-scm.com) bir yaygın olarak dağıtılmış düzeltme denetimi ve kaynak kodu Yönetimi (SCM) sistemidir. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. Git deponuzu VM üzerinde ayarlayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       git clone https://github.com/mattbriggs/flask-hello-world.git
    
       cd flask-hello-world
    ```

2. Bir sanal ortam oluşturma ve Paket bağımlılıklarını ile doldurun.  Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    
    export FLASK_APP=application.py
    # export FLASK_DEBUG=1 
    flask run -h 0.0.0.0
    ```

3.  Şimdi, yeni sunucuya gidin ve çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:5000
    ```

## <a name="update-your-server"></a>Sunucunuzu güncelleştirmek

1. SSH oturumunuzda, VM'ye bağlanın. Yazarak sunucu stop `CTRL+C`.
2. Aşağıdaki komutları yazın:

    ```bash  
    deactivate
    open the git repo
    git pull
    ```

3. Sanal ortam etkinleştirin ve uygulamayı başlatın

    ```bash  
    source venv/bin/activate
    export FLASK_APP=application.py
    flask run -h 0.0.0.0
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md)
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
