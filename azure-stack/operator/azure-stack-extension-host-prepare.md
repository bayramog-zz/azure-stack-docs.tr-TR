---
title: Azure Stack 'de uzantı konağı için hazırlanma | Microsoft Docs
description: Sürüm 1808 ' den sonra otomatik olarak bir Azure Stack güncelleştirme paketiyle etkinleştirilen Azure Stack uzantı konağına nasıl hazırlanacağınızı öğrenin.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 10/02/2019
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 75070550f1863457c3a2aaf9ab5915536372d55b
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019263"
---
# <a name="prepare-for-extension-host-in-azure-stack"></a>Azure Stack uzantı konağı için hazırlanma

Uzantı ana bilgisayarı, gerekli TCP/IP bağlantı noktalarının sayısını azaltarak Azure Stack güvenliğini sağlar. Bu makale, 1808 güncelleştirmesinden sonra otomatik olarak bir Azure Stack güncelleştirme paketi aracılığıyla etkinleştirilen uzantı ana bilgisayarı için Azure Stack hazırlamaya bakar. Bu makale 1808, 1809 ve 1811 Azure Stack güncelleştirmeleri için geçerlidir.

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Uzantı ana bilgisayarı, her Portal uzantısı için benzersiz konak girdilerinin garanti etmek üzere iki yeni etki alanı ad alanı uygular. Yeni etki alanı ad alanları, güvenli iletişim sağlamak için iki ek joker sertifika gerektirir.

Tablo, yeni ad alanlarını ve ilişkili sertifikaları gösterir:

| Dağıtım klasörü | Gerekli sertifika konusu ve konu diğer adları (SAN) | Kapsam (bölge başına) | Alt etki alanı adı |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Yönetici uzantısı ana bilgisayarı | *. adminhosting. \<region >. \<fqdn > (joker karakter SSL sertifikaları) | Yönetici uzantısı ana bilgisayarı | adminhosting. \<region >. \<fqdn > |
| Ortak uzantı Konağı | *. Hosting. \<region >. \<fqdn > (joker karakter SSL sertifikaları) | Ortak uzantı Konağı | barındırma. \<region >. \<fqdn > |

Ayrıntılı sertifika gereksinimleri için bkz. [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri](azure-stack-pki-certs.md).

## <a name="create-certificate-signing-request"></a>Sertifika imzalama isteği oluştur

Azure Stack hazırlık Denetleyicisi Aracı, iki yeni ve gerekli SSL sertifikası için bir sertifika imzalama isteği oluşturmanızı sağlar. [Sertifika imzalama isteği oluşturma Azure Stack](azure-stack-get-pki-certs.md)makalesindeki adımları izleyin.

> [!Note]  
> SSL sertifikalarınızı nasıl istemiş olduğunuza bağlı olarak bu adımı atlayabilirsiniz.

## <a name="validate-new-certificates"></a>Yeni sertifikaları doğrulama

1. PowerShell 'i, donanım yaşam döngüsü konağında veya Azure Stack yönetim iş istasyonunda yükseltilmiş izinle açın.
2. Azure Stack hazırlık Denetleyicisi aracını yüklemek için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. Gerekli klasör yapısını oluşturmak için aşağıdaki betiği çalıştırın:

    ```powershell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Azure Active Directory Federasyon Hizmetleri (AD FS) ile dağıtırsanız, şu dizinlerin betiğe **$Directories** eklenmesi gerekir: `ADFS`, `Graph`.

4. Azure Stack, kullanmakta olduğunuz mevcut sertifikaları uygun dizinlerde yerleştirin. Örneğin, **yönetıcı ARM** sertifikasını `Arm Admin` klasörüne yerleştirin. Ve sonra yeni oluşturulan barındırma sertifikalarını `Admin extension host` ve `Public extension host` dizinlerine yerleştirin.
5. Sertifika denetimini başlatmak için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

6. Çıktıyı denetleyin ve tüm sertifikalar tüm testleri geçer.


## <a name="import-extension-host-certificates"></a>Uzantı konak sertifikalarını içeri aktar

Sonraki adımlar için Azure Stack ayrıcalıklı uç noktaya bağlanabilecek bir bilgisayar kullanın. Bu bilgisayardan yeni sertifika dosyalarına erişiminizin olduğundan emin olun.

1. Sonraki adımlar için Azure Stack ayrıcalıklı uç noktaya bağlanabilecek bir bilgisayar kullanın. Bu bilgisayardan yeni sertifika dosyalarına erişiminizin olduğundan emin olun.
2. Sonraki betik bloklarını yürütmek için PowerShell ıSE 'yi açın.
3. Yönetici barındırma uç noktası için sertifikayı içeri aktarın.

    ```powershell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
4. Barındırma uç noktası için sertifikayı içeri aktarın.
    ```powershell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```

### <a name="update-dns-configuration"></a>DNS yapılandırmasını Güncelleştir

> [!Note]  
> DNS tümleştirmesi için DNS bölge temsilcisini kullandıysanız, bu adım gerekli değildir.
Bireysel ana bilgisayar bir kayıt Azure Stack bitiş noktaları yayımlamak üzere yapılandırılmışsa, iki ek ana bilgisayar A kaydı oluşturmanız gerekir:

| IP | Konak Adı | Type |
|----|------------------------------|------|
| \<IP > | *. Adminhosting. \<Region >. \<FQDN > | A |
| \<IP > | *. Barındırma. \<Region >. \<FQDN > | A |

Ayrılan IP 'Ler, **Get-Azurestackstampınformation**cmdlet 'ini çalıştırarak ayrıcalıklı uç nokta kullanılarak alınabilir.

### <a name="ports-and-protocols"></a>Bağlantı noktaları ve protokoller

[Veri merkezi tümleştirme-yayımlama uç noktaları Azure Stack](azure-stack-integrate-endpoints.md) makale, uzantı ana bilgisayar dağıtımı öncesinde Azure Stack yayımlamak üzere gelen iletişim gerektiren bağlantı noktalarını ve protokolleri ele alır.

### <a name="publish-new-endpoints"></a>Yeni uç noktaları Yayımla

Güvenlik duvarınız aracılığıyla yayımlanması gereken iki yeni uç nokta vardır. Ortak VIP havuzundan ayrılan IP 'Ler, Azure Stack [ortamınızın ayrıcalıklı uç noktasından](azure-stack-privileged-endpoint.md)çalıştırılması gereken aşağıdaki kod kullanılarak alınabilir.

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Obtain DNS Servers and extension host information from Azure Stack Stamp Information and find the IPs for the Host Extension Endpoints
$StampInformation = Invoke-Command $PEPSession {Get-AzureStackStampInformation} | Select-Object -Property ExternalDNSIPAddress01, ExternalDNSIPAddress02, @{n="TenantHosting";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://*.","testdnsentry"-replace "/"}},  @{n="AdminHosting";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://*.","testdnsentry"-replace "/"}},@{n="TenantHostingDNS";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://",""-replace "/"}},  @{n="AdminHostingDNS";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://",""-replace "/"}}
If (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting -ErrorAction SilentlyContinue) {
    Write-Host "Can access AZS DNS" -ForegroundColor Green
    $AdminIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress02 -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Else {
    Write-Host "Cannot access AZS DNS" -ForegroundColor Yellow
    $AdminIP = (Resolve-DnsName -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Remove-PSSession -Session $PEPSession
```

#### <a name="sample-output"></a>Örnek çıkış

```powershell
Can access AZS DNS
The IP for the Admin Extension Host is: *.adminhosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.adminhosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
The IP address for the Tenant Extension Host is *.hosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.hosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
```

> [!Note]  
> Uzantı konağını etkinleştirmeden önce bu değişikliği yapın. Bu, Azure Stack portallarının sürekli olarak erişilebilir olmasını sağlar.

| Uç nokta (VIP) | Protocol | Bağlantı Noktaları |
|----------------|----------|-------|
| Yönetici barındırma | HTTPS | 443 |
| Barındırma | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Var olan yayımlama kurallarını güncelleştir (uzantı ana bilgisayarını etkinleştirme)

> [!Note]  
> 1808 Azure Stack güncelleştirme paketi henüz uzantı **konağını etkinleştirmez.** Gerekli sertifikaları içeri aktararak uzantı konağına hazırlanmanıza imkan tanır. Uzantı ana bilgisayarı, 1808 güncelleştirmesinden sonra bir Azure Stack güncelleştirme paketiyle otomatik olarak etkinleştirilmeden önce herhangi bir bağlantı noktasını kapatmayın.

Mevcut güvenlik duvarı kurallarınız için aşağıdaki mevcut uç nokta bağlantı noktalarının kapalı olması gerekir.

> [!Note]  
> Doğrulama başarılı olduktan sonra bu bağlantı noktalarını kapatmanız önerilir.

| Uç nokta (VIP) | Protocol | Bağlantı Noktaları |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (yönetici) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015 |
| Portal (Kullanıcı) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (yönetici) | HTTPS | 30024 |
| Azure Resource Manager (Kullanıcı) | HTTPS | 30024 |

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)hakkında bilgi edinin.
- [Azure Stack sertifikaları imzalayan istek oluşturma](azure-stack-get-pki-certs.md)hakkında bilgi edinin.
