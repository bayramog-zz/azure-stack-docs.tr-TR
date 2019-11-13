---
title: Birim düğümü eylemlerini Azure Stack ölçeklendirme | Microsoft Docs
description: Güç açma, kapatma, devre dışı bırakma, sürdürülme ve Azure Stack tümleşik sistemlerde düğüm durumunun nasıl görüntüleneceği dahil olmak üzere ölçek birimi düğüm eylemleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 5bc65418c1dd098dc8784cb9bf306676a1cd79ed
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955285"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Azure Stack birim düğümü eylemlerini ölçeklendirme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, bir ölçek biriminin durumunun nasıl görüntüleneceği açıklanır. Birimin düğümlerini görüntüleyebilirsiniz. Güç açma, kapatma, kapatma, boşaltma, yeniden başlatma ve onarma gibi düğüm eylemlerini çalıştırabilirsiniz. Genellikle, bu düğüm eylemlerini alan değiştirme sırasında veya bir düğümü kurtarmaya yardımcı olacak şekilde kullanırsınız.

> [!Important]  
> Bu makalede açıklanan tüm düğüm eylemleri tek seferde bir düğüm hedeflemelidir.

## <a name="view-the-node-status"></a>Düğüm durumunu görüntüleme

Yönetici portalında, bir ölçek biriminin ve ilişkili düğümlerinin durumunu görüntüleyebilirsiniz.

Ölçek biriminin durumunu görüntülemek için:

1. **Bölge yönetimi** kutucuğunda bölge ' yi seçin.
2. Sol tarafta, **altyapı kaynakları**altında **ölçek birimleri**' ni seçin.
3. Sonuçlarda ölçek birimini seçin.
4. Sol tarafta, **genel**altında **düğümler**' i seçin.

   Aşağıdaki bilgileri görüntüleyin:

   - Tek tek düğümlerin listesi.
   - İşlemsel durum (aşağıdaki listeye bakın).
   - Güç durumu (çalışıyor veya durduruldu).
   - Sunucu modeli.
   - Temel kart yönetim denetleyicisi (BMC) IP adresi.
   - Toplam çekirdek sayısı.
   - Toplam bellek miktarı.

![ölçek biriminin durumu](media/azure-stack-node-actions/multinodeactions.png)

### <a name="node-operational-states"></a>Düğüm işletimsel durumlar

| Durum | Açıklama |
|----------------------|-------------------------------------------------------------------|
| Çalışıyor | Düğüm, ölçek birimine etkin bir şekilde katılıyor. |
| Durdurulan | Düğüm kullanılamıyor. |
| Eklemektir | Düğüm, ölçek birimine etkin bir şekilde ekleniyor. |
| Onar | Düğüm, etkin bir şekilde onarıldı. |
| Bakım | Düğüm duraklatılır ve etkin bir kullanıcı iş yükü çalışmıyor. |
| Düzeltme gerektirir | Düğümün onarılması gereken bir hata algılandı. |

## <a name="scale-unit-node-actions"></a>Birim düğümü eylemlerini ölçeklendirme

Bir ölçek birimi düğümüyle ilgili bilgileri görüntülediğinizde, şunun gibi düğüm eylemleri de gerçekleştirebilirsiniz:

 - Başlat ve Durdur (geçerli güç durumuna bağlı olarak).
 - Devre dışı bırak ve sürdürülür (işlemler durumuna bağlı olarak).
 - Onarımı.
 - Event.

Düğümün işlemsel durumu hangi seçeneklerin kullanılabilir olduğunu belirler.

PowerShell modüllerini Azure Stack yüklemeniz gerekir. Bu cmdlet 'ler **AZS. Fabric. admin** modülüdür. Azure Stack için PowerShell yüklemenizi yüklemek veya doğrulamak için bkz. PowerShell 'i [Azure Stack için](azure-stack-powershell-install.md)yükleme.

## <a name="stop"></a>Durdur

**Durdur** eylemi düğümü kapatır. Bu, güç düğmesine basmayla aynıdır. İşletim sistemine bir kapalı sinyal göndermez. Planlı durdurma işlemleri için, önce her zaman kapatılma işlemini deneyin.

Bu eylem, genellikle bir düğüm askıda durumdaysa ve isteklere yanıt vermemesi durumunda kullanılır.

Durdur eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

```powershell  
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Durma eyleminin çalışmamasının olası olmaması durumunda işlemi yeniden deneyin ve ikinci kez başarısız olursa, bunun yerine BMC Web arabirimini kullanın.

Daha fazla bilgi için bkz. [stop-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/stop-azsscaleunitnode).

## <a name="start"></a>Başlayın

**Başlangıç** eylemi, düğümü açar. Bu, güç düğmesine basdüğmesidir.

Başlatma eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

```powershell  
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Başlatma eyleminin işe yaramasının olası olmaması durumunda işlemi yeniden deneyin. İkinci bir kez başarısız olursa, bunun yerine BMC Web arabirimini kullanın.

Daha fazla bilgi için bkz. [Start-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/start-azsscaleunitnode).

## <a name="drain"></a>Drena

**Boşalt** eylemi, tüm etkin iş yüklerini söz konusu ölçek birimindeki kalan düğümlere taşımıştır.

Bu eylem, genellikle tüm düğümün yerini değiştirme gibi bölümlerin yerini değiştirme sırasında kullanılır.

> [!Important]
> Kullanıcıların bilgilendirildi planlı bakım penceresi sırasında düğüm üzerinde bir boşaltma işlemi kullandığınızdan emin olun. Bazı koşullarda, etkin iş yükleri kesintiye karşı karşılaşabilir.

Boşalt eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

```powershell  
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Daha fazla bilgi için bkz. [Disable-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/disable-azsscaleunitnode).

## <a name="resume"></a>Sürdür

**Devam** eylemi devre dışı bırakılmış bir düğümü sürdürür ve iş yükü yerleşimi için etkin olarak işaretler. Düğümde çalışan önceki iş yükleri yeniden çalışmaz. (Bir düğümde boşaltma işlemi kullanıyorsanız, kapandığınızdan emin olun. Düğümü yeniden açtığınızda iş yükü yerleştirmesi için etkin olarak işaretlenmemiştir. Hazırsanız, düğümü etkin olarak işaretlemek için, geri dön eylemini kullanmanız gerekir.)

Özgeçmişi eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

```powershell  
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

Daha fazla bilgi için bkz. [Enable-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/enable-azsscaleunitnode).

## <a name="repair"></a>Onarımı

> [!CAUTION]  
> Üretici yazılımı seviyelendirme, bu makalede açıklanan işlemin başarısı için önemlidir. Bu adımın eksik olması, sistem kararsızlığına, performans, güvenlik iş parçacıkları veya Azure Stack Automation işletim sistemini dağıtırken hata oluşmasına neden olabilir. Uygulanan üretici yazılımının [Azure Stack yönetici portalında](azure-stack-updates.md)görünen OEM sürümüyle eşleştiğinden emin olmak için donanımı değiştirirken her zaman donanım iş ortağınızın belgelerine danışın.<br><br>
Daha fazla bilgi ve iş ortağı belgelerinin bağlantıları için bkz. [bir donanım bileşenini değiştirme](azure-stack-replace-component.md).

| Donanım Iş ortağı | Bölge | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | Tümü | [Microsoft Azure Stack Işlemler Kılavuzu için Cisco Tümleşik Sistem](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb)<br><br>[Microsoft Azure Stack için Cisco Tümleşik sistemi sürüm notları](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) |
| Dell EMC | Tümü | [Microsoft Azure Stack 14G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Microsoft Azure Stack 13G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | Japonya | [Fujitsu yönetilen hizmet destek masası (hesap ve oturum açma gereklidir)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | EMEA | [Fujitsu BT ürünlerini ve sistemlerini destekler](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) |
|  |  | [Fujitsu MySupport (hesap ve oturum açma gereklidir)](https://support.ts.fujitsu.com/IndexMySupport.asp) |
| HPE | Tümü | [Microsoft Azure Stack için HPE ProLiant](http://www.hpe.com/info/MASupdates) |
| Lenovo | Tümü | [Ölçülü Kagile SXD En Iyi Tarifler](https://datacentersupport.lenovo.com/us/en/solutions/ht505122) |

**Onarım** eylemi bir düğümü onarır. Bunu yalnızca aşağıdaki senaryolardan biri için kullanın:

- Tam düğüm değiştirme (yeni veri diskleri ile veya olmayan).
- Donanım bileşeni hatasından ve değiştirildikten sonra (alan değiştirilebilir birim [FRU] belgelerinde belirtilmişse).

> [!Important]  
> Bir düğümü veya tek bir donanım bileşenini değiştirmeniz gerektiğinde, tam adımlar için OEM Donanım satıcınızın FRU belgelerine bakın. FRU belgeleri, bir donanım bileşenini değiştirdikten sonra onarım eylemini çalıştırmanız gerekip gerekmediğini belirtir.

Onarma eylemini çalıştırdığınızda BMC IP adresini belirtmeniz gerekir.

Onarma eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

  ```powershell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPv4Address <BMCIPv4Address>
  ```

## <a name="shutdown"></a>Kapat

Bu **işlemi ilk önce, tüm** etkin iş yüklerini aynı ölçek birimindeki kalan düğümlere taşımakta. Ardından Eylem, ölçek birimi düğümünü düzgün bir şekilde kapatır.

Kapatılmış bir düğümü başlattıktan sonra, [özgeçmişi](#resume) eylemini çalıştırmanız gerekir. Düğümde çalışan önceki iş yükleri yeniden çalışmaz.

Kapatılma işlemi başarısız olursa, [boşaltma](#drain) işlemini ve sonra da kapalı işlemini deneyin.

Kapalı eylemini çalıştırmak için, yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet 'i çalıştırın:

  ```powershell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -Shutdown
  ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Fabric işleç modülü hakkında bilgi edinin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.6.0).