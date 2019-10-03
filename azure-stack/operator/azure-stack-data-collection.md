---
title: Azure Stack günlüğü ve müşteri verilerini işleme | Microsoft Docs
description: Azure Stack müşteri verilerini ve bilgilerini nasıl toplayıp topladığını öğrenin.
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
ms.openlocfilehash: 31fa9bcdf60b27b2eb2e8396be1d2a0e89f31b70
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829181"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack günlüğü ve müşteri verileri işleme 
*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Microsoft 'un Azure Stack ile bağlantılı olarak kişisel verilerin bir işlemcisi veya alt işlemcisi olduğu için, Microsoft tüm müşterileri, geçerli 25 Mayıs 2018, aşağıdaki taahhütlere sahiptir:

- "Kişisel verileri Işleme; [Çevrimiçi hizmet koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)'Nın "veri koruma koşulları" bölümünde "GDPR" sağlayın.
- Avrupa Birliği, [çevrimiçi hizmet koşullarının](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)ek 4 ' te yer aldığı terimleri genel veri koruma yönetmeliği.

Azure Stack müşteri veri merkezlerinde bulunduğu için, Microsoft, yalnızca [Tanılama](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs), [telemetri](azure-stack-telemetry.md)ve [faturalandırma](azure-stack-usage-reporting.md)aracılığıyla Microsoft ile paylaşılan verilerden oluşan veri denetleyicisidir.  

## <a name="data-access-controls"></a>Veri erişim denetimleri 
Belirli bir destek durumunu araştırmak için atanan Microsoft çalışanlarına, şifrelenmiş verilere salt okuma erişimi verilecektir. Microsoft çalışanları, gerekirse verileri silmek için kullanılan araçlara de erişebilir. Müşteri verilerine tüm erişim denetlenir ve günlüğe kaydedilir.  

Veri erişim denetimleri:
- Veriler yalnızca, büyük/küçük harf kapatıldıktan sonra maksimum 90 gün boyunca tutulur.
- Müşterinin her zaman, söz konusu 90 günlük dönemde verileri her zaman kaldırma seçeneği vardır.
- Microsoft çalışanları, büyük/küçük harfe göre verilere erişim ve yalnızca destek sorununu çözmeye yardımcı olmak için gereklidir.
- Microsoft 'un müşteri verilerini OEM iş ortaklarıyla paylaşması gereken olayda, müşteri onayı zorunludur.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>Müşterilerin hangi veri sahibi Istekleri (DSR) denetimleri vardır?
Microsoft, müşteri isteği başına isteğe bağlı veri silmeyi destekler. Müşteriler, veri kalıcı olarak silinmeden önce, destek mühendislerimizden herhangi bir zamanda belirli bir durumda tüm günlüklerini silmesini isteyebilir.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>Microsoft, veriler silindiğinde müşterilere bilgilendirsin mi?
Otomatik veri silme eylemi için (büyük/küçük bir süre sonra 90 gün sonra), müşterilere etkin bir şekilde başvurmayız ve silme hakkında bildirim göndereceğiz.

İsteğe bağlı veri silme eylemi için, Microsoft Destek mühendislerinin istek üzerine verileri silmesine izin veren araca erişimi vardır. Müşteriler tamamlandığında müşteriyle ilgili onay sunabilir.

## <a name="diagnostic-data"></a>Tanılama verileri
Destek sürecinin bir parçası olarak Azure Stack Işleçler, sorun gidermeye yardımcı olmak için Azure Stack destek ve mühendislik ekipleriyle [tanılama günlüklerini paylaşabilir](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs) .

Microsoft, müşterilerin istenen tanılama günlük dosyalarını toplayıp yüklemesi için bir araç ve betik sağlar. Toplandıktan sonra, günlük dosyaları HTTPS ile korunan şifreli bir bağlantı üzerinden Microsoft 'a aktarılır. HTTPS, kablo üzerinden şifrelemeyi sağladığından, aktarım sırasında şifreleme için gereken parola yok. Bunlar alındıktan sonra, destek durumu kapatıldıktan sonraki 90 gün sonra otomatik olarak silinene kadar Günlükler şifrelenir ve depolanır.

## <a name="telemetry-data"></a>Telemetri verileri
[Azure Stack telemetri](azure-stack-telemetry.md) , bağlı kullanıcı deneyimi aracılığıyla sistem verilerini Microsoft 'a otomatik olarak yükler. Azure Stack Işleçleri, telemetri özelliklerini ve gizlilik ayarlarını dilediğiniz zaman özelleştirmek için denetimlere sahiptir.

Microsoft, kredi kartı numaraları, Kullanıcı adları ve parolalar, e-posta adresleri vb. gibi hassas veriler toplamaya karar vermez. Gizli bilgilerin yanlışlıkla alındığını belirlediğimiz takdirde, bu bilgileri sileriz.

## <a name="billing-data"></a>Faturalandırma verileri
[Azure Stack faturalandırma](azure-stack-usage-reporting.md) küresel Azure 'ın faturalandırma ve kullanım ardışık düzenine yararlanır ve bu nedenle Microsoft Uyumluluk kılavuzlarıyla hizalanır.

Azure Stack Işleçler, faturalandırma için kullanım bilgilerini Azure 'a iletecek Azure Stack yapılandırabilir. Bu yapılandırma, Kullandıkça Öde faturalandırma modelini seçen Azure Stack tümleşik sistemler müşterileri için gereklidir. Kullanım raporlaması telemetriden bağımsız olarak denetlenir ve kapasite modelini veya Azure Stack Geliştirme Seti kullanıcıları seçen tümleşik sistemler müşterileri için gerekli değildir. Bu senaryolar için, kullanım raporlaması [kayıt betiği](azure-stack-usage-reporting.md)kullanılarak kapatılabilir.


## <a name="next-steps"></a>Sonraki adımlar 
[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md) 
