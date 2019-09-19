---
title: Müşteri faturalandırma ve geri ödeme Azure Stack | Microsoft Docs
description: Azure Stack kullanıcıların kaynak kullanımı için nasıl faturalandırıldığını ve analiz ve geri ödeme için faturalandırma bilgilerine nasıl erişildiğini öğrenin.
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
ms.date: 06/21/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: bcdf43f7be95c76cfd4fc454d85e08ad197551a6
ms.sourcegitcommit: c196463492732218d2474d3a964f88e995272c80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71094332"
---
# <a name="usage-and-billing-in-azure-stack"></a>Azure Stack kullanım ve faturalandırma

Bu makalede, Azure Stack kullanıcıların kaynak kullanımı için nasıl faturalandırıldığını ve analiz ve geri ödeme için faturalandırma bilgilerine nasıl erişildiği açıklanmaktadır.

Azure Stack kullanılan kaynaklar için kullanım verilerini toplayıp gruplandırır ve bu verileri Azure ticareti 'ne iletir. Azure ticareti, Azure Stack kullanımı için sizi Azure kullanımı ile aynı şekilde faturalandırır.

Ayrıca, bir faturalandırma bağdaştırıcısı kullanarak kullanım verilerini alabilir ve kendi faturalandırma veya geri ödeme sisteminize aktarabilir ya da Microsoft Power BI gibi bir iş zekası aracına dışarı aktarabilirsiniz.

## <a name="usage-pipeline"></a>Kullanım işlem hattı

Azure Stack içindeki her kaynak sağlayıcısı, kaynak kullanımı başına kullanım verilerini gönderir. Kullanım hizmeti düzenli aralıklarla (saatlik ve günlük) kullanım verilerini toplar ve kullanım veritabanında depolar. Azure Stack işleçler ve kullanıcılar, Azure Stack kaynak kullanımı API 'Leri aracılığıyla depolanan kullanım verilerine erişebilirler.

[Azure Stack örneğinizi Azure ile kaydolduysanız](azure-stack-registration.md), Azure Stack kullanım verilerini Azure ticareti 'ne gönderecek şekilde yapılandırılır. Veriler Azure 'a yüklendikten sonra faturalandırma portalı aracılığıyla veya Azure Kaynak kullanımı API 'Leri kullanarak erişebilirsiniz. Azure 'a hangi kullanım verilerinin bildirildiği hakkında daha fazla bilgi için bkz. [kullanım verilerini raporlama](azure-stack-usage-reporting.md).  

Aşağıdaki görüntüde, kullanım ardışık düzeninde anahtar bileşenleri gösterilmektedir:

![Kullanım işlem hattı](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Hangi kullanım bilgilerini bulabilirim ve nasıl?

Azure Stack kaynak sağlayıcıları (Işlem, depolama ve ağ), her abonelik için saatlik aralıklarla kullanım verileri oluşturur. Kullanım verileri, kaynak adı, kullanılan abonelik ve kullanılan miktar gibi kullanılan kaynak hakkındaki bilgileri içerir. Ölçüm KIMLIK kaynakları hakkında bilgi edinmek için bkz. [kullanım API 'SI SSS](azure-stack-usage-related-faq.md).

Kullanım verileri toplandıktan sonra, Azure Faturalandırma portalı aracılığıyla görüntülenebilen bir fatura oluşturmak için [Azure 'a bildirilir](azure-stack-usage-reporting.md) .

> [!NOTE]  
> Kullanım verilerini raporlama Azure Stack Geliştirme Seti (ASDK) ve kapasite modeli kapsamında lisans veren Azure Stack tümleşik sistem kullanıcıları için gerekli değildir. Azure Stack ' de lisanslama hakkında daha fazla bilgi edinmek için [paketleme ve fiyatlandırma veri sayfasına](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf)bakın.

Azure Faturalandırma portalı, Borçlandırılabilir kaynaklar için kullanım verilerini gösterir. Borçlandırılabilir kaynaklara ek olarak, Azure Stack REST API 'leri veya PowerShell cmdlet 'leri aracılığıyla Azure Stack ortamınıza erişebileceğiniz daha geniş bir kaynak kümesi için kullanım verilerini yakalar. Azure Stack işleçleri tüm Kullanıcı abonelikleri için kullanım verilerini alabilir. Bireysel kullanıcılar yalnızca kendi kullanım ayrıntılarını alabilir.

## <a name="usage-reporting-for-multi-tenant-cloud-service-providers"></a>Çok kiracılı bulut hizmeti sağlayıcıları için kullanım raporlaması

Azure Stack kullanan çok kiracılı bir bulut çözümü sağlayıcısı (CSP) her müşteri kullanımını ayrı ayrı raporlamak isteyebilir, böylece sağlayıcı farklı Azure aboneliklerine kullanım ücreti verebilir.

Her müşterinin kimliği, farklı bir Azure Active Directory (Azure AD) kiracısı tarafından temsil edilir. Azure Stack, her bir Azure AD kiracısına bir CSP aboneliği atanmasını destekler. Kiracıların ve aboneliklerinin temelini temel Azure Stack kaydına ekleyebilirsiniz. Temel kayıt tüm Azure Stack örnekleri için yapılır. Bir abonelik bir kiracı için kayıtlı değilse, Kullanıcı Azure Stack kullanmaya devam edebilir ve bu kullanıcıların kullanımları temel kayıt için kullanılan aboneliğe gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Kaydet](azure-stack-registration.md)
- [Azure Stack kullanım verilerini Azure'a bildirme](azure-stack-usage-reporting.md)
- [Sağlayıcı kaynağı kullanım API 'SI](azure-stack-provider-resource-api.md)
- [Kiracı Kaynak Kullanım API’si](azure-stack-tenant-resource-usage-api.md)
- [Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)
