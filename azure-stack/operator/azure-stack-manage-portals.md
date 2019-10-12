---
title: Azure Stack 'de yönetici portalını kullanma | Microsoft Docs
description: Azure Stack 'de yönetici portalını kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/07/2019
ms.author: justinha
ms.reviewer: efemmano
ms.lastreviewed: 06/07/2019
ms.openlocfilehash: a5a2c86d4680d38b3923eea4a04cacbf61fff108
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283507"
---
# <a name="use-the-administrator-portal-in-azure-stack"></a>Azure Stack 'da Yönetici portalını kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack iki Portal vardır: Yönetici portalı ve Kullanıcı portalı. Azure Stack operatörü olarak, yönetim portalını günlük yönetimi ve Azure Stack işlemleri için kullanırsınız.

## <a name="access-the-administrator-portal"></a>Yönetici portalına erişim

Yönetici portalına erişmek için Portal URL 'sine gidin ve Azure Stack operatör kimlik bilgilerinizi kullanarak oturum açın. Tümleşik bir sistem için Portal URL 'SI, Azure Stack dağıtımınızın bölge adına ve dış tam etki alanı adına (FQDN) göre değişir. Yönetici portalı URL 'SI her zaman Azure Stack Geliştirme Seti (ASDK) dağıtımları için aynıdır.

| Ortam | Yönetici portalı URL 'SI |   
| -- | -- | 
| ASDK| https://adminportal.local.azurestack.external  |
| Tümleşik sistemler | https://adminportal.&lt; *bölge*&gt;. &lt;*FQDN*&gt; | 
| | |

> [!TIP]
> Bir ASDK ortamında, öncelikle Uzak Masaüstü Bağlantısı veya bir sanal özel ağ (VPN) üzerinden [geliştirme seti ana bilgisayarına bağlanabildiğinizden](../asdk/asdk-connect.md) emin olmanız gerekir.

 ![Azure Stack Yönetici portalı](media/azure-stack-manage-portals/admin-portal.png)

Tüm Azure Stack dağıtımları için varsayılan saat dilimi Eşgüdümlü Evrensel Saat (UTC) olarak ayarlanır.

Yönetici portalında şunları yapabilirsiniz:

* [Azure ile Azure Stack kaydetme](azure-stack-registration.md)
* [Marketi doldurun](azure-stack-download-azure-marketplace-item.md)
* [Kullanıcılar için planlar, teklifler ve abonelikler oluşturma](service-plan-offer-subscription-overview.md)
* [Sistem durumunu ve Uyarıları izleme](azure-stack-monitor-health.md)
* [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)

**Hızlı Başlangıç Öğreticisi** kutucuğu, en yaygın görevlere yönelik çevrimiçi belgelere bağlantılar sağlar.

Bir operatör, yönetici portalında sanal makineler (VM), sanal ağlar ve depolama hesapları gibi kaynaklar oluşturabilse de, kaynak oluşturmak ve test etmek için [Kullanıcı portalında oturum açmanız](../user/azure-stack-use-portal.md) gerekir.

>[!NOTE]
>Hızlı başlangıç öğreticisinde bir **sanal makine oluştur** bağlantısı, yönetici PORTALıNDA bir VM oluşturur, ancak bu yalnızca Azure Stack başarıyla dağıtıldığını doğrulamaya yöneliktir.

## <a name="understand-subscription-behavior"></a>Abonelik davranışını anlama

Yönetici portalında varsayılan olarak oluşturulan üç abonelik vardır: tüketim, varsayılan sağlayıcı ve ölçüm. Bir operatör olarak, genellikle *varsayılan sağlayıcı aboneliğini*kullanacaksınız. Başka bir abonelik ekleyemez ve bunları yönetici portalında kullanamazsınız.

Diğer abonelikler, kullanıcılar tarafından, sizin için oluşturduğunuz planları temel alarak Kullanıcı portalında oluşturulur. Ancak, Kullanıcı Portalı yönetici portalının yönetim veya işletimsel yetilerine erişim sağlamaz.

Yönetici ve Kullanıcı portalları Azure Resource Manager ayrı örnekleri tarafından desteklenir. Bu Azure Resource Manager ayrımı nedeniyle abonelikler çapraz portallara sahip değildir. Örneğin, bir Azure Stack operatörü olarak, Kullanıcı portalında oturum açtığınızda, *varsayılan sağlayıcı aboneliğine*erişemezsiniz. Herhangi bir yönetim işlevlerine erişiminiz olmasa da, mevcut kamu tekliflerinden kendiniz için abonelikler oluşturabilirsiniz. Kullanıcı portalında oturum açtığınız sürece Kiracı Kullanıcı olarak kabul edilir.

  >[!NOTE]
  >Bir ASDK ortamında, bir Kullanıcı Azure Stack işleçle aynı kiracı dizinine aitse, yönetici portalında oturum açması engellenmez. Ancak, yönetici işlevlerine erişemez veya Kullanıcı Portalı 'nda kullanılabilir olan erişim tekliflerine abonelik ekleyemazlar.

## <a name="administrator-portal-tips"></a>Yönetici portalı ipuçları

### <a name="customize-the-dashboard"></a>Panoyu özelleştirme

Panoda bir dizi varsayılan kutucuk bulunur. **Panoyu Düzenle** ' yi seçerek Varsayılan panoyu değiştirebilir veya özel bir pano eklemek için **Yeni Pano** ' yı seçebilirsiniz. Ayrıca, bir panoya kutucuk ekleyebilirsiniz. Örneğin **+ kaynak oluştur**' u seçin, **teklifler + planlar**' a sağ tıklayın ve ardından **panoya sabitle**' yi seçin.

Bazen portalda boş bir pano görebilirsiniz. Panoyu kurtarmak için **panoyu Düzenle**' ye tıklayın ve ardından sağ tıklayıp **varsayılan duruma Sıfırla**' yı seçin.

### <a name="quick-access-to-online-documentation"></a>Çevrimiçi belgelere hızlı erişim

Azure Stack operatör belgelerine erişmek için, yönetici portalının sağ üst köşesindeki Yardım ve destek simgesini (soru işareti) kullanın. İmlecinizi simgeye taşıyın ve ardından **Yardım + Destek**' i seçin.

### <a name="quick-access-to-help-and-support"></a>Yardım ve desteğe hızlı erişim

Yönetici portalının sağ üst köşesindeki Yardım simgesine (soru işareti) tıklarsanız, **Yardım ve destek**' e tıklayın ve ardından **destek**altında **Yeni destek isteği** ' ne tıklayın, aşağıdaki sonuçlardan biri olur:

- Tümleşik bir sistem kullanıyorsanız, bu eylem Microsoft Müşteri Destek Hizmetleri (CSS) ile bir destek bileti açmak için doğrudan bir site açar. Microsoft destek 'e veya özgün ekipman üreticisi (OEM) donanım satıcısı destek aracılığıyla ne zaman gidebileceğinizi anlamak için [nereden destek alınacağını](azure-stack-manage-basics.md#where-to-get-support) inceleyin.
- ASDK kullanıyorsanız, bu eylem [Azure Stack forumları sitesini](https://social.msdn.microsoft.com/Forums/home?forum=AzureStack) doğrudan açar. Bu Forumlar düzenli olarak izlenir. ASDK bir değerlendirme ortamı olduğundan, Microsoft CSS aracılığıyla sunulan resmi bir destek yoktur.

### <a name="quick-access-to-the-azure-roadmap"></a>Azure yol haritasına hızlı erişim

Yönetici portalının sağ üst köşesinde bulunan **Yardım ve destek** (soru işareti) seçeneğini belirleyip **Azure yol haritası**' nı seçerseniz, yeni bir tarayıcı sekmesi açılır ve sizi Azure yol haritası 'na götürür. **Ürünler** arama kutusuna **Azure Stack** yazarak, tüm Azure Stack yol haritası güncelleştirmelerini görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Azure Ile kaydedin](azure-stack-registration.md) ve kullanıcılarınıza sunmak Için [Azure Stack Market](azure-stack-marketplace.md) 'i maddelerle doldurun.
