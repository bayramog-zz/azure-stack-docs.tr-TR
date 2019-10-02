---
title: Azure Stack Portal 'ı kullanarak şablon dağıtma | Microsoft Docs
description: Bir şablonu dağıtmak için Azure Stack portalını nasıl kullanacağınızı öğrenin.
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
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: bbbbbc5548397f82752a43c7a1aaca7b62151b75
ms.sourcegitcommit: e8aa26b078a9bab09c8fafd888a96785cc7abb4d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71708990"
---
# <a name="deploy-a-template-using-the-portal-in-azure-stack"></a>Azure Stack portalı kullanarak şablon dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager şablonları dağıtmak üzere portalını kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Şablon dağıtmak için

1. Portalda oturum açın, **+ kaynak oluştur**' u seçin ve ardından **özel**' i seçin.

   ![Azure Stack portalında kaynak oluşturma](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **Şablon dağıtımı**'nı seçin.

   ![Azure Stack portalında şablon dağıtma](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. **Şablonu Düzenle**' yi seçin ve ardından JSON şablonu kodunuzu kod penceresine yapıştırın. **Kaydet**’i seçin.

   ![Azure Stack portalında şablonu düzenleme](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. **Parametreleri Düzenle**' yi seçin, gösterilen parametreler için değerler sağlayın ve ardından **Tamam**' ı seçin.

   ![Azure Stack portalındaki parametreleri düzenleme](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. **Abonelik**' ı seçin. Kullanmak istediğiniz aboneliği seçin ve ardından **Tamam**' ı seçin.

   ![Azure Stack portalında abonelik seçin](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. **Kaynak grubu**' nu seçin. Var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun ve ardından **Tamam**' ı seçin.

   ![Azure Stack portalında kaynak grubu seç](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **Oluştur**’u seçin. Panoda yeni bir kutucuk, şablon dağıtımınızın ilerlemesini izler.

   ![Azure Stack portalında şablon oluşturma](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>Sonraki adımlar

Şablon dağıtma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
