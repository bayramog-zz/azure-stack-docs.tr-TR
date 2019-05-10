---
title: Şablonları portal kullanarak Azure Stack'te dağıtma | Microsoft Docs
description: Şablonları dağıtmak için Azure Stack portal'ı kullanmayı öğrenin.
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
ms.openlocfilehash: 6e848af9049c7f105e6a31fdef8dc03308bd276a
ms.sourcegitcommit: a78c0d143eadcab65a601746b9ea24be28091ad2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212368"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Şablonları Azure Stack portalını kullanarak dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Portal, Azure Stack için Azure Resource Manager şablonlarını dağıtmak için kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. Portal, select oturum **+ kaynak Oluştur**ve ardından **özel**.

   ![Oluştur](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **Şablon dağıtımı**'nı seçin.

   ![Şablon dağıtma](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. Seçin **şablonu Düzen**ve ardından JSON şablon kodunuz kod penceresine yapıştırın. **Kaydet**’i seçin.

   ![Şablonu düzenle](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. Seçin **parametreleri Düzenle**, gösterilir ve ardından bu parametrelerin değerlerini sağlamasını **Tamam**.

   ![Parametreleri düzenle](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. Seçin **abonelik**. Kullanın ve ardından istediğiniz aboneliği seçin **Tamam**.

   ![Abonelik](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. Seçin **kaynak grubu**. Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun ve ardından **Tamam**.

   ![Kaynak grubu](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **Oluştur**’u seçin. Yeni bir kutucuk Panoda şablon dağıtımınızın ilerlemesini izler.

   ![Şablon oluşturma](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>Sonraki adımlar

Şablon dağıtma hakkında daha fazla bilgi edinmek için şu makaleye bakın:

- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
