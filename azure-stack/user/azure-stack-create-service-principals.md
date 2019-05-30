---
title: Azure Stack için hizmet sorumlusu oluşturma | Microsoft Docs
description: Kaynaklara erişimi yönetmek için rol tabanlı erişim denetimi, Azure Resource Manager ile kullanmak için bir hizmet sorumlusu oluşturmayı öğrenin.
services: azure-resource-manager
documentationcenter: na
author: PatAltimore
manager: femila
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/15/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 12/12/2018
ms.openlocfilehash: 8b3d0ad1f0854f7028e9dfff2d9114df141394a6
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269619"
---
# <a name="create-service-principals-to-give-applications-access-to-azure-stack-resources"></a>Azure Stack kaynaklarına uygulamalar erişim vermek için hizmet sorumlusu oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack kaynaklara bir hizmet sorumlusu Azure Resource Manager kullanan oluşturarak, bir uygulama erişimi verebilirsiniz. Bir hizmet sorumlusu kullanarak temsilci belirli izinleri sağlayan [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md).

En iyi uygulama, uygulamalarınız için hizmet sorumluları kullanmanız gerekir. Hizmet sorumluları, aşağıdaki nedenlerden dolayı kendi kimlik bilgilerini kullanarak bir uygulama çalıştırmaya tercih edilir:

* Hizmet sorumlusu kendi hesap izinleri farklı olan izinler atayabilirsiniz. Genellikle, tam olarak hangi uygulamanın yapması için bir hizmet sorumlusunun izinlerini kısıtlanır.
* Rol veya sorumlulukları değiştirirseniz uygulamanın kimlik bilgilerini değiştirmeniz gerekmez.
* Kimlik doğrulaması katılımsız bir komut dosyası çalıştırılırken otomatik hale getirmek için bir sertifika kullanabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo

Azure Resource Manager kullanarak Azure kaynaklarını envantere almak üzere gereken bir yapılandırma yönetim uygulamanız var. Bir hizmet sorumlusu oluşturma ve okuyucu rolüne atayın. Bu rol, Azure kaynaklarına uygulama salt okunur erişim sağlar.

## <a name="getting-started"></a>Başlarken

Bir kılavuz olarak, bu makaledeki adımları kullanın:

* Uygulamanız için hizmet sorumlusu oluşturun.
* Uygulamanızı kaydetmenizi ve kimlik doğrulama anahtarı oluşturun.
* Uygulamanızı bir role atayın.

Azure Stack için Active Directory yapılandırdığınız şekilde, bir hizmet sorumlusu oluşturma belirler. Aşağıdaki seçeneklerden birini seçin:

* Bir hizmet sorumlusu için oluşturma [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Bir hizmet sorumlusu için oluşturma [Active Directory Federasyon Hizmetleri (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

Bir hizmet sorumlusu, rol atama adımlarını Azure için aynı olan AD ve AD FS. Hizmet sorumlusu oluşturduktan sonra bir rol atayarak izinleri verebilirsiniz.

## <a name="create-service-principal-for-azure-ad"></a>Azure AD hizmet sorumlusu oluşturma

Azure Stack, Azure AD kimlik deposu olarak kullanıyorsa, bir hizmet sorumlusu Azure, Azure portalını kullanarak olduğu gibi aynı adımları kullanarak oluşturabilirsiniz.

> [!NOTE]
> Sahip olduğunuzu denetleyin [Azure AD izinleri gerekli](/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions) bir hizmet sorumlusu oluşturma işlemi başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluştur

Uygulamanız için hizmet sorumlusu oluşturmak için:

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.
3. Bir ad sağlayın.
4. Seçin **desteklenen hesap türleri**.
5.  Uygulama için bir URI ekleyin. Seçin **Web** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **kaydetme**.

Uygulamanız için bir hizmet sorumlusu oluşturdunuz.

### <a name="get-credentials"></a>Kimlik bilgilerini al

Programlamayla oturum açılırken, uygulamanızın ve bir kimlik doğrulama anahtarı kimliği kullanın. Bu değerleri almak için:

1. Gelen **uygulama kayıtları** Active Directory'de, uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Örnek uygulamalar uygulamalarda kullanımı **istemci kimliği** söz konusu olduğunda **uygulama kimliği**.

     ![Uygulama için uygulama kimliği](./media/azure-stack-create-service-principals/image12.png)
3. Kimlik doğrulama anahtarını oluşturmak üzere **sertifikaları ve parolaları**.

4. **Yeni istemci gizli dizisi**’ni seçin.

5. Anahtarın bir açıklama sağlayın, anahtar için bir süre seçin ve seçin **Ekle**. 

6. İşiniz bittiğinde, gizli dizisinin değeri görüntülenir. Daha sonra anahtarı alınamıyor çünkü bu değeri yazın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

![Anahtar değeri uyarısı için kaydedilen anahtarı.](./media/azure-stack-create-service-principals/image15.png)

Son adım [uygulamanızı rol atama](azure-stack-create-service-principals.md).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma

AD FS kimlik deposu olarak kullanarak Azure Stack dağıttıysanız, aşağıdaki görevler için PowerShell kullanabilirsiniz:

* Bir hizmet sorumlusu oluşturun.
* Hizmet sorumlusu, rol atayın.
* Hizmet sorumlusunun kimliğini kullanarak oturum açın.

Hizmet sorumlusu oluşturma hakkında daha fazla ayrıntı için bkz. [AD FS için hizmet sorumlusu oluşturma](../operator/azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

## <a name="assign-the-service-principal-to-a-role"></a>Hizmet sorumlusu, rol atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: Yerleşik roller](/azure/role-based-access-control/built-in-roles).

> [!NOTE]
> Bir abonelik, kaynak grubu veya bir kaynak düzeyinde bir rolün kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne sahip bir uygulama, uygulama kaynak grubundaki kaynakların okuyabilirsiniz anlamına gelir.

Aşağıdaki adımlar, bir hizmet sorumlusuna bir rol atamak için bir kılavuz olarak kullanın.

1. Azure Stack Portalı'nda, uygulamayı atamak istediğiniz kapsam düzeyini gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

2. Uygulamaya atamak için bir abonelik seçin. Bu örnekte, Visual Studio Enterprise bir aboneliktir.

     ![Atama için Visual Studio Enterprise aboneliği seçin](./media/azure-stack-create-service-principals/image16.png)

3. Seçin **erişim denetimi (IAM)** abonelik için.

4. Seçin **rol ataması Ekle**.

5. Uygulamayı atamak istediğiniz rolü seçin.

6. Uygulamanız için arama yapın ve seçin.

7. Seçin **Tamam** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görebilirsiniz.

Bir hizmet sorumlusu oluşturuldu ve rol atanmış göre uygulamanızı Azure Stack kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
