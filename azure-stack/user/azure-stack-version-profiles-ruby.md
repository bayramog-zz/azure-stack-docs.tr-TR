---
title: Azure Stack 'de Ruby ile API sürüm profillerini kullanma | Microsoft Docs
description: Azure Stack 'de Ruby ile API sürüm profillerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: c0faaa7be69ad8d23dc94eec1107362a7a7eadfa
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959335"
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Ruby ile API sürüm profillerini kullanın Azure Stack

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="ruby-and-api-version-profiles"></a>Ruby ve API sürüm profilleri

Azure Stack Kaynak Yöneticisi için Ruby SDK, altyapınızı oluşturmanıza ve yönetmenize yardımcı olacak araçlar sağlar. SDK 'daki kaynak sağlayıcıları, Ruby dili ile Işlem, sanal ağlar ve depolama alanı içerir. Ruby SDK 'daki API profilleri, Azure Stack Genel Azure kaynakları ve kaynakları arasında geçiş yapmanıza yardımcı olarak karma bulut geliştirmeyi sağlar.

Bir API profili, kaynak sağlayıcılarının ve hizmet sürümlerinin bir birleşimidir. Farklı kaynak türlerini birleştirmek için bir API profili kullanabilirsiniz.

- Tüm hizmetlerin en son sürümlerini kullanmak için, Azure SDK toplamasının GED 'nin **en son** profilini kullanın.
- Azure Stack ile uyumlu hizmetleri kullanmak için, Azure SDK toplamasının geI 'nin **V2019_03_01_Hybrid** veya **V2018_03_01** profilini kullanın.
- Bir hizmetin en son **api sürümünü** kullanmak için, belirli bir GED profilini kullanın. Örneğin, tek başına işlem hizmeti 'nin en son **api sürümünü** kullanmak Için, **işlem** GED 'nin **en son** profilini kullanın.
- Bir hizmet için belirli bir **API-sürümü** kullanmak için, GED içinde tanımlanan belirli API sürümlerini kullanın.

> [!NOTE]
> Aynı uygulamadaki tüm seçenekleri birleştirebilirsiniz.

## <a name="install-the-azure-ruby-sdk"></a>Azure Ruby SDK 'sını yükler

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)'i yüklemek için resmi yönergeleri izleyin.
- [Ruby](https://www.ruby-lang.org/en/documentation/installation/)'yi yüklemek için resmi yönergeleri izleyin.
  - Yüklenirken Ruby ' yi **yol değişkenine Ekle**' yi seçin.
  - İstendiğinde, Ruby yüklemesi sırasında geliştirme setini yükleme.
  - Sonra, aşağıdaki komutu kullanarak paketcisi 'ni yüklersiniz:  
    `Gem install bundler`
- Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Abonelik oluşturmaya yönelik yönergeler [burada](../operator/azure-stack-subscribe-plan-provision-vm.md)verilmiştir.
- Bir hizmet sorumlusu oluşturun ve KIMLIĞINI ve parolasını kaydedin. Azure Stack için bir hizmet sorumlusu oluşturma yönergeleri [burada](../operator/azure-stack-create-service-principals.md)verilmiştir.
- Hizmet sorumlunuz, aboneliğiniz üzerinde katkıda bulunan/Owner rolüne sahip olduğundan emin olun. Hizmet sorumlusuna rol atama yönergeleri [burada](../operator/azure-stack-create-service-principals.md)verilmiştir.

## <a name="install-the-rubygem-packages"></a>Rubyged paketlerini yükler

Azure Rubyged paketlerini doğrudan yükleyebilirsiniz.

```Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
```

Azure Resource Manager Ruby SDK 'Sı önizlemededir ve yaklaşan sürümlerde büyük olasılıkla arabirim değişiklikleri olacaktır. Küçük sürümde artan bir sayı, son değişiklikleri gösterebilir.

## <a name="use-the-azure_sdk-gem"></a>Azure_sdk GED 'yi kullanma

**Azure_sdk** gem, Ruby SDK 'daki tüm desteklenen Gems 'lerin bir toplamasıyla sonuçlanır. Bu ged, tüm hizmetlerin en son sürümünü destekleyen **en son** bir profilden oluşur. Azure Stack için oluşturulan sürümlenmiş profiller **V2017_03_09** ve **V2019_03_01_Hybrid**içerir.

Azure_sdk ROLLUP GED 'yi aşağıdaki komutla yükleyebilirsiniz:  

```Ruby  
gem install 'azure_sdk'
```

## <a name="prerequisites"></a>Önkoşullar

Ruby Azure SDK 'sını Azure Stack ile kullanmak için aşağıdaki değerleri belirtmeniz ve ardından değerleri ortam değişkenleriyle ayarlamanız gerekir. Ortam değişkenlerini ayarlamak için, belirli işletim sisteminize ait tabloyu izleyen yönergelere bakın.

| Value | Ortam değişkenleri | Açıklama |
| --- | --- | --- |
| Kiracı Kimliği | `AZURE_TENANT_ID` | Azure Stack [KIRACı kimliğinizin](../operator/azure-stack-identity-overview.md)değeri. |
| İstemci Kimliği | `AZURE_CLIENT_ID` | Hizmet sorumlusu bu belgenin önceki bölümünde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI.  |
| Abonelik Kimliği | `AZURE_SUBSCRIPTION_ID` | [ABONELIK kimliği](../operator/azure-stack-plan-offer-quota-overview.md#subscriptions) , Azure Stack tekliflere nasıl erişirsiniz. |
| İstemci Gizli Anahtarı | `AZURE_CLIENT_SECRET` | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı. |
| Resource Manager uç noktası | `ARM_ENDPOINT` | [Azure Stack Kaynak Yöneticisi uç noktasına](#the-azure-stack-resource-manager-endpoint)bakın.  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Microsoft Azure Kaynak Yöneticisi, yöneticilerin Azure kaynaklarını dağıtmaları, yönetmesi ve izlemesine olanak tanıyan bir yönetim çerçevesidir. Azure Resource Manager, bir grup olarak yerine tek tek bir işlemde bu görevleri işleyebilir.

Meta veri bilgilerini Kaynak Yöneticisi uç noktasından alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri içeren bir JSON dosyası döndürür.

 > [!NOTE]  
 > Azure Stack Geliştirme Seti (ASDK) içindeki **Resourcemanagerurl 'si** : `https://management.local.azurestack.external/`Tümleşik sistemlerdeki **Resourcemanagerurl 'si** :`https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
 > Gerekli meta verileri almak için:`<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
 Örnek JSON dosyası:

 ```json
 {
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
   "graphEndpoint": "https://graph.windows.net/",  
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": {
     "loginEndpoint": "https://login.windows.net/",
     "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
   }
 }
```

### <a name="set-environmental-variables"></a>Ortam değişkenlerini ayarlama

**Microsoft Windows**  
Ortam değişkenlerini ayarlamak için bir Windows komut isteminde aşağıdaki biçimi kullanın:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**macOS, Linux ve UNIX tabanlı sistemler** <br>
UNIX tabanlı sistemlerde aşağıdaki komutu kullanın:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Mevcut API profilleri

Azure_sdk ROLLUP GED aşağıdaki üç profil içerir:

1. **V2019_03_01_Hybrid** <br>
  Azure Stack için oluşturulan profil. Azure Stack damga sürüm 1904 veya üzeri sürümlerde sunulan hizmetlerin tüm en son sürümlerini kullanmak için bu profili kullanın.
1. **V2017_03_09**  
  Azure Stack için oluşturulan profil. Hizmetler için bu profili, Azure Stack damga sürümü 1808 veya önceki bir sürümüyle en uyumlu olacak şekilde kullanın.
1. **Sürümü**  
  Profil, tüm hizmetlerin en son sürümlerinden oluşur. Tüm hizmetlerin en son sürümlerini kullanın.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API profillerinin Özeti](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Azure Ruby SDK API profili kullanımı

Bir profil istemcisini başlatmak için aşağıdaki kodu kullanın. Bu parametre yalnızca Azure Stack veya diğer özel bulut için gereklidir. Küresel Azure, bu ayarlar varsayılan olarak zaten sahiptir.

```Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
  ENV['AZURE_TENANT_ID'],
  ENV['AZURE_CLIENT_ID'],
  ENV['AZURE_CLIENT_SECRET'],
  active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
  credentials: credentials,
  subscription_id: subscription_id,
  active_directory_settings: active_directory_settings,
  base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2019_03_01_Hybrid::Mgmt::Client.new(options)
```

Profil istemcisi, Işlem, depolama ve ağ gibi ayrı kaynak sağlayıcılarına erişmek için kullanılabilir:

```Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2019_03_01_Hybrid::Compute::Mgmt::Models::PurchasePlan.new
```

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack ortamı ayarı işlevleri tanımlayın

Azure Stack ortamında hizmet sorumlusunun kimliğini doğrulamak için, kullanarak `get_active_directory_settings()`uç noktaları tanımlayın. Bu yöntem, ortam değişkenlerinizi oluştururken ayarladığınız **ARM_Endpoint** ortam değişkenini kullanır:

```Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
  settings = MsRestAzure::ActiveDirectoryServiceSettings.new
  response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
  status_code = response.code
  response_content = response.body
  unless status_code == "200"
    error_model = JSON.load(response_content)
    fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
  end
  result = JSON.load(response_content)
  settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
  settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
  settings
end
```

## <a name="samples-using-api-profiles"></a>API profillerini kullanan örnekler

Ruby ve Azure Stack API profilleriyle çözüm oluşturmak için GitHub 'da bulunan aşağıdaki örnekleri kullanın:

- [Ruby Ile Azure kaynaklarını ve kaynak gruplarını yönetin](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups).
- [Ruby kullanarak sanal makineleri yönetme](https://github.com/Azure-Samples/Hybrid-Compute-Ruby-Manage-VM) (Azure Stack tarafından desteklenen en son API sürümlerini hedeflemek için 2019-03-01 karma profili kullanan örnek.
- [Ruby içindeki bir ŞABLONLA SSH özellikli BIR VM dağıtın](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Template-Deployment).

### <a name="sample-resource-manager-and-groups"></a>Örnek Kaynak Yöneticisi ve gruplar

Örneği çalıştırmak için Ruby 'yi yüklediğinizden emin olun. Visual Studio Code kullanıyorsanız, Ruby SDK uzantısını da indirin.

> [!NOTE]  
> "[Azure kaynaklarını ve kaynak gruplarını Ruby Ile yönetme](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups)" adresindeki örnek için depoyu alın.

1. Depoyu kopyalayın:

   ```bash
   git clone https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups.git
   ```

2. Demeti kullanarak bağımlılıkları yükler:

   ```Bash
   cd Hybrid-Resource-Manager-Ruby-Resources-And-Groups
   bundle install
   ```

3. PowerShell kullanarak bir Azure hizmet sorumlusu oluşturun ve gereken değerleri alın.

   Bir hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](../operator/azure-stack-create-service-principals.md).

   Gerekli değerler şunlardır:
   - Kiracı Kimliği
   - İstemci Kimliği
   - İstemci Gizli Anahtarı
   - Abonelik Kimliği
   - Resource Manager uç noktası

   Oluşturduğunuz hizmet sorumlusunun aldığınız bilgileri kullanarak aşağıdaki ortam değişkenlerini ayarlayın.

   - `export AZURE_TENANT_ID={your tenant ID}`
   - `export AZURE_CLIENT_ID={your client ID}`
   - `export AZURE_CLIENT_SECRET={your client secret}`
   - `export AZURE_SUBSCRIPTION_ID={your subscription ID}`
   - `export ARM_ENDPOINT={your Azure Stack Resource Manager URL}`

   > [!NOTE]  
   > Windows 'ta, dışa aktarma yerine set kullanın.

4. Konum değişkeninin Azure Stack konumunuza ayarlandığından emin olun; Örneğin, `LOCAL="local"`.

5. Doğru Active Directory uç noktalarını hedeflemek için Azure Stack veya diğer özel bulutlar kullanıyorsanız aşağıdaki kod satırını ekleyin:

   ```Ruby  
   active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
   ```

6. Seçenekler değişkeninde, Azure Stack çalışmak için Active Directory ayarlarını ve temel URL 'YI ekleyin:

   ```ruby  
   options = {
   credentials: credentials,
   subscription_id: subscription_id,
   active_directory_settings: active_directory_settings,
   base_url: ENV['ARM_ENDPOINT']
   }
   ```

7. Azure Stack profilini hedefleyen bir profil istemcisi oluşturun:

   ```ruby  
   client = Azure::Resources::Profiles::V2019_03_01_Hybrid::Mgmt::Client.new(options)
   ```

8. Azure Stack ile hizmet sorumlusunun kimliğini doğrulamak için uç noktaların **get_active_directory_settings ()** kullanılarak tanımlanması gerekir. Bu yöntem, ortam değişkenlerinizi oluştururken ayarladığınız **ARM_Endpoint** ortam değişkenini kullanır:

   ```ruby  
   def get_active_directory_settings(armEndpoint)
     settings = MsRestAzure::ActiveDirectoryServiceSettings.new
     response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
     status_code = response.code
     response_content = response.body
     unless status_code == "200"
       error_model = JSON.load(response_content)
       fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
     end
     result = JSON.load(response_content)
     settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
     settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
     settings
   end
   ```

9. Örnek uygulamayı çalıştırın.

   ```ruby
   bundle exec ruby example.rb
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
