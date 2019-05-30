---
title: Azure stack'teki performansını iyileştirmek için en iyi uygulamalar SQL Server. | Microsoft Docs
description: Bu makalede, performansı artırmak ve Azure Stack Vm'lerde SQL Server en iyi duruma getirmek için SQL server en iyi uygulamalar sağlanır.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: deed8e358c339e5a55cf2928002b9c0e6910f0d4
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269432"
---
# <a name="sql-server-best-practices-to-optimize-performance-in-azure-stack"></a>Azure stack'teki performansını iyileştirmek için SQL server en iyi uygulamalar

Bu makalede, SQL Server en iyi duruma getirmek ve Microsoft Azure Stack sanal makineleri (VM'ler) performansını artırmak için SQL server en iyi uygulamalar sağlanır. Azure Stack Vm'lerde SQL Server çalıştıran, aynı veritabanı performans ayarlama seçenekleri bir şirket içi sunucu ortamında SQL Server için geçerli kullanın. Azure Stack bulut ilişkisel bir veritabanında performansını bir VM boyutu ailesi ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

SQL Server görüntülerini oluştururken [Vm'lerinizi Azure Stack portalında sağlamayı göz önünde bulundurun](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). Azure Stack Yönetici portalında Market yönetimden SQL Iaas uzantısı'nı indirin ve kendi seçtiğiniz SQL VM sanal sabit diskleri (VHD'ler) indirin. Bunlar, SQL2014SP2 SQL2016SP1 ve SQL2017 içerir.

> [!NOTE]  
> Makale genel Azure portalını kullanarak bir SQL Server VM'si sağlama işlemini açıklamaktadır, ancak kılavuzu aşağıdaki farklar ile Azure Stack için de geçerlidir: SSD işletim sistemi diski için kullanılamaz, yönetilen diskleri kullanılabilir değil ve depolama yapılandırması küçük farklılıklar vardır.

Başlama *en iyi* performansı için Azure Stack vm'lerde SQL Server, bu makalenin odak noktası. İş yükünüzü daha az zorlu ise, önerilen tüm en iyi duruma getirme gerekmeyebilir. Bu önerileri değerlendirin gibi iş yükü ve performans gereksinimlerini göz önünde bulundurun.

> [!NOTE]  
> Azure Vm'lerde SQL Server için performans yönergeleri için bkz [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).

## <a name="checklist-for-sql-server-best-practices"></a>SQL server en iyi uygulamaları denetim listesi

Azure Stack sanal makinelerinde SQL Server'ın en iyi performans için aşağıdaki denetim listesi verilmiştir:


|Alan|En iyi duruma getirme|
|-----|-----|
|VM boyutu |[DS3](azure-stack-vm-sizes.md) veya SQL Server Enterprise edition için daha yüksek.<br><br>[DS2](azure-stack-vm-sizes.md) veya SQL Server Standard edition ve Web edition için daha yüksek.|
|Depolama |Destekleyen bir VM ailesinin kullanılacağını [Premium depolama](azure-stack-acs-differences.md).|
|Diskler |En az iki veri diski (bir günlük dosyaları için) ve bir veri dosyası ve TempDB kullanın ve kapasite gereksinimlerinize göre disk boyutu seçin. Varsayılan veri dosyası konumları, SQL Server yüklemesi sırasında bu disklere ayarlayın.<br><br>Veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br>Depolama alanları'nı kullanarak daha yüksek g/ç aktarım hızı alma için birden çok Azure veri diski stripe.<br><br>Belgelenen ayırma boyutları ile biçimlendirin.|
|G/Ç|Veri dosyaları için anında dosya başlatma etkinleştirin.<br><br>Otomatik büyüme veritabanlarında makul ölçüde kısa sabit artışlarla (64 MB - 256 MB) ile sınırlayın.<br><br>Otomatik olarak küçültme veritabanında devre dışı bırakın.<br><br>Veri diskleri, işletim sistemi diski varsayılan yedekleme ve veritabanı dosyası konumlarının ayarlayın.<br><br>Kilitli sayfalar sağlar.<br><br>SQL Server hizmet paketlerini ve toplu güncelleştirmeleri uygulayın.|
|Özelliğe özgü|(Kullanılan SQL Server sürümü tarafından destekleniyorsa) doğrudan blob depolama alanına yedekleyin.|
|||

Daha fazla bilgi için *nasıl* ve *neden* bu iyileştirmeler yapmak için Ayrıntılar ve aşağıdaki bölümlerde verilen yönergeleri gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyutu Kılavuzu

Aşağıdaki performans açısından duyarlı uygulamalar için [VM boyutları](azure-stack-vm-sizes.md) önerilir:

- **SQL Server Enterprise sürümü:** DS3 veya üzeri

- **SQL Server Standard edition ve Web edition:** DS2 veya üzeri

Azure Stack ile DS ve DS_v2 VM ailesi seriler arasında performans farkı yoktur.

## <a name="storage-guidance"></a>Depolama yönergeleri

Azure stack'teki (birlikte, DSv2 serisi) DS serisi VM'ler, en yüksek işletim sistemi diski ve veri disk aktarım hızı (IOPS) sağlayın. İşletim sistemi diski ve 2,300 IOPS türü veya seçili disk boyutu ne olursa olsun, veri disk başına en fazla, DS veya DSv2 serisi bir VM'den en çok 1000 IOPS sağlar.

Veri diski aktarım hızı VM ailesi serisi benzersiz göre belirlenir. Yapabilecekleriniz [bu makaleye başvurun](azure-stack-vm-sizes.md) VM ailesi serisi veri disk aktarım hızı tanımlamak için.

> [!NOTE]  
> Üretim iş yükleri için olası maksimum IOPS işletim sistemi diski ve veri diskleri sağlamak için bir DS serisi veya DSv2 serisi VM seçin.

Bu özellik Azure Stack'te kullanılamadığı için bir depolama hesabı Azure Stack'te oluştururken, coğrafi çoğaltma seçeneği etkisi yoktur.

## <a name="disks-guidance"></a>Diskleri Kılavuzu

Azure Stack sanal makinesinde üç ana disk türü vardır:

- **İşletim sistemi diski:** Azure Stack VM oluşturduğunuzda, platform en az bir diski ekler (olarak etiketlenmiş **C** sürücü), işletim sistemi diski VM'ye. Bu disk depodaki bir sayfa blobu olarak depolanan bir vhd'dir.

- **Geçici disk:** Azure Stack sanal makineleri içeren başka bir disk geçici diski de denen (olarak etiketlenmiş **D** sürücüsü). Bu çalışma alanı için kullanılabilir düğüm üzerinde bir disktir.

- **Veri diskleri:** Sanal makinenizde ek diskler veri diskleri ekleyebilir ve bu diskleri sayfa blobları depolama alanında depolanır.

Aşağıdaki bölümlerde, bu farklı diskler kullanarak önerileri açıklanmaktadır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

Önyükleme ve bağlama çalışan bir işletim sistemi sürümü bir VHD bir işletim sistemi diskidir ve olarak etiketlenmiş **C** sürücü.

### <a name="temporary-disk"></a>Geçici disk

Geçici depolama sürücü olarak etiketlenmiş **D** sürücü, kalıcı değildir. Üzerinde kaybetmek istemiyor herhangi bir veri depolama **D** sürücü. Bu kullanıcı veritabanı dosyaları ve kullanıcı işlem günlüğü dosyaları içerir.

Her veri diski maksimum veri diski başına en fazla 2,300 IOPS sağladığından, TempDB üzerinde veri diski depolamanızı öneririz.

### <a name="data-disks"></a>Veri diskleri

- **Veri diskleri için veri ve günlük dosyalarını kullanın.** Disk bölümleme türüyle kullanmıyorsanız, burada bir disk günlük dosyalarını içeren Premium Depolama'yı destekleyen bir VM'den iki veri diskleri kullanma ve diğer TempDB dosyaları ve verileri içerir. Her veri diski IOPS ve bant genişliği (MB/sn) VM ailesinin bağlı olarak birkaç açıklandığı sağlar [Azure Stack'te desteklenen VM boyutları](azure-stack-vm-sizes.md). Depolama alanları gibi bir disk bölümleme türüyle teknik kullanıyorsanız, tüm veri ve günlük dosyaları (TempDB dahil) aynı sürücüde yerleştirin. Bu yapılandırma için SQL Server'ı kullanmak hangi dosya gereksinimleri ne olursa olsun bunları belirli bir zamanda kullanılabilir IOPS sayısını sağlar.

> [!NOTE]  
> Bir SQL Server VM'si portalında sağladığınızda, depolama yapılandırmanızı düzenleme seçeneğiniz vardır. Yapılandırmanıza bağlı olarak, Azure Stack, bir veya daha fazla disk yapılandırır. Birden çok disk, bir tek bir depolama havuzu halinde birleştirilir. Veri ve günlük dosyaları, bu yapılandırmada birlikte yer alır.

- **Disk bölümleme türüyle:** Daha fazla aktarım hızı için ek veri diskleri ekleyip disk bölümleme türüyle kullanabilirsiniz. Gereksinim duyduğunuz veri diski sayısı belirlemek için IOPS ve günlük dosyalarınızı ve verilerinizi ve TempDB dosyaları için gereken bant sayısını analiz edin. Temel VM serisi ailesi ve VM boyutuna göre değil veri disk başına IOPS limitlerine olduğuna dikkat edin. Ağ bant genişliği sınırlarını, ancak VM boyutunu temel alır. Tablolara bakın [VM boyutları Azure Stack'te](azure-stack-vm-sizes.md) daha fazla ayrıntı için. Aşağıdaki yönergeleri kullanın:

  - Windows Server 2012 veya daha sonra kullanmanız [depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergeleri ile:

    1. Ayırma (stripe boyutu) çevrimiçi işlem (gerçekleştirme OLTP) iş yükleri ve 256 bölüm yanlış hizalanmış nedeniyle performansı etkilemelerini önlemek için KB (262.144 bayt) veri ambarı iş yükleri için 64 KB (65.536 bayt) olarak ayarlayın. PowerShell ile ayarlamanız gerekir.

    2. Ayarlama sütun sayısı = fiziksel disk numarası. PowerShell, sekizden fazla disk (Sunucu Yöneticisi kullanıcı Arabirimi değil) yapılandırırken kullanın.

       Örneğin, aşağıdaki PowerShell yeni bir depolama havuzu oluşturur ve ayırma boyutu 64 KB ile 2 sütun sayısını ayarlayın:

       ```powershell  
       $PoolCount = Get-PhysicalDisk -CanPool $True
       $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

       New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple -UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
       ```

- Depolama havuzunuz yük beklentilerinizi ilişkili disk sayısını belirler. Farklı VM boyutları, bağlı veri diskleri farklı sayıda izin verdiğini unutmayın. Daha fazla bilgi için [Azure Stack'te desteklenen VM boyutları](azure-stack-vm-sizes.md).
- Veri diskleri için en fazla olası IOPS almak için tarafından desteklenen veri diskleri en fazla sayıda eklemek için kullanılması önerilir, [VM boyutu](azure-stack-vm-sizes.md) ve disk bölümleme türüyle kullanılacak.
- **NTFS ayırma birimi boyutu:** Veri diski biçimlendirme sırasında TempDB yanı sıra veri ve günlük dosyaları için 64 KB ayırma birimi boyutu kullanmanızı öneririz.
- **Disk Yönetimi uygulamalar:** Bir veri diskinin kaldırılması, değişiklik sırasında SQL Server hizmetini durdurun. Ayrıca, herhangi bir performans iyileştirmeleri sağlamaz olarak disk üzerindeki önbellek ayarlarını değiştirmeyin.

> [!WARNING]  
> Bu işlemler sırasında SQL hizmetini durdurmak için hata veritabanında bozulmaya neden olabilir.

## <a name="io-guidance"></a>G/ç Kılavuzu

- İlk dosya ayırma için gereken süreyi azaltmak üzere anında dosya başlatma etkinleştirmeyi düşünün. Anında dosya başlatma yararlanmak için SQL Server (MSSQLSERVER) hizmet hesabıyla vermek **SE_MANAGE_VOLUME_NAME** ve eklemeniz **Birim bakım görevleri gerçekleştirme** güvenlik ilke. Azure için bir SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (**NT Service\MSSQLSERVER**) için eklenmez **Birim bakım görevleri gerçekleştirme** güvenlik ilkesi. Diğer bir deyişle, bir SQL Server Azure platform görüntüsüne anında dosya başlatma etkin değil. SQL Server hizmet hesabına ekledikten sonra **Birim bakım görevleri gerçekleştirme** güvenlik ilkesi, SQL Server hizmetini yeniden başlatın. Bu özelliği kullanmak için güvenlik konuları olabilir. Daha fazla bilgi için [veritabanı dosya başlatma](https://msdn.microsoft.com/library/ms175935.aspx).
- **Otomatik büyüme** beklenmeyen büyüme için bir yedek olduğu. Veri ve günlük büyümesini otomatik büyüme ile günlük olarak yönetmezsiniz. Kullanarak dosya ve otomatik büyüme kullandıysanız, önceden büyütün **boyutu** geçin.
- Emin **otomatik olarak küçültme** performansı olumsuz etkileyebilir gereksiz ek yükten kaçınmak için devre dışı bırakıldı.
- Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makalede önerileri kullanın ve sunucu Özellikler penceresinde değişiklikleri yapın. Yönergeler için [görüntülemek veya veri ve günlük dosyaları (SQL Server Management Studio) için varsayılan konumları değiştirme](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri yapmak nereye gösterir:

    > ![Görüntüleme veya varsayılan konumları değiştirme](./media/sql-server-vm-considerations/image1.png)

- GÇ ve herhangi bir disk belleği etkinlik azaltmak kilitli sayfalar sağlar. Daha fazla bilgi için [bellek seçeneği (Windows) kilit sayfalarında etkinleştirme](https://msdn.microsoft.com/library/ms190730.aspx).

- Yedekler de dahil olmak üzere Azure Stack içeri/dışarı aktarma, veri dosyalarının sıkıştırma göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü yönergeleri

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajlarını elde edebilirsiniz. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özelliklerini vurgular:

- **Azure yedekleme** **depolama.** Azure Stack Vm'lerinde çalışan SQL Server için yedekleme yaparken, SQL Server Yedekleme URL kullanabilirsiniz. Bu özellik, SQL Server 2012 SP1 CU2'ile başlayan kullanılabilir ve bağlı veri diskleri yedekleme için önerilen.

    Yedekleme mi, Azure depolama kullanarak geri yükleme, sağlanan önerilere uyun [URL en iyi yöntemler ve sorun giderme için SQL Server Yedekleme](https://msdn.microsoft.com/library/jj919149.aspx) ve [geri gelen yedeklemeler depolanan Microsoft azure'da](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017). Ayrıca bu yedeklemeler kullanarak otomatikleştirebilirsiniz [Azure Vm'lerde SQL Server için otomatik yedekleme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup).

-   **Azure Stack depolama alanına yedekleyin.** Azure Stack depolama benzer bir şekilde yedekleme gibi Azure depolama ile yedekleyebilirsiniz. SQL Server Management Studio (SSMS) içinde yedekleme oluşturduğunuzda, yapılandırma bilgilerini el ile girmeniz gerekir. SSMS, depolama kapsayıcısını veya paylaşılan erişim imzası oluşturmak için kullanamazsınız. SSMS, yalnızca Azure Abonelikleri, Azure Stack aboneliklerini bağlanır. Bunun yerine, Azure Stack portalında veya PowerShell ile depolama hesabı, kapsayıcı ve paylaşılan erişim imzası oluşturma gerekir.


    ![SQL Server Yedekleme](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > Paylaşılan erişim imzası öncü olmadan Azure Stack Portalı'ndan SAS belirtecidir '?' dizesi. Önde gelen silmenize gerek portalından kopyalama işlevini kullanırsanız, '?' SQL Server'ın içinde çalışmak için belirteci.

    Ayarlanan yedekleme hedefi olduğunda ve SQL Server'da yapılandırılan, ardından Azure Stack blob depolama alanına yedekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmetleri kullanarak veya Azure Stack için uygulamalar oluşturma](azure-stack-considerations.md)