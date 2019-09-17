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
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: 7847d79d0f2816aa56940fd7b81d25cbb1a3a7f2
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019221"
---
# <a name="scale-a-kubernetes-cluster-on-azure-stack"></a>Azure Stack bir Kubernetes kümesini ölçeklendirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

**Ölçek** komutunu kullanarak kümenizi aks altyapısıyla ölçeklendirebilirsiniz. **Scale** komutu, yeni bir Azure Resource Manager dağıtımı için giriş`apimodel.json`olarak çıktı dizini içindeki küme yapılandırma dosyanızı () yeniden kullanır. Motor, belirtilen aracı havuzunda ölçeklendirme işlemini yürütür. Ölçeklendirme işlemi tamamlandığında, altyapı güncelleştirilmiş, geçerli küme yapılandırmasını yansıtmak için aynı `apimodel.json` dosyadaki küme tanımını yeni düğüm sayısını yansıtacak şekilde güncelleştirir.

## <a name="scale-a-cluster"></a>Kümeyi ölçeklendirme

Komut, bir `aks-engine` Kubernetes kümesindeki mevcut bir aracı havuzundaki düğüm sayısını artırabilir veya azaltabilir. `aks-engine scale` Düğümler, aracı havuzunun sonuna her zaman eklenir veya kaldırılır. Düğümler, silinmeden önce bir daha erişilebilir ve drenapacaktır.

### <a name="values-for-the-scale-command"></a>Scale komutu için değerler

Aşağıdaki parametreler, ölçek komutu tarafından küme tanımı dosyanızı bulmak ve kümenizi güncelleştirmek için kullanılır.

| Parametre | Örnek | Açıklama |
| --- | --- | --- | 
| Azure-env | AzureStackCloud | Azure Stack kullanırken, ortam adlarının olarak `AzureStackCloud`ayarlanması gerekir. | 
| location | yerel | Bu, Azure Stack örneğinizin bölgesidir. Bir ASDK için bölge olarak `local`ayarlanır.  | 
| resource-group | kuin-RG | Kümenizi içeren kaynak grubunun adı. | 
| abonelik kimliği |  | Kümeniz tarafından kullanılan kaynakları içeren aboneliğin GUID 'SI. Aboneliğinizi ölçeklendirmek için yeterli kotayı kullandığınızdan emin olun. | 
| istemci kimliği |  | AKS altyapısından kümenizi oluştururken kullanılan hizmet sorumlusunun istemci KIMLIĞI. | 
| istemci parolası |  | Kümeniz oluşturulurken kullanılan hizmet sorumlusu gizli anahtarı. | 
| api modeli | Kube-RG/apimodel. JSON | Küme tanımı dosyanızın yolu (apimodel. JSON). Bu şu konumda olabilir: _Output/\<dnspredüzeltmesini >/apimodel.exe | 
| -New-node-Count | 9 | İstenen düğüm sayısı. | 
| -Master-FQDN |  | Ana FQDN. Ölçeği azaltma sırasında gereklidir. | 

Azure Stack bir kümeyi ölçeklendirirken **– Azure-env** parametresini belirtmeniz gerekir. AKS altyapısının **Ölçek** komutunda kullanılan parametreler ve değerleri hakkında daha fazla bilgi için bkz. [Ölçek-parametreleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/scale.md#parameters).

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
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin
- [Azure Stack bir Kubernetes kümesini yükseltme](azure-stack-kubernetes-aks-engine-upgrade.md)