---
title: Azure Stack performansını iyileştirmek için en iyi yöntemleri SQL Server. | Microsoft Docs
description: Bu makale, performansı artırmaya ve Azure Stack VM 'lerde SQL Server iyileştirmenize yardımcı olmak için SQL Server en iyi uygulamaları sağlar.
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
ms.openlocfilehash: eca886314388f404e7a26a22f7a3b03294ff0577
ms.sourcegitcommit: 5e53eb5d43d28ab07b4f84891dd269bbfcf65622
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71311303"
---
# <a name="sql-server-best-practices-to-optimize-performance-in-azure-stack"></a>Azure Stack performansını iyileştirmek için SQL Server en iyi uygulamaları

Bu makalede, SQL Server iyileştirmek ve Microsoft Azure Stack sanal makinelerde (VM) performansı iyileştirmek için SQL Server en iyi uygulamaları sağlanmaktadır. SQL Server Azure Stack sanal makinelerde çalıştırırken, şirket içi sunucu ortamında SQL Server için geçerli olan veritabanı performans ayarlama seçeneklerini kullanın. Bir Azure Stack buluttaki ilişkisel veritabanının performansı, bir VM 'nin aile boyutu ve veri disklerinin yapılandırılması dahil olmak üzere birçok etkene bağlıdır.

SQL Server görüntülerini oluştururken, [Azure Stack portalında sanal makinelerinizi sağlamayı düşünün](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). Azure Stack yönetici portalında Market yönetiminden SQL IaaS uzantısını indirin ve seçtiğiniz SQL VM sanal sabit sürücülerinizi (VHD) indirin. Bunlar şunlardır SQL2014SP2, SQL2016SP1 ve SQL2017.

> [!NOTE]  
> Makalesinde, genel Azure portal kullanılarak bir SQL Server VM nasıl sağlanacağı açıklanmaktadır. Bu kılavuz, aşağıdaki farklılıklara sahip Azure Stack için de geçerlidir: SSD, işletim sistemi diski için kullanılamaz, yönetilen diskler kullanılamıyor ve depolama yapılandırmasında küçük farklılıklar var.

Azure Stack VM 'lerde SQL Server için *en iyi* performansı alma, bu makaleye odaklanılmıştır. İş yükünüz daha az güç alıyorsa, önerilen en iyi duruma getirme gerektirmeyebilirsiniz. Bu önerileri değerlendirdiğiniz için performans ihtiyaçlarınızı ve iş yükü desenlerini göz önünde bulundurun.

> [!NOTE]  
> Azure VM 'lerinde SQL Server için performans Kılavuzu için [Bu makaleye](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)bakın.

## <a name="checklist-for-sql-server-best-practices"></a>SQL Server en iyi uygulamaları için denetim listesi

Aşağıdaki denetim listesi, Azure Stack VM 'lerde SQL Server en iyi performansı elde etmek için kullanılır:


|Alan|İyileştirmeleri|
|-----|-----|
|VM boyutu |SQL Server Enterprise Edition için [DS3](azure-stack-vm-sizes.md) veya üzeri.<br><br>SQL Server Standard Edition ve Web Edition için [DS2](azure-stack-vm-sizes.md) veya üzeri.|
|Depolama |[Premium depolamayı](azure-stack-acs-differences.md)destekleyen bir VM ailesi kullanın.|
|Diskler |En az iki veri diski (bir günlük dosyası ve bir veri dosyası ve TempDB için bir tane) kullanın ve kapasite gereksinimlerinize göre disk boyutunu seçin. SQL Server yüklemesi sırasında varsayılan veri dosyası konumlarını bu disklere ayarlayın.<br><br>Veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br>Depolama alanlarını kullanarak daha fazla GÇ üretilen işi sağlamak için birden çok Azure veri diski Stripe.<br><br>Belgelenmiş ayırma boyutları ile biçimlendirin.|
|G/Ç|Veri dosyaları için anlık dosya başlatmayı etkinleştirin.<br><br>Veritabanları üzerinde otomatik büyüme 'yı makul ölçüde küçük sabit artışlarla sınırlayın (64 MB-256 MB).<br><br>Veritabanında, oto 'yi devre dışı bırakın.<br><br>İşletim sistemi diskinde değil, veri disklerinde varsayılan yedekleme ve veritabanı dosya konumlarını ayarlayın.<br><br>Kilitli sayfaları etkinleştirin.<br><br>SQL Server hizmet paketlerini ve toplu güncelleştirmeleri uygulayın.|
|Özelliğe özgü|Doğrudan blob depolamaya yedekleme (SQL Server sürümü tarafından destekleniyorsa).|
|||

Bu iyileştirmelerin *nasıl* ve *neden* yapılacağı hakkında daha fazla bilgi için aşağıdaki bölümlerde sunulan ayrıntıları ve Kılavuzu gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyut Kılavuzu

Performansa duyarlı uygulamalar için aşağıdaki [VM boyutları](azure-stack-vm-sizes.md) önerilir:

- **SQL Server Enterprise sürümü:** DS3 veya üzeri

- **SQL Server Standard Edition ve Web Edition:** DS2 veya üzeri

Azure Stack, DS ve DS_v2 VM ailesi serisi arasında bir performans farkı yoktur.

## <a name="storage-guidance"></a>Depolama yönergeleri

Azure Stack içindeki DS serisi (DSv2-Series ile birlikte) VM 'Ler maksimum işletim sistemi diski ve veri diski verimlilik (ıOPS) sağlar. DS veya DSv2 serisindeki bir VM, işletim sistemi diski için 1.000 ıOPS ve veri diski başına en fazla 2.300 ıOPS sağlar. Bu, seçilen diskin türü veya boyutu ne olsun.

Veri diski verimlilik, VM ailesi serisine göre benzersiz şekilde belirlenir. VM aile serisi başına veri diski aktarım hızını belirlemek için [Bu makaleye başvurabilirsiniz](azure-stack-vm-sizes.md) .

> [!NOTE]  
> Üretim iş yükleri için, işletim sistemi diskinde ve veri disklerinde olabilecek en fazla ıOPS 'yi sağlamak üzere bir DS serisi veya DSv2 serisi VM seçin.

Azure Stack ' de bir depolama hesabı oluştururken, bu özellik Azure Stack kullanılamadığından coğrafi çoğaltma seçeneğinin hiçbir etkisi yoktur.

## <a name="disks-guidance"></a>Diskler Kılavuzu

Azure Stack VM 'de üç ana disk türü vardır:

- **İşletim sistemi diski:** Bir Azure Stack VM oluşturduğunuzda, Platform işletim sistemi diskinizin VM 'sine en az bir disk ( **C** sürücüsü olarak etiketlenir) iliştirir. Bu disk, depolamada bir Sayfa Blobu olarak depolanan bir VHD 'dir.

- **Geçici disk:** Azure Stack VM 'Ler, geçici disk ( **D** sürücüsü olarak etiketlenir) adlı başka bir disk içerir. Bu, düğüm üzerinde karalama alanı için kullanılabilecek bir disktir.

- **Veri diskleri:** Sanal makinenize veri diskleri olarak ek diskler ekleyebilirsiniz ve bu diskler depolama alanında sayfa Blobları olarak depolanır.

Aşağıdaki bölümlerde bu farklı diskleri kullanmaya yönelik öneriler açıklanır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

İşletim sistemi diski, işletim sisteminin çalışan bir sürümü olarak önyüklenebilir ve takabilmeniz ve **C** sürücüsü olarak ETIKETLEDIĞINIZ bir VHD 'dir.

### <a name="temporary-disk"></a>Geçici disk

**D** sürücüsü olarak etiketlenen geçici depolama sürücüsü kalıcı değildir. **D** sürücüsünde kaybetmek istemediğiniz herhangi bir veri depolamayın. Bu, Kullanıcı veritabanı dosyalarınızı ve Kullanıcı işlem günlüğü dosyalarını içerir.

Her veri diski, veri diski başına en fazla 2.300 ıOPS sağladığından TempDB 'yi bir veri diskinde depolamanızı öneririz.

### <a name="data-disks"></a>Veri diskleri

- **Veri ve günlük dosyaları için veri disklerini kullanın.** Disk şeritleme kullanmıyorsanız, Premium depolamayı destekleyen bir VM 'den iki veri diski kullanın; burada bir disk, günlük dosyalarını ve diğeri ise veri ve TempDB dosyalarını içerir. Her veri diski, [Azure Stack sürümünde desteklenen VM boyutları](azure-stack-vm-sizes.md)bölümünde açıklandığı gıbı, VM ailesine bağlı olarak BIR dizi IOPS sağlar. Depolama alanları gibi bir disk şeridi tekniği kullanıyorsanız, tüm verileri ve günlük dosyalarını aynı sürücüye (TempDB dahil) yerleştirin. Bu yapılandırma, belirli bir zamanda hangi dosyayı kullanması gerektiğine bakılmaksızın SQL Server için kullanılabilir maksimum ıOPS sayısını sağlar.

> [!NOTE]  
> Portalda bir SQL Server VM sağladığınızda, depolama yapılandırmanızı Düzenle seçeneğiniz vardır. Yapılandırmanıza bağlı olarak, Azure Stack bir veya daha fazla disk yapılandırır. Birden çok disk tek bir depolama havuzunda birleştirilir. Hem veri hem de günlük dosyaları, bu yapılandırmada birlikte bulunur.

- **Disk şeridi:** Daha fazla verimlilik için, ek veri diskleri ekleyebilir ve disk şeritleme kullanabilirsiniz. İhtiyacınız olan veri disklerinin sayısını öğrenmek için, günlük dosyalarınız ve verileriniz ile TempDB dosyalarınız için gereken ıOPS sayısını çözümleyin. IOPS limitlerinin VM 'nin boyutuna bağlı olarak değil, VM Serisi ailesini temel alan veri diski başına olduğuna dikkat edin. Ancak ağ bant genişliği sınırları, VM boyutunu temel alır. Daha fazla ayrıntı için [Azure Stack sanal makine boyutlarında](azure-stack-vm-sizes.md) bulunan tablolara bakın. Aşağıdaki yönergeleri kullanın:

  - Windows Server 2012 veya üzeri için, [depolama alanlarını](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergelerle kullanın:

    1. Bölüm hatalı hizalaması nedeniyle performans etkisini önlemek için, çevrimiçi işlem işleme (OLTP) iş yükleri ve 256 KB (262.144 bayt) için Interleave (Stripe boyutu) 64 KB (65.536 bayt) olarak ayarlayın. Bunun PowerShell ile ayarlanması gerekir.

    2. Sütun sayısını ayarla = fiziksel disk sayısı. Sekiz ' dan fazla disk yapılandırırken PowerShell kullanın (Sunucu Yöneticisi Kullanıcı arabirimi).

       Örneğin, aşağıdaki PowerShell, ayırma boyutu 64 KB ve sütun sayısı 2 olarak ayarlanan yeni bir depolama havuzu oluşturur:

       ```powershell  
       $PoolCount = Get-PhysicalDisk -CanPool $True
       $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

       New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple -UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
       ```

- Yük beklentilerinize göre depolama havuzunuzun ilişkili disk sayısını belirleme. Farklı VM boyutlarının farklı sayıda ekli veri diskine izin verdiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [Azure Stack desteklenen VM boyutları](azure-stack-vm-sizes.md).
- Veri disklerine yönelik olabilecek en yüksek ıOPS 'yi almak için, [VM boyutunuz](azure-stack-vm-sizes.md) tarafından desteklenen en fazla veri diski sayısını ve disk şeritleme 'yi kullanmak için öneri önerilir.
- **NTFS ayırma birimi boyutu:** Veri diski biçimlendirilirken, veri ve günlük dosyaları için bir 64 KB ayırma birimi boyutu ve TempDB de kullanmanızı öneririz.
- **Disk yönetimi uygulamaları:** Bir veri diski kaldırılırken, değişiklik sırasında SQL Server hizmetini durdurun. Ayrıca, disklerde herhangi bir performans geliştirmesi sağlamadığından önbellek ayarlarını değiştirmeyin.

> [!WARNING]  
> Bu işlemler sırasında SQL hizmeti durdurulamaması veritabanının bozulmasına neden olabilir.

## <a name="io-guidance"></a>G/ç Kılavuzu

- İlk dosya ayırma için gereken süreyi azaltmak üzere anlık dosya başlatmayı etkinleştirmeyi düşünün. Anlık dosya başlatmasının avantajlarından yararlanabilmek için, **se_manage_volume_name** ile SQL Server (MSSQLSERVER) hizmet hesabına izin verirsiniz ve **Birim bakımı görevleri** güvenlik ilkesine eklersiniz. Azure için SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (**NT Service\MSSQLSERVER**) **toplu bakım görevleri** oluştur güvenlik ilkesine eklenmez. Diğer bir deyişle, Azure platform görüntüsünde SQL Server anlık dosya başlatma etkin değildir. SQL Server hizmet hesabını **toplu bakım görevleri Için gerçekleştir** güvenlik ilkesine ekledikten sonra, SQL Server hizmetini yeniden başlatın. Bu özelliğin kullanımıyla ilgili güvenlik konuları olabilir. Daha fazla bilgi için bkz. [veritabanı dosyası başlatma](https://msdn.microsoft.com/library/ms175935.aspx).
- **Otomatik büyüme** beklenmeyen büyüme için bir yedek değildir. Verilerinizi ve günlük büyümesini otomatik büyüme ile günde bir gün boyunca yönetmeyin. Otomatik büyüme kullanılıyorsa, **Boyut** anahtarını kullanarak dosyayı önceden büyütün.
- Performansı olumsuz etkileyebilecek gereksiz ek yükü önlemek için, **oto küçültme** 'nın devre dışı bırakıldığından emin olun.
- Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makaledeki önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz. [veri ve günlük dosyaları Için varsayılan konumları görüntüleme veya değiştirme (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişikliklerin nerede oluşturulacağı gösterilmektedir:

    > ![Varsayılan konumları görüntüleme veya değiştirme](./media/sql-server-vm-considerations/image1.png)

- GÇ ve tüm disk belleği etkinliklerini azaltmak için kilitli sayfaları etkinleştirin. Daha fazla bilgi için bkz. [bellek Içinde sayfaları kilitleme seçeneği (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Yedeklemeler de dahil olmak üzere Azure Stack içine/dışına aktarma sırasında tüm veri dosyalarını sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü rehberlik

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajları elde edebilir. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özellikler vurgulanmıştır:

- **Azure 'A yedekleme** **depolama alanı.** Azure Stack VM 'lerde çalışan SQL Server için yedeklemeler yaparken, URL 'ye SQL Server yedekleme 'YI kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ ile başlayarak kullanılabilir ve ekli veri disklerine yedekleme için önerilir.

    Azure Storage kullanarak yedekleme veya geri yükleme yaptığınızda, [URL 'Yi en iyi duruma getirmek için SQL Server Yedekleme ve](https://msdn.microsoft.com/library/jj919149.aspx) [Microsoft Azure depolanan yedeklemelerden geri yükleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017)konularında belirtilen önerileri izleyin. Ayrıca, [Azure VM 'lerinde SQL Server Için Otomatik yedeklemeyi](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup)kullanarak bu yedeklemeleri otomatikleştirebilir.

-   **Azure Stack depolama alanına yedekleyin.** Azure depolama 'ya yedekleme ile benzer bir şekilde depolama Azure Stack için yedekleme yapabilirsiniz. SQL Server Management Studio (SSMS) içinde bir yedek oluşturduğunuzda, yapılandırma bilgilerini el ile girmeniz gerekir. SSMS 'yi depolama kapsayıcısını veya paylaşılan erişim Imzasını oluşturmak için kullanamazsınız. SSMS yalnızca Azure Stack abonelikleri değil Azure aboneliklerine bağlanır. Bunun yerine, Azure Stack portalında veya PowerShell ile depolama hesabı, kapsayıcı ve paylaşılan erişim Imzası oluşturmanız gerekir.


    ![SQL Server yedekleme](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > Paylaşılan erişim Imzası, önde gelen '? ' olmadan Azure Stack portalından SAS belirtemidir dizesinde. Portalda kopyalama işlevini kullanırsanız, önde gelen '? ' öğesini silmeniz gerekir belirtecin SQL Server içinde çalışması için.

    SQL Server ' de yedekleme hedefi ayarladıktan ve yapılandırıldıktan sonra, Azure Stack blob depolamaya yedekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack için Hizmetleri kullanma veya uygulama oluşturma](azure-stack-considerations.md)
