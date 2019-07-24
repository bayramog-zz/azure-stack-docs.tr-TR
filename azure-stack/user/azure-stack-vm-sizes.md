---
title: Azure Stack desteklenen VM boyutları | Microsoft Docs
description: Azure Stack 'de desteklenen VM boyutları için başvuru.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 7c2fd7f67c036cba921069950bd9c0d6d37b97b4
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418430"
---
# <a name="vm-sizes-supported-in-azure-stack"></a>Azure Stack desteklenen VM boyutları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack kullanılabilen sanal makine (VM) boyutları listelenir.

Azure Stack üzerinde disk ıOPS (saniye başına giriş/çıkış Işlemi), disk türü yerine VM boyutunun bir işlevidir. Bu, bir Standard_Fs serisi VM için, SSD veya HDD 'yi disk türü için seçip seçmeksizin bağımsız olarak, tek bir ek veri diski için ıOPS sınırının 2300 ıOPS olduğunu gösterir. Uygulanan ıOPS sınırları, gürültülü komşuları engellemek için bir sınır (en fazla olası) olur. Bu, belirli bir VM boyutuna alacağınız ıOPS 'nin bir güvencesi değildir.

## <a name="vm-general-purpose"></a>VM genel amacı

Genel amaçlı VM boyutları, dengeli bir CPU-bellek oranı sağlar. Bunlar, test ve geliştirme, küçük ve orta ölçekli veritabanları ve orta düzey trafik Web sunucuları için kullanılır. Her veri diski, temel bir seriler dışında, Premium VM boyutları için 2300 ıOPS 'dir. Temel A için, veri diski boyutu 500 ıOPS 'dir.

### <a name="basic-a"></a>Temel A

> [!NOTE]
> *Temel A* VM boyutları, Portal üzerinden [Sanal Makine Ölçek Kümeleri](../operator/azure-stack-compute-add-scalesets.md) (VMSS) oluşturmak için kullanımdan kaldırıldı. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın.

|Boyut-Size\Name |Sanal işlemci     |Bellek | En fazla geçici disk boyutu | En yüksek işletim sistemi disk aktarım hızı: 'YE | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski verimlilik (ıOPS) | En fazla NIC |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1\.    |768 MB   | 20 GB   |300  | 300  |1/1x300  |2   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2/2x300  |2   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4/4x300  |2   |
|**A3\Basic_A3**  |4    |7 GB     | 110 GB  |300  | 300  |8/8x300  |2   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |16/16X300 |2   |

### <a name="standard-a"></a>Standart A 
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1\. |0,768 |20  |500 |500 |1x500  |2 |
|**Standard_A1** |1\. |1,75  |70  |500 |500 |2x500  |2 |
|**Standard_A2** |2 |3,5   |135 |500 |500 |4x500  |2 |
|**Standard_A3** |4 |7     |285 |500 |500 |8x500  |2 |
|**Standard_A4** |8 |14    |605 |500 |500 |16x500 |4 |
|**Standard_A5** |2 |14    |135 |500 |500 |4x500  |2 |
|**Standard_A6** |4 |28    |285 |500 |500 |8x500  |2 |
|**Standard_A7** |8 |56    |605 |500 |500 |16x500 |4 |

### <a name="av2-series"></a>Av2 Serisi
*Azure Stack sürüm 1804 veya üstünü gerektirir*

|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-----------------|----|----|-----|-----|------|--------------|---------|
|**Standard_A1_v2**  |1\.   |2   |10   |500 |1000  |2/2x500   |2 |
|**Standard_A2_v2**  |2   |4   |20   |500 |2000  |4/4x500   |2 |
|**Standard_A4v2**   |4   |8   |40   |500 |4000  |8/8x500   |4 |
|**Standard_A8_v2**  |8   |16  |80   |500 |8000  |16/16x500 |8 |
|**Standard_A2m_v2** |2   |16  |20   |500 |2000  |4/4x500   |2 |
|**Standard_A4m_v2** |4   |32  |40   |500 |4000  |8/8x500   |4 |
|**Standard_A8m_v2** |8   |64  |80   |500 |8000  |16/16x500 |8 |

### <a name="d-series"></a>D Serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|----------------|----|----|-----|----|------|------------|---------|
|**Standard_D1** |1\.   |3,5 |50   |500 |3000  |4/4x500   |2 |
|**Standard_D2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**Standard_D3** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32/32x500 |8 |


### <a name="ds-series"></a>DS serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1\.   |3,5 |7    |1000 |4000  |4/4x2300   |2 |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8/8x2300   |2 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16/16x2300 |4 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32/32x2300 |8 |

### <a name="dv2-series"></a>Dv2 Serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1\.   |3,5 |50   |500 |3000  |4/4x500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32/32x500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64/64x500 |8 |

### <a name="dsv2-series"></a>DSv2 serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1\.   |3,5 |7   |1000 |4000  |4/4x2300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8/8x2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16/16x2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32/32x2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64/64x2300 |8 |


## <a name="compute-optimized"></a>İşlem için iyileştirilmiş
### <a name="f-series"></a>F Serisi
*Azure Stack sürüm 1804 veya üstünü gerektirir*

|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-----------------|----|----|-----|----|------|------------|---------|
|**Standard_F1**  |1\.   |2   |16   |500 |3000  |4/4x500   |2 |
|**Standard_F2**  |2   |4   |32   |500 |6000  |8/8x500   |2 |
|**Standard_F4**  |4   |8   |64   |500 |12000 |16/16x500 |4 |
|**Standard_F8**  |8   |16  |128  |500 |24000 |32/32x500 |8 |
|**Standard_F16** |16  |32  |256  |500 |48000 |64/64x500 |8 |


### <a name="fs-series"></a>Fs serisi
*Azure Stack sürüm 1804 veya üstünü gerektirir*  

|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|------------------|----|----|----|-----|------|-------------|---------|
|**Standard_F1s**  |1\.   |2   |4   |1000 |4000  |4/4x2300   |2 |
|**Standard_F2s**  |2   |4   |8   |1000 |8000  |8/8x2300   |2 |
|**Standard_F4s**  |4   |8   |16  |1000 |16000 |16/16x2300 |4 |
|**Standard_F8s**  |8   |16  |32  |1000 |32000 |32/32x2300 |8 |
|**Standard_F16s** |16  |32  |64  |1000 |64000 |64/64x2300 |8 |


### <a name="fsv2-series"></a>Fsv2-serisi
*Azure Stack sürüm 1804 veya üstünü gerektirir* 

|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_F2s_v2**  |2   |4   |16   |1000 |4000   |4/4x2300    |2 |
|**Standard_F4s_v2**  |4   |8   |32   |1000 |8000   |8/8x2300    |2 |
|**Standard_F8s_v2**  |8   |16  |64   |1000 |16000  |16/16x2300  |4 |
|**Standard_F16s_v2** |16  |32  |128  |1000 |32000  |32/32x2300  |4 |
|**Standard_F32s_v2** |32  |64  |256  |1000 |64000  |32/32x2300  |8 |
|**Standard_F64s_v2** |64  |128 |512  |1000 |128000 |32/32x2300  |8 |


## <a name="memory-optimized"></a>Bellek için iyileştirilmiş

Bellek için iyileştirilmiş VM boyutları, ilişkisel veritabanı sunucuları, orta ve büyük önbellekler ve bellek içi analizler için tasarlanan yüksek bellekten CPU oranı sağlar.

### <a name="mo-d"></a>D serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_D11**  |2  |14  |100 |500     |6000  |8/8x500   |2 |
|**Standard_D12**  |4  |28  |200 |500     |12000 |16/16x500 |4 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32/32x500 |8 |
|**Standard_D14**  |16 |112 |800 |500     |48000 |64/64x500 |8 |

### <a name="mo-ds"></a>DS serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8/8x2300   |2 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16/16x2300 |4 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32/32x2300 |8 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64/64x2300 |8 |

### <a name="mo-dv2"></a>Dv2 serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8/8x500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16/16x500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32/32x500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64/64x500  |8 |


### <a name="mo-dsv2"></a>DSv2 serisi
|Size     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4/4x2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8/8x2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16/16x2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32/32x2300  |8 |


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack VM özellikleri](azure-stack-vm-considerations.md)
