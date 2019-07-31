---
title: Azure Stack yönelik uygulamalar geliştirin | Microsoft Docs
description: Azure hizmetlerini kullanarak Azure Stack prototipsiz uygulamalar için geliştirme konuları.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: 059e7961cae5c6d6faa8d79d05dbc08a05a13893
ms.sourcegitcommit: b3dac698f2e1834491c2f9af56a80e95654f11f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658559"
---
# <a name="develop-for-azure-stack"></a>Azure Stack için geliştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bir Azure Stack ortamına erişiminiz olmasa bile, bugün uygulama geliştirmeye başlamanızı sağlayabilirsiniz. Azure Stack, veri merkezinizde çalışan Microsoft Azure hizmetleri sunar. Bu, Azure Stack geliştirmek için aynı Azure araçlarını ve süreçlerini kullanabileceğiniz anlamına gelir.

## <a name="development-considerations"></a>Geliştirme konuları

Bazı hazırlıklar ve aşağıdaki konularda bulunan Kılavuzu kullanarak, bir Azure Stack ortamına öykünmek için Azure 'u kullanabilirsiniz.

* Azure 'da, Azure Stack dağıtılabilir Azure Resource Manager şablonlar oluşturabilirsiniz. Taşınabilirlik sağlamak için şablon geliştirmeye yönelik yönergeler için bkz. [şablon konuları](azure-stack-develop-templates.md) .
* Azure ile Azure Stack arasında hizmet kullanılabilirliği ve hizmet sürümü oluşturma farklılıkları vardır. Azure hizmet kullanılabilirliğini ve kaynak türlerini Azure Stack kullanılabilir olanlarla kısıtlamak için [Azure Stack İlkesi modülünü](azure-stack-policy-module.md) kullanabilirsiniz. Kısıtlama Hizmetleri, uygulamalarınızın Azure Stack için kullanılabilen hizmetlere dayanmasını sağlar.
* [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) , hem Azure hem de Azure Stack için dağıtılabilir şablonlar geliştirmeyi gösteren yaygın senaryo örnekleridir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Resource Manager şablonu en iyi yöntemleri](azure-stack-develop-templates.md)
* [GitHub 'daki Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates)