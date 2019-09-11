---
title: ASDK sürüm notları | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) için geliştirmeler, düzeltmeler ve bilinen sorunlar.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 9d0820634a469f775c1e3b6637c604ae98681be2
ms.sourcegitcommit: 7d7a4c8c46613b6104caf23763bfd2275f6a826b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70188185"
---
# <a name="asdk-release-notes"></a>ASDK sürüm notları

Bu makalede, Azure Stack Geliştirme Seti (ASDK) içindeki değişiklikler, düzeltmeler ve bilinen sorunlar hakkında bilgi verilmektedir. Hangi sürümü çalıştırdığınızdan emin değilseniz, [denetlemek için portalını kullanın](../operator/azure-stack-updates.md).

RSS RSS [akışına](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)abone [ ![](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) olarak, asdk 'deki yenilikleri güncel tutun.

::: moniker range="azs-1908"
## <a name="build-11908020"></a>Derleme 1.1908.0.20

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1908.md#whats-new) bakın.

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](../operator/azure-stack-release-notes-1908.md#fixes) of the Azure Stack release notes. -->
- Bilinen sorunların bir listesi için, [Bu makaleye](../operator/azure-stack-release-notes-known-issues-1908.md)bakın.
- Kullanılabilir Azure Stack düzeltmelerinin ASDK 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>Derleme 1.1907.0.20

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1907.md#whats-in-this-update) bakın.

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- Bazı Market görüntülerini kullanarak VM kaynakları oluştururken, dağıtımı tamamlayamayabilir. Geçici bir çözüm olarak, **Özet** sayfasındaki **şablon ve parametreleri Indir** bağlantısına tıklayıp **şablon** dikey penceresinde **Dağıt** düğmesine tıklayabilirsiniz.
- Bu sürümde düzeltilen Azure Stack sorunların listesi için, Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1907.md#fixes) bakın.
- Bilinen sorunların bir listesi için, [Bu makaleye](../operator/azure-stack-release-notes-known-issues-1907.md)bakın.
- [Kullanılabilir Azure Stack düzeltmelerinin](../operator/azure-stack-release-notes-1907.md#hotfixes) Azure Stack asdk 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1906"
## <a name="build-11906030"></a>Derleme 1.1906.0.30

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1906.md#whats-in-this-update) bakın.

### <a name="changes"></a>Değişiklikler

- Azure Stack için günlük toplama hizmetini barındıran bir **AZS-SRNG01** destek halkası sanal makinesi eklendi. Daha fazla bilgi için bkz. [sanal makine rolleri](asdk-architecture.md).

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- Bazı Market görüntülerini kullanarak VM kaynakları oluştururken, dağıtımı tamamlayamayabilir. Geçici bir çözüm olarak, **Özet** sayfasındaki **şablon ve parametreleri Indir** bağlantısına tıklayıp **şablon** dikey penceresinde **Dağıt** düğmesine tıklayabilirsiniz.
- Bu sürümde düzeltilen Azure Stack sorunların listesi için, Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1906.md#fixes) bakın.
- Bilinen sorunların bir listesi için, [Bu makaleye](../operator/azure-stack-release-notes-known-issues-1906.md)bakın.
- [Kullanılabilir Azure Stack düzeltmelerinin](../operator/azure-stack-release-notes-1906.md#hotfixes) Azure Stack asdk 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1905"
## <a name="build-11905040"></a>Derleme 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1905.md#whats-in-this-update) bakın.

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- [ASDK](asdk-register.md) 'yi başarıyla kaydetmek Için **registerwithazure. psm1** PowerShell betiğini düzenlemeniz gereken bir sorun düzeltildi.
- Bu sürümde düzeltilen diğer Azure Stack sorunların listesi için, Azure Stack sürüm notlarının [Bu bölümüne](../operator/azure-stack-release-notes-1905.md#fixes) bakın.
- Bilinen sorunların bir listesi için, [Bu makaleye](../operator/azure-stack-release-notes-known-issues-1905.md)bakın.
- [Kullanılabilir Azure Stack düzeltmelerinin](../operator/azure-stack-release-notes-1905.md#hotfixes) Azure Stack asdk 'ye uygulanmadığını unutmayın.
::: moniker-end
