---
title: Azure Stack aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirme | Microsoft Docs
description: Azure Stack'te bir abonelik erişmek hizmet sağlayıcısı'nı etkinleştirin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/20/2019
ms.openlocfilehash: 209a0da31d4bd5b519771f68f4bddcbb833284fa
ms.sourcegitcommit: d2012e765c3fa5bccb4756d190349e890f9f48bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65941220"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Azure Stack aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştir

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bir bulut hizmeti sağlayıcısı (CSP) ile Azure Stack kullanırsanız, azure'da ve Azure Stack kaynaklara erişmek için kendi aboneliğinizi yönetmek seçin. Ayrıca, aboneliğinizi yönetmek sağlayıcısı sağlayabilirsiniz. Bu makalede gösterilmektedir için:

* Hizmet sağlayıcısı erişiminizi aboneliğinize verin.
* Hizmet sağlayıcısı hizmetinizi yönetebilmeniz için emin olun.

> [!NOTE]
> CSP hesabınızı yönetme değil ve aşağıdaki adımları atlayın, CSP sizin için Azure Stack aboneliğine yönetemez.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Bir bulut hizmeti sağlayıcısına aboneliğinizi yönetin

CSP olarak ekleme **kullanıcı** aboneliğinize.

1. CSP ile Konuk kullanıcı ekleme **kullanıcı** Kiracı dizininize rol. Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory)
2. CSP, yerel Azure Stack aboneliğine sizin için oluşturur. Azure Stack kullanmaya başlamak hazır olursunuz.
3. CSP, bunlar Ayrıca, kaynakları yönetebilir doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, yapabilirler [Azure Stack portal ile bir Windows sanal makinesi oluşturma](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>RBAC haklarını kullanarak aboneliğinizi yönetmek bulut hizmeti sağlayıcısı etkinleştir

CSP olarak ekleme **sahibi** aboneliğinize.

1. CSP, Kiracı dizinine Konuk kullanıcı olarak ekleyin. Kullanıcı ekleme adımları için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory).
2. Ekleme **sahibi** CSP Konuk kullanıcıya rol. CSP kullanıcıyı aboneliğinize eklemek adımları için bkz. [Use Role-Based erişim denetimi, Azure abonelik kaynaklarınıza erişimi yönetmek için](/azure/role-based-access-control/role-assignments-portal). CSP, yerel Azure Stack aboneliğine sizin için oluşturur. Azure Stack kullanmaya başlamak hazır olursunuz.
3. CSP kaynaklarınızı yönetebildiğini doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](../operator/azure-stack-billing-and-chargeback.md).
