---
title: Azure Stack Key Vault giriş | Microsoft Docs
description: Key Vault anahtarları ve gizli dizileri Azure Stack nasıl yönettiğini öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: sethm
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: fc68f80688f6b8cbe0376d332d706c9dc7b6dd92
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842900"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Azure Stack Key Vault giriş

## <a name="prerequisites"></a>Önkoşullar

* Azure Key Vault hizmetini içeren bir teklife abone olun.  
* [PowerShell, Azure Stack birlikte kullanılmak üzere yapılandırıldı](azure-stack-powershell-configure-user.md).

## <a name="key-vault-basics"></a>Key Vault temel bilgiler

Azure Stack Key Vault, bulut uygulamalarının ve hizmetlerinin kullandığı şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Key Vault kullanarak anahtarları ve gizli dizileri şifreleyebilirsiniz, örneğin:

* Kimlik doğrulama anahtarları
* Depolama hesabı anahtarları
* Veri şifreleme anahtarları
* . pfx dosyaları
* Parolalar

Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve ardından bunları üretim anahtarlarına sorunsuz bir şekilde geçirebilir. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

Azure Stack aboneliği olan anybody, Anahtar kasaları oluşturup kullanabilir. Key Vault, geliştiricilerin ve güvenlik yöneticilerinin avantajları olsa da, bir kuruluş için diğer Azure Stack hizmetlerini yöneten operatör onu uygulayabilir ve yönetebilir. Örneğin, Azure Stack işleci bir Azure Stack aboneliğiyle oturum açabilir ve anahtarların depolayabileceği kuruluş için bir kasa oluşturabilir. Bu işlem yapıldıktan sonra şunları yapabilir:

* Anahtar veya gizli dizi oluşturun veya içeri aktarın.
* Bir anahtarı veya parolayı iptal edin veya silin.
* Kullanıcıları veya uygulamaları, anahtar kasasına erişmek için yetkilendirirler ve bu sayede anahtarlarını ve gizli dizilerini yönetebilir veya kullanabilir.
* Anahtar kullanımını yapılandırın (örneğin, imzalama veya şifreleme).

İşleci daha sonra geliştiricilere, uygulamalarından çağrı yapmak için Tekdüzen Kaynak tanımlayıcıları (URI 'Ler) sağlayabilir. İşleçler ayrıca güvenlik yöneticilerine anahtar kullanımı günlük bilgilerini sağlayabilir.

Geliştiriciler anahtarları doğrudan API 'Leri kullanarak da yönetebilir. Daha fazla bilgi için, [Key Vault Geliştirici Kılavuzu](/azure/key-vault/key-vault-developers-guide)' na bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki senaryolarda, Key Vault geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılayacak şekilde nasıl yardımcı olduğu açıklanır.

### <a name="developer-for-an-azure-stack-app"></a>Azure Stack uygulaması için geliştirici

**Gidermek** İmzalama ve şifreleme için anahtarları kullanan Azure Stack bir uygulama yazmak istiyorum. Çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olması için bu anahtarların uygulamamın dışında olmasını istiyorum.

**Ekstre** Anahtarlar bir kasada depolanır ve gerektiğinde bir URI tarafından çağırılır.

### <a name="developer-for-software-as-a-service-saas"></a>Hizmet olarak yazılım (SaaS) geliştiricisi

**Gidermek** Müşterinin anahtarları ve gizli dizilerimin sorumluluğunu veya olası borcumu istemiyorum. Müşterilerin, temel yazılım özelliklerini sağlayan en iyi ne yaptığına odaklanabilmesi için kendi anahtarlarını sahip olmasını ve yönetmesini istiyorum.

**Ekstre** Müşteriler, Azure Stack içinde kendi anahtarlarını içeri aktarabilir ve yönetebilir.

### <a name="chief-security-officer-cso"></a>Güvenlik Müdürü (CSO)

**Gidermek** Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum.

**Ekstre** Key Vault, Microsoft 'un anahtarlarınızı görmemesi veya ayıklanmaması için tasarlanmıştır. Bir uygulamanın, müşteri anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Key Vault anahtarları uygulama adına kullanır. Uygulama, müşteri anahtarlarını görmez. Birden çok Azure Stack hizmet ve kaynak kullandığımızda, anahtarları Azure Stack tek bir konumdan yönetebilirsiniz. Kasa, Azure Stack kaç kasasının olduğunu, destekledikleri bölgeleri ve hangi uygulamaların bunları kullandığını bağımsız olarak tek bir arabirim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Portal tarafından Azure Stack Key Vault yönetme](azure-stack-key-vault-manage-portal.md)  
* [PowerShell kullanarak Azure Stack Key Vault yönetme](azure-stack-key-vault-manage-powershell.md)
