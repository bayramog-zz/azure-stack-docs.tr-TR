---
title: Azure App Service, Azure Stack fatura genel bakış ve sık sorulan sorular | Microsoft Docs
description: Azure Stack'te Azure App Service nasıl hesaplanır ve faturalandırılır ayrıntıları.
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
ms.openlocfilehash: 8b17020419183067793309055a6487f31da51246
ms.sourcegitcommit: d67ae2877e3b049bf70b2567b917e55394fb8984
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67712690"
---
# <a name="azure-app-service-on-azure-stack-billing-overview-and-faq"></a>Azure App Service, Azure Stack fatura genel bakış ve sık sorulan sorular

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, bulut operatörlerinin Azure Stack'te Azure App Service teklifi için nasıl faturalandırılır ve bunlar, kiracıları hizmet kullanımları için nasıl fatura gösterir.

## <a name="billing-overview"></a>Faturaya genel bakış

Azure Stack bulut operatörleri, Azure App Service ve Azure işlevleri Kiracı yeteneklerini müşterilerinin kullanımına sunmak için Azure Stack damga üzerine Azure Stack'te Azure App Service dağıtmak seçin. Azure App Service kaynak sağlayıcısı, birden çok tür altyapı ve çalışan katmanları arasında bölünebilir rolleri oluşur.

Hizmetin temel işlevleri için gerekli olduğundan altyapısı rollerinin faturalandırılır değildir. Altyapı rollerini bulut işlecin kiracılar taleplerini desteklemek için gerektiği şekilde genişletilebilir. Altyapı rollerini aşağıdaki gibidir:

- Denetleyiciler
- Yönetim rolleri
- Yayımcılar
- Ön Uçlar

Çalışan katmanları, iki ana tür oluşur: paylaşılan ve ayrılmış. Alt kullanım bulut işleci aşağıdaki ölçütlere göre faturalandırılır.

## <a name="shared-workers"></a>Paylaşılan çalışan

Çok kiracılı ve konak ücretsiz ve paylaşılan App Service planları ve birçok kiracısı için Azure işlevleri tüketim tabanlı bunun paylaşılan çalışanlardır. Paylaşılan çalışan Azure App Service kaynak sağlayıcısı hazır olarak işaretlendiğinde kullanım ölçümleri gösterin.

## <a name="dedicated-workers"></a>Adanmış çalışanları

Adanmış çalışanları App Service planlarına kiracılarınızı oluşturmanız gerektirir. Örneğin, S1 SKU'ya kiracılar varsayılan olarak 10 örneğe genişletebilir. Kiracı S1 App Service planı oluşturur, Azure App Service, kiracının App Service planına küçük çalışan katmanı ölçek örneklerinden birinde ayırır. Atanan alt sonra artık diğer kiracılar için atanacak kullanılamıyor. Kiracı ölçeği 10 örneğe App Service planı seçerse dokuz çalışan daha kullanılabilir havuzundan kaldırılır ve kiracının App Service planına atanan.

Olduklarında ölçümleri adanmış çalışanların gönderilir:

- Azure App Service kaynak sağlayıcısı hazır olarak işaretlendi.
- Bir App Service planına atanan.

Bu faturalandırma modeli, bulut operatörlerinin adanmış çalışan müşterilerin kiracısının App Service planı tarafından etkili bir şekilde ayrılmış kadar çalışanları için ödeme yapmadan kullanmak hazır bir havuz sağlama sağlar. 

Örneğin, küçük çalışan katmanı 20 çalışanları olduğunu varsayalım. Ardından, oluşturduğunuz iki S1 App Service planı her beş imajlarını ve her iki örnek kadar App Service planını ölçeklendirme, kullanılabilir çalışanlar sağlayın. Sonuç olarak, var. Ayrıca müşterilerinize veya yeni müşterilerin ölçeği genişletmek veya yeni bir App Service planları oluşturmak için hiçbiri için hiçbir kapasite 

Bulut operatörleri, Azure Stack Yönetim üzerinde Azure App Service yapılandırmasında çalışan katmanları bakarak başına çalışan katmanı kullanılabilir çalışanların geçerli sayısını görüntüleyebilirsiniz.

![App Service - çalışan katmanları ekranı][1]

## <a name="see-customer-usage-by-using-the-azure-stack-usage-service"></a>Azure Stack kullanım hizmeti kullanılarak müşteri kullanımını bakın

Bulut operatörleri sorgulayabilir [Azure Stack Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md) müşterileri için kullanım bilgileri alınamadı. Tüm Kiracı kullanımı açıklamak için App Service yayan bireysel ölçümleri bulabilirsiniz [kullanım SSS](azure-stack-usage-related-faq.md). Bu ölçümler, ardından kullanım ücretleri hesaplamak için müşteri abonelik başına hesaplamak için kullanılır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-license-the-sql-server-and-file-server-infrastructure-required-in-the-prerequisites"></a>Nasıl yapılır? lisans SQL Server ve dosya sunucu altyapısı gerekli önkoşulları

SQL Server ve dosya sunucu altyapısı, Azure App Service kaynak sağlayıcısı tarafından gerekli lisanslama Azure Stack'te Azure App Service'te alınmıştır [başlamadan önce](azure-stack-app-service-before-you-get-started.md#licensing-concerns-for-required-file-server-and-sql-server) makalesi.

### <a name="the-usage-faq-lists-the-tenant-meters-but-not-the-prices-for-those-meters-where-can-i-find-them"></a>Kullanım SSS, Kiracı ölçümleri ancak değil bu ölçümleri fiyatı listeler. Bunları nerede bulabilirim?

Bir bulut işleci olarak kendi fiyatlandırma modeli müşterilerinize uygulamak ücretsiz. Kullanım ölçümü kullanım hizmeti sağlar. Ardından, siz belirlersiniz fiyatlandırma modelini temel alan, müşterilere Ücret için ölçüm miktarı da kullanabilirsiniz. Diğer Azure Stack operatörlerinin ayırt etmek işleçleri fiyatlandırma ayarlamanıza olanak sağlar.

### <a name="as-a-csp-how-can-i-offer-free-and-shared-skus-for-customers-to-try-out-the-service"></a>Bir CSP olarak nasıl miyim hizmeti deneyebilmesi müşteriler için ücretsiz ve paylaşılan SKU'lar sunabilirsiniz?

Bir bulut operatör olarak, paylaşılan çalışanlar barındırılan çünkü sunan ücretsiz ve paylaşılan SKU'lar için ücret yansıtılmaz. Bu maliyeti en aza indirmek için en az için paylaşılan çalışan katmanı ölçeğini seçebilirsiniz. 

Örneğin, ücretsiz ve paylaşılan App Service planı SKU'ları sunan ve tüketim tabanlı işlevleri sunmak için en az bir A1 örneği gerekir. Paylaşılan çalışan çok kiracılı, olduğundan bunlar her ayrı ayrı yalıtılmış ve App Service korumalı alanı tarafından korunan birden fazla müşteri uygulamalarını barındırabilir. Bu şekilde paylaşılan çalışan katmanı ölçeğini genişleterek bir vCPU başına aylık maliyet, outlay sınırlayabilirsiniz.

Yalnızca ücretsiz ve paylaşılan SKU'lar sunar ve müşteri oluşturabilirsiniz ücretsiz ve paylaşılan App Service planları sayısını sınırlayan bir plan kullanmak için bir kota oluşturmak seçim yapabilirsiniz.

## <a name="sample-scripts-to-assist-with-billing"></a>Faturalandırmayla yardımcı olacak örnek betikler

Azure App Service ekibi örnek PowerShell betikleri ile Azure Stack kullanım hizmeti sorgulama yardımcı olmak için oluşturdu. Bulut operatörleri, kiracıları kendi faturalandırma hazırlamak için bu örnek komut dosyalarını kullanabilirsiniz. Örnek betikler bulunan [Azure Stack araçları depo](https://github.com/Azure/AzureStack-tools) github'da. App Service içinde betikleridir [AppService klasör kullanımı altında](https://github.com/Azure/AzureStack-Tools/tree/master/Usage/AppService).

Örnek betikler şunlardır:

- [Get-AppServiceBillingRecords](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceBillingRecords.ps1): Bu örnek Azure App Service, Azure Stack fatura kayıtlarını Azure Stack kullanım API'SİNDEN getirir.
- [Get-AppServiceSubscriptionUsage](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceSubscriptionUsage.ps1): Bu örnek, Azure Stack abonelik başına kullanım tutarları üzerinde Azure App Service hesaplar. Bu betik, kullanım API'si ve ölçüm bulut operatör tarafından sağlanan fiyatlar verilerini temel kullanım tutarları hesaplar.
- [Askıya alma UserSubscriptions](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Suspend-UserSubscriptions.ps1): Bu örnek askıya alır ya da bulut işleci tarafından belirtilen kullanım sınırları temel abonelikleri etkinleştirir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)

<!--Image references-->
[1]: ./media/app-service-billing-faq/app-service-worker-tiers.png
