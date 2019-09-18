---
title: Kiracı kaynak kullanımı API 'Leri | Microsoft Docs
description: Azure Stack kullanım bilgilerini alıp kaynak kullanımı API 'Lerine yönelik başvuru.
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
ms.date: 09/17/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 85bb518335c473a70ff97473d1b8b61654372cb8
ms.sourcegitcommit: 95f30e32e5441599790d39542ff02ba90e70f9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71070109"
---
# <a name="tenant-resource-usage-api"></a>Kiracı kaynak kullanım API’si

Kiracı, kiracının kendi kaynak kullanım verilerini görüntülemek için Kiracı API 'Lerini kullanabilir. Bu API 'Ler Azure kullanım API 'Leri ile tutarlıdır.

Azure 'a benzeyen kullanım verilerini almak için Windows PowerShell cmdlet [Get-Usagetoplamaları](/powershell/module/azurerm.usageaggregates/get-usageaggregates) ' nı kullanabilirsiniz.

## <a name="api-call"></a>API çağrısı

### <a name="request"></a>İstek

İstek, istenen abonelikler ve istenen zaman dilimi için tüketim ayrıntılarını alır. İstek gövdesi yok.

| **Yöntemi** | **İstek URI 'SI** |
| --- | --- |
| GET |https://{Ermenistan/{0}/abonelikler/{Subıd}/sağlayıcılar/Microsoft. Commerce/Usagetoplamalar? reportedStartTime = {reportedStartTime} & reportedEndTime = {reportedEndTime} & Aggregationayrıntı düzeyi = {ayrıntı düzeyi} & api-Version = 2015-06-01-önizleme & continuationToken = {Token-Value} |

### <a name="parameters"></a>Parametreler

| **Parametre** | **Açıklama** |
| --- | --- |
| Armendpoint |Azure Stack ortamınızın uç noktası Azure Resource Manager. Azure Stack kuralı, Azure Resource Manager uç noktanın adının biçimde `https://management.{domain-name}`olması olur. Örneğin, geliştirme seti için, etki alanı adı Local. azurestack. External, ardından Kaynak Yöneticisi uç noktası olur `https://management.local.azurestack.external`. |
| Subıd |Çağrıyı yapan kullanıcının abonelik KIMLIĞI. Bu API 'yi yalnızca tek bir aboneliğin kullanımını sorgulamak için kullanabilirsiniz. Sağlayıcılar, tüm kiracıların kullanımını sorgulamak için sağlayıcı kaynak kullanımı API 'sini kullanabilir. |
| reportedStartTime |Sorgunun başlangıç saati. *DateTime* değeri UTC biçiminde ve saatin başında olmalıdır; Örneğin, 13:00. Günlük toplama için bu değeri UTC gece yarısı olarak ayarlayın. Biçim kaçışdır ISO 8601; Örneğin, **2015-06-16T18% 3A53% 3A11% 2b00% 3a00Z**, iki nokta üst üste% 3A ve artı, URI kolay olması için% 2B öğesine atmalıdır. |
| reportedEndTime |Sorgunun bitiş saati. **Reportedstarttime** için uygulanan kısıtlamalar bu parametre için de geçerlidir. **Reportedendtime** değeri gelecekte olamaz. |
| Aggregationayrıntı düzeyi |İki ayrı olası değeri olan isteğe bağlı parametre: **günlük** ve **saatlik**. Değerler önereceği için, biri günlük ayrıntı düzeyi halinde verileri döndürür ve diğeri saatlik bir çözünürlüğlük olur. **Günlük** seçeneği varsayılandır. |
| api-version |Bu isteği yapmak için kullanılan protokolün sürümü. **2015-06-01-Preview**kullanmanız gerekir. |
| continuationToken |Son çağrıdan kullanım API sağlayıcısına alınan belirteç. Yanıt 1.000 satırdan fazla olduğunda bu belirteç gereklidir. İlerleme için bir yer işareti görevi görür. Mevcut değilse, veriler, geçirilen ayrıntı düzeyine göre günün veya saatin başından itibaren alınır. |

### <a name="response"></a>Yanıt

```html
GET
/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&api-version=1.0
```

```json
{
"value": [
{

"id":
"/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",
"name": "sub1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

...
```

### <a name="response-details"></a>Yanıt ayrıntıları

| **Parametre** | **Açıklama** |
| --- | --- |
| id |Kullanım toplamanın benzersiz KIMLIĞI. |
| name |Kullanım toplamanın adı. |
| type |Kaynak tanımı. |
| subscriptionId |Azure kullanıcısının abonelik tanımlayıcısı. |
| usageStartTime |Bu kullanım toplamasının ait olduğu kullanım demetini 'nin UTC başlangıç saati. |
| Usagebitişsaati |Bu kullanım toplamasının ait olduğu kullanım demetini 'nin UTC bitiş saati. |
| InstanceData |Örnek ayrıntılarının anahtar-değer çiftleri (yeni bir biçimde):<br>  *resourceUri*: Kaynak grupları ve örnek adı dahil olmak üzere tam kaynak KIMLIĞI. <br>  *konum*: Bu hizmetin çalıştırıldığı bölge. <br>  *Etiketler*: Kullanıcının belirttiği kaynak etiketleri. <br>  *AdditionalInfo*: Tüketilen kaynak hakkında daha fazla ayrıntı, örneğin, işletim sistemi sürümü veya görüntü türü. |
| quantity |Bu zaman çerçevesinde gerçekleşen kaynak tüketimi miktarı. |
| meterId |Tüketilen kaynak için benzersiz KIMLIK ( **RESOURCEID**olarak da bilinir). |

## <a name="next-steps"></a>Sonraki adımlar

- [Sağlayıcı kaynak kullanım API’si](azure-stack-provider-resource-api.md)
- [Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)
