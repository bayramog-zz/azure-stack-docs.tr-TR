---
title: Azure Stack 'de Azure Izleyici için desteklenen ölçümler | Microsoft Docs
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
ms.date: 07/23/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: c099d67c6446fbd77db62f4c496868437d861f85
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418495"
---
# <a name="supported-metrics-for-azure-monitor-on-azure-stack"></a>Azure Stack 'de Azure Izleyici için desteklenen ölçümler

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack üzerindeki Azure izleyici ölçümleri, küresel Azure 'da oldukları şekilde alınır. Portalda, ölçü oluşturma, REST API'SİNDEN alın veya PowerShell veya CLI ile sorgulama.

Aşağıdaki tablolarda, Azure Stack üzerinde Azure Izleyici ölçüm işlem hattı ile kullanılabilen ölçümler listelenmektedir. Bu ölçümleri sorgulamak ve erişmek için API profilinin **2018-01-01** api sürümü sürümünü kullanın. API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| CPU yüzdesi | CPU yüzdesi | Yüzde | Average | VM 'ler tarafından şu anda kullanılmakta olan ayrılmış işlem birimlerinin yüzdesi. | Boyut yok |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Kullanılan kapasite | Bayt | Average | Hesap kullanılan kapasite. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob Kapasitesi | Bayt | Toplam | Depolama hesabının blob hizmeti tarafından bayt olarak kullanılan depolama miktarı. | BlobType |
| BLOB sayısı | Blob Sayısı | Sayı | Toplam | Depolama hesabının blob hizmetindeki Blobların sayısı. | BlobType |
| ContainerCount | Blob Kapsayıcı Sayısı | Sayı | Average | Depolama hesabının blob hizmetindeki kapsayıcı sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Tablo Kapasitesi | Bayt | Average | Bayt olarak depolama hesabının tablo hizmeti tarafından kullanılan depolama miktarı. | Boyut yok |
| TableCount | Tablo Sayısı | Sayı | Average | Depolama hesabının tablo hizmetindeki tablo sayısı. | Boyut yok |
| TableEntityCount | Tablo Varlık Sayısı | Sayı | Average | Depolama hesabının tablo hizmetindeki tablo varlıklarının sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Kuyruk Kapasitesi | Bayt | Average | Depolama hesabının Kuyruk hizmeti bayt cinsinden kullandığı depolama miktarı. | Boyut yok |
| QueueCount | Kuyruk Sayısı | Sayı | Average | Depolama hesabının Kuyruk hizmeti sıra sayısı. | Boyut yok |
| QueueMessageCount | Kuyruk İleti Sayısı | Sayı | Average | Depolama hesabının Kuyruk hizmeti sıra iletilerinin yaklaşık sayısı. | Boyut yok |
| İşlemler | İşlemler | Sayı | Toplam | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve ayrıca hata üreten istekleri içerir. Farklı yanıt türlerinin sayısı için ResponseType boyutunu kullanın. | ResponseType, GeoType, ApiName |
| Giriş | Giriş | Bayt | Toplam | Bayt olarak giriş verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya giriş ve ayrıca Azure 'da giriş içerir. | GeoType, ApiName |
| Çıkış | Çıkış | Bayt | Toplam | Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış bir istemciden Azure depolama 'ya çıkış ve ayrıca Azure 'da çıkış içerir. Sonuç olarak, bu numara faturalandırılabilir çıkış yansıtmamaktadır. | GeoType, ApiName |
| SuccessServerLatency | Başarı Sunucu Gecikme Süresi | Milisaniye | Ortalama | Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency içinde belirtilen ağ gecikmesini içermez. | GeoType, ApiName |
| Başarı E2e | Başarı E2E Gecikme Süresi | Milisaniye | Ortalama | Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. | GeoType, ApiName |
| Kullanılabilirlik | Kullanılabilirlik | Yüzde | Ortalama | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. TotalBillableRequests değerini alarak kullanılabilirliği hesaplayın ve beklenmeyen hatalar üreten istekler dahil olmak üzere geçerli istek sayısına bölün. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır. | GeoType, ApiName |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
