---
title: Azure Stack için depolama altyapısını yönetme | Microsoft Docs
description: Azure Stack için depolama altyapısını yönetin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: article
ms.date: 08/27/2019
ms.author: mabrigg
ms.lastreviewed: 03/11/2019
ms.reviewer: jiahan
ms.openlocfilehash: c2a61e165ee64c4b0ee91fbe387973aaa5039f46
ms.sourcegitcommit: 9cb82df1eccb0486bcabec0bd674162d4820c00c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70060210"
---
# <a name="manage-storage-infrastructure-for-azure-stack"></a>Azure Stack için depolama altyapısını yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede Azure Stack depolama altyapısı kaynaklarının sistem durumu ve işletimsel durumu açıklanır. Bu kaynaklar depolama sürücüleri ve birimleri içerir. Bu konudaki bilgiler, bir sürücü gibi çeşitli sorunları gidermeye çalışırken çok değerli olabilir.

## <a name="understand-drives-and-volumes"></a>Sürücüleri ve birimleri anlama

### <a name="drives"></a>Disk

Windows Server yazılımıyla desteklenen Azure Stack, bir performans, ölçeklenebilir ve dayanıklı depolama hizmeti sağlamak için Depolama Alanları Doğrudan (S2D) ve Windows Server Yük Devretme Kümelemesi birleşimiyle depolama özelliklerini tanımlar.

Azure Stack tümleşik sistem ortakları, geniş kapsamlı depolama esnekliği dahil olmak üzere çok sayıda çözüm çeşitlemelerini sunmaktadır. Şu anda üç sürücü türünün birleşimini seçebilirsiniz: NVMe (geçici olmayan bellek Express), SATA/SAS SSD (katı hal sürücüsü), HDD (sabit disk sürücüsü).

Depolama Alanları Doğrudan depolama performansını en üst düzeye çıkarmak için bir önbellek sunar. Tek veya birden çok sürücü türü olan Azure Stack gereç içinde, depolama alanları doğrudan, önbelleğe alma için "en hızlı" (NVMe &gt; SSD &gt; HDD) türünün tüm sürücülerinizi otomatik olarak kullanır. Kalan sürücüler kapasite için kullanılır. Sürücüler "All-Flash" veya "hibrit" dağıtımında gruplandırılabilir:

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image1.png)

Tüm-Flash dağıtımları depolama performansını en üst düzeye çıkarmak ve rotasyonlu sabit disk sürücüleri (HDD) içermez.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image2.png)

Karma dağıtımlar performans ve kapasiteyi dengelemek ya da kapasiteyi en üst düzeye çıkarmak ve rotasyonlu sabit disk sürücüleri (HDD) dahil etmek için hedeflenir.

Önbelleğin davranışı, önbelleğe alınan sürücülerin türlerine göre otomatik olarak belirlenir. Katı hal sürücüleri (SSD 'Ler için NVMe önbelleğe alma gibi) önbelleğe alırken yalnızca yazma işlemleri önbelleğe alınır. Bu, kapasite sürücülerinde aşı azaltır, bu da kapasite sürücülerine yönelik kümülatif trafiği azaltır ve yaşam sürelerini genişletmelidir. Bu arada, okuma, Flash 'un ömrü önemli ölçüde etkilemediğinden ve katı hal sürücüleri evrensel olarak düşük okuma gecikmesi sağladığından, okumalar önbelleğe alınmaz. Sabit disk sürücüleri (HDD 'Ler için SSDs önbelleği gibi) için önbelleğe alırken hem okuma hem de yazma işlemleri, her ikisi için de Flash benzeri gecikme süresi (genellikle/~ 10X daha iyi) sağlamak üzere önbelleğe alınır.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image3.png)

Depolama alanının kullanılabilir yapılandırması için, Azure Stack OEM iş ortağı (https://azure.microsoft.com/overview/azure-stack/partners/) ayrıntılı belirtim için) kontrol edebilirsiniz.

> [!Note]  
> Azure Stack gereç, hem HDD hem de SSD (veya NVMe) sürücülerle karma bir dağıtımda teslim edilebilir. Ancak daha hızlı türdeki sürücüler önbellek sürücüleri olarak kullanılır ve kalan tüm sürücüler havuz olarak kapasite sürücüleri olarak kullanılır. Kiracı verileri (blob 'lar, tablolar, kuyruklar ve diskler) kapasite sürücülerine yerleştirilir. Bu nedenle, Premium diskleri sağlama veya Premium depolama hesabı türü seçme, nesnelerin SSD veya NVMe sürücülerinde ayrılmaları garanti edilir ve daha fazla performans elde edilir.

### <a name="volumes"></a>Birimler

*Depolama hizmeti* , kullanılabilir depolamayı sistem ve kiracı verilerini tutmak için ayrılan ayrı birimlerde bölümler. Birimler, Depolama Alanları Doğrudan hata toleransı, ölçeklenebilirlik ve performans avantajlarını tanıtmak için depolama havuzundaki sürücüleri birleştirir.

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image4.png)

Azure Stack depolama havuzunda oluşturulan üç tür birim vardır:

-   Altyapı: Azure Stack altyapısı VM 'Leri ve Çekirdek Hizmetleri tarafından kullanılan ana bilgisayar dosyaları.

-   VM geçici: Kiracı VM 'lerine bağlı geçici diskleri barındırın ve bu veriler bu disklerde saklanır.

-   Nesne deposu: kiracı veri Bakımı blob 'ları, tabloları, kuyrukları ve VM disklerini barındırın.

Çok düğümlü bir dağıtımda, üç altyapı birimi görürsünüz, ancak VM geçici birimlerinin ve nesne deposu birimlerinin sayısı Azure Stack dağıtımındaki düğümlerin sayısına eşittir:

-   Dört düğümlü bir dağıtımda, dört adet eşit VM geçici birimi ve dört eşit nesne deposu birimi vardır.

-   Kümeye yeni bir düğüm eklerseniz, her iki tür için de yeni bir birim oluşturulur.

-   Bir düğüm arızalı veya kaldırılmış olsa bile birim sayısı aynı kalır.

-   Azure Stack geliştirici setini kullanıyorsanız, birden çok paylaşım içeren tek bir birim vardır.

Depolama Alanları Doğrudan birimler, sürücü veya sunucu hataları gibi donanım sorunlarına karşı koruma sağlamak ve yazılım güncelleştirmeleri gibi sunucu bakımı genelinde sürekli kullanılabilirliği etkinleştirmek için dayanıklılık sağlar. Azure Stack dağıtımı, Data esnekliği sağlamak için üç yönlü yansıtma kullanıyor. Kiracı verilerinin üç kopyası, önbellekte yer aldıkları farklı sunuculara yazılır:

![Azure Stack depolama altyapısı](media/azure-stack-storage-infrastructure-overview/image5.png)

Yansıtma, tüm verilerin birden çok kopyasını tutarak hata toleransı sağlar. Bu verilerin nasıl şeritlenmiştir ve yerleştirilebileceği, ancak yansıtma kullanılarak depolanan verilerin tamamen, birden çok kez yazıldığı söylenmiştir. Her kopya, bağımsız olarak başarısız olarak kabul edilen farklı fiziksel donanıma (farklı sunuculardaki farklı sürücülere) yazılır. Üç yönlü yansıtma, aynı anda en az iki donanım sorununu (sürücü veya sunucu) güvenle kabul edebilir. Örneğin, aniden başka bir sürücü veya sunucu başarısız olduğunda bir sunucuyu yeniden yüklüyorsanız, tüm veriler güvenli ve sürekli olarak erişilebilir durumda kalır.

## <a name="volume-states"></a>Birim durumları

Hangi durum birimlerinin içinde olduğunu öğrenmek için aşağıdaki PowerShell komutlarını kullanın:

```powershell
$scaleunit_name = (Get-AzsScaleUnit)[0].name

$subsystem_name = (Get-AzsStorageSubSystem -ScaleUnit $scaleunit_name)[0].name

Get-AzsVolume -ScaleUnit $scaleunit_name -StorageSubSystem $subsystem_name | Select-Object VolumeLabel, HealthStatus, OperationalStatus, RepairStatus, Description, Action, TotalCapacityGB, RemainingCapacityGB
```

Ayrılmış bir birimi ve düşürülmüş/tamamlanmamış bir birimi gösteren bir çıktı örneği aşağıda verilmiştir:

| VolumeLabel | HealthStatus | OperationalStatus |
|-------------|--------------|------------------------|
| ObjStore_1 | Bilinmiyor | Ayrılmış |
| ObjStore_2 | Uyarı | {Azaltılmış, tamamlanmamış} |

Aşağıdaki bölümlerde sistem durumu ve işletimsel durumlar listelenmektedir.

### <a name="volume-health-state-healthy"></a>Birim sistem durumu: Sorunsuz

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tamam | Birim sağlıklı. |
| Yetersiz | Veriler, sürücüler arasında eşit olarak yazılmaz.<br> <br>**Ön** Depolama havuzundaki sürücü kullanımını iyileştirmek için lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. Başarısız bağlantı geri yüklendikten sonra yedekten geri yükleme yapmanız gerekebilir. |


### <a name="volume-health-state-warning"></a>Birim sistem durumu: Uyarı

Birim bir uyarı sistem durumundaysa, verilerinizin bir veya daha fazla kopyasının kullanılamadığı anlamına gelir, ancak Azure Stack verilerinizin en az bir kopyasını okuyabilirler.

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hizmette | Azure Stack, bir sürücü eklendikten veya kaldırıldıktan sonra olduğu gibi birimi onarıyor. Onarım tamamlandığında, birim, tamam sistem durumuna döndürmelidir.<br> <br>**Ön** Azure Stack birimi onarmayı bitirmesini bekleyin ve durumu daha sonra denetleyin. |
| Eksik | Bir veya daha fazla sürücü başarısız olduğu veya eksik olduğu için birimin esnekliği düzeyi azaltılmıştır. Ancak, eksik sürücüler verilerinizin güncel kopyalarını içerir.<br> <br>**Ön** Eksik sürücüleri yeniden bağlayın, tüm başarısız sürücüleri değiştirin ve çevrimdışı olan tüm sunucuları çevrimiçi hale getirin. |
| Düşürüldü | Bir veya daha fazla sürücü başarısız olduğu veya eksik olduğu için birimin esnekliği düşürüldü ve bu sürücülerde verilerinizin güncel olmayan kopyaları var.<br> <br>**Ön** Eksik sürücüleri yeniden bağlayın, tüm başarısız sürücüleri değiştirin ve çevrimdışı olan tüm sunucuları çevrimiçi hale getirin. |

 

### <a name="volume-health-state-unhealthy"></a>Birim sistem durumu: İyi durumda değil

Bir birim sağlıksız bir sistem durumundaysa, birimdeki verilerin bazılarına veya tümüne Şu anda erişilemiyor.

| İşlemsel durum | Açıklama |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Artıklık yok | Çok fazla sürücü başarısız olduğundan birim veri kaybetti.<br> <br>**Ön** Lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. |


### <a name="volume-health-state-unknown"></a>Birim sistem durumu: Bilinmiyor

Sanal disk ayrılmışsa birim bilinmeyen sistem durumunda da olabilir.

| İşlemsel durum | Açıklama |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ayrılmış | Birimin erişilemez olmasına neden olabilecek bir depolama cihazı hatası oluştu. Bazı veriler kaybolabilir.<br> <br>**Ön** <br>1. Tüm depolama cihazlarının fiziksel ve ağ bağlantısını denetleyin.<br>2. Tüm cihazlar doğru bağlandıysa, lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. Başarısız bağlantı geri yüklendikten sonra yedekten geri yükleme yapmanız gerekebilir. |

## <a name="drive-states"></a>Sürücü durumları

Sürücülerin durumunu izlemek için aşağıdaki PowerShell komutlarını kullanın:

```powershell
$scaleunit_name = (Get-AzsScaleUnit)[0].name

$subsystem_name = (Get-AzsStorageSubSystem -ScaleUnit $scaleunit_name)[0].name

Get-AzsDrive -ScaleUnit $scaleunit_name -StorageSubSystem $subsystem_name | Select-Object StorageNode, PhysicalLocation, HealthStatus, OperationalStatus, Description, Action, Usage, CanPool, CannotPoolReason, SerialNumber, Model, MediaType, CapacityGB
```

Aşağıdaki bölümlerde bir sürücünün içinde bulunabileceği sistem durumları açıklanır.

### <a name="drive-health-state-healthy"></a>Sürücü durumu: Sorunsuz

| İşlemsel durum | Açıklama |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Tamam | Birim sağlıklı. |
| Hizmette | Sürücü, bazı iç temizlik işlemleri gerçekleştiriyor. Eylem tamamlandığında, sürücü, tamam sistem durumuna döndürmelidir. |

### <a name="drive-health-state-healthy"></a>Sürücü durumu: Sorunsuz

Uyarı durumundaki bir sürücü verileri okuyabilir ve yazabilir, ancak bir sorun olabilir.

| İşlemsel durum | Açıklama |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İletişim kesildi | Sürücüye bağlantı kaybedildi.<br> <br>**Ön** Tüm sunucuları yeniden çevrimiçi duruma getirin. Bu dosyayı düzelmezse sürücüyü yeniden bağlayın. Bu durum devam ederse, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Tahmine dayalı hata | Sürücünün bir başarısızlığı yakında gerçekleşecek şekilde tahmin edilir.<br> <br>**Ön** Tam dayanıklılık sağlamak için sürücüyü mümkün olan en kısa sürede değiştirin. |
| GÇ hatası | Sürücüye erişilirken geçici bir hata oluştu.<br> <br>**Ön** Bu durum devam ederse, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Geçici hata | Sürücüde geçici bir hata oluştu. Bu genellikle sürücünün yanıt vermediği anlamına gelir, ancak Depolama Alanları Doğrudan koruyucu bölümünün sürücüden uygun şekilde kaldırıldığı anlamına da gelebilir. <br> <br>**Ön** Bu durum devam ederse, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Olağan dışı gecikme | Sürücü bazen yanıt vermiyor ve hata işaretlerini gösteriyor.<br> <br>**Ön** Bu durum devam ederse, tam dayanıklılık sağlamak için sürücüyü değiştirin. |
| Havuzdan kaldırılıyor | Azure Stack, sürücüyü depolama havuzundan kaldırma sürecinizdir.<br> <br>**Ön** Azure Stack sürücüyü kaldırma işleminin bitmesini bekleyin ve durumu daha sonra denetleyin.<br>Durum kalırsa, lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. |
|  |  |
| Bakım modu başlatılıyor | Azure Stack, sürücüyü bakım moduna getirme sürecinizdeki bir işlemdir. Bu geçici bir durumdur. sürücü yakında bakım modu durumunda olmalıdır.<br> <br>**Ön** Azure Stack işlemin bitmesini bekleyin ve ardından durumu daha sonra denetleyin. |
| Bakım modunda | Sürücü bakım modunda olduğundan, sürücüden okuma ve yazma işlemleri durduruluyor. Bu genellikle, PNU veya FRU gibi yönetim görevlerinin sürücüyü Azure Stack anlamına gelir. Ancak yönetici Ayrıca sürücüyü bakım moduna geçirebilir.<br> <br>**Ön** Azure Stack yönetim görevinin bitmesini bekleyip durumu daha sonra denetleyin.<br>Durum kalırsa, lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. |
|  |  |
| Bakım modu durduruluyor | Azure Stack, sürücüyü yeniden çevrimiçi hale getirme sürecidir. Bu geçici bir durumdur. sürücünün yakında daha Iyi bir durumda olması gerekir.<br> <br>**Ön** Azure Stack işlemin bitmesini bekleyin ve ardından durumu daha sonra denetleyin. |

 

### <a name="drive-health-state-unhealthy"></a>Sürücü durumu: İyi durumda değil

Sağlıksız durumdaki bir sürücü şu anda yazılamaz veya erişilemez durumda olamaz.

| İşlemsel durum | Açıklama |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Böl | Sürücü havuzdan ayrılmış hale geldi.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. Bu diski kullanmanız gerekiyorsa, diski sistemden kaldırın, diskte yararlı bir veri olmadığından emin olun, diski silin ve sonra diski yeniden takın. |
| Kullanılamaz | Fiziksel disk, çözüm satıcınız tarafından desteklenmediğinden karantinaya alındı. Yalnızca çözüm için onaylanan ve doğru disk üretici yazılımına sahip olan diskler desteklenir.<br> <br>**Ön** Sürücüyü, çözüm için onaylanmış bir üretici ve model numarası olan bir diskle değiştirin. |
| Eski meta veriler | Değiştirilen disk daha önce kullanılmış ve bilinmeyen bir depolama sisteminden veri içeriyor olabilir. Disk karantinaya alındı.        <br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. Bu diski kullanmanız gerekiyorsa, diski sistemden kaldırın, diskte yararlı bir veri olmadığından emin olun, diski silin ve sonra diski yeniden takın. |
| Tanınmayan meta veriler | Sürücüde tanınmayan meta veriler bulundu, bu, genellikle sürücüde bulunan farklı bir havuzdan meta veriler olduğu anlamına gelir.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. Bu diski kullanmanız gerekiyorsa, diski sistemden kaldırın, diskte yararlı bir veri olmadığından emin olun, diski silin ve sonra diski yeniden takın. |
| Başarısız medya | Sürücü başarısız oldu ve artık depolama alanları tarafından kullanılmayacak.<br> <br>**Ön** Tam dayanıklılık sağlamak için sürücüyü mümkün olan en kısa sürede değiştirin. |
| Cihaz donanım hatası | Bu sürücüde bir donanım hatası vardı. <br> <br>**Ön** Tam dayanıklılık sağlamak için sürücüyü mümkün olan en kısa sürede değiştirin. |
| Üretici yazılımı güncelleştiriliyor | Azure Stack, sürücüdeki üretici yazılımını güncelleştiriyor. Bu, genellikle bir dakikadan kısa bir süre boyunca ve havuzdaki diğer sürücülerin tüm okuma ve yazma işlemlerini işleyeceği geçici bir durumdur.<br> <br>**Ön** Azure Stack güncelleştirmeyi tamamlamasını bekleyin ve sonra durumu kontrol edin. |
| Başlatılıyor | Sürücü işleme hazırlanıyor. Bu geçici bir durum olmalıdır. tamamlandıktan sonra sürücü farklı bir işlemsel duruma geçiş yapılmalıdır.<br> <br>**Ön** Azure Stack işlemin bitmesini bekleyin ve ardından durumu daha sonra denetleyin. |
 

## <a name="reasons-a-drive-cant-be-pooled"></a>Bir sürücünün havuza alınamıyor nedenleri

Bazı sürücüler yalnızca Azure Stack depolama havuzunda olmaya hazırız. Bir sürücünün CannotPoolReason özelliğine bakarak, bir sürücünün neden havuza uygun olmadığını öğrenebilirsiniz. Aşağıdaki tabloda nedenlerden her biri hakkında biraz daha ayrıntılı bilgi verilmektedir.

| Reason | Açıklama |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Donanım uyumlu değil | Sürücü, Sistem Sağlığı Hizmeti kullanılarak belirtilen onaylanan depolama modelleri listesinde değildir.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. |
| Bellenim uyumlu değil | Fiziksel sürücüdeki üretici yazılımı, Sistem Sağlığı Hizmeti kullanılarak onaylanan bellenim düzeltmeleri listesinde değildir.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. |
| Küme tarafından kullanılıyor | Sürücü şu anda bir yük devretme kümesi tarafından kullanılıyor.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. |
| Çıkarılabilir medya | Sürücü çıkarılabilir bir sürücü olarak sınıflandırılır. <br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. |
| Sağlıklı durumda değil | Sürücü sağlıklı durumda değil ve değiştirilmelidir.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. |
| Yetersiz kapasite | Sürücüdeki boş alanı alan bölümler vardır.<br> <br>**Ön** Sürücüyü yeni bir diskle değiştirin. Bu diski kullanmanız gerekiyorsa, diski sistemden kaldırın, diskte yararlı bir veri olmadığından emin olun, diski silin ve sonra diski yeniden takın. |
| Doğrulama devam ediyor | Sistem Sağlığı Hizmeti, sürücüdeki sürücünün veya bellenimin kullanım için onaylandığını denetlemek için kontrol edilir.<br> <br>**Ön** Azure Stack işlemin bitmesini bekleyin ve ardından durumu daha sonra denetleyin. |
| Doğrulama başarısız oldu | Sistem Sağlığı Hizmeti sürücüdeki sürücünün veya bellenimin kullanım için onaylandığından emin olup olmadığını denetlemedi.<br> <br>**Ön** Lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. |
| Çevrimdışı | Sürücü çevrimdışı. <br> <br>**Ön** Lütfen desteğe başvurun. Bunu yapmadan önce, ' den gelen https://aka.ms/azurestacklogfiles Kılavuzu kullanarak günlük dosyası toplama işlemini başlatın. |

## <a name="next-step"></a>Sonraki adım

[Depolama kapasitesini yönetme](azure-stack-manage-storage-shares.md) 
