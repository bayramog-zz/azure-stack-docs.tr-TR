---
title: Uygulamaların Key Vault gizli dizileri Azure Stack erişmesine izin ver | Microsoft Docs
description: Azure Stack bir anahtar kasasından anahtarlar ve gizli dizileri alan örnek bir uygulamayı çalıştırmayı öğrenin.
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
ms.date: 07/17/2019
ms.author: sethm
ms.lastreviewed: 04/08/2019
ms.openlocfilehash: fa5a602fbdca32aed635f587fef248bcff0dae06
ms.sourcegitcommit: 2063332b4d7f98ee944dd1f443847eea70eb5614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303158"
---
# <a name="allow-apps-to-access-azure-stack-key-vault-secrets"></a>Uygulamaların Key Vault gizli dizileri Azure Stack erişmesine izin ver

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack bir anahtar kasasından anahtarlar ve gizli dizileri alan örnek uygulama **Hellokeykasasını** çalıştırmak için bu makaledeki adımları izleyin.

## <a name="prerequisites"></a>Önkoşullar

[VPN üzerinden bağlıysanız](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn), [Azure Stack geliştirme seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp)veya Windows tabanlı bir dış istemciden aşağıdaki önkoşulları yükleyebilirsiniz:

* Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).

## <a name="create-a-key-vault-and-register-an-app"></a>Anahtar Kasası oluşturma ve bir uygulamayı kaydetme

Örnek uygulamaya hazırlanmak için:

* Azure Stack bir Anahtar Kasası oluşturun.
* Azure Active Directory (Azure AD) bir uygulamayı kaydedin.

Örnek uygulamaya hazırlanmak için Azure portal veya PowerShell kullanın.

> [!NOTE]
> Varsayılan olarak, PowerShell betiği Active Directory yeni bir uygulama oluşturur. Ancak, mevcut uygulamalarınızdan birini kaydedebilirsiniz.

Aşağıdaki betiği çalıştırmadan önce `aadTenantName` ve `applicationPassword` değişkenleri için değerler sağladığınızdan emin olun. İçin `applicationPassword`bir değer belirtmezseniz, bu betik rastgele bir parola oluşturur.

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

Aşağıdaki görüntüde, anahtar kasasını oluşturmak için kullanılan Betiğin çıktısı gösterilmektedir:

![Erişim anahtarları olan Anahtar Kasası](media/azure-stack-key-vault-sample-app/settingsoutput.png)

Önceki betik tarafından döndürülen **Vaulturl**, **Authclientıd**ve **authclientsecret** değerlerini bir yere göz atın. Bu değerleri, **Hellokeykasası** uygulamasını çalıştırmak için kullanırsınız.

## <a name="download-and-configure-the-sample-application"></a>Örnek uygulamayı indirme ve yapılandırma

Azure [Key Vault istemci örnekleri](https://www.microsoft.com/download/details.aspx?id=45343) sayfasından Anahtar Kasası örneğini indirin. Geliştirme iş istasyonunuzda. zip dosyasının içeriğini ayıklayın. Samples klasöründe iki uygulama vardır. Bu makalede, **Hellokeykasası**kullanılmaktadır.

**Merhaba Anahtar Kasası** örneğini yüklemek için:

1. **Microsoft. Azure. keykasası. Samples** > **Samples** > **hellokeykasa** klasörüne gidin.
2. Visual Studio 'da **Hellokeykasası** uygulamasını açın.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

Visual Studio'da:

1. Merhaba keyvault\appnconfig dosyasını açın ve `<appSettings>` öğesini bulun.
2. Anahtar Kasası oluştururken döndürülen değerlerle **Vaulturl**, **Authclientıd**ve **authclientsecret** anahtarlarını güncelleştirin. Varsayılan olarak, App. config dosyası için `AuthCertThumbprint`bir yer tutucudur. Bu yer tutucusunu ile `AuthClientSecret`değiştirin.

   ![Uygulama ayarları](media/azure-stack-key-vault-sample-app/appconfig.png)

3. Çözümü yeniden derleyin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

**Merhaba Anahtar Kasası**'nı çalıştırdığınızda, uygulama Azure AD 'de oturum açar ve ardından Azure Stack, anahtar kasasında kimlik doğrulaması yapmak için `AuthClientSecret` belirteci kullanır.

**Merhaba Keykasa** örneğini kullanarak şunları yapabilirsiniz:

* Anahtarlar ve gizli diziler üzerinde oluşturma, şifreleme, sarması ve silme gibi temel işlemleri gerçekleştirin.
* `encrypt` Ve`decrypt` gibi parametreleri **Merhaba Anahtar Kasası**'na geçirin ve belirtilen değişiklikleri bir anahtar kasasına uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault sertifikası ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
