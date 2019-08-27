---
title: Azure Stack telemetri | Microsoft Docs
description: PowerShell kullanarak Azure Stack telemetri ayarlarını yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: c904f77834e9195ab942f13028fe5ef2fc7a5366
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025834"
---
# <a name="azure-stack-telemetry"></a>Azure Stack telemetri

Azure Stack sistem verileri veya telemetri, bağlantılı Kullanıcı deneyimi aracılığıyla Microsoft 'a otomatik olarak yüklenir. Azure Stack telemetrisinden toplanan veriler, birincil olarak müşteri deneyimimizi geliştirmek için Microsoft ekipleri tarafından kullanılır. Güvenlik, sistem durumu, kalite ve performans analizi için de kullanılır.

Azure Stack bir operatör olarak, telemetri kurumsal dağıtımlara değerli içgörüler sunabilir ve size Azure Stack gelecek sürümlerini şekillendirmeye yardımcı olan bir ses sağlar.

> [!NOTE]
> Azure Stack Ayrıca, faturalandırma için kullanım bilgilerini Azure 'a iletecek şekilde yapılandırılabilir. Bu, Kullandıkça Öde faturalandırması 'nı seçen çok düğümlü Azure Stack müşteriler için gereklidir. Kullanım raporlaması telemetriden bağımsız olarak denetlenir ve kapasite modelini veya Azure Stack Geliştirme Seti (ASDK) kullanıcılarını seçen çok düğümlü müşteriler için gerekli değildir. Bu senaryolar için, kullanım raporlaması [kayıt betiği kullanılarak](../operator/azure-stack-usage-reporting.md)kapatılabilir.

Azure Stack telemetri, telemetri olaylarını ve verilerini toplamak ve depolamak üzere [Windows Için olay izleme (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) izleme günlüğü teknolojisini kullanan *Windows Server 2016 bağlantılı Kullanıcı deneyimi ve telemetr*y bileşenini temel alır. Azure Stack bileşenleri, genel işletim sistemi olay günlüğü ve izleme API 'Leri kullanılarak toplanan olayları ve verileri yayımlamak için aynı günlük teknolojisini kullanır. Azure Stack bileşenlere örnek olarak ağ kaynak sağlayıcısı, depolama kaynak sağlayıcısı, Izleme kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı verilebilir. Bağlı kullanıcı deneyimi ve telemetri bileşeni, SSL kullanarak verileri şifreler ve Microsoft Veri Yönetimi hizmetine HTTPS üzerinden telemetri verileri aktarmak için sertifika sabitleme kullanır.

> [!NOTE]
> Telemetri veri akışını desteklemek için, ağınızda bağlantı noktası 443 (HTTPS) açık olmalıdır. Bağlı kullanıcı deneyimi ve telemetri bileşeni, https://v10.vortex-win.data.microsoft.com ' https://settings-win.data.microsoft.com de Microsoft veri yönetimi hizmetine bağlanır ve ayrıca yapılandırma bilgilerini indirmek için.

## <a name="privacy-considerations"></a>Gizlilik konuları
ETW hizmeti rotaları, telemetri verilerini korumalı bulut depolamaya geri gönderir. En az ayrıcalıklı kılavuz, telemetri verilerine erişim prensibi. Yalnızca geçerli bir iş gerektiren Microsoft personeline telemetri verilerine erişim izni verilir. Microsoft, müşterinin veya [Azure Stack gizlilik bildiriminde](https://privacy.microsoft.com/PrivacyStatement)açıklanan sınırlı amaçlar dışında müşterinin kişisel verilerini üçüncü taraflarla paylaşmaz. İş raporlarını, toplanan ve anonim telemetri bilgilerini içeren OEM 'Ler ve iş ortaklarıyla paylaşıyoruz. Veri paylaşımı kararları, gizlilik, yasal ve veri yönetimi katılımcıları dahil olmak üzere dahili bir Microsoft ekibi tarafından yapılır.

Microsoft, ve uygulama bilgilerinin en az düzeyde olması gerektiğine inanmaktadır. Yalnızca ihtiyacımız olan bilgileri topladık ve yalnızca bir hizmet sağlamak ya da analiz için gerekli olduğu sürece depolarız. Azure Stack sistemi ve Azure hizmetlerinin nasıl çalıştığı hakkında bilgi, altı ay içinde silinir. Özetlenen veya toplanmış veriler daha uzun bir süre tutulur.

Müşterilerin bilgilerinin gizliliği ve güvenliğinin önemli olduğunu anladık. Müşteri gizliliği ve Azure Stack ile müşteri verilerinin korunması için bir düşünceli ve kapsamlı bir yaklaşım yaptık. BT yöneticileri, özellikleri ve gizlilik ayarlarını dilediğiniz zaman özelleştirmek için denetimlere sahiptir. Saydamlıkla ve güvenimize yönelik taahhüdizin açık:
- Topladığımız veri türleri hakkında müşterilerle birlikte çalışıyoruz.
- Kurumsal müşterileri denetime yerleştiririz, kendi gizlilik ayarlarını özelleştirebilir.
- Önce Müşteri Gizlilik ve güvenlik yerleştiririz.
- Telemetriyi nasıl kullanılan hakkında saydamlıyoruz.
- Müşteri deneyimlerini geliştirmek için telemetri kullanıyoruz.

Microsoft, kredi kartı numaraları, Kullanıcı adları ve parolalar, e-posta adresleri gibi hassas bilgileri toplamaya yönelik değildir. Gizli bilgiler yanlışlıkla alındığını tespit ettiğimiz takdirde, bu bilgileri sileriz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft 'un telemetri verilerini nasıl kullandığı hakkında örnekler
Telemetri, müşterilerin dağıtımlarımızda ve yapılandırmalarında kritik güvenilirlik sorunlarını hızla tanımlayıp düzeltmenize yardımcı olan önemli bir rol oynar. Topladığımız telemetri verileri hakkında Öngörüler, hizmetlerle veya donanım yapılandırmalarının sorunlarını hızla belirlememize yardımcı olur. Microsoft 'un bu verileri müşterilerden alma ve ekosisteme yönelik iyileştirmeler, tümleşik Azure Stack çözümlerimizin kalitesi için çubuğun oluşturulmasına yardımcı olur.

Telemetri ayrıca Microsoft 'un müşterilerin bileşenleri nasıl dağıtdığını, özellikleri nasıl kullandığını ve iş hedeflerine ulaşmak için Hizmetleri nasıl kullandığını daha iyi anlamasına yardımcı olur. Bu verilerden Öngörüler elde etmek, müşterilerin deneyimlerini ve iş yüklerini doğrudan etkileyebilecek alanlardaki mühendislik yatırımlarının önceliklerini belirlemeye yardımcı olur.

Bazı örneklerde, Azure Stack rolleriyle ilişkili kapsayıcıların, depolamanın ve ağ yapılandırmalarının müşteri kullanımı sayılabilir. Ayrıca, Öngörüler ve zekası yönetim ve izleme çözümlerimizden bazılarına yönlendirmek için öngörüleri kullanırız. Bu geliştirme, müşterilerin Microsoft 'a daha az destek çağrısı yaparak kalite sorunlarını tanılamanıza ve paradan tasarruf etmenize yardımcı olur.

## <a name="manage-telemetry-collection"></a>Telemetri toplamayı Yönet
Telemetri, geliştirilmiş ürün işlevselliği ve kararlılığı destekleyen veriler sağladığından, kuruluşunuzda Telemetriyi kapatmanızı önermeyiz. Ancak bazı senaryolarda bunun gerekli olabileceğini anlayacağız.

Bu örneklerde, Microsoft 'a gönderilen telemetri düzeyini, kayıt defteri ayarları ön dağıtımını kullanarak veya telemetri uç noktaları dağıtım sonrası kullanarak yapılandırabilirsiniz.

### <a name="set-telemetry-level-in-the-windows-registry"></a>Windows kayıt defterinde telemetri düzeyini ayarlama
Windows kayıt defteri Düzenleyicisi, Azure Stack dağıtılmadan önce fiziksel ana bilgisayar bilgisayarındaki telemetri düzeyini el ile ayarlamak için kullanılır. Grup ilkesi gibi bir yönetim ilkesi zaten varsa, bu kayıt defteri ayarını geçersiz kılar.

ASDK ana bilgisayarında Azure Stack dağıtılmadan önce CloudBuilder. vhdx ' i önyükleyin ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:

```powershell
### Get current AllowTelemetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Telemetri düzeyleri birikimlidir ve dört düzeyde kategorize edilir (0-3):

**0 (güvenlik)** : Yalnızca güvenlik verileri. Bağlı kullanıcı deneyimi ve telemetri bileşen ayarları ve Windows Defender hakkındaki veriler de dahil olmak üzere işletim sistemini güvende tutmaya yardımcı olmak için gerekli bilgiler. Bu düzeyde Azure Stack belirli telemetri yayılmadı.

**1 (temel)** : Güvenlik verileri ve temel sistem durumu ve kalite verileri. Temel cihaz bilgileri: kalite ile ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verileri ve güvenlik düzeyinden veri. Telemetri düzeyinizi temel olarak ayarlamak Azure Stack telemetri sağlar. Bu düzeyde toplanan veriler şunları içerir:

- Ekosistemde yerel ve sanallaştırılmış Windows Server 2016 örneklerinin türleri ve konfigürasyonları hakkında bilgi sağlanmasına yardımcı olan **temel cihaz bilgileri** , örneğin:
  - OEM ve model gibi makine öznitelikleri.
  - Ağ bağdaştırıcılarının sayısı ve hızı gibi ağ öznitelikleri.
  - Çekirdek sayısı ve bellek boyutu gibi işlemci ve bellek öznitelikleri.
  - Depolama öznitelikleri; Örneğin, sürücü sayısı, tür ve boyut.
- Karşıya yüklenen olayların yüzdesi, bırakılan olaylar ve son karşıya yükleme zamanı dahil **telemetri işlevselliği**.
- Microsoft 'un Azure Stack nasıl çalıştığını öğrenmek için temel bir bilgi sahibi geliştirmenize yardımcı olan **kalite ile ilgili bilgiler** . Bir örnek, belirli bir donanım yapılandırmasındaki kritik uyarıların sayısıdır.
- **Uyumluluk verileri**, bir SISTEME ve VM 'ye hangi kaynak sağlayıcılarının yüklendiği hakkında bilgi sağlamaya ve olası uyumluluk sorunlarını tanımlamanıza yardımcı olur.

**2 (Gelişmiş)** : İşletim sisteminin ve diğer Azure Stack hizmetlerinin nasıl kullanıldığı, nasıl gerçekleştirdikleri, gelişmiş güvenilirlik verilerinin ve hem temel hem de güvenlik seviyelerinin verileri dahil ek Öngörüler.

**3 (tam)** : Sorunları belirlemek ve gidermek için gereken tüm veriler, ayrıca güvenlik, temel ve gelişmiş düzeylerdeki veriler.

> [!NOTE]
> Varsayılan telemetri düzeyi değeri 2 ' dir (Gelişmiş).

Windows ve Azure Stack telemetrisini kapatmak SQL telemetrisini devre dışı bırakır. Windows Server telemetri ayarlarının etkileri hakkında daha fazla bilgi için [Windows telemetri teknik incelemesine](https://aka.ms/winservtelemetry)başvurun.

> [!IMPORTANT]
> Bu telemetri düzeyleri yalnızca Microsoft Azure Stack bileşenleri için geçerlidir. Azure Stack Hardware iş ortaklarından donanım yaşam döngüsü ana bilgisayarında çalışan Microsoft dışı yazılım bileşenleri ve Hizmetleri, bu telemetri düzeyleri dışında bulut hizmetleriyle iletişim kurabilir. Telemetri ilkesini anlamak için Azure Stack donanım çözümü sağlayıcınızla birlikte çalışmanız ve bunları nasıl kabul edebilir veya geri çevirebilirsiniz.

### <a name="enable-or-disable-telemetry-after-deployment"></a>Dağıtımdan sonra Telemetriyi etkinleştir veya devre dışı bırak

Dağıtımdan sonra Telemetriyi etkinleştirmek veya devre dışı bırakmak için, ERCS VM 'lerde kullanıma sunulan ayrıcalıklı uç noktaya (PEP) erişiminizin olması gerekir.
1.  Şunları etkinleştirmek için:`Set-Telemetry -Enable`
2.  Devre dışı bırakmak için:`Set-Telemetry -Disable`

PARAMETRE ayrıntısı:
> . PARAMETRE etkinleştirme-telemetri verilerini karşıya yüklemeyi aç
> 
> . PARAMETRE devre dışı-telemetri verilerini karşıya yüklemeyi devre dışı bırakma  

**Telemetriyi etkinleştirmek için betik:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Enable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

**Telemetriyi devre dışı bırakma betiği:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Disable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

## <a name="next-steps"></a>Sonraki adımlar
[ASDK 'yi başlatma ve durdurma](asdk-start-stop.md)
