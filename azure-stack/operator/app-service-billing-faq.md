---
title: Azure App Service'te Azure Stack faturaya genel bakış ve sık sorulan sorular | Microsoft Docs
description: Azure Stack'te Azure App Service ölçülür ve diğer IP adresleri için ücret hakkında ayrıntılar.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 05/23/2019
ms.openlocfilehash: 7b11f00c98c6433ad035b6190789276ea5aa6fa7
ms.sourcegitcommit: be5382f715a9c1c18c660b630d8fcd823f13aae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66213023"
---
# <a name="azure-app-service-on-azure-stack-billing-overview-and-faq"></a>Azure App Service, Azure Stack fatura genel bakış ve sık sorulan sorular

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure App Service teklifi için bulut operatörlerinin nasıl faturalandırılır ve nasıl bunlar sırayla hizmetin kendi kiracılarını faturalandırmak bu belgedeki bilgiler gösterilmektedir.

## <a name="billing-overview"></a>Faturaya genel bakış

Azure Stack bulut operatörleri, Azure App Service ve Azure işlevleri Kiracı yeteneklerini müşterilerinin kullanımına sunmak için Azure Stack damga Azure Stack'te Azure App Service dağıtmak seçin.  Azure App Service kaynak sağlayıcısı, birden çok tür altyapı ve çalışan katmanları arasında bölünebilir rolleri oluşur.

Hizmetin temel işlevleri için gerekli olduğu gibi altyapı rollerini faturalandırılmaz.  Altyapı rollerini bulut işlecin kiracılar taleplerini desteklemek için gerektiği şekilde genişletilebilir.  Altyapı rollerini aşağıdaki gibidir:

- Denetleyiciler
- Yönetim rolleri
- Yayımcılar
- Ön Uçlar

Çalışan katmanları, iki ana tür oluşur: paylaşılan ve ayrılmış. Alt kullanım bulut işleci aşağıdaki ölçütlere göre faturalandırılır.

## <a name="shared-workers"></a>Paylaşılan çalışan

Paylaşılan çalışan çok kiracılı ve konak ücretsiz ve paylaşılan App Service planları ve tüketim tabanlı Azure işlevleri birçok kiracısı için ' dir. Paylaşılan çalışan Azure App Service kaynak sağlayıcısı hazır olarak işaretlendiğinde kullanım ölçümleri gösterin.

## <a name="dedicated-workers"></a>Adanmış çalışanları

Adanmış çalışanları kiracınız (kiracılarınız) oluşturma için App Service planları bağlıdır. Örneğin, S1 SKU'ya varsayılan olarak kiracılar için 10 örnek varsayılan olarak ölçeklendirebilirsiniz. Bu nedenle, Kiracı S1 App Service planı oluşturur, Azure App Service, kiracının App Service planına küçük çalışan katmanı ölçek örneklerinden birinde ayırır. Sonuç olarak, atanan çalışan artık diğer kiracılar için atanacak uygun değil.  Kiracı ölçeği 10 App Service planı seçerse örnekler, sonra başka 9 çalışanları kullanılabilir havuzundan kaldırılır ve kiracının App Service planına atanan.

Olduklarında ölçümleri adanmış çalışanların gönderilir:

- Azure App Service kaynak sağlayıcısı hazır olarak işaretlendi.
- Bir App Service planına atanan.

Bu nedenle, bu faturalama modeli, bulut operatörlerinin adanmış çalışan müşterilerin kadar etkili bir şekilde kiracısının app service planı tarafından ayrılan çalışanları için ödeme yapmadan kullanmak hazır bir havuz sağlama sağlar. Örneğin, her iki örnek kadar App Service planını ölçeklendirme küçük çalışan katmanı 20 çalışanlarının ve sonra iki S1 App Service planı her oluşturma 5 Müşteri da ardından, kullanılabilir çalışanlar gerekir. Sonuç olarak da olacaktır müşterilerinize veya yeni müşterilerin ölçeği genişletmek veya yeni bir App Service planları oluşturmak için hiçbiri için hiçbir kapasite. Bulut operatörleri, Azure Stack Yönetim üzerinde Azure App Service yapılandırmasında çalışan katmanları bakarak başına çalışan katmanı kullanılabilir çalışanların geçerli sayısını görüntüleyebilirsiniz.

![App Service çalışan katmanları][1]

## <a name="see-customer-usage-using-the-azure-stack-usage-service"></a>Azure Stack kullanım hizmeti kullanılarak müşteri kullanımını bakın

Bulut operatörleri sorgulayabilir [Azure Stack Kiracı kullanım API'si](azure-stack-tenant-resource-usage-api.md) müşterileri ve bu App Service tüm bireysel ölçümleri bulabilirsiniz için kullanım almak için bilgi Kiracı kullanımı açıklamak için yayan [kullanım SSS ](azure-stack-usage-related-faq.md). Bu ölçümler, ardından kullanım ücretleri hesaplamak için kullanılabilir bir müşteri abonelik başına hesaplamak için kullanılabilir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-license-the-sql-server-and-file-server-infrastructure-required-in-the-pre-requisites"></a>SQL Server Lisans ve dosya sunucu altyapısı gerekli önkoşullarda nasıl yaparım

Azure App Service kaynak sağlayıcısı tarafından gerekli SQL ve dosya sunucu altyapısı ve Azure Stack'te - Azure App Service'te kapsamdaki için lisanslama [başlamadan önce](azure-stack-app-service-before-you-get-started.md#licensing-concerns-for-required-file-server-and-sql-server) makalesi.

### <a name="the-usage-faq-lists-the-tenant-meters-but-not-the-prices-for-those-meters-where-can-i-find-them"></a>Kullanım SSS Kiracı ölçümleri ancak değil bunları nereden bulabilirim, bu ölçümleri fiyatı listeler

Bulut operatörleri, kendi fiyatlandırma modeli son müşterilerine uygulamak ücretsizdir. Kullanım hizmeti kullanım ölçümünü sağlar ve bulut operatörü belirlerler fiyatlandırma modelini temel alan, müşterilerine kaydedilecek ölçüm miktarı ardından kullanması gerekir. Etkinleştirir işleçleri fiyatlandırma diğer Azure Stack operatörlerinin ayırt etmek için bu özelliğine sahip.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Kiracı kullanım API'si](azure-stack-tenant-resource-usage-api.md)

<!--Image references-->
[1]: ./media/app-service-billing-faq/app-service-worker-tiers.png