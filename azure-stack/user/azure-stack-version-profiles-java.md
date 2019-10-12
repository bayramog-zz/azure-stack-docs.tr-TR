---
title: Azure Stack | için Java ile API sürüm profillerini kullanma | Microsoft Docs
description: Azure Stack ' de Java ile API sürüm profillerini nasıl kullanacağınızı öğrenin.
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
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 7d2a98656671a5e6f59d46054e074e45bb9d3c89
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282898"
---
# <a name="use-api-version-profiles-with-java-in-azure-stack"></a>Azure Stack 'de Java ile API sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Kaynak Yöneticisi için Java SDK, altyapınızı oluşturmanıza ve yönetmenize yardımcı olacak araçlar sağlar. SDK 'daki kaynak sağlayıcıları Işlem, ağ, depolama, uygulama hizmetleri ve [Key Vault](/azure/key-vault/key-vault-whatis)içerir.

Java SDK 'sı, **. Java** dosyasına doğru modülleri yükleyen **Pod. xml** dosyasına bağımlılıklar ekleyerek API profillerini içerir. Ancak, Azure profili olarak **2019-03-01 karma**veya **en son**gibi birden çok profil ekleyebilirsiniz. Bu bağımlılıkların kullanılması doğru modülü yükler, böylece kaynak türü oluşturduğunuzda, kullanmak istediğiniz profillerden hangi API sürümünü seçebilirsiniz. Bu, Azure Stack için en güncel API sürümleriyle geliştirme yaparken Azure 'da en son sürümleri kullanmanıza olanak sağlar.

Java SDK 'sını kullanmak, doğru bir karma bulut geliştirici deneyimine izin vermez. Java SDK 'daki API profilleri, Azure Stack Genel Azure kaynakları ve kaynakları arasında geçiş yapmanıza yardımcı olarak karma bulut geliştirmesini sağlar.

## <a name="java-and-api-version-profiles"></a>Java ve API sürüm profilleri

Bir API profili, kaynak sağlayıcılarının ve API sürümlerinin bir birleşimidir. Bir kaynak sağlayıcısı paketindeki her kaynak türünün en son, en kararlı sürümünü almak için bir API profili kullanın.

- Tüm hizmetlerin en son sürümlerini kullanmak için, bağımlılık olarak **en son** profili kullanın.

  - En son profili kullanmak için, bağımlılık **com. Microsoft. Azure**' tır.

  - Azure Stack ' de bulunan en son desteklenen hizmetleri kullanmak için **com. Microsoft. Azure. Profile @ no__t-12019 @ no__t-203 @ no__t-301 @ no__t-4hibrit** profilini kullanın.

    - Profil, açılan listeden sağ sınıfı seçerseniz (.NET ile yaptığınız gibi), bir bağımlılık olarak **Pod. xml** dosyasında belirtilir.

  - Bağımlılıklar şu şekilde görünür:

     ```xml
     <dependency>
     <groupId>com.microsoft.azure.profile_2019_03_01_hybrid</groupId>
     <artifactId>azure</artifactId>
     <version>1.0.0-beta</version>
     </dependency>
     ```

  - Belirli bir kaynak sağlayıcısındaki bir kaynak türü için belirli API sürümlerini kullanmak üzere IntelliSense aracılığıyla tanımlanan belirli API sürümlerini kullanın.

Aynı uygulamadaki tüm seçenekleri birleştirebilirsiniz.

## <a name="install-the-azure-java-sdk"></a>Azure Java SDK 'sını yükler

Java SDK 'sını yüklemek için şu adımları izleyin:

1. Git 'i yüklemek için resmi yönergeleri izleyin. Bkz. Başlarken [-Git yükleme](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

2. [Java SDK 'sını](https://zulu.org/download/) ve [Maven](https://maven.apache.org/)'yi yüklemek için yönergeleri izleyin. Doğru sürüm, Java geliştirici seti 'nin 8. sürümüdür. Apache Maven 'nin doğru sürümü 3,0 veya üzeri bir sürümdür. Hızlı başlangıcı tamamlayabilmeniz için, `JAVA_HOME` ortam değişkeni, Java Development Kit 'in yükleneceği konum olarak ayarlanmalıdır. Daha fazla bilgi için bkz. [Java ve Maven ile ilk işlevinizi oluşturma](/azure/azure-functions/functions-create-first-java-maven).

3. Doğru bağımlılık paketlerini yüklemek için, Java uygulamanızda **Pod. xml** dosyasını açın. Aşağıdaki kodda gösterildiği gibi bir bağımlılık ekleyin:

   ```xml  
   <dependency>
   <groupId>com.microsoft.azure.profile_2019_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <version>1.0.0-beta</version>
   </dependency>
   ```

4. Yüklenmesi gereken paket kümesi, kullanmak istediğiniz profil sürümüne bağlıdır. Profil sürümleri için paket adları şunlardır:

   - **com. Microsoft. Azure. Profile @ no__t-12019 @ no__t-203 @ no__t-301 @ no__t-4hibrit**
   - **com. Microsoft. Azure**
     - **sürümü**

5. Kullanılabilir değilse, abonelik oluşturun ve daha sonra kullanmak üzere abonelik KIMLIĞINI kaydedin. Abonelik oluşturma hakkında yönergeler için, bkz. [Azure Stack tekliflere abonelik oluşturma](../operator/azure-stack-subscribe-plan-provision-vm.md).

6. Bir hizmet sorumlusu oluşturun ve istemci KIMLIĞINI ve istemci parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz. [uygulamalar için Azure Stack erişimi sağlama](../operator/azure-stack-create-service-principals.md). İstemci KIMLIĞI, hizmet sorumlusu oluştururken uygulama KIMLIĞI olarak da bilinir.

7. Hizmet sorumlunun aboneliğinizde katkıda bulunan/Owner rolüne sahip olduğundan emin olun. Hizmet sorumlusuna rol atama hakkında yönergeler için bkz. [uygulama Azure Stack erişimi sağlama](../operator/azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Prerequisites

Azure Stack ile Azure Java SDK 'sını kullanmak için aşağıdaki değerleri belirtmeniz ve ardından değerleri ortam değişkenleriyle ayarlamanız gerekir. Ortam değişkenlerini ayarlamak için, işletim sisteminizin tablosunu izleyen yönergelere bakın.

| Değer                     | Ortam değişkenleri | Açıklama                                                                                                                                                                                                          |
| ------------------------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kiracı KIMLIĞI                 | `AZURE_TENANT_ID`            | Azure Stack [KIRACı kimliğiniz](../operator/azure-stack-identity-overview.md).                                                          |
| İstemci KIMLIĞI                 | `AZURE_CLIENT_ID`             | Hizmet sorumlusu önceki bölümde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI.                                                                                              |
| Abonelik Kimliği           | `AZURE_SUBSCRIPTION_ID`      | Azure Stack tekliflere erişmek için [ABONELIK kimliğini](../operator/service-plan-offer-subscription-overview.md#subscriptions) kullanırsınız.                |
| İstemci parolası             | `AZURE_CLIENT_SECRET`        | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı.                                                                                                                                   |
| Kaynak Yöneticisi uç noktası | `ARM_ENDPOINT`              | [Azure Stack Kaynak Yöneticisi uç noktası](../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint) makalesine bakın. |
| Konum                  | `RESOURCE_LOCATION`    | Azure Stack için **Yerel** .                                                                                                                                                                                                |

Azure Stack kiracı KIMLIĞINI bulmak için [buradaki](../operator/azure-stack-csp-ref-operations.md)yönergelere bakın. Ortam değişkenlerinizi ayarlamak için aşağıdaki bölümlerde bulunan yordamları kullanın:

### <a name="microsoft-windows"></a>Microsoft Windows

Windows komut isteminde ortam değişkenlerini ayarlamak için aşağıdaki biçimi kullanın:

```shell
Set AZURE_TENANT_ID=<Your_Tenant_ID>
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux ve UNIX tabanlı sistemler

UNIX tabanlı sistemlerde aşağıdaki komutu kullanın:

```shell
Export AZURE_TENANT_ID=<Your_Tenant_ID>
```

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Azure Stack tümleşik sistemlerle CA kök sertifikasına güvenmeniz gerekmez.

#### <a name="windows"></a>Windows

1. Azure Stack kendinden imzalı sertifikayı masaüstünüze dışarı aktarın.

1. Bir komut isteminde dizinini `%JAVA_HOME%\bin` olarak değiştirin.

1. Şu komutu çalıştırın:

   ```shell
   .\keytool.exe -importcert -noprompt -file <location of the exported certificate here> -alias root -keystore %JAVA_HOME%\lib\security\cacerts -trustcacerts -storepass changeit
   ```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack Kaynak Yöneticisi uç noktası

Azure Resource Manager, yöneticilerin Azure kaynaklarını dağıtmaları, yönetmesi ve izlemesine olanak tanıyan bir yönetim çerçevesidir. Azure Resource Manager, tek bir işlemde bu görevleri tek tek yerine bir grup olarak işleyebilir.

Meta veri bilgilerini Kaynak Yöneticisi uç noktasından alabilirsiniz. Uç nokta, kodunuzu çalıştırmak için gereken bilgileri içeren bir JSON dosyası döndürür.

Aşağıdaki noktalara dikkat edin:

- ASDK 'deki **Resourcemanagerurl** : `https://management.local.azurestack.external/` ' dir.

- Tümleşik sistemlerdeki **Resourcemanagerurl** : `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` ' dir.

Gerekli meta verileri almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`.

Örnek JSON dosyası:

```json
{
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication":
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Mevcut API profilleri

- **com. Microsoft. Azure. Profile @ no__t-12019 @ no__t-203 @ no__t-301 @ no__t-4hibrit**: Azure Stack için derlenmiş en son profil. Bu profili, 1904 veya sonraki bir sürümde yaptığınız sürece Azure Stack en uyumlu olacak şekilde kullanın.

- **com. Microsoft. Azure. Profile @ no__t-12018 @ no__t-203 @ no__t-301 @ no__t-4hibrit**: Azure Stack için oluşturulan profil. Hizmetler için bu profili, Azure Stack sürümleri 1808 veya sonraki bir sürümüyle uyumlu olacak şekilde kullanın.

- **com. Microsoft. Azure**: tüm hizmetlerin en son sürümlerinden oluşan profil. Tüm hizmetlerin en son sürümlerini kullanın.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [API profillerinin Özeti](../user/azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-java-sdk-api-profile-usage"></a>Azure Java SDK API profili kullanımı

Aşağıdaki kod Azure Stack için hizmet sorumlusunun kimliğini doğrular. Kiracı KIMLIĞI ve kimlik doğrulama tabanı kullanılarak Azure Stack özel bir belirteç oluşturur:

```java
AzureTokenCredentials credentials = new ApplicationTokenCredentials(client, tenant, key, AZURE_STACK)
                    .withDefaultSubscriptionID(subscriptionID);
Azure azureStack = Azure.configure()
                    .withLogLevel(com.microsoft.rest.LogLevel.BASIC)
                    .authenticate(credentials, credentials.defaultSubscriptionID());
```

Bu kod, Azure Stack için uygulamanızı başarıyla dağıtmak üzere API profili bağımlılıklarını kullanmanıza olanak sağlar.

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack ortamı ayarı işlevlerini tanımlama

Azure Stack bulutu doğru uç noktalara kaydetmek için aşağıdaki kodu kullanın:

```java
// Get Azure Stack cloud endpoints
final HashMap<String, String> settings = getActiveDirectorySettings(armEndpoint);

AzureEnvironment AZURE_STACK = new AzureEnvironment(new HashMap<String, String>() {
                {
                    put("managementEndpointUrl", settings.get("audience"));
                    put("resourceManagerEndpointUrl", armEndpoint);
                    put("galleryEndpointUrl", settings.get("galleryEndpoint"));
                    put("activeDirectoryEndpointUrl", settings.get("login_endpoint"));
                    put("activeDirectoryResourceID", settings.get("audience"));
                    put("activeDirectoryGraphResourceID", settings.get("graphEndpoint"));
                    put("storageEndpointSuffix", armEndpoint.substring(armEndpoint.indexOf('.')));
                    put("keyVaultDnsSuffix", ".vault" + armEndpoint.substring(armEndpoint.indexOf('.')));
                }
            });
```

Önceki koddaki `getActiveDirectorySettings` çağrısı, meta veri uç noktalarından uç noktaları alır. Oluşturulan çağrıdan ortam değişkenlerini belirtir:

```java
public static HashMap<String, String> getActiveDirectorySettings(String armEndpoint) {

    HashMap<String, String> adSettings = new HashMap<String, String>();
    try {

        // create HTTP Client
        HttpClient httpClient = HttpClientBuilder.create().build();

        // Create new getRequest with below mentioned URL
        HttpGet getRequest = new HttpGet(String.format("%s/metadata/endpoints?api-version=1.0",
                             armEndpoint));

        // Add additional header to getRequest which accepts application/xml data
        getRequest.addHeader("accept", "application/xml");

        // Execute request and catch response
        HttpResponse response = httpClient.execute(getRequest);

        // Check for HTTP response code: 200 = success
        if (response.getStatusLine().getStatusCode() != 200) {
            throw new RuntimeException("Failed : HTTP error code : " + response.getStatusLine().getStatusCode());
        }

        String responseStr = EntityUtils.toString(response.getEntity());
        JSONObject responseJson = new JSONObject(responseStr);
        adSettings.put("galleryEndpoint", responseJson.getString("galleryEndpoint"));
        JSONObject authentication = (JSONObject) responseJson.get("authentication");
        String audience = authentication.get("audiences").toString().split("\"")[1];
        adSettings.put("login_endpoint", authentication.getString("loginEndpoint"));
        adSettings.put("audience", audience);
        adSettings.put("graphEndpoint", responseJson.getString("graphEndpoint"));

    } catch (ClientProtocolException cpe) {
        cpe.printStackTrace();
        throw new RuntimeException(cpe);
    } catch (IOException ioe) {
        ioe.printStackTrace();
        throw new RuntimeException(ioe);
    }
    return adSettings;
}
```

## <a name="samples-using-api-profiles"></a>API profillerini kullanan örnekler

.NET ve Azure Stack API profilleriyle çözüm oluşturmak için aşağıdaki GitHub örneklerini başvuru olarak kullanın:

- [Kaynak gruplarını yönetme](https://github.com/Azure-Samples/Hybrid-resources-java-manage-resource-group)

- [Depolama hesaplarını yönetme](https://github.com/Azure-Samples/hybrid-storage-java-manage-storage-accounts)

- [Bir sanal makineyi yönetme](https://github.com/Azure-Samples/hybrid-compute-java-manage-vm) (2019-03-01-karma profille güncelleştirildi)

### <a name="sample-unit-test-project"></a>Örnek birim testi projesi

1. Aşağıdaki komutu kullanarak depoyu kopyalayın:

   ```shell
   git clone https://github.com/Azure-Samples/Hybrid-resources-java-manage-resource-group.git`
   ```

2. Bir Azure hizmet sorumlusu oluşturun ve aboneliğe erişmek için bir rol atayın. Hizmet sorumlusu oluşturma hakkında yönergeler için bkz. [Azure PowerShell kullanarak sertifikayla hizmet sorumlusu oluşturma](../operator/azure-stack-create-service-principals.md).

3. Aşağıdaki gerekli ortam değişkenlerini alın:

   - `AZURE_TENANT_ID`
   - `AZURE_CLIENT_ID`
   - `AZURE_CLIENT_SECRET`
   - `AZURE_SUBSCRIPTION_ID`
   - `ARM_ENDPOINT`
   - `RESOURCE_LOCATION`

4. Aşağıdaki ortam değişkenlerini, komut istemi kullanılarak oluşturduğunuz hizmet sorumlusunun aldığı bilgileri kullanarak ayarlayın:

   - `export AZURE_TENANT_ID={your tenant ID}`
   - `export AZURE_CLIENT_ID={your client ID}`
   - `export AZURE_CLIENT_SECRET={your client secret}`
   - `export AZURE_SUBSCRIPTION_ID={your subscription ID}`
   - `export ARM_ENDPOINT={your Azure Stack Resource Manager URL}`
   - `export RESOURCE_LOCATION={location of Azure Stack}`

   Windows 'ta, **dışa aktarma**yerine **set** kullanın.

5. Azure Resource Manager meta veri uç noktalarını almak için `getActiveDirectorySettings` işlevini kullanın.

    ```java
    // Get Azure Stack cloud endpoints
    final HashMap<String, String> settings = getActiveDirectorySettings(armEndpoint);
    ```

6. **Poz. xml** dosyasında, Azure Stack için **2019-03-01 karma** profilini kullanmak üzere aşağıdaki bağımlılığı ekleyin. Bu bağımlılık, Işlem, ağ, depolama, Key Vault ve uygulama Hizmetleri kaynak sağlayıcıları için bu profille ilişkili modülleri yüklüyor:

   ```xml
   <dependency>
   <groupId>com.microsoft.azure.profile_2019_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <vers1s.0.0-beta</version>
   </dependency>
   ```

7. Ortam değişkenlerini ayarlamak için açılmış olan komut isteminde aşağıdaki komutu girin:

   ```shell
   mvn clean compile exec:java
   ```

## <a name="next-steps"></a>Sonraki adımlar

API profilleri hakkında daha fazla bilgi için bkz.

- [Azure Stack sürüm profilleri](azure-stack-version-profiles.md)
- [Profiller tarafından desteklenen kaynak sağlayıcısı API sürümleri](azure-stack-profiles-azure-resource-manager-versions.md)
