---
title: Kullanım verilerini Azure 'a rapor Azure Stack | Microsoft Docs
description: Azure Stack ' de kullanım verilerini raporlamayı ayarlamayı öğrenin.
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
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 3aeae5c1a0106a0c13c9b6bbe2eb4ba07de14dd4
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959450"
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Kullanım verilerini Azure 'a rapor Azure Stack

Tüketim verileri olarak da bilinen kullanım verileri, kullanılan kaynak miktarını temsil eder.

Tüketim tabanlı faturalandırma modelini kullanan çok düğümlü sistemler Azure Stack faturalandırma amaçlarıyla kullanım verilerini Azure 'a bildirmelidir. Azure Stack işleçleri kullanım verilerini Azure 'a bildirmek için Azure Stack örneklerini yapılandırmalıdır.

> [!IMPORTANT]
> Azure Stack lisanslama koşullarına uymak için, tüm iş [yüklerinin kiracı abonelikleri altında dağıtılması gerekir](#are-users-charged-for-the-infrastructure-vms) .

Kullanım verilerini raporlama, Kullandıkça Öde modeli kapsamında lisans veren çok düğümlü kullanıcıların Azure Stack için gereklidir. Kapasite modeli kapsamında lisans veren müşteriler için isteğe bağlıdır (bkz. sayfa [satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/) ). Azure Stack Geliştirme Seti (ASDK) kullanıcıları için, Azure Stack işleçleri kullanım verilerini bildirebilir ve özelliği test edebilir. Bununla birlikte, kullanıcılar, istedikleri kullanım için ücretlendirilmeyecektir.

![Faturalandırma akışı](media/azure-stack-usage-reporting/billing-flow.png)

Kullanım verileri, Azure Bridge üzerinden Azure 'a Azure Stack gönderilir. Azure 'da, ticari sistem kullanım verilerini işler ve faturayı oluşturur. Fatura oluşturulduktan sonra, Azure aboneliği sahibi [Azure Hesap Merkezi](https://account.windowsazure.com/subscriptions)görüntüleyip indirebilir. Azure Stack nasıl lisanslandığını öğrenmek için, [Azure Stack paketleme ve fiyatlandırma belgesine](https://go.microsoft.com/fwlink/?LinkId=842847)bakın.

## <a name="set-up-usage-data-reporting"></a>Kullanım verilerini raporlamayı ayarlama

Kullanım verilerini raporlama 'yı ayarlamak için [Azure Stack örneğinizi Azure ile kaydetmeniz](azure-stack-registration.md)gerekir. Kayıt sürecinin bir parçası olarak, Azure Stack Azure 'a bağlayan ve kullanım verilerini gönderen Azure Stack Azure Bridge bileşeni yapılandırılır. Aşağıdaki kullanım verileri Azure Stack 'ten Azure 'a gönderilir:

- **Ölçüm kimliği** -tüketilen kaynak IÇIN benzersiz kimlik.
- **Miktar** -kaynak kullanımı miktarı.
- **Konum** -geçerli Azure Stack kaynağının dağıtıldığı konum.
- **Kaynak URI 'si** -kullanımın bildirildiği kaynağın tam nitelikli URI 'si.
- **ABONELIK kimliği** -yerel (Azure Stack) aboneliği olan Azure Stack kullanıcının abonelik kimliği.
- Kullanım verilerinin **zaman** başlangıç ve bitiş zamanı. Bu kaynakların Azure Stack tüketilen zaman ve kullanım verilerinin ticaret 'e bildirildiği zaman arasında bir gecikme vardır. Azure Stack, her 24 saatte kullanım verilerini toplar ve Azure 'daki ticari işlem hattına raporlama kullanım verilerini birkaç saat sürer. Bu nedenle, gece yarısından önce gerçekleşen kullanım, Azure 'da aşağıdaki gün görünebilir.

## <a name="generate-usage-data-reporting"></a>Kullanım verilerini raporlama oluşturma

- Kullanım verilerini raporlamayı test etmek için Azure Stack birkaç kaynak oluşturun. Örneğin, temel kullanımın nasıl raporlanacağı hakkında bilgi almak için temel ve standart SKU 'Lar içeren bir [depolama hesabı](azure-stack-provision-storage-account.md), [Windows Server VM](../user/azure-stack-create-vm-template.md)'si ve bir Linux sanal makinesi oluşturabilirsiniz. Farklı kaynak türleri için kullanım verileri farklı ölçümler altında raporlanır.

- Kaynaklarınızı birkaç saat boyunca çalışır durumda bırakın. Kullanım bilgileri her saatte yaklaşık olarak toplanır. Toplandıktan sonra, bu veriler Azure 'a iletilir ve Azure ticaret sistemine işlenir. Bu işlem birkaç saat sürebilir.

## <a name="view-usage---csp-subscriptions"></a>Kullanım-CSP aboneliklerini görüntüleme

Azure Stack CSP aboneliği kullanarak kaydolduysanız, kullanımınızı ve ücretlerinizi Azure 'un tüketimini görüntülediğiniz şekilde görüntüleyebilirsiniz. Azure Stack kullanımı, [Iş Ortağı Merkezi](https://partnercenter.microsoft.com/partner/home)aracılığıyla faturanızda ve mutabakat dosyasında bulunur. Mutabakat dosyası aylık olarak güncelleştirilir. Son Azure Stack kullanım bilgilerine erişmeniz gerekiyorsa, Iş Ortağı Merkezi API 'Lerini kullanabilirsiniz.

![İş Ortağı Merkezi](media/azure-stack-usage-reporting/partner-center.png)

## <a name="view-usage---enterprise-agreement-subscriptions"></a>Kullanım-Kurumsal Anlaşma aboneliklerini görüntüleme

Azure Stack bir Kurumsal Anlaşma aboneliği kullanarak kaydolduysanız, kullanım ve ücretlerinizi [EA portalında](https://ea.azure.com/)görüntüleyebilirsiniz. Azure Stack kullanımı, bu portalın Raporlar bölümünde Azure kullanımı ile birlikte gelişmiş indirmelere dahildir.

## <a name="view-usage---other-subscriptions"></a>Kullanımı görüntüleme-diğer abonelikler

Azure Stack başka bir abonelik türünü kullanarak kaydolduysanız; Örneğin, Kullandıkça Öde aboneliği, Azure Hesap Merkezi kullanımı ve ücretleri görüntüleyebilirsiniz. [Azure Hesap Merkezi](https://account.windowsazure.com/subscriptions) Azure Hesap Yöneticisi olarak oturum açın ve Azure Stack kaydettirmek Için kullandığınız Azure aboneliğini seçin. Aşağıdaki görüntüde gösterildiği gibi, kullanılan kaynakların her biri için ücretlendirilen miktarı Azure Stack kullanım verilerini görüntüleyebilirsiniz:

![Faturalandırma akışı](media/azure-stack-usage-reporting/pricing-details.png)

ASDK için Azure Stack kaynaklar ücretlendirilmez, bu nedenle gösterilen fiyat $0,00 ' dir.

## <a name="which-azure-stack-deployments-are-charged"></a>Hangi Azure Stack dağıtımları ücretlendirilir?

Kaynak kullanımı, ASDK için ücretsizdir. Çok düğümlü sistemler, iş yükü VM 'Leri, depolama hizmetleri ve uygulama hizmetleri Azure Stack ücretlendirilir.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Kullanıcılar altyapı VM 'Leri için ücretlendirilir mi?

Hayır. Bazı Azure Stack kaynak sağlayıcısı sanal makineleri için kullanım verileri Azure 'a bildirilir, ancak bu VM 'Ler için ve dağıtım sırasında oluşturulan VM 'Ler için Azure Stack altyapısını etkinleştirmek üzere bir ücret alınmaz.  

Kullanıcılar yalnızca kiracı abonelikleri altında çalışan VM 'Ler için ücretlendirilir. Azure Stack lisanslama koşullarına uymak için, tüm iş yüklerinin kiracı abonelikleri altında dağıtılması gerekir.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Azure Stack için kullanmak istediğim bir Windows Server lisansına sahip olduğumu nasıl yapabilirim?

Mevcut lisansların kullanılması kullanım ölçümlerini oluşturmayı önler. Mevcut Windows Server lisansları, [Azure Stack lisanslama kılavuzunun](https://go.microsoft.com/fwlink/?LinkId=851536)"Azure Stack ile mevcut yazılımları kullanma" bölümünde açıklandığı gibi Azure Stack kullanılabilir. Müşterilerinin mevcut lisanslarını kullanabilmesi için, müşterilerin Windows Server sanal makinelerini [Windows Server lisansına yönelik karma avantaj](/azure/virtual-machines/windows/hybrid-use-benefit-licensing)bölümünde açıklandığı gibi dağıtması gerekir.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Tüketilen kaynaklar için hangi abonelik ücretlendirilir?

[Azure Stack Azure ile kaydolurken](azure-stack-registration.md) sağlanmış olan abonelik ücretlendirilir.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Kullanım verilerini raporlama için hangi tür abonelikler desteklenir?

Azure Stack çok düğümlü, Kurumsal Anlaşma (EA) ve CSP abonelikleri desteklenir. Azure Stack Geliştirme Seti, Kurumsal Anlaşma (EA), Kullandıkça öde, CSP ve MSDN abonelikleri için kullanım verilerini raporlama desteği.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Kullanım verilerini raporlama, bağımsız bulutlarda çalışıyor mu?

Kullanım verilerini raporlama Azure Stack Geliştirme Seti, genel Azure sisteminde oluşturulan abonelikleri gerektirir. Bağımsız bulutlardan birinde oluşturulan abonelikler (Azure Kamu, Azure Almanya ve Azure Çin bulutları) Azure 'a kaydedilemez, bu nedenle kullanım verilerini raporlamayı desteklemezler.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Azure Stack içinde bildirilen kullanım, Azure Hesap Merkezi oluşturulan raporla eşleşmiyor mu?

Azure Stack kullanım API 'Leri tarafından raporlanan kullanım verileri ve Azure Hesap Merkezi bildirilen kullanım verileri arasında her zaman bir gecikme vardır. Bu gecikme, kullanım verilerini Azure Stack 'tan Azure ticareti 'na yüklemek için gereken süredir. Bu gecikme nedeniyle, gece yarısından önce gerçekleşen kullanım şu gün Azure 'da görünebilir. [Azure Stack kullanım API 'lerini](azure-stack-provider-resource-api.md)kullanır ve sonuçları Azure Faturalandırma portalı 'nda raporlanan kullanımla karşılaştırırsanız, bir farklılık görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Sağlayıcı kullanım API’si](azure-stack-provider-resource-api.md)  
- [Kiracı kullanım API’si](azure-stack-tenant-resource-usage-api.md)
- [Kullanım Hakkında SSS](azure-stack-usage-related-faq.md)
- [Kullanım ve faturalandırma hizmetini bir bulut hizmeti sağlayıcısı olarak yönetme](azure-stack-add-manage-billing-as-a-csp.md)
