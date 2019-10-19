---
title: Azure Stack 'de depolama kapasitesini yönetme | Microsoft Docs
description: Azure Stack ' de depolama kapasitesini ve kullanılabilirliği izlemeyi ve yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: 73c4594672dea4a8bb8030a35c79f3d7e7cca04c
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534167"
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Azure Stack için depolama kapasitesini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makaledeki bilgiler Azure Stack bulut işlecinin Azure Stack dağıtımının depolama kapasitesini izlemesine ve yönetmesine yardımcı olur. Azure Stack depolama altyapısı, **Depolama Hizmetleri**için kullanılacak Azure Stack dağıtımının toplam depolama kapasitesinin bir alt kümesini ayırır. Depolama hizmetleri kiracının verilerini paylaşımlarda, dağıtımın düğümlerine karşılık gelen birimlerde depolar.

Bulut operatörü olarak, üzerinde çalışmak için sınırlı miktarda depolama alanı vardır. Depolama miktarı, uyguladığınız çözüm tarafından tanımlanır. Çözümünüz, çok düğümlü bir çözüm kullandığınızda veya Azure Stack Geliştirme Seti (ASKD) yüklediğiniz donanımla, OEM satıcınız tarafından sağlanır.

Azure Stack, depolama kapasitesini genişletmeyi desteklemediğinden, etkin işlemlerin korunmasını sağlamak için kullanılabilir depolamayı [izlemeniz](#monitor-shares) önemlidir.

Bir paylaşımın kalan boş kapasitesi sınırlı hale geldiğinde, paylaşımların kapasitenin tükenme çalışmasını engellemek için [alanı yönetmeyi](#manage-available-space) planlayın.

Kapasiteyi yönetme seçenekleri şunlardır:
- Kapasiteyi geri kazanma
- Kapsayıcıyı geçirme

Bir paylaşımın %100 olarak kullanıldığı durumlarda, depolama hizmeti artık bu paylaşıma yönelik olarak çalışmaz. Paylaşıma yönelik geri yükleme işlemlerinde yardım almak için Microsoft desteği 'ne başvurun.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Birimleri ve paylaşımları, kapsayıcıları ve diskleri anlama
### <a name="volumes-and-shares"></a>Birimler ve paylaşımlar
*Depolama hizmeti* , kullanılabilir depolamayı kiracı verilerini tutmak için ayrılan ayrı ve eşit birimlere bölümler. Birim sayısı Azure Stack dağıtımındaki düğüm sayısına eşittir:

- Dört düğümlü bir dağıtımda dört birim vardır. Her birimin tek bir paylaşılması vardır. Çok düğümlü bir dağıtımda, bir düğüm kaldırılırsa veya hatalı çalışmıyorsa paylaşım sayısı azalmıştır.
- ASDK kullanırsanız, tek bir paylaşıma sahip tek bir birim vardır.

Depolama hizmeti paylaşımları depolama hizmetlerinin dışlamalı kullanımı için olduğundan, paylaşımlardaki dosyaları doğrudan değiştirmemelidir, eklememelidir veya kaldırmanız gerekmez. Bu birimlerde depolanan dosyalarda yalnızca depolama hizmetleri çalışmalıdır.

Birimlerde bulunan paylaşımlar, kiracı verilerini tutar. Kiracı verileri sayfa Blobları, blok Blobları, ekleme Blobları, tablolar, kuyruklar, veritabanları ve ilgili meta veri depoları içerir. Depolama nesneleri (blob 'lar vb.) tek bir paylaşımda tek bir paylaşımda bulunduğundan, her bir nesnenin en büyük boyutu bir paylaşımın boyutunu aşamaz. Yeni nesnelerin en büyük boyutu, yeni nesne oluşturulduğunda kullanılmayan alan olarak bir paylaşımda kalan kapasiteye bağlıdır.

Bir paylaşımın boş alana sahip olduğu ve alanı yeniden [kazanmak](#reclaim-capacity) için gerçekleştirilen eylemler başarılı veya kullanılabilir olmadığında, Azure Stack Cloud işleci blob kapsayıcılarını bir paylaşımdan diğerine geçirebilir.

- Kiracı kullanıcılarının Azure Stack blob depolamayla nasıl çalıştığı hakkında bilgi için, bkz. [Azure Stack Storage Services](/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Kapsayıcılar
Kiracı kullanıcıları daha sonra blob verilerini depolamak için kullanılan kapsayıcılar oluşturur. Kullanıcı hangi kapsayıcının blob 'ları yerleşeceğine karar verdiğinde, depolama hizmeti kapsayıcının hangi birimde yerleştirileceğini belirleyen bir algoritma kullanır. Algoritma genellikle en fazla kullanılabilir alanı olan birimi seçer.  

Bir blob bir kapsayıcıya yerleştirildikten sonra, o blob daha fazla alan kullanmak için büyüyebilir. Yeni blob 'lar ekler ve mevcut Bloblar büyüdükçe, o kapsayıcının bulunduğu birimdeki kullanılabilir alan küçülür.  

Kapsayıcılar tek bir paylaşımıyla sınırlı değildir. Bir kapsayıcıdaki birleştirilmiş blob verileri, %80 veya daha fazla kullanılabilir alanı kullanmak üzere büyüdükçe kapsayıcı, *taşma* moduna girer. Taşma modundayken, bu kapsayıcıda oluşturulan tüm yeni Bloblar yeterli alana sahip farklı bir birime ayrılır. Zaman içinde, taşma modundaki bir kapsayıcı birden çok birime dağıtılan bloblara sahip olabilir.

%80 olduğunda (ve sonra %90) bir birimdeki kullanılabilir alanın kullanıldığı sistem, Azure Stack yönetici portalında uyarı oluşturur. Bulut işleçleri, kullanılabilir depolama kapasitesini gözden geçirmeli ve içeriğin yeniden dengelenmesi planlanmalıdır. Bir disk kullanılan %100 olduğunda ve ek uyarı harekete çıktığında depolama hizmeti çalışmayı durduruyor.

### <a name="disks"></a>Diskler
VM diskleri kiracılar tarafından kapsayıcılara eklenir ve bir işletim sistemi diski içerir. VM 'Ler aynı zamanda bir veya daha fazla veri diskine sahip olabilir. Her iki disk türü de sayfa Blobları olarak depolanır. Kiracılara kılavuzluk, sanal makinenin performansını artırmak için her diski ayrı bir kapsayıcıya yerleştirmaktır.

- Bir VM 'den bir disk (Sayfa Blobu) tutan her bir kapsayıcı, diskin sahibi olan VM 'ye bağlı bir kapsayıcı olarak değerlendirilir.
- Bir sanal makineden (VM) herhangi bir disk tutan bir kapsayıcı, ücretsiz bir kapsayıcı olarak kabul edilir.

Eklenmiş bir kapsayıcıda alan boşaltma seçenekleri [sınırlıdır](#move-vm-disks).

>[!TIP]  
> Bulut işleçleri, kiracıların bir kapsayıcıya ekleyebileceği VM 'lere bağlı diskleri doğrudan yönetmez. Ancak, depolama paylaşımlarında alanı yönetmeyi planlarken, disklerin kapsayıcılarla ve paylaşımlarla ilişkisini anlamak için kullanılabilir.

## <a name="monitor-shares"></a>Paylaşımları izle
Boş alanın sınırlı olduğunu anlayabilmeniz için paylaşımları izlemek üzere PowerShell veya Yönetici portalını kullanın. Portalı kullandığınızda, alan yetersizliği olan paylaşımlar hakkında uyarılar alırsınız.

### <a name="use-powershell"></a>PowerShell'i kullanma
Bir bulut operatörü olarak, PowerShell **Get-Azsstoragesshare** cmdlet 'ini kullanarak bir paylaşımın depolama kapasitesini izleyebilirsiniz. Get-Azsstoragesshares cmdlet 'i, paylaşımların her birinde toplam, ayrılan ve boş alanı bayt olarak döndürür.

![Örnek: paylaşımlar için boş alan döndürün](media/azure-stack-manage-storage-shares/free-space.png)

- **Toplam kapasite** , paylaşımda bulunan baytların toplam alanıdır. Bu alan, depolama hizmetleri tarafından tutulan veriler ve meta veriler için kullanılır.
- **Kullanılan kapasite** , kiracı verilerini ve ilişkili meta verileri depolayan dosyalardaki tüm kapsamlar tarafından kullanılan bayt cinsinden veri miktarıdır.

### <a name="use-the-administrator-portal"></a>Yönetici portalını kullanma
Bulut operatörü olarak, tüm paylaşımların depolama kapasitesini görüntülemek için Yönetici portalını kullanabilirsiniz.

1. [Yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.
2. Kullanım bilgilerini görüntüleyebileceğiniz dosya paylaşımı listesini açmak için, **tüm hizmetler**  > **depolama**  > **dosya paylaşımları** ' nı seçin.

    ![Örnek: Azure Stack yönetici portalında depolama dosya paylaşımları](media/azure-stack-manage-storage-shares/storage-file-shares.png)

   - **Toplam** , paylaşımda bulunan baytların toplam alanıdır. Bu alan, depolama hizmetleri tarafından tutulan veriler ve meta veriler için kullanılır.
   - **Kullanılan** , kiracı verilerini ve ilişkili meta verileri depolayan dosyalardaki tüm kapsamlar tarafından kullanılan bayt cinsinden veri miktarıdır.

### <a name="storage-space-alerts"></a>Depolama alanı uyarıları
Yönetici portalını kullanırken, alan yetersizliği olan paylaşımlar hakkında uyarılar alırsınız.

> [!IMPORTANT]
> Bulut operatörü olarak, paylaşımların tam kullanıma ulaşmasını sağlayın. Bir paylaşımın %100 olarak kullanıldığı durumlarda, depolama hizmeti artık bu paylaşıma yönelik olarak çalışmaz. %100 kullanılan bir paylaşımdaki boş alanı ve geri yükleme işlemlerini kurtarmak için Microsoft desteği 'ne başvurmanız gerekir.

**Uyarı**: bir dosya paylaşımının %80 üzerinde olması durumunda, yönetici portalında bir *Uyarı* uyarısı alırsınız:

![Örnek: Azure Stack yönetici portalında uyarı uyarısı](media/azure-stack-manage-storage-shares/alert-warning.png)

**Kritik**: bir dosya paylaşımının %90 ' den fazla olması halinde, yönetici portalında *kritik* bir uyarı alırsınız:

![Örnek: Azure Stack yönetici portalında kritik uyarı](media/azure-stack-manage-storage-shares/alert-critical.png)

**Ayrıntıları görüntüle**: yönetici portalında, azaltma seçeneklerini görüntülemek için bir uyarının ayrıntılarını açabilirsiniz: ![Example: Azure Stack yönetici portalında uyarı ayrıntılarını görüntüleyin ](media/azure-stack-manage-storage-shares/alert-details.png)

## <a name="manage-available-space"></a>Kullanılabilir alanı Yönet
Bir paylaşımdaki alanı boşaltmak gerektiğinde, önce en az bağımsız yöntemleri kullanın. Örneğin, kapsayıcıyı geçirmeyi seçmeden önce alanı geri almaya çalışın.  

### <a name="reclaim-capacity"></a>Kapasiteyi geri kazanma
*Bu seçenek hem çok düğümlü dağıtımlar hem de Azure Stack Geliştirme Seti için geçerlidir.*

Silinen kiracı hesaplarının kullandığı kapasiteyi geri kazanabilirsiniz. Bu kapasite, veri [saklama süresine](azure-stack-manage-storage-accounts.md#set-the-retention-period) ulaşıldığında otomatik olarak geri kazanılır veya hemen geri kazanmak için işlem yapabilirsiniz.

Daha fazla bilgi için bkz. depolama kaynaklarını yönetme bölümünde [kapasiteyi geri](azure-stack-manage-storage-accounts.md#reclaim) alma.

### <a name="migrate-a-container-between-volumes"></a>Bir kapsayıcıyı birimler arasında geçirin
*Bu seçenek yalnızca Azure Stack tümleşik sistemler için geçerlidir.*

Kiracı kullanım desenleri nedeniyle, bazı kiracı paylaşımları diğerlerinden daha fazla alan kullanır. Sonuç, görece olarak kullanılmayan diğer paylaşımlardan önce boşluk az çalışan bir paylaşım olabilir.

Bazı blob kapsayıcılarını farklı bir paylaşıma el ile geçirerek aşırı kullanılan bir paylaşımda boş alan açabilirsiniz. Birkaç küçük kapsayıcıyı, tümünü tutan kapasiteye sahip tek bir paylaşıma geçirebilirsiniz. *Ücretsiz* kapsayıcıları taşımak için geçiş kullanın. Ücretsiz kapsayıcılar, bir sanal makine için disk içermeyen kapsayıcılardır.

Geçiş, kapsayıcının tüm bloblarını yeni paylaşımda birleştirir.

- Bir kapsayıcı taşma moduna girmiş ve BLOB 'ları ek birimlere yerleştirdiyse, yeni paylaşımın, geçiş yaptığınız kapsayıcının tüm bloblarını tutacak yeterli kapasiteye sahip olması gerekir. Buna ek paylaşımlarda bulunan Bloblar dahildir.

- *Get-AzsStorageContainer* PowerShell cmdlet 'i, yalnızca bir kapsayıcı için ilk birimde kullanılan alanı tanımlar. Cmdlet 'i, Bloblar tarafından kullanılan ve ek birimlere yerleştirilen alanı tanımlamaz. Bu nedenle, bir kapsayıcının tam boyutu açık olmayabilir. Yeni bir paylaşımdaki kapsayıcının birleştirilmesi, bu yeni paylaşımın verileri ek paylaşımlara yerleştirdiği bir taşma koşuluna gönderebilmesini olanaklı hale gelir. Sonuç olarak, paylaşımları yeniden dengelemeniz gerekebilir.

- Bir kaynak grubuna yönelik izinleriniz yoksa ve taşma verilerine yönelik ek birimleri sorgulamak için PowerShell 'i kullanamıyoruz, bu verileri geçirmeden önce geçirilecek verilerin toplam boyutunu anlamak için bu kaynak grupları ve kapsayıcılarının sahibiyle çalışın.  

> [!IMPORTANT]
> Bir kapsayıcı için Blobların geçirilmesi, PowerShell kullanımını gerektiren çevrimdışı bir işlemdir. Geçiş tamamlanana kadar geçirdiğiniz kapsayıcının tüm Blobları çevrimdışı kalır ve kullanılamaz. Devam eden tüm geçiş tamamlanana kadar Azure Stack yükseltmemeye de kaçınmalısınız.

#### <a name="to-migrate-containers-using-powershell"></a>PowerShell kullanarak kapsayıcıları geçirmek için
1. [Azure PowerShell yüklendiğini ve yapılandırıldığını](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](https://go.microsoft.com/fwlink/?LinkId=394767).
2. Geçirmeyi planladığınız paylaşımdaki verileri anlamak için kapsayıcıyı inceleyin. Bir birimdeki geçiş için en iyi aday kapsayıcıları belirlemek üzere **Get-AzsStorageContainer** cmdlet 'ini kullanın:

   ```powershell  
   $farm_name = (Get-AzsStorageFarm)[0].name
   $shares = Get-AzsStorageShare -FarmName $farm_name
   $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -FarmName $farm_name
   ```
   Sonra $containers inceleyin:

   ```powershell
   $containers
   ```

   ![Örnek: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3. Geçiş yaptığınız kapsayıcıyı barındıracak en iyi hedef paylaşımlarını belirler:

   ```powershell
   $destinationshare = ($shares | Sort-Object FreeCapacity -Descending)[0]
   ```

   Sonra $destinationshares inceleyin:

   ```powershell 
   $destinationshares
   ```

   ![Örnek: $destination paylaşımlar](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Bir kapsayıcı için geçişi başlatın. Geçiş zaman uyumsuzdur. İlk geçiş tamamlanmadan önce ek kapsayıcıların geçişini başlatırsanız, her birinin durumunu izlemek için iş KIMLIĞINI kullanın.

   ```powershell
   $job_id = Start-AzsStorageContainerMigration -StorageAccountName $containers[0].Accountname -ContainerName $containers[0].Containername -ShareName $containers[0].Sharename -DestinationShareUncPath $destinationshares[0].UncPath -FarmName $farm_name
   ```

   Ardından $jobId inceleyin. Aşağıdaki örnekte, *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* değerini incelemek ISTEDIĞINIZ iş kimliğiyle değiştirin:

   ```powershell
   $jobId
   d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
   ```

5. Geçiş işinin durumunu denetlemek için iş KIMLIĞINI kullanın. Kapsayıcı geçişi tamamlandığında, geçiş **durumu** **Tamam**olarak ayarlanır.

   ```powershell 
   Get-AzsStorageContainerMigrationStatus -JobId $job_id -FarmName $farm_name
   ```

   ![Örnek: geçiş durumu](media/azure-stack-manage-storage-shares/migration-status1.png)

6. Sürmekte olan geçiş işini iptal edebilirsiniz. İptal edilen geçiş işleri zaman uyumsuz olarak işlenir. $Jobid kullanarak iptali izleyebilirsiniz:

   ```powershell
   Stop-AzsStorageContainerMigration -JobId $job_id -FarmName $farm_name
   ```

   ![Örnek: geri alma durumu](media/azure-stack-manage-storage-shares/rollback.png)

7. Yeniden 6. adımdaki komutu çalıştırarak, durum geçiş işini **Iptal edildiğini**doğrular:  

    ![Örnek: Iptal edildi durumu](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>VM disklerini taşıma
*Bu seçenek yalnızca Azure Stack tümleşik sistemler için geçerlidir.*

Alanı yönetmek için en Extreme yöntemi VM disklerinin taşınmasını içerir. Eklenmiş bir kapsayıcı (bir VM diski içeren) taşınması karmaşık olduğundan, bu eylemi gerçekleştirmek için Microsoft Desteği başvurun.

## <a name="next-steps"></a>Sonraki Adımlar
[Kullanıcılara VM 'ler sunma](azure-stack-tutorial-tenant-vm.md)hakkında daha fazla bilgi edinin.
