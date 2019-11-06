---
title: Azure Stack | ' de Node. js ile API sürüm profilleri kullanma | Microsoft Docs
description: Azure Stack ' de Node. js ile API sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 07/30/2019
ms.openlocfilehash: 65ea0b4f6f7f7cb3769e83bf9052ac2953668e48
ms.sourcegitcommit: 20d1c0ab3892e9c4c71d5b039457f1e15b1c84c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73618222"
---
# <a name="use-api-version-profiles-with-nodejs-software-development-kit-sdk-in-azure-stack"></a>Azure Stack 'de Node. js yazılım geliştirme seti (SDK) ile API sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="nodejs-and-api-version-profiles"></a>Node. js ve API sürüm profilleri

Uygulamalarınız için altyapıyı oluşturmaya ve yönetmeye yardımcı olması için Node. js SDK 'sını kullanabilirsiniz. Node. js SDK 'daki API profilleri, genel Azure kaynakları ve Azure Stack kaynakları arasında geçiş yapmanıza olanak sağlayarak karma bulut çözümlerinizle ilgili yardım sağlar. Bir kez kodleyebilir ve ardından hem küresel Azure hem de Azure Stack hedefleyin. 

Bu makalede geliştirme aracınız olarak [Visual Studio Code](https://code.visualstudio.com/) kullanabilirsiniz. Visual Studio Code Node. js SDK 'sını ayıklayabilir ve uygulamayı çalıştırıp uygulamayı Azure Stack örneğinize gönderebilmesine izin verir. Visual Studio Code veya `node <nodefile.js>` komutunu çalıştıran bir Terminal penceresi aracılığıyla hata ayıklaması yapabilirsiniz.

## <a name="the-nodejs-sdk"></a>Node. js SDK 'Sı

Node. js SDK 'Sı Azure Stack Kaynak Yöneticisi araçları sağlar. SDK 'daki kaynak sağlayıcıları işlem, ağ, depolama, uygulama hizmetleri ve Keykasasını içerir. Node. js uygulamanıza yükleyebileceğiniz 10 kaynak sağlayıcısı istemci kitaplığı vardır. Uygulamanıza yönelik belleği iyileştirmek için **2018-03-01 karma** veya **2019-03-01 profili** için kullanacağınız kaynak sağlayıcısını da yükleyebilirsiniz. Her modül bir kaynak sağlayıcısından, ilgili API sürümünden ve API profilinden oluşur. 

Bir API profili, kaynak sağlayıcılarının ve API sürümlerinin bir birleşimidir. Bir kaynak sağlayıcısı paketindeki her kaynak türünün en son, en kararlı sürümünü almak için bir API profili kullanabilirsiniz.

  -   Tüm hizmetlerin en son sürümlerini kullanmak için paketlerin **en son** profilini kullanın.

  -   Azure Stack ile uyumlu hizmetleri kullanmak için **\@Azure/ARM-Resources-profile-karma-2019-03-01** veya **\@azure/ARM-Storage-profile-2019-03 -01-hibrit** kullanın

### <a name="packages-in-npm"></a>NPM 'deki paketler

Her kaynak sağlayıcısı kendi paketine sahiptir. Paketi [NPM kayıt defteri](https://www.npmjs.com/package/@azure/arm-storage-profile-2019-03-01-hybrid)edinebilirsiniz.

Aşağıdaki paketleri bulabilirsiniz:

| Kaynak sağlayıcısı | Paket |
| --- | --- |
| [App Service](https://www.npmjs.com/package/@azure/arm-appservice-profile-2019-03-01-hybrid) | @azure/arm-appservice-profile-2019-03-01-hybrid |
| [Azure Resource Manager abonelikler](https://www.npmjs.com/package/@azure/arm-subscriptions-profile-hybrid-2019-03-01) | @azure/arm-subscriptions-profile-hybrid-2019-03-01  |
| [Azure Resource Manager Ilkesi](https://www.npmjs.com/package/@azure/arm-policy-profile-hybrid-2019-03-01) | @azure/arm-policy-profile-hybrid-2019-03-01
| [DNS Azure Resource Manager](https://www.npmjs.com/package/@azure/arm-dns-profile-2019-03-01-hybrid) | @azure/arm-dns-profile-2019-03-01-hybrid  |
| [Yetkilendirme](https://www.npmjs.com/package/@azure/arm-authorization-profile-2019-03-01-hybrid) | @azure/arm-authorization-profile-2019-03-01-hybrid  |
| [İşlem](https://www.npmjs.com/package/@azure/arm-compute-profile-2019-03-01-hybrid) | @azure/arm-compute-profile-2019-03-01-hybrid |
| [Depolama](https://www.npmjs.com/package/@azure/arm-storage-profile-2019-03-01-hybrid) | @azure/arm-storage-profile-2019-03-01-hybrid |
| [Ağ](https://www.npmjs.com/package/@azure/arm-network-profile-2019-03-01-hybrid) | @azure/arm-network-profile-2019-03-01-hybrid |
| [Kaynaklar](https://www.npmjs.com/package/@azure/arm-resources-profile-hybrid-2019-03-01) | @azure/arm-resources-profile-hybrid-2019-03-01 |
 | [Keyvault](https://www.npmjs.com/package/@azure/arm-keyvault-profile-2019-03-01-hybrid) | @azure/arm-keyvault-profile-2019-03-01-hybrid |

Bir hizmetin en son API sürümünü kullanmak için, belirli istemci kitaplığının **en son** profilini kullanın. Örneğin, tek başına kaynak hizmeti 'nin en son API sürümünü kullanmak isterseniz, **kaynak yönetimi Istemci kitaplığının** `azure-arm-resource` profilini kullanın. leyebilir.

Kaynak sağlayıcının belirli API sürümleri için paket içinde tanımlanan belirli API sürümlerini kullanın.

  > [!Note]  
  > Aynı uygulamadaki tüm seçenekleri birleştirebilirsiniz.

## <a name="install-the-nodejs-sdk"></a>Node. js SDK 'sını yükler

1. Git 'i yükler. Yönergeler için bkz. Başlarken [-Git yükleme](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

2. [Node. js](https://nodejs.org/en/download/)' nin güncel sürümünü yükler veya yükseltin. Node. js [NPM](https://www.npmjs.com/) JavaScript paket yöneticisini da içerir.

3. [Visual Studio Code](https://code.visualstudio.com/) yükleyip yükseltin ve Visual Studio Code için [Node. js uzantısını](https://code.visualstudio.com/docs/nodejs/nodejs-debugging) yükler.

2.  Azure Stack Kaynak Yöneticisi için istemci paketlerini yükler. Daha fazla bilgi için bkz. [istemci kitaplıklarını yüklemek](https://www.npmjs.com/package/@azure/arm-keyvault-profile-2019-03-01-hybrid).

3.  Yüklenmesi gereken paketler, kullanmak istediğiniz profil sürümüne bağlıdır. [NPM bölümündeki paketlerde](#packages-in-npm) kaynak sağlayıcılarının bir listesini bulabilirsiniz.

4. NPM kullanarak kaynak sağlayıcısı istemci kitaplığını yükler. Komut satırından: `npm install <package-name>` ' ı çalıştırın. Örneğin, yetkilendirme kaynak sağlayıcısı kitaplığını yüklemek için `npm install @azure/arm-authorization-profile-2019-03-01-hybrid` ' ı çalıştırabilirsiniz.

5.  SDK 'Yı kullandığınızda abonelik oluşturun ve abonelik KIMLIĞINI bir yere göz önünde yapın. Yönergeler için bkz. [Azure Stack tekliflere abonelik oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm).

6.  Bir hizmet sorumlusu oluşturun ve istemci KIMLIĞINI ve istemci parolasını kaydedin. İstemci KIMLIĞI, hizmet sorumlusu oluştururken uygulama KIMLIĞI olarak da bilinir. Yönergeler için bkz. [Azure Stack uygulamalarına erişim sağlama](../operator/azure-stack-create-service-principals.md).

7.  Hizmet sorumlunun aboneliğinizde katkıda bulunan/Owner rolüne sahip olduğundan emin olun. Hizmet sorumlusuna rol atama hakkında yönergeler için bkz. [uygulama Azure Stack erişimi sağlama](../operator/azure-stack-create-service-principals.md).

### <a name="nodejs-prerequisites"></a>Node. js önkoşulları 

Node. js Azure SDK 'sını Azure Stack ile kullanmak için aşağıdaki değerleri belirtmeniz ve ardından değerleri ortam değişkenleriyle ayarlamanız gerekir. Ortam değişkenlerini ayarlamak için, işletim sisteminizin tablosunu izleyen yönergelere bakın.

| Değer | Ortam değişkenleri | Açıklama |
| --- | --- | --- |
| Kiracı Kimliği | KIRACı\_KIMLIĞI | Azure Stack [KIRACı kimliğinizin](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview)değeri. |
| İstemci Kimliği | ISTEMCI\_KIMLIĞI | Hizmet sorumlusu bu belgenin önceki bölümünde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI.  |
| Abonelik Kimliği | AZURE\_ABONELIĞI\_KIMLIĞI [ABONELIK kimliği](/azure-stack/operator/service-plan-offer-subscription-overview#subscriptions) Azure Stack tekliflere nasıl erişirsiniz.  |
| İstemci parolası | Uygulama\_gızlı dizisi | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı. |
| Kaynak Yöneticisi uç noktası | ARM\_uç noktası | [Azure Stack Kaynak Yöneticisi uç noktasına](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-ruby#the-azure-stack-resource-manager-endpoint)bakın. |

#### <a name="set-your-environmental-variables-for-nodejs"></a>Node. js için ortam değişkenlerinizi ayarlama

Ortam değişkenlerinizi ayarlamak için:

  - **Microsoft Windows**  

    Ortam değişkenlerini ayarlamak için bir Windows komut isteminde aşağıdaki biçimi kullanın:
      
      `set Tenant_ID=<Your_Tenant_ID>`

  - **macOS, Linux ve UNIX tabanlı sistemler**  

    Bir bash isteminden ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:

    `export Azure_Tenant_ID=<Your_Tenant_ID>`

**Azure Stack Kaynak Yöneticisi uç noktası**

Microsoft Azure Kaynak Yöneticisi, yöneticilerin Azure kaynaklarını dağıtmaları, yönetmesi ve izlemesine olanak tanıyan bir yönetim çerçevesidir. Azure Resource Manager, tek bir işlemde bu görevleri tek tek yerine bir grup olarak işleyebilir.

Meta veri bilgilerini Kaynak Yöneticisi uç noktasından alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri içeren bir JSON dosyası döndürür.

> [!Note]  
> Azure Stack Geliştirme Seti (ASDK) içindeki **Resourcemanagerurl** 'si: `https://management.local.azurestack.external` tümleşik sistemlerdeki **resourcemanagerurl 'si** , gereken meta verileri almak için `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com`: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

Örnek JSON dosyası:

```JSON  
{
    "galleryEndpoint": "https://portal.local.azurestack.external/",

    "graphEndpoint": "https://graph.windowsNode.js/",

    "portal Endpoint": "https://portal.local.azurestack.external/",

    "authentication": {

        "loginEndpoint": "https://login.windowsNode.js/",

        "audiences": ["https://management.<yourtenant>.onmicrosoft.com/"]

    }

}
```

### <a name="existing-api-profiles"></a>Mevcut API profilleri

-  **\@Azure/ARM-ResourceProvider-profile-2019-03 -01-hibrit**

    Azure Stack için derlenmiş en son profil. Bu profili, 1808 damgasında veya daha fazla olduğu sürece Azure Stack en uyumlu olacak şekilde kullanın.

-  **\@Azure-ARM-kaynak**

    Profil, tüm hizmetlerin en son sürümlerinden oluşur. Azure 'daki tüm hizmetlerin en son sürümlerini kullanın.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API profillerinin Özeti](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles#summary-of-api-profiles).

### <a name="azure-nodejs-sdk-api-profile-usage"></a>Azure Node. js SDK 'Sı profili kullanımı

Aşağıdaki satırlar bir profil istemcisi örneği oluşturmak için kullanılmalıdır. Bu parametre yalnızca Azure Stack veya diğer özel bulutlar için gereklidir. Global Azure, varsayılan olarak @azure-arm-resource veya @azure-arm-storage ile bu ayarlara sahiptir.

```Node.js  
var ResourceManagementClient = require('@azure/arm-resources-profile-hybrid-2019-03-01').ResourceManagementClient;

var StorageManagementClient = require('@azure/arm-storage-profile-2019-03-01-hybrid').StorageManagementClient;
````

Azure Stack üzerindeki hizmet sorumlusunun kimliğini doğrulamak için aşağıdaki kod gereklidir. Kiracı KIMLIĞI ve kimlik doğrulama tabanı tarafından Azure Stack özgü bir belirteç oluşturur.

```Node.js  
var clientId = process.env['AZURE_CLIENT_ID'];
var tenantId = process.env['AZURE_TENANT_ID']; //"adfs"
var secret = process.env['AZURE_CLIENT_SECRET'];
var subscriptionId = process.env['AZURE_SUBSCRIPTION_ID'];
var base_url = process.env['ARM_ENDPOINT'];
var resourceClient, storageClient;
```

Bu, uygulamanızı Azure Stack başarılı bir şekilde dağıtmak için API profili istemci kitaplığını kullanmanıza olanak sağlar.

Aşağıdaki kod parçacığı, Azure Stack örneğiniz için belirttiğiniz Azure Resource Manager uç noktasını kullanır ve Galeri uç noktası, grafik uç noktası, izleyiciler ve Portal uç noktası gibi yukarıda gösterilen verileri toplar.

```Node.js  
var map = {};
const fetchUrl = base_url + 'metadata/endpoints?api-version=1.0'
```

## <a name="environment-settings"></a>Ortam ayarları

Hizmet sorumlusu Azure Stack ortamda kimlik doğrulaması yapmak için aşağıdaki kodu kullanın: Bu kodu kullanmak ve ortam değişkenlerinizin komut isteminde ayarlanması, geliştirici için bu eşlemeyi otomatik olarak oluşturur.

```Node.js  
function main() {
  var initializePromise = initialize();
  initializePromise.then(function (result) {
    var userDetails = result;
    console.log("Initialized user details");
    // Use user details from here
    console.log(userDetails)
    map["name"] = "AzureStack"
    map["portalUrl"] = userDetails.portalEndpoint 
    map["resourceManagerEndpointUrl"] = base_url 
    map["galleryEndpointUrl"] = userDetails.galleryEndpoint 
    map["activeDirectoryEndpointUrl"] = userDetails.authentication.loginEndpoint.slice(0, userDetails.authentication.loginEndpoint.lastIndexOf("/") + 1) 
    map["activeDirectoryResourceId"] = userDetails.authentication.audiences[0] 
    map["activeDirectoryGraphResourceId"] = userDetails.graphEndpoint 
    map["storageEndpointSuffix"] = "." + base_url.substring(base_url.indexOf('.'))  
    map["keyVaultDnsSuffix"] = ".vault" + base_url.substring(base_url.indexOf('.')) 
    map["managementEndpointUrl"] = userDetails.authentication.audiences[0] 
    map["validateAuthority"] = "false"
    Environment.Environment.add(map);
```

## <a name="samples-using-api-profiles"></a>API profillerini kullanan örnekler

Node. js ve Azure Stack API profilleriyle çözüm oluşturmak için aşağıdaki örnekleri bir başvuru olarak kullanabilirsiniz. GitHub 'dan aşağıdaki depolardaki örnekleri alabilirsiniz:

- [Depolama düğümü kaynak sağlayıcısı Başlarken](https://github.com/sijuman/storage-node-resource-provider-getting-started)
- [İşlem düğümü yönetimi](https://github.com/sijuman/compute-node-manage-vm)
- [Resource-Manager düğüm kaynakları ve grupları](https://github.com/sijuman/resource-manager-node-resources-and-groups)

### <a name="sample-create-storage-account"></a>Örnek depolama hesabı oluştur 

1.  Depoyu kopyalayın.

    ```bash  
    git clone https://github.com/sijuman/storage-node-resource-provider-getting-started.git
    ```

2.  Bir Azure hizmet sorumlusu oluşturun ve aboneliğe erişmek için bir rol atayın. Yönergeler için bkz. [sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

3.  Aşağıdaki gerekli değerleri alın:
    - Kiracı Kimliği
    - İstemci Kimliği
    - Gizli anahtar
    - Azure abonelik KIMLIĞI
    - Azure Stack Kaynak Yöneticisi uç noktası

4.  Aşağıdaki ortam değişkenlerini, komut istemi kullanarak oluşturduğunuz hizmet sorumlusunun aldığınız bilgileri kullanarak ayarlayın:

    ```bash  
    export TENANT_ID=<your tenant id>
    export CLIENT_ID=<your client id>
    export APPLICATION_SECRET=<your client secret>K
    export AZURE_SUBSCRIPTION_ID=<your subscription id>
    export ARM_ENDPOINT=<your Azure Stack Resource manager URL>
    ```

    > [!Note]  
    > Windows 'ta, **dışa aktarma**yerine **set** kullanın.

5.  Örnek uygulamanın `index.js` dosyasını açın.

6.  Konum değişkenini Azure Stack konumunuz olarak ayarlayın. Örneğin, `LOCAL = "local"`.

7.  Azure Stack kimlik doğrulaması yapmanıza olanak sağlayacak kimlik bilgilerini ayarlayın. Bu kodun bu kısmı index. js dosyasında bu örneğe dahildir.

    ```Node.js  
    var clientId = process.env['CLIENT_ID'];
    var tenantId = process.env['TENANT_ID']; //"adfs"
    var secret = process.env['APPLICATION_SECRET'];
    var subscriptionId = process.env['AZURE_SUBSCRIPTION_ID'];
    var base_url = process.env['ARM_ENDPOINT'];
    var resourceClient, storageClient;
    ```

8.  Yukarıda belirlenen istemci kitaplıklarının bir birleşimini kullanarak doğru istemci kitaplıklarını ayarlamış olup olmadığını denetler. Bu örnekte aşağıdaki adımları kullandınız:

    ```Node.js
    var ResourceManagementClient = require('@azure/arm-resources-profile-hybrid-2019-03-01').ResourceManagementClient;
    var StorageManagementClient = require('@azure/arm-storage-profile-2019-03-01-hybrid').StorageManagementClient;
    ```

9.  [NPM modülleri aramasını](https://www.npmjs.com/package/@azure/arm-keyvault-profile-2019-03-01-hybrid)kullanarak, Işlem, ağ, depolama, anahtar kasası ve uygulama Hizmetleri kaynak sağlayıcıları için **2019-03-01 karma** 'i bulun ve bu profille ilişkili paketleri yüklemelisiniz.

    Bu, komut istemi açılarak, deponun kök klasörüne yönlendirerek ve kullanılan her kaynak sağlayıcısı için `npm install @azure/arm-keyvault-profile-2019-03-01-hybrid` çalıştırılarak yapılabilir.

10.  Tüm Node. js modüllerini yüklemek için komut isteminde `npm install` komutunu çalıştırın.

11.  Örnek uygulamayı çalıştırın.

        ```Node.js  
        node index.js
        ```

12.  İndex. js ' den sonra temizlemek için Temizleme betiğini çalıştırın.

        ```Node.js  
        Node cleanup.js <resourceGroupName> <storageAccountName>
        ```

13.  Örnek başarıyla tamamlandı. Örnek hakkında daha fazla bilgi için aşağıya bakın.

### <a name="what-does-indexjs-do"></a>İndex. js ne yapar?

Örnek yeni bir depolama hesabı oluşturur, abonelik veya kaynak grubundaki depolama hesaplarını listeler, depolama hesabı anahtarlarını listeler, depolama hesabı anahtarlarını yeniden oluşturur, depolama hesabı özelliklerini alır, depolama hesabı SKU 'sunu güncelleştirir ve denetimler depolama hesabı adı kullanılabilirliği.

Örnek, hizmet sorumlunuzu kullanarak oturum açar ve kimlik bilgilerinizi ve abonelik KIMLIĞINIZI kullanarak **ResourceManagementClient** ve **storagemanagementclient** nesneleri oluşturur.

Örnek daha sonra yeni depolama hesabını oluşturacağınız bir kaynak grubu oluşturur.

```Node.js  
function createResourceGroup(callback) {
  var groupParameters = { location: location, tags: { sampletag: 'sampleValue' } };
  console.log('\nCreating resource group: ' + resourceGroupName);
  return resourceClient.resourceGroups.createOrUpdate(resourceGroupName, groupParameters, callback);
}
```

### <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma

Ardından örnek, önceki adımda oluşturulan kaynak grubuyla ilişkili yeni bir depolama hesabı oluşturur.

Bu durumda, benzersizlik sağlamak için depolama hesabı adı rastgele olarak oluşturulur. Ancak, abonelikte aynı ada sahip bir hesap zaten varsa, yeni bir depolama hesabı oluşturma çağrısı başarılı olur.

```Node.js  
var createParameters = {
    location: location,
    sku: {
        name: accType,
    },
    kind: 'Storage',
    tags: {
        tag1: 'val1',
        tag2: 'val2'
    }
};


console.log('\\n--&gt;Creating storage account: ' + storageAccountName + ' with parameters:\\n' + util.inspect(createParameters));

return storageClient.storageAccounts.create(resourceGroupName, storageAccountName, createParameters, callback);
```

### <a name="list-storage-accounts-in-the-subscription-or-resource-group"></a>Abonelik veya kaynak grubundaki depolama hesaplarını listeleme

Örnek, belirli bir abonelikteki tüm depolama hesaplarını listeler:

```Node.js  
console.log('\\n--&gt;Listing storage accounts in the current subscription.');

return storageClient.storageAccounts.list(callback);

It also lists storage accounts in the resource group:

    console.log('\\n--&gt;Listing storage accounts in the resourceGroup : ' + resourceGroupName);

return storageClient.storageAccounts.listByResourceGroup(resourceGroupName, callback);
```

### <a name="read-and-regenerate-storage-account-keys"></a>Depolama hesabı anahtarlarını okuyun ve yeniden oluşturun

Örnek, yeni oluşturulan depolama hesabı ve kaynak grubu için depolama hesabı anahtarlarını listeler:

```Node.js  
console.log('\\n--&gt;Listing storage account keys for account: ' + storageAccountName);

return storageClient.storageAccounts.listKeys(resourceGroupName, storageAccountName, callback);
```

Ayrıca hesap erişim anahtarlarını yeniden oluşturur:

```Node.js  
console.log('\\n--&gt;Regenerating storage account keys for account: ' + storageAccountName);

return storageClient.storageAccounts.regenerateKey(resourceGroupName, storageAccountName, 'key1', callback);
```

### <a name="get-storage-account-properties"></a>Depolama hesabı özelliklerini al

Örnek, depolama hesabının özelliklerini okur:

```Node.js  
console.log('\\n--&gt;Getting info of storage account: ' + storageAccountName);

return storageClient.storageAccounts.getProperties(resourceGroupName, storageAccountName, callback);
```

### <a name="check-storage-account-name-availability"></a>Depolama hesabı adının kullanılabilirliğini denetle

Örnek, belirli bir depolama hesabı adının Azure 'da kullanılabilir olup olmadığını denetler:

```Node.js  
console.log('\\n--&gt;Checking if the storage account name : ' + storageAccountName + ' is available.');

return storageClient.storageAccounts.checkNameAvailability(storageAccountName, callback);
```

### <a name="delete-the-storage-account-and-resource-group"></a>Depolama hesabı ve kaynak grubunu silme

Temizlik. js çalıştırıldığında, örneğin oluşturduğu depolama hesabı silinir:

```Node.js  
console.log('\\nDeleting storage account : ' + storageAccountName);
return storageClient.storageAccounts.deleteMethod(resourceGroupName, storageAccountName, callback);
```

Ayrıca, örneğin oluşturulduğu kaynak grubunu da siler:

```Node.js  
console.log('\\nDeleting resource group: ' + resourceGroupName);

return resourceClient.resourceGroups.deleteMethod(resourceGroupName, callback);
```

## <a name="next-steps"></a>Sonraki adımlar

API profilleri hakkında daha fazla bilgi için bkz.

- [Azure Stack’de API sürümü profillerini yönetme](azure-stack-version-profiles.md)
- [Profiller tarafından desteklenen kaynak sağlayıcısı API sürümleri](azure-stack-profiles-azure-resource-manager-versions.md)
