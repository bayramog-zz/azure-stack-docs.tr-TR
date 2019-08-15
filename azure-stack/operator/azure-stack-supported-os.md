---
title: Azure Stack için desteklenen konuk işletim sistemleri | Microsoft Docs
description: Bu konuk işletim sistemleri Azure Stack kullanılabilir.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 06/06/2019
ms.openlocfilehash: 98baf8217bc245f7aa6ac7bc0e2b8f8a5284c6d6
ms.sourcegitcommit: 86e2b776383a9f761072199ec6b6b8a7d2e6b798
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69021839"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure Stack desteklenen konuk işletim sistemleri

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="windows"></a>Windows

Azure Stack, aşağıdaki tabloda listelenen Windows Konuk işletim sistemlerini destekler:

| İşletim sistemi | Açıklama | Market 'te kullanılabilir |
| --- | --- | --- |
| Windows Server, sürüm 1709 | 64 bit | Kapsayıcılarla çekirdek |
| Windows Server 2019 | 64 bit |  Veri Merkezi, veri merkezi çekirdeği, kapsayıcılar içeren veri merkezi |
| Windows Server 2016 | 64 bit |  Veri Merkezi, veri merkezi çekirdeği, kapsayıcılar içeren veri merkezi |
| Windows Server 2012 R2 | 64 bit |  Veri merkezi |
| Windows Server 2012 | 64 bit |  Veri merkezi |
| Windows Server 2008 R2 SP1 | 64 bit |  Veri merkezi |
| Windows Server 2008 SP2 | 64 bit |  Kendi görüntünüzü getir |
| Windows 10 *(bkz. Note 1)* | 64-bit, Pro ve Enterprise | Kendi görüntünüzü getir |

> [!NOTE]
> Windows 10 istemci işletim sistemlerini Azure Stack dağıtmak için, [Windows Kullanıcı başına lisanslama](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) veya nitelikli bir çok kiracılı barındırıcı ([qmth](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)) aracılığıyla satın alma hakkına sahip olmanız gerekir.

Market görüntüleri, Kullandıkça Öde veya KLG (EA/SPLA) lisanslaması için kullanılabilir. Tek bir Azure Stack örneğinde birlikte kullanılması desteklenmez. Dağıtım sırasında, Konuk aracısının uygun bir sürümünü görüntüye çıkartır Azure Stack.

Datacenter sürümleri, pazarda indirmek üzere kullanılabilir; müşteriler, diğer sürümleri de dahil olmak üzere kendi sunucu görüntülerini getirebilir. Windows istemci görüntüleri Market 'te yok.

## <a name="linux"></a>Linux

Market 'te kullanılabilir olarak listelenen Linux dağıtımları, gerekli Windows Azure Linux Aracısı 'nı (WALA) içerir. Kendi görüntünüzü Azure Stack yaparsanız, [Azure Stack Için Linux görüntüleri ekleme](azure-stack-linux.md)bölümündeki yönergeleri izleyin.

> [!NOTE]
> Özel görüntüler en son ortak WALA sürümü ile oluşturulmalıdır (1903 Azure Stack derleme ve yukarıdaki ya da 1901/1902 düzeltme ile) ya da sürüm 2.2.20 ile oluşturulmalıdır. 2\.2.20 ve 2.2.20 ile 2.2.35 (dışlamalı) arasındaki sürümler Azure Stack düzgün şekilde çalışmayabilir.
>
> Şu anda Azure Stack [Cloud-init](https://cloud-init.io/) desteklenmez.

| Dağıtım | Açıklama | Yayımcı | Market |
| --- | --- | --- | --- |
| CentOS tabanlı 6,9 | 64 bit | Standart dışı dalga | Evet |
| CentOS tabanlı 7,5 | 64 bit | Standart dışı dalga | Evet |
| CentOS tabanlı 7.3 | 64 bit | Standart dışı dalga | Evet |
| ClearLinux | 64 bit | ClearLinux.org | Evet |
| Kapsayıcı Linux |  64 bit | CoreOS | Dengeli |
| Desek8 "Jese" | 64 bit | credativ |  Evet |
| 9 "uzat" | 64 bit | credativ | Evet |
| Oracle Linux | 64 bit | Oracle | Evet |
| Red Hat Enterprise Linux 7,1 (ve üzeri) | 64 bit | Red Hat | Kendi görüntünüzü getir |
| SLES 11SP4 | 64 bit | SUSE | Evet |
| SLES 12SP3 | 64 bit | SUSE | Evet |
| Ubuntu 14,04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 16,04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 18,04-LTS | 64 bit | Canonical | Evet |

Red Hat Enterprise Linux destek bilgileri için bkz [. Red Hat ve Azure Stack: Sık sorulan sorular](https://access.redhat.com/articles/3413531).

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack marketi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
- [Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
