---
title: Azure Stack'te bir sanal makineye Ruby uygulamasını dağıtma | Microsoft Docs
description: Azure Stack'te bir sanal makineye Ruby uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: f7867e963551d04336ac4092e8b1b2f033c29e94
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838344"
---
# <a name="deploy-a-ruby-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için Ruby web uygulaması dağıtma

Azure stack'teki Ruby web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, bir sunucu kurulumu yapmak, Ruby web uygulamanızı barındırmak için sunucuyu yapılandırmak ve sonra uygulamayı Azure Stack'e dağıtma.

Bu makalede, Ruby ve Ruby on Rails web çerçevesi kullanır.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure stack'teki VM'nizi ayarlayın. Yönergeler için [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ bölmesinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), Web sayfalarını sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür. |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanın. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), makinenizde bir grafik kullanıcı arabirimini kullanarak bir Uzak Masaüstü bağlantısı sağlar.   |
    | 3000 | Özel | Ruby on Rails web çerçevesi geliştirme tarafından kullanılan bağlantı noktası. Bir üretim sunucusu için 80 ve 443 üzerinden trafiğiniz yol. |

## <a name="install-ruby"></a>Ruby’yi yükleme

1. SSH istemciniz kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. PPA deposuna yükleyin. Sanal makinenizin üzerinde bash isteminde aşağıdaki komutları girin:

    ```bash  
    sudo apt -y install software-properties-common
    sudo apt-add-repository ppa:brightbox/ruby-ng

    sudo apt update
    ```

2. Rails, VM'de Ruby ve Ruby yükleyin. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
    sudo apt install ruby
    gem install rails -v 4.2.6
    ```

3. Ruby, Rails bağımlılıkları yükleyin. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash  
    sudo apt-get install make
    sudo apt-get install gcc
    sudo apt-get install sqlite3
    sudo apt-get install nodejs
    sudo gem install sqlite
    sudo gem install bundler
    ```

    > [!Note]  
    > Ruby, Rails bağımlılıkları yüklüyorsanız, ancak sürekli olarak çalıştırmanız gerekebilir `sudo gem install bundler`. Yükleme başarısız olursa hata günlüklerini gözden geçirin ve sorunları çözün.

4. Yüklemenizi doğrulayın. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
        ruby -v
    ```

3. [Git'i yükleyin](https://git-scm.com), yaygın olarak dağıtılmış sürüm denetimi ve kaynak kodu Yönetimi (SCM) sistemi. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="create-and-run-an-app"></a>Oluşturma ve uygulama çalıştırma

1. SSH oturumunuzda, sanal makinenizde hala bağlıyken aşağıdaki komutları girin:

    ```bash
        rails new myapp
        cd myapp
        rails server -b 0.0.0.0 -p 3000
    ```

2. Yeni sunucunuza gidin. Çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md).
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
- Ruby programlama dilini öğrenin ve Python için ek kaynakları bulmak için bkz: [Ruby lang.org](https://www.ruby-lang.org).
