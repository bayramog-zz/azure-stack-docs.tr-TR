---
title: Azure Stack dağıtımı ağ trafiği | Microsoft Docs
description: Bu makalede Azure Stack dağıtım ağ işlemleri hakkında ne bekleneceğiniz açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 08/29/2019
ms.openlocfilehash: a0829f2bc8cb45bdfd6f68ac15418a05adcc7afb
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188330"
---
# <a name="about-deployment-network-traffic"></a>Dağıtım ağ trafiği hakkında
Azure Stack dağıtımı sırasında ağ trafiğini anlamak dağıtımın başarılı olmasına yardımcı olur. Bu makalede, dağıtım işlemi sırasında ağ trafiği akışında ne beklendiğini bilmeniz için adım adım gösterilmektedir.

Bu çizimde, dağıtım sürecinde yer alan tüm bileşenler ve bağlantılar gösterilmektedir:

![Azure Stack dağıtım ağı topolojisi](media/deployment-networking/figure1.png)

> [!NOTE]
> Bu makalede, bağlı bir dağıtım için gereksinimler açıklanmaktadır ve diğer dağıtım yöntemleri hakkında bilgi edinmek için bkz. [Azure Stack dağıtım bağlantı modelleri](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>Dağıtım VM 'si
Azure Stack çözümü, Azure Stack bileşenlerini barındırmak için kullanılan bir sunucu grubunu ve donanım yaşam döngüsü Konağı (HLH) adlı ek bir sunucuyu içerir. Bu sunucu, çözümünüzün yaşam döngüsünü dağıtmak ve yönetmek için kullanılır ve dağıtım sırasında dağıtım VM 'sini (DVD) barındırır.

Azure Stack çözüm sağlayıcıları ek yönetim VM 'Leri sağlayabilir. Bir çözüm sağlayıcısından yönetim VM 'lerinde değişiklik yapmadan önce çözüm sağlayıcısıyla birlikte onaylayın.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri
Dağıtım başlamadan önce, dağıtımın başarılı bir şekilde tamamlanmasını sağlamak için OEM tarafından doğrulanabilen bazı minimum gereksinimler vardır:

-   [Sertifikalar](azure-stack-pki-certs.md)
-   [Azure aboneliği](https://azure.microsoft.com/free/?b=17.06)
-   İnternet erişimi
-   DNS
-   NTP

> [!NOTE]
> Bu makalede son üç gereksinim ele alınmaktadır. İlk ikisi hakkında daha fazla bilgi için yukarıdaki bağlantılara bakın.

## <a name="deployment-network-traffic"></a>Dağıtım ağı trafiği
DVD, BMC ağından bir IP ile yapılandırılır ve internet 'e ağ erişimi gerektirir. BMC ağ bileşenlerinin tümü dış yönlendirme veya Internet erişimi gerektirmese de, bu ağdan IP kullanan bazı OEM 'ye özgü bileşenlere de gereksinim duyabilirsiniz.

Dağıtım sırasında DVG, aboneliğinizden bir Azure hesabı kullanarak Azure Active Directory (Azure AD) ile kimlik doğrulaması yapar. Bunu yapmak için, Dvl 'nin [belirli bağlantı noktaları ve URL 'ler](azure-stack-integrate-endpoints.md)listesine internet erişimi olması gerekir. DVD, iç bileşenler tarafından yapılan DNS isteklerini dış URL 'lere iletmek için bir DNS sunucusu kullanır. İç DNS, bu istekleri dağıtımdan önce OEM 'ye sağladığınız DNS ileticisi adresine iletir. Aynı, NTP sunucusu için de geçerlidir. tüm Azure Stack bileşenleri için tutarlılık ve zaman eşitlemesini sürdürmek için güvenilir bir saat sunucusu gerekir.

Dağıtım sırasında DVı 'nin gerektirdiği internet erişimi yalnızca giden, dağıtım sırasında gelen hiçbir çağrı yapılmaz. Bunun IP 'sini kaynak olarak kullandığını ve Azure Stack proxy yapılandırmalarının desteklemediğini aklınızda bulundurun. Bu nedenle, gerekirse, internet 'e erişmek için bir saydam proxy veya NAT sağlamanız gerekir. Dağıtım sırasında bazı iç bileşenler, genel VIP 'ler kullanılarak dış ağ üzerinden İnternet 'e erişmeye başlayacaktır. Dağıtım tamamlandıktan sonra, Azure ile Azure Stack arasındaki tüm iletişimler, genel VIP 'ler kullanılarak dış ağ üzerinden yapılır.

Azure Stack anahtarlarındaki ağ yapılandırması, belirli ağ kaynakları ve hedefler arasındaki trafiği kısıtlayan erişim denetim listelerini (ACL 'Ler) içerir. DVD, sınırsız erişime sahip tek bileşendir; HLH de çok kısıtlanıyor. OEM 'nizden, ağlarınızdan yönetimi ve erişimi kolaylaştırmak için özelleştirme seçenekleri isteyebilirsiniz. Bu ACL 'Ler nedeniyle, dağıtım zamanında DNS ve NTP sunucu adreslerini değiştirmekten kaçınmak önemlidir. Bunu yaparsanız, çözüme yönelik tüm anahtarları yeniden yapılandırmanız gerekir.

Dağıtım tamamlandıktan sonra, belirtilen DNS ve NTP sunucu adresleri, dış ağı kullanarak SDN aracılığıyla sistem bileşenleri tarafından çalışmaya devam edecektir. Örneğin, dağıtım tamamlandıktan sonra DNS isteklerini denetledikten sonra kaynak DVD IP 'sinden ortak bir VIP 'ye değişir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure kaydını doğrulama](azure-stack-validate-registration.md)
