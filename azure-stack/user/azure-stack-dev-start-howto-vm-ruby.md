---
title: Azure Stack bir sanal makineye Ruby uygulaması dağıtma | Microsoft Docs
description: Azure Stack bir sanal makineye Ruby uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: e54fc1cd1e8fe8588a9498b5af8476437880e988
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824179"
---
# <a name="deploy-a-ruby-web-app-to-a-vm-in-azure-stack"></a>Azure Stack bir sanal makineye Ruby Web uygulaması dağıtma

Ruby Web uygulamanızı barındırabilmeniz için bir VM oluşturabilirsiniz Azure Stack. Bu makalede, bir sunucu ayarlar, sunucuyu Ruby Web uygulamanızı barındıracak şekilde yapılandırır ve ardından uygulamayı Azure Stack dağıtmanız gerekir.

Bu makalede Ruby ve bir Ruby on rayın Web çerçevesi kullanılmaktadır.

## <a name="create-a-vm"></a>VM oluşturma

1. Azure Stack ' de sanal makineyi ayarlayın. Yönergeler için bkz. [Azure Stack bir Web uygulamasını barındırmak Için LINUX VM dağıtma](azure-stack-dev-start-howto-deploy-linux.md).

2. VM ağı bölmesinde, aşağıdaki bağlantı noktalarına erişilebilir olduğundan emin olun:

    | Port | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), sunuculardan Web sayfalarını teslim etmek için kullanılan protokoldür. İstemciler bir DNS adı veya IP adresi ile HTTP aracılığıyla bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), bir güvenlik sertifikası gerektiren ve şifreli bilgi iletimi sağlayan güvenli bir HTTP sürümüdür. |
    | 22 | SSH | Secure Shell (SSH), güvenli iletişimler için şifreli bir ağ protokolüdür. Bu bağlantıyı, sanal makineyi yapılandırmak ve uygulamayı dağıtmak için bir SSH istemcisiyle birlikte kullanırsınız. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP), uzak masaüstü bağlantısının makinenizde bir grafik kullanıcı arabirimi kullanmasına izin verir.   |
    | 3000 | Özel | Ruby on, geliştirme aşamasında Web çerçevesi tarafından kullanılan bağlantı noktası. Bir üretim sunucusu için, trafiğinizi 80 ve 443 ile yönlendirmenize sahip olursunuz. |

## <a name="install-ruby"></a>Ruby’yi yükleme

1. SSH istemcinizi kullanarak sanal makinenize bağlanın. Yönergeler için bkz. [PuTTy Ile SSH aracılığıyla bağlanma](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty).

1. PPA deposunu yükler. SANAL makinenizin Bash isteminde aşağıdaki komutları girin:

    ```bash  
    sudo apt -y install software-properties-common
    sudo apt-add-repository ppa:brightbox/ruby-ng

    sudo apt update
    ```

2. VM 'nize Ruby ve Ruby on rayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash  
    sudo apt install ruby
    gem install rails -v 4.2.6
    ```

3. Ruby on rayın bağımlılıklarını yükler. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash  
    sudo apt-get install make
    sudo apt-get install gcc
    sudo apt-get install sqlite3
    sudo apt-get install nodejs
    sudo gem install sqlite
    sudo gem install bundler
    ```

    > [!Note]  
    > Ruby on raya bağımlılıklarını yüklerken, @no__t tekrar tekrar çalıştırmanız gerekebilir. Yükleme başarısız olursa, hata günlüklerini gözden geçirin ve sorunları çözün.

4. Yüklemenizi doğrulayın. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
        ruby -v
    ```

3. Yaygın olarak dağıtılmış bir sürüm denetimi ve kaynak kodu yönetimi (SCM) sistemi olan [Git 'ı yükler](https://git-scm.com). SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda, aşağıdaki komutu girin:

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="create-and-run-an-app"></a>Uygulama oluşturma ve çalıştırma

1. SSH oturumunuzda sanal makinenize hala bağlı olduğunuzda aşağıdaki komutları girin:

    ```bash
        rails new myapp
        cd myapp
        rails server -b 0.0.0.0 -p 3000
    ```

2. Yeni sunucunuza gidin. Çalışan Web uygulamanızı görmeniz gerekir.

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için geliştirme](azure-stack-dev-start.md)hakkında daha fazla bilgi edinin.
- [Azure Stack için genel dağıtımlar IaaS olarak](azure-stack-dev-start-deploy-app.md)hakkında bilgi edinin.
- Ruby programlama dilini öğrenmek ve Ruby için ek kaynaklar bulmak için bkz. [Ruby-lang.org](https://www.ruby-lang.org).
