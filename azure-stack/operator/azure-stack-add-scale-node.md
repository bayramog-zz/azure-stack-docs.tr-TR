---
title: Azure Stack ölçek birimi düğümleri ekleme | Microsoft Docs
description: Ölçek birim düğümlerini Azure Stack ölçek birimlerine ekleme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: ab06f5d3674000733227894a5a69778d90c29d48
ms.sourcegitcommit: e8f7fe07b32be33ef621915089344caf1fdca3fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118747"
---
# <a name="add-additional-scale-unit-nodes-in-azure-stack"></a>Azure Stack ek ölçek birimi düğümleri ekleyin

Azure Stack işleçleri, ek bir fiziksel bilgisayar ekleyerek mevcut bir ölçek biriminin genel kapasitesini artırabilir. Fiziksel bilgisayar, ölçek birimi düğümü olarak da adlandırılır. Eklediğiniz her yeni ölçek birimi düğümünün CPU türü, belleği ve disk numarası ve boyutu, ölçek biriminde zaten mevcut olan düğümlere sahip olması gerekir.

Bir ölçek birimi düğümü eklemek için, donanım ekipmanı üreticinizden (OEM) Azure Stack çalışır ve araçları çalıştırabilirsiniz. OEM araçları, yeni fiziksel bilgisayarın mevcut düğümlerle aynı bellenim düzeyiyle eşleştiğinden emin olmak için donanım yaşam döngüsü ana bilgisayarında (HLH) çalışır.

Aşağıdaki akış diyagramı, bir ölçek birimi düğümü eklemek için genel işlemi gösterir:

![Ölçek birimi akışı Ekle](media/azure-stack-add-scale-node/add-node-flow.png)
<br> *OEM Donanım satıcınızın fiziksel sunucu raf yerleşimini ve güncelleştirmelerini hareket etmeksizin, destek sözleşmeniz temel alınarak bellenim değişir.*

Yeni düğüm ekleme işleminin tamamlanması birkaç saat veya gün sürebilir.

> [!Note]  
> Ölçek birim düğümü ekleme işlemi zaten devam ederken aşağıdaki işlemlerden herhangi birini denemeyin:
>
>  - Güncelleştirme Azure Stack
>  - Sertifikaları döndürme
>  - Azure Stack durdur
>  - Ölçek birimi düğümünü Onar


## <a name="add-scale-unit-nodes"></a>Ölçek birimi düğümleri Ekle

Aşağıdaki adımlar, bir düğüm ekleme hakkında üst düzey bir genel bakıştır. Öncelikle OEM tarafından sunulan kapasite genişletme belgelerinize başvurmadan bu adımları takip etmeyin.

1. Yeni fiziksel sunucuyu rafa yerleştirin ve uygun şekilde bağlayın. 
2. Fiziksel anahtar bağlantı noktalarını etkinleştirin ve uygunsa erişim denetim listelerini (ACL 'Ler) ayarlayın.
3. Temel kart yönetim denetleyicisindeki (BMC) doğru IP adresini yapılandırın ve tüm BIOS ayarlarını OEM tarafından sağlanmış belgeleriniz uyarınca uygulayın.
4. Geçerli bellenim temelini, HLH üzerinde çalışan donanım üreticisi tarafından sunulan araçları kullanarak tüm bileşenlere uygulayın.
5. Azure Stack yönetici portalında düğüm ekleme işlemini çalıştırın.
6. Düğüm ekleme işleminin başarılı olduğunu doğrulayın. Bunu yapmak için, [ölçek biriminin **durumunu** ](#monitor-add-node-operations)kontrol edin. 

## <a name="add-the-node"></a>Düğümü ekleme

Yeni düğümler eklemek için Yönetici portalı veya PowerShell kullanabilirsiniz. Düğüm Ekle işlemi önce yeni ölçek birimi düğümünü kullanılabilir işlem kapasitesi olarak ekler ve ardından depolama kapasitesini otomatik olarak genişletir. *İşlem* ve *depolama* ölçeğinin birlikte Azure Stack hiper yakınsanmış bir sistem olduğundan, kapasite otomatik olarak genişletilir.

### <a name="use-the-admin-portal"></a>Yönetici portalını kullanma

1. Azure Stack yönetici portalında Azure Stack işleci olarak oturum açın.
2. **+ Kaynak** > kapasitesiölçek > **birimi düğümü**oluştur ' a gidin.
   ![Birim düğümünü Ölçeklendir](media/azure-stack-add-scale-node/select-node1.png)
3. **Düğüm Ekle** bölmesinde *bölge*' yi seçin ve ardından düğümü eklemek istediğiniz *ölçek birimini* seçin. Ayrıca, eklemekte olduğunuz ölçek birimi düğümünün *BMC IP adresini* belirtin. Tek seferde yalnızca bir düğüm ekleyebilirsiniz.
   ![Düğüm ayrıntıları Ekle](media/azure-stack-add-scale-node/select-node2.png)
 

### <a name="use-powershell"></a>PowerShell kullanma

Bir düğüm eklemek için **New-AzsScaleUnitNodeObject** cmdlet 'ini kullanın.  

Aşağıdaki örnek PowerShell betiklerinden birini kullanmadan önce, değer *düğümü adlarını* ve *IP adreslerini* Azure Stack ortamınızdaki değerlerle değiştirin.

  > [!Note]  
  > Bir düğümü adlandırırken, adı 15 karakterden az olacak şekilde saklamanız gerekir. `\`Ayrıca, boşluk içeren veya şu karakterlerden herhangi birini içeren bir ad kullanamazsınız:, `<` `>` `"` `:` `*` `/`,,, `?`,,,, `|`, `\`, `~`, `!`, `@`, `#`, `$`, `%`, `^`, `&`, `(`, `)`, `{`, `}`, `_`.

**Düğüm ekleyin:**
  ```powershell
  ## Add a single Node 
  $NewNode=New-AzsScaleUnitNodeObject -computername "<name_of_new_node>" -BMCIPv4Address "<BMCIP_address_of_new_node>" 
 
  Add-AzsScaleUnitNode -NodeList $NewNode -ScaleUnit "<name_of_scale_unit_cluster>" 
  ```  

## <a name="monitor-add-node-operations"></a>Düğüm ekleme işlemlerini izleme 
Düğüm ekleme işleminin durumunu almak için yönetim portalı 'nı veya PowerShell 'i kullanın. Düğüm ekleme işlemlerinin tamamlanması birkaç saat sürebilir.

### <a name="use-the-admin-portal"></a>Yönetici portalını kullanma 
Yeni bir düğümün eklenmesini izlemek için, yönetim portalındaki ölçek birimini veya ölçek birimi düğüm nesnelerini gözden geçirin. Bunu yapmak için **Bölge yönetimi** > **ölçek birimleri**' ne gidin. Ardından, gözden geçirmek istediğiniz ölçek birimini veya ölçek birimi düğümünü seçin. 

### <a name="use-powershell"></a>PowerShell kullanma
Ölçek birimi ve ölçek birimi düğümlerinin durumu aşağıdaki gibi PowerShell kullanılarak alınabilir:
  ```powershell
  #Retrieve Status for the Scale Unit
  Get-AzsScaleUnit|select name,state
 
  #Retrieve Status for each Scale Unit Node
  Get-AzsScaleUnitNode |Select Name, ScaleUnitNodeStatus
```

### <a name="status-for-the-add-node-operation"></a>Düğüm ekleme işleminin durumu 
**Ölçek birimi için:**

|Durum               |Açıklama  |
|---------------------|---------|
|Çalışıyor              |Tüm düğümler, ölçek birimine etkin bir şekilde katılıyor.|
|Durduruldu              |Ölçek birimi düğümü çalışmıyor veya ulaşılamaz durumda.|
|Genişletip            |Bir veya daha fazla ölçek birimi düğümü şu anda işlem kapasitesi olarak ekleniyor.|
|Depolamayı yapılandırma  |İşlem kapasitesi genişletildi ve depolama yapılandırması çalışıyor.|
|Düzeltme gerektirir |Bir veya daha fazla ölçek birimi düğümünün onarılabilmesini gerektiren bir hata algılandı.|


**Ölçek birimi düğümü için:**

|Durum                |Açıklama  |
|----------------------|---------|
|Çalışıyor               |Düğüm, ölçek birimine etkin bir şekilde katılıyor.|
|Durduruldu               |Düğüm kullanılamıyor.|
|Ekleniyor                |Düğüm, ölçek birimine etkin bir şekilde ekleniyor.|
|Onarılıyor             |Düğüm, etkin bir şekilde onarıldı.|
|Bakım           |Düğüm duraklatılır ve etkin bir kullanıcı iş yükü çalışmıyor. |
|Düzeltme gerektirir  |Düğümün onarılması gereken bir hata algılandı.|


## <a name="troubleshooting"></a>Sorun giderme
Düğüm eklenirken görülen yaygın sorunlar aşağıda verilmiştir. 

**Senaryo 1:**  Ölçek birim düğümü ekleme işlemi başarısız oluyor, ancak bir veya daha fazla düğüm durdurulmuş durumuyla listelendi.  
- Düzeltmesi Bir veya daha fazla düğümü onarmak için onarım işlemini kullanın. Tek seferde yalnızca bir onarım işlemi çalıştırılabilir.

**Senaryo 2:** Bir veya daha fazla ölçek birimi düğümü eklendi, ancak depolama genişletmesi başarısız oldu. Bu senaryoda, ölçek birimi düğüm nesnesi çalışma durumunu bildirir ancak depolama alanını yapılandırma görevi başlatılmaz.  
- Düzeltmesi Aşağıdaki PowerShell cmdlet 'ini çalıştırarak depolama durumunu gözden geçirmek için ayrıcalıklı uç noktayı kullanın:
  ```powershell
     Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
  ```
 
**Senaryo 3:** Depolama ölçeği genişletme işinin başarısız olduğunu belirten bir uyarı aldınız.  
- Düzeltmesi Bu durumda, depolama yapılandırması görevi başarısız oldu. Bu sorun, desteğe başvurmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar 
[Genel IP adresleri ekleme](azure-stack-add-ips.md) 
