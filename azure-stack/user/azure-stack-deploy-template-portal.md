---
title: Portalı kullanarak Azure Stack'te şablon dağıtma | Microsoft Docs
description: Bir şablonu dağıtmak için Azure Stack portal'ı kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 8f61667351a6d22094d5f8a0ba39348cc92549a8
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692022"
---
# <a name="deploy-a-template-using-the-portal-in-azure-stack"></a>Azure Stack'te portalı kullanarak bir şablonu dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Portal, Azure Stack için Azure Resource Manager şablonlarını dağıtmak için kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. Portal, select oturum **+ kaynak Oluştur**ve ardından **özel**.

   ![Azure Stack portalında kaynak oluştur](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **Şablon dağıtımı**'nı seçin.

   ![Azure Stack portal şablonu dağıtma](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. Seçin **şablonu Düzen**ve ardından JSON şablon kodunuz kod penceresine yapıştırın. **Kaydet**’i seçin.

   ![Azure Stack Portalı'nda Şablonu Düzenle](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. Seçin **parametreleri Düzenle**, gösterilir ve ardından bu parametrelerin değerlerini sağlamasını **Tamam**.

   ![Azure Stack portalında parametreleri Düzenle](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. Seçin **abonelik**. Kullanın ve ardından istediğiniz aboneliği seçin **Tamam**.

   ![Azure Stack portalında abonelik seçin](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. Seçin **kaynak grubu**. Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun ve ardından **Tamam**.

   ![Azure Stack portalında kaynak grubu seçin](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **Oluştur**’u seçin. Yeni bir kutucuk Panoda şablon dağıtımınızın ilerlemesini izler.

   ![Azure Stack portalında şablon oluşturma](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>Sonraki adımlar

Şablon dağıtma hakkında daha fazla bilgi edinmek için şu makaleye bakın:

- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
