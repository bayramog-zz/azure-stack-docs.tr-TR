---
title: Sağlayıcı kaynağı kullanım API 'SI | Microsoft Docs
titleSuffix: Azure Stack
description: Azure Stack kullanım bilgilerini alan kaynak kullanımı API 'sine yönelik başvuru.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/25/2018
ms.openlocfilehash: 75a4adca6d9265314c74cdebe642d43b8c2f11ef
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802395"
---
# <a name="provider-resource-usage-api"></a>Sağlayıcı kaynak kullanım API’si

*Sağlayıcı* terimi, hizmet yöneticisi ve tüm temsilci sağlayıcılar için geçerlidir. Azure Stack işleçleri ve Temsilcili sağlayıcılar, doğrudan kiracılarının kullanımını görüntülemek için sağlayıcı kullanım API 'sini kullanabilir. Örneğin, aşağıdaki diyagramda gösterildiği gibi, P0, P1 ve P2 üzerinde doğrudan kullanım bilgilerini almak için sağlayıcı API 'sini çağırabilir ve P1, P3 ve P4 üzerinde kullanım bilgilerini çağırabilir.

![Sağlayıcı hiyerarşisinin kavramsal modeli](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>API çağrısı başvurusu

### <a name="request"></a>İste

İstek, istenen abonelikler ve istenen zaman dilimi için tüketim ayrıntılarını alır. İstek gövdesi yok.

Bu kullanım API 'si bir sağlayıcı API 'sidir, bu nedenle çağıranın sağlayıcının aboneliğine bir **sahip**, **katkıda bulunan**veya **okuyucu** rolü atanması gerekir.

| Yöntem | İstek URI 'SI |
| --- | --- |
| GET |`https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value}` |

### <a name="arguments"></a>Bağımsız Değişkenler

| Bağımsız Değişken | Açıklama |
| --- | --- |
| `armendpoint` |Azure Stack ortamınızın uç noktası Azure Resource Manager. Azure Stack kuralı, Azure Resource Manager uç noktasının adının `https://adminmanagement.{domain-name}`biçimindedir. Örneğin, Azure Stack Geliştirme Seti (ASDK) için, etki alanı adı *yerel. azurestack. external*ise, Kaynak Yöneticisi uç noktası `https://adminmanagement.local.azurestack.external`. |
| `subId` |Çağrıyı yapan kullanıcının abonelik KIMLIĞI. |
| `reportedStartTime` |Sorgunun başlangıç saati. `DateTime` değeri Eşgüdümlü Evrensel Saat (UTC) ve saatin başında olmalıdır; Örneğin, 13:00. Günlük toplama için bu değeri UTC gece yarısı olarak ayarlayın. Biçim kaçışdır ISO 8601; Örneğin, `2015-06-16T18%3a53%3a11%2b00%3a00Z`, iki nokta üst üste `%3a`, ve Plus 'nin URI kullanımı kolay olması için `%2b` kaçışlarından oluşur. |
| `reportedEndTime` |Sorgunun bitiş saati. `reportedStartTime` için uygulanan kısıtlamalar bu bağımsız değişken için de geçerlidir. `reportedEndTime` değeri gelecekte ya da geçerli tarih olamaz. Bu ise, sonuç "işlem tamamlanamadı" olarak ayarlanır. |
| `aggregationGranularity` |İki ayrı olası değeri olan isteğe bağlı parametre: **günlük** ve **saatlik**. Değerler önereceği için, biri günlük ayrıntı düzeyi halinde verileri döndürür ve diğeri saatlik bir çözünürlüğlük olur. **Günlük** seçeneği varsayılandır. |
| `subscriberId` |Abonelik KIMLIĞI. Filtrelenmiş verileri almak için, sağlayıcının doğrudan kiracının abonelik KIMLIĞI gereklidir. Abonelik KIMLIĞI parametresi belirtilmemişse, çağrı tüm sağlayıcının doğrudan kiracılarının kullanım verilerini döndürür. |
| `api-version` |Bu isteği yapmak için kullanılan protokolün sürümü. Bu değer `2015-06-01-preview`olarak ayarlanır. |
| `continuationToken` |Son çağrıdan kullanım API sağlayıcısına alınan belirteç. Yanıt 1.000 satırdan fazla olduğunda bu belirteç gereklidir. İlerleme için bir yer işareti görevi görür. Belirteç yoksa, veri, geçirilen ayrıntı düzeyi temel alınarak günün veya saatin başından alınır. |

### <a name="response"></a>Yanıt

```http
GET
/subscriptions/sub1/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&subscriberId=sub1.1&api-version=1.0
```

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce.Admin/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce.Admin/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

. . .
```

### <a name="response-details"></a>Yanıt ayrıntıları

| Bağımsız Değişken | Açıklama |
| --- | --- |
|`id` |Kullanım toplamanın benzersiz KIMLIĞI. |
|`name` |Kullanım toplamanın adı. |
|`type` |Kaynak tanımı. |
|`subscriptionId` |Azure Stack kullanıcının abonelik tanımlayıcısı. |
|`usageStartTime`|Bu kullanım toplamasının ait olduğu kullanım demetini 'nin UTC başlangıç saati.|
|`usageEndTime`|Bu kullanım toplamasının ait olduğu kullanım demetini 'nin UTC bitiş saati. |
|`instanceData` |Örnek ayrıntılarının anahtar-değer çiftleri (yeni bir biçimde):<br> `resourceUri`: kaynak gruplarını ve örnek adını içeren tam kaynak KIMLIĞI. <br> `location`: Bu hizmetin çalıştırıldığı bölge. <br> `tags`: Kullanıcı tarafından belirtilen kaynak etiketleri. <br> `additionalInfo`: tüketilen kaynakla ilgili daha fazla ayrıntı; Örneğin, işletim sistemi sürümü veya görüntü türü. |
|`quantity`|Bu zaman çerçevesinde gerçekleşen kaynak tüketimi miktarı. |
|`meterId` |Tüketilen kaynak için benzersiz KIMLIK (`ResourceID`de denir). |

## <a name="retrieve-usage-information"></a>Kullanım bilgilerini al

### <a name="powershell"></a>PowerShell

Kullanım verilerini oluşturmak için, çalıştıran ve sistemi etkin bir şekilde kullanan kaynaklarınız olmalıdır; Örneğin, etkin bir sanal makine (VM) veya bazı verileri içeren bir depolama hesabıdır. Azure Stack marketi 'nde çalışan bir kaynağınız olup olmadığından emin değilseniz, bir VM dağıtın ve çalıştığından emin olmak için VM izleme dikey penceresini doğrulayın. Kullanım verilerini görüntülemek için aşağıdaki PowerShell cmdlet 'lerini kullanın:

1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).
2. [Azure Stack kullanıcısını](../user/azure-stack-powershell-configure-user.md) veya [Azure Stack Işleci](azure-stack-powershell-configure-admin.md) PowerShell ortamını yapılandırın.
3. Kullanım verilerini almak için [Get-AzsSubscriberUsage](/powershell/module/azs.commerce.admin/get-azssubscriberusage) PowerShell cmdlet 'ini çağırın:

   ```powershell
   Get-AzsSubscriberUsage -ReportedStartTime "2017-09-06T00:00:00Z" -ReportedEndTime "2017-09-07T00:00:00Z"
   ```

### <a name="rest-api"></a>REST API

Silinen abonelikler için kullanım bilgilerini Microsoft. Commerce. Admin hizmetini çağırarak toplayabilirsiniz.

#### <a name="return-all-tenant-usage-for-deleted-for-active-users"></a>Etkin kullanıcılar için silinen tüm Kiracı kullanımını döndür

| Yöntem | İstek URI 'SI |
| --- | --- |
| GET | `https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&api-version=2015-06-01-preview` |

#### <a name="return-usage-for-deleted-or-active-tenant"></a>Silinen veya etkin kiracı için kullanım dönüşü

| Yöntem | İstek URI 'SI |
| --- | --- |
| GET |`https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&subscriberId={subscriber-id}&api-version=2015-06-01-preview` |

## <a name="next-steps"></a>Sonraki adımlar

- [Kiracı kaynak kullanımı API başvurusu](azure-stack-tenant-resource-usage-api.md)
- [Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)
