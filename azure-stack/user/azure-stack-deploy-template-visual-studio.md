---
title: Azure Stack 'de Visual Studio ile şablonlar dağıtma | Microsoft Docs
description: Azure Stack 'de Visual Studio ile şablon dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: c84965295abd1218458f46e998dcec8b1b2fdc88
ms.sourcegitcommit: b3dac698f2e1834491c2f9af56a80e95654f11f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658591"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Visual Studio 'Yu kullanarak Azure Stack şablonları dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Visual Studio, Azure Stack Azure Resource Manager şablonları dağıtmak için kullanılabilir.

## <a name="to-deploy-a-template"></a>Şablon dağıtmak için

1. Visual Studio ile Azure Stack [yükleyip bağlayın](azure-stack-install-visual-studio.md) .
2. Visual Studio'yu açın.
3. **Dosya**' yı seçin ve ardından **Yeni**' yi seçin. **Yeni proje**' de **Azure Kaynak grubu**' nu seçin.
4. Yeni proje için bir **ad** girin ve ardından **Tamam**' ı seçin.
5. **Azure şablonu seç**' te, açılan listeden **Azure Stack hızlı başlangıç** ' i seçin.
6. **101-Create-Storage-Account**' ı seçin ve ardından **Tamam**' ı seçin.
7. Yeni projenizde, kullanılabilir şablonları görmek için **Çözüm Gezgini** **Şablonlar** düğümünü genişletin.
8. **Çözüm Gezgini**, projenizin adını seçin ve ardından **Dağıt**' ı seçin. **Yeni dağıtım**' ı seçin.
9. **Kaynak grubuna dağıt**' da, **abonelik** açılan listesini kullanarak Microsoft Azure Stack aboneliğinizi seçin.
10. **Kaynak grubu** listesinden, var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun.
11. **Kaynak grubu konum** listesinden bir konum seçin ve ardından **Dağıt**' ı seçin.
12. **Parametreleri Düzenle**' de parametreler için değerler sağlayın (şablona göre farklılık gösterir) ve ardından **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları komut satırı ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
