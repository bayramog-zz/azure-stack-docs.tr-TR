---
title: Azure Stack anahtar Kasası'nda hizmet sorumlusu kimlik bilgileri Store | Microsoft Docs
description: Key Vault hizmet sorumlusu kimlik bilgileri Azure Stack üzerinde nasıl depoladı öğrenin
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
ms.date: 05/21/2019
ms.author: sethm
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: efa8dda8061ce81d751e9cce47c5e81a3917f2bf
ms.sourcegitcommit: ad2f2cb4dc8d5cf0c2c37517d5125921cff44cdd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67138851"
---
# <a name="store-service-principal-credentials-in-azure-stack-key-vault"></a>Azure Stack anahtar Kasası'nda hizmet sorumlusu kimlik bilgileri Store

Uygulamaları Azure Stack'te genellikle geliştirme, hizmet sorumlusu oluşturma ve dağıtmadan önce kimliğini doğrulamak için bu kimlik bilgilerini kullanarak gerektirir. Ancak, bazen, hizmet sorumlusu için depolanan kimlik bilgileri kaybedersiniz. Bu makalede, bir hizmet sorumlusu oluşturma ve değerleri daha sonra alma işlemi için Azure Key Vault'ta depolamak açıklar.

Key Vault hakkında daha fazla bilgi için bkz. [bu makalede](azure-stack-key-vault-intro.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure Key Vault hizmeti içeren bir teklif aboneliği.
- PowerShell, Azure Stack ile kullanmak için yapılandırılır.

## <a name="key-vault-in-azure-stack"></a>Azure Stack'te anahtar kasası

Şifreleme anahtarlarını korumak için Azure Stack Key Vault'ta yardımcı olur ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanabilirsiniz. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları şifreleyebilirsiniz.

Bir anahtar kasası oluşturmak için aşağıdaki adımları izleyin:

1. Azure Stack portalında oturum açın.

2. Panoda **+ kaynak Oluştur**, ardından **güvenlik + kimlik**, ardından **anahtar kasası.**

   ![Anahtar kasası oluşturma](media/azure-stack-key-vault-store-credentials/create-key-vault.png)

3. İçinde **Key Vault Oluştur** bölmesinde atamak bir **adı** kasanız için. Kasa adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir. Bunlar, bir sayı ile başlayamaz olmamalıdır.

4. Kullanılabilir abonelikler listesinden bir abonelik seçin.

5. Mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun.

6. Fiyatlandırma katmanını seçin.

7. Var olan erişim ilkelerinden birini seçin veya yeni bir tane oluşturun. Bir erişim ilkesi, bir kullanıcı, uygulama veya bir güvenlik grubu bu kasayla işlemleri gerçekleştirmek izinler sağlar.

8. İsteğe bağlı olarak, özelliklere erişimi etkinleştirmek için bir Gelişmiş erişim ilkesini seçin.

9. Ayarları yapılandırdıktan sonra seçin **Tamam**ve ardından **Oluştur**. Anahtar kasası dağıtım başlar.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

1. Azure portalı üzerinden Azure hesabınızda oturum açın.

2. Seçin **Azure Active Directory**, ardından **uygulama kayıtları**, ardından **Ekle**.

3. Uygulama için bir ad ve URL belirtin. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **Oluştur**.

4. Seçin **Active Directory**, ardından **uygulama kayıtları**ve uygulamanızı seçin.

5. Kopyalama **uygulama kimliği** ve uygulama kodunuzda depolayın. Örnek uygulamalar kullanımı **istemci kimliği** söz konusu olduğunda **uygulama kimliği**.

6. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

7. Anahtarın süresi ve açıklama sağlayın.

8. **Kaydet**’i seçin.

9. Kopyalama **anahtarı** , kullanılabilir tıkladığınız sonra **Kaydet**.

## <a name="store-the-service-principal-inside-key-vault"></a>Hizmet sorumlusu Key Vault içinde Store

1. Kullanıcı portalında oturum için Azure Stack, ardından daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **gizli** Döşe.

2. İçinde **gizli** bölmesinde **Oluştur/içeri aktarma**.

3. İçinde **gizli dizi oluşturma** bölmesinde **el ile** seçeneklerini listeden. Sertifikalarını kullanarak hizmet sorumlusu oluşturduysanız sertifikaları aşağı açılan listeden seçin ve ardından dosyayı karşıya yükleyin.

4. Girin **uygulama Kimliğini** anahtarınızı adı olarak hizmet sorumlusundan kopyalanır. Anahtar adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir.

5. Hizmet sorumlusu içine anahtarınızdan değerini yapıştırın **değer** sekmesi.

6. Seçin **hizmet sorumlusu** için **içerik türü**.

7. Ayarlama **etkinleştirme tarihi** ve **sona erme tarihi** anahtarınız için değerler.

8. Seçin **Oluştur** dağıtımı başlatmak için.

Gizli dizi başarıyla oluşturulduktan sonra hizmet sorumlusu bilgileri burada depolanır. Altında herhangi bir zamanda seçin **gizli dizileri** görüntüleyebilir veya özellikleri değiştirin. Özellikler bölümü, dış uygulama bu gizli dizi erişmek için bir Tekdüzen Kaynak Tanımlayıcısı (URI) olan gizli tanımlayıcısını içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet sorumlularını kullanma](azure-stack-create-service-principals.md)
- [Anahtar Kasası'nda Azure Stack portal ile yönetme](azure-stack-key-vault-manage-portal.md)  
- [Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme](azure-stack-key-vault-manage-powershell.md)