---
title: Azure Stack 'de Azure Izleyici için desteklenen ölçümler | Microsoft Docs
description: Azure Stack 'de Azure Izleyici için desteklenen ölçümler hakkında bilgi edinin.
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
ms.date: 11/11/2019
ms.author: mabrigg
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: c3824b80ddd6d980b408bfdb2a2c4a55f599e6d0
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955693"
---
# <a name="supported-metrics-for-azure-monitor-on-azure-stack"></a>Azure Stack 'de Azure Izleyici için desteklenen ölçümler

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack üzerindeki Azure izleyici ölçümleri, küresel Azure 'da oldukları şekilde alınır. Ölçümlerinizi portalda oluşturabilir, REST API alabilir veya PowerShell veya CLı ile sorgulayabilirsiniz.

Aşağıdaki tablolarda, Azure Stack üzerinde Azure Izleyici ölçüm işlem hattı ile kullanılabilen ölçümler listelenmektedir. Bu ölçümleri sorgulamak ve erişmek için API profilinin **2018-01-01** api sürümü sürümünü kullanın. API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama | Boyutlar |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| CPU yüzdesi | CPU yüzdesi | Yüzde | Ortalama | VM 'ler tarafından şu anda kullanılmakta olan ayrılmış işlem birimlerinin yüzdesi. | Boyut yok |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Kullanılan kapasite | Sayacının | Ortalama | Hesap kullanılan kapasite. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Sayacının | Toplam | Bayt cinsinden giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Sayacının | Toplam | Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| Başarılı Sunucugecikmesi | Başarı sunucusu gecikmesi | Mayacak | Ortalama | Azure depolama tarafından başarılı bir isteği işlemek için kullanılan ortalama gecikme süresi (milisaniye cinsinden). Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| SuccessE2ELatency | Başarılı E2E gecikmesi | Mayacak | Ortalama | Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Erişilebilirlik | Erişilebilirlik | Yüzde | Ortalama | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft. Storage/storageAccounts/blobServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama | Boyutlar |
|--------|---------------------|------|------------------|-------------|------------|
| BlobCapacity | Blob kapasitesi | Sayacının | Toplam | Depolama hesabının blob hizmeti tarafından bayt olarak kullanılan depolama miktarı. | BlobType |
| BlobCount | BLOB sayısı | Sayı | Toplam | Depolama hesabının blob hizmetindeki Blobların sayısı. | BlobType |
| ContainerCount | Blob kapsayıcı sayısı | Sayı | Ortalama | Depolama hesabının blob hizmetindeki kapsayıcı sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Sayacının | Toplam | Bayt cinsinden giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Sayacının | Toplam | Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| Başarılı Sunucugecikmesi | Başarı sunucusu gecikmesi | Mayacak | Ortalama | Azure depolama tarafından başarılı bir isteği işlemek için kullanılan ortalama gecikme süresi (milisaniye cinsinden). Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| SuccessE2ELatency | Başarılı E2E gecikmesi | Mayacak | Ortalama | Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Erişilebilirlik | Erişilebilirlik | Yüzde | Ortalama | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft. Storage/storageAccounts/tableServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| Tablokapasitesi | Tablo kapasitesi | Sayacının | Ortalama | Bayt olarak depolama hesabının tablo hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| Tablosayısı | Tablo sayısı | Sayı | Ortalama | Depolama hesabının tablo hizmetindeki tablo sayısı. | Boyut yok |
| TableEntityCount | Tablo varlık sayısı | Sayı | Ortalama | Depolama hesabının tablo hizmetindeki tablo varlıklarının sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Sayacının | Toplam | Bayt cinsinden giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Sayacının | Toplam | Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| Başarılı Sunucugecikmesi | Başarı sunucusu gecikmesi | Mayacak | Ortalama | Azure depolama tarafından başarılı bir isteği işlemek için kullanılan ortalama gecikme süresi (milisaniye cinsinden). Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| SuccessE2ELatency | Başarılı E2E gecikmesi | Mayacak | Ortalama | Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Erişilebilirlik | Erişilebilirlik | Yüzde | Ortalama | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft. Storage/storageAccounts/queueServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Kuyruk kapasitesi | Sayacının | Ortalama | Depolama hesabının Kuyruk hizmeti bayt cinsinden kullandığı depolama miktarı. | Boyut yok |
| QueueCount | Sıra sayısı | Sayı | Ortalama | Depolama hesabının Kuyruk hizmeti sıra sayısı. | Boyut yok |
| QueueMessageCount | Kuyruk Iletisi sayısı | Sayı | Ortalama | Depolama hesabının Kuyruk hizmeti sıra iletilerinin yaklaşık sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Sayacının | Toplam | Bayt cinsinden giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Sayacının | Toplam | Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| Başarılı Sunucugecikmesi | Başarı sunucusu gecikmesi | Mayacak | Ortalama | Azure depolama tarafından başarılı bir isteği işlemek için kullanılan ortalama gecikme süresi (milisaniye cinsinden). Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| SuccessE2ELatency | Başarılı E2E gecikmesi | Mayacak | Ortalama | Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Erişilebilirlik | Erişilebilirlik | Yüzde | Ortalama | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. | GeoType, ApiName |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack üzerinde Azure izleyici](azure-stack-metrics-azure-data.md)hakkında daha fazla bilgi edinin.
