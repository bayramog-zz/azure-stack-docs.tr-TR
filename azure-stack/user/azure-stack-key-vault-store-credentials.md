---
title: Hizmet sorumlusu kimlik bilgilerini Azure Stack Key Vault depolama | Microsoft Docs
description: Key Vault hizmet sorumlusu kimlik bilgilerini Azure Stack nasıl depolayacağınızı öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 9d86f7e68b2e96eb4a22f9896ff65a4ed6b96f92
ms.sourcegitcommit: bbf3edbfc07603d2c23de44240933c07976ea550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714733"
---
# <a name="store-service-principal-credentials-in-azure-stack-key-vault"></a>Hizmet sorumlusu kimlik bilgilerini Azure Stack Key Vault depolama

Azure Stack uygulamaların geliştirilmesi, genellikle bir hizmet sorumlusu oluşturulmasını ve dağıtımdan önce kimlik doğrulaması yapmak için bu kimlik bilgilerini kullanmayı gerektirir. Ancak, bazen hizmet sorumlusu için depolanan kimlik bilgilerini kaybedebilirsiniz. Bu makalede, bir hizmet sorumlusu oluşturma ve daha sonra alımı için Azure Key Vault değerleri depolama işlemlerinin nasıl yapılacağı açıklanır.

Key Vault hakkında daha fazla bilgi için bkz. [Azure Stack Key Vault giriş](azure-stack-key-vault-intro.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure Key Vault hizmetini içeren bir teklifin aboneliği.
- PowerShell, Azure Stack ile kullanım için yüklendi ve yapılandırıldı.

## <a name="key-vault-in-azure-stack"></a>Azure Stack Key Vault

Azure Stack Key Vault, bulut uygulamalarının ve hizmetlerinin kullandığı şifreleme anahtarlarını ve gizli dizileri korumanıza yardımcı olur. Key Vault kullanarak anahtarları ve gizli dizileri şifreleyebilirsiniz.

Bir Anahtar Kasası oluşturmak için aşağıdaki adımları izleyin:

1. Azure Stack portalında oturum açın.

2. Panodan **+ kaynak oluştur**' u seçin ve **güvenlik ve kimlik**sonra Key Vault ' yi seçin **.**

   ![Anahtar kasası oluştur](media/azure-stack-key-vault-store-credentials/create-key-vault.png)

3. **Key Vault oluştur** bölmesinde, kasanıza bir **ad** atayın. Kasa adları yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir. Bunlar bir sayıyla başlamamalıdır.

4. Kullanılabilir abonelikler listesinden bir abonelik seçin.

5. Var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun.

6. Fiyatlandırma katmanını seçin.

7. Mevcut erişim ilkelerinden birini seçin veya yeni bir tane oluşturun. Bir erişim ilkesi, bu kasada işlem gerçekleştirmek üzere bir Kullanıcı, uygulama veya güvenlik grubu için izin vermenizi sağlar.

8. İsteğe bağlı olarak, özelliklere erişimi etkinleştirmek için gelişmiş bir erişim ilkesi seçin.

9. Ayarları yapılandırdıktan sonra **Tamam**' ı seçin ve ardından **Oluştur**' u seçin. Anahtar Kasası dağıtımı başlar.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

1. Azure portal aracılığıyla Azure hesabınızda oturum açın.

2. **Azure Active Directory**seçin, sonra **uygulama kayıtları** **ekleyin**.

3. Uygulama için bir ad ve URL belirtin. Oluşturmak istediğiniz uygulama türü için **Web uygulaması/API** ya da **Yerel** ' i seçin. Değerleri ayarladıktan sonra **Oluştur**' u seçin.

4. **Active Directory**ve ardından **uygulama kayıtları**' nı seçin ve uygulamanızı seçin.

5. **Uygulama kimliğini** kopyalayın ve uygulama kodunuzda depolayın. Örnek uygulamalar, **uygulama kimliğine**başvururken **istemci kimliğini** kullanır.

6. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

7. Anahtar için bir açıklama ve süre belirtin.

8. **Kaydet**’i seçin.

9. **Kaydet**' i tıklatmanızdan sonra kullanılabilir hale gelen **anahtarı** kopyalayın.

## <a name="store-the-service-principal-inside-key-vault"></a>Hizmet sorumlusunu Key Vault içinde depola

1. Azure Stack için Kullanıcı portalında oturum açın, ardından daha önce oluşturduğunuz anahtar kasasını seçip **gizli** kutucuğunu seçin.

2. **Gizli** bölmesinde **Oluştur/içeri aktar**' ı seçin.

3. Gizli dizi **Oluştur** bölmesinde, seçenekler listesinden **el ile** ' yi seçin. Hizmet sorumlusunu sertifikaları kullanarak oluşturduysanız, açılan listeden sertifikaları seçin ve ardından dosyayı karşıya yükleyin.

4. Anahtarın adı olarak hizmet sorumlusu 'ndan kopyalanmış **uygulama kimliğini** girin. Anahtar adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir.

5. Hizmet sorumlusunun anahtar değerini **değer** sekmesine yapıştırın.

6. **İçerik türü**Için **hizmet sorumlusu** ' nı seçin.

7. Anahtarınız için **etkinleştirme tarihi** ve **sona erme tarihi** değerlerini ayarlayın.

8. Dağıtımı başlatmak için **Oluştur** ' u seçin.

Gizli anahtar başarıyla oluşturulduktan sonra, hizmet sorumlusu bilgileri burada depolanır. **Gizli**dizileri istediğiniz zaman seçebilir ve özelliklerini görüntüleyebilir veya değiştirebilirsiniz. **Özellikler** bölümü, dış uygulamaların bu gizliliğe erişmek için kullandığı bir Tekdüzen Kaynak tanımlayıcısı (URI) olan gizli dizi tanımlayıcısını içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet sorumlularını kullanma](azure-stack-create-service-principals.md)
- [Portal tarafından Azure Stack Key Vault yönetme](azure-stack-key-vault-manage-portal.md)  
- [PowerShell kullanarak Azure Stack Key Vault yönetme](azure-stack-key-vault-manage-powershell.md)
