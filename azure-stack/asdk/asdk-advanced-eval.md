---
title: Gelişmiş ASDK değerlendirme görevleri | Microsoft Docs
description: Gelişmiş Azure Stack geliştirme seti (ASDK) değerlendirme görevleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 2390f2c21325a059ea6f651e7fcd944d890bf307
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73594779"
---
# <a name="advanced-asdk-evaluation-tasks"></a>Gelişmiş ASDK değerlendirme görevleri
Temel Azure Stack Geliştirme Seti (ASDK) hizmet özellikleri ve özellikleriyle benzerlik kazandıktan sonra, daha gelişmiş senaryolar sunarak Azure Stack daha fazla bilgi sahibi olabilirsiniz. Bu daha gelişmiş değerlendirme görevleri, Azure Stack Işleci belgelerinde tam olarak belgelenmiştir.

> [!NOTE]
> Hem ASDK hem de üretim ve çok düğümlü Azure Stack dağıtımları için birçok işleç görevi desteklenirken, tüm kullanım senaryoları ASDK dağıtımları için desteklenmez. Daha fazla bilgi için bkz. [asdk ve çok düğümlü Azure Stack farkları](asdk-what-is.md#asdk-and-multi-node-azure-stack-hub-differences).

## <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme
Azure Stack operatörü olarak genellikle diğer kişileri teklif oluşturma ve Kullanıcı imzalama ücretine koymak istersiniz. Örneğin, bir hizmet sağlayıcısıysanız, satıcıların müşterilere kaydolmasını ve bunları sizin adınıza yönetmesini isteyebilirsiniz. Ya da bir kuruluştaki merkezi bir BT grubunun parçasıysa, bağlı kuruluşlarının kullanıcıları müdahale etmeden imzalamasını isteyebilirsiniz.

[Azure Stack teklifleri temsilci seçme](../operator/azure-stack-delegated-provider.md) , doğrudan sizin tarafınızdan daha fazla kullanıcıyı daha fazla kullanıcıya ulaşarak bu görevlerde size yardımcı olur.

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL veritabanlarını Azure Stack kullanıcılarınız için kullanılabilir hale getirme
Azure Stack operatörü olarak, kullanıcılarınızın (kiracılar) bulutta yerel uygulamalar, Web siteleri ve iş yükleriyle kullanabilecekleri SQL veritabanları oluşturmalarına olanak sağlayan teklifler oluşturabilirsiniz. Kullanıcılarınıza bu özel, isteğe bağlı, bulut tabanlı veritabanlarını sağlayarak zaman ve kaynak tasarrufu sağlayabilirsiniz.

SQL veritabanlarını bir Azure Stack hizmeti olarak [Azure Stack kullanıcılarınız için kullanılabilir hale getirmek](../operator/azure-stack-tutorial-sql-server.md) üzere SQL Server kaynak sağlayıcısı bağdaştırıcısını kullanın. Kaynak sağlayıcısını yükledikten sonra, bir veya daha fazla SQL Server örneğine bağlanırsınız.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web ve API uygulamalarını Azure Stack kullanıcılarınız için kullanılabilir hale getirme
Azure Stack operatörü olarak, kullanıcılarınızın (kiracılar) Azure Işlevleri ve Web ve API Apps oluşturmalarına olanak sağlayan teklifler oluşturabilirsiniz. Kullanıcılarınıza bu isteğe bağlı ve bulut tabanlı uygulamalara erişim sağlayarak bunları ve kaynaklarını kaydedebilirsiniz.

[Web ve API uygulamalarını Azure Stack kullanıcılarınız için kullanılabilir hale getirmek](../operator/azure-stack-tutorial-app-service.md)için App Service kaynak sağlayıcısını dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack tümleşik sistemlerle hizmet sunma hakkında daha fazla bilgi edinin](../operator/service-plan-offer-subscription-overview.md)
