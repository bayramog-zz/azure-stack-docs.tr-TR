---
title: Azure Stack kapasite planlama depolama | Microsoft Docs
description: Kapasite planlaması için Azure Stack dağıtımları hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: prchint
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 2845a90f97c1b859269f73333448bf42ff699da9
ms.sourcegitcommit: b79a6ec12641d258b9f199da0a35365898ae55ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131082"
---
# <a name="azure-stack-storage"></a>Azure Stack depolama

Aşağıdaki bölümler, çözümün depolama gereksinimlerini planlama stratejilerinde destek olmak için Azure Stack depolama kapasitesini planlama bilgileri sağlar.

## <a name="uses-and-organization-of-storage-capacity"></a>Kullanır ve kuruluş depolama kapasitesi
Fiziksel depolama cihazları paylaşmak için Azure Stack hiper yakınsanmış yapılandırmasını sağlar. Üç ana bölüm paylaşılabilir kullanılabilir depolama alanı vardır: altyapı ve Kiracı sanal makinelerinin geçici depolama blobları, tablolar ve Kuyruklar Azure tutarlı depolama (ACS) Hizmetleri, yedekleme depolama.

## <a name="storage-spaces-direct-cache-and-capacity-tiers"></a>Depolama alanları doğrudan önbellek ve kapasite katmanları
Depolama kapasitesi gereksinimlerini işletim sistemi, yerel günlüğe kaydetme, dökümleri ve diğer altyapı geçici depolama için kullanılan yoktur. Depolama alanları doğrudan Yapılandırması Yönetim altına duruma depolama aygıtlarını (cihazlar ve kapasite) ayırmak bu yerel depolama kapasitesidir. Depolama aygıtlarını kalanını tek bir ölçek birimindeki sunucularının sayısından bağımsız olarak depolama kapasitesi havuzu yerleştirilir.

Bu cihazları iki türü şunlardır: önbellek ve kapasite. Depolama alanları doğrudan geri yazma ve okuma önbelleği için önbellek cihazları kullanır. Bu önbellek cihazları kapasiteleri kullanılabilir ancak, biçimlendirilmiş, "visible" biçimlendirilmiş sanal disklerin kapasitesi taahhüt değildir. Aksine, depolama alanları doğrudan kapasite cihazları bu amaç için "home" yönetilen verilerin konumundan sağlama kullanın.

Azure Stack altyapısının doğrudan ayırır ve tüm depolama kapasitesi yönetir. İşleci, yapılandırma, ayırma, kapasite genişletmesi hakkında seçimler gerekmez. Azure Stack ile çözüm gereksinimleri, ilk yükleme ve dağıtım ya da kapasite genişletmesi sırasında hizalamak için aşağıdaki tasarım kararlarını otomatikleştirir. Azure Stack tasarımının bir parçası göz önünde bulundurarak dayanıklılık, yeniden oluşturulması için ayrılmış kapasite ve diğer ayrıntıları alır. 

İşleçleri arasında ya da seçim yapabileceğiniz bir *tüm flash* veya *karma* depolama yapılandırması:

![Azure depolama kapasitesini planlama diyagramı](media/azure-stack-capacity-planning/storage.png)

Tümü flash yapılandırmada, iki katmanlı veya bir tek katmanlı yapılandırma yapılandırma olabilir. Tek katmanlı yapılandırma ise aynı türde (örneğin, NVMe veya SATA SSD veya SAS SSD) tüm kapasite cihazları olduğunu ve önbellek cihazları kullanılmaz. İki katmanlı tüm, flash, tipik configuration yapılandırmadır NVMe önbellek cihazları ve ardından her iki SATA veya SAS SSD kapasite cihazları olarak.

Karma, iki katmanlı yapılandırma önbelleği NVMe, SATA veya SAS SSD arasından bir seçim değil ve HDD kapasitesidir. 

Kısa bir özeti depolama alanları doğrudan ve Azure Stack depolama yapılandırması aşağıdaki gibidir:
- Bir depolama alanları doğrudan havuz başına ölçek birimi (tüm depolama cihazları içinde tek bir havuz yapılandırılır).
- Sanal diskler, en iyi performans ve dayanıklılık için üç kopyalama yansıtma olarak oluşturulur.
- Her sanal disk bir ReFS dosya sistemi olarak biçimlendirilir.
- Sanal disk kapasitesi hesaplanır ve veri kapasitesi havuzu ayrılmamış bir kapasite cihazın miktarını bırakmak için farklı bir şekilde ayrılmış. Bu sunucu başına tek bir kapasite sürücü eşdeğerdir.
- Her ReFS dosya sistemi BitLocker için bekleyen veri şifrelemesi etkin sahiptir. 

Otomatik olarak oluşturulan sanal diskler ve kapasitelerini, aşağıdaki gibidir:

|Ad|Kapasite hesaplama|Açıklama|
|-----|-----|-----|
|Yerel/önyükleme aygıtı|En az 340 GB<sup>1</sup>|Tek bir sunucu işletim sistemi görüntüleri ve "yerel" altyapı Vm'leri için depolama alanı.|
|Altyapı|3,5 TB|Tüm Azure Stack altyapısını kullanımı.|
|VmTemp|Aşağıya bakın<sup>2</sup>|Kiracı sanal makinelerinizin bağlı olarak geçici bir diskle ve bu verileri bu sanal diskler depolanır.|
|ACS|Aşağıya bakın <sup>3</sup>|Bloblar, tablolar ve Kuyruklar bakım için azure tutarlı depolama kapasitesi.|

<sup>1</sup> Azure Stack çözüm iş ortağı gerekli en düşük depolama kapasitesi.

<sup>2</sup> Kiracı sanal makine geçici diskler için kullanılan sanal disk boyutu, sunucunun fiziksel bellek oranı hesaplanır. Geçici diskin bir sanal makineye atanan fiziksel bellek oranıdır. Azure stack'teki depolama "disk temp" Bitti ayırma, çoğu kullanım örnekleri yakalar, ancak tüm geçici disk depolama gereksinimlerini karşılamak mümkün olmayabilir. Geçici depolama kullanılabilir hale getirir ve çözümü yalnızca temp disk kapasitesi için depolama kapasitesi çoğunu tüketen değil arasında bir denge oranıdır. Bir geçici depolama diskini, Ölçek birimindeki sunucu başına oluşturulur. Geçici depolama kapasitesini ölçek birimi depolama havuzundaki genel olarak kullanılabilir depolama kapasitesinin yüzde 10 ötesinde büyütün değil. Hesaplama aşağıdaki örnekte olduğu gibi bir şeydir:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> ACS tarafından kullanılmak üzere oluşturulan sanal diskler kalan kapasite basit bir bölümü olan. Belirtildiği gibi tüm sanal disklerin üç yönlü yansıtma ve her sunucu için kapasite tutarında bir kapasite sürücünün ayrılmamış. Daha önce listelenmiş çeşitli sanal diskler ilk ayrılır ve kalan kapasite sonra ACS sanal diskler için kullanılır.


## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure Stack kapasite Planlayıcısı](azure-stack-capacity-planner.md).
