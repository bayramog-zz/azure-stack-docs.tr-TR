---
title: Rol tabanlı erişim denetimi kullanarak erişim izinlerini ayarlama | Microsoft Docs
description: Azure Stack ' de rol tabanlı erişim denetimi (RBAC) kullanarak erişim izinleri ayarlamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: fa4e836a2c7cd5b59a6234a05efcc1cface12620
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277059"
---
# <a name="set-access-permissions-using-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanarak erişim izinlerini ayarlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack bir Kullanıcı, bir abonelik, kaynak grubu veya hizmetin her örneği için bir okuyucu, sahip veya katkıda bulunan olabilir. Örneğin, A kullanıcısının bir abonelik aboneliği için okuyucu izinleri olabilir, ancak sanal makine yedi için sahip izinlerine sahip olabilir.

 - Okuyucu: Kullanıcı her şeyi görüntüleyebilir, ancak herhangi bir değişiklik yapamaz.
 - Katkıda bulunan: Kullanıcı, kaynaklara erişim dışında her şeyi yönetebilir.
 - Sahip: Kullanıcı, kaynaklara erişim dahil her şeyi yönetebilir.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlama

1. Yönetmek istediğiniz kaynak için sahip izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde **erişim** simgesine ![](media/azure-stack-manage-permissions/image1.png) ' e tıklayın.
3. **Kullanıcılar** dikey penceresinde **Roller**' e tıklayın.
4. Kullanıcı izinleri eklemek için **Roller** dikey penceresinde **Ekle** ' ye tıklayın.

## <a name="set-access-permissions-for-a-universal-group"></a>Evrensel grup için erişim izinlerini ayarlama 

> [!Note]
> Yalnızca Active Directory Federasyon Hizmetleri (AD FS) için geçerlidir.

1. Yönetmek istediğiniz kaynak için sahip izinlerine sahip bir hesapla oturum açın.
2. Kaynak dikey penceresinde **erişim** simgesine ![](media/azure-stack-manage-permissions/image1.png) ' e tıklayın.
3. **Kullanıcılar** dikey penceresinde **Roller**' e tıklayın.
4. Evrensel grup Active Directory grubuna izinler eklemek için **Roller** dikey penceresinde **Ekle** ' ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack kiracı ekleme](azure-stack-add-new-user-aad.md)