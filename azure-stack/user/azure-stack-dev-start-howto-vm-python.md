---
title: Azure Stack'te bir sanal makineye bir Python web uygulaması dağıtma | Microsoft Docs
description: Azure Stack'te bir sanal makineye bir Python web uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: f37e963ad73a361f9d4cd5a6e68ec4213d5f32fb
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838319"
---
# <a name="deploy-a-python-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Python web uygulaması dağıtma

Azure stack'teki Python web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, bir sunucu kurulumu yapmak, Python web uygulamanızı barındırmak için sunucuyu yapılandırmak ve sonra uygulamayı Azure Stack'e dağıtma.

Bu makalede Python kullanan Flask bir Nginx sunucusunda bir sanal ortamda çalışan 3.x.

## <a name="create-a-vm"></a>VM oluşturma

1. ' Ndaki yönergeleri takip ederek, Azure Stack sanal ayarlama [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ bölmesinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), Web sayfalarını sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür. |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanın. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), makinenizde bir grafik kullanıcı arabirimini kullanarak bir Uzak Masaüstü bağlantısı sağlar.   |
    | 5000, 8000 | Özel | Geliştirme Flask web çerçevesi tarafından kullanılan bağlantı noktaları. Bir üretim sunucusu için 80 ve 443 üzerinden trafiğiniz yol. |

## <a name="install-python"></a>Python'ı yükleme

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).
2. Sanal makinenizin üzerinde bash isteminde, aşağıdaki komutu girin:

    ```bash  
    sudo apt-get -y install python3 python3-venv python3-dev
    ```

3. Yüklemenizi doğrulayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
        python -version
    ```

3. [Ngınx yükleme](https://www.nginx.com/resources/wiki/), basit bir web sunucusunun. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install nginx git
    ```

4. [Git'i yükleyin](https://git-scm.com), yaygın olarak dağıtılmış sürüm denetimi ve kaynak kodu Yönetimi (SCM) sistemi. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. Git deponuzu VM üzerinde ayarlayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
       git clone https://github.com/mattbriggs/flask-hello-world.git
    
       cd flask-hello-world
    ```

2. Sanal ortam oluştur ve Paket bağımlılıklarını ile doldurun. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    
    export FLASK_APP=application.py
    # export FLASK_DEBUG=1 
    flask run -h 0.0.0.0
    ```

3. Yeni sunucunuza gidin. Çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:5000
    ```

## <a name="update-your-server"></a>Sunucunuzu güncelleştirmek

1. SSH oturumunuzda, VM'ye bağlanın. Sunucu, Ctrl + C tuşlarına basarak durdurun.

2. Aşağıdaki komutları girin:

    ```bash  
    deactivate
    open the git repo
    git pull
    ```

3. Sanal ortam etkinleştirin ve uygulamayı başlatın:

    ```bash  
    source venv/bin/activate
    export FLASK_APP=application.py
    flask run -h 0.0.0.0
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md).
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
- Python programlama dili öğrenin ve Python için ek kaynakları bulmak için bkz: [Python.org](https://www.python.org).
