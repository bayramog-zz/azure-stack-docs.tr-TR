---
title: Rol tabanlı erişim denetimiyle Azure Stack kaynaklara erişimi yönetme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) izinlerini Azure Stack bir yönetici veya kiracı olarak yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2019
ms.author: bryanla
ms.reviewer: fiseraci
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: a784da0d16f6ec92a105d9360430f4e8da2817ef
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974922"
---
# <a name="manage-access-to-resources-in-azure-stack-with-role-based-access-control"></a>Rol tabanlı erişim denetimiyle Azure Stack kaynaklara erişimi yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, rol tabanlı erişim denetimi (RBAC), aynı destekler [erişim yönetimi için güvenlik modeli](/azure/role-based-access-control/overview) , Microsoft Azure'ı kullanır. Abonelik, kaynak ve hizmetlere Kullanıcı, Grup veya uygulama erişimini yönetmek için RBAC kullanabilirsiniz.

## <a name="basics-of-access-management"></a>Erişim yönetimi ile ilgili temel bilgiler

Rol tabanlı erişim denetimi (RBAC), ortamınızı güvenli hale getirmek için kullanabileceğiniz ayrıntılı erişim denetimi sağlar. Belirli bir kapsamda RBAC rolü atayarak kullanıcılara ihtiyaç duydukları tam izinleri verirsiniz. Rol atama kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir. Erişim yönetimi hakkında daha ayrıntılı bilgi için, Azure portal makalesindeki [rol tabanlı Access Control](/azure/role-based-access-control/overview) bakın.

> [!NOTE]
> Azure Stack, kimlik sağlayıcısı olarak Active Directory Federasyon Hizmetleri (AD FS) kullanılarak dağıtıldığında, RBAC senaryolarında yalnızca evrensel gruplar desteklenir.

### <a name="built-in-roles"></a>Yerleşik roller

Azure Stack, tüm kaynak türleri için uygulayabileceğiniz üç temel rolüne sahiptir:

* **Sahip**: kaynaklara erişim dahil her şeyi yönetebilir.
* **Katkıda bulunan**: kaynaklara erişim dışında her şeyi yönetebilir.
* **Okuyucu**: her şeyi görüntüleyebilir, ancak değişiklik yapamaz.

### <a name="resource-hierarchy-and-inheritance"></a>Kaynak hiyerarşisi ve devralma

Azure Stack aşağıdaki kaynak hiyerarşi vardır:

* Her abonelik bir dizine ait.
* Her kaynak grubu, tek bir aboneliğe ait.
* Her kaynak bir kaynak grubuna aittir.

Bir üst kapsamda verdiğiniz erişim alt kapsamların devralınır. Örneğin:

* Atadığınız **okuyucu** abonelik kapsamında bir Azure AD grubu rol. Bu grubun üyeleri, her kaynak grubu ve kaynak abonelikte görüntüleyebilirsiniz.
* **Katkıda** bulunan rolünü, kaynak grubu kapsamındaki bir uygulamaya atarsınız. Uygulama, söz konusu kaynak grubundaki tüm türlerin kaynaklarını yönetebilir, ancak abonelikte diğer kaynak gruplarını yönetemez.

### <a name="assigning-roles"></a>Rol atama

Bir kullanıcı için birden çok rol atayabilirsiniz ve her rol farklı bir kapsam ile ilişkili olabilir. Örneğin:

* -1 aboneliğine TestUser-A **okuyucu** rolü atarsınız.
* TestVM-1 ' e TestUser-A **sahip** rolü atarsınız.

Azure [rol atamaları](/azure/role-based-access-control/role-assignments-portal) makale görüntüleme, atama ve rollerin silinmesine hakkında ayrıntılı bilgi sağlar.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlayın

Aşağıdaki adımları izinlerin bir kullanıcı için nasıl yapılandırılacağını açıklar.

1. Yönetmek istediğiniz kaynağa sahip izinlerine sahip bir hesapla oturum açın.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. İzinlerini ayarlamak istediğiniz kaynak grubu adını seçin.
4. Kaynak grubu Gezinti bölmesinde **erişim denetimi (IAM)** .<BR> **Rol atamaları** görünümü kaynak grubuna erişimi olan öğeleri listeler. Filtre uygulayabilir ve Grup sonuçları.
5. **Erişim denetimi** menü çubuğunda **Ekle**' yi seçin.
6. **Izin Ekle** bölmesinde:

   * Atamak istediğiniz rolü seçin **rol** aşağı açılan listesi.
   * Atamak istediğiniz kaynağı seçin **erişim Ata** aşağı açılan listesi.
   * Dizininizde erişim vermek istediğiniz kullanıcı, Grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

7. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet sorumlusu oluşturma](../operator/azure-stack-create-service-principals.md)
