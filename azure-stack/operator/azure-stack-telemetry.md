---
title: Azure Stack telemetri | Microsoft Docs
description: PowerShell kullanarak Azure Stack telemetri ayarlarının nasıl yapılandırılacağını açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: comartin
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 439a058ea4153e0b6f74932a0ad027a0f5f8a42b
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829246"
---
# <a name="azure-stack-telemetry"></a>Azure Stack telemetri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack telemetri, bağlı kullanıcı deneyimi aracılığıyla sistem verilerini Microsoft 'a otomatik olarak yükler. Microsoft ekipleri, müşteri deneyimlerini geliştirmek için telemetri topladığı Azure Stack verileri kullanır. Bu veriler güvenlik, sistem durumu, kalite ve performans analizi için de kullanılır.

Azure Stack bir operatör için telemetri kurumsal dağıtımlara değerli içgörüler sunabilir ve size Azure Stack gelecek sürümlerini şekillendirmeye yardımcı olan bir ses sağlar.

> [!NOTE]
> Ayrıca, kullanım bilgilerini faturalandırma için Azure 'a iletmek üzere Azure Stack de yapılandırabilirsiniz. Bu, Kullandıkça Öde faturalandırması 'nı seçen çok düğümlü Azure Stack müşteriler için gereklidir. Kullanım raporlaması telemetriden bağımsız olarak denetlenir ve kapasite modelini veya Azure Stack Geliştirme Seti kullanıcıları seçen çok düğümlü müşteriler için gerekli değildir. Bu senaryolar için, kullanım raporlaması [kayıt betiği kullanılarak](azure-stack-usage-reporting.md)kapatılabilir.

Azure Stack telemetri, olay ve veri toplamak ve depolamak üzere [Windows Için olay izleme (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) TraceLogging teknolojisini kullanan windows Server 2016 bağlantılı Kullanıcı deneyimi ve telemetri bileşenini temel alır. Azure Stack bileşenleri, genel işletim sistemi olay günlüğü ve izleme API 'Leri kullanılarak toplanan olayları ve verileri yayımlamak için aynı teknolojiyi kullanır. Bu Azure Stack bileşenlerinin örnekleri şunlardır: Ağ kaynağı, depolama kaynağı, Izleme kaynağı ve kaynak güncelleştirme. Bağlı kullanıcı deneyimi ve telemetri bileşeni, SSL kullanarak verileri şifreler ve HTTPS üzerinden Microsoft Veri Yönetimi hizmetine veri aktarmak için sertifika sabitleme kullanır.

> [!IMPORTANT]
> Telemetri veri akışını etkinleştirmek için, ağınızda bağlantı noktası 443 (HTTPS) açık olmalıdır. Bağlı kullanıcı deneyimi ve telemetri bileşeni, https://v10.vortex-win.data.microsoft.com ' da Microsoft Veri Yönetimi hizmetine bağlanır. Bağlı kullanıcı deneyimi ve telemetri bileşeni Ayrıca yapılandırma bilgilerini indirmek için https://settings-win.data.microsoft.com ' a bağlanır.

## <a name="privacy-considerations"></a>Gizlilik konuları

ETW hizmeti, telemetri verilerini korumalı bulut depolamaya geri yönlendirir. En az ayrıcalık, telemetri verilerine erişimi kılavuzluk eder. Yalnızca geçerli bir iş gerektiren Microsoft personeline telemetri verilerine erişim verilir. Microsoft, müşterinin veya [Microsoft gizlilik bildiriminde](https://privacy.microsoft.com/PrivacyStatement)açıklanan sınırlı amaçlar dışında kişisel müşteri verilerini üçüncü taraflarla paylaşmaz. OEM 'Ler ve iş ortakları ile paylaşılan iş raporları, toplanmış, anonimleştirilmiş verileri içerir. Veri paylaşımı kararları, gizlilik, yasal ve veri yönetimi katılımcıları dahil olmak üzere dahili bir Microsoft ekibi tarafından yapılır.

Microsoft, ve uygulama bilgilerinin en az düzeyde olduğunu düşündüğü. Yalnızca gereken bilgileri toplamak ve yalnızca bir hizmet sağlamak ya da analiz için gereken sürece depolamak için çaba duyuyoruz. Azure Stack sistemi ve Azure hizmetlerinin nasıl çalıştığı hakkında bilgilerin çoğu altı ay içinde silinir. Özetlenen veya toplanmış veriler, daha uzun bir süre için saklanacaktır.

Müşteri bilgilerinin gizliliği ve güvenliğinin önemli olduğunu anladık.  Microsoft, müşteri gizliliği ve Azure Stack içindeki müşteri verilerinin korunması için bir düşünceli ve kapsamlı bir yaklaşım alır. BT yöneticileri, özellikleri ve gizlilik ayarlarını dilediğiniz zaman özelleştirmek için denetimlere sahiptir. Saydamlıkla ve güvenimize yönelik taahhüdizin açık:

- Topladığımız veri türleri hakkında müşterilerle birlikte çalışıyoruz.
- Kurumsal müşterileri denetime yerleştiririz, kendi gizlilik ayarlarını özelleştirebilir.
- Önce Müşteri Gizlilik ve güvenlik yerleştiririz.
- Telemetri verilerinin nasıl kullanıldığı hakkında saydamız.
- Müşteri deneyimlerini geliştirmek için telemetri verilerini kullanıyoruz.

Microsoft, kredi kartı numaraları, Kullanıcı adları ve parolalar, e-posta adresleri veya benzer gizli bilgiler gibi hassas veriler toplamaya yönelik değildir. Gizli bilgilerin yanlışlıkla alındığını belirlediğimiz takdirde, bu bilgileri sileriz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft 'un telemetri verilerini nasıl kullandığı hakkında örnekler

Telemetri, müşteri dağıtımlarında ve yapılandırmalarında kritik güvenilirlik sorunlarını hızla tanımlayıp gidermeye yardımcı olan önemli bir rol oynar. Telemetri verilerinden Öngörüler, hizmetlerle veya donanım yapılandırmalarında sorunları belirlemenize yardımcı olabilir. Microsoft 'un bu verileri müşterilerinden alma ve ekosisteme yönelik iyileştirmeler geliştirme özelliği, tümleşik Azure Stack çözümlerinin kalitesi için bir çubuk yükseltir.

Telemetri ayrıca Microsoft 'un müşterilerin bileşenleri nasıl dağıtdığını, özellikleri nasıl kullandığını ve iş hedeflerine ulaşmak için Hizmetleri nasıl kullandığını daha iyi anlamasına yardımcı olur. Bu Öngörüler, müşteri deneyimlerini ve iş yüklerini doğrudan etkileyebilecek alanlardaki mühendislik yatırımlarının önceliklerini belirlemeye yardımcı olur.

Bazı örneklerde, Azure Stack rolleriyle ilişkili kapsayıcılar, depolama ve ağ yapılandırmalarının müşteri kullanımı sayılabilir. Ayrıca, geliştirmeleri ve zeka Azure Stack yönetimi ve izleme çözümlerini sağlamak için öngörüleri kullanırız. Bu geliştirmeler, müşterilerin sorunları tanılamasını ve Microsoft 'a daha az destek çağrısı yaparak para tasarrufu yapmalarını kolaylaştırır.

## <a name="manage-telemetry-collection"></a>Telemetri toplamayı Yönet

Kuruluşunuzda telemetri kapatmayı önermedik. Ancak bazı senaryolarda bu gerekli olabilir.

Bu senaryolarda, Azure Stack dağıtmadan önce kayıt defteri ayarlarını kullanarak veya Azure Stack dağıttıktan sonra telemetri uç noktalarını kullanarak Microsoft 'a gönderilen telemetri düzeyini yapılandırabilirsiniz.

### <a name="telemetry-levels-and-data-collection"></a>Telemetri düzeyleri ve veri toplama

Telemetri ayarlarını değiştirmeden önce telemetri düzeylerini ve her düzeyde hangi verilerin toplandığını anlamanız gerekir.

Telemetri ayarları, aşağıdaki şekilde birikimli ve kategorilere ayrılmış dört düzeyde (0-3) gruplandırılır:

**0 (güvenlik)**</br>
Yalnızca güvenlik verileri. İşletim sistemini güvenli tutmak için gereken bilgiler. Bu, bağlı kullanıcı deneyimi ve telemetri bileşen ayarları ve Windows Defender hakkındaki verileri içerir. Azure Stack özgü telemetri bu düzeyde yayılır.

**1 (temel)**</br>
Güvenlik verileri ve temel sistem durumu ve kalite verileri. Kalite ile ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verileri ve **güvenlik** düzeyinden alınan veriler dahil olmak üzere temel cihaz bilgileri. Telemetri düzeyinizi temel olarak ayarlamak Azure Stack telemetri sağlar. Bu düzeyde toplanan veriler şunları içerir:

- Ekosistemde yerel ve sanal Windows Server 2016 örneklerinin türleri ve konfigürasyonları hakkında bilgi sahibi olan *temel cihaz bilgileri* . Buna aşağıdakiler dahildir:

  - OEM ve model gibi makine öznitelikleri.
  - Ağ bağdaştırıcılarının sayısı ve hızı gibi ağ öznitelikleri.
  - İşlemci ve bellek öznitelikleri (çekirdek sayısı ve yüklü bellek miktarı).
  - Sürücü sayısı, sürücü türü ve sürücü boyutu gibi depolama öznitelikleri.

- Karşıya yüklenen olayların yüzdesi, bırakılan olaylar ve son veri yükleme zamanı dahil *telemetri işlevselliği*.
- Microsoft 'un Azure Stack nasıl gerçekleştirileceği hakkında temel bir bilgi sahibi geliştirmenize yardımcı olan, *kaliteyle ilgili bilgiler* . Örneğin, belirli bir donanım yapılandırmasındaki kritik uyarı sayısı.
- Bir sistemde ve bir sanal makinede hangi kaynak sağlayıcılarının yüklü olduğunu anlamak için yardım veren *Uyumluluk verileri* . Bu, olası uyumluluk sorunlarını belirler.

**2 (Gelişmiş)**</br>
İşletim sistemi ve Azure Stack hizmetlerinin nasıl kullanıldığı, bu hizmetlerin nasıl gerçekleştirdiği, gelişmiş güvenilirlik verilerinin ve **güvenlik** ve **temel** düzeylerdeki verilerin nasıl gerçekleştirileceği hakkında ek Öngörüler.

> [!NOTE]
> Bu, varsayılan telemetri ayarıdır.

**3 (tam)**</br>
Sorunları belirlemek ve gidermek için gereken tüm veriler, ayrıca **güvenlik**, **temel**ve **Gelişmiş** düzeylerdeki veriler.

> [!IMPORTANT]
> Bu telemetri düzeyleri yalnızca Microsoft Azure Stack bileşenleri için geçerlidir. Azure Stack Hardware iş ortaklarından donanım yaşam döngüsü ana bilgisayarında çalışan Microsoft dışı yazılım bileşenleri ve Hizmetleri, bu telemetri düzeyleri dışında bulut hizmetleriyle iletişim kurabilir. Telemetri ilkesini anlamak için Azure Stack donanım çözümü sağlayıcınızla birlikte çalışmanız ve bunları nasıl kabul edebilir veya geri çevirebilirsiniz.

Windows ve Azure Stack telemetrinin kapatılması SQL telemetrisini de devre dışı bırakır. Windows Server telemetri ayarlarının etkileri hakkında daha fazla bilgi için bkz. [Windows telemetri teknik incelemesi](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: Windows kayıt defterinde telemetri düzeyini ayarlama

Azure Stack dağıtmadan önce, fiziksel ana bilgisayar bilgisayarındaki telemetri düzeyini el ile ayarlamak için Windows kayıt defteri düzenleyicisini kullanabilirsiniz. Grup ilkesi gibi bir yönetim ilkesi zaten varsa, bu kayıt defteri ayarını geçersiz kılar.

Geliştirme seti ana bilgisayarında Azure Stack dağıtılmadan önce CloudBuilder. vhdx ' i önyükleyin ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:

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

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK ve çok düğümlü: dağıtımdan sonra Telemetriyi etkinleştirme veya devre dışı bırakma

Dağıtımdan sonra Telemetriyi etkinleştirmek veya devre dışı bırakmak için, ERCS VM 'lerde kullanıma sunulan ayrıcalıklı uç noktaya (PEP) erişiminizin olması gerekir.

1. Etkinleştirmek için: `Set-Telemetry -Enable`
2. Devre dışı bırakmak için: `Set-Telemetry -Disable`

PARAMETRE ayrıntıları:
> . PARAMETRE etkinleştirme-telemetri verilerini karşıya yüklemeyi aç</br>
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

[Azure Stack'i Azure'a kaydetme](azure-stack-registration.md)