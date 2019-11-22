---
title: Azure Stack bir güvenli Service Fabric kümesi dağıtma | Microsoft Docs
description: Azure Stack 'da güvenli Service Fabric kümesini dağıtmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: shnatara
ms.lastreviewed: 09/25/2019
ms.openlocfilehash: 1b6975490a876c0ff0b51fdf9f21ba010e14622d
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74299096"
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Azure Stack bir Service Fabric kümesi dağıtma

Azure Stack güvenli bir Service Fabric kümesi dağıtmak için Azure Marketi 'ndeki **Service Fabric küme** öğesini kullanın. 

Service Fabric ile çalışma hakkında daha fazla bilgi için bkz. Azure [Service Fabric 'ye genel bakış](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) ve Azure belgelerindeki [küme güvenliği senaryoları Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) .

Azure Stack Service Fabric kümesi, Microsoft. ServiceFabric kaynak sağlayıcısını kullanmaz. Bunun yerine, Azure Stack Service Fabric kümesi, [Istenen durum yapılandırması (DSC)](https://docs.microsoft.com/powershell/dsc/overview/overview)kullanılarak önceden yüklenmiş yazılım içeren bir sanal makine ölçek kümesidir.

## <a name="prerequisites"></a>Önkoşullar

Service Fabric kümesi dağıtmak için aşağıdakiler gereklidir:
1. **Küme sertifikası**  
   Bu, Service Fabric dağıtımı sırasında Key Vault eklediğiniz X. 509.440 sunucu sertifikasıdır. 
   - Bu sertifika üzerindeki **CN** , oluşturduğunuz Service Fabric kümesinin tam etki alanı adı (FQDN) ile eşleşmelidir. 
   - Hem ortak hem de özel anahtarlar gerekli olduğundan, sertifika biçiminin PFX olması gerekir. 
     Bu sunucu tarafı sertifikası oluşturma [gereksinimlerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bakın.

     > [!NOTE]  
     > Test amaçları için X. 509.440 sunucu sertifikasının kendinden imzalı bir sertifika kullanabilirsiniz. Otomatik olarak imzalanan sertifikaların kümenin FQDN 'siyle eşleşmesi gerekmez.

1. **Yönetici Istemci sertifikası**  
   Bu, istemcinin, kendinden imzalı olabilen Service Fabric kümesinde kimlik doğrulamak için kullandığı sertifikadır. Bu istemci sertifikası oluşturma [gereksinimlerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) bakın.

1. **Aşağıdaki öğeler Azure Stack marketi 'nde kullanılabilir olmalıdır:**
    - **Windows server 2016** -şablon, kümeyi oluşturmak Için windows Server 2016 görüntüsünü kullanır.  
    - **Özel Betik uzantısı** -Microsoft 'Tan sanal makine uzantısı.  
    - **PowerShell Istenen aşama yapılandırması** -sanal makine uzantısı Microsoft 'tan.


## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme
Service Fabric kümesi dağıtmak için, Service Fabric kümesi için doğru Key Vault *gizli tanımlayıcı* veya URL 'yi belirtmeniz gerekir. Azure Resource Manager şablonu girdi olarak bir Key Vault alır. Ardından şablon, Service Fabric kümesini yüklerken küme sertifikasını alır.

> [!IMPORTANT]  
> Service Fabric ile kullanmak üzere Key Vault bir gizli dizi eklemek için PowerShell kullanmanız gerekir. Portalını kullanmayın.  

Key Vault oluşturmak ve *kümeye küme sertifikası* eklemek için aşağıdaki betiği kullanın. ( [Önkoşullara](#prerequisites)bakın.) Betiği çalıştırmadan önce, örnek betiği gözden geçirin ve belirtilen parametreleri ortamınızla eşleşecek şekilde güncelleştirin. Bu betik Ayrıca Azure Resource Manager şablonuna sağlamanız gereken değerleri de çıktısını verir. 

> [!TIP]  
> Betiğin başarılı olabilmesi için Işlem, ağ, depolama ve Key Vault hizmetlerini içeren bir genel teklif olmalıdır. 

  ```powershell
    function Get-ThumbprintFromPfx($PfxFilePath, $Password) 
        {
            return New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($PfxFilePath, $Password)
        }
    
    function Publish-SecretToKeyVault ($PfxFilePath, $Password, $KeyVaultName)
       {
            $keyVaultSecretName = "ClusterCertificate"
            $certContentInBytes = [io.file]::ReadAllBytes($PfxFilePath)
            $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
    
            $jsonObject = ConvertTo-Json -Depth 10 ([pscustomobject]@{
                data     = $pfxAsBase64EncodedString
                dataType = 'pfx'
                password = $Password
            })
    
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName -SecretValue $secret
            
            $pfxCertObject = Get-ThumbprintFromPfx -PfxFilePath $PfxFilePath -Password $Password
    
            Write-Host "KeyVault id: " -ForegroundColor Green
            (Get-AzureRmKeyVault -VaultName $KeyVaultName).ResourceId
            
            Write-Host "Secret Id: " -ForegroundColor Green
            (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName).id
    
            Write-Host "Cluster Certificate Thumbprint: " -ForegroundColor Green
            $pfxCertObject.Thumbprint
       }
    
    #========================== CHANGE THESE VALUES ===============================
    $armEndpoint = "https://management.local.azurestack.external"
    $tenantId = "your_tenant_ID"
    $location = "local"
    $clusterCertPfxPath = "Your_path_to_ClusterCert.pfx"
    $clusterCertPfxPassword = "Your_password_for_ClusterCert.pfx"
    #==============================================================================
    
    Add-AzureRmEnvironment -Name AzureStack -ARMEndpoint $armEndpoint
    Login-AzureRmAccount -Environment AzureStack -TenantId $tenantId
    
    $rgName = "sfvaultrg"
    Write-Host "Creating Resource Group..." -ForegroundColor Yellow
    New-AzureRmResourceGroup -Name $rgName -Location $location
    
    Write-Host "Creating Key Vault..." -ForegroundColor Yellow
    $Vault = New-AzureRmKeyVault -VaultName sfvault -ResourceGroupName $rgName -Location $location -EnabledForTemplateDeployment -EnabledForDeployment -EnabledForDiskEncryption
    
    Write-Host "Publishing certificate to Vault..." -ForegroundColor Yellow
    Publish-SecretToKeyVault -PfxFilePath $clusterCertPfxPath -Password $clusterCertPfxPassword -KeyVaultName $vault.VaultName
   ``` 


Daha fazla bilgi için bkz. [PowerShell ile Azure Stack Key Vault yönetme](azure-stack-key-vault-manage-powershell.md).

## <a name="deploy-the-marketplace-item"></a>Market öğesini dağıtma

1. Kullanıcı portalında, **Service Fabric küme** > **Işlem** > **kaynak oluştur ' a** gidin. 

   ![Service Fabric kümesi seçin](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

2. *Temel bilgiler*gibi her sayfa için dağıtım formunu doldurun. Bir değerden emin değilseniz Varsayılanları kullanın.

    Bağlantısı kesilmiş bir Azure Stack dağıtımlar veya Service Fabric başka bir sürümünü dağıtmak için, Service Fabric dağıtım paketini ve buna karşılık gelen çalışma zamanı paketini indirin ve bir Azure Stack blobu üzerinde barındırın. Bu değerleri **Service Fabric dağıtım paketi URL 'si** ve **Service Fabric çalışma zamanı paketi URL 'si** alanlarına girin.
    > [!NOTE]  
    > En son Service Fabric sürümü ile karşılık gelen SDK arasında uyumluluk sorunları vardır. Bu sorun giderilene kadar lütfen dağıtım paketi URL 'SI ve çalışma zamanı paketi URL 'SI için aşağıdaki parametreleri sağlayın. Aksi takdirde dağıtımlarınız başarısız olur.
    > - Service Fabric dağıtım paketi URL 'SI: <https://download.microsoft.com/download/8/3/6/836E3E99-A300-4714-8278-96BC3E8B5528/6.5.641.9590/Microsoft.Azure.ServiceFabric.WindowsServer.6.5.641.9590.zip>
    > - Service Fabric çalışma zamanı paketi URL 'SI: <https://download.microsoft.com/download/B/0/B/B0BCCAC5-65AA-4BE3-AB13-D5FF5890F4B5/6.5.641.9590/MicrosoftAzureServiceFabric.6.5.641.9590.cab>
    >
    > Bağlantısı kesilen dağıtımlar için, bu paketleri belirtilen konumdan indirin ve Azure Stack blob üzerinde yerel olarak barındırın.

   ![Temel Bilgiler](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

    
3. *Ağ ayarları* sayfasında, uygulamalarınız için açmak üzere belirli bağlantı noktaları belirtebilirsiniz:

   ![Ağ ayarları](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

4. *Güvenlik* sayfasında, [Azure Key Vault oluşturup](#add-a-secret-to-key-vault) parolayı karşıya yüklemeden aldığınız değerleri ekleyin.

   *Yönetici Istemci sertifikası parmak izi*Için, *yönetici istemci sertifikasının*parmak izini girin. ( [Önkoşullara](#prerequisites)bakın.)
   
   - Kaynak Key Vault: betik sonuçlarından tüm `keyVault id` dizesini belirtin. 
   - Küme sertifikası URL 'SI: betik sonuçlarından `Secret Id` tüm URL 'YI belirtin. 
   - Küme sertifikası parmak izi: betik sonuçlarından *küme sertifikası parmak Izini* belirtin.
   - Sunucu sertifikası URL 'SI: küme sertifikasından ayrı bir sertifika kullanmak istiyorsanız, sertifikayı bir anahtar kasasına yükleyin ve gizli dizinin tam URL 'sini sağlayın. 
   - Sunucu sertifikası parmak izi: sunucu sertifikası için parmak izini belirtin
   - Yönetici Istemci sertifikası parmak Izleri: ön koşullarda oluşturulan *yönetici Istemci sertifikası parmak Izini* belirtin. 

   ![Betik çıkışı](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Güvenlik](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

5. Sihirbazı tamamlayıp Service Fabric kümesi dağıtmak için **Oluştur** ' u seçin.



## <a name="access-the-service-fabric-cluster"></a>Service Fabric kümesine erişme
Service Fabric kümesine Service Fabric Explorer veya Service Fabric PowerShell kullanarak erişebilirsiniz.


### <a name="use-service-fabric-explorer"></a>Service Fabric Explorer kullan
1.  Tarayıcının yönetici istemci sertifikaya erişebildiğinden ve Service Fabric kümenizde kimlik doğrulayabilecek şekilde emin olun.  

    a. Internet Explorer 'ı açın ve **içerik** > **sertifikaları** > **Internet seçenekleri** ' ne gidin.
  
    b. Sertifikalar üzerinde **Içeri aktar** ' ı seçerek *sertifika alma Sihirbazı*' nı başlatın ve ardından **İleri**' ye tıklayın. *Içeri aktarılacak dosya* sayfasında, **Araştır**' a tıklayın ve Azure Resource Manager şablonuna verdiğiniz **yönetici istemci sertifikasını** seçin.
        
       > [!NOTE]  
       > Bu sertifika, Key Vault daha önce eklenmiş olan küme sertifikası değil.  

    c. Dosya Gezgini penceresinin uzantı açılan listesinde "kişisel bilgi değişimi" nin seçili olduğundan emin olun.  

       ![Kişisel bilgi değişimi](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. *Sertifika depolama alanı* sayfasında **Kişisel**' i seçin ve ardından Sihirbazı doldurun.  
       ![sertifika depolama](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
1. Service Fabric kümenizin FQDN 'sini bulmak için:  

    a. Service Fabric kümeniz ile ilişkili kaynak grubuna gidin ve *genel IP adresi* kaynağını bulun. Genel IP *adresi* dikey penceresini açmak IÇIN genel IP adresiyle ilişkili nesneyi seçin.  

      ![Genel IP adresi](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. Genel IP adresi dikey penceresinde FQDN, *DNS adı*olarak görüntülenir.  

      ![DNS adı](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

1. Service Fabric Explorer URL 'sini ve Istemci bağlantı uç noktasını bulmak için, Şablon dağıtımı sonuçlarını gözden geçirin.

1. Tarayıcınızda <https://*FQDN*:19080>' a gidin. *FQDN* 'yi, adım 2 ' deki SERVICE fabrıc kümenizin FQDN 'siyle değiştirin.   
   Kendinden imzalı bir sertifika kullandıysanız bağlantının güvenli olmadığını belirten bir uyarı alırsınız. Web sitesine devam etmek için **daha fazla bilgi**' yi seçin ve ardından **Web sayfasına gidin**. 

1. Sitenin kimliğini doğrulamak için kullanılacak bir sertifika seçmeniz gerekir. **Diğer seçenekler**' i seçin, uygun sertifikayı seçin ve ardından Service Fabric Explorer bağlanmak için **Tamam** ' ı tıklatın. 

   ![Kimlik doğrulaması](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



### <a name="use-service-fabric-powershell"></a>Service Fabric PowerShell kullanma

1. Azure Service Fabric belgelerinde [Windows üzerinde geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) *Microsoft Azure Service Fabric SDK 'sını* yükler.  

1. Yükleme tamamlandıktan sonra, Service Fabric cmdlet 'lerinin PowerShell 'den erişilebilir olduğundan emin olmak için sistem ortam değişkenlerini yapılandırın.  
    
    a. **Sistem ve güvenlik** > **sistemi** > **Denetim Masası** ' na gidin ve **Gelişmiş sistem ayarları**' nı seçin.  
    
      ![Denetim Masası](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. *Sistem Özellikleri*' nin **Gelişmiş** sekmesinde **ortam değişkenleri**' ni seçin.  

    c. *Sistem değişkenleri*Için, **yolu** düzenleyin ve **C:\\program dosyalarının Microsoft Service Fabric\\bin\\Fabric\\Fabric\\** olduğundan emin olun. kod, ortam değişkenleri listesinin en üstünde yer alan.  

      ![Ortam değişkeni listesi](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

1. Ortam değişkenlerinin sırasını değiştirdikten sonra PowerShell 'i yeniden başlatın ve ardından Service Fabric kümesine erişim kazanmak için aşağıdaki PowerShell betiğini çalıştırın:

   ```powershell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ```
   
   > [!NOTE]  
   > Betikte küme adından önce *https://* yok. 19000 numaralı bağlantı noktası gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes 'i Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)
