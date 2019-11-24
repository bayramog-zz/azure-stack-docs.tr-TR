---
title: Azure Stack sunucuda güvenli bir şekilde depolanan sertifikayla VM dağıtma | Microsoft Docs
description: Bir sanal makineyi dağıtmayı ve Azure Stack bir Anahtar Kasası kullanarak bu sertifikaya nasıl sertifika göndereceğinizi öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 8741d63dbbcefde950fc10c0917d87bc4e9718f7
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961531"
---
# <a name="deploy-a-vm-with-a-securely-stored-certificate-on-azure-stack"></a>Azure Stack üzerinde güvenli bir şekilde depolanan sertifikayla VM dağıtma 

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, bir Key Vault sertifikası yüklü Azure Stack sanal makinenin (VM) nasıl dağıtılacağı açıklanır.

## <a name="overview"></a>Genel Bakış

Sertifikalar, Active Directory kimlik doğrulaması veya Web trafiğini şifreleme gibi birçok senaryoda kullanılır. Sertifikaları bir Azure Stack anahtar kasasında gizli dizi olarak güvenli bir şekilde depolayabilirsiniz. Azure Stack Key Vault kullanmanın avantajları şunlardır:

* Sertifikalar bir betikte, komut satırı geçmişinde veya şablonda gösterilmez.
* Sertifika Yönetimi işlemi basitleştirilmiştir.
* Sertifikalara erişim sağlayan anahtarların denetimine sahipsiniz.

## <a name="process-description"></a>İşlem açıklaması

Aşağıdaki adımlarda, VM 'ye bir sertifika göndermek için gereken işlem açıklanır:

1. Anahtar Kasası gizli dizisi oluşturun.
2. **Azuredeploy. Parameters. JSON** dosyasını güncelleştirin.
3. Şablonu dağıtın.

> [!NOTE]
> Bu adımları Azure Stack Geliştirme Seti (ASDK) veya VPN üzerinden bağlıysanız bir dış istemciden kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Key Vault hizmetini içeren bir teklife abone olmanız gerekir.
* [Azure Stack Için PowerShell 'ı yükler](../operator/azure-stack-powershell-install.md).
* [Azure Stack kullanıcının PowerShell ortamını yapılandırın](azure-stack-powershell-configure-user.md).

## <a name="create-a-key-vault-secret"></a>Anahtar Kasası gizli dizisi oluşturma

Aşağıdaki betik. pfx biçiminde bir sertifika oluşturur, bir Anahtar Kasası oluşturur ve sertifikayı anahtar kasasında gizli dizi olarak depolar.

> [!IMPORTANT]
> Anahtar kasasını oluştururken `-EnabledForDeployment` parametresini kullanmanız gerekir. Bu parametre, anahtar kasasının Azure Resource Manager şablonlarından başvurulabilmesini sağlar.

```powershell
# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<certificate thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path to where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path to where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret
```

Bu betiği çalıştırdığınızda, çıktı gizli URI 'yi içerir. Bu URI 'yi, [gönderme sertifikası Windows Kaynak Yöneticisi şablonuna](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate)başvurmanız gerektiği için bir yere unutmayın. [VM-Push-Certificate-Windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) şablon klasörünü geliştirme bilgisayarınıza indirin. Bu klasör, aşağıdaki adımlarda ihtiyacınız olan **azuredeploy. JSON** ve **azuredeploy. Parameters. JSON** dosyalarını içerir.

**Azuredeploy. Parameters. JSON** dosyasını ortam değerlerinize göre değiştirin. Önemli parametreler, kasa adı, kasa kaynak grubu ve gizli URI (önceki komut dosyası tarafından oluşturulan). Aşağıdaki bölümde bir parametre dosyası örneği gösterilmektedir.

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy. Parameters. json dosyasını güncelleştirme

**Azuredeploy. Parameters. JSON** dosyasını `vaultName`, gizli urı, `VmName`ve diğer parametrelerle ortamınıza göre güncelleştirin. Aşağıdaki JSON dosyası, şablon parametreleri dosyasına bir örnek gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki PowerShell betiğini kullanarak şablonu dağıtın:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret to it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablon başarıyla dağıtıldığında, aşağıdaki çıktıyı görüntüler:

![Şablon dağıtımı sonuçları](media/azure-stack-key-vault-push-secret-into-vm/deployment-output.png)

Azure Stack, dağıtım sırasında sertifikayı VM 'ye iter. Sertifika konumu, sanal makinenin işletim sistemine bağlıdır:

* Windows 'da, sertifika, kullanıcının sağladığımız sertifika deposuyla, **LocalMachine** sertifika konumuna eklenir.
* Linux 'ta, sertifika **/var/lib/waagent** dizinine yerleştirilir ve bu, x509 sertifika dosyası için en üst **öğe adı ve özel anahtar Için üst** öğe **parmak izi. prv** .

## <a name="retire-certificates"></a>Sertifikaları devre dışı bırak

Sertifikaları devre dışı bırakma, sertifika yönetimi sürecinin bir parçasıdır. Bir sertifikanın eski sürümünü silemezsiniz, ancak `Set-AzureKeyVaultSecretAttribute` cmdlet 'ini kullanarak devre dışı bırakabilirsiniz.

Aşağıdaki örnek, bir sertifikanın nasıl devre dışı bırakılacağını gösterir. `VaultName`, `Name`ve `Version` parametreleri için kendi değerlerinizi kullanın.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası parolası ile VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Uygulamanın Key Vault erişmesine izin ver](azure-stack-key-vault-sample-app.md)
