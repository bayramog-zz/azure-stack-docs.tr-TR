---
title: Azure Stack genel bakış | App Service | Microsoft Docs
description: Azure Stack App Service genel bakış.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: anwestg
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 698d38e971d37457d50637cb2e7e2ffe88890c20
ms.sourcegitcommit: 7d7a4c8c46613b6104caf23763bfd2275f6a826b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70808230"
---
# <a name="app-service-on-azure-stack-overview"></a>Azure Stack’te App Service’e genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Azure App Service, Azure Stack üzerinde kullanılabilir Microsoft Azure bir hizmet olarak platform (PaaS) tekliftir. Hizmet, iç veya dış müşterilerinizin herhangi bir platform veya cihaz için Web, API ve Azure Işlevleri uygulamaları oluşturmalarına olanak sağlar. Uygulamalar Şirket içi uygulamalarla tümleşebilir ve iş süreçlerini otomatikleştirebilir. Azure Stack bulut işleçleri, şirket uygulamalarını, tam olarak yönetilen sanal makinelerde (VM), paylaşılan VM kaynakları veya adanmış VM 'Ler üzerinde çalıştırabilir.

Azure App Service, iş süreçlerini otomatikleştirmenizi ve bulut API 'Lerini barındırmanızı sağlar. Tek bir tümleşik hizmet olarak, Azure App Service Web siteleri, REST API 'Ler ve iş süreçlerini gibi çeşitli bileşenleri tek bir çözümde birleştirmenize olanak tanır.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Neden Azure Stack Azure App Service sundunuz?

App Service’in temel özelliklerinden bazıları şunlardır:

- **Birden çok dil ve çerçeve**: App Service, ASP.NET, Node. js, Java, PHP ve Python için birinci sınıf desteğe sahiptir. Ayrıca, App Service VM 'lerde Windows PowerShell ve diğer betikleri veya yürütülebilir dosyaları da çalıştırabilirsiniz.
- **DevOps iyileştirmesi**: GitHub, Local git veya BitBucket ile sürekli tümleştirme ve dağıtım ayarlayın. Azure PowerShell veya platformlar arası komut satırı arabirimi (CLı) kullanarak güncelleştirmeleri test ve hazırlama ortamları aracılığıyla yükselterek App Service uygulamalarınızı yönetin.
- **Visual Studio tümleştirmesi**: Visual Studio 'da adanmış araçlar, uygulama oluşturma ve dağıtma işini kolaylaştırır.

## <a name="app-types-in-app-service"></a>App Service’deki uygulama türleri

App Service, her biri belirli bir iş yükünü barındırmak amaçlanan birkaç uygulama türü sunar:

- Web sitelerini ve Web uygulamalarını barındırmak için [Web Apps](/azure/app-service/overview) .
- REST API 'Leri barındırmak için [API Apps](/azure/app-service/overview) .
- Olay odaklı, sunucusuz iş yüklerini barındırmak için Azure Işlevleri.

Word *uygulaması* , bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. *Web uygulamasını* örnek olarak ele almak, bir Web uygulamasını bir tarayıcıya işlevselliği sağlayan işlem kaynakları ve uygulama kodu olarak düşünmek için büyük olasılıkla alışkın olabilirsiniz. App Service, bir Web uygulaması, uygulama kodunuzu barındırmak için Azure Stack sağladığı işlem kaynağıdır.

Uygulamanız farklı türlerdeki birden çok App Service uygulamadan oluşabilir. Örneğin, uygulamanız bir Web ön ucu ve bir REST API arka ucu içeriyorsa şunları yapabilirsiniz:

- Her ikisini de (ön uç ve API) tek bir Web uygulamasına dağıtma
- Ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtma.

   [![İzleme verileriyle ilgili App Service genel bakış](media/azure-stack-app-service-overview/image01.png "İzleme verileriyle ilgili App Service genel bakış")](media/azure-stack-app-service-overview/image01.png#lightbox)

## <a name="what-is-an-app-service-plan"></a>App Service planı nedir?

App Service kaynak sağlayıcısı, Azure App Service tarafından kullanılan kodu kullanır ve bu nedenle bazı yaygın kavramları paylaşır. App Service, uygulamalar için fiyatlandırma kapsayıcısına *App Service planı*denir. Uygulamalarınızı tutmak için kullanılan adanmış VM 'lerin kümesini temsil eder. Belirli bir abonelik içinde, birden fazla App Service planınız olabilir.

Azure 'da paylaşılan ve adanmış çalışanlar vardır. Paylaşılan bir çalışan, yüksek yoğunluklu ve çok kiracılı uygulama barındırmayı destekler ve yalnızca bir paylaşılan çalışan iş kümesi vardır. Ayrılmış sunucular yalnızca bir kiracı tarafından kullanılır ve üç boyutta gelir: küçük, orta ve büyük. Şirket içi müşterilerin ihtiyaçları, bu terimler kullanılarak her zaman açıklanmamaktadır. Azure Stack App Service, kaynak sağlayıcısı yöneticileri kullanılabilir hale getirmek istedikleri çalışan katmanlarını tanımlayabilir. Benzersiz barındırma gereksinimlerinize göre, birden fazla paylaşılan çalışan kümesi veya farklı adanmış çalışan kümesi tanımlayabilirsiniz. Bu çalışan katmanı tanımlarını kullanarak, kendi fiyatlandırma SKU 'Larını tanımlayabilirler.

## <a name="portal-features"></a>Portal özellikleri

Azure Stack App Service Azure App Service kullandığı kullanıcı arabirimini kullanır. Arka uçta de aynı değer geçerlidir. Ancak bazı özellikler Azure Stack devre dışı bırakılır. Bu özelliklerin gerektirdiği Azure 'a özgü beklentiler veya hizmetler şu anda Azure Stack ' de kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack App Service dağıtmaya yönelik önkoşullar](azure-stack-app-service-before-you-get-started.md)
- [App Service kaynak sağlayıcısını yükler](azure-stack-app-service-deploy.md)

Ayrıca, [SQL Server kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) ve [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md)gibi başka [bir hizmet olarak platform (PaaS) hizmeti](azure-stack-offer-services-overview.md)de deneyebilirsiniz.
