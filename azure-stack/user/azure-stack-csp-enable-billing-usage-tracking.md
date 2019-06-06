---
title: Azure Stack abonelik yönetmek bulut hizmet sağlayıcınız izin | Microsoft Docs
description: İzin, bulut hizmet sağlayıcısı (Azure Stack aboneliğinizi yönetmek CSP) hakkında bilgi edinin.
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
ms.openlocfilehash: 0dc162dd1d4021323f1bf80ad4f89fc721e575a9
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691982"
---
# <a name="let-your-cloud-service-provider-manage-your-azure-stack-subscription"></a>Bulut hizmet sağlayıcınız Azure Stack abonelik yönetmek istiyorum

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bir bulut hizmeti sağlayıcısı (CSP) ile Azure Stack kullanırsanız, azure'da ve Azure Stack kaynaklara erişmek için kendi aboneliğinizi yönetmek seçin. Ayrıca, aboneliğinizi yönetmek sağlayıcısı sağlayabilirsiniz. Bu makalede gösterilmektedir için:

* Hizmet sağlayıcısı erişiminizi aboneliğinize verin.
* Hizmet sağlayıcısı hizmetinizi yönetebilmeniz için emin olun.

> [!NOTE]
> CSP hesabınızı yönetme değil ve aşağıdaki adımları atlayın, CSP sizin için Azure Stack aboneliğine yönetemez.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Bir bulut hizmeti sağlayıcısına aboneliğinizi yönetin

CSP olarak ekleme **kullanıcı** aboneliğinize.

1. CSP ile Konuk kullanıcı ekleme **kullanıcı** Kiracı dizininize rol. Bir kullanıcı ekleme konusunda daha fazla yardım için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory).

2. CSP, yerel Azure Stack aboneliğine sizin için oluşturur. Azure Stack kullanmaya başlamak hazırsınız demektir.
3. CSP, bunlar Ayrıca, kaynakları yönetebilir doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, yapabilirler [Azure Stack portal ile bir Windows sanal makinesi oluşturma](azure-stack-quick-windows-portal.md).

## <a name="let-the-cloud-service-provider-manage-your-subscription-using-rbac-rights"></a>Bulut hizmeti sağlayıcısı RBAC haklarını kullanarak aboneliğinizi yönetmek istiyorum

CSP olarak ekleme **sahibi** aboneliğinize.

1. CSP, Kiracı dizinine Konuk kullanıcı olarak ekleyin. Bir kullanıcı ekleme konusunda daha fazla yardım için bkz: [Azure Active Directory'ye yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory).

2. Ekleme **sahibi** CSP Konuk kullanıcıya rol. Aboneliğinize bir CSP kullanıcı ekleme konusunda daha fazla yardım için bkz: [Use Role-Based erişim denetimi, Azure abonelik kaynaklarınıza erişimi yönetmek için](/azure/role-based-access-control/role-assignments-portal). CSP, yerel Azure Stack aboneliğine sizin için oluşturur. Azure Stack kullanmaya başlamak hazırsınız demektir.
3. CSP kaynaklarınızı yönetebildiğini doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](../operator/azure-stack-billing-and-chargeback.md).
