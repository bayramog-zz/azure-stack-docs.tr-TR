---
title: Azure Stack'te bir sanal makineye Ruby uygulama dağıtma | Microsoft Docs
description: Azure Stack'te bir sanal makineye bir Ruby uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 24774e122be0c4088c4d5841f71318d266bad78f
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782677"
---
# <a name="how-to-deploy-a-ruby-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için Ruby web uygulaması dağıtma

Azure stack'teki Ruby Web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, sunucu, Ruby web uygulamanızı barındırmak için sunucu yapılandırma ve ardından uygulamanızı dağıtmak ayarında izleyeceğiniz adımlar bakar.



Bu makalede, Ruby ve Ruby on Rails web çerçevesi kullanır.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure Stack'te ayarlamak, bir VM oluşturun. Bağlantısındaki [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ dikey penceresinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), web sayfaları sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür.  |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanır. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı için makinenizi sağlar.   |
    | 3000 | Özel | Bağlantı noktası 3000 geliştirme rails üzerinde Ruby web çerçevesi tarafından kullanılır. Bir üretim sunucusu için 80 ve 443, trafiği yönlendirmek isteyebilirsiniz. |

## <a name="install-ruby"></a>Ruby’yi yükleme

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).
1. PPA deposuna yükleyin. Sanal makinenizin üzerinde bash isteminde, aşağıdaki komutları yazın:

    ```bash  
    sudo apt -y install software-properties-common
    sudo apt-add-repository ppa:brightbox/ruby-ng

    sudo apt update
    ```

2. Ruby ve Ruby yükleyin, sanal Makinenizin on rails. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
    sudo apt install ruby
    gem install rails -v 4.2.6
    ```

3. Ruby, rails bağımlılıkları yükleyin. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
    sudo apt-get install make
    sudo apt-get install gcc
    sudo apt-get install sqlite3
    sudo apt-get install nodejs
    sudo gem install sqlite
    sudo gem install bundler
    ```

    > [!Note]  
    > Yükleme, sürekli olarak çalıştırmanız gerekebilir `sudo gem install bundler`. Bağımlılıkları çalışırsanız yükleyin ve başarısız, gözden geçirmek Hata günlüklerini ve sorunları çözün.

4. Yüklemenizi doğrulayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
        ruby -v
    ```

3. Git'i yükleyin. [Git](https://git-scm.com) bir yaygın olarak dağıtılmış düzeltme denetimi ve kaynak kodu Yönetimi (SCM) sistemidir. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="create-and-run-an-app"></a>Oluşturma ve uygulama çalıştırma

1. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash
        rails new myapp
        cd myapp
        rails server -b 0.0.0.0 -p 3000
    ```

2.  Şimdi, yeni sunucuya gidin ve çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md)
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
- Ruby programlama dilini öğrenin ve Python için ek kaynakları bulmak için bkz: [Ruby lang.org](https://www.ruby-lang.org).