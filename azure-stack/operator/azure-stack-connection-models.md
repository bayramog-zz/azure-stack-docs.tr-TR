---
title: Azure Stack tümleşik sistemler bağlantı modelleri | Microsoft Docs
description: Azure Stack tümleşik sistemler için bağlantı modellerini ve diğer dağıtım planlama kararlarını belirleme.
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
ms.openlocfilehash: de9051ceee89244182c1d6d9d5724fa80a594ae6
ms.sourcegitcommit: c196463492732218d2474d3a964f88e995272c80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71094379"
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Azure Stack tümleşik sistemler bağlantı modelleri
Azure Stack tümleşik bir sistem satın almak istiyorsanız, sistemin veri merkezinize nasıl uyadığını belirleyebilmek için Azure Stack dağıtımı için [çeşitli veri merkezi tümleştirme konularını](azure-stack-datacenter-integration.md) anlamanız gerekir. Ayrıca, Azure Stack karma bulut ortamınıza nasıl tümleştirmenize karar vermeniz gerekir. Bu makalede, Azure bağlantı modelleri, kimlik deposu seçenekleri ve faturalandırma modeli seçenekleri de dahil olmak üzere bu önemli kararlar hakkında genel bir bakış sunulmaktadır.

Tümleşik bir sistem satın almaya karar verirseniz, özgün ekipman üreticisi (OEM) donanım satıcınız, planlama sürecinde daha ayrıntılı bilgi için size kılavuzluk eder. OEM donanım satıcısı Ayrıca gerçek dağıtımı da gerçekleştirir.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Bir Azure Stack dağıtım bağlantı modeli seçin
İnternet 'e bağlı olan veya bağlantısı kesilen Azure Stack dağıtmayı seçebilirsiniz. Azure Stack ile Azure arasında Karma senaryolar da dahil olmak üzere Azure Stack en avantajını almak için Azure 'a bağlı dağıtın. Bu seçenek, aşağıdaki diyagramda ve tabloda özetlenen kimlik deponuzda (Azure Active Directory veya Active Directory Federasyon Hizmetleri (AD FS)) ve faturalandırma modelinde (kullandığınız veya kapasite tabanlı faturalandırma olarak öde) hangi seçeneklerin kullanılabildiğini tanımlar:

![Azure Stack dağıtım ve faturalandırma senaryoları](media/azure-stack-connection-models/azure-stack-scenarios.png)
  
> [!IMPORTANT]
> Bu bir temel karar noktasıdır! Active Directory Federasyon Hizmetleri (AD FS) (AD FS) veya Azure Active Directory (Azure AD) seçeneğinin belirlenmesi, dağıtım zamanında yapmanız gereken tek seferlik bir karardır. Bunu daha sonra tüm sistemi yeniden dağıtmaya gerek kalmadan değiştiremezsiniz.  


|Seçenekler|Azure 'a bağlanıldı|Azure bağlantısı kesildi|
|-----|:-----:|:-----:|
|Azure AD|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|AD FS|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Tüketim tabanlı faturalandırma|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|Kapasite tabanlı faturalandırma|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Lisanslama| Kurumsal Anlaşma veya bulut çözümü sağlayıcısı | Kurumsal Anlaşma |
|Düzeltme Eki ve güncelleştirme|Güncelleştirme paketi doğrudan Internet 'ten Azure Stack indirilebilir |  Gerekli<br><br>Ayrıca çıkarılabilir medya gerektirir<br> ve ayrı bir bağlı cihaz |
| Kayıt | Otomatik | Gerekli<br><br>Ayrıca çıkarılabilir medya gerektirir<br> ve ayrı bir bağlı cihaz |

Azure Stack dağıtımınız için kullanılacak Azure bağlantı modeline karar verdikten sonra, kimlik depolama ve faturalandırma yöntemi için ek bağlantıya bağımlı kararlar verilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure bağlantılı Azure Stack dağıtım kararları](azure-stack-connected-deployment.md)

[Azure bağlantısız Azure Stack dağıtım kararları](azure-stack-disconnected-deployment.md)
