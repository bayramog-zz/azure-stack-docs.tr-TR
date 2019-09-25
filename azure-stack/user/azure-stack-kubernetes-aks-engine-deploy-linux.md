---
title: Linux 'ta AKS altyapısını Azure Stack | Microsoft Docs
description: Bir Kubernetes kümesi dağıtmak ve yönetmek üzere AKS altyapısını barındırmak için Azure Stack Linux makinesi kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: 9c600451070373d10ee943d8e497693d89708801
ms.sourcegitcommit: 4e48f1e5af74712a104eda97757dc5f50a591936
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224959"
---
# <a name="install-the-aks-engine-on-linux-in-azure-stack"></a>Linux 'ta AKS altyapısını Azure Stack ' de yükler

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bir Kubernetes kümesini dağıtmak ve yönetmek üzere AKS altyapısını barındırmak için Azure Stack Linux makinesini kullanabilirsiniz. Bu makalede, hem bağlı hem de bağlantısı kesilen Azure Stack örnekleri için, istemci sanal makinesini hazırlama, yüklemeyi denetleme ve istemci sanal makinesini ASDK üzerinde ayarlama hakkında inceliyoruz.

## <a name="prepare-the-client-vm"></a>İstemci VM 'sini hazırlama

AKS motoru, Kubernetes kümenizi dağıtmak ve yönetmek için kullanılan bir komut satırı aracıdır. Altyapıyı Azure Stack bir makinede çalıştırabilirsiniz. Bu makineden, kümelerinizi çalıştırmak için gereken IaaS kaynaklarını ve yazılımlarını dağıtmak üzere AKS altyapısını çalıştıracaksınız. Daha sonra, kümelerinizde yönetim görevlerini gerçekleştirmek için altyapıyı çalıştıran makineyi kullanabilirsiniz.

İstemci makinenizi seçerken şunları göz önünde bulundurun:

1. İstemci makinenin bir olağanüstü durum oluşması halinde kurtarılabilir olması gerekir.
2. İstemci makinesine nasıl bağlanacağınızı ve makinenin kümenizle nasıl etkileşime gireceğini öğrenin.

## <a name="install-in-a-connected-environment"></a>Bağlı bir ortama yüklensin

Kubernetes kümenizi Internet 'e bağlı bir Azure Stack yönetmek için istemci sanal makinesini yükleyebilirsiniz.

1. Azure Stack bir Linux sanal makinesi oluşturun. Yönergeler için bkz [. hızlı başlangıç: Azure Stack portalını](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-linux-portal)kullanarak BIR Linux sunucu VM 'si oluşturun.
2. Sanal makinenize bağlanın.
3. [Desteklenen Kubernetes sürümleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions) tablosunda aks altyapısının sürümünü bulun. AKS temel altyapısı, Azure Stack marketi 'nde kullanılabilir olmalıdır. Komutunu çalıştırırken sürümü `--version v0.41.0`belirtmeniz gerekir. Sürümü belirtmezseniz, komut Market 'te kullanılamayan bir VHD görüntüsü gerektirebilecek en son sürümü yükler.
4. Şu komutu çalıştırın:

    ```bash  
        curl -o get-akse.sh https://raw.githubusercontent.com/Azure/aks-engine/master/scripts/get-akse.sh
        chmod 700 get-akse.sh
        ./get-akse.sh --version v0.41.0
    ```

    > [!Note]  
    > Yükleme yöntemi başarısız olursa, [bağlantısı kesilen ortamda](#install-in-a-disconnected-environment)adımları deneyebilir veya alternatif bir paket yöneticisi olan [gofsi 'yi deneyebilirsiniz](azure-stack-kubernetes-aks-engine-troubleshoot.md#try-gofish).

## <a name="install-in-a-disconnected-environment"></a>Bağlantısı kesilmiş bir ortama yüklensin

Kubernetes kümenizi Internet bağlantısı kesilen bir Azure Stack yönetmek için istemci sanal makinesini yükleyebilirsiniz.

1.  Internet erişimi olan bir makineden GitHub [Azure/aks-Engine](https://github.com/Azure/aks-engine/releases/latest)sayfasına gidin. Linux makinesi için bir arşivi (*. tar. gz) (örneğin, `aks-engine-v0.xx.x-linux-amd64.tar.gz`) indirin.

2.  AKS altyapısı ikilisini içeren arşiv dosyasını (*. tar. gz) karşıya yüklemek için Azure Stack örneğiniz için bir depolama hesabı oluşturun. Azure Depolama Gezgini kullanımı hakkında yönergeler için, bkz. [Azure Stack Azure Depolama Gezgini](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-connect-se).

3. Azure Stack bir Linux sanal makinesi oluşturun. Yönergeler için bkz [. hızlı başlangıç: Azure Stack portalını](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-linux-portal)kullanarak BIR Linux sunucu VM 'si oluşturun.

3.  Arşiv dosyasını (*. tar. gz) karşıya yüklediğiniz Azure Stack depolama hesabı blobu URL 'sinden, dosyayı Yönetim sanal makinenize indirin. Arşivi Dizine `/usr/local/bin`ayıklayın.

4. Sanal makinenize bağlanın.

5.  Şu komutu çalıştırın:

    ```bash  
    curl -o aks-engine-v0.xx.x-linux-amd64.tar.gz <httpurl/aks-engine-v0.xx.x-linux-amd64.tar.gz>
    tar xvzf aks-engine-v0.xx.x-linux-amd64.tar.gz -C /usr/local/bin
    ```

## <a name="verify-the-installation"></a>Yüklemeyi doğrulama

İstemci VM 'niz kurulduktan sonra, AKS altyapısını yüklediğinizden emin olun.

1. İstemci sanal makinenize bağlanın.
2. Şu komutu çalıştırın:

    ```bash  
    aks-engine version
    ```

AKS altyapısını istemci sanal makinenize yüklediğinizi doğrulayadıysanız bkz. [aks altyapısı yükleme sorunlarını giderme](azure-stack-kubernetes-aks-engine-troubleshoot.md)


## <a name="asdk-installation"></a>ASDK yüklemesi

ASDK 'de AKS altyapısının istemci sanal makinesini çalıştırırken bir sertifika eklemeniz gerekir.

Bir ASDK kullandığınızda Azure Resource Manager uç noktası otomatik olarak imzalanan bir sertifika kullanıyorsa, bu sertifikayı makinenin güvenilen sertifika deposuna eklemeniz gerekir. Asdk kök sertifikasını, ASDK 'de dağıttığınız herhangi bir VM 'de bulabilirsiniz. Örneğin, bir Ubuntu sanal makinesinde bu dizinde `/var/lib/waagent/Certificates.pem`bulacaksınız. 

Sertifika dosyasını aşağıdaki komutla kopyalayın:

```bash
sudo cp /var/lib/waagent/Certificates.pem /usr/local/share/ca-certificates/azurestackca.crt

sudo update-ca-certificates
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Kubernetes kümesini AKS altyapısı ile Azure Stack dağıtma](azure-stack-kubernetes-aks-engine-deploy-cluster.md)