---
title: Azure Stack'te Azure İzleyici ölçümleri desteklenen | Microsoft Docs
description: Azure Stack'te Azure İzleyici için desteklenen ölçümler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: 442fc6080f9b0aba87e0141257f79cdf910e0a41
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816200"
---
# <a name="supported-metrics-for-azure-monitor-on-azure-stack"></a>Azure Stack'te Azure İzleyici ile desteklenen ölçümler

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Azure Stack'te Azure ölçümleri izleme genel Azure'da olduğu gibi aynı şekilde alınır. Portalda, ölçü oluşturma, REST API'SİNDEN alın veya PowerShell veya CLI ile sorgulama.

Aşağıdaki tablolarda Azure İzleyicisi'nin Azure Stack'te ölçüm işlem hattı ile mevcut olan ölçümler listelenmektedir. Sorgu ve bu ölçümlere erişmek için kullandığınız **2018-01-01** API profili api sürümü sürümü. API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| CPU yüzdesi | CPU yüzdesi | Yüzde | Average | VM'ler tarafından kullanıldığından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi. | Boyut yok |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Kullanılan kapasite | Bayt | Average | Hesabın kullanılan kapasitesi. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız istekleri ve hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı giriş dış istemciden Azure Depolama'ya ve Azure içinde içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı çıkış dış istemciden Azure Depolama'ya ve ayrıca Azure içinde çıkış içerir. Sonuç olarak, bu sayı, Faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten bu istekleri dahil ilgili istek sayısına göre bölme hesaplayın. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob Kapasitesi | Bayt | Toplam | Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı. | BlobType |
| BLOB sayısı | Blob Sayısı | Sayı | Toplam | Depolama hesabının Blob hizmetindeki BLOB sayısı. | BlobType |
| ContainerCount | Blob Kapsayıcı Sayısı | Sayı | Average | Depolama hesabının Blob hizmetindeki kapsayıcı sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız istekleri ve hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı giriş dış istemciden Azure Depolama'ya ve Azure içinde içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı çıkış dış istemciden Azure Depolama'ya ve ayrıca Azure içinde çıkış içerir. Sonuç olarak, bu sayı, Faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten bu istekleri dahil ilgili istek sayısına göre bölme hesaplayın. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Tablo Kapasitesi | Bayt | Average | Bayt olarak depolama hesabının tablo hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| TableCount | Tablo Sayısı | Sayı | Average | Depolama hesabının tablo hizmetindeki tablo sayısı. | Boyut yok |
| TableEntityCount | Tablo Varlık Sayısı | Sayı | Average | Depolama hesabının tablo hizmetindeki tablo varlıklarının sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız istekleri ve hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı giriş dış istemciden Azure Depolama'ya ve Azure içinde içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı çıkış dış istemciden Azure Depolama'ya ve ayrıca Azure içinde çıkış içerir. Sonuç olarak, bu sayı, Faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten bu istekleri dahil ilgili istek sayısına göre bölme hesaplayın. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Kuyruk Kapasitesi | Bayt | Average | Bayt olarak depolama hesabının kuyruk hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| QueueCount | Kuyruk Sayısı | Sayı | Average | Depolama hesabının kuyruk hizmetindeki sıra sayısı. | Boyut yok |
| QueueMessageCount | Kuyruk İleti Sayısı | Sayı | Average | Depolama hesabının kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız istekleri ve hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı giriş dış istemciden Azure Depolama'ya ve Azure içinde içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı çıkış dış istemciden Azure Depolama'ya ve ayrıca Azure içinde çıkış içerir. Sonuç olarak, bu sayı, Faturalanabilir çıkışı yansıtmaz. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten bu istekleri dahil ilgili istek sayısına göre bölme hesaplayın. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
