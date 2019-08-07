---
title: Key Vault içinde depolanan bir parolayı kullanarak Azure Stack VM dağıtma | Microsoft Docs
description: Azure Stack anahtar kasasında depolanan bir parolayı kullanarak bir VM dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 480740b12796fe90e2acd6fd1eb164b4c89d5ded
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842820"
---
# <a name="deploy-an-azure-stack-vm-using-a-password-stored-in-key-vault"></a>Key Vault depolanan bir parolayı kullanarak Azure Stack VM dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack Key Vault depolanan bir parolayı kullanarak bir Windows Server sanal makinesi (VM) dağıtma adımları sağlanır. Anahtar Kasası parolasının kullanılması, düz metin parolası geçirmeden daha güvenlidir.

## <a name="overview"></a>Genel Bakış

Parola gibi değerleri, bir Azure Stack anahtar kasasında gizli dizi olarak saklayabilirsiniz. Gizli dizi oluşturduktan sonra, Azure Resource Manager şablonlarda başvurabilirsiniz. Kaynak Yöneticisi ile gizli dizileri kullanmak aşağıdaki avantajları sağlar:

* Her kaynak dağıttığınızda gizli anahtarı el ile girmeniz gerekmez.
* Hangi kullanıcıların veya hizmet sorumlularının gizli bir parolaya erişebileceğini belirtebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Key Vault hizmetini içeren bir teklife abone olmanız gerekir.
* [Azure Stack için PowerShell 'i yükler.](../operator/azure-stack-powershell-install.md)
* [PowerShell ortamınızı yapılandırın.](azure-stack-powershell-configure-user.md)

Aşağıdaki adımlarda, bir Key Vault depolanan parolayı alarak VM oluşturmak için gereken işlem açıklanır:

1. Key Vault gizli dizisi oluşturun.
2. `azuredeploy.parameters.json` Dosyayı güncelleştirin.
3. Şablonu dağıtın.

> [!NOTE]  
> Bu adımları Azure Stack Geliştirme Seti (ASDK) veya VPN üzerinden bağlıysanız bir dış istemciden kullanabilirsiniz.

## <a name="create-a-key-vault-secret"></a>Key Vault gizli dizi oluşturma

Aşağıdaki betik bir Anahtar Kasası oluşturur ve anahtar kasasında bir parolayı gizli olarak depolar. Anahtar kasasını oluştururken parametresini kullanın. `-EnabledForDeployment` Bu parametre, anahtar kasasının Azure Resource Manager şablonlarından başvurulduğundan emin olmanızı sağlar.

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Önceki betiği çalıştırdığınızda, çıktı gizli URI 'yi (Tekdüzen Kaynak tanımlayıcısı) içerir. Bu URI 'yi bir yere getirin. [Anahtar Kasası şablonunda WINDOWS VM 'yi parolayla dağıtma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) ' ya başvurmanız gerekir. Geliştirme bilgisayarınıza [101-VM-Secure-Password](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) klasörünü indirin. Bu klasör, sonraki `azuredeploy.json` adımlarda `azuredeploy.parameters.json` ihtiyacınız olacak ve dosyalarını içerir.

`azuredeploy.parameters.json` Dosyayı ortam değerlerinize göre değiştirin. Özel ilgilendiğiniz parametreler kasa adı, kasa kaynak grubu ve gizli URI (önceki komut dosyası tarafından oluşturulan). Aşağıdaki dosya bir parametre dosyası örneğidir.

## <a name="update-the-azuredeployparametersjson-file"></a>Azuredeploy. Parameters. json dosyasını güncelleştirme

`azuredeploy.parameters.json` Dosyayı ortamınıza göre sanal makine değerlerinin keykasası URI 'si, secretname, AdminUserName ile güncelleştirin. Aşağıdaki JSON dosyası, şablon parametreleri dosyasına bir örnek gösterir:

```json
{
    "$schema":  "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Şablon dağıtımı

Şimdi aşağıdaki PowerShell betiğini kullanarak şablonu dağıtın:

```powershell  
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Şablon başarıyla dağıtıldığında, aşağıdaki çıkışa neden olur:

![Dağıtım çıkışı](media/azure-stack-key-vault-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault örnek uygulama dağıtma](azure-stack-key-vault-sample-app.md)
* [Key Vault sertifikası ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
