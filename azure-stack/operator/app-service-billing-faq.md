---
title: Azure App Service'te Azure Stack faturaya genel bakış ve sık sorulan sorular | Microsoft Docs
description: Azure Stack'te Azure App Service ölçülür ve diğer IP adresleri için ücret hakkında ayrıntılar.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: fa3bc647d11bca915c58aa1bd948881628405776
ms.sourcegitcommit: af63214919e798901399fdffef09650de4176956
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828367"
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

### <a name="as-a-csp-how-can-i-offer-free-and-shared-skus-for-customers-to-trial-the-service"></a>CSP olarak nasıl miyim ücretsiz ve paylaşılan SKU'lar için deneme müşterilere bir hizmetin sunduğu

Bir bulut işleci olarak paylaşılan çalışanlar barındırılan olarak Azure'da yaptığımız gibi ücretsiz ve paylaşılan SKU'lar teklifi için maliyetlere neden olur.  Size maliyeti en aza indirmek için en az için paylaşılan çalışan katmanı ölçeğini seçebilirsiniz.  Örneğin, ücretsiz ve paylaşılan App Service planı SKU'lar sunmak ve tüketim tabanlı işlevleri sunmak için en az 1 A1 örnek kullanılabilir olması gerekir.  Çok kiracılı paylaşılan çalışanlar, bu nedenle birden çok müşteri uygulamalarını barındırmak, her ayrı ayrı yalıtılmış App Service korumalı alanı tarafından korunan.  Bu şekilde paylaşılan çalışan katmanı ölçeğini genişleterek 1vCPU aylık maliyet, outlay sınırlayabilirsiniz.

Ayrıca, ardından yalnızca ücretsiz ve paylaşılan SKU'lar sunar ve müşteri oluşturabilirsiniz ücretsiz ve paylaşılan app service planları sayısını sınırlayan bir plan kullanmak için bir kota oluşturmayı seçebilirsiniz.

## <a name="sample-scripts-to-assist-with-billing"></a>Faturalandırmayla yardımcı olacak örnek betikler

Azure Stack kullanım hizmeti sorgulama ile kiracıları kendi faturalandırma hazırlamak için bulut operatörlerinin yardımcı olmak için yardımcı olmak için Azure App Service ekip oluşturulan örnek PowerShell betikleri.  Örnek betikler bulunabilir [Azure Stack araçları depo](https://github.com/Azure/AzureStack-tools) GitHub ve App Service içinde betikleridir [AppService klasör kullanımı altında](https://github.com/Azure/AzureStack-Tools/tree/master/Usage/AppService).

Örnek betikler şunlardır:

- [Get-AppServiceBillingRecords](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceBillingRecords.ps1) -Bu örnek, Azure Stack kullanım API'si kayıtlardan faturalama Azure Stack'te Azure App Service getirir
- [Get-AppServiceSubscriptionUsage](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceSubscriptionUsage.ps1) -Bu örnek, Azure Stack abonelik başına kullanım tutarları üzerinde Azure App Service hesaplar.  Bu betik, kullanım API'si ve ölçüm bulut operatör tarafından sağlanan fiyatlar verilerini temel kullanım tutarları hesaplar.
- [Askıya alma UserSubscriptions](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Suspend-UserSubscriptions.ps1) - Bu örnek askıya alır veya kullanım sınırı bulut operatör tarafından belirtilen temel abonelik etkinleştirir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Kiracı kullanım API'si](azure-stack-tenant-resource-usage-api.md)

<!--Image references-->
[1]: ./media/app-service-billing-faq/app-service-worker-tiers.png