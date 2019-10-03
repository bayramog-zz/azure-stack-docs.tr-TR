---
title: GO ile API sürüm profillerini kullanın Azure Stack | Microsoft Docs
description: Azure Stack GO ile API sürüm profillerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/26/2019
ms.openlocfilehash: 0636f3069db80613f02e979b5a102a471f12efad
ms.sourcegitcommit: 3d14ae30ce3ee44729e5419728cce14b3000e968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71814475"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>Go ile API sürüm profillerini kullanın Azure Stack

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="go-and-version-profiles"></a>Git ve sürüm profilleri

Profil, farklı hizmetlerden farklı sürümlere sahip farklı kaynak türlerinin bir birleşimidir. Profil kullanmak farklı kaynak türleri arasında karıştırmanıza ve eşleşmenize yardımcı olur. Profiller aşağıdaki avantajları sağlayabilir:

- Belirli API sürümlerine kilitleyerek uygulamanızın kararlılığı.
- Azure Stack ve bölgesel Azure veri merkezleriyle uygulamanız için uyumluluk.

Git SDK 'da profiller, profiller yolu altında bulunur. Profil sürüm numaraları **yyyy-aa-gg** biçiminde etiketlenir. En son Azure Stack API profili sürümü 1904 veya üzeri Azure Stack sürümleri için **2019-03-01** . Bir profilden belirli bir hizmeti içeri aktarmak için ilgili modülün profilini içeri aktarın. Örneğin, **2019-03-01** profilinden **işlem** hizmetini içeri aktarmak için aşağıdaki kodu kullanın:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/compute/mgmt/compute"
```

## <a name="install-the-azure-sdk-for-go"></a>Go için Azure SDK’yı yükleme

1. Git'i yükleyin. Yönergeler için [Git'i yükledikten Başlarken -](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2. [Go programlama dilini](https://golang.org/dl)yükler. Azure için API profilleri, go sürüm 1,9 veya daha yeni bir sürümü gerektirir.
3. Aşağıdaki Bash komutunu çalıştırarak Azure go SDK 'sını ve bağımlılıklarını yükler:

   ```bash
   go get -u -d github.com/Azure/azure-sdk-for-go/...
   ```

### <a name="the-go-sdk"></a>Go SDK 'Sı

Aşağıdaki bağlantılarda Azure go SDK hakkında daha fazla bilgi edinebilirsiniz:

- [Go için Azure SDK yüklenirken](/go/azure/azure-sdk-go-install)Azure go SDK 'sı.
- Azure go SDK, [Azure-SDK-for-go](https://github.com/Azure/azure-sdk-for-go) deposunda GitHub 'da herkese açık bir şekilde sunulmaktadır.

### <a name="go-autorest-dependencies"></a>Go-oto Rest bağımlılıkları

Go SDK 'Sı, REST isteklerini Azure Resource Manager uç noktalarına göndermek için Azure **Go-oto Rest** modüllerine bağımlıdır. Azure go **-oto Rest** modülü bağımlılıklarını [GitHub 'daki Azure go-oto Rest](https://github.com/Azure/go-autorest)modülünden içeri aktarmanız gerekir. Install Bash komutunu **Install** bölümüne bulabilirsiniz.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Azure Stack go SDK profillerini kullanma

Azure Stack bir go kodu örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Go için Azure SDK ve bağımlılıklarını yükler. Yönergeler için bkz. önceki bölüm, [Go için Azure SDK yüklemesi](#install-the-azure-sdk-for-go).
2. Kaynak Yöneticisi uç noktasından meta veri bilgilerini alın. Uç nokta, go kodunuzu çalıştırmak için gereken bilgileri içeren bir JSON dosyası döndürür.

   > [!NOTE]  
   > **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/`  
   > Tümleşik sistemlerdeki **Resourcemanagerurl** : `https://management.<region>.<fqdn>/`  
   > Gerekli meta verileri almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
   Örnek JSON dosyası:

   ```json
   { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
     "graphEndpoint": "https://graph.windows.net/",  
     "portal Endpoint": "https://portal.local.azurestack.external/",
     "authentication": {
       "loginEndpoint": "https://login.windows.net/",
       "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
     }
   }
   ```

3. Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Abonelik oluşturma hakkında bilgi için, bkz. [Azure Stack tekliflere abonelik oluşturma](../operator/azure-stack-subscribe-plan-provision-vm.md).

4. **Abonelik** kapsamı ve **sahip** rolü ile bir istemci gizli anahtarı kullanan bir hizmet sorumlusu oluşturun. Hizmet sorumlusu KIMLIĞINI ve gizli anahtarı kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında bilgi için bkz. [bir uygulama kimliğini kullanarak kaynaklara erişme](../operator/azure-stack-create-service-principals.md). Azure Stack ortamınız artık ayarlandı.

5. Kodunuzda go SDK profilinden bir hizmet modülünü içeri aktarın. Azure Stack profilinin geçerli sürümü **2019-03-01**' dir. Örneğin, **2019-03-01** profil türünden bir ağ modülünü içeri aktarmak için aşağıdaki kodu kullanın:

   ```go
   package main
    import "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/network/mgmt/network"
   ```

6. İşlevinizde, **Yeni** bir istemci işlevi çağrısıyla bir istemci oluşturun ve kimliğini doğrulayın. Bir sanal ağ istemcisi oluşturmak için aşağıdaki kodu kullanın:  

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/network/mgmt/network"

   func main() {
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
   ```

   @No__t-0 ' yı adım 2 ' de kullanılan **Resourcemanagerurl** değerine ayarlayın. @No__t-0 ' yı 3. adımda kaydedilen **SubscriptionID** değerine ayarlayın.

   Belirteci oluşturmak için bir sonraki bölüme bakın.  

7. Önceki adımda oluşturduğunuz istemciyi kullanarak API yöntemlerini çağırın. Örneğin, önceki adımdan istemcisini kullanarak bir sanal ağ oluşturmak için aşağıdaki örneğe bakın:

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/network/mgmt/network"
   func main() {
   vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
   vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

   vnetClient .CreateOrUpdate( )
   ```

Go SDK profilini kullanarak Azure Stack üzerinde sanal ağ oluşturma hakkında ayrıntılı bir örnek için, [örneğe](#example)bakın.

## <a name="authentication"></a>Authentication

Go SDK 'sını kullanarak Azure Active Directory **Authorizer** özelliğini almak için **Go-oto Rest** modüllerini kullanın. Bu modüller, "Go SDK" yüklemesiyle zaten yüklenmiş olmalıdır. Bu değillerse, [GitHub 'dan kimlik doğrulama paketini](https://github.com/Azure/go-autorest/tree/master/autorest/adal)yükler.

Yetkilendirici, kaynak istemcisi için yetkilendirici olarak ayarlanmalıdır. Azure Stack istemci kimlik bilgilerini kullanarak yetkilendirme belirteçleri almanın farklı yolları vardır:

1. Abonelik üzerinde sahip rolüne sahip bir hizmet sorumlusu varsa, bu adımı atlayın. Aksi takdirde, bir istemci gizli dizisi kullanan hizmet sorumlusu oluşturma hakkında yönergeler için bkz. [bir uygulama kimliği kullanma](../operator/azure-stack-create-service-principals.md) ve aboneliğinize kapsamlı bir "sahip" rolü atama hakkında yardım için. Hizmet sorumlusu uygulama KIMLIĞI ve gizli anahtarını yakaladığınızdan emin olun.

2. Kodunuzda, **Go-oto Rest** öğesinden **adal** paketini içeri aktarın.

   ```go
   package main
   import "github.com/Azure/go-autorest/autorest/adal"
   ```

3. **Adal** modülünden NewOAuthConfig metodunu kullanarak bir **oauthconfig** oluşturun.

   ```go
   package main

   import "github.com/Azure/go-autorest/autorest/ada1"

   func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
   }
   ```

   Bu belgenin önceki bölümünde alınan `ResourceManagerUrl` meta verilerinden `loginEndpoint` özelliğinin değerine `<activeDirectoryEndpoint>` değerini ayarlayın. @No__t-0 değerini Azure Stack kiracı KIMLIĞINIZ olarak ayarlayın.

4. Son olarak, **adal** modülünden `NewServicePrincipalToken` yöntemini kullanarak bir hizmet sorumlusu belirteci oluşturun:

   ```go
   package main

   import "github.com/Azure/go-autorest/autorest/adal"

   func CreateToken() (adal.OAuthTokenProvider, error) {
       var token adal.OAuthTokenProvider
       oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
       token, err = adal.NewServicePrincipalToken(
           *oauthConfig,
           clientID,
           clientSecret,
           activeDirectoryResourceID)
       return token, err
   ```

    Bu makalenin önceki bölümünde alınan **Resourcemanagerurl** meta verilerinden, `<activeDirectoryResourceID>` ' i "hedef kitle" listesindeki değerlerden birine ayarlayın.
    Bu makalenin önceki bölümünde hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞINE `<clientID>` olarak ayarlayın.
    Bu makalenin önceki bölümünde hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarını `<clientSecret>` olarak ayarlayın.

## <a name="example"></a>Örnek

Bu örnek, Azure Stack üzerinde bir sanal ağ oluşturan go kodunun bir örneğini gösterir. Go SDK 'sının tüm örnekleri için bkz. [Azure go SDK örnekleri deposu](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack örnekleri, deponun hizmet klasörlerinin içindeki karma yol altında bulunabilir.

> [!NOTE]  
> Bu örnekteki kodu çalıştırmak için, kullanılan aboneliğin, **kayıtlı**olarak listelenen **ağ** kaynak sağlayıcısı olduğunu doğrulayın. Doğrulamak için Azure Stack portalında aboneliği arayın ve **kaynak sağlayıcıları** ' nı seçin.

1. Kodunuzda gerekli paketleri içeri aktarın. Ağ modülünü içeri aktarmak için Azure Stack üzerinde bulunan en son profili kullanın:

   ```go
   package main

   import (
       "context"
       "fmt"
       "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/network/mgmt/network"
       "github.com/Azure/go-autorest/autorest"
       "github.com/Azure/go-autorest/autorest/adal"
       "github.com/Azure/go-autorest/autorest/to"
   )
   ```

2. Ortam değişkenlerinizi tanımlayın. Bir sanal ağ oluşturmak için bir kaynak grubunuz olması gerekir.

   ```go
   var (
       activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
       tenantID = "yourAzureStackTenantID"
       clientID = "yourServicePrincipalApplicationID"
       clientSecret = "yourServicePrincipalSecret"
       activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
       subscriptionID = "yourSubscriptionID"
       baseURI = "yourResourceManagerURL"
       resourceGroupName = "existingResourceGroupName"
   )
   ```

3. Ortam değişkenlerinizi tanımladığınıza göre, **adal** paketini kullanarak kimlik doğrulama belirteci oluşturmak için bir yöntem ekleyin. Kimlik doğrulama hakkında daha fazla bilgi için önceki bölüme bakın.

   ```go
   //CreateToken creates a service principal token
   func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
   }
   ```

4. `main` Yöntemini ekleyin. @No__t-0 yöntemi ilk olarak, önceki adımda tanımlanan yöntemi kullanarak bir belirteç alır. Daha sonra, profilinden bir ağ modülü kullanarak bir istemci oluşturur. Son olarak, bir sanal ağ oluşturur.

   ```go
   package main

   import (
      "context"
      "fmt"
      "github.com/Azure/azure-sdk-for-go/profiles/2019-03-01/network/mgmt/network"
      "github.com/Azure/go-autorest/autorest"
      "github.com/Azure/go-autorest/autorest/adal"
      "github.com/Azure/go-autorest/autorest/to"
   )

   var (
      activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
      tenantID = "yourAzureStackTenantID"
      clientID = "yourServicePrincipalApplicationID"
      clientSecret = "yourServicePrincipalSecret"
      activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
     subscriptionID = "yourSubscriptionID"
     baseURI = "yourResourceManagerURL"
     resourceGroupName = "existingResourceGroupName"
   )

   //CreateToken creates a service principal token
   func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
   }

   func main() {
      token, _ := CreateToken()
      vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
      future, _ := vnetClient.CreateOrUpdate(
          context.Background(),
          resourceGroupName,
          "sampleVnetName",
          network.VirtualNetwork{
              Location: to.StringPtr("local"),
              VirtualNetworkPropertiesFormat: &network.VirtualNetworkPropertiesFormat{
                  AddressSpace: &network.AddressSpace{
                      AddressPrefixes: &[]string{"10.0.0.0/8"},
                  },
                  Subnets: &[]network.Subnet{
                      {
                          Name: to.StringPtr("subnetName"),
                          SubnetPropertiesFormat: &network.SubnetPropertiesFormat{
                              AddressPrefix: to.StringPtr("10.0.0.0/16"),
                          },
                      },
                  },
              },
          })
      err := future.WaitForCompletionRef(context.Background(), vnetClient.Client)
      if err != nil {
          fmt.Printf(err.Error())
          return
      }
   }
   ```

Go SDK kullanılarak Azure Stack için kullanılabilir kod örneklerinden bazıları şunlardır:

- [Sanal makine oluştur](https://github.com/Azure-Samples/Hybrid-Compute-Go-Create-VM)
- [Depolama veri düzlemi](https://github.com/Azure-Samples/Hybrid-Storage-Go-Dataplane)
- [Yönetilen diskleri](https://github.com/Azure-Samples/Hybrid-Compute-Go-ManagedDisks) (Azure Stack tarafından desteklenen en son API sürümlerini hedefleyen 2019-03-01 profili kullanan bir örnek) kullanın

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
