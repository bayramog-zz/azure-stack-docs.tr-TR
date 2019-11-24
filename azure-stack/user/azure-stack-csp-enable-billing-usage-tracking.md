---
title: Bulut çözümü sağlayıcınızda Azure Stack aboneliğinizi yönetmesine izin verin | Microsoft Docs
description: Bulut çözümü sağlayıcınızın (CSP) sizin için Azure Stack aboneliğinizi yönetmesine nasıl izin vereceğinizi öğrenin.
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
ms.date: 10/03/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/20/2019
ms.openlocfilehash: a11846feb852a44a5ae526e9c060ca1626bbe245
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961624"
---
# <a name="let-your-cloud-solution-provider-manage-your-azure-stack-subscription"></a>Bulut çözümü sağlayıcınızda Azure Stack aboneliğinizi yönetmesine izin verin

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bir bulut çözümü sağlayıcısı (CSP) ile Azure Stack kullanıyorsanız, Azure 'daki ve Azure Stack kaynaklara erişmek için kendi aboneliğinizi yönetmeyi tercih edebilirsiniz. Ayrıca, sağlayıcının aboneliğinizi sizin yerinize yönetmesine de izin verebilirsiniz. Bu makalede nasıl yapılacağı gösterilmektedir:

* Hizmet sağlayıcınıza aboneliğinize erişim izni verin.
* Hizmet sağlayıcısının hizmetinizi yönetebileceğini doğrulayın.

> [!NOTE]
> CSP hesabınızı yönetmez ve aşağıdaki adımları atlarsanız, CSP Azure Stack aboneliğinizi sizin için yönetemez.

## <a name="manage-your-subscription-with-a-csp"></a>Bir CSP ile aboneliğinizi yönetme

CSP 'yi aboneliğinize **Kullanıcı** olarak ekleyin.

1. CSP 'nizi kiracı dizininize **Kullanıcı** rolüne sahip konuk kullanıcı olarak ekleyin. Kullanıcı ekleme konusunda yardım için bkz. [Azure Active Directory yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory).

2. CSP, sizin için yerel Azure Stack aboneliği oluşturur. Azure Stack kullanmaya başlamaya hazırlanıyor.

3. CSP 'nizin da kaynaklarınızı yönetebildiğini doğrulamak için aboneliğinizde bir kaynak oluşturmanız gerekir. Örneğin, [Azure Stack portalı ile bir Windows sanal makinesi oluşturamazlar](azure-stack-quick-windows-portal.md).

## <a name="let-the-csp-manage-your-subscription-using-rbac-rights"></a>CSP 'nin RBAC haklarını kullanarak aboneliğinizi yönetmesine izin verin

CSP 'yi aboneliğinize **sahip** olarak ekleyin.

1. CSP 'nizi kiracı dizininize Konuk Kullanıcı olarak ekleyin. Kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory yeni kullanıcı ekleme](/azure/active-directory/add-users-azure-active-directory).

2. CSP Konuk kullanıcısına **sahip** rolünü ekleyin. Aboneliğinize bir CSP kullanıcısı ekleme hakkında daha fazla bilgi için, bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek Için rol tabanlı Access Control kullanma](/azure/role-based-access-control/role-assignments-portal). CSP, sizin için yerel Azure Stack aboneliği oluşturur. Azure Stack kullanmaya başlamaya hazırlanıyor.
3. Kendi kaynaklarınızı yönetebildiğini doğrulamak için CSP 'nizin aboneliğinizde bir kaynak oluşturması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak kullanım bilgilerini Azure Stack alma hakkında daha fazla bilgi için, bkz. [Azure Stack ' de kullanım ve faturalandırma](../operator/azure-stack-billing-and-chargeback.md).
