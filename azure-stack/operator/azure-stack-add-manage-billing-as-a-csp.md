---
title: Bulut çözümü sağlayıcısı olarak Azure Stack için kullanımı ve faturalandırmayı yönetme | Microsoft Docs
description: Azure Stack bir bulut çözümü sağlayıcısı (CSP) olarak kaydetmeyi ve faturalandırma için müşterilerin nasıl ekleneceğini öğrenin.
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
ms.date: 10/02/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: e14c57cb5d0444c68619e6be5db688d7b1542e93
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961883"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-solution-provider"></a>Bulut çözümü sağlayıcısı olarak Azure Stack için kullanımı ve faturalandırmayı yönetme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede, Azure Stack bir bulut çözümü sağlayıcısı (CSP) olarak nasıl kaydedileceği ve müşterilerin nasıl ekleneceği açıklanmaktadır.

CSP olarak, Azure Stack kullanarak farklı müşterilerle çalışırsınız. Her müşterinin Azure 'da bir CSP aboneliği vardır. Azure Stack kullanımı her kullanıcı aboneliğine yönlendirmelidir.

Aşağıdaki şekilde, paylaşılan hizmetleri hesabınızı seçmek ve Azure hesabını Azure Stack hesabıyla kaydetmek için gereken adımlar gösterilmektedir. Kaydolduktan sonra, son müşterilerinizi ekleyebilirsiniz:

[(media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png "Bulut çözümü sağlayıcısı olarak kullanımı ve yönetimi etkinleştirmek için") ![bir bulut çözümü sağlayıcısı işlemi olarak kullanımı ve yönetimi etkinleştirme işlemi]](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>CSP veya APSS aboneliği oluşturma

### <a name="csp-subscription-types"></a>CSP Abonelik türleri

Azure Stack için kullandığınız paylaşılan hizmetler hesabının türünü seçin. Çok kiracılı bir Azure Stack'in kaydında kullanılabilecek abonelik türleri şunlardır:

- Bulut Çözümü Sağlayıcısı
- İş Ortağı Paylaşılan Hizmetler aboneliği

#### <a name="azure-partner-shared-services"></a>Azure Iş ortağı paylaşılan hizmetleri

Azure Iş ortağı paylaşılan hizmetleri (APSS) abonelikleri, doğrudan CSP veya CSP dağıtıcısı Azure Stack çalıştığında kayıt için tercih edilen seçenektir.

APSS abonelikleri, paylaşılan hizmetler kiracısı ile ilişkilendirilir. Azure Stack kaydettiğinizde, aboneliğin sahibi olan bir hesabın kimlik bilgilerini sağlarsınız. Azure Stack kaydetmek için kullandığınız hesap, dağıtım için kullandığınız yönetici hesabından farklı olabilir. Ayrıca, iki hesabın aynı etki alanına ait olması gerekmez; zaten kullandığınız kiracıyı kullanarak dağıtım yapabilirsiniz. Örneğin, `ContosoCSP.onmicrosoft.com`kullanabilir ve sonra farklı bir kiracı kullanarak kaydolabilirsiniz; Örneğin, `IURContosoCSP.onmicrosoft.com`. Günlük Azure Stack yönetimi gerçekleştirirken `ContosoCSP.onmicrosoft.com` kullanarak oturum açmayı unutmamanız gerekir. Kayıt işlemleri gerçekleştirmeniz gerektiğinde `IURContosoCSP.onmicrosoft.com` kullanarak Azure 'da oturum açın.

APSS aboneliklerinin açıklaması ve bunların nasıl oluşturulacağı hakkında bilgi için bkz. [Azure Iş ortağı paylaşılan hizmetleri ekleme](/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP abonelikleri

CSP abonelikleri, CSP satıcısı veya son müşteri Azure Stack çalıştığında kayıt için tercih edilen seçenektir.

## <a name="register-azure-stack"></a>Azure Stack kaydetme

Azure Stack Azure ile kaydetmek için önceki bölümde bulunan bilgiler kullanılarak oluşturulan APSS aboneliğini kullanın. Daha fazla bilgi için bkz. [Azure aboneliğinize kaydolma Azure Stack](azure-stack-registration.md).

## <a name="add-end-customer"></a>Son müşteri Ekle

Yeni bir kiracının kaynak kullanımının CSP aboneliğine bildirildiği şekilde Azure Stack yapılandırmak için, bkz. [kullanım ve faturalandırma için kiracı ekleme Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Doğru abonelikleri ücretlendirin

Azure Stack, *kayıt*adlı bir özellik kullanır. Kayıt, Azure 'da depolanan bir nesnedir. Kayıt nesnesi, belirli bir Azure Stack için ücret almak için hangi Azure aboneliklerini kullanacağınızı belgeler. Bu bölüm, kaydın önemini ele alınmaktadır.

Kayıt Azure Stack kullanarak şunları yapabilirsiniz:

- Kullanım verilerini Azure ticareti 'ne Azure Stack iletin ve bir Azure aboneliği faturanız.
- Her müşterinin kullanımını çok kiracılı Azure Stack dağıtımıyla farklı bir abonelikte raporla. Çoklu kiracı, Azure Stack aynı Azure Stack örneğindeki farklı kuruluşları desteklemeye olanak sağlar.

Her Azure Stack için bir varsayılan abonelik ve birçok kiracı aboneliği vardır. Varsayılan abonelik, kiracıya özgü abonelik yoksa ücretlendirilen bir Azure aboneliğdir. Bu, kaydedilecek ilk abonelik olmalıdır. Çok kiracılı kullanım raporlaması 'nın çalışması için aboneliğin CSP veya APSS aboneliği olması gerekir.

Ardından kayıt, Azure Stack kullanan her kiracı için bir Azure aboneliğiyle güncelleştirilir. Kiracı aboneliklerinin CSP türünde olması gerekir ve varsayılan aboneliğin sahibi olan iş ortağına toplaması gerekir. Başka birinin müşterilerini kaydedemezsiniz.

Azure Stack kullanım bilgilerini küresel Azure 'a ilettiğinde, Azure 'daki bir hizmet kaydı yönlendirir ve her kiracının kullanımını uygun kiracı aboneliğine eşler. Bir kiracı kaydedilmemişse, bu kullanım kaynaklandığı Azure Stack örneği için varsayılan aboneliğe gider.

Kiracı abonelikleri CSP abonelikleri olduğundan, faturanız CSP iş ortağına gönderilir ve kullanım bilgileri son müşteriye görünmez.

## <a name="next-steps"></a>Sonraki adımlar

- CSP programı hakkında daha fazla bilgi edinmek için bkz. [bulut çözümü sağlayıcısı programı](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Azure Stack kaynak kullanımı bilgilerini alma hakkında daha fazla bilgi için, bkz. [Azure Stack ' de kullanım ve faturalandırma](azure-stack-billing-and-chargeback.md).
