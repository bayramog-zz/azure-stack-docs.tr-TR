---
title: Azure Stack Geliştirme Seti mimarisi | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) mimarisini açıklar.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 06/28/2019
ms.openlocfilehash: 4cd20ed4c76b062ea2fcfdf3d314ef2bcdecaff0
ms.sourcegitcommit: eccbd0098ef652919f357ef6dba62b68abde1090
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67492341"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Microsoft Azure Stack Geliştirme Seti mimarisi
Azure Stack geliştirme Seti'ni (ASDK) Azure Stack tek ana bilgisayar üzerinde çalışan tek düğümlü dağıtımıdır. Azure Stack için NAT ve VPN özellikler sağlamak için ana bilgisayarda Edge yönlendirme bileşenleri yüklenir. Azure Stack altyapısını rolleri Hyper-V fiziksel ana bilgisayar katmanındaki çalıştırın.


## <a name="virtual-machine-roles"></a>Sanal makine rolleri
ASDK Geliştirme Seti ana bilgisayarda bulunan aşağıdaki sanal makineleri kullanarak hizmetleri sunar:

| Ad | Açıklama |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack depolama hizmetleri.|
| **AzS-ADFS01** | Active Directory Federasyon Hizmetleri (ADFS).  |
| **AzS-CA01** | Azure Stack rol hizmetleri için sertifika yetkilisi Hizmetleri.|
| **AzS-DC01** | Active Directory, DNS ve DHCP hizmetleri Microsoft Azure Stack için.|
| **AzS-ERCS01** | Acil Durum Kurtarma Konsolu VM. |
| **AzS-GWY01** | Sınır ağ geçidi, VPN siteden siteye bağlantıları için Kiracı ağları gibi hizmetler.|
| **AzS-NC01** | Ağ denetleyicisi, Azure Stack ağ hizmetlerini yönetir.  |
| **AzS-SLB01** | Hem Kiracı hem de Azure Stack altyapı hizmetleri için Dengeleme Azure Stack'te çoğaltıcı hizmetlerini yükleyin.  |
| **AzS-SQL01** | Azure Stack altyapısını rolleri için iç verileri depolar.  |
| **AzS-WAS01** | Azure Stack Yönetim Portalı ve Azure Resource Manager Hizmetleri.|
| **AzS-WASP01**| Azure Stack (Kiracı) Kullanıcı Portalı ve Azure Resource Manager Hizmetleri.|
| **AzS-XRP01** | İşlem, ağ ve depolama kaynak sağlayıcıları dahil, Microsoft Azure Stack için altyapı yönetim denetleyicisi.|
| **AzS-SRNG01** | Halka Azure Stack için günlük toplama hizmetini barındıran VM destekler. |

## <a name="next-steps"></a>Sonraki adımlar
[Temel ASDK yönetim görevleri hakkında bilgi edinin](asdk-admin-basics.md)
