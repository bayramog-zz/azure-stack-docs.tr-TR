---
title: ASDK mimarisi | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) mimarisi hakkında bilgi edinin.
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
ms.openlocfilehash: 5a34061b1fa6cd30f3bbf9f9780b13c01f0a4866
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579089"
---
# <a name="asdk-architecture"></a>ASDK mimarisi
Azure Stack Geliştirme Seti (ASDK), tek bir ana bilgisayarda çalışan Azure Stack tek düğümlü bir dağıtımdır. Edge yönlendirme bileşenleri, Azure Stack için NAT ve VPN özellikleri sağlamak üzere ana bilgisayara yüklenir. Azure Stack altyapı rolleri fiziksel ana bilgisayarın Hyper-V katmanında çalışır.


## <a name="virtual-machine-roles"></a>Sanal makine rolleri
ASDK, geliştirme seti ana bilgisayarında barındırılan aşağıdaki VM 'Leri kullanarak hizmetler sunar:

| Name | Açıklama |
| ----- | ----- |
| **AzS-ACS01** | Depolama Hizmetleri Azure Stack.|
| **AzS-ADFS01** | Active Directory Federasyon Hizmetleri (AD FS) (ADFS).  |
| **AzS-CA01** | Azure Stack rol hizmetleri için sertifika yetkilisi Hizmetleri.|
| **AzS-DC01** | Microsoft Azure Stack için Active Directory, DNS ve DHCP hizmetleri.|
| **AzS-ERCS01** | Acil durum kurtarma konsolu VM 'si. |
| **AzS-GWY01** | Kiracı ağları için VPN siteden siteye bağlantıları gibi kenar ağ geçidi Hizmetleri.|
| **AzS-NC01** | Azure Stack ağ hizmetlerini yöneten ağ denetleyicisi.  |
| **AzS-SLB01** | Hem kiracılar hem de Azure Stack altyapı hizmetleri için Azure Stack içinde yük dengeleme çoğullama Hizmetleri.  |
| **AzS-SQL01** | Azure Stack altyapı rolleri için iç veri deposu.  |
| **AzS-WAS01** | Yönetici portalı ve Azure Resource Manager hizmetleri Azure Stack.|
| **AzS-WASP01**| Azure Stack Kullanıcı (kiracı) portalı ve Azure Resource Manager hizmetleri.|
| **AzS-XRP01** | Işlem, ağ ve depolama kaynak sağlayıcıları da dahil olmak üzere Microsoft Azure Stack için altyapı yönetimi denetleyicisi.|
| **AzS-SRNG01** | Azure Stack için günlük toplama hizmetini barındıran halka sanal makinesini destekler. |

## <a name="next-steps"></a>Sonraki adımlar
[Temel ASDK yönetici görevleri hakkında bilgi edinin](asdk-admin-basics.md)
