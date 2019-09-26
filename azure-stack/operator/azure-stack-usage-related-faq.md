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
ms.openlocfilehash: d63d4876674c66fcccab942cd856dce958e62644
ms.sourcegitcommit: 32609bdb04a07b063c8f20f892c30769ad6903dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71269474"
---
# <a name="frequently-asked-questions-about-azure-stack-usage"></a>Azure Stack kullanımı hakkında sık sorulan sorular

Bu makalede Azure Stack kullanımı ve Azure Stack kullanım API 'SI hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçüm kimliklerini görebilirim?

Aşağıdaki kaynak sağlayıcıları için kullanım bildirilir:

### <a name="network"></a>Ağ
  
**Ölçüm kimliği**: F271A8A388C44D93956A063E1D2FA80B  
**Ölçüm adı**: Statik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: Kullanılan IP adresi sayısı. Kullanım API 'sini günlük bir ayrıntı düzeyiyle çağırırsanız, ölçüm IP adresini saat sayısıyla çarparak döndürür.  
  
**Ölçüm kimliği**: 9E2739BA86744796B465F64674B822BA  
**Ölçüm adı**: Dinamik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: Kullanılan IP adresi sayısı. Kullanım API 'sini günlük bir ayrıntı düzeyiyle çağırırsanız, ölçüm IP adresini saat sayısıyla çarparak döndürür.  
  
### <a name="storage"></a>Depolama
  
**Ölçüm kimliği**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Ölçüm adı**: TableCapacity  
**Birim**: GB\*saat  
**Notlar**: Tablolar tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Ölçüm adı**: PageBlobCapacity  
**Birim**: GB\*saat  
**Notlar**: Sayfa Blobları tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Ölçüm adı**: QueueCapacity  
**Birim**: GB\*saat  
**Notlar**: Sıra tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Ölçüm adı**: Blok Blobkapasitesi  
**Birim**: GB\*saat  
**Notlar**: Blok Blobları tarafından tüketilen toplam kapasite.  
  
**Ölçüm kimliği**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Ölçüm adı**: TableTransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: Tablo hizmeti istekleri (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Ölçüm adı**: TableDataTransIn  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: GB cinsinden tablo hizmeti veri girişi.  
  
**Ölçüm kimliği**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Ölçüm adı**: TableDataTransOut  
**Birim**: GB cinsinden çıkış  
**Notlar**: GB cinsinden tablo hizmeti veri çıkışı.
  
**Ölçüm kimliği**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Ölçüm adı**: BlobTransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: Blob hizmeti istekleri (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Ölçüm adı**: BlobDataTransIn  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: GB cinsinden blob hizmeti veri girişi.  
  
**Ölçüm kimliği**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Ölçüm adı**: BlobDataTransOut  
**Birim**: GB cinsinden çıkış  
**Notlar**: GB cinsinden blob hizmeti veri çıkışı.  
  
**Ölçüm kimliği**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Ölçüm adı**: QueueTransactions  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: İstekleri Kuyruk hizmeti (10.000 'lik bloklar).  
  
**Ölçüm kimliği**: E518E809-E369-4A45-9274-2017B29FFF25  
**Ölçüm adı**: QueueDataTransIn  
**Birim**: GB cinsinden giriş verileri  
**Notlar**: Verilerin GB cinsinden Kuyruk hizmeti.  
  
**Ölçüm kimliği**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Ölçüm adı**: QueueDataTransOut  
**Birim**: GB cinsinden çıkış  
**Notlar**: GB cinsinden veri çıkışı Kuyruk hizmeti  

### <a name="compute"></a>İşlem
  
**Ölçüm kimliği**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Ölçüm adı**: Temel VM boyut saatleri  
**Birim**: Sanal çekirdek saatleri  
**Notlar**: Sanal çekirdek sayısı VM 'nin çalıştırıldığı saatlere göre çarpılır.  
  
**Ölçüm kimliği**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Ölçüm adı**: Windows VM boyut saatleri  
**Birim**: Sanal çekirdek saatleri  
**Notlar**: Sanal çekirdek sayısı, VM 'nin çalıştırıldığı saatlere göre çarpılır.  
  
**Ölçüm kimliği**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Ölçüm adı**: VM boyut saatleri  
**Birim**: VM saatleri  
**Notlar**: Hem temel hem de Windows VM yakalar. Çekirdekler için ayarlama yapmaz.  
  
### <a name="managed-disks"></a>Yönetilen Diskler

**Ölçüm kimliği**: 380874f9-300C-48e0-95a0-d2d9a21ade8f   
**Ölçüm adı**: S4   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Standart yönetilen disk-32 GB 

**Ölçüm kimliği**: 1b77d90f-427b-4435-b4f1-d78adec53222   
**Ölçüm adı**: S6   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Standart yönetilen disk-64 GB 

**Ölçüm kimliği**: d5f7731b-f639-404A-89d0-e46186e22c8d   
**Ölçüm adı**: S10   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Standart yönetilen disk-128 GB 

**Ölçüm kimliği**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a   
**Ölçüm adı**: S15   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Standart yönetilen disk-256 GB 

**Ölçüm kimliği**: 88ea9228-457A-4091-adc9-ad5194f30b6e   
**Ölçüm adı**: S20   
**Birim**: Disk\*ayı sayısı      
**Notlar**: Standart yönetilen disk-512 GB 

**Ölçüm kimliği**: 5b1db88a-8596-4002-8052-347947c26940   
**Ölçüm adı**: S30   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Standart yönetilen disk-1024 GB 

**Ölçüm kimliği**: 7660b45b-b29d-49cb-b816-59f30fbe011   
**Ölçüm adı**: P4   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-32 GB 

**Ölçüm kimliği**: 817007fd-a077-477f-BC01-b876f27205fd   
**Ölçüm adı**: P6   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-64 GB 

**Ölçüm kimliği**: e554b6bc-96cd-4938-a5b5-0dad990278519   
**Ölçüm adı**: P10   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-128 GB  

**Ölçüm kimliği**: cdc0f53a-62a9-4472-a06c-e99a23b02907   
**Ölçüm adı**: P15  
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-256 GB 

**Ölçüm kimliği**: b9cb2d1a-84C2-4275-aa8b-70d2145d59aa   
**Ölçüm adı**: P20   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-512 GB 

**Ölçüm kimliği**: 06bde724-9f94-43c0-84c3-d0fc54538369   
**Ölçüm adı**: P30   
**Birim**: Disk\*ayı sayısı   
**Notlar**: Premium yönetilen disk-1024 GB 

**Ölçüm kimliği**: 7ba084ec-ef9c-4d64-a179-7732c6cb5e28   
**Ölçüm adı**: ActualStandardDiskSize   
**Birim**: GB\*ay      
**Notlar**: Standart yönetilen diskin disk üzerindeki gerçek boyut  

**Ölçüm kimliği**: daef389a-06e5-4684-A7F7-8813d9f792d5  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: GB\*ay      
**Notlar**: Premium yönetilen disk diskteki gerçek boyut 

**Ölçüm kimliği**: 108fa95b-be0d-4cd9-96e8-5b0d59505df1  
**Ölçüm adı**: ActualStandardSnapshotSize   
**Birim**: GB\*ay   
**Notlar**: Yönetilen standart anlık görüntünün disk üzerindeki gerçek boyut.  

**Ölçüm kimliği**: 578ae51d-4ef9-42f9-85ae-42b52d3d83ac   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: GB\*ay   
**Notlar**: Yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut.   

**Ölçüm kimliği**: 5g76e09f-4567-452A-94cc-7d1f097761f0   
**Ölçüm adı**: S4   
**Birim**: Disk\*saati sayısı   
**Notlar**: Standart yönetilen disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Ölçüm adı**: S6   
**Birim**: Disk\*saati sayısı   
**Notlar**: Standart yönetilen disk-64 GB (kullanım dışı) 

**Ölçüm kimliği**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Ölçüm adı**: S10   
**Birim**: Disk\*saati sayısı   
**Notlar**: Standart yönetilen disk-128 GB (kullanım dışı) 

**Ölçüm kimliği**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Ölçüm adı**: S15   
**Birim**: Disk\*saati sayısı   
**Notlar**: Standart yönetilen disk-256 GB (kullanım dışı) 

**Ölçüm kimliği**: 5938f8dav-0ecd-4c48-8d5a-c7c6c23546be   
**Ölçüm adı**: S20   
**Birim**: Disk\*saati sayısı      
**Notlar**: Standart yönetilen disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Ölçüm adı**: S30   
**Birim**: Disk\*saati sayısı   
**Notlar**: Standart yönetilen disk-1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Ölçüm adı**: P4   
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: 518b412b-1927-4f25-985F-4aea24e55c4f   
**Ölçüm adı**: P6   
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-64 GB (kullanım dışı) 

**Ölçüm kimliği**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Ölçüm adı**: P10   
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-128 GB (kullanım dışı)  

**Ölçüm kimliği**: 8de91c94-F740-4d9a-b665-bd5974fa08d4   
**Ölçüm adı**: P15  
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-256 GB (kullanım dışı) 

**Ölçüm kimliği**: c7e7839c-293B-4761-ae4c-848eda91130b   
**Ölçüm adı**: P20   
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Ölçüm adı**: P30   
**Birim**: Disk\*saati sayısı   
**Notlar**: Premium yönetilen disk-1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Ölçüm adı**: ActualStandardDiskSize   
**Birim**: Bayt\*saatleri      
**Notlar**: Standart yönetilen diskin disk üzerindeki gerçek boyut (kullanım dışı)  

**Ölçüm kimliği**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: Bayt\*saatleri      
**Notlar**: Premium yönetilen disk diskteki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Ölçüm adı**: ActualStandardSnapshotSize   
**Birim**: Bayt\*saatleri   
**Notlar**: Yönetilen standart anlık görüntünün disk üzerindeki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 95b0c03f-8A82-4524-8961-ccfbf575f536   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: Bayt\*saatleri   
**Notlar**: Yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut (kullanım dışı) 

**Ölçüm kimliği**: 75d4b707-1027-4403-9986-6ec7c05579c8 **ölçüm adı**: ActualStandardSnapshotSize **birimi**: GB\*ay **notları**: Yönetilen standart anlık görüntünün disk üzerindeki gerçek boyut (kullanım dışı)  

**Ölçüm kimliği**: 5ca1cbb9-6f14-4E76-8be8-1ca91547965e **ölçer adı**: ActualPremiumSnapshotSize **birimi**: GB\*ay **notları**: Yönetilen Premium anlık görüntüsünün disk üzerindeki gerçek boyut (kullanım dışı)  

### <a name="sql-rp"></a>SQL RP
  
**Ölçüm kimliği**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Ölçüm adı**: DatabaseSizeHourSqlMeter  
**Birim**: MB\*saat  
**Notlar**: Oluşturma sırasında toplam DB kapasitesi. Kullanım API 'sini günlük bir ayrıntı düzeyi ile çağırırsanız, ölçüm MB değerini saat sayısıyla çarpılır.  
  
### <a name="mysql-rp"></a>MySql RP   
  
**Ölçüm kimliği**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Ölçüm adı**: DatabaseSizeHourMySqlMeter  
**Birim**: MB\*saat  
**Notlar**: Oluşturma sırasında toplam DB kapasitesi. Kullanım API 'sini günlük bir ayrıntı düzeyi ile çağırırsanız, ölçüm MB değerini saat sayısıyla çarpılır.    
### <a name="key-vault"></a>Key Vault   
  
**Ölçüm kimliği**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Ölçüm adı**: Key Vault işlemler  
**Birim**: 10000 ' de istek sayısı  
**Notlar**: Key Vault veri düzlemi tarafından alınan REST API isteklerinin sayısı.  
  
**Ölçüm kimliği**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Ölçüm adı**: Gelişmiş Anahtarlar işlemleri  
**Birim**:  10.000 işlem  
**Notlar**: RSA 3K/4K, ECC anahtar işlemleri. (Önizleme).  
  
### <a name="app-service"></a>App Service   
  
**Ölçüm kimliği**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Ölçüm adı**: App Service  
**Birim**: Sanal çekirdek saatleri  
**Notlar**: App Service 'i çalıştırmak için kullanılan sanal çekirdek sayısı. Not: Microsoft, Azure Stack App Service ücretlendirmesi için bu ölçümü kullanır. Bulut hizmeti sağlayıcıları, kiracılarının kullanımını hesaplamak için diğer App Service ölçüleri (aşağıda) kullanabilir.  
  
**Ölçüm kimliği**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Ölçüm adı**: İşlev Istekleri  
**Birim**: 10 istek  
**Notlar**: Toplam istenen yürütmeler sayısı (10 yürütme başına). Bir olaya yanıt olarak bir işlev çalıştırıldığında yürütmeler sayılır veya bir bağlama tarafından tetiklenir.  
  
**Ölçüm kimliği**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Ölçüm adı**: İşlevler-Işlem  
**Birim**:  GB-s  
**Notlar**:  Gigabayt saniye (GB/sn) cinsinden ölçülen kaynak tüketimi. **Gözlemlenen kaynak tüketimi** , işlevi yürütmek için geçen milisaniye cinsinden ortalama bellek boyutu GB cinsinden çarpılarak hesaplanır. Bir işlev tarafından kullanılan bellek en yakın 128 MB 'a kadar, en yakın 1 MS 'ye kadar yuvarlanan yürütme süresi olan 1.536 MB 'lık maksimum bellek boyutuna kadar ölçülür. Tek bir işlev yürütmesi için en düşük yürütme süresi ve bellek, sırasıyla 100 ms ve 128 MB 'dir.  
  
**Ölçüm kimliği**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Ölçüm adı**: Paylaşılan App Service saatleri  
**Birim**: 1 saat  
**Notlar**: Parça App Service planının kullanım başına kullanımı. Planlar, uygulama başına temelinde Ücretlendirilebilir.  
  
**Ölçüm kimliği**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Ölçüm adı**: Ücretsiz App Service saat  
**Birim**: 1 saat  
**Notlar**: Ücretsiz App Service plan kullanım başına saat. Planlar, uygulama başına temelinde Ücretlendirilebilir.  
  
**Ölçüm kimliği**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Ölçüm adı**: Küçük Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: Boyut ve örnek sayısına göre hesaplanır.  
  
**Ölçüm kimliği**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Ölçüm adı**: Orta Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: Boyut ve örnek sayısına göre hesaplanır.  
  
**Ölçüm kimliği**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Ölçüm adı**: Büyük Standart App Service saatleri  
**Birim**: 1 saat  
**Notlar**: Boyut ve örnek sayısına göre hesaplanır.  
  
### <a name="custom-worker-tiers"></a>Özel çalışan katmanları   
  
**Ölçüm kimliği**: *Özel çalışan katmanları*  
**Ölçüm adı**: Özel çalışan katmanları  
**Birim**: Saat  
**Notlar**: Belirleyici ölçüm KIMLIĞI, SKU ve özel çalışan katmanı adına göre oluşturulur. Bu ölçüm KIMLIĞI her özel çalışan katmanı için benzersizdir.  
  
**Ölçüm kimliği**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Ölçüm adı**: SNI SSL  
**Birim**: SNI SSL bağlama başına  
**Notlar**: App Service iki tür SSL bağlantısını destekler: Sunucu Adı Belirtme (SNI) SSL Bağlantıları ve IP Adresi SSL Bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Ölçüm adı**: IP SSL  
**Birim**: IP tabanlı SSL bağlama başına  
**Notlar**: App Service iki tür SSL bağlantısını destekler: Sunucu Adı Belirtme (SNI) SSL Bağlantıları ve IP Adresi SSL Bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Ölçüm adı**:  Web İşlemi  
**Birim**:  
**Notlar**: Etkin site başına saat başına hesaplanır.  
  
**Ölçüm kimliği**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Ölçüm adı**: Dış çıkış bant genişliği  
**Birim**: GB  
**Notlar**: Gelen toplam istek yanıtı bayt sayısı + toplam giden istek baytı + toplam gelen FTP isteği yanıt baytı + toplam gelen Web dağıtımı isteği yanıt baytı.  
  
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
