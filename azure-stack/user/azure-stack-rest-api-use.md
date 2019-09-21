---
title: Azure Stack için API istekleri yap | Microsoft Docs
description: Azure Stack için API istekleri yapmak üzere Azure 'dan kimlik doğrulaması alma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 0be1e7832d5ac32b092e44674b78c59552af351c
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159728"
---
<!--  cblackuk and charliejllewellyn. This is a community contribution by cblackuk-->

# <a name="make-api-requests-to-azure-stack"></a>Azure Stack için API istekleri yapın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack bulutuna bir sanal makine (VM) ekleme gibi işlemleri otomatik hale getirmek için uygulama programlama arabirimini (API) kullanabilirsiniz.

API, istemcinizin Microsoft Azure oturum açma uç noktasında kimlik doğrulamasını gerektirir. Uç noktası, Azure Stack API 'sine gönderilen her isteğin üstbilgisinde kullanılacak bir belirteç döndürür. Microsoft Azure OAuth 2,0 kullanır.

Bu makalede, Azure Stack istekleri oluşturmak için **kıvrımlı** yardımcı programını kullanan örnekler sağlanmaktadır. Kıvrımlı, verileri aktarmaya yönelik bir kitaplığı içeren bir komut satırı aracıdır. Bu örnekler, Azure Stack API 'sine erişmek için belirteç alma sürecini ele alır. Çoğu programlama dili, güçlü belirteç yönetimine sahip OAuth 2,0 kitaplıklarını sağlar ve belirteci yenileme gibi görevleri işler.

Temel alınan istekleri ve yanıt yükünde neler beklediğinizi anlamanıza yardımcı olmak için, Azure Stack REST API kullanarak, bir genel REST istemcisiyle, sözgelimi bir **bütün olarak '** ı inceleyin.

Bu makalede, etkileşimli oturum açma veya adanmış uygulama kimlikleri oluşturma gibi belirteçleri almak için kullanılabilen tüm seçenekler araştırılamaz. Bu konular hakkında bilgi almak için bkz. [Azure REST API başvurusu](https://docs.microsoft.com/rest/api/).

## <a name="get-a-token-from-azure"></a>Azure 'dan bir belirteç alın

Erişim belirteci almak için x-www-form-urlencoded içerik türünü kullanarak biçimlendirilen bir istek gövdesi oluşturun. İsteğinizi Azure REST kimlik doğrulaması ve oturum açma uç noktasına GÖNDERIN.

### <a name="uri"></a>URI

```bash  
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**KIRACı kimliği** :

 - Kiracı etki alanınız, örneğin`fabrikam.onmicrosoft.com`
 - Kiracı KIMLIĞINIZ, örneğin`8eaed023-2b34-4da1-9baa-8bc8c9d6a491`
 - Kiracıdan bağımsız anahtarlar için varsayılan değer:`common`

### <a name="post-body"></a>Gövde gönder

```bash  
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

Her bir değer için:

- **grant_type**:  
   Kullanacağınız kimlik doğrulama şemasının türü. Bu örnekte, değeri `password`.

- **kaynak**:  
   Belirtecin eriştiği kaynak. Azure Stack yönetim meta veri uç noktasını sorgulayarak kaynağı bulabilirsiniz. **İzleyiciler** bölümüne bakın.

- **Azure Stack yönetim uç noktası**:  
   ```
   https://management.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-01
   ```

  > [!NOTE]  
  > Kiracı API 'sine erişmeye çalışan bir yöneticsahipseniz kiracı uç noktasını kullandığınızdan emin olun. Örneğin, `https://adminmanagement.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-011`  

  Örneğin, uç nokta olarak Azure Stack Geliştirme Seti:

   ```bash
   curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
   ```

  Yanıt:

  ```
  {
  "galleryEndpoint":"https://adminportal.local.azurestack.external:30015/",
  "graphEndpoint":"https://graph.windows.net/",
  "portalEndpoint":"https://adminportal.local.azurestack.external/",
  "authentication":{
     "loginEndpoint":"https://login.windows.net/",
     "audiences":["https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"]
     }
  }
  ```

### <a name="example"></a>Örnek

  ```
  https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
  ```

  **client_id**

  Bu değer bir varsayılan değere sabit olarak kodlanmıştır:

  ```
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Belirli senaryolar için alternatif seçenekler mevcuttur:

  
  | Uygulama | Uygulama |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

  **Kullanıcı adı**

  Örneğin, Azure Stack Azure AD hesabı:

  ```
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

  **Parola**

  Azure AD yönetici parolasını Azure Stack.

### <a name="example"></a>Örnek

İstek:

```
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Yanıt:

```
{
  "token_type": "Bearer",
  "scope": "user_impersonation",
  "expires_in": "3599",
  "ext_expires_in": "0",
  "expires_on": "1512574780",
  "not_before": "1512570880",
  "resource": "https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155",
  "access_token": "eyJ0eXAiOi...truncated for readability..."
}
```

## <a name="api-queries"></a>API sorguları

Erişim belirtecinizi aldıktan sonra API isteklerinizin her birine bir üst bilgi olarak ekleyin. Üst bilgi olarak eklemek için, şu değere sahip bir üst bilgi **yetkilendirmesi** oluşturun: `Bearer <access token>`. Örneğin:

İstek:

```bash  
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Yanıt:

```bash  
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### <a name="url-structure-and-query-syntax"></a>URL yapısı ve sorgu söz dizimi

Genel istek URI 'SI, aşağıdakilerden oluşur:`{URI-scheme} :// {URI-host} / {resource-path} ? {query-string}`

- **URI şeması**:  
URI, isteği göndermek için kullanılan protokolü belirtir. Örneğin, `http` veya `https`.
- **URI ana bilgisayarı**:  
Konak, veya `graph.microsoft.com` `adminmanagement.local.azurestack.external`gibi REST hizmeti uç noktasının barındırıldığı sunucunun etki alanı adını veya IP adresini belirtir.
- **Kaynak yolu**:  
Yol, bu kaynakların seçimini belirlemede hizmet tarafından kullanılan birden çok parçayı içerebilen kaynak veya kaynak koleksiyonunu belirtir. Örneğin: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` uygulama koleksiyonu içindeki belirli bir uygulamanın sahipleri listesini sorgulamak için kullanılabilir.
- **Sorgu dizesi**:  
Dize, API sürümü veya kaynak seçim ölçütü gibi ek basit parametreler sağlar.

## <a name="azure-stack-request-uri-construct"></a>Azure Stack URI yapısını iste

```
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version}
```

### <a name="uri-syntax"></a>URI sözdizimi

```
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version}
```

### <a name="query-uri-example"></a>Sorgu URI 'SI örneği

```
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure yeniden deneme uç noktalarını kullanma hakkında daha fazla bilgi için bkz. [azure REST API başvurusu](https://docs.microsoft.com/rest/api/).
