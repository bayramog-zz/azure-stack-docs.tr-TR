---
title: Azure Stack hub 'ındaki kapsayıcılar için Azure Izleyici 'yi kullanma | Microsoft Docs
description: Azure Stack hub 'ında kapsayıcılar için Azure Izleyici 'yi nasıl kullanacağınızı öğrenin.
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
ms.date: 11/15/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 11/15/2019
ms.openlocfilehash: 2d13b5d2296d8dc76a154e1f8edf1a0238d0226b
ms.sourcegitcommit: f2a059f1be36f82adea8877f3f6e90d41ef3b161
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2019
ms.locfileid: "74163703"
---
# <a name="use-azure-monitor-for-containers-on-azure-stack-hub"></a>Azure Stack hub 'daki kapsayıcılar için Azure Izleyicisini kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Kapsayıcılar için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/) 'yi, Azure Stack hub 'ında Kubernetes kümesi dağıtılan bir aks altyapısındaki Kapsayıcılarınızı izlemek için kullanabilirsiniz. 

> [!IMPORTANT]
> AKS altyapısı şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ölçüm API 'SI aracılığıyla Kubernetes 'te bulunan denetleyiciler, düğümler ve kapsayıcılardan bellek ve işlemci ölçümleri toplayarak Azure Izleyici ile kapsayıcı performansını gözden geçirebilirsiniz. Ayrıca hizmet, kapsayıcı günlüklerini toplar. Azure 'dan şirket içi kümenizdeki sorunları tanılamak için bu günlükleri kullanabilirsiniz. Kubernetes kümelerinizdeki izlemeyi ayarladıktan sonra, bu ölçümler ve Günlükler otomatik olarak toplanır. Linux için Azure Izleyici Log Analytics aracısının kapsayıcılı bir sürümü günlükleri toplar. Azure Izleyici, Log Analytics çalışma alanınızdaki ölçümleri ve günlükleri Azure aboneliğinizde erişilebilir şekilde depolar.

Kümenizde Azure Izleyicisini etkinleştirmenin iki yolu vardır. Her iki şekilde de Azure 'da bir Azure Izleyici Log Analytics çalışma alanı ayarlamanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

Her iki yöntem de [Azure izleyici – kapsayıcılarında](https://github.com/Helm/charts/tree/master/incubator/azuremonitor-containers)listelenen [önkoşulları](https://github.com/Helm/charts/tree/master/incubator/azuremonitor-containers#pre-requisites) gerektirir.

## <a name="method-one"></a>Yöntem bir

Ayrıca [, bu grafikleri kümenize](https://helm.sh/) izleme aracılarını yüklemek için de kullanabilirsiniz. Aşağıdaki makaledeki [Azure izleyici – kapsayıcılar](https://github.com/Helm/charts/tree/master/incubator/azuremonitor-containers)makalesindeki yönergeleri izleyin.

## <a name="method-two"></a>İkinci yöntem

AKS motoru küme belirtimi json dosyasında bir **eklenti** belirtebilirsiniz. Dosya, API modeli olarak da adlandırılır. Bu eklenti içinde, çalışma alanı **GUID** 'inin Base64 kodlamalı sürümü ve izleme bilgilerinin depolanacağı Azure Log Analytics çalışma alanının **WorkspaceKey** sağlayın.

Azure Stack hub kümesi için desteklenen API tanımları şu örnekte bulunabilir: [Kubernetes-Container-monitoring_existing_workspace_id_and_key. JSON](https://github.com/Azure/aks-engine/blob/master/examples/addons/container-monitoring/kubernetes-container-monitoring_existing_workspace_id_and_key.json). Özellikle, **Kubernetesconfig**içinde **addons** özelliğini bulabilirsiniz:

    ```JSON  
    "orchestratorType": "Kubernetes",
          "kubernetesConfig": {
            "addons": [
              {
                "name": "container-monitoring",
                "enabled": true,
                "config": {
                  "workspaceGuid": "<Azure Log Analytics Workspace Guid in Base-64 encoded>",
                  "workspaceKey": "<Azure Log Analytics Workspace Key in Base-64 encoded>"
                }
              }
            ]
          }
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin  
- [Kapsayıcılar Için Azure izleyici 'ye genel bakış](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) hakkında bilgi edinin