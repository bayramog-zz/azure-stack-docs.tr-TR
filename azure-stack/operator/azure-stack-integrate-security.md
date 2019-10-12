---
title: Azure Stack Syslog iletme kullanarak izleme çözümleriyle tümleştirme | Microsoft Docs
description: Azure Stack Syslog iletme kullanarak izleme çözümleriyle tümleştirme hakkında bilgi edinin.
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 04/23/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 04/23/2019
keywords: ''
ms.openlocfilehash: f28eda4a54ae95b1d5f3cc9c694344f8aec46f33
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277267"
---
# <a name="integrate-azure-stack-with-monitoring-solutions-using-syslog-forwarding"></a>Syslog iletme kullanarak Azure Stack izleme çözümleriyle tümleştirme

Bu makalede, veri merkezinizde zaten dağıtılmış olan dış güvenlik çözümlerinden Azure Stack altyapısını bütünleştirmek için Syslog 'ın nasıl kullanılacağı gösterilmektedir. Örneğin, bir güvenlik bilgileri olay yönetimi (SıEM) sistemi. Syslog kanalı, Azure Stack altyapısının tüm bileşenlerinden denetimleri, uyarıları ve güvenlik günlüklerini kullanıma sunar. Güvenlik izleme çözümleriyle tümleştirmek ve tüm denetimleri, uyarıları ve güvenlik günlüklerini bekletme amacıyla depolamak üzere almak için Syslog iletmeyi kullanın.

1809 güncelleştirmesiyle başlayarak, Azure Stack yapılandırıldıktan sonra, ortak olay biçiminde (CEF) yük ile Syslog iletileri sunan tümleşik bir Syslog istemcisine sahiptir.

Aşağıdaki diyagramda Azure Stack bir dış SıEM ile tümleştirme açıklanmaktadır. Göz önünde bulundurulması gereken iki Tümleştirme deseni vardır: ilki (mavi), altyapı sanal makinelerini ve Hyper-V düğümlerini kapsayan Azure Stack altyapısıdır. Bu bileşenlerin tüm denetimleri, güvenlik günlükleri ve uyarıları merkezi olarak toplanır ve CEF yükü ile Syslog aracılığıyla sunulur. Bu tümleştirme deseninin bu belge sayfasında açıklanmaktadır.
İkinci tümleştirme deseninin ikisi de turuncu olarak gösterilmiştir ve temel kart yönetim denetleyicileri (BMC 'ler), donanım yaşam döngüsü ana bilgisayarı (HLH), donanım ortağı izleme ve yönetim yazılımını çalıştıran sanal makineler ve sanal gereçlerini içerir. ve raf üstü (TOR) anahtarları. Bu bileşenler, donanım ortağına özel olduğundan, bunları bir dış SıEM ile tümleştirme hakkındaki belgeler için donanım iş ortağınızla iletişim kurun.

![Syslog iletme diyagramı](media/azure-stack-integrate-security/syslog-forwarding.png)

## <a name="configuring-syslog-forwarding"></a>Syslog iletmeyi yapılandırma

Azure Stack 'deki Syslog istemcisi aşağıdaki konfigürasyonları destekler:

1. **Karşılıklı kimlik doğrulaması (istemci ve sunucu) ve TLS 1,2 şifrelemesi Ile TCP üzerinden Syslog:** Bu yapılandırmada, Syslog sunucusu ve Syslog istemcisi sertifikaları aracılığıyla birbirlerinin kimliğini doğrulayabilirler. İletiler TLS 1,2 şifreli bir kanal üzerinden gönderilir.

2. **Sunucu kimlik doğrulaması ve TLS 1,2 şifrelemesi Ile TCP üzerinden Syslog:** Bu yapılandırmada, syslog istemcisi bir sertifika aracılığıyla Syslog sunucusunun kimliğini doğrulayabilirler. İletiler TLS 1,2 şifreli bir kanal üzerinden gönderilir.

3. **Şifreleme olmadan TCP üzerinden Syslog:** Bu yapılandırmada, syslog istemcisi ve Syslog Sunucu kimlikleri doğrulanmaz. İletiler TCP üzerinden şifresiz metin olarak gönderilir.

4. **Şifreleme olmadan UDP üzerinden Syslog:** Bu yapılandırmada, syslog istemcisi ve Syslog Sunucu kimlikleri doğrulanmaz. İletiler, UDP üzerinden şifresiz metin olarak gönderilir.

> [!IMPORTANT]
> Microsoft, tüm üretim ortamları için kimlik doğrulama ve şifreleme (yapılandırma #1 veya en düşük, #2) kullanarak TCP kullanarak, iletilerin baş adam saldırılarına ve gizlice dinlemesine karşı koruma sağlar.

### <a name="cmdlets-to-configure-syslog-forwarding"></a>Syslog iletmeyi yapılandırmak için cmdlet 'ler
Syslog iletme yapılandırması, ayrıcalıklı uç noktaya (PEP) erişim gerektirir. Syslog iletmeyi yapılandırmak için PEP 'ye iki PowerShell cmdlet 'i eklenmiştir:


```powershell
### cmdlet to pass the syslog server information to the client and to configure the transport protocol, the encryption and the authentication between the client and the server

Set-SyslogServer [-ServerName <String>] [-ServerPort <String>] [-NoEncryption] [-SkipCertificateCheck] [-SkipCNCheck] [-UseUDP] [-Remove]

### cmdlet to configure the certificate for the syslog client to authenticate with the server

Set-SyslogClient [-pfxBinary <Byte[]>] [-CertPassword <SecureString>] [-RemoveCertificate] [-OutputSeverity]
```
#### <a name="cmdlets-parameters"></a>Cmdlet parametreleri

*Set-SyslogServer* cmdlet parametreleri:

| Parametre | Açıklama | Tür | Gerekli |
|---------|---------|---------|---------|
|*ServerName* | Syslog sunucusunun FQDN 'SI veya IP adresi. | Dize | Yes|
|*Sunucu bağlantı noktası* | Syslog sunucusunun dinlediği bağlantı noktası numarası. | Dize | Yes|
|*NoEncryption*| İstemciye syslog iletilerini şifresiz metin olarak göndermesini zorunlu kılın. | bayrağıyla | eşleşen|
|*SkipCertificateCheck*| İlk TLS el sıkışması sırasında Syslog sunucusu tarafından belirtilen sertifikanın doğrulanmasını atlayın. | bayrağıyla | eşleşen|
|*SkipCNCheck*| İlk TLS el sıkışması sırasında Syslog sunucusu tarafından belirtilen sertifikanın ortak ad değerinin doğrulanmasını atlayın. | bayrağıyla | eşleşen|
|*UseUDP*| UDP as Aktarım Protokolü ile Syslog kullanın. |bayrağıyla | eşleşen|
|*Kaldır*| Sunucu yapılandırmasını istemciden kaldırın ve Syslog iletmeyi durdurun.| bayrağıyla | eşleşen|

*Set-SyslogClient* cmdlet parametreleri:

| Parametre | Açıklama | Tür |
|---------|---------| ---------|
| *pfxBinary* | istemci tarafından Syslog sunucusunda kimlik doğrulaması yapmak için kimlik olarak kullanılacak sertifikayı içeren pfx dosyası.  | Byte [] |
| *CertPassword* |  Pfx dosyasıyla ilişkili özel anahtarı içe aktarmak için parola. | SecureString |
|*Removecercertificate* | Sertifikayı istemciden kaldırın. | bayrağıyla|
| *Outputönem derecesi* | Çıktı günlüğü düzeyi. Değerler **varsayılan** veya **ayrıntılıdır**. Varsayılan değer, önem düzeylerini içerir: uyarı, kritik veya hata. Verbose tüm önem düzeylerini içerir: Verbose, bilgilendirici, uyarı, kritik veya hata.  | Dize |
### <a name="configuring-syslog-forwarding-with-tcp-mutual-authentication-and-tls-12-encryption"></a>TCP, karşılıklı kimlik doğrulaması ve TLS 1,2 şifrelemesi ile Syslog iletmeyi yapılandırma

Bu yapılandırmada, Azure Stack ' deki Syslog istemcisi, TLS 1,2 şifrelemesi ile iletileri TCP üzerinden Syslog sunucusuna iletir. İlk anlaşma sırasında istemci, sunucunun geçerli, güvenilen bir sertifika sağladığını doğrular. İstemci Ayrıca, kimlik kanıtı olarak sunucuya bir sertifika sağlar. Bu yapılandırma en güvenli olanıdır çünkü hem istemci hem de sunucu kimliğinin tam doğrulamasını sağlar ve iletileri şifreli bir kanal üzerinden gönderir.

> [!IMPORTANT]
> Microsoft bu yapılandırmayı üretim ortamları için kullanmayı kesinlikle önerir. 

TCP, karşılıklı kimlik doğrulaması ve TLS 1,2 şifrelemesi ile Syslog iletmeyi yapılandırmak için, her iki cmdlet 'i bir PEP oturumunda çalıştırın:

```powershell
# Configure the server
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>

# Provide certificate to the client to authenticate against the server
Set-SyslogClient -pfxBinary <Byte[] of pfx file> -CertPassword <SecureString, password for accessing the pfx file>
```

İstemci sertifikası, Azure Stack dağıtımı sırasında sağlanana kadar aynı köke sahip olmalıdır. Ayrıca özel bir anahtar içermelidir.

```powershell
##Example on how to set your syslog client with the certificate for mutual authentication.
##This example script must be run from your hardware lifecycle host or privileged access workstation.

$ErcsNodeName = "<yourPEP>"
$password = ConvertTo-SecureString -String "<your cloudAdmin account password" -AsPlainText -Force
 
$cloudAdmin = "<your cloudAdmin account name>"
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ($cloudAdmin, $password)
 
$certPassword = $password
$certContent = Get-Content -Path C:\cert\<yourClientCertificate>.pfx -Encoding Byte
 
$params = @{ 
    ComputerName = $ErcsNodeName 
    Credential = $CloudAdminCred 
    ConfigurationName = "PrivilegedEndpoint" 
}

$session = New-PSSession @params
 
$params = @{ 
    Session = $session 
    ArgumentList = @($certContent, $certPassword) 
}
Write-Verbose "Invoking cmdlet to set syslog client certificate..." -Verbose 
Invoke-Command @params -ScriptBlock { 
    param($CertContent, $CertPassword) 
    Set-SyslogClient -PfxBinary $CertContent -CertPassword $CertPassword }
```

### <a name="configuring-syslog-forwarding-with-tcp-server-authentication-and-tls-12-encryption"></a>TCP, sunucu kimlik doğrulaması ve TLS 1,2 şifrelemesi ile Syslog iletmeyi yapılandırma

Bu yapılandırmada, Azure Stack ' deki Syslog istemcisi, TLS 1,2 şifrelemesi ile iletileri TCP üzerinden Syslog sunucusuna iletir. İlk anlaşma sırasında istemci ayrıca sunucunun geçerli, güvenilen bir sertifika sağladığını doğrular. Bu yapılandırma, istemcinin güvenilmeyen hedeflere ileti göndermesini engeller.
Kimlik doğrulama ve şifreleme kullanan TCP, varsayılan yapılandırmadır ve Microsoft 'un bir üretim ortamı için önerdiği en düşük güvenlik düzeyini temsil eder.

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
```

Otomatik olarak imzalanan veya güvenilmeyen bir sertifika kullanarak Syslog sunucunuzun Azure Stack istemcisiyle tümleştirilmesini test etmek isterseniz, ilk anlaşma sırasında istemci tarafından gerçekleştirilen sunucu doğrulamasını atlamak için bu bayrakları kullanabilirsiniz.

```powershell
 #Skip validation of the Common Name value in the server certificate. Use this flag if you provide an IP address for your syslog server
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCNCheck

 #Skip entirely the server certificate validation
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCertificateCheck
```

> [!IMPORTANT]
> Microsoft, üretim ortamları için-SkipCertificateCheck bayrağının kullanılmasını önerir. 

### <a name="configuring-syslog-forwarding-with-tcp-and-no-encryption"></a>TCP ile Syslog iletmeyi yapılandırma ve şifreleme yok

Bu yapılandırmada, Azure Stack içindeki Syslog istemcisi iletileri şifreleme olmadan TCP üzerinden Syslog sunucusuna iletir. İstemci, sunucunun kimliğini doğrulamaz veya doğrulama için sunucuya kendi kimliğini sağlar.

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -NoEncryption
```

> [!IMPORTANT]
> Microsoft bu yapılandırmayı üretim ortamları için kullanmayı önerir.


### <a name="configuring-syslog-forwarding-with-udp-and-no-encryption"></a>UDP ile Syslog iletmeyi yapılandırma ve şifreleme yok

Bu yapılandırmada Azure Stack Syslog istemcisi iletileri şifreleme olmadan UDP üzerinden Syslog sunucusuna iletir. İstemci, sunucunun kimliğini doğrulamaz veya doğrulama için sunucuya kendi kimliğini sağlar.

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -UseUDP
```

Şifreleme gerektirmeyen UDP, yapılandırmanın en kolay olmadığı sürece, bu, ortadaki adam saldırılarına ve iletileri gizlice dinlemesine karşı koruma sağlamaz.

> [!IMPORTANT]
> Microsoft bu yapılandırmayı üretim ortamları için kullanmayı önerir.


## <a name="removing-syslog-forwarding-configuration"></a>Syslog iletme yapılandırması kaldırılıyor

Syslog sunucusu yapılandırmasını tamamen kaldırmak ve Syslog iletmeyi durdurmak için:

**Syslog sunucusu yapılandırmasını istemciden kaldırma**

```powershell  
Set-SyslogServer -Remove
```

**İstemci sertifikasını istemciden kaldır**

```powershell  
Set-SyslogClient -RemoveCertificate
```

## <a name="verifying-the-syslog-setup"></a>Syslog kurulumu doğrulanıyor

Syslog istemcisini Syslog sunucunuza başarıyla bağladıysanız, en kısa zamanda olay almaya başlamanız gerekir. Herhangi bir olay görmüyorsanız, aşağıdaki cmdlet 'leri çalıştırarak Syslog istemcinizin yapılandırmasını doğrulayın:

**Syslog istemcisinde Sunucu yapılandırmasını doğrulama**

```powershell  
Get-SyslogServer
```

**Syslog istemcisinde sertifika kurulumunu doğrulama**

```powershell  
Get-SyslogClient
```

## <a name="syslog-message-schema"></a>Syslog ileti şeması

Azure Stack altyapısının Syslog iletimi, ortak olay biçiminde (CEF) biçimlendirilen iletileri gönderir.
Her Syslog iletisi bu şemaya göre yapılandırılır:

```Syslog
<Time> <Host> <CEF payload>
```

CEF yükü aşağıdaki yapıya dayanır, ancak her bir alana yönelik eşleme, ileti türüne (Windows olayı, uyarı oluşturuldu, uyarı kapandı) göre değişir.

```CEF
# Common Event Format schema
CEF: <Version>|<Device Vendor>|<Device Product>|<Device Version>|<Signature ID>|<Name>|<Severity>|<Extensions>
* Version: 0.0
* Device Vendor: Microsoft
* Device Product: Microsoft Azure Stack
* Device Version: 1.0
```

### <a name="cef-mapping-for-privileged-endpoint-events"></a>Ayrıcalıklı uç nokta olayları için CEF eşleme

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <PEP Event ID>
* Name: <PEP Task Name>
* Severity: mapped from PEP Level (details see the PEP Severity table below)
```

Ayrıcalıklı uç nokta için olay tablosu:

| Olay | PEP olay KIMLIĞI | PEP görev adı | Önem Derecesi |
|-------|--------------| --------------|----------|
|Ayrıcalıklı Gedendpointaccessed|1000|PrivilegedEndpointAccessedEvent|5|
|Supportsessiontokenistendi |1001|SupportSessionTokenRequestedEvent|5|
|SupportSessionDevelopmentTokenRequested |1002|SupportSessionDevelopmentTokenRequestedEvent|5|
|Supportsessionkilitlenmemiş |1003|SupportSessionUnlockedEvent|10|
|SupportSessionFailedToUnlock |1004|SupportSessionFailedToUnlockEvent|10|
|PrivilegedEndpointClosed |1005|PrivilegedEndpointClosedEvent|5|
|NewCloudAdminUser |1006|NewCloudAdminUserEvent|10|
|RemoveCloudAdminUser |1007|RemoveCloudAdminUserEvent|10|
|SetCloudAdminUserPassword |1008|SetCloudAdminUserPasswordEvent|5|
|GetCloudAdminPasswordRecoveryToken |1009|GetCloudAdminPasswordRecoveryTokenEvent|10|
|ResetCloudAdminPassword |1010|ResetCloudAdminPasswordEvent|10|

PEP önem derecesi tablosu:

| Önem Derecesi | Düzey | Sayısal değer |
|----------|-------| ----------------|
|0|Tanımlayan|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik uyarı için günlükleri belirtir|
|8|Hata| Değer: 2. Bir hata için günlükleri gösterir|
|5|Uyarı|Değer: 3. Uyarı için günlükleri belirtir|
|2|Bilgiler|Değer: 4. Bilgilendirici bir ileti için günlükleri belirtir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

### <a name="cef-mapping-for-recovery-endpoint-events"></a>Kurtarma uç noktası olayları için CEF eşleme

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <REP Event ID>
* Name: <REP Task Name>
* Severity: mapped from REP Level (details see the REP Severity table below)
```

Kurtarma uç noktası için olay tablosu:

| Olay | REP olay KIMLIĞI | REP görev adı | Önem Derecesi |
|-------|--------------| --------------|----------|
|RecoveryEndpointAccessed |1011|RecoveryEndpointAccessedEvent|5|
|Recoverysessiontokenistendi |1012|RecoverySessionTokenRequestedEvent |5|
|Recoverysessiondevelopmenttokenistendi |1013|RecoverySessionDevelopmentTokenRequestedEvent|5|
|Recoverysessionkilitlenmemiş |1014|RecoverySessionUnlockedEvent |10|
|RecoverySessionFailedToUnlock |1015|RecoverySessionFailedToUnlockEvent|10|
|RecoveryEndpointClosed |1016|RecoveryEndpointClosedEvent|5|

REP önem derecesi tablosu:

| Önem Derecesi | Düzey | Sayısal değer |
|----------|-------| ----------------|
|0|Tanımlayan|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik uyarı için günlükleri belirtir|
|8|Hata| Değer: 2. Bir hata için günlükleri gösterir|
|5|Uyarı|Değer: 3. Uyarı için günlükleri belirtir|
|2|Bilgiler|Değer: 4. Bilgilendirici bir ileti için günlükleri belirtir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

### <a name="cef-mapping-for-windows-events"></a>Windows olayları için CEF eşleme

```
* Signature ID: ProviderName:EventID
* Name: TaskName
* Severity: Level (for details, see the severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Windows olayları için önem tablosu:

| CEF önem derecesi değeri | Windows olay düzeyi | Sayısal değer |
|--------------------|---------------------| ----------------|
|0|Tanımlayan|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik uyarı için günlükleri belirtir|
|8|Hata| Değer: 2. Bir hata için günlükleri gösterir|
|5|Uyarı|Değer: 3. Uyarı için günlükleri belirtir|
|2|Bilgiler|Değer: 4. Bilgilendirici bir ileti için günlükleri belirtir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

Azure Stack Windows olayları için özel uzantı tablosu:

| Özel uzantı adı | Windows olay örneği | 
|-----------------------|---------|
|MasChannel | Sistem|
|Masbilgisayar | test.azurestack.contoso.com|
|MasCorrelationActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasCorrelationRelatedActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasEventData| Svchost!! 4132, G, 0!!!! EseDiskFlushConsistency!! ESENT!! 0x800000|
|MasEventDescription| Kullanıcının grup ilkesi ayarları başarıyla işlendi. Son başarılı grup ilkesi işlemeden bu yana hiçbir değişiklik algılanmadı.|
|Mayettıd|1501|
|Maseventrecordıd|26637|
|Masexecutionprocessıd | 29380|
|Masexecutionthreadıd |25480|
|MasKeywords |0x8000000000000000|
|MasKeywordName |Denetim başarılı|
|MasLevel |4|
|MasOpcode |1\.|
|Masopkod adı |Bilgisine|
|MasProviderEventSourceName ||
|MasProviderGuid |AEA1B4FA-97D1-45F2-A64C-4D69FFFD92C9|
|MasProviderName |Microsoft-Windows-GroupPolicy|
|Massecurityuserıd |\<Windows SID @ no__t-1 |
|MasTask |0|
|MasTaskCategory| İşlem oluşturma|
|MasUserData|KB4093112!! 5112!! Yüklendi!! 0x0!! WindowsUpdateAgent XPath:/Event/UserData/*|
|MasVersion|0|

### <a name="cef-mapping-for-alerts-created"></a>Oluşturulan uyarılar için CEF eşlemesi

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Alert Severity (for details, see alerts severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Uyarılar önem derecesi tablosu:

| Önem Derecesi | Düzey |
|----------|-------|
|0|Tanımlayan|
|10|Kritik|
|5|Uyarı|

Azure Stack oluşturulan uyarılar için özel uzantı tablosu:

| Özel uzantı adı | Örnek | 
|-----------------------|---------|
|MasEventDescription|Açıklama: \<TestDomain @ no__t-3 için \<TestUser @ no__t-1 Kullanıcı hesabı oluşturuldu. Bu, olası bir güvenlik riskidir. --Düzeltme: desteğe başvurun. Bu sorunu çözmek için müşteri yardımı gereklidir. Bu sorunu, yardımı olmadan çözmeyi denemeyin. Bir destek isteği açmadan önce, https://aka.ms/azurestacklogfiles ' dan gelen kılavuzu kullanarak günlük dosyası toplama işlemini başlatın.

### <a name="cef-mapping-for-alerts-closed"></a>Uyarılar için CEF eşlemesi kapatıldı

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Information
```

Aşağıdaki örnekte, CEF yükünün bulunduğu bir Syslog iletisi gösterilmektedir:
```
2018:05:17:-23:59:28 -07:00 TestHost CEF:0.0|Microsoft|Microsoft Azure Stack|1.0|3|TITLE: User Account Created -- DESCRIPTION: A user account \<TestUser\> was created for \<TestDomain\>. It's a potential security risk. -- REMEDIATION: Please contact Support. Customer Assistance is required to resolve this issue. Do not try to resolve this issue without their assistance. Before you open a support request, start the log file collection process using the guidance from https://aka.ms/azurestacklogfiles|10
```

## <a name="next-steps"></a>Sonraki adımlar

[Bakım ilkesi](azure-stack-servicing-policy.md)