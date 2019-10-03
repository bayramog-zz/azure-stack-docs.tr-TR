---
title: Azure Stack 'de kullanım bağlantısı sorunları ve hataları | Microsoft Docs
description: Azure Stack kullanım sorunlarını ve hatalarını giderme.
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
ms.openlocfilehash: 3548574ce8ece470c67101d42b115dbafe2c9a1c
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829211"
---
# <a name="usage-connectivity-errors"></a>Kullanım bağlantısı hataları

Azure Stack kullanım verileri, Azure Stack [ *Azure Bridge* bileşeni](azure-stack-usage-reporting.md) tarafından Azure 'a gönderilir. Azure Stack içindeki köprü Azure kullanım hizmeti 'ne bağlanamıyorsa aşağıdaki hatayı görürsünüz:

![Kullanım Köprüsü hatası](media/azure-stack-usage-issues/usageerror2.png)

Bu pencere, hata ve çözüm hakkında daha fazla bilgi verebilir:

![hata çözümleme](media/azure-stack-usage-issues/usageerror3.png)

## <a name="resolve-connectivity-issues"></a>Bağlantı sorunlarını çözme

Sorunu azaltmak için aşağıdaki adımları deneyin:

- Ağ yapılandırmasının, Azure Bridge uzak hizmete bağlanmasına izin verdiğini doğrulayın.

- Kayıt kaynağının kaydı, kaynak grubu ve adı için kullanılan Azure abonelik KIMLIĞINI bulmak için [ **bölge yönetimi** > **Özellikler** ](azure-stack-registration.md#verify-azure-stack-registration) dikey penceresine gidin. Kayıt kaynağının, Azure portal içindeki doğru Azure abonelik KIMLIĞI altında mevcut olduğunu doğrulayın. Bunu yapmak için Azure abonelik KIMLIĞI altında oluşturulan **tüm kaynaklar** ' a gidin ve **gizli türleri göster** kutusunu işaretleyin. Kayıt kaynağını bulamıyorsanız, Azure Stack yeniden kaydettirmek için [yenileme veya değişiklik kaydı](azure-stack-registration.md#renew-or-change-registration) bölümündeki adımları izleyin.

  ![Portal](media/azure-stack-usage-issues/stackres.png)

## <a name="error-codes"></a>Hata kodları

Bu bölümde, kullanım hata kodları açıklanmaktadır.

| Hata kodu                 | Sorun                                                                                                                                             | Düzeltme                                                                                                                                                                                                                                                                                        |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NetworkError               | Azure Stack Köprüsü Azure 'da kullanım hizmeti uç noktasına istek gönderemedi.                                                            | Bir proxy 'nin kullanım hizmeti uç noktasına erişimi engelleyip engellemediğini veya kesintiye uğratan emin olun.                                                                                                                                                                                                             |
| Istek zaman aşımına uğradı            | İstek Azure Bridge gönderildi ancak Azure 'daki kullanım hizmeti, zaman aşımı süresi içinde yanıt veremedi.                             | Bir proxy 'nin kullanım hizmeti uç noktasına erişimi engelleyip engellemediğini veya kesintiye uğratan emin olun.                                                                                                                                                                                                                        |
| LoginError                 | Microsoft Azure Active Directory kimlik doğrulaması yapılamıyor.                                                                                                             | Azure AD oturum açma uç noktasına Azure Stack içindeki tüm XRP VM 'lerinden erişilebildiğinden emin olun.                                                                                                                                                                                                                     |
| CertificateValidationError | Azure Köprüsü, Azure hizmeti ile kimlik doğrulaması yapamadığı için isteği gönderemiyor.                                    | Azure Stack XRP makinesi ile kullanım ağ geçidi uç noktası arasında bir proxy kesintiye uğratan HTTPS trafiği olup olmadığını denetleyin.                                                                                                                                                                                      |
| Yetkilendirilmemiş               | Azure Köprüsü, Azure Stack Köprüsü kimliğini doğrulayamadığından Azure 'daki kullanım hizmetine veri gönderemeyebilir. | Kayıt kaynağının değiştirilip değiştirilmediğini denetleyin ve varsa Azure Stack yeniden kaydedin. <br><br> Bazen Azure Stack ile Azure AD arasında bir zaman eşitleme sorunu bu hataya neden olabilir. Bu durumda, Azure Stack üzerindeki XRP VM 'lerinin Azure AD ile eşitlenmiş durumda olduğundan emin olun. |
|                            |                                                                                                                                                   |                                                                                                                                                                                                                                                                                                    |

Ayrıca, [Bu adımları](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs)izleyerek Azure BRIDGE, was ve Waspublik bileşenlerine yönelik günlük dosyalarını sağlamanız gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanım verilerini Azure 'a raporlama Azure Stack](azure-stack-usage-reporting.md)hakkında daha fazla bilgi edinin.
- Kayıt sürecinizdeki tetiklendiklerinde hata iletilerini gözden geçirmek için bkz. [kiracı kayıt hata iletileri](azure-stack-registration-errors.md).
- [Bulut hizmeti sağlayıcıları Için kullanım raporlama altyapısı](azure-stack-csp-ref-infrastructure.md)hakkında daha fazla bilgi edinin.
