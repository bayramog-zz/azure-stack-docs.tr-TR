---
title: Azure Stack 'de .NET ile API sürüm profillerini kullanın | Microsoft Docs
description: Azure Stack 'de .NET SDK ile API sürüm profillerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 14f19fa432d782eace721d47b6b578dc73846631
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959370"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>. NET'te Azure Stack ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

.NET SDK'sı için Azure Stack Kaynak Yöneticisi'ni oluşturmanıza ve altyapınızı yönetmenize yardımcı olacak araçlar sağlar. SDK 'daki kaynak sağlayıcıları Işlem, ağ, depolama, uygulama hizmetleri ve [Key Vault](/azure/key-vault/key-vault-whatis)içerir. .NET SDK 'Sı 14 NuGet paketi içerir. Projenizi her derlerken bu paketleri çözümünüze indirmeniz gerekir. Bununla birlikte, uygulamanız için belleği iyileştirmek üzere 2019-03-01 karma veya 2018-03-01-karma için kullanacağınız kaynak sağlayıcısını özellikle indirebilirsiniz. Her paket, kaynak sağlayıcısı, ilgili API sürümü ve ait olduğu API profili oluşur. .NET SDK'sı API profillerinde genel Azure kaynakları ve Azure Stack'te kaynakları arasında geçiş yardımcı olarak karma bulut geliştirmeyi etkinleştirin.

## <a name="net-and-api-version-profiles"></a>.NET ve API sürümü profillerini

Bir API profili, kaynak sağlayıcıları ve API sürümlerini birleşimidir. Bir kaynak sağlayıcısı paketindeki her kaynak türünün en son, en kararlı sürümünü almak için bir API profili kullanın.

-   Tüm hizmetler en son sürümlerini kullanın, yapmak **son** paketlerin profili. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

-   Azure Stack ile uyumlu hizmetleri kullanmak için aşağıdaki paketlerden birini kullanın:
    - **Microsoft. Azure. Management. Profiles. karma\_2019\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg** 
    - **Microsoft. Azure. Management. Profiles. karma\_2018\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg**
    
    Emin **ResourceProvider** bölümü yukarıdaki NuGet paketinin doğru sağlayıcıya değiştirilir.

-   Bir hizmetin en son API-version'ı kullanmak için **son** belirli NuGet paketinin profili. Örneğin, tek başına Işlem hizmeti 'nin **en son API** sürümünü kullanmak istiyorsanız, **işlem** paketinin **en son** profilini kullanın. **Son** profilidir parçası **Microsoft.Azure.Management** NuGet paketi.

-   Belirli bir kaynak sağlayıcısındaki bir kaynak türü için belirli API sürümlerini kullanmak için bir paket içinde tanımlanan belirli API sürümlerini kullanın.

Aynı uygulamadaki tüm seçenekleri birleştirebilirsiniz.

## <a name="install-the-azure-net-sdk"></a>Azure .NET SDK'sını yükleyin

1.  Git'i yükleyin. Yönergeler için [Başlarken - Git yükleme][].

2.  Doğru NuGet paketlerini yüklemek için bkz [bulma ve bir paket yükleme][].

3.  Yüklenmesi gereken paketler, kullanmak istediğiniz profil sürümüne bağlıdır. Profil sürümleri için paket adları şunlardır:

    1.  **Microsoft. Azure. Management. Profiles. karma\_2019\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg**

    2.  **Microsoft. Azure. Management. Profiles. karma\_2018\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg**

4.  Visual Studio Code için doğru NuGet paketlerini yüklemek için indirmek için aşağıdaki bağlantıya bakın [NuGet Paket Yöneticisi yönergeleri][].

5.  Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir abonelik oluşturmak yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma][].

6.  Hizmet sorumlusu oluşturma ve istemci Kimliğini ve istemci gizli anahtarını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için, bkz. [uygulama erişimi sağlamak için Azure Stack] []. Istemci KIMLIĞI, hizmet sorumlusu oluştururken uygulama KIMLIĞI olarak da bilinir.

7.  Hizmet sorumlunuzu aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusuna bir rol atamak yönergeler için bkz: [Azure Stack için uygulamalar erişim sağlayın][].

## <a name="prerequisites"></a>Önkoşullar

Azure .NET SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından ortam değişkenleriyle değerleri ayarlayın. Ortam değişkenlerini ayarlamak için, belirli işletim sisteminize ait tabloyu izleyen yönergelere bakın.

| Value                     | Ortam değişkenleri   | Açıklama                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kimliği                 | `AZURE_TENANT_ID `      | Azure Stack değerini [ *Kiracı kimliği*][].                                                                          |
| İstemci Kimliği                 | `AZURE_CLIENT_ID `      | Hizmet sorumlusu bu makalenin önceki bölümünde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI. |
| Abonelik Kimliği           | `AZURE_SUBSCRIPTION_ID` | [ *Abonelik kimliği* ][] nasıl, teklifler eriştiği Azure Stack'te.                                                      |
| İstemci Gizli Anahtarı             | `AZURE_CLIENT_SECRET`   | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı.                                      |
| Resource Manager uç noktası | `ARM_ENDPOINT`          | [*Azure Stack Kaynak Yöneticisi uç noktasına*][]bakın.                                                                    |
| Location                  | `RESOURCE_LOCATION`     | Azure Stack konumu.

Azure Stack için Kiracı Kimliğini bulmak için bulunan yönergeleri takip [burada](../operator/azure-stack-csp-ref-operations.md). Ortam değişkenlerinizi ayarlamak için aşağıdaki adımları uygulayın:

### <a name="microsoft-windows"></a>Microsoft Windows

Bir Windows Komut İstemi'nde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux ve UNIX tabanlı sistemlerde

UNIX tabanlı sistemlerde aşağıdaki komutu kullanın:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Microsoft Azure Kaynak Yöneticisi, yöneticilerin Azure kaynaklarını dağıtmaları, yönetmesi ve izlemesine olanak tanıyan bir yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.

Meta veri bilgilerini Kaynak Yöneticisi uç noktasından alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri içeren bir JSON dosyası döndürür.

Aşağıdaki konuları göz önünde bulundurun:

- **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): https://management.local.azurestack.external/

- Tümleşik sistemlerdeki **Resourcemanagerurl 'si** : `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`Gerekli meta verileri almak için:`<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

Örnek JSON dosyası:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Mevcut API profilleri

1.  **Microsoft. Azure. Management. Profiles. karma\_2019\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg**: Azure Stack için derlenmiş en son profil. Bu profili, 1904 damgasında veya sonraki sürümlerde olduğunuz sürece Azure Stack en uyumlu olacak şekilde kullanın.

2.  **Microsoft. Azure. Management. Profiles. karma\_2018\_03\_01. <*ResourceProvider*>. 0.9.0-Preview. nupkg**: Bu profili, damga sürümleri 1808 veya sonraki bir sürümü için Azure Stack uyumlu olacak şekilde kullanın.

3.  **En son**: Tüm hizmetlerin en son sürümlerinden oluşan profil. Tüm hizmetler en son sürümlerini kullanın. Bu profili parçasıdır **Microsoft.Azure.Management** NuGet paketi.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API profillerini özeti][].

## <a name="azure-net-sdk-api-profile-usage"></a>Azure .NET SDK API profili kullanımı

Aşağıdaki kod, bir kaynak Yönetimi istemcisinin örneğini oluşturmak için kullanılmalıdır. Benzer kod, diğer kaynak sağlayıcılarının (örneğin, Bilgi Işlem, ağ ve depolama) Istemciler için örnek oluşturmak için kullanılabilir.

```csharp
var client = new ResourceManagementClient(armEndpoint, credentials)
{
    SubscriptionId = subscriptionId
};
```

Yukarıdaki `credentials` koddaki parametre, bir istemcinin örneğini oluşturmak için gereklidir. Aşağıdaki kod, kiracı KIMLIĞI ve hizmet sorumlusu tarafından bir kimlik doğrulama belirteci oluşturur.

```csharp
var azureStackSettings = getActiveDirectoryServiceSettings(armEndpoint);
var credentials = ApplicationTokenProvider.LoginSilentAsync(tenantId, servicePrincipalId, servicePrincipalSecret, azureStackSettings).GetAwaiter().GetResult();
```
Koddaki `getActiveDirectoryServiceSettings` çağrı, meta veri uç noktasından Azure Stack uç noktaları alır. Oluşturulan çağrıdan ortam değişkenlerini belirtir:

```csharp
public static ActiveDirectoryServiceSettings getActiveDirectoryServiceSettings(string armEndpoint)
{
    var settings = new ActiveDirectoryServiceSettings();
    try
    {
        var request = (HttpWebRequest)HttpWebRequest.Create(string.Format("{0}/metadata/endpoints?api-version=1.0", armEndpoint));
        request.Method = "GET";
        request.UserAgent = ComponentName;
        request.Accept = "application/xml";
        using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
        {
            using (StreamReader sr = new StreamReader(response.GetResponseStream()))
            {
                var rawResponse = sr.ReadToEnd();
                var deserialized = JObject.Parse(rawResponse);
                var authenticationObj = deserialized.GetValue("authentication").Value<JObject>();
                var loginEndpoint = authenticationObj.GetValue("loginEndpoint").Value<string>();
                var audiencesObj = authenticationObj.GetValue("audiences").Value<JArray>();
                settings.AuthenticationEndpoint = new Uri(loginEndpoint);
                settings.TokenAudience = new Uri(audiencesObj[0].Value<string>());
                settings.ValidateAuthority = loginEndpoint.TrimEnd('/').EndsWith("/adfs", StringComparison.OrdinalIgnoreCase) ? false : true;
            }
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine(String.Format("Could not get AD service settings. Exception: {0}", ex.Message));
    }
    return settings;
}
```

Bu adımlar, Azure Stack için uygulamanızı başarıyla dağıtmak üzere API profili NuGet paketlerini kullanmanıza olanak sağlar.

## <a name="samples-using-api-profiles"></a>API profillerini kullanma örnekleri

.NET ve Azure Stack API profilleriyle çözüm oluşturmak için bir başvuru olarak aşağıdaki örnekleri kullanın.
- [Kaynak gruplarını yönetme](https://github.com/Azure-Samples/hybrid-resources-dotnet-manage-resource-group)
- [Depolama hesaplarını yönetme](https://github.com/Azure-Samples/hybird-storage-dotnet-manage-storage-accounts)
- [Bir sanal makineyi yönetme](https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm) (Bu örnek, Azure Stack tarafından desteklenen 2019-03-01 karma profilini kullanır)

## <a name="next-steps"></a>Sonraki adımlar

API profilleri hakkında daha fazla bilgi için bkz.

- [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md)
- [Profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri](azure-stack-profiles-azure-resource-manager-versions.md)

  [Başlarken - Git yükleme]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Bulma ve bir paket yükleme]: /nuget/tools/package-manager-ui
  [NuGet Paket Yöneticisi yönergeleri]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Azure Stack'te teklifleri abonelikleri oluşturma]: ../operator/azure-stack-subscribe-plan-provision-vm.md
  [Azure Stack için uygulamalar erişim sağlayın]: ../operator/azure-stack-create-service-principals.md
  [* Kiracı kimliği *]: ../operator/azure-stack-identity-overview.md
  [* Abonelik kimliği *]: ../operator/azure-stack-plan-offer-quota-overview.md#subscriptions
  [* Azure Stack Kaynak Yöneticisi uç noktası *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [API profillerini özeti]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Test Project to Virtual Machine, vNet, resource groups, and storage account]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Use Azure PowerShell to create a service principal with a certificate]: ../operator/azure-stack-create-service-principals.md
  [Run unit tests with Test Explorer.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
