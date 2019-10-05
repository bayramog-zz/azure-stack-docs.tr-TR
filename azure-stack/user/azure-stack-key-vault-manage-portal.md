---
title: Portalı kullanarak Azure Stack Key Vault yönetme | Microsoft Docs
description: Azure Stack portalını kullanarak Azure Stack Key Vault yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2019
ms.author: sethm
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: fc0907ffb905012043a3e446fcc29cc9e06f6932
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961498"
---
# <a name="manage-key-vault-in-azure-stack-using-the-portal"></a>Portalı kullanarak Azure Stack Key Vault yönetme

Bu makalede Azure Stack portalını kullanarak Azure Stack bir anahtar kasasının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Azure Key Vault hizmetini içeren bir teklife abone olmanız gerekir.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. [Kullanıcı portalında](https://portal.local.azurestack.external)oturum açın.

2. Panodan **+ kaynak oluştur**' u seçin, sonra **Güvenlik ve Kimlik**ve ardından **Key Vault**.

    ![Key Vault ekranı](media/azure-stack-key-vault-manage-portal/image1.png)

3. **Key Vault oluştur** bölmesinde, kasanıza bir **ad** atayın. Kasa adları yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir. Bir sayıyla başlamamaları gerekir.

4. Kullanılabilir abonelikler listesinden bir **abonelik** seçin. Key Vault hizmeti sunan tüm abonelikler açılan listede görüntülenir.

5. Var olan bir **kaynak grubunu**seçin veya yeni bir tane oluşturun.

6. **Fiyatlandırma katmanını**seçin. Azure Stack Geliştirme Seti (ASDK), Anahtar kasaları yalnızca **Standart** SKU 'ları destekler.

7. Mevcut **erişim ilkelerinden** birini seçin veya yeni bir tane oluşturun. Erişim ilkesi, bu kasada işlem gerçekleştirmek üzere bir Kullanıcı, uygulama veya güvenlik grubu için izin vermenizi sağlar.

8. İsteğe bağlı olarak, özelliklere erişimi etkinleştirmek için **Gelişmiş bir erişim ilkesi** seçin. Örneğin: dağıtım için sanal makineler (VM), şablon dağıtımı için Kaynak Yöneticisi ve birim şifrelemesi için Azure disk şifrelemesi 'ne erişim.

9. Ayarları yapılandırdıktan sonra **Tamam**' ı seçin ve ardından **Oluştur**' u seçin. Bu adım Anahtar Kasası dağıtımını başlatır.

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli dizileri yönetme

Bir Anahtar Kasası oluşturduktan sonra, kasadaki anahtarları ve gizli dizileri oluşturmak ve yönetmek için aşağıdaki yordamı kullanın:

### <a name="create-a-key"></a>Anahtar oluşturma

1. [Kullanıcı portalında](https://portal.local.azurestack.external)oturum açın.

2. Panodan **tüm kaynaklar**' ı seçin, daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **anahtarlar** kutucuğunu seçin.

3. **Anahtarlar** bölmesinde **Ekle**' yi seçin.

4. **Anahtar oluştur** bölmesinde, **Seçenekler**listesinden, anahtar oluşturmak için kullanmak istediğiniz yöntemi seçin. Yeni bir anahtar **oluşturabilir** , var olan bir anahtarı **yükleyebilir** veya bir anahtarın yedeğini seçmek için **yedekleme geri yükleme** ' yi kullanabilirsiniz.

5. Anahtarınız için bir **ad** girin. Anahtar adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir.

6. İsteğe bağlı olarak, anahtar için **etkinleştirme tarihini ayarla** ve **sona erme tarihi değerlerini ayarla** ' yı yapılandırın.

7. Dağıtımı başlatmak için **Oluştur** ' u seçin.

Anahtar başarılı bir şekilde oluşturulduktan sonra **anahtarlar** altında seçim yapabilir ve özelliklerini görüntüleyebilir veya değiştirebilirsiniz. Özellikler bölümü, dış uygulamaların bu anahtara erişmek için kullandığı bir Tekdüzen Kaynak tanımlayıcısı (URI) olan **anahtar tanımlayıcısını**içerir. Bu anahtardaki işlemleri sınırlandırmak için **Izin verilen işlemler**altındaki ayarları yapılandırın.

![URI anahtarı](media/azure-stack-key-vault-manage-portal/image4.png)

### <a name="create-a-secret"></a>Gizli dizi oluşturma

1. [Kullanıcı portalında](https://portal.local.azurestack.external)oturum açın.

2. Panodan **tüm kaynaklar**' ı seçin, daha önce oluşturduğunuz anahtar kasasını seçin ve ardından **gizlilikler** kutucuğunu seçin.

3. **Gizlilikler**altında **Ekle**' yi seçin.

4. **Gizli dizi oluştur**altında, **karşıya yükleme seçenekleri**listesinden, gizli dizi oluşturmak istediğiniz bir seçeneği belirleyin. Gizli dizi için bir değer girerseniz veya yerel makinenizden bir **sertifika** yüklerseniz **el ile** gizli dizi oluşturabilirsiniz.

5. Gizli dizi için bir **ad** girin. Gizli dizi adı yalnızca alfasayısal karakterler ve kısa çizgi (-) karakterini içerebilir.

6. İsteğe bağlı olarak, **içerik türünü**belirtin ve **etkinleştirme tarihini ayarla** ve gizli anahtar Için **süre sonu tarihi ayarla** değerlerini yapılandırın.

7. Dağıtımı başlatmak için **Oluştur** ' u seçin.

Gizli anahtar başarıyla oluşturulduktan sonra **gizli** dizi altında seçim yapabilir ve özelliklerini görüntüleyebilir veya değiştirebilirsiniz. **Gizli dizi kimliği** , dış uygulamaların bu gizli dizi erişimi için KULLANABILECEĞI bir URI 'dir.

![URI gizli dizisi](media/azure-stack-key-vault-manage-portal/image5.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault depolanan parolayı alarak VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault depolanan sertifika ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
