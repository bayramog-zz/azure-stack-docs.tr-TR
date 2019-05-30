---
title: Azure Stack'te desteklenen VM boyutları | Microsoft Docs
description: Azure stack'teki desteklenen VM boyutları için başvuru.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: a7ec54d88a259b883f72c2396ab572118ba96a23
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269435"
---
# <a name="vm-sizes-supported-in-azure-stack"></a>Azure Stack'te desteklenen VM boyutları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack'te kullanılabilir sanal makine (VM) boyutları listeler.

Disk IOPS (giriş/çıkış işlem / saniye) Azure Stack'te bir VM boyutu diskin türünü yerine işlevidir. Bu SSD veya HDD, disk türünü seçmenize bakılmaksızın Standard_Fs serisi VM için tek bir ek veri diski IOPS sınırı 2300 IOPS anlamına gelir. IOPS sınır uygulanmaz gürültücü Komşuları için üst sınır (maksimum olası) olur. Belirli bir VM boyutuna elde edecekleriniz IOPS bir güvencesi öyle.

## <a name="vm-general-purpose"></a>VM genel amaçlı

Dengeli CPU / bellek oranı genel amaçlı bir VM boyutları sunar. Bunlar test ve geliştirme, küçük ve orta büyüklükte veri tabanları ve düşük, orta düzey trafiğe sahip web sunucuları için kullanılır. Her veri diski hariç temel A serisi, premium VM boyutları için 2300 IOPS ' dir. Temel A veri disk boyutu 500 IOPS ' dir.

### <a name="basic-a"></a>Temel A

> [!NOTE]
> *Temel A* VM boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](../operator/azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın.

|Boyutu - boyut\ad |vCPU     |Bellek | En fazla geçici disk boyutu | En yüksek işletim sistemi disk aktarım hızı: (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski aktarım hızı (IOPS) | En fazla NIC |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1    |768 MB   | 20 GB   |300  | 300  |1 / 1 x 300  |1   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2 / 2 x 300  |1   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4 / 4 x 300  |1   |
|**A3\Basic_A3**  |4    |7 GB     | 110 GB  |300  | 300  |8 / 8 x 300  |1   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |16 / 16 X 300 |1   |

### <a name="standard-a"></a>Standart A 
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1 |0,768 |20  |500 |500 |1x500  |1 |
|**Standard_A1** |1 |1,75  |70  |500 |500 |2x500  |1 |
|**Standard_A2** |2 |3,5   |135 |500 |500 |4x500  |1 |
|**Standard_A3** |4 |7     |285 |500 |500 |8x500  |2 |
|**Standard_A4** |8 |14    |605 |500 |500 |16x500 |4 |
|**Standard_A5** |2 |14    |135 |500 |500 |4x500  |2 |
|**Standard_A6** |4 |28    |285 |500 |500 |8x500  |2 |
|**Standard_A7** |8 |56    |605 |500 |500 |16x500 |4 |

### <a name="av2-series"></a>Av2 Serisi
*Azure Stack 1804 veya sonraki bir sürümü gerektirir*

|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|-----------------|----|----|-----|-----|------|--------------|---------|
|**Standard_A1_v2**  |1   |2   |10   |500 |1000  |2/2x500   |2 |
|**Standard_A2_v2**  |2   |4   |20   |500 |2000  |4/4x500   |2 |
|**Standard_A4v2**   |4   |8   |40   |500 |4000  |8/8x500   |4 |
|**Standard_A8_v2**  |8   |16  |80   |500 |8000  |16/16x500 |8 |
|**Standard_A2m_v2** |2   |16  |20   |500 |2000  |4/4x500   |2 |
|**Standard_A4m_v2** |4   |32  |40   |500 |4000  |8/8x500   |4 |
|**Standard_A8m_v2** |8   |64  |80   |500 |8000  |16/16x500 |8 |

### <a name="d-series"></a>D Serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|----------------|----|----|-----|----|------|------------|---------|
|**İşler için standart_d1** |1   |3,5 |50   |500 |3000  |4/4x500   |1 |
|**Standard_D2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**İşler için standart_d3** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32/32x500 |8 |


### <a name="ds-series"></a>DS serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1   |3,5 |7    |1000 |4000  |4 / 4 x 2300   |1 |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8 / 8 x 2300   |2 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16 / 16 x 2300 |4 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32 / 32 x 2300 |8 |

### <a name="dv2-series"></a>Dv2 Serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4/4x500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32/32x500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64 x 500 |8 |

### <a name="dsv2-series"></a>DSv2 serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1000 |4000  |4 / 4 x 2300   |1 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8 x 2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16 x 2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32 x 2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64 x 2300 |8 |


## <a name="compute-optimized"></a>İşlem için iyileştirilmiş
### <a name="f-series"></a>F Serisi
*Azure Stack 1804 veya sonraki bir sürümü gerektirir*

|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|-----------------|----|----|-----|----|------|------------|---------|
|**İşler için standart_f1**  |1   |2   |16   |500 |3000  |4/4x500   |2 |
|**İşler için standart_f2**  |2   |4   |32   |500 |6000  |8/8x500   |2 |
|**İşler için standart_f4**  |4   |8   |64   |500 |12000 |16/16x500 |4 |
|**İşler için standart_f8**  |8   |16  |128  |500 |24000 |32/32x500 |8 |
|**İşler için standart_f16** |16  |32  |256  |500 |48000 |64 / 64 x 500 |8 |


### <a name="fs-series"></a>Fs serisi
*Azure Stack 1804 veya sonraki bir sürümü gerektirir*  

|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|------------------|----|----|----|-----|------|-------------|---------|
|**Standard_F1s**  |1   |2   |4   |1000 |4000  |4 / 4 x 2300   |2 |
|**Standard_F2s**  |2   |4   |8   |1000 |8000  |8 / 8 x 2300   |2 |
|**Standard_F4s**  |4   |8   |16  |1000 |16000 |16 / 16 x 2300 |4 |
|**Standard_F8s**  |8   |16  |32  |1000 |32000 |32 / 32 x 2300 |8 |
|**Standard_F16s** |16  |32  |64  |1000 |64000 |64 / 64 x 2300 |8 |


### <a name="fsv2-series"></a>Fsv2-serisi
*Azure Stack 1804 veya sonraki bir sürümü gerektirir* 

|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_F2s_v2**  |2   |4   |16   |1000 |4000   |4 / 4 x 2300    |2 |
|**Standard_F4s_v2**  |4   |8   |32   |1000 |8000   |8 / 8 x 2300    |4 |
|**Standard_F8s_v2**  |8   |16  |64   |1000 |16000  |16 / 16 x 2300  |8 |
|**Standard_F16s_v2** |16  |32  |128  |1000 |32000  |32 / 32 x 2300  |8 |
|**Standard_F32s_v2** |32  |64  |256  |1000 |64000  |32 / 32 x 2300  |8 |
|**Standard_F64s_v2** |64  |128 |512  |1000 |128000 |32 / 32 x 2300  |8 |


## <a name="memory-optimized"></a>Bellek için iyileştirilmiş

Belleği en iyi duruma getirilmiş VM boyutları, ilişkisel veritabanı sunucuları, Orta veya büyük boyutlu önbellekler ve bellek içi analiz için tasarlanmış yüksek bellek CPU oranı sağlayın.

### <a name="mo-d"></a>D serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_F1s**  |2  |14  |100 |500     |6000  |8/8x500   |2 |
|**İşler için standart_d12**  |4  |28  |200 |500     |12000 |16/16x500 |4 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32/32x500 |8 |
|**İşler için standart_d14**  |16 |112 |800 |500     |48000 |64 / 64 x 500 |8 |

### <a name="mo-ds"></a>DS serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8 / 8 x 2300   |2 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16 / 16 x 2300 |4 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32 / 32 x 2300 |8 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64 / 64 x 2300 |8 |

### <a name="mo-dv2"></a>Dv2 serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8/8x500    |2 |
|**İşler için standart_d12_v2** |4   |28  |200  |500 |12000  |16/16x500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32/32x500  |8 |
|**İşler için standart_d14_v2** |16  |112 |800  |500 |48000  |64 / 64 x 500  |8 |


### <a name="mo-dsv2"></a>DSv2 serisi
|Boyutlandır     |vCPU     |Bellek (GiB) | Geçici depolama alanı (GiB)  | En yüksek işletim sistemi disk aktarım hızı (IOPS) | Maksimum geçici depolama aktarım hızı (IOPS) | Maksimum veri diski / aktarım hızı (IOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4 / 4 x 2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8 / 8 x 2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16 / 16 x 2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32 / 32 x 2300  |8 |


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack VM özellikleri](azure-stack-vm-considerations.md)
