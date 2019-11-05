---
title: Azure Stack bir Kubernetes kümesini ölçeklendirme | Microsoft Docs
description: Azure Stack bir Kubernetes kümesini ölçeklendirmeyi öğrenin.
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
ms.date: 09/25/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/25/2019
ms.openlocfilehash: 39c651649aea60ab12aae9daf35b0d07615bf697
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73595291"
---
# <a name="scale-a-kubernetes-cluster-on-azure-stack"></a>Azure Stack bir Kubernetes kümesini ölçeklendirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

**Ölçek** komutunu kullanarak kümenizi aks altyapısıyla ölçeklendirebilirsiniz. **Scale** komutu, çıkış dizini içindeki küme yapılandırma dosyanızı (`apimodel.json`) yeni bir Azure Resource Manager dağıtımına giriş olarak yeniden kullanır. Motor, belirtilen aracı havuzunda ölçeklendirme işlemini yürütür. Ölçeklendirme işlemi tamamlandığında, altyapı güncelleştirilmiş, geçerli küme yapılandırmasını yansıtmak için aynı `apimodel.json` dosyadaki küme tanımını yeni düğüm sayısını yansıtacak şekilde güncelleştirir.

## <a name="scale-a-cluster"></a>Kümeyi ölçeklendirme

`aks-engine scale` komutu, bir `aks-engine` Kubernetes kümesindeki mevcut bir aracı havuzundaki düğümlerin sayısını artırabilir veya azaltabilir. Düğümler, aracı havuzunun sonuna her zaman eklenir veya kaldırılır. Düğümler, silinmeden önce bir daha erişilebilir ve drenapacaktır.

### <a name="values-for-the-scale-command"></a>Scale komutu için değerler

Aşağıdaki parametreler, ölçek komutu tarafından küme tanımı dosyanızı bulmak ve kümenizi güncelleştirmek için kullanılır.

| Parametre | Örnek | Açıklama |
| --- | --- | --- | 
| Azure-env | AzureStackCloud | Azure Stack kullanırken, ortam adlarının `AzureStackCloud`olarak ayarlanması gerekir. | 
| location | Yerel | Bu, Azure Stack örneğinizin bölgesidir. Bir ASDK için bölge `local`olarak ayarlanır.  | 
| resource-group | kuin-RG | Kümenizi içeren kaynak grubunun adı. | 
| abonelik kimliği |  | Kümeniz tarafından kullanılan kaynakları içeren aboneliğin GUID 'SI. Aboneliğinizi ölçeklendirmek için yeterli kotayı kullandığınızdan emin olun. | 
| istemci kimliği |  | AKS altyapısından kümenizi oluştururken kullanılan hizmet sorumlusunun istemci KIMLIĞI. | 
| istemci parolası |  | Kümeniz oluşturulurken kullanılan hizmet sorumlusu gizli anahtarı. | 
| api modeli | Kube-RG/apimodel. JSON | Küme tanımı dosyanızın yolu (apimodel. JSON). Bu şu konumda olabilir: _Output/\<Dnspredüzeltmesini >/apimodel.exe JSON | 
| -New-node-Count | 9 | İstenen düğüm sayısı. | 
| -Master-FQDN |  | Ana FQDN. Ölçeği azaltma sırasında gereklidir. |
| kimlik sistemi | FS | İsteğe bağlı. Active Directory Federasyon Hizmetleri kullanıyorsanız kimlik yönetimi çözümünüzü belirtin (AD FS). |

Azure Stack bir kümeyi ölçeklendirirken **--Azure-env** parametresini belirtmeniz gerekir. AKS altyapısının **Ölçek** komutunda kullanılan parametreler ve değerleri hakkında daha fazla bilgi için bkz. [Ölçek-parametreleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/scale.md#parameters).

### <a name="command-to-scale-your-cluster"></a>Kümenizi ölçeklendirmeye yönelik komut

Kümeyi ölçeklendirmek için aşağıdaki komutu çalıştırın:

```bash
aks-engine scale \
    --azure-env AzureStackCloud   \
    --location <for an ASDK is local> \
    --resource-group <cluster resource group>
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --api-model <path to your apomodel.json file>
    --new-node-count <desired node count> \
    --master-FQDN <master FQDN> \
    --identity-system adfs # required if using AD FS
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin
- [Azure Stack bir Kubernetes kümesini yükseltme](azure-stack-kubernetes-aks-engine-upgrade.md)