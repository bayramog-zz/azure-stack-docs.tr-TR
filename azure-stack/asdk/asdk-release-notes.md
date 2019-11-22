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
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: f5955751df89262441b7afc2381d34b6cc74691c
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298742"
---
# <a name="asdk-release-notes"></a>ASDK sürüm notları

Bu makalede, Azure Stack Geliştirme Seti (ASDK) içindeki değişiklikler, düzeltmeler ve bilinen sorunlar hakkında bilgi verilmektedir. Hangi sürümü çalıştırdığınızdan emin değilseniz, [denetlemek için portalını kullanın](../operator/azure-stack-updates.md).

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS akışına](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)abone olarak, asdk 'deki yenilikleri güncel tutun.

::: moniker range="azs-1910"
## <a name="build-11910058"></a>Derleme 1.1910.0.58

### <a name="new-features"></a>Yeni özellikler

- Bu sürümdeki düzeltilen sorunlar, değişiklikler ve yeni özelliklerin listesi için [Azure Stack sürüm notlarındaki](../operator/release-notes.md)ilgili bölümlere bakın.

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- Günlükleri toplama ve bunları bir Azure Storage blob kapsayıcısında depolama ile ilgili bir sorun düzeltildi. Bu işlem için sözdizimi aşağıdaki gibidir:

  ```powershell
  Get-AzureStackLog -OutputSasUri "<Blob service SAS Uri>"
  ``` 

- Yavaş bir yükleme biriktirici hizmetinin bazı Windows özelliklerinin kaldırılmasını önlediği ve yeniden başlatma gerektirdiği bir dağıtım sorunu düzeltildi.
- Bu sürümdeki bilinen sorunların Azure Stack listesi için [bilinen sorunlar](../operator/known-issues.md) makalesine bakın.
- Kullanılabilir Azure Stack düzeltmelerinin ASDK 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1908"
  
## <a name="build-11908020"></a>Derleme 1.1908.0.20

### <a name="new-features"></a>Yeni özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](/azure-stack/operator/release-notes?view=azs-1908#whats-new-1908) bakın.

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](/azure-stack/operator/release-notes?view=azs-1908#fixes-1908) of the Azure Stack release notes. -->
- Bilinen sorunların bir listesi için, [Bu makaleye](/azure-stack/operator/known-issues?view=azs-1908)bakın.
- Kullanılabilir Azure Stack düzeltmelerinin ASDK 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>Derleme 1.1907.0.20

### <a name="new-features"></a>Yeni özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](/azure-stack/operator/release-notes?view=azs-1907#whats-in-this-update-1907) bakın.

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- Bazı Market görüntülerini kullanarak VM kaynakları oluştururken, dağıtımı tamamlayamayabilir. Geçici bir çözüm olarak, **Özet** sayfasındaki **şablon ve parametreleri Indir** bağlantısına tıklayıp **şablon** dikey penceresinde **Dağıt** düğmesine tıklayabilirsiniz.
- Bu sürümde düzeltilen Azure Stack sorunların listesi için, Azure Stack sürüm notlarının [Bu bölümüne](/azure-stack/operator/release-notes?view=azs-1907#fixes-1907) bakın.
- Bilinen sorunların bir listesi için, [Bu makaleye](/azure-stack/operator/known-issues?view=azs-1907)bakın.
- [Kullanılabilir Azure Stack düzeltmelerinin](/azure-stack/operator/release-notes?view=azs-1907#hotfixes-1907) Azure Stack asdk 'ye uygulanmadığını unutmayın.
::: moniker-end

::: moniker range="azs-1906"
## <a name="build-11906030"></a>Derleme 1.1906.0.30

### <a name="new-features"></a>Yeni özellikler

- Bu sürümdeki yeni özelliklerin listesi için Azure Stack sürüm notlarının [Bu bölümüne](/azure-stack/operator/release-notes?view=azs-1906#whats-in-this-update-1906) bakın.

### <a name="changes"></a>Değişiklikler

- Azure Stack için günlük toplama hizmetini barındıran bir **AZS-SRNG01** destek halkası sanal makinesi eklendi. Daha fazla bilgi için bkz. [sanal makine rolleri](asdk-architecture.md).

### <a name="fixed-and-known-issues"></a>Düzeltilen ve bilinen sorunlar

- Bazı Market görüntülerini kullanarak VM kaynakları oluştururken, dağıtımı tamamlayamayabilir. Geçici bir çözüm olarak, **Özet** sayfasındaki **şablon ve parametreleri Indir** bağlantısına tıklayıp **şablon** dikey penceresinde **Dağıt** düğmesine tıklayabilirsiniz.
- Bu sürümde düzeltilen Azure Stack sorunların listesi için, Azure Stack sürüm notlarının [Bu bölümüne](/azure-stack/operator/release-notes?view=azs-1906#fixes-1906) bakın.
- Bilinen sorunların bir listesi için, [Bu makaleye](/azure-stack/operator/known-issues?view=azs-1906)bakın.
- [Kullanılabilir Azure Stack düzeltmelerinin](/azure-stack/operator/release-notes?view=azs-1906#hotfixes-1906) Azure Stack asdk 'ye uygulanmadığını unutmayın.
::: moniker-end
