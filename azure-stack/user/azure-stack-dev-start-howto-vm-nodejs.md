---
title: Azure Stack'te bir sanal makine için bir Node.js uygulaması dağıtma | Microsoft Docs
description: Azure Stack için bir Node.js uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 879e4e552fbeaa6178f06f85959d543680b2bd3e
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782692"
---
# <a name="how-to-deploy-a-nodejs-web-app-to-a-vm-in-azure-stack"></a>Azure Stack'te bir VM için bir Node.js web uygulaması dağıtma

Azure stack'teki Node.js Web uygulamanızı barındırmak için bir VM oluşturabilirsiniz. Bu makalede, sunucu, düğüm web uygulamanızı barındırmak için sunucu yapılandırma ve ardından uygulamanızı dağıtmak ayarında izleyeceğiniz adımlar bakar.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure Stack'te ayarlamak, bir VM oluşturun. Bağlantısındaki [Azure Stack'te bir web uygulaması barındırmak üzere bir Linux VM dağıtmak](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağ dikey penceresinde, aşağıdaki bağlantı noktalarının erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), web sayfaları sunuculardan sunmak için kullanılan protokolüdür. İstemciler HTTP üzerinden bir DNS adı veya IP adresi ile bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS) bir güvenlik sertifikası gerektirir ve şifrelenmiş bilgi aktarımını için sağlayan HTTP güvenli bir sürümüdür.  |
    | 22 | SSH | Güvenli Kabuk (SSH), güvenli iletişim için kullanılan bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanır. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı için makinenizi sağlar.   |
    | 1337 | Özel | Bağlantı noktası 1337 Node.js tarafından kullanılır. Bir üretim sunucusu için 80 ve 443, trafiği yönlendirmek isteyebilirsiniz. |

## <a name="install-node"></a>Düğüme yükleyin

1. SSH istemcinizi kullanarak VM'nize bağlanın. Yönergeler için [PuTTy SSH üzerinden Bağlan](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty).
1. Sanal makinenizin üzerinde bash isteminde, aşağıdaki komutları yazın:

    ```bash  
      sudo apt install nodejs-legacy
    ```

2. NPM yükleyin. [NPM](https://www.npmjs.com/) Node.js paketler ve modüller için bir paket yöneticisidir. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       go version
    ```

3. Git'i yükleyin. [Git](https://git-scm.com) bir yaygın olarak dağıtılmış düzeltme denetimi ve kaynak kodu Yönetimi (SCM) sistemidir. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       sudo apt-get -y install git
    ```

3. Yüklemenizi doğrulayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       node -v
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. Git deponuzu VM üzerinde ayarlayın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutları yazın:

    ```bash  
       git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
    
       cd nodejs-docs-hello-world
        npm start
    ```

2. Uygulamayı başlatın. Hala sanal Makinenize SSH oturumunuzda bağlı, aşağıdaki komutu yazın:

    ```bash  
       sudo node app.js
    ```

3.  Şimdi, yeni sunucuya gidin ve çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:1337
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md)
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
- Programlama dili düğüm öğrenin ve düğüm için ek kaynakları bulmak için bkz: [Nodejs.org](https://nodejs.org).