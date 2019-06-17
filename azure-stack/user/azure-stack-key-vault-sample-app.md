---
title: Azure Stack Key Vault gizli dizileri erişmek uygulamalara izin ver | Microsoft Docs
description: Azure Stack anahtar kasasındaki anahtarları ve gizli anahtarları alır ve örnek bir uygulama çalıştırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sethm
ms.lastreviewed: 04/08/2019
ms.openlocfilehash: 6cc9475ee04bcdcb7b4c35f4ca5a39efc7c36aa8
ms.sourcegitcommit: ad2f2cb4dc8d5cf0c2c37517d5125921cff44cdd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67138888"
---
# <a name="allow-apps-to-access-azure-stack-key-vault-secrets"></a>Azure Stack Key Vault gizli dizileri erişmek uygulamalara izin ver

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Örnek uygulamayı çalıştırmak için bu makaledeki adımları **HelloKeyVault** , anahtarları alır ve gizli diziler bir anahtar kasası Azure Stack'te.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulları yükleyebileceğiniz [Azure Stack geliştirme Seti'ni](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp), veya size bir istemciden Windows tabanlı dış [VPN üzerinden bağlı](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn):

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).

## <a name="create-a-key-vault-and-register-an-app"></a>Key vault oluşturma ve bir uygulamayı kaydetme

Örnek uygulama için hazırlamak için:

* Azure Stack'te bir anahtar kasası oluşturma.
* Azure'da bir uygulamayı kaydetme Active Directory (Azure AD).

Örnek uygulama için hazırlamak üzere Azure portal veya PowerShell kullanın.

> [!NOTE]
> Varsayılan olarak, PowerShell Betiği Active Directory'de yeni bir uygulama oluşturur. Bununla birlikte, mevcut uygulamalarınızı birini kaydedebilirsiniz.

Aşağıdaki komut dosyasını çalıştırmadan önce değerleri için sağladığınız emin olun `aadTenantName` ve `applicationPassword` değişkenleri. İçin bir değer belirtmezseniz `applicationPassword`, bu betik, rastgele bir parola oluşturur.

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script generates a random password during app creation.
$applicationPassword = ''

# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator's PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Add-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `

$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey

# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "https://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault in that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host
```

Aşağıdaki görüntüde, anahtar kasası oluşturmak için kullanılan komut dosyası çıktısı gösterilmektedir:

![Anahtar kasası ile erişim tuşları](media/azure-stack-key-vault-sample-app/settingsoutput.png)

Not **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** önceki komut dosyası tarafından döndürülen değer. Bu değerleri çalıştırmak için kullandığınız **HelloKeyVault** uygulama.

## <a name="download-and-configure-the-sample-application"></a>İndirme ve örnek uygulamayı yapılandırma

Azure anahtar kasası örneği indirin [anahtar kasası istemci örnekleri](https://www.microsoft.com/download/details.aspx?id=45343) sayfası. Geliştirme iş istasyonunuzda .zip dosyasının içeriğini ayıklayın. Örnekler klasöründe bulunan iki uygulama vardır. Bu makalede **HelloKeyVault**.

Yüklenecek **HelloKeyVault** örnek:

1. Gözat **Microsoft.Azure.KeyVault.Samples** > **örnekleri** > **HelloKeyVault** klasör.
2. Açık **HelloKeyVault** Visual Studio'da uygulama.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

Visual Studio'da:

1. HelloKeyVault\App.config dosyasını açın ve bulma `<appSettings>` öğesi.
2. Güncelleştirme **VaultUrl**, **AuthClientId**, ve **AuthClientSecret** anahtar kasası oluştururken, değerleri anahtarlarıyla döndürdü. Varsayılan olarak, App.config dosyası için bir yer tutucu sahip `AuthCertThumbprint`. Bu yer tutucu ile değiştirin `AuthClientSecret`.

   ![Uygulama ayarları](media/azure-stack-key-vault-sample-app/appconfig.png)

3. Çözümü yeniden derleyin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Çalıştırdığınızda **HelloKeyVault**, uygulamayı Azure AD'ye açar ve ardından `AuthClientSecret` anahtar kasasına Azure Stack'te kimlik doğrulaması için belirteç.

Kullanabileceğiniz **HelloKeyVault** için örnek:

* Anahtarları ve gizli anahtarları gibi oluştururken, şifrelemek, Kaydır ve silme temel işlemleri gerçekleştirin.
* Gibi parametreler `encrypt` ve `decrypt` için **HelloKeyVault**, bir anahtar kasasına belirtilen değişiklikleri uygularsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Anahtar kasası sertifikası ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
