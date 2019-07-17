---
title: Sağlayıcı kaynak kullanım API'si | Microsoft Docs
description: Başvuru için kaynak kullanım API'si, Azure Stack kullanım bilgilerini alır.
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
ms.openlocfilehash: 631d6764ca7947ddafd70ec57b607df1ea5a4ab5
ms.sourcegitcommit: 2a4cb9a21a6e0583aa8ade330dd849304df6ccb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286697"
---
# <a name="provider-resource-usage-api"></a>Sağlayıcı kaynak kullanım API’si

Terim *sağlayıcısı* Hizmet Yöneticisi ve herhangi bir sağlayıcı temsilcisi için geçerlidir. Azure Stack operatörleri ve sağlayıcı temsilcisi sağlayıcı kullanım API'si doğrudan kiracıları kullanımını görüntülemek için kullanabilirsiniz. Örneğin, aşağıdaki diyagramda gösterildiği gibi P1 ve P2 doğrudan kullanım bilgilerini almak için API sağlayıcısı P0 çağırabilir ve P1 kullanım bilgileri P3 ve P4 çağırabilirsiniz.

![Sağlayıcı hiyerarşinin kavramsal model](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>API çağrısı başvurusu

### <a name="request"></a>İstek

İstek tüketim ayrıntılarını ve istenen zaman çerçevesi için istenen abonelikleri alır. Hiçbir istek gövdesi yok.

Bu kullanım API'si çağıran atanmalıdır bir API sağlayıcısı olduğundan bir **sahibi**, **katkıda bulunan**, veya **okuyucu** sağlayıcının abonelik rolü.

| Yöntem | İstek URI'si |
| --- | --- |
| GET |`https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value}` |

### <a name="arguments"></a>Bağımsız Değişkenler

| Bağımsız Değişken | Açıklama |
| --- | --- |
| `armendpoint` |Azure Stack ortamınıza Azure Resource Manager uç noktası. Azure Resource Manager uç nokta adı biçiminde olduğunu Azure Stack kuraldır `https://adminmanagement.{domain-name}`. Etki alanı adıdır, örneğin, Geliştirme Seti için *local.azurestack.external*, Resource Manager uç noktasını ise `https://adminmanagement.local.azurestack.external`. |
| `subId` |Çağrıyı yapan kullanıcının abonelik kimliği. |
| `reportedStartTime` |Başlangıç saati sorgu. Değeri `DateTime` (UTC) Eşgüdümlü Evrensel Saat ve saat; başında olmalıdır. Örneğin, 13:00. Günlük toplama için UTC gece yarısı ile bu değeri ayarlayın. Biçim kaçırılmışsa ISO 8601; Örneğin, `2015-06-16T18%3a53%3a11%2b00%3a00Z`, iki nokta üst üste için kaçış burada `%3a` ve artı için kaçış `%2b` URI dostu olmasını sağlayın. |
| `reportedEndTime` |Sorgu bitiş saati. Uygulanan kısıtlamaları `reportedStartTime` bu bağımsız değişken için de geçerlidir. Değeri `reportedEndTime` gelecekte ya da geçerli tarih olamaz. İse, sonuç "tam işleme yok." ayarlanır |
| `aggregationGranularity` |İki ayrı olası değerlere sahip isteğe bağlı bir parametre: **günlük** ve **saatlik**. Değerleri Öner gibi günlük ayrıntı düzeyi, verileri döndürür ve diğeri ise saatlik bir çözüm. **Günlük** varsayılan seçenektir. |
| `subscriberId` |Abonelik kimliği Filtrelenmiş veri almak için sağlayıcının doğrudan bir kiracının abonelik kimliği gereklidir. Abonelik kimliği parametre belirtilmediğinde arama sağlayıcısı'nın doğrudan kiracılar için kullanım verilerini döndürür. |
| `api-version` |Bu isteği yapmak için kullanılan protokol sürümü. Bu değeri şuna ayarlı `2015-06-01-preview`. |
| `continuationToken` |Belirteç sağlayıcı kullanım API'si son çağrısından alınan. Bir yanıt 1.000 satır büyük olduğunda bu belirteci gereklidir. İlerleme durumu için bir yer işareti olarak görev yapar. Belirteç mevcut değilse, verileri günün başlangıcından itibaren alınır veya saat ayrıntı düzeyi üzerinde göre geçirilen. |

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

### <a name="response-details"></a>Yanıt Ayrıntıları

| Bağımsız Değişken | Açıklama |
| --- | --- |
|`id` |Kullanım toplama benzersiz kimliği. |
|`name` |Kullanım toplama adı. |
|`type` |Kaynak tanımı. |
|`subscriptionId` |Azure Stack kullanıcı abonelik tanımlayıcısı. |
|`usageStartTime`|UTC başlangıç zamanı, bu kullanım toplama ait olduğu kullanım demeti.|
|`usageEndTime`|Bu kullanım toplama ait olduğu kullanım demeti bitiş saati UTC. |
|`instanceData` |Örnek ayrıntıları (yeni biçimde) anahtar-değer çiftleri:<br> `resourceUri`: Kaynak grupları ve örnek adını içeren tam kaynak kimliği. <br> `location`: Bu hizmetin çalıştırıldığı bölge. <br> `tags`: Kullanıcı tarafından belirtilen kaynak etiketleri. <br> `additionalInfo`: Tüketilen kaynak hakkında daha fazla ayrıntı; Örneğin, işletim sistemi sürümü veya görüntü türü. |
|`quantity`|Bu zaman çerçevesinde gerçekleşen kaynak tüketimi miktarı. |
|`meterId` |Tüketilen kaynak için benzersiz kimlik (olarak da adlandırılan `ResourceID`). |

## <a name="retrieve-usage-information"></a>Kullanım bilgilerini alma

### <a name="powershell"></a>PowerShell

Kullanım verilerini oluşturmak için çalışan ve sistem etkin olarak kullanan kaynaklar olmalıdır; Örneğin, etkin bir sanal makineye veya bazı veriler içeren bir depolama hesabı. Dikey emin olmak için izleme olup Azure Stack Market'te çalıştıran herhangi bir kaynağa sahip, bir sanal makine (VM) dağıtma ve VM doğrulayın konusunda emin değilseniz çalıştığı. Kullanım verilerini görüntülemek için aşağıdaki PowerShell cmdlet'lerini kullanın:

1. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
2. [Azure Stack kullanıcısı yapılandırma](../user/azure-stack-powershell-configure-user.md) veya [Azure Stack operatörü](azure-stack-powershell-configure-admin.md) PowerShell ortamı.
3. Kullanım verilerini almak için arama [Get-AzsSubscriberUsage](/powershell/module/azs.commerce.admin/get-azssubscriberusage) PowerShell cmdlet:

   ```powershell
   Get-AzsSubscriberUsage -ReportedStartTime "2017-09-06T00:00:00Z" -ReportedEndTime "2017-09-07T00:00:00Z"
   ```

### <a name="rest-api"></a>REST API

Microsoft.Commerce.Admin hizmet çağırarak silinen abonelikler için kullanım bilgilerini toplayabilirsiniz.

#### <a name="return-all-tenant-usage-for-deleted-for-active-users"></a>İade için tüm Kiracı kullanımı için etkin kullanıcı silindi

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&api-version=2015-06-01-preview` |

#### <a name="return-usage-for-deleted-or-active-tenant"></a>Silinmiş veya etkin Kiracı dönüş kullanımı

| Yöntem | İstek URI'si |
| --- | --- |
| GET |`https://{armendpoint}/subscriptions/{subId}/providersMicrosoft.Commerce.Admin/subscriberUsageAggregates?reportedStartTime={start-time}&reportedEndTime={end-endtime}&aggregationGranularity=Hourly&subscriberId={subscriber-id}&api-version=2015-06-01-preview` |

## <a name="next-steps"></a>Sonraki adımlar

- [Kiracı kaynak kullanım API'si başvurusu](azure-stack-tenant-resource-usage-api.md)
- [Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)
