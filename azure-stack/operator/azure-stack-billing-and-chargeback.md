---
title: Müşteri faturalandırma ve Azure stack'teki geri ödeme | Microsoft Docs
description: Azure yığını kaynak kullanım bilgilerini alma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: 8e47022d826d5983369fe7849cbde8c00f18838e
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64290984"
---
# <a name="usage-and-billing-in-azure-stack"></a>Kullanım ve faturalandırma Azure Stack'te

Bu makalede, Azure Stack kullanıcıları için kaynak kullanımı nasıl faturalandırılır ve faturalandırma bilgileri analiz ve Ücret geri için nasıl erişilir açıklanır.

Azure Stack toplar ve kullanılan kaynaklar için kullanım verilerini gruplandırır. Daha sonra Azure Stack, Azure ticari için bu verileri iletir. Azure ticari için Azure Stack kullanım Azure kullanımı için düzenler aynı şekilde düzenler.

Ayrıca, kullanım verilerini ve kendi faturalandırma veya ücret fatura bağdaştırıcısı kullanarak sistemi yedeklemek veya Microsoft Power BI gibi bir İş Zekası aracını aktarmak dışarı aktarma da alabilirsiniz.

## <a name="usage-pipeline"></a>Kullanım işlem hattı

Azure stack'teki her kaynak sağlayıcısı, kaynak kullanımı başına kullanım verilerini gönderir. Kullanım hizmeti düzenli aralıklarla (saatlik ve günlük) kullanım verilerini toplayan ve kullanım veritabanına depolar. Azure Stack operatörleri ve kullanıcıları saklı kullanım verilerini Azure Stack kaynak kullanımını API'leri erişebilir.

Varsa [Azure Stack örneğinizin Azure Active Directory'ye](azure-stack-registration.md ), Azure Stack, Azure ticari için kullanım verileri göndermek için yapılandırılır. Verileri Azure'a karşıya yüklendikten sonra faturalandırma portalı üzerinden ya da Azure kaynak kullanımı API'leri kullanarak erişebilirsiniz. Hangi kullanım verileri Azure'a bildirildiği hakkında daha fazla bilgi için bkz. [veri Kullanım raporlaması](azure-stack-usage-reporting.md).  

Aşağıdaki görüntüde, kullanım işlem hattında anahtar bileşenleri gösterilmektedir:

![Kullanım işlem hattı](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Hangi kullanım bilgilerini bulabilirim ve nasıl?

Azure Stack kaynak sağlayıcıları (örneğin, işlem, depolama ve ağ), her abonelik için saatlik aralıklarla kullanım verilerini oluşturur. Kullanım verileri, kaynak adı, abonelik kullanılan ve kullanılan miktarı gibi kullanılan kaynak hakkında bilgi içerir. Ölçümleri kimliği kaynaklar hakkında bilgi edinmek için [kullanım API'si SSS](azure-stack-usage-related-faq.md).

Kullanım verilerini topladıktan sonra [Azure'a bildirilen](azure-stack-usage-reporting.md) Azure fatura portalı üzerinden görüntülenebilir bir fatura oluşturulacak.

> [!NOTE]  
> Kullanım verilerini raporlama, Azure Stack geliştirme Seti'ni (ASDK) için ve kapasite modeli altında lisans Azure Stack tümleşik sistemi kullanıcılar için gerekli değildir. Azure Stack'te lisanslama hakkında daha fazla bilgi için bkz: [paketleme ve veri sayfası fiyatlandırma](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

Azure faturalama portalını ücrete tabi kaynaklar için kullanım verilerini gösterir. Azure Stack, ek ücrete tabi kaynaklar, daha geniş bir REST API veya PowerShell cmdlet'leri aracılığıyla Azure Stack ortamınızdaki erişebileceğiniz kaynak kümesi için kullanım verilerini yakalar. Azure Stack operatörleri, tüm kullanıcı abonelikler için kullanım verileri elde edebilirsiniz. Bireysel kullanıcılar yalnızca kendilerine ait kullanım ayrıntılarını alabilirsiniz.

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Çok kiracılı bulut hizmeti sağlayıcıları için raporlama kullanım

Sağlayıcı kullanım farklı Azure abonelikleri için ücret, böylece Azure Stack kullanarak birçok müşteri olan bir çok kiracılı bulut hizmeti sağlayıcısı (CSP) ayrı ayrı her müşteri kullanımını raporlamak isteyebilirsiniz.

Her müşteri kimliklerini farklı bir Azure Active Directory (Azure AD) kiracısı tarafından temsil edilen sahiptir. Azure Stack, her Azure AD kiracısı için atama bir CSP aboneliği destekler. Kiracılar ve abonelikleri için temel Azure Stack kayıt ekleyebilirsiniz. Temel kayıt, tüm Azure Stack örnekleri için gerçekleştirilir. Bir kiracı için bir aboneliğe kayıtlı değilse, kullanıcının Azure Stack kullanmaya devam edebilirsiniz ve bunların kullanımını temel kayıt için kullanılan abonelik gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack ile kaydetme](azure-stack-registration.md)
- [Azure Stack kullanım verilerini Azure'a bildirme](azure-stack-usage-reporting.md)
- [Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)
- [Kiracı Kaynak Kullanım API’si](azure-stack-tenant-resource-usage-api.md)
- [Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)