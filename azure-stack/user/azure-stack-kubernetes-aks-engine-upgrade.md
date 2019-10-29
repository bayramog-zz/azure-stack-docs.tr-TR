---
title: Azure Stack bir Kubernetes kümesini yükselt | Microsoft Docs
description: Azure Stack bir Kubernetes kümesini nasıl yükselteceğinizi öğrenin.
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
ms.date: 10/16/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: 3720781dc2545fefaff0b2cd703d7c3880c4b97b
ms.sourcegitcommit: 83cef2c4ec6e1b2fd3f997c91675c1058a850e2f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "72999891"
---
# <a name="upgrade-a-kubernetes-cluster-on-azure-stack"></a>Azure Stack bir Kubernetes kümesini yükseltme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="upgrade-a-cluster"></a>Kümeyi yükseltme

AKS motoru, Aracı kullanılarak ilk olarak dağıtılan kümeyi yükseltmenize olanak tanır. AKS altyapısını kullanarak kümeleri koruyabilirsiniz. Bakım görevleriniz herhangi bir IaaS sistemine benzerdir. Yeni güncelleştirmelerin kullanılabilirliğini bilmeniz ve bunları uygulamak için AKS altyapısını kullanmanız gerekir.

Upgrade komutu Kubernetes sürümünü ve temel işletim sistemi görüntüsünü güncelleştirir. Yükseltme komutunu her çalıştırdığınızda, kümenin her düğümü için, AKS altyapısı kullanılan aks **-Engine** sürümüyle ilişkili aks temel görüntüsünü kullanarak yenı bir VM oluşturur. Kümenizdeki her ana ve aracı düğümünün para birimini korumak için `aks-engine upgrade` komutunu kullanabilirsiniz. 

Microsoft, kümenizi yönetmez. Ancak Microsoft, kümenizi yönetmek için kullanabileceğiniz araç ve VM görüntüsünü sağlar. 

Dağıtılan bir küme yükseltmeleri için:

-   Kubernetes
-   Kubernetes sağlayıcısını Azure Stack
-   Temel işletim sistemi

Bir üretim kümesini yükseltirken şunları göz önünde bulundurun:

-   Hedef küme için doğru küme belirtimini (`apimodel.json`) ve kaynak grubunu kullanıyor musunuz?
-   İstemci makinenin AKS altyapısını çalıştırmak ve yükseltme işlemlerini gerçekleştirmeleri için güvenilir bir makine kullanıyor musunuz?
-   Bir Yedekleme kümeniz olduğundan ve çalışır durumda olduğundan emin olun.
-   Mümkünse, ağ atlamalarını ve olası bağlantı başarısızlıklarını azaltmak için Azure Stack ortamındaki bir VM 'den komutunu çalıştırın.
-   Aboneliğinizin tüm işlem için yeterli alana sahip olduğundan emin olun. İşlem sırasında yeni VM 'Ler ayırır.
-   Hiçbir sistem güncelleştirmesi veya zamanlanmış görev planlanmamaktadır.
-   Üretim kümesi olarak tam olarak yapılandırılmış bir kümede hazırlanmış bir yükseltme kurun ve üretim kümenizde bunu yapmadan önce Bu yükseltmeyi test edin

## <a name="steps-to-upgrade-to-a-newer-kubernetes-version"></a>Daha yeni bir Kubernetes sürümüne yükseltme adımları

> [!Note]  
> Aks-Engine ' in daha yeni bir sürümünü kullanıyorsanız ve görüntünün Market 'te kullanılabilir olması durumunda AKS temel görüntüsü de yükseltilir.

Aşağıdaki yönergeler yükseltmeyi gerçekleştirmek için gereken en düşük adımları kullanır. Ek ayrıntı isterseniz, [Kubernetes kümelerini yükseltme](https://github.com/Azure/aks-engine/blob/master/docs/topics/upgrade.md)makalesine bakın.

1. Öncelikle yükseltme için hedefleceğiniz sürümleri belirlemeniz gerekir. Bu sürüm, şu anda sahip olduğunuz sürüme bağlıdır ve sonra yükseltmeyi gerçekleştirmek için bu sürüm değerini kullanır.

    Aşağıdaki komutları çalıştırın:

    ```bash  
    $ aks-engine get-versions
    Version Upgrades
    1.15.0
    1.14.3  1.15.0
    1.14.1  1.14.3, 1.15.0
    1.13.7  1.14.1, 1.14.3
    1.13.5  1.13.7, 1.14.1, 1.14.3
    1.12.8  1.13.5, 1.13.7
    1.12.7  1.12.8, 1.13.5, 1.13.7
    1.11.10 1.12.7, 1.12.8
    1.11.9  1.11.10, 1.12.7, 1.12.8
    1.10.13 1.11.9, 1.11.10
    1.10.12 1.10.13, 1.11.9, 1.11.10
    1.9.11  1.10.12, 1.10.13
    1.9.10  1.9.11, 1.10.12, 1.10.13
    1.6.9   1.9.10, 1.9.11
    ```

    Örneğin, `get-versions` komutunun çıktısına göre, geçerli Kubernetes sürümünüz "1.13.5" ise "1.13.7, 1.14.1, 1.14.3" sürümüne yükseltebilirsiniz.

2. `upgrade` komutunu çalıştırmak için ihtiyacınız olacak bilgileri toplayın. Yükseltme şu parametreleri kullanır:

    | Parametre | Örnek | Açıklama |
    | --- | --- | --- |
    | Azure-env | AzureStackCloud | Hedef platformunuzun Azure Stack `AzureStackCloud` ' ın kullanacağı AKS motoruna göstermek için. |
    | location | Yerel | Azure Stack için bölge adı. ASDK için bölge `local`olarak ayarlanır. |
    | resource-group | kuin-RG | Yeni bir kaynak grubunun adını girin veya var olan bir kaynak grubunu seçin. Kaynak adının alfasayısal ve küçük harf olması gerekir. |
    | abonelik kimliği | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Abonelik KIMLIĞINIZI girin. Daha fazla bilgi için bkz. [teklife abone olma](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services#subscribe-to-an-offer) |
    | api modeli | ./Kubernetes-azurestack.exe JSON | Küme yapılandırma dosyasının yolu veya API modeli. |
    | istemci kimliği | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Hizmet sorumlusu GUID 'INI girin. Azure Stack yöneticiniz hizmet sorumlusunu oluştururken uygulama KIMLIĞI olarak tanımlanan Istemci KIMLIĞI. |
    | istemci parolası | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Hizmet sorumlusu gizli anahtarını girin. Bu, hizmetinizi oluştururken ayarladığınız istemci sırrı. |
    | kimlik sistemi | FS | İsteğe bağlı. Active Directory Federasyon Hizmetleri kullanıyorsanız kimlik yönetimi çözümünüzü belirtin (AD FS). |

3. Değerlerinizle birlikte aşağıdaki komutu çalıştırın:

    ```bash  
    aks-engine upgrade \
    --azure-env AzureStackCloud   
    --location <for an ASDK is local> \
    --resource-group kube-rg \
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --api-model kube-rg/apimodel.json \
    --upgrade-version 1.13.5 \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --identity-system adfs # required if using AD FS
    ```

4.  Yükseltme işlemi bir hatayla karşılaştığında herhangi bir nedenden dolayı, sorunu çözdükten sonra yükseltme komutunu yeniden çalıştırabilirsiniz. AKS altyapısı, işlemi önceki zamanda başarısız olduğu yerde sürdürecek.

## <a name="steps-to-only-upgrade-the-os-image"></a>Yalnızca işletim sistemi görüntüsünü yükseltmek için adımlar

1. [Desteklenen-Kubernetes-Versions tablosunu](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions) gözden geçirin ve yükseltmeniz için planladığınız aks-Engine ve aks temel görüntüsünün sürümüne sahip olup olmadığınızı saptayın. Aks-Engine çalıştırmasının sürümünü görüntülemek için: `aks-engine version`.
2. AKS altyapısını, aks-Engine çalıştırmasını yüklediğiniz makinede (`./get-akse.sh --version vx.xx.x` **x. xx. x** ' i hedeflenen sürümünüzle değiştirerek) uygun şekilde yükseltin.
3. Kullanmayı planladığınız Azure Stack Market 'te ihtiyacınız olan AKS temel görüntüsünün sürümünü eklemek için Azure Stack işleçinizden sorun.
4. Zaten kullanmakta olduğunuz Kubernetes sürümünü kullanarak `aks-engine upgrade` komutunu çalıştırın, ancak `--force`ekleyin. [Bir yükseltmeyi zorluyor](#forcing-an-upgrade)bir örnek görebilirsiniz.


## <a name="forcing-an-upgrade"></a>Yükseltmeye zorlama

Kümenizin yükseltmesini zorlamak isteyebileceğiniz koşullar olabilir. Örneğin, bir gün, en son Kubernetes sürümünü kullanarak, bağlantısı kesilen bir ortamda bir küme dağıtırsınız. Aşağıdaki gün Ubuntu, Microsoft 'un yeni bir **aks temel görüntüsü**oluşturduğu bir güvenlik açığına yönelik bir düzeltme eki yayınlar. Daha önce dağıttığınız Kubernetes sürümünü kullanarak bir yükseltmeyi zorlayarak yeni görüntüyü uygulayabilirsiniz.

```bash  
aks-engine upgrade \
--azure-env AzureStackCloud   
--location <for an ASDK is local> \
--resource-group kube-rg \
--subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
--api-model kube-rg/apimodel.json \
--upgrade-version 1.13.5 \
--client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
--client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
--force
```

Yönergeler için bkz. [yükseltmeyi zorla](https://github.com/Azure/aks-engine/blob/master/docs/topics/upgrade.md#force-upgrade).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin
- [Azure Stack bir Kubernetes kümesini ölçeklendirme](azure-stack-kubernetes-aks-engine-scale.md)