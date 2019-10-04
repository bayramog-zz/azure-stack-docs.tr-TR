---
title: Azure Stack Kullanıcı portalını kullanın | Microsoft Docs
description: Azure Stack 'da Kullanıcı portalına erişmeyi ve kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: efemmano
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 392d850e5d74a1bd069653aae3b9def6438c5288
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909569"
---
# <a name="use-the-azure-stack-user-portal"></a>Azure Stack Kullanıcı portalını kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Genel tekliflere abone olmak ve bu tekliflerin sağladığı Hizmetleri kullanmak için Azure Stack portalını kullanın. Küresel Azure portal kullandıysanız, sitenin nasıl çalıştığını zaten öğrenirsiniz.

## <a name="access-the-portal"></a>Portala erişin

Azure Stack operatörünüz (bir hizmet sağlayıcısı ya da kuruluşunuzdaki bir yönetici), portala erişmek için doğru URL 'yi bilmenizi sağlar.

- Tümleşik bir sistem için, URL, işlecin bölgesine ve dış etki alanı adına göre değişir ve biçiminde https://portal.&lt olacaktır; *bölge.* &gt;&lt; *FQDN* &gt;.
- Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, Portal adresi olur https://portal.local.azurestack.external.
- Tüm Azure Stack dağıtımları için varsayılan saat dilimi Eşgüdümlü Evrensel Saat (UTC) olarak ayarlanır. Azure Stack yüklerken bir saat dilimi seçebilirsiniz ancak yükleme sırasında otomatik olarak UTC 'ye geri döner.

## <a name="customize-the-dashboard"></a>Panoyu özelleştirme

Pano varsayılan kutucuk kümesini içerir. Varsayılan panoyu değiştirmek için **panoyu Düzenle** ' yi seçin veya özel bir pano oluşturmak için **Yeni Pano** ' yı seçin. Bölmeleri, kutucuk ekleyerek veya kaldırarak kolayca özelleştirebilirsiniz. Örneğin, bir Işlem kutucuğu eklemek için **+ kaynak oluştur**' u seçin. **işlem**' a sağ tıklayın ve ardından **panoya sabitle**' yi seçin.

![Azure Stack Kullanıcı portalının ekran görüntüsü](media/azure-stack-use-portal/userportal.png)

Panoyu özgün ayarlarına geri yüklemek için:
1.  **Panoyu Düzenle**' yi seçin. 
2.  Sağ tıklayın ve **varsayılan duruma Sıfırla**' yı seçin.

## <a name="create-subscription-and-browse-available-resources"></a>Abonelik oluşturma ve kullanılabilir kaynaklara gözatın

Aboneliğiniz yoksa bir teklife abone olmanız gerekir. Bundan sonra, kullanılabilir kaynaklara gözatabilmeniz gerekir. Kaynaklara gitmek ve kaynakları oluşturmak için aşağıdaki yaklaşımlardan birini kullanın:

- Panoda **Market** kutucuğunu seçin.
- **Tüm kaynaklar** kutucuğunda **kaynak oluştur**' u seçin.
- Sol gezinti bölmesinde **+ kaynak oluştur**' u seçin.

## <a name="learn-how-to-use-available-services"></a>Kullanılabilir hizmetleri nasıl kullanacağınızı öğrenin

Kullanılabilir hizmetlerin nasıl kullanılacağına ilişkin kılavuza ihtiyacınız varsa, kullanabileceğiniz farklı seçenekler olabilir.

- Kuruluşunuz veya hizmet sağlayıcınız kendi belgelerini sağlayabilir, bu genellikle özelleştirilmiş hizmetler veya uygulamalar sunduklarında bu durumdur.
- Üçüncü taraf uygulamaların kendi belgeleri vardır.
- Azure ile tutarlı hizmetler için öncelikle Azure Stack belgelerini incelemenizi önemle öneririz. Azure Stack kullanıcı belgelerine erişmek için, yardım simgesini ( **?** ) seçin ve ardından **Yardım + Destek**' i seçin.

    ![Kullanıcı arabirimindeki yardım ve destek seçeneği](media/azure-stack-use-portal/HelpAndSupport.png)

    Özellikle, başlamak için aşağıdaki makaleleri incelemenizi öneririz:

    - [Önemli noktalar: Azure Stack](azure-stack-considerations.md)için Hizmetleri kullanma veya uygulama oluşturma.
    - Belgelerde **Hizmetleri kullanma** bölümünde her bir hizmet için bir dikkat edilecek konular vardır. Önemli noktalar sayfasında, Azure 'da sunulan hizmet ile Azure Stack sunulan hizmet arasındaki farklar açıklanmaktadır. Bir örnek için bkz. [VM değerlendirmeleri](azure-stack-vm-considerations.md). Azure Stack için benzersiz olan **Hizmetleri kullan** bölümünde başka bilgiler olabilir.

      Azure belgelerini bir hizmet için genel başvuru olarak kullanabilirsiniz, ancak bu farklılıklara dikkat etmeniz gerekir. **Hızlı başlangıç öğreticileri** kutucuğunda bulunan belge bağlantılarının Azure belgelerine anlayın.

## <a name="get-support"></a>Destek alın

Desteğe ihtiyacınız varsa, yardım için kuruluşunuzun veya hizmet sağlayıcınızla iletişim kurun.

Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, [Azure Stack Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) tek destek kaynağıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Önemli noktalar: Azure Stack için Hizmetleri kullanma veya uygulama oluşturma](azure-stack-considerations.md)
