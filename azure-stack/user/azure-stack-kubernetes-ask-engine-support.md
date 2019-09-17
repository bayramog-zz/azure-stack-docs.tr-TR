---
title: Azure Stack için AKS altyapısının destek ilkeleri | Microsoft Docs
description: Bu konu, Azure Stack üzerindeki AKS altyapısının destek ilkelerini içerir.
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
ms.openlocfilehash: 6a9ce8551975ccc98bb6e47bfa07b703dfdd341c
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019254"
---
# <a name="support-policies-for-aks-engine-on-azure-stack"></a>Azure Stack AKS altyapısı için destek ilkeleri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack üzerindeki AKS altyapısına yönelik teknik destek ilkeleri ve sınırlamaları hakkında ayrıntılı bilgi verilmektedir. Makalede ayrıca Kubernetes Market öğesi, üçüncü taraf açık kaynaklı bileşenler ve güvenlik ya da düzeltme eki yönetimi de ayrıntılı olarak açıklanır. 

> [!IMPORTANT]
> AKS altyapısı şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="self-managed-kubernetes-clusters-on-azure-stack-with-aks-engine"></a>AKS altyapısıyla Azure Stack üzerinde kendi kendine yönetilen Kubernetes kümeleri

İşlem veya ağ bileşenleri gibi hizmet olarak altyapı (IaaS) bulut bileşenleri, kullanıcılara alt düzey denetimler ve özelleştirme seçenekleri erişimi sağlar. AKS motoru, kullanıcının bu IaaS bileşenlerini kullanarak Kubernetes kümelerinin bir şekilde dağıtımını yapmasına izin verir, kullanıcılar dağıtımların tüm yönlerini erişebilir ve etkileyebilir.

Bir küme oluşturulduğunda müşteri, AKS altyapısının oluşturduğu Kubernetes ana ve çalışan düğümlerini tanımlar. Müşteri iş yükleri bu düğümlerde yürütülür. Müşteriler, ana ve çalışan düğümlerini görüntüleyebilir ve değiştirebilir. Gereksiz şekilde değiştirilen düğümler, veri ve iş yüklerinin zararlara neden olabilir ve kümeyi işlevsel olmayan şekilde işleyebilir. Ayrıca, yükseltme veya ölçeklendirme gibi AKS altyapısı işlemleri, tüm bağlantılı değişikliklere göre üzerine yazar. Örneğin, küme statik Pod içeriyorsa, bu, bir aks Altyapısı yükseltme işleminden sonra korunmaz.

Müşteri kümesi düğümleri özel kodu yürüttiğinden ve hassas verileri depoladığı için Microsoft Desteği bunlara yalnızca sınırlı bir şekilde erişebilir. Microsoft Desteği, ' de oturum açamaz, içindeki komutları yürütemiyor veya Express Müşteri izni veya yardımı olmadan bu düğümlere ait günlükleri görüntüleyemez.

## <a name="aks-engine-supported-areas"></a>AKS altyapısı desteklenen alanı

Microsoft aşağıdakiler için teknik destek sağlar:

-  AKS motoru komutlarıyla ilgili sorunlar: dağıtma, oluşturma, yükseltme ve ölçeklendirme. Araç, Azure 'daki davranışıyla tutarlı olmalıdır.
-  [AKS altyapısının genel bakışını](azure-stack-kubernetes-aks-engine-overview.md)takip eden bir Kubernetes kümesi ile ilgili sorunlar.
-  Diğer Azure Stack hizmetleriyle bağlantı sorunları 
-  Kubernetes API bağlantısı ile ilgili sorunlar
-  Azure Stack Kubernetes sağlayıcısı işlevselliği ve Azure Resource Manager bağlantısı ile ilgili sorunlar
-  AKS altyapısı tarafından oluşturulan, yük dengeleyiciler, ağ güvenlik grupları, VNET 'ler, alt ağlar, ağ arabirimleri, yol tablosu, kullanılabilirlik kümeleri, genel IP adresleri, depolama hesabı ve VM makineleri gibi Azure Stack yerel yapıtlar ile ilgili sorunlar 
-  Ağ performansı ve gecikme sorunları
-  Bağlantısı kesilmiş dağıtımlarda AKS altyapısı tarafından kullanılan AKS temel görüntüsündeki sorunlar. 

## <a name="aks-engine-areas-not-supported"></a>AKS altyapısı alanı desteklenmiyor

Microsoft aşağıdakiler için teknik destek sağlamaz:

-  Azure 'da AKS altyapısını kullanma.
-  Kubernetes Market öğesi Azure Stack.
-  Aşağıdaki AKS motoru küme tanımı seçeneklerini ve eklentileri kullanma.
    -  Desteklenmeyen Eklentiler:  
            -AAD Pod kimliği  
            -ACI Bağlayıcısı  
            -Blobsigortası Flex birimi  
            -Küme otomatik Scaler  
            -Kapsayıcı Izleme  
            -Keykasa esnek hacmi  
            -NVıDıA cihaz eklentisi  
            - Rescheduler  
            -SMB esnek birimi  
        
    -  Desteklenmeyen küme tanımı seçenekleri:  
            -KubernetesConfig altında:  
                    -cloudControllerManagerConfig  
                    -enableDataEncryptionAtRest  
                    -enableEncryptionWithExternalKms  
                    -Enablepod SecurityPolicy  
                    -etcdEncryptionKey  
                    -Useınstancemetadata  
                    -Usemanagedıdentity  
                    -azureCNIURLLinux  
                    -azureCNIURLWindows  
            -MasterProfile:  
                    -Kullanılabilirliği olan Bilityzones  
            -AgentPoolProfiles altında:  
                    -Kullanılabilirliği olan Bilityzones  
                    -singlePlacementGroup  
                    -scaleSetPriority  
                    -scaleSetEvictionPolicy  
                    -Ivme Networkingenabled  
                    -Ivmi Networkingenabledwindows

-  Kubernetes yapılandırma deposu, Kubernetes yapılandırma deposu etcd dışında devam ediyor. Örneğin, kümenin düğümlerinde çalışan statik Pod 'ler.
-  Kubernetes 'i kullanma hakkında sorular. Örneğin, Microsoft Desteği özel giriş denetleyicileri oluşturma, uygulama iş yüklerini kullanma veya üçüncü taraf ya da açık kaynaklı yazılım paketlerini veya araçları uygulama hakkında öneri sağlamaz.
-  AKS altyapısı tarafından dağıtılan Kubernetes kümesinin bir parçası olarak sağlanmayan üçüncü taraf açık kaynaklı projeler. Bu projeler Kubeadm, Kubespray, Native, Istio, Helm, Envoy veya diğerleri içerebilir.
-  Aks altyapısını kullanarak aks altyapısını [desteklenen senaryolarda](azure-stack-kubernetes-aks-engine-overview.md#supported-scenarios-with-the-aks-engine)belirtenler dışındaki kullanım örneği senaryolarında kullanma.
-  Üçüncü taraf yazılım. Bu yazılım, güvenlik tarama araçları ve ağ aygıtlarını veya yazılımlarını içerebilir.
-  Birden çok yüksek veya çok satıcılı derleme hakkında sorunlar. Örneğin, Microsoft, bir Federasyon çoğulublik bulut satıcısı çözümünü çalıştırmaya ilişkin sorunları desteklemez.
-  [Aks altyapısı tarafından desteklenen bölgeler](#aks-engine-supported-areas) bölümünde listelenenler dışındaki ağ özelleştirmeleri.

##  <a name="security-issues-and-patching"></a>Güvenlik sorunları ve düzeltme eki uygulama

AKS altyapısının veya Kubernetes sağlayıcısı 'nın Azure Stack için bir veya daha fazla bileşende güvenlik kusuru bulunursa, Microsoft bu sorunu azaltmak için müşterilerin etkilenen kümeleri yamaları için bir düzeltme eki kullanıma sunulacaktır. Alternatif olarak, takım kullanıcılara Yükseltme Kılavuzu sağlayacaktır. Yamaların kümede kapalı kalma süresi gerektirebileceğini unutmayın. Yeniden başlatmalar gerektiğinde, Microsoft bu gereksinimin müşterilerine bildirimde bulunur. Kullanıcılar, düzeltme eklerini Microsoft Kılavuzu 'na göre uygulayamıyorsanız, bu, kendi kümeleri güvenlik sorununa karşı savunmasız olmaya devam edecektir.

## <a name="kubernetes-marketplace-item"></a>Kubernetes Market öğesi

Kullanıcılar bir Kubernetes Market öğesini indirebilir, böylece kullanıcılar, AKS altyapısını Azure Stack Kullanıcı portalındaki bir şablon aracılığıyla dolaylı olarak kullanarak Kubernetes 'leri dağıtabilir, bu, AKS altyapısını doğrudan kullanmaktan daha kolay hale getirir. Bu, gösterimler, test ve geliştirme için kümeleri hızlıca kurmak üzere kullanışlı bir araçtır. Bu, Microsoft tarafından desteklenen öğeler kümesine dahil edilmediğinden üretime yönelik değildir.

## <a name="preview-features"></a>Önizleme özellikleri

Genişletilmiş test ve Kullanıcı geri bildirimi gerektiren özellikler ve işlevler için, Microsoft yeni Önizleme özellikleri veya özellik bayrağının arkasında Özellikler yayınlar. Bu özellikleri, ön sürüm veya beta özellikleri olarak değerlendirin. Önizleme özellikleri veya özellik bayrağı özellikleri üretime yönelik değildir. Devam eden işlevsellik değişiklikleri ve davranışı, hata düzeltmeleri ve diğer değişiklikler kararsız kümeler ve kapalı kalma süresine yol açabilir. Bu özellikler Microsoft tarafından desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin