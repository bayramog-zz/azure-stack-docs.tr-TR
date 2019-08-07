---
title: PowerShell kullanarak Azure Stack Key Vault yönetme | Microsoft Docs
description: PowerShell kullanarak Azure Stack Key Vault yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2019
ms.author: sethm
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: ca303590d4dc923380e10e50fc9b8b9ce2e5aac6
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842968"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>PowerShell kullanarak Azure Stack Key Vault yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, PowerShell kullanarak Azure Stack bir anahtar kasasının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır. Key Vault PowerShell cmdlet 'lerini nasıl kullanacağınızı öğreneceksiniz:

* Bir anahtar kasası oluşturma.
* Şifreleme anahtarlarını ve gizli dizileri depolayın ve yönetin.
* Kullanıcıları veya uygulamaları kasadaki işlemleri çağırmak için yetkilendirin.

>[!NOTE]
>Bu makalede açıklanan Key Vault PowerShell cmdlet 'leri Azure PowerShell SDK 'sında verilmiştir.

## <a name="prerequisites"></a>Önkoşullar

* Azure Key Vault hizmetini içeren bir teklife abone olmanız gerekir.
* [Azure Stack Için PowerShell 'ı yükler](../operator/azure-stack-powershell-install.md).
* [Azure Stack kullanıcının PowerShell ortamını yapılandırın](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Key Vault işlemler için kiracı aboneliğinizi etkinleştirme

Bir anahtar kasasında herhangi bir işlem yapabilmeniz için önce kiracı aboneliğinizin kasa işlemlerinde etkinleştirildiğinden emin olmanız gerekir. Kasa işlemlerinin etkinleştirildiğini doğrulamak için şu komutu çalıştırın:

```powershell  
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

Aboneliğiniz kasa işlemleri için etkinleştirilmişse, çıkış, bir anahtar kasasının tüm kaynak türleri Için **registrationstate** ' i gösterir.

![PowerShell 'de Anahtar Kasası kayıt durumu](media/azure-stack-key-vault-manage-powershell/image1.png)

Kasa işlemleri etkinleştirilmemişse Key Vault hizmetini aboneliğinize kaydetmek için aşağıdaki komutu verin:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

Kayıt başarılı olursa, aşağıdaki çıktı döndürülür:

![PowerShell 'de Anahtar Kasası kaydı başarılı](media/azure-stack-key-vault-manage-powershell/image2.png)

Anahtar Kasası komutlarını çağırdığınızda, "abonelik ' Microsoft. Keykasası ' ad alanını kullanacak şekilde kaydedilmemiş" gibi bir hata alabilirsiniz. Bir hata alırsanız, önceki yönergeleri izleyerek Key Vault kaynak sağlayıcısını etkinleştirdiğini doğrulayın.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Anahtar Kasası oluşturmadan önce, anahtar kasası ile ilgili tüm kaynakların bir kaynak grubunda mevcut olması için bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "VaultRG" -Location local -verbose -Force
```

![PowerShell 'de oluşturulan yeni kaynak grubu](media/azure-stack-key-vault-manage-powershell/image3.png)

Şimdi, daha önce oluşturduğunuz kaynak grubunda bir Anahtar Kasası oluşturmak için **New-Azurermkeykasası** komutunu kullanın. Bu komut üç zorunlu parametreyi okur: kaynak grubu adı, Anahtar Kasası adı ve coğrafi konum.

Bir Anahtar Kasası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmKeyVault -VaultName "Vault01" -ResourceGroupName "VaultRG" -Location local -verbose
```

![PowerShell 'de oluşturulan yeni Anahtar Kasası](media/azure-stack-key-vault-manage-powershell/image4.png)

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. Bir uygulama bu kasaya eriştiğinde, bu örnekteki **kasa URI** özelliğini `https://vault01.vault.local.azurestack.external` kullanması gerekir.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Active Directory Federasyon Hizmetleri (AD FS) (AD FS) dağıtımı

AD FS dağıtımında şu uyarıyı alabilirsiniz: "Erişim ilkesi ayarlanmadı. Bu kasayı kullanmak için hiçbir Kullanıcı veya uygulamanın erişim izni yok. " Bu sorunu çözmek için [set-AzureRmKeyVaultAccessPolicy](#authorize-an-app-to-use-a-key-or-secret) komutunu kullanarak kasa için bir erişim ilkesi ayarlayın:

```powershell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli dizileri yönetme

Bir kasa oluşturduktan sonra, kasada anahtar ve gizli dizileri oluşturmak ve yönetmek için bu adımları kullanın.

### <a name="create-a-key"></a>Anahtar oluşturma

Anahtar kasasında yazılım korumalı anahtar oluşturmak veya içeri aktarmak için **Add-AzureKeyVaultKey** komutunu kullanın:

```powershell
Add-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01" -verbose -Destination Software
```

**Hedef** parametresi, anahtarın yazılım korumalı olduğunu belirtmek için kullanılır. Anahtar başarıyla oluşturulduğunda, komut oluşturulan anahtarın ayrıntılarını çıktı olarak verir.

![PowerShell 'de oluşturulan yeni anahtar kasası anahtarı](media/azure-stack-key-vault-manage-powershell/image5.png)

Şimdi oluşturulan anahtara URI 'sini kullanarak başvurabilirsiniz. Varolan bir anahtarla aynı ada sahip bir anahtar oluşturur veya içeri aktarırsanız, özgün anahtar yeni anahtarda belirtilen değerlerle güncelleştirilir. Önceki sürüme, anahtarın sürüme özgü URI 'sini kullanarak erişebilirsiniz. Örneğin:

* Her `https://vault10.vault.local.azurestack.external:443/keys/key01` zaman geçerli sürümü almak için kullanın.
* Bu `https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a` belirli sürümü almak için kullanın.

### <a name="get-a-key"></a>Anahtar al

Bir anahtarı ve ayrıntılarını okumak için **Get-AzureKeyVaultKey** komutunu kullanın:

```powershell
Get-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01"
```

### <a name="create-a-secret"></a>Gizli dizi oluşturma

Bir kasada gizli dizi oluşturmak veya güncelleştirmek için **set-AzureKeyVaultSecret** komutunu kullanın. Bir gizli dizi yoksa bir gizli dizi oluşturulur. Zaten varsa, gizli dizinin yeni bir sürümü oluşturulur:

```powershell
$secretvalue = ConvertTo-SecureString "User@123" -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01" -SecretValue $secretvalue
```

![PowerShell 'de gizli dizi oluşturma](media/azure-stack-key-vault-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Gizli dizi alın

Anahtar kasasındaki bir gizli anahtarı okumak için **Get-AzureKeyVaultSecret** komutunu kullanın. Bu komut, bir gizli dizinin tüm veya belirli sürümlerini döndürebilir:

```powershell
Get-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01"
```

Anahtarları ve gizli dizileri oluşturduktan sonra, bunları kullanmak için dış uygulamalara yetki verebilirsiniz.

## <a name="authorize-an-app-to-use-a-key-or-secret"></a>Bir uygulamayı anahtar veya gizli dizi kullanacak şekilde yetkilendirme

Bir uygulamanın anahtar kasasındaki bir anahtara veya gizli anahtara erişmesini yetkilendirmek için **set-AzureRmKeyVaultAccessPolicy** komutunu kullanın.

Aşağıdaki örnekte, kasa adı *Contosokeykasadır* ve yetkilendirmek istediğiniz uygulamanın, *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*istemci kimliği vardır. Uygulamayı yetkilendirmek için aşağıdaki komutu çalıştırın. Ayrıca, bir Kullanıcı, uygulama veya güvenlik grubu için izinleri ayarlamak üzere **Permissionstokeys** parametresini de belirtebilirsiniz.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Kasadaki gizli dizileri okumak için aynı uygulamayı yetkilendirmek istiyorsanız aşağıdaki cmdlet 'i çalıştırın:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault depolanan bir parolayla VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault depolanan bir sertifika ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
