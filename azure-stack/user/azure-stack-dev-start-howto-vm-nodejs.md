---
title: Bir Node. js uygulamasını Azure Stack bir sanal makineye dağıtma | Microsoft Docs
description: Azure Stack için bir Node. js uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 0b145ab315e855ee08b25ea4980bdde40d0bfc1c
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824189"
---
# <a name="deploy-a-nodejs-web-app-to-a-vm-in-azure-stack"></a>Azure Stack bir sanal makineye Node. js web uygulaması dağıtma

Azure Stack bir Node. js web uygulaması barındırmak için bir sanal makine (VM) oluşturabilirsiniz. Bu makalede, bir sunucu ayarlar, sunucuyu Node. js web uygulamanızı barındıracak şekilde yapılandırır ve ardından uygulamayı Azure Stack dağıtmanız gerekir.

## <a name="create-a-vm"></a>VM oluşturma

1. [Azure Stack ' de bir Web uygulaması barındırmak için bir Linux sanal makinesi dağıtma](azure-stack-dev-start-howto-deploy-linux.md)bölümündeki yönergeleri izleyerek sanal makineyi Azure Stack ' de ayarlayın.

2. VM ağı bölmesinde, aşağıdaki bağlantı noktalarına erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), sunuculardan Web sayfalarını teslim etmek için kullanılan protokoldür. İstemciler bir DNS adı veya IP adresi ile HTTP aracılığıyla bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), bir güvenlik sertifikası gerektiren ve şifreli bilgi iletimi sağlayan güvenli bir HTTP sürümüdür. |
    | 22 | SSH | Secure Shell (SSH), güvenli iletişimler için şifreli bir ağ protokolüdür. Bu bağlantıyı, sanal makineyi yapılandırmak ve uygulamayı dağıtmak için bir SSH istemcisiyle birlikte kullanırsınız. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), uzak masaüstü bağlantısının makinenizde bir grafik kullanıcı arabirimi kullanmasına izin verir.   |
    | 1337 | Özel | Node. js tarafından kullanılan bağlantı noktası. Bir üretim sunucusu için, trafiğinizi 80 ve 443 ile yönlendirmenize sahip olursunuz. |

## <a name="install-node"></a>Düğümü Install

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTY Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. SANAL makinenizin Bash isteminde aşağıdaki komutu girin:

    ```bash  
      sudo apt install nodejs-legacy
    ```

2. Node. js paketleri veya modülleri için bir paket yöneticisi olan [NPM 'Yi yükler](https://www.npmjs.com/). SSH oturumunuzda sanal makinenize hala bağlı, aşağıdaki komutu girin:

    ```bash  
       go version
    ```

3. Yaygın olarak dağıtılmış bir sürüm denetimi ve kaynak kodu yönetimi (SCM) sistemi olan [Git 'ı yükler](https://git-scm.com). SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

3. Yüklemenizi doğrulayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       node -v
    ```

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

1. VM 'de git deponuzu ayarlayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash  
       git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
    
       cd nodejs-docs-hello-world
        npm start
    ```

2. Uygulamayı başlatın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       sudo node app.js
    ```

3. Yeni sunucunuza gidin. Çalışan Web uygulamanızı görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:1337
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için geliştirme](azure-stack-dev-start.md)hakkında daha fazla bilgi edinin.
- [Azure Stack için genel dağıtımlar IaaS olarak](azure-stack-dev-start-deploy-app.md)hakkında bilgi edinin.
- Düğüm programlama dilini öğrenmek ve düğüm için ek kaynaklar bulmak için bkz. [NodeJS.org](https://nodejs.org).
