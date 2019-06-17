---
title: Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak Stack dağıtma | Microsoft Docs
description: Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak Stack dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 02/11/2019
ms.openlocfilehash: c5d1643dbeb3c4c2805ebc1411cdce451889a288
ms.sourcegitcommit: ad2f2cb4dc8d5cf0c2c37517d5125921cff44cdd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67138867"
---
# <a name="deploy-kubernetes-to-azure-stack-using-active-directory-federated-services"></a>Kubernetes kullanarak Active Directory Federasyon Hizmetleri Azure Stack'e dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo, preview tarafından şu anda desteklenmemektedir.

Dağıtma ve Active Directory Federasyon Hizmetleri'nde (AD FS) kimlik yönetimi hizmetiniz olduğunda, Kubernetes için kaynakları ayarlamak için bu makaledeki adımları izleyin.

## <a name="prerequisites"></a>Önkoşullar 

Başlamak için doğru izinlere sahip ve Azure Stack hazır olduğundan emin olun.

1. Azure Stack'te Linux sanal makinesi (VM) oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtar gerekir.

    Bir anahtarı oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Azure Stack Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Kullanmanız gereken bir **kullanıcı** abonelik. 

1. Anahtar kasası hizmeti, Azure Stack aboneliğinize gerekir.

1. Kubernetes kümesi, Market'te gerekir. 

Key Vault hizmetine ve Kubernetes kümesi Market öğesi kayıpsa, Azure Stack yöneticinizle konuşun.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

AD FS kimlik çözümünüz olarak kullanırken hizmet sorumlunuzu ayarlamak için Azure Stack yöneticinizle birlikte çalışın. Hizmet sorumlusu, Azure Stack kaynakları için erişim sağlar.

1. Azure Stack yöneticinize bir sertifika ve hizmet sorumlusu bilgilerini sağlar.

   - Hizmet sorumlusu bilgileri gibi görünmelidir:

     ```Text  
       ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
       ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
       Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
       ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
       PSComputerName        : azs-ercs01
       RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
     ```

   - Sertifikanızı uzantılı bir dosya olacaktır `.pfx`. Bir keyvault'ta gizli olarak sertifikanızı depolayacağınızı.

2. Yeni hizmet sorumlunuzu Aboneliğinize bir katkıda bulunan olarak bir rol atayın. Yönergeler için [rol atama](../operator/azure-stack-create-service-principals.md).

3. Dağıtım için sertifikanızın depolanacağı key vault oluşturma. Portal yerine aşağıdaki PowerShell betiklerini kullanın:

   - Şu bilgilere ihtiyacınız vardır:

       | Değer | Açıklama |
       | ---   | ---         |
       | Azure Resource Manager uç noktası | Microsoft Azure Resource Manager yöneticilerin dağıtma, yönetme ve Azure kaynaklarını izleme sağlayan yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.<br>Azure Stack geliştirme Seti'ni (ASDK) uç noktası şöyledir: `https://management.local.azurestack.external/`<br>Tümleşik sistemlerde uç noktadır: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
       | Abonelik Kimliğiniz | [Abonelik kimliği](../operator/azure-stack-plan-offer-quota-overview.md#subscriptions) nasıl, teklifler eriştiği Azure Stack'te. |
       | Kullanıcı adınız | Yalnızca kullanıcı adı yerine etki alanı adı ve kullanıcı adı gibi kullanın `username` yerine `azurestack\username`. |
       | Kaynak grubu adı  | Mevcut bir kaynak grubu seçin ya da yeni kaynak grubu adı. Kaynak adı alfasayısal ve küçük harf olması gerekir. |
       | Keyvault adı | Kasa adı.<br> Regex deseni: `^[a-zA-Z0-9-]{3,24}$` |
       | Kaynak grubu konumu | Azure Stack yüklemeniz için seçtiğiniz kaynak grubunda ve bölgede konumu. |

   - Yükseltilmiş bir İstemi ile PowerShell'i açın ve [Azure Stack'e bağlanma](azure-stack-powershell-configure-user.md#connect-to-azure-stack-with-ad-fs). Değerlerinizi güncelleştirilmiş parametrelerle birlikte aşağıdaki betiği çalıştırın:

   ```powershell  
       $armEndpoint="<Azure Resource Manager Endpoint>"
       $subscriptionId="<Your Subscription ID>"
       $username="<your user name >"
       $resource_group_name = "<the resource group name >"
       $key_vault_name = "<keyvault name>"
       $resource_group_location="<resource group location>"
        
       # Login Azure Stack Environment
       Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
       $mycreds = Get-Credential
       Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
        
       # Create new Resource group and key vault
       New-AzureRmResourceGroup -Name $resource_group_name -Location $resource_group_location -Force
        
       # Note, Do not omit -EnabledForTemplateDeployment flag
       New-AzureRmKeyVault -VaultName $key_vault_name -ResourceGroupName $resource_group_name -Location $resource_group_location -EnabledForTemplateDeployment
        
       # Obtain the security identifier(SID) of the active directory user
       $adUser = Get-ADUser -Filter "Name -eq '$username'" -Credential $mycreds
       $objectSID = $adUser.SID.Value
       # Set the key vault access policy
       Set-AzureRmKeyVaultAccessPolicy -VaultName $key_vault_name -ResourceGroupName $resource_group_name -ObjectId $objectSID -BypassObjectIdValidation -PermissionsToKeys all -PermissionsToSecrets all
     ```

4. Sertifikanızın anahtar Kasası'na yükleyin.

   - Şu bilgilere ihtiyacınız vardır:

       | Değer | Açıklama |
       | ---   | ---         |
       | Sertifika yolu | Sertifika FQDN veya dosya yolu. |
       | Sertifika parolası | Sertifika parolası. |
       | Gizli dizi adı | Kasada depolanan sertifika başvurmak için kullanılan gizli dizi adı. |
       | Anahtar kasası adı | Önceki adımda oluşturduğunuz anahtar kasasının adı. |
       | Azure Resource Manager uç noktası | Azure Stack geliştirme Seti'ni (ASDK) uç noktası şöyledir: `https://management.local.azurestack.external/`<br>Tümleşik sistemlerde uç noktadır: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
       | Abonelik Kimliğiniz | [Abonelik kimliği](../operator/azure-stack-plan-offer-quota-overview.md#subscriptions) nasıl, teklifler eriştiği Azure Stack'te. |

   - Yükseltilmiş bir İstemi ile PowerShell'i açın ve [Azure Stack'e bağlanma](azure-stack-powershell-configure-user.md#connect-to-azure-stack-with-ad-fs). Değerlerinizi güncelleştirilmiş parametrelerle birlikte aşağıdaki betiği çalıştırın:

    ```powershell
        
     # upload the pfx to key vault
     $tempPFXFilePath = "<certificate path>"
     $password = "<certificate password>"
     $keyVaultSecretName = "<secret name>"
     $keyVaultName = "<key vault name>"
     $armEndpoint="<Azure Resource Manager Endpoint>"
     $subscriptionId="<Your Subscription ID>"
     # Login Azure Stack Environment
     Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
     $mycreds = Get-Credential
     Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
    
     $certContentInBytes = [io.file]::ReadAllBytes($tempPFXFilePath)
     $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
     $jsonObject = @"
     {
     "data": "$pfxAsBase64EncodedString",
     "dataType" :"pfx",
     "password": "$password"
     }
     "@
     $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
     $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
     $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
     $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret 
     ```

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).

1. Seçin **+ kaynak Oluştur** > **işlem** > **Kubernetes kümesi**. **Oluştur**’a tıklayın.

    ![Kubernetes kümesi oluşturma](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Temel Bilgiler

1. Seçin **Temelleri** Kubernetes kümesi oluşturun.

    ![Temel bilgileri Kubernetes kümesi oluşturma](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Seçin, **abonelik** kimliği

1. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

1. Seçin **konumu** kaynak grubu. Azure Stack yüklemeniz için seçtiğiniz bölgeye konumdur.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes küme ayarları

1. Seçin **Kubernetes küme ayarlarını** Kubernetes kümesi oluşturun.

    ![Kubernetes küme ayarları](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-adfs.png)

1. Girin **Linux VM yönetici kullanıcı adı** için Linux Kubernetes kümesi ve DVM parçası olan sanal makineler.

1. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

1. Girin **ana profili DNS ön eki**. Bu ad bir bölge-benzersiz gibi olmalıdır `k8s-12345`. Kaynak grubu adı en iyi uygulama olarak eşleşecek şekilde deneyin.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

1. Seçin **Kubernetes ana havuzu profili sayısı**. Sayıyı ana havuzdaki düğüm sayısını içerir. 7 1'den olabilir. Bu değer, tek bir sayı olmalıdır.

1. Seçin **Kubernetes ana Vm'lerden oluşan bir VMSize**.

1. Seçin **Kubernetes düğüm havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 

1. Seçin **depolama profili**. Seçebileceğiniz **Blob Disk** veya **yönetilen Disk**. Bu seçim, VM boyutu Kubernetes düğümünü sanal makinelere belirtir. 

1. Seçin **ADFS** için **Azure Stack kimlik sistemi** Azure Stack yüklemenizin.

1. Girin **hizmet sorumlusu ClientID**. Bu tanımlayıcı, Kubernetes Azure bulut sağlayıcısı tarafından kullanılır. Azure Stack yöneticinize hizmet sorumlusu oluştururken istemci kimliği, uygulama kimliği olarak adlandırılır.

1. Girin **Key Vault kaynak grubu** sertifikanızı içeren anahtar kasası kaynakları.

1. Girin **Key Vault adı**. Sertifikanızı bir gizli dizi olarak içeren anahtar kasası adıdır. 

1. Girin **Key Vault gizli**. Bu gizli dizi adı, sertifikanızın başvuruyor.

1. Girin **Kubernetes Azure bulut sağlayıcısı sürümü**. Bu Kubernetes Azure sağlayıcısı sürüm numarasıdır. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

### <a name="3-summary"></a>3. Özet

1. Özet'i seçin. Dikey bir Kubernetes kümesi yapılandırma ayarlarınızı doğrulama iletisi görüntüler.

    ![Kubernetes küme yapılandırma özeti](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Ayarlarınızı gözden geçirin.

3. Seçin **Tamam** kümenize dağıtmak için.

> [!TIP]  
>  Dağıtımınız hakkında sorularınız varsa sorunuzu gönderin veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="next-steps"></a>Sonraki adımlar

[Kümenize bağlanın](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Kubernetes panosunu etkinleştir](azure-stack-solution-template-kubernetes-dashboard.md)