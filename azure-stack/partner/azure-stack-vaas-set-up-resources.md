---
title: Öğretici-hizmet olarak doğrulama için kaynakları ayarlama | Microsoft Docs
description: Bu öğreticide, bir hizmet olarak doğrulama için kaynakları ayarlamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 33ead43fd8dbe77feae12db58ca7c689030c6f98
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829099"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>Öğretici: Hizmet olarak doğrulama için kaynakları ayarlama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Hizmet olarak doğrulama (VaaS), pazardaki Azure Stack çözümlerini doğrulamak ve desteklemek için kullanılan bir Azure hizmetidir. Çözümünüzü doğrulamak için hizmeti kullanmadan önce bu makaleyi izleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Active Directory (AD) ayarlayarak VaaS kullanmaya hazırlanın.
> * Bir depolama hesabı oluşturun.

## <a name="configure-an-azure-ad-tenant"></a>Azure AD kiracısı yapılandırma

Bir Azure AD kiracısı, bir kuruluşu kaydetmek ve VaaS ile kullanıcıların kimliğini doğrulamak için kullanılır. İş ortağı, kiracı 'nın rol tabanlı erişim denetimi (RBAC) özelliklerini kullanarak iş ortağı kuruluşunun VaaS 'yi kullanabileceği kişileri yönetebilir. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis).

### <a name="create-a-tenant"></a>Kiracı oluşturma

Kuruluşunuzun VaaS hizmetlerine erişmek için kullanacağı bir kiracı oluşturun. Açıklayıcı bir ad kullanın (örneğin, `ContosoVaaS@onmicrosoft.com`).

1. [Azure Portal](https://portal.azure.com)BIR Azure AD kiracısı oluşturun veya mevcut bir kiracıyı kullanın. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. Kuruluşunuzun üyelerini kiracıya ekleyin. Bu kullanıcılar, testleri görüntülemek veya zamanlamak için Hizmeti kullanmaktan sorumlu olacaktır. Kayıt işini tamamladıktan sonra, kullanıcıların erişim düzeylerini tanımlayacaksınız.

    Aşağıdaki rollerden birini atayarak kiracınızdaki kullanıcıları VaaS 'de Çalıştır eylemini yetkilendirin.

    | Rol adı | Açıklama |
    |---------------------|------------------------------------------|
    | Sahip | Tüm kaynaklara tam erişim sağlar. |
    | Okuyucu | Tüm kaynakları görüntüleyebilir, ancak oluşturmaz veya yönetemez. |
    | Test Katılımcısı | , Test kaynakları oluşturabilir ve yönetebilir. |

    **Azure Stack doğrulama hizmeti** uygulamasına roller atamak için:

   1. [Azure Portal](https://portal.azure.com) oturum açın.
   2. **Kimlik** bölümünün altındaki  > **Azure Active Directory** **tüm hizmetleri**seçin.
   3. @No__t-1**Azure Stack doğrulama hizmeti** uygulaması **Kurumsal uygulamalar**' ı seçin.
   4. **Kullanıcı ve gruplar**'ı seçin. **Azure Stack doğrulama hizmeti-kullanıcılar ve grup** dikey penceresinde, uygulamayı kullanma izni olan kullanıcılar listelenir.
   5. Kiracınızdan bir kullanıcı eklemek ve bir rol atamak için **+ Kullanıcı Ekle** ' yi seçin.

      VaaS kaynaklarını ve eylemlerini bir kuruluştaki farklı gruplar arasında yalıtmak isterseniz, birden çok Azure AD kiracı dizini oluşturabilirsiniz.

### <a name="register-your-tenant"></a>Kiracınızı kaydedin

Bu işlem, kiracınızı **Azure Stack doğrulama hizmeti** Azure AD uygulaması ile yetkilendirir.

1. [@No__t-1](mailto:vaashelp@microsoft.com)' de kiracı hakkında aşağıdaki bilgileri Microsoft 'a gönderin.

    | Data | Açıklama |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Kuruluş adı | Resmi kuruluş adı. |
    | Azure AD kiracı dizin adı | Kaydolmakta olan Azure AD kiracı dizin adı. |
    | Azure AD kiracı dizin KIMLIĞI | Dizinle ilişkili Azure AD kiracı Dizin GUID 'ı. Azure AD kiracı Dizin KIMLIĞINIZI bulma hakkında daha fazla bilgi için bkz. [KIRACı kimliğini alma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-values-for-signing-in). |

2. Kiracınızın VaaS portalını kullanıp kullandığına emin olmak için Azure Stack doğrulama ekibinden onay için bekleyin.

### <a name="consent-to-the-vaas-application"></a>VaaS uygulamasına onay

Azure AD yöneticisi olarak, VaaS Azure AD uygulamasına kiracınız adına gerekli izinleri verin:

1. Kiracı için genel yönetici kimlik bilgilerini kullanarak [Vaas portalında](https://azurestackvalidation.com/)oturum açın. 

2. **Hesabım**' ı seçin.

3 listelenen Azure AD izinleri vermek için istendiğinde devam etmek için koşulları kabul edin.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Test yürütmesi sırasında, VaaS, tanılama günlüklerini bir Azure depolama hesabına verir. Test günlüklerine ek olarak, depolama hesabı da paket doğrulama iş akışı için OEM uzantı paketlerini karşıya yüklemek için de kullanılabilir.

Azure depolama hesabı, Azure Stack ortamınızda değil, Azure genel bulutunda barındırılır.

1. Azure portal, **tüm hizmetler** > **depolama** > **depolama hesabı**' nı seçin. **Depolama hesapları** dikey penceresinde **Ekle**' yi seçin.

2. Depolama hesabının oluşturulacağı aboneliği seçin.

3. **Kaynak grubu**altında **Yeni oluştur**' u seçin. Yeni kaynak grubunuz için bir ad girin.

4. Azure depolama hesapları için [adlandırma kurallarını](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#storage) gözden geçirin. Depolama hesabınız için bir ad girin.

5. Depolama hesabınızın **ABD Batı** bölgesini seçin.

    Günlüklerin depolanması için ağ ücretlerinden sorumlu olmadığından emin olmak için, Azure depolama hesabı yalnızca **ABD Batı** bölgesini kullanacak şekilde yapılandırılabilir. Veri çoğaltma ve sık erişimli depolama katmanı özelliği bu veriler için gerekli değildir. İki özelliğin de etkinleştirilmesi, maliyetlerinizi ciddi ölçüde artırır.

6. **Hesap türü**hariç varsayılan değerlere ayarları bırakın:

    - **Dağıtım modeli** alanı varsayılan olarak **Kaynak Yöneticisi** olarak ayarlanır.
    - **Performans** alanı varsayılan olarak **Standart**’a ayarlanır.
    - **Hesap türü** alanını **BLOB depolama**alanı olarak seçin.
    - **Çoğaltma alanı** varsayılan olarak **yerel olarak yedekli depolama (LRS)** olarak ayarlanır.
    - **Erişim katmanı** varsayılan olarak **Sık erişim**’e ayarlanır.

7. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınız bağlı bağlantılara izin vermediğinden, donanımınızla ilgili bir test çalıştırmak için yerel aracıyı dağıtma öğreticisini izleyin.

> [!div class="nextstepaction"]
> [Yerel aracıyı dağıtma](azure-stack-vaas-local-agent.md)
