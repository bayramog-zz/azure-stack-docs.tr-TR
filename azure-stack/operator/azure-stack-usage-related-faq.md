---
title: Kullanım API 'SI ile ilgili SSS | Microsoft Docs
description: Azure Stack ölçümlerinin listesi, Azure kullanım API 'sine karşılaştırma, kullanım süresi ve raporlanan süre, hata kodları.
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
ms.date: 09/25/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 02/26/2019
ms.openlocfilehash: b4780077f015c060c63abc3abd33bd3e71c63e15
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019328"
---
# <a name="frequently-asked-questions-about-azure-stack-usage"></a>Azure Stack kullanımı hakkında sık sorulan sorular

Bu makalede Azure Stack kullanımı ve Azure Stack kullanım API 'SI hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçüm kimliklerini görebilirim?

Aşağıdaki kaynak sağlayıcıları için kullanım bildirilir:

### <a name="network"></a>Ağ
  
**Ölçüm kimliği**: F271A8A388C44D93956A063E1D2FA80B  
**Ölçüm adı**: statik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: kullanılan IP adresi sayısı. Kullanım API 'sini günlük bir ayrıntı düzeyiyle çağırırsanız, ölçüm IP adresini saat sayısıyla çarparak döndürür.  
  
**Ölçüm kimliği**: 9e2739ba86744796b465f64674b822ba  
**Ölçüm adı**: dinamik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: kullanılan IP adresi sayısı. Kullanım API 'sini günlük bir ayrıntı düzeyiyle çağırırsanız, ölçüm IP adresini saat sayısıyla çarparak döndürür.  
  
### <a name="storage"></a>Depolama
  
**Ölçüm kimliği**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Ölçüm adı**: tablokapasitesi  
**Birim**: GB\*saat  
**Notlar**: tablolar tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Ölçüm adı**: pageblobcapacity  
**Birim**: GB\*saat  
**Notlar**: sayfa Blobları tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Ölçüm adı**: queuecapacity  
**Birim**: GB\*saat  
**Notlar**: sıra tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: 09F8879e-87e9-4305-A572-4b7be209f857  
**Ölçüm adı**: blockblobcapacity  
**Birim**: GB\*saat  
**Notlar**: blok Blobları tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Ölçüm adı**: tabletransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: tablo hizmeti istekleri (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Ölçüm adı**: tabledatatransin  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: tablo HIZMETI verileri GB cinsinden giriş.  
  
**Ölçüm kimliği**: 1B8c1dec-EE42-414b-aa36-6229cf199370  
**Ölçüm adı**: tabledatatransout  
**Birim**: GB cinsinden çıkış  
**Notlar**: GB cinsinden tablo hizmeti veri çıkışı.
  
**Ölçüm kimliği**: 43Davf82b-4618-444a-b994-40c23f7cd438  
**Ölçüm adı**: blobtransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: blob hizmeti istekleri (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: 9764F92c-e44a-498e-8DC1-aad66587a810  
**Ölçüm adı**: blobdatatransin  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: blob hizmeti VERI girişi GB cinsinden.  
  
**Ölçüm kimliği**: 3023Fef4-eca5-4d7b-87b3-cfbc061931e8  
**Ölçüm adı**: blobdatatransout  
**Birim**: GB cinsinden çıkış  
**Notlar**: blob hizmeti VERI çıkış GB olarak.  
  
**Ölçüm kimliği**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Ölçüm adı**: queuetransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: kuyruk hizmeti istekleri (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: E518E809-E369-4A45-9274-2017B29FFF25  
**Ölçüm adı**: queuedatatransin  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: verilerin GB cinsinden kuyruk hizmeti.  
  
**Ölçüm kimliği**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Ölçüm adı**: queuedatatransout  
**Birim**: GB cinsinden çıkış  
**Notlar**: kuyruk hizmeti VERI çıkışı GB cinsinden  

### <a name="compute"></a>İşlem
  
**Ölçüm kimliği**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Ölçüm adı**: Temel VM boyut saatleri  
**Birim**: sanal çekirdek saatleri  
**Notlar**: sanal çekırdek sayısı VM 'nin çalıştırıldığı saatlere göre çarpılır.  
  
**Ölçüm kimliği**: 9cd92d4c-bafd-4492-B278-BEDC2DE8232A  
**Ölçüm adı**: Windows VM boyut saatleri  
**Birim**: sanal çekirdek saatleri  
**Notlar**: sanal çekirdeklerin sayısı, VM 'nin çalıştırıldığı saatlere göre çarpılır.  
  
**Ölçüm kimliği**: 6Davb500f-a4fd-49c4-956d-229bb9c8c793  
**Ölçüm adı**: VM boyut saatleri  
**Birim**: VM saatleri  
**Notlar**: hem temel hem de Windows VM yakalar. Çekirdekler için ayarlama yapmaz.  
  
### <a name="managed-disks"></a>Yönetilen Diskler

**Ölçüm kimliği**: 380874f9-300C-48e0-95a0-d2d9a21ade8f   
**Ölçüm adı**: S4   
**Birim**: ay\*disk sayısı   
**Notlar**: Standart yönetilen Disk-32 GB 

**Ölçüm kimliği**: 1b77d90f-427B-4435-b4f1-d78adec53222   
**Ölçüm adı**: S6   
**Birim**: ay\*disk sayısı   
**Notlar**: Standart yönetilen Disk-64 GB 

**Ölçüm kimliği**: d5f7731b-f639-404A-89d0-e46186e22c8d   
**Ölçüm adı**: S10   
**Birim**: ay\*disk sayısı   
**Notlar**: Standart yönetilen Disk-128 GB 

**Ölçüm kimliği**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a   
**Ölçüm adı**: S15   
**Birim**: ay\*disk sayısı   
**Notlar**: Standart yönetilen Disk-256 GB 

**Ölçüm kimliği**: 88ea9228-457A-4091-adc9-ad5194f30b6e   
**Ölçüm adı**: S20   
**Birim**: ay\*disk sayısı      
**Notlar**: Standart yönetilen Disk-512 GB 

**Ölçüm kimliği**: 5b1db88a-8596-4002-8052-347947c26940   
**Ölçüm adı**: S30   
**Birim**: ay\*disk sayısı   
**Notlar**: Standart yönetilen Disk-1024 GB 

**Ölçüm kimliği**: 7660b45b-b29d-49cb-b816-59f30fbe011   
**Ölçüm adı**: P4   
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-32 GB 

**Ölçüm kimliği**: 817007fd-a077-477f-BC01-b876f27205fd   
**Ölçüm adı**: P6   
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-64 GB 

**Ölçüm kimliği**: e554b6bc-96cd-4938-a5b5-0dad990278519   
**Ölçüm adı**: P10   
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-128 GB  

**Ölçüm kimliği**: cdc0f53a-62a9-4472-a06c-e99a23b02907   
**Ölçüm adı**: P15  
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-256 GB 

**Ölçüm kimliği**: b9cb2d1a-84C2-4275-aa8b-70d2145d59aa   
**Ölçüm adı**: P20   
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-512 GB 

**Ölçüm kimliği**: 06bde724-9f94-43c0-84c3-d0fc54538369   
**Ölçüm adı**: P30   
**Birim**: ay\*disk sayısı   
**Notlar**: Premium yönetilen Disk-1024 GB 

**Ölçüm kimliği**: 7ba084ec-ef9c-4d64-A179-7732c6cb5e28   
**Ölçüm adı**: actualstandarddisksize   
**Birim**: GB\*ay      
**Notlar**: Standart yönetilen diskin disk üzerindeki gerçek boyutu  

**Ölçüm kimliği**: daef389a-06e5-4684-A7F7-8813d9f792d5  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: GB\*ay      
**Notlar**: Premium yönetilen disk diskteki gerçek boyut 

**Ölçüm kimliği**: 108fa95b-be0d-4cd9-96e8-5b0d59505df1  
**Ölçüm adı**: actualstandardsnapshotsize   
**Birim**: GB\*ay   
**Notlar**: yönetilen standart anlık görüntünün diskindeki gerçek boyut.  

**Ölçüm kimliği**: 578ae51d-4ef9-42f9-85ae-42b52d3d83ac   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: GB\*ay   
**Notlar**: yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut.   

**Ölçüm kimliği**: 5d76e09f-4567-452A-94cc-7d1f097761f0   
**Ölçüm adı**: S4   
**Birim**: disk sayısı\*saat   
**Notlar**: Standart yönetilen Disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Ölçüm adı**: S6   
**Birim**: disk sayısı\*saat   
**Notlar**: Standart yönetilen Disk-64 GB (kullanım dışı) 

**Ölçüm kimliği**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Ölçüm adı**: S10   
**Birim**: disk sayısı\*saat   
**Notlar**: Standart yönetilen Disk-128 GB (kullanım dışı) 

**Ölçüm kimliği**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Ölçüm adı**: S15   
**Birim**: disk sayısı\*saat   
**Notlar**: Standart yönetilen Disk-256 GB (kullanım dışı) 

**Ölçüm kimliği**: 5938f8dav-0ecd-4c48-8d5a-c7c6c23546be   
**Ölçüm adı**: S20   
**Birim**: disk sayısı\*saat      
**Notlar**: Standart yönetilen Disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Ölçüm adı**: S30   
**Birim**: disk sayısı\*saat   
**Notlar**: Standart yönetilen Disk-1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Ölçüm adı**: P4   
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: 518b412b-1927-4f25-985F-4aea24e55c4f   
**Ölçüm adı**: P6   
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-64 GB (kullanım dışı) 

**Ölçüm kimliği**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Ölçüm adı**: P10   
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-128 GB (kullanım dışı)  

**Ölçüm kimliği**: 8de91c94-F740-4d9a-b665-bd5974fa08d4   
**Ölçüm adı**: P15  
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-256 GB (kullanım dışı) 

**Ölçüm kimliği**: c7e7839c-293B-4761-ae4c-848eda91130b   
**Ölçüm adı**: P20   
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Ölçüm adı**: P30   
**Birim**: disk sayısı\*saat   
**Notlar**: Premium yönetilen Disk-1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 8a409390-1913-40AE-917b-08d0f16f3c38   
**Ölçüm adı**: actualstandarddisksize   
**Birim**: bayt\*saat      
**Notlar**: Standart yönetilen disk diskteki gerçek boyut (kullanım dışı)  

**Ölçüm kimliği**: 1273b16f-8458-4C34-8CE2-a515de551ef6  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: bayt\*saat      
**Notlar**: Premium yönetilen disk diskteki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Ölçüm adı**: actualstandardsnapshotsize   
**Birim**: bayt\*saat   
**Notlar**: yönetilen standart anlık görüntünün disk üzerindeki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 95b0c03f-8A82-4524-8961-ccfbf575f536   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: bayt\*saat   
**Notlar**: yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 75d4b707-1027-4403-9986-6ec7c05579c8 **ölçüm adı**: Actualstandardsnapshotsize **Unit**: GB\*ay **notları**: yönetilen standart anlık görüntünün disk üzerindeki gerçek boyut (kullanım dışı)  

**Ölçüm kimliği**: 5ca1cbb9-6f14-4E76-8be8-1ca91547965e **ölçüm adı**: ActualPremiumSnapshotSize **Unit**: GB\*ay **notları**: yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut (kullanım dışı)  

### <a name="sql-rp"></a>SQL RP
  
**Ölçüm kimliği**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Ölçüm adı**: DatabaseSizeHourSqlMeter  
**Birim**: MB\*saat  
**Notlar**: oluşturma SıRASıNDA toplam DB kapasitesi. Kullanım API 'sini günlük bir ayrıntı düzeyi ile çağırırsanız, ölçüm MB değerini saat sayısıyla çarpılır.  
  
### <a name="mysql-rp"></a>MySql RP   
  
**Ölçüm kimliği**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Ölçüm adı**: DatabaseSizeHourMySqlMeter  
**Birim**: MB\*saat  
**Notlar**: oluşturma SıRASıNDA toplam DB kapasitesi. Kullanım API 'sini günlük bir ayrıntı düzeyi ile çağırırsanız, ölçüm MB değerini saat sayısıyla çarpılır.    
### <a name="key-vault"></a>Key Vault   
  
**Ölçüm kimliği**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Ölçüm adı**: Key Vault işlemler  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: Key Vault veri düzlemi tarafından alınan REST API isteklerinin sayısı.  
  
**Ölçüm kimliği**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Ölçüm adı**: Gelişmiş Anahtarlar işlemleri  
**Birim**: 10.000 işlem  
**Notlar**: RSA 3K/4k, ECC anahtar işlemleri. (Önizleme).  
  
### <a name="app-service"></a>App Service   
  
**Ölçüm kimliği**: 190C935e-9ada-48ff-9ab8-56ea1cf9adaa  
**Ölçüm adı**: App Service  
**Birim**: sanal çekirdek saatleri  
**Notlar**: App Service 'i çalıştırmak için kullanılan sanal çekirdek sayısı. Note: Microsoft, Azure Stack App Service ücretlendirmesi için bu ölçümü kullanır. Bulut çözümü sağlayıcıları, kiracılarının kullanımını hesaplamak için diğer App Service ölçüleri (aşağıda) kullanabilir.  
  
**Ölçüm kimliği**: 67Cc4afc-0691-48e1-a4b8-d744d1fedbde  
**Ölçüm adı**: işlev istekleri  
**Birim**: 10 istek  
**Notlar**: istenen yürütmelerin toplam sayısı (10 yürütme başına). Bir olaya yanıt olarak bir işlev çalıştırıldığında yürütmeler sayılır veya bir bağlama tarafından tetiklenir.  
  
**Ölçüm kimliği**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Ölçüm adı**: Işlevler-işlem  
**Birim**: GB-s  
**Notlar**: kaynak tüketimi gigabayt sanıye (GB/sn) cinsinden ölçülür. **Gözlemlenen kaynak tüketimi** , işlevi yürütmek için geçen milisaniye cinsinden ortalama bellek boyutu GB cinsinden çarpılarak hesaplanır. Bir işlev tarafından kullanılan bellek en yakın 128 MB 'a kadar, en yakın 1 MS 'ye kadar yuvarlanan yürütme süresi olan 1.536 MB 'lık maksimum bellek boyutuna kadar ölçülür. Tek bir işlev yürütmesi için en düşük yürütme süresi ve bellek, sırasıyla 100 ms ve 128 MB 'dir.  
  
**Ölçüm kimliği**: 957E9f36-2c14-45a1-b6a1-1723ef71a01d  
**Ölçüm adı**: paylaşılan App Service saatleri  
**Birim**: 1 saat  
**Notlar**: bir saatlik parça App Service planı kullanımı. Planlar, uygulama başına temelinde Ücretlendirilebilir.  
  
**Ölçüm kimliği**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Ölçüm adı**: ücretsiz App Service saat  
**Birim**: 1 saat  
**Notlar**: saatlik ücretsiz App Service planı kullanımı. Planlar, uygulama başına temelinde Ücretlendirilebilir.  
  
**Ölçüm kimliği**: 88039D51-A206-3a89-e9de-c5117e2d10a6  
**Ölçüm adı**: küçük Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: boyut ve örnek sayısına göre hesaplanır.  
  
**Ölçüm kimliği**: 83A2a13e-4788-78dd-5d55-2831b68ed825  
**Ölçüm adı**: orta Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: boyut ve örnek sayısına göre hesaplanır.  
  
**Ölçüm kimliği**: 1083B9db-e9bb-24be-a5e9-d6fdd0ddefe6  
**Ölçüm adı**: büyük Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: boyut ve örnek sayısına göre hesaplanır.  
  
### <a name="custom-worker-tiers"></a>Özel çalışan katmanları   
  
**Ölçüm kimliği**: *özel çalışan katmanları*  
**Ölçüm adı**: özel çalışan katmanları  
**Birim**: Saat  
**Notlar**: SKU ve özel çalışan katmanı adı temel alınarak BELIRLEYICI ölçüm kimliği oluşturulur. Bu ölçüm KIMLIĞI her özel çalışan katmanı için benzersizdir.  
  
**Ölçüm kimliği**: 264Acb47-AD38-47F8-Add3-47f01dc4f473  
**Ölçüm adı**: SNI SSL  
**Birim**: SNI SSL bağlama başına  
**Notlar**: App Service ıkı tür SSL bağlantısını destekler: sunucu adı belirtme (SNI) SSL BAĞLANTıLARı ve IP adresi SSL bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 60B42d72-dc1c-472c-9895-6c516277edb4  
**Ölçüm adı**: IP SSL  
**Birim**: IP tabanlı SSL bağlaması başına  
**Notlar**: App Service ıkı tür SSL bağlantısını destekler: sunucu adı belirtme (SNI) SSL BAĞLANTıLARı ve IP adresi SSL bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Ölçüm adı**: Web işlemi  
**Birim**:  
**Notlar**: etkin site başına saat başına hesaplanır.  
  
**Ölçüm kimliği**: 5887D39b-0253-4e12-83c7-03e1a93dffd9  
**Ölçüm adı**: dış çıkış bant genişliği  
**Birim**: GB  
**Notlar**: toplam gelen istek yanıtı bayt sayısı + toplam giden istek baytı + toplam gelen FTP isteği yanıt baytı + toplam gelen Web dağıtımı istek yanıtı baytı.  
  
## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>Azure Stack kullanım API 'Leri [Azure kullanım API 'si](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) ile nasıl karşılaştırılır (Şu anda genel önizlemede)?

* Kiracı kullanım API 'si Azure API 'siyle tutarlıdır, tek bir istisna vardır: *ShowDetails* bayrağı şu anda Azure Stack desteklenmez.
* Sağlayıcı kullanım API 'SI yalnızca Azure Stack için geçerlidir.
* Şu anda Azure 'da kullanılabilen [ratecard API 'si](/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) Azure Stack ' de kullanılamaz.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Kullanım süresi ve raporlanan süre arasındaki fark nedir?

Kullanım verileri raporlarının iki ana zaman değeri vardır:

* **Raporlanan süre**. Kullanım olayının kullanım sistemine girdiği saat
* **Kullanım süresi**. Azure Stack kaynağın tüketilme zamanı

Kullanım süresi ve belirli bir kullanım olayı için raporlanan süre değerlerinde bir tutarsızlık görebilirsiniz. Gecikme, herhangi bir ortamda birkaç saat kadar sürebilir.

Şu anda yalnızca *raporlanan süre*ile sorgulama yapabilirsiniz.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Bu kullanım API 'SI hata kodlarının anlamı nedir?

| **HTTP durum kodu** | **Hata kodu** | **Açıklama** |
| --- | --- | --- |
| 400/Hatalı Istek |*NoApiVersion* |*Api sürümü* sorgu parametresi eksik. |
| 400/Hatalı Istek |*Invalidproperty* |Bir özellik eksik veya geçersiz bir değere sahip. Yanıt gövdesindeki hata kodundaki ileti eksik özelliği tanımlar. |
| 400/Hatalı Istek |*Requestendtimeısınfuture* |*Reportedendtime* değeri gelecekte. Bu bağımsız değişken için gelecekte değerlere izin verilmez. |
| 400/Hatalı Istek |*SubscriberIdIsNotDirectTenant* |Sağlayıcı API çağrısı, çağıranın geçerli bir kiracısı olmayan bir abonelik KIMLIĞI kullandı. |
| 400/Hatalı Istek |*SubscriptionIdMissingInRequest* |Çağıranın abonelik KIMLIĞI eksik. |
| 400/Hatalı Istek |*Invalidaggregationayrıntı düzeyi* |Geçersiz bir toplama ayrıntı düzeyi istendi. Geçerli değerler günlük ve saatlik değerlerdir. |
| 503 |*ServiceUnavailable* |Hizmetin meşgul olması veya çağrının kısıtlandığı için yeniden denenebilir bir hata oluştu. |

## <a name="what-is-the-policy-for-charging-for-vms"></a>VM 'Leri doldurma ilkesi nedir?

Çalışan ve durdurulmuş VM 'Ler kullanım verileri oluşturur. Azure ile tutarlı, kullanım verilerinin emisyonunu durdurmak için ayırmayı kaldırma gerekir. Portalın kullanılamadığı ancak işlem kaynak sağlayıcısının hala çalıştığı durumlarda, kullanım de kullanıma sunulacaktır.

## <a name="how-do-i-extract-usage-data-from-the-azure-stack-usage-apis"></a>Kullanım verilerini Azure Stack kullanım API 'Lerinden Nasıl yaparım? ayıklayın?

Bir Azure Stack yerel kullanım API 'Lerinden kullanım verilerini ayıklamanın en kolay yolu, [GitHub 'daki Kullanım Özeti betiğini](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/Usagesummary.ps1)kullanmaktır. Komut dosyası başlangıç ve bitiş tarihlerini giriş parametreleri olarak gerektirir.

Alternatif olarak, [sağlayıcı kaynak kullanımı API 'si](azure-stack-provider-resource-api.md) ve [KIRACı kaynak kullanımı API](azure-stack-tenant-resource-usage-api.md) makalelerinde açıklandığı gibi REST API 'lerini de kullanabilirsiniz.

## <a name="how-can-i-associate-usage-extracted-from-azure-usage-apis-to-a-specific-azure-stack-user-subscription"></a>Azure kullanım API 'Lerinden ayıklanan kullanımı, belirli bir Azure Stack kullanıcı aboneliğine nasıl ilişkilendirebilirim?

Kullanım kayıtları, Azure Stack abonelik KIMLIĞINI içeren **AdditionalInfo**adlı bir özellik paketi içerir. Bu, ilgili kullanım kaydını yayan kullanıcı aboneliğine sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack Müşteri Faturalama ve geri ödeme](azure-stack-billing-and-chargeback.md)
* [Sağlayıcı kaynağı kullanım API 'SI](azure-stack-provider-resource-api.md)
* [Kiracı Kaynak Kullanım API’si](azure-stack-tenant-resource-usage-api.md)
