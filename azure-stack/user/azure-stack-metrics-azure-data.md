---
title: Azure Stack Azure Izleyici 'yi kullanma | Microsoft Docs
description: Azure Stack 'de Azure Izleyici 'yi kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2019
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 71855d557f26fc1eb4d86ab0d5a48bf8cf987506
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974668"
---
# <a name="use-azure-monitor-on-azure-stack"></a>Azure Stack Azure Izleyicisini kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, Azure Stack 'deki Azure Izleyici hizmetine genel bir bakış sunulmaktadır. Azure Izleyici 'nin işlemini ve Azure Stack üzerinde Azure Izleyici kullanma hakkında ek bilgiler açıklanmaktadır. 

Azure Izleyici 'ye genel bakış için [Azure Stack Azure izleyici 'yi kullanmaya başlama başlıklı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started)küresel Azure makalesine bakın.

![Azure Stack Izleyici dikey penceresi](./media/azure-stack-metrics-azure-data/azs-monitor.png)

Azure Izleyici, Azure kaynaklarını izlemeye yönelik tek bir kaynak sağlayan bir platform hizmetidir. Azure Izleyici, Azure 'daki kaynaklardan gelen ölçümler ve Günlükler üzerinde görselleştirme, sorgulama, yönlendirme, arşivleme ve diğer eylemleri gerçekleştirmenizi sağlar. Bu verilerle, Azure Stack Yönetici portalı, Izleme PowerShell cmdlet 'Leri, platformlar arası CLı veya Azure Izleyici REST API 'Leri kullanarak çalışabilirsiniz. Azure Stack tarafından desteklenen belirli bir bağlantı için bkz. [Azure Stack izleme verilerini kullanma](azure-stack-metrics-monitor.md).

> [!Note]
> Ölçümler ve tanılama günlükleri Azure Stack Geliştirme Seti için kullanılamaz.

## <a name="prerequisites-for-azure-monitor-on-azure-stack"></a>Azure Stack 'de Azure Izleyici önkoşulları

**Microsoft. Insights** kaynak sağlayıcısını, aboneliğinizin teklif kaynak sağlayıcıları ayarlarına kaydedin. Kaynak sağlayıcının, aboneliğinizle ilişkili teklifiniz için kullanılabilir olduğunu doğrulayabilirsiniz:

1. Azure Stack Kullanıcı portalını açın.
2. **Teklifler**' i seçin.
3. Abonelikle ilişkili teklifi seçin.
4. Ayarlar altında **kaynak sağlayıcıları** ' nı seçin **.** 
5. Listede **Microsoft. Insights** 'ı bulun ve durumun **kayıtlı**olduğunu doğrulayın.

## <a name="overview-of-azure-monitor-on-azure-stack"></a>Azure Stack Azure Izleyici 'ye Genel Bakış

Azure'da Azure İzleyici gibi Azure Stack'te Azure İzleyici, temel düzeyde altyapı ölçümlerini ve günlüklerini çoğu hizmetleri sağlar.

## <a name="azure-monitor-sources-compute-subset"></a>Azure Izleyici kaynakları: işlem alt kümesi

![Azure Stack kaynaklarında Azure Izleyici-işlem alt kümesi](media//azure-stack-metrics-azure-data/azs-monitor-computersubset.png)

Azure Stack içindeki **Microsoft. COMPUTE** kaynak sağlayıcısı şunları içerir:
 - Sanal makineler 
 - Sanal makine ölçek kümeleri

### <a name="application---diagnostics-logs-app-logs-and-metrics"></a>Uygulama tanılama günlükleri, uygulama günlükleri ve ölçümler

Uygulamalar, **Microsoft. COMPUTE** kaynak sağlayıcısı ile ÇALıŞTıRAN bir VM 'nin işletim sisteminde çalışabilir. Bu uygulamalar ve VM 'Ler kendi günlük ve ölçümleri kümesini yayar. Azure Izleyici, çoğu uygulama düzeyi ölçüm ve günlüğü toplamak için Azure tanılama uzantısı 'nı (Windows veya Linux) kullanır.

Ölçü türleri şunlardır:
 - Performans sayaçları
 - Uygulama günlükleri
 - Windows olay günlükleri
 - .NET olay kaynağı
 - IIS günlükleri
 - Bildirim tabanlı ETW
 - Kilitlenme dökümleri
 - Müşteri hata günlükleri

> [!Note]  
> Azure Stack üzerindeki Linux Tanılama uzantısı desteklenmiyor.

### <a name="host-and-guest-vm-metrics"></a>Konak ve Konuk VM ölçümleri

Daha önce listelenen işlem kaynakları ayrılmış bir konak VM 'sine ve konuk işletim sistemine sahiptir. Konak VM ve konuk işletim sistemi, Hyper-V Hiper yöneticide kök VM ve konuk VM 'nin eşdeğeridir. Hem konak VM hem de konuk işletim sistemi için ölçümleri toplayabilirsiniz. Konuk işletim sistemi için tanılama günlükleri de toplayabilirsiniz. Azure Stack üzerindeki konak ve konuk VM ölçümleri için toplanabilir ölçümlerin bir listesi [Azure Stack Azure izleyici Ile desteklenen ölçümlerde](azure-stack-metrics-supported.md)mevcuttur. 

### <a name="activity-log"></a>Etkinlik günlüğü

Azure Stack altyapısında görüldüğü gibi işlem kaynaklarınız hakkında bilgi edinmek için etkinlik günlüklerinde arama yapabilirsiniz. Günlük, kaynakların oluşturulduğu veya yok edildiği zamanlar gibi bilgiler içerir. Azure Stack etkinlik günlükleri Azure ile tutarlıdır. Daha fazla bilgi için bkz. [Azure 'Da etkinlik günlüğüne genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)açıklamasına bakın. 


## <a name="azure-monitor-sources-everything-else"></a>Azure izleyici kaynakları: diğer her şey

![Azure Stack kaynaklarında Azure Izleyici-diğer her şey](media//azure-stack-metrics-azure-data/azs-monitor-othersubset.png)

### <a name="resources---metrics-and-diagnostics-logs"></a>Kaynaklar-ölçümler ve tanılama günlükleri

Toplanabilir ölçümler ve tanılama günlükleri, kaynak türüne göre farklılık gösterir. Azure Stack üzerindeki her kaynak için toplanabilir ölçümlerin bir listesi desteklenen ölçümlerde mevcuttur. Daha fazla bilgi için bkz. [Azure Stack Azure izleyici Ile desteklenen ölçümler](azure-stack-metrics-supported.md).

### <a name="activity-log"></a>Etkinlik günlüğü

Etkinlik günlüğü, işlem kaynakları için aynıdır. 

### <a name="uses-for-monitoring-data"></a>İzleme verileri için kullanımlar

**Depola ve Arşivle**  

Bazı izleme verileri Azure İzleyici'de belirli bir süre boyunca depolanır ve kullanılabilir. 
 - Ölçümler 90 gün süreyle depolanır. 
 - Etkinlik günlüğü girişleri 90 gün süreyle depolanır. 
 - Tanılama günlükleri depolanmaz.
 - Daha uzun bekletme için verileri bir depolama hesabına arşivleyin.

**Sorgu**  

Azure Izleyici REST API, platformlar arası komut satırı arabirimi (CLı) komutlarını, PowerShell cmdlet 'lerini veya .NET SDK 'sını kullanarak sistemdeki verilere veya Azure Storage 'a erişebilirsiniz. 

**Ğiyle**

İzleme verilerinizi grafik ve tablo olarak görselleştirmek eğilimleri doğrudan veriye bakmaya göre daha hızlı bulmanıza yardımcı olur. 

Görselleştirme yöntemlerinden bazıları şunlardır:
 - Azure Stack Kullanıcı ve Yönetici portalı ' nı kullanın.
 - Verileri Microsoft Power BI 'a yönlendirin.
 - Canlı akış kullanarak veya aracın Azure Storage 'daki bir arşivden okunmasından yararlanarak, verileri üçüncü taraf bir görselleştirme aracına yönlendirin.

## <a name="methods-of-accessing-azure-monitor-on-azure-stack"></a>Azure Stack Azure izleyici 'ye erişme yöntemleri

Aşağıdaki yöntemlerden birini kullanarak veri izleme, Yönlendirme ve alma ile çalışabilirsiniz. Tüm eylemler veya veri türleri için tüm yöntemler kullanılabilir olmayabilir. 

 - [Azure Stack Kullanıcı Portalı](azure-stack-use-portal.md)
 - [PowerShell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples)
 - [Platformlar arası komut satırı arabirimi (CLı)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples)
 - [REST API](https://docs.microsoft.com/rest/api/monitor)
 - [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

> [!Important]  
> Bir VM 'nin performans grafiğini görüntülerken **kaynak bulunamadı** hatasıyla karşılaşırsanız, Microsoft. ıNSIGHTS 'ı VM ile ilişkili abonelikte kaydettiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack üzerinde veri tüketimini izleme hakkında daha fazla bilgi edinin [Azure Stack izleme verilerini tüketme](azure-stack-metrics-monitor.md)makalesindeki.
