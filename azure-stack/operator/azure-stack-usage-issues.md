---
title: Azure Stack hatalarını ve kullanım bağlantı sorunları | Microsoft Docs
description: Azure Stack kullanım sorunlarını ve hataları giderme.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 06/27/2019
ms.openlocfilehash: 57c546ec3583c9e04594e4da542a3c2ce3f72c62
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419763"
---
# <a name="usage-connectivity-errors"></a>Kullanım bağlantı hataları

Azure Stack kullanım verileri, Azure tarafından gönderildiği [ *Azure köprüsü* bileşen](azure-stack-usage-reporting.md) Azure Stack'te. Azure Stack içinde köprü Azure kullanım hizmetine bağlanamıyor, aşağıdaki hatayı görürsünüz:

![Kullanım köprü hatası](media/azure-stack-usage-issues/usageerror2.png)

Pencere hata ve çözümü hakkında daha fazla bilgi sağlayabilir:

![Hata Çözümleme](media/azure-stack-usage-issues/usageerror3.png)

## <a name="resolve-connectivity-issues"></a>Bağlantı sorunlarını gidermek

Sorunu gidermek için aşağıdaki adımları deneyin:

- Bu ağ yapılandırması, uzak hizmete bağlanmak Azure köprüsü izin verdiğinden emin olun.

- Git [ **bölge Yönetimi** > **özellikleri** ](azure-stack-registration.md#verify-azure-stack-registration) dikey penceresinde, kayıt, kaynak grubu ve adı için kullanılan Azure abonelik kimliği bulunamadı Kayıt kaynak. Azure portalında doğru Azure abonelik kimliği altında kayıt kaynağı var olduğunu doğrulayın. Bunu yapmak için Git **tüm kaynakları** Azure abonelik kimliği ve onay altında oluşturulan **gizli türleri Göster** kutusu. Kayıt kaynağı bulamıyorsanız, adımları [yenileme ya da değişiklik kaydı](azure-stack-registration.md#renew-or-change-registration) Azure Stack yeniden kaydetmek için.

  ![Portal](media/azure-stack-usage-issues/stackres.png)

## <a name="error-codes"></a>Hata kodları

Bu bölümde, kullanım hata kodları açıklanmaktadır.

| Hata kodu                 | Sorun                                                                                                                                             | Düzeltme                                                                                                                                                                                                                                                                                        |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NetworkError               | Kullanım Hizmeti uç noktası azure'da isteği göndermek Azure Stack köprüsü silemiyor.                                                            | Bir proxy engelleme veya kullanım Hizmeti uç noktası erişimi kesintiye olmadığını kontrol edin.                                                                                                                                                                                                             |
| RequestTimedOut            | Azure Köprüsü'nden bir istek gönderildi ancak azure'da kullanım hizmeti zaman aşımı süresi içinde yanıt vermedi.                             | Bir proxy engelleme veya kullanım Hizmeti uç noktası erişimi kesintiye olmadığını kontrol edin.                                                                                                                                                                                                                        |
| LoginError                 | Microsoft Azure Active Directory ile kimlik doğrulaması yapılamıyor.                                                                                                             | Azure AD oturum açma uç noktası Azure Stack'te tüm XRP vm'lerinden erişilebilir olduğundan emin olun.                                                                                                                                                                                                                     |
| CertificateValidationError | Azure köprüsü Azure hizmeti ile kimlik doğrulaması mümkün olmadığı için istek gönderebilirsiniz değil.                                    | Azure Stack XRP makine kullanım ağ geçidi uç noktası arasındaki HTTPS trafiğini kesintiye bir ara sunucu olup olmadığını denetleyin.                                                                                                                                                                                      |
| Yetkilendirilmemiş               | Azure hizmet Azure Stack köprüsü kimlik doğrulaması için olmadığından Azure köprüsü, azure'da kullanım hizmeti için veri göndermeye ilişkin kuramıyor. | Kayıt kaynak değiştirilmiş denetleyin ve bu durumda, Azure Stack yeniden kaydedin. <br><br> Bazı durumlarda, bir zaman Azure Stack ve Azure AD arasında eşitleme sorunu bu hataya neden olabilir. Bu durumda, zamanları Azure Stack'te XRP vm'lerinde Azure AD ile eşitlenmiş durumda emin olun. |
|                            |                                                                                                                                                   |                                                                                                                                                                                                                                                                                                    |

Ayrıca, Azure Köprüsü ve WAS WASPublic bileşenleri için günlük dosyalarını sağlıyorlar gerekebilir [adımları](azure-stack-diagnostics.md#log-collection-tool).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure'da Azure Stack kullanım verilerini raporlama](azure-stack-usage-reporting.md).
- Kayıt işleminizi tetiklendikten hata iletilerini gözden geçirin için bkz: [Kiracı kayıt hata iletileri](azure-stack-registration-errors.md).
- Daha fazla bilgi edinin [kullanım raporlama altyapısı için bulut hizmeti sağlayıcıları](azure-stack-csp-ref-infrastructure.md).
