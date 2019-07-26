---
title: Günlük ve veri işleme Azure Stack | Microsoft Docs
description: Azure Stack nasıl bilgi topladığı hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: patricka
ms.reviewer: chengwei
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 495b75359cb8c859e532885a1c9fa284691bd90f
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68493805"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack günlüğü ve müşteri verileri işleme 
*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Microsoft, Microsoft 'un Azure Stack ile bağlantılı olarak kişisel verilerin bir işlemcisi veya alt işlemcisidir. Microsoft, "kişisel verileri Işleme" içindeki (a) taahhütlik, 25 Mayıs 2018 ' de tüm müşterileri sağlar. GDPR, çevrimiçi hizmetler koşullarının "veri koruma koşulları" [bölümünün ve (](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) b), [çevrimiçi hizmet koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)'nın ek 4 ' te bulunan Avrupa Birliği genel veri koruma yönetmeliği terimleri. 

Azure Stack müşteri veri merkezlerinde bulunduğu için, Microsoft, yalnızca [Tanılama](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep), [telemetri](azure-stack-telemetry.md)ve [faturalandırma](azure-stack-usage-reporting.md)aracılığıyla Microsoft ile paylaşılan verilerden oluşan veri denetleyicisidir.  

## <a name="data-access-controls"></a>Veri erişim denetimleri 
Belirli bir destek durumunu araştırmak için atanan Microsoft çalışanlarına, şifrelenmiş verilere salt okuma erişimi verilecektir. Microsoft çalışanları, gerekirse verileri silmek için kullanılan araçlara de erişebilir. Müşteri verilerine tüm erişim denetlenir ve günlüğe kaydedilir.  

Veri erişim denetimleri:
- Veriler yalnızca, büyük/küçük harf kapatıldıktan sonra en fazla 90 gün boyunca tutulur.
- Müşterinin her zaman, söz konusu 90 günlük dönemde verileri her zaman kaldırma seçeneği vardır.
- Microsoft çalışanları, büyük/küçük harfe göre verilere erişim ve yalnızca destek sorununu çözmeye yardımcı olmak için gereklidir. 
- Microsoft 'un müşteri verilerini OEM iş ortaklarıyla paylaşması gereken olayda, müşteri onayı zorunludur.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>Müşterilerin hangi veri sahibi Istekleri (DSR) denetimleri vardır?
Daha önce belirtildiği gibi, Microsoft müşteri isteği başına isteğe bağlı veri silmeyi destekler. Müşteriler, veri kalıcı olarak silinmeden önce, destek mühendisimizin herhangi bir zamanda belirli bir servis talebi için tüm günlüklerini silmesini talep edebilir.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>Microsoft, veriler silindiğinde müşterilere bilgilendirsin mi?
Otomatik veri silme eylemi için (büyük/küçük bir süre sonra 90 gün sonra), müşterilerle ilgili olarak iletişim kurmazlar ve silme hakkında bildirim göndereceğiz. 

İsteğe bağlı veri silme eylemi için, Microsoft destek mühendisinin istek üzerine veri silme işlemini başlatabilecekleri ve tamamlandığında müşteriyle ilgili onay sağlayabilecekleri araca erişimi vardır.

## <a name="diagnostic-data"></a>Tanılama verileri
Azure Stack Işleçleri, destek sürecinin bir parçası olarak, sorun gidermeyi kolaylaştırmak için Azure Stack destek ve mühendislik ekipleriyle [tanılama günlüklerini paylaşabilir](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep) .

Microsoft, müşterilerin istenen tanılama günlük dosyalarını toplayıp yüklemesi için bir araç ve betik sağlar. Toplandıktan sonra, günlük dosyaları HTTPS ile korunan şifreli bir bağlantı üzerinden Microsoft 'a aktarılır. HTTPS, kablo üzerinde şifrelemeyi sağladığından, aktarım sırasında şifreleme için gereken parola yok. Bunlar alındıktan sonra, destek durumu kapatıldıktan sonraki 90 gün sonra otomatik olarak silinene kadar Günlükler şifrelenir ve depolanır.

## <a name="telemetry-data"></a>Telemetri verileri
[Azure Stack telemetri](azure-stack-telemetry.md) , bağlı kullanıcı deneyimi aracılığıyla sistem verilerini Microsoft 'a otomatik olarak yükler. Azure Stack Işleçleri, telemetri özelliklerini ve gizlilik ayarlarını dilediğiniz zaman özelleştirmek için denetimlere sahiptir.

Microsoft, kredi kartı numaraları, Kullanıcı adları ve parolalar, e-posta adresleri veya benzer gizli bilgiler gibi hassas veriler toplamaya yönelik değildir. Gizli bilgilerin yanlışlıkla alındığını belirlediğimiz takdirde, bu bilgileri sileriz. 

## <a name="billing-data"></a>Faturalandırma verileri
[Azure Stack faturalandırma](azure-stack-usage-reporting.md) küresel Azure 'ın faturalandırma ve kullanım ardışık düzenine yararlanır ve bu nedenle Microsoft Uyumluluk kılavuzlarıyla hizalanır.

Azure Stack Işleçler, faturalandırma için kullanım bilgilerini Azure 'a iletecek Azure Stack yapılandırabilir. Bu, Kullandıkça Öde faturalandırma modeli 'ni seçen çok düğümlü Azure Stack müşteriler için gereklidir. Kullanım raporlaması telemetriden bağımsız olarak denetlenir ve kapasite modelini veya Azure Stack Geliştirme Seti kullanıcıları seçen çok düğümlü müşteriler için gerekli değildir. Bu senaryolar için, kullanım raporlaması [kayıt betiği](azure-stack-usage-reporting.md)kullanılarak kapatılabilir.


## <a name="next-steps"></a>Sonraki adımlar 
[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md) 
