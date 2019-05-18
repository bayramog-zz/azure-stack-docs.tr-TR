---
title: Azure Stack'te bir sanal makineye bir Go web uygulaması dağıtma | Microsoft Docs
description: Azure Stack'te bir VM için bir Go web uygulaması dağıtma
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 8b1f207929e018b27bb3f20db8d2b7d5abe46088
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838355"
---
# <a name="deploy-a-go-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Go web uygulaması dağıtma

Bir sanal makine (VM) Azure Stack'te bir Go web uygulaması barındırmak için oluşturabilirsiniz. Bu makalede, bir sunucu kurulumu yapmak, Go web uygulamanızı barındırmak için sunucuyu yapılandırmak ve sonra uygulamayı Azure Stack'e dağıtma.

## <a name="create-a-vm"></a>VM oluşturma

1. ' Ndaki yönergeleri takip ederek, Azure Stack sanal ayarlama [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ bölmesinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), Web sayfalarını sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür. |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanın. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), makinenizde bir grafik kullanıcı arabirimini kullanarak bir Uzak Masaüstü bağlantısı sağlar.   |
    | 3000 | Özel | Bağlantı noktası 3000 geliştirme Go web çerçevesi tarafından kullanılır. Bir üretim sunucusu için 80 ve 443 üzerinden trafiğiniz yol. |

## <a name="install-go"></a>Git'i yükleyin

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTY SSH üzerinden Bağlan ](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. Sanal makinenizin üzerinde bash isteminde aşağıdaki komutları girin:

    ```bash  
    wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
    sudo tar -xvf go1.10.linux-amd64.tar.gz
    sudo mv go /usr/local
    ```

2. Vm'nizde Git ortamı ayarlayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
    export GOROOT=/usr/local/go
    export GOPATH=$HOME/Projects/ADMFactory/Golang
    export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

    vi ~/.profile
    ```

3. Yüklemenizi doğrulayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
        go version
    ```

3. [Git'i yükleyin](https://git-scm.com), yaygın olarak dağıtılmış sürüm denetimi ve kaynak kodu Yönetimi (SCM) sistemi. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. Git deponuzu VM üzerinde ayarlayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
       git clone https://github.com/appleboy/go-hello
    
       cd go-hello
       go get -d
    ```

2. Uygulamayı başlatın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
       go run hello-world.go
    ```

3. Yeni sunucunuza gidin. Çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md).
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
- Go için ek kaynaklar bulma ve Go programlama dili öğrenmek için bkz: [Golang.org](https://golang.org).
