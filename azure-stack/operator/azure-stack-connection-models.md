---
title: Azure Stack tümleşik sistemleri bağlantı modelleri | Microsoft Docs
description: Dağıtım Planlama çok düğümlü Azure Stack için kararları belirleyin.
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
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: 2fc9416515b09941deefbeb97d4a3801b47e146f
ms.sourcegitcommit: b79a6ec12641d258b9f199da0a35365898ae55ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131328"
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Azure Stack tümleşik sistemleri bağlantı modelleri
Azure Stack tümleşik sistem satın almak istiyorsanız, anlamanız gereken [birden çok veri merkezinde tümleştirme konuları](azure-stack-datacenter-integration.md) sistem Merkezinizde nasıl sığacak belirlemek Azure Stack dağıtımı için. Ayrıca, Azure Stack, hibrit bulut ortamına nasıl tümleştirilecek karar vermeniz gerekir. Bu makalede, bu önemli kararlar modeli karar fatura ve Azure bağlantısı, kimlik deposu dahil olmak üzere genel bir bakış sağlar.

Bir tümleşik sistem satın almak karar verirseniz, orijinal ekipman üreticisi (OEM) donanım satıcınıza çok daha ayrıntılı Planlama işlemi boyunca size rehberlik yardımcı olur. Bunlar ayrıca gerçek dağıtım yapar.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Bir Azure Stack dağıtım bağlantı modeli seçin
İnternet'e (Azure) bağlı veya bağlantısı kesilmiş bir Azure Stack dağıtmayı tercih edebilirsiniz. Azure Stack, Azure Stack ve Azure arasında hibrit senaryoları da dahil olmak üzere en çok faydayı alın, dağıtılacak Azure'a bağlı isteyebilirsiniz. Bu seçenek (Azure Active Directory veya Active Directory Federasyon Hizmetleri) kimlik deposu ve faturalandırma modeli için hangi seçenekler kullanılabilir tanımlar (siz kullanım tabanlı ödemesi fatura veya kapasite tabanlı faturalandırma) Aşağıdaki diyagramda ve tabloda özetlendiği gibi: 

![Azure Stack dağıtımı ve faturalama senaryoları](media/azure-stack-connection-models/azure-stack-scenarios.png)  
  
> [!IMPORTANT]
> Bir anahtar karar noktası budur! Active Directory Federasyon Hizmetleri (AD FS) veya Azure Active Directory (Azure AD) seçme dağıtım sırasında yapmanız gereken tek seferlik bir karardır. Bu daha sonra tüm sistemin yeniden dağıtmadan değiştiremezsiniz.  


|Seçenekler|Azure'a bağlı|Azure'dan bağlantısı kesildi|
|-----|:-----:|:-----:|
|Azure AD|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|AD FS|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Kullanıma dayalı faturalandırma|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|Kapasite kullanıma dayalı faturalandırma|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Lisanslama| Kurumsal Anlaşma ya da bulut çözümü sağlayıcısı | Kurumsal Anlaşma |
|Düzeltme eki ve güncelleştirme|Güncelleştirme paketini doğrudan Internet'ten Azure Stack'e indirilebilir. |  Gerekli<br><br>Ayrıca çıkarılabilir medya gerektirir<br> ve ayrı bir bağlı cihaz |
| Kayıt | Otomatik | Gerekli<br><br>Ayrıca çıkarılabilir medya gerektirir<br> ve ayrı bir bağlı cihaz |

Azure bağlantı modelinde Azure Stack dağıtımı için kullanılacak geçirmeye karar verdikten sonra ek olarak, bağlantı bağlı kararları kimlik deposunu ve ödeme yöntemi için yapılması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure bağlantılı Azure Stack dağıtım kararları](azure-stack-connected-deployment.md)

[Azure bağlantısız Azure Stack dağıtım kararları](azure-stack-disconnected-deployment.md)
