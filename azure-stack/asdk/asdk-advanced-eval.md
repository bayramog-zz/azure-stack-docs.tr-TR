---
title: Gelişmiş Azure Stack değerlendirme görevleri | Microsoft Docs
description: Bu makalede, Gelişmiş Azure Stack değerlendirme görevleri açıklar.
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/12/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.reviewer: misainat
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 04a1dbb28c55fff15d1087bc85ca65e5dcc7b8c0
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64290324"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Gelişmiş Azure Stack geliştirme Seti'ni değerlendirme görevleri
Temel Azure Stack geliştirme Seti'ni (ASDK) hizmeti özellikler ve yetenekler konusunda öğrendikten sonra daha gelişmiş senaryolarını test etmeye göre daha fazla Azure Stack Anlayışınızı güçlendirmenizi. Bu daha gelişmiş değerlendirme görevleri Azure Stack operatörü belgelerinde belgelenmiştir.

> [!NOTE]
> Tüm kullanım senaryoları, birçok işleci görev ASDK hem de üretim, çok düğümlü Azure Stack dağıtımlar için desteklenirken ASDK dağıtımları için desteklenir. Bkz: [ASDK ve çok düğümlü Azure Stack farklılıkları](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) daha fazla bilgi için.

## <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme
Azure Stack operatörü, genellikle diğer kişilerin sorumlu teklifleri oluşturma ve kullanıcıları imzalama yerleştirmek istediğiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, müşterileri oturum ve sizin adınıza yönetmek için Satıcılar isteyebilirsiniz. Veya merkezi bir BT grubu, bir kuruluşta bir parçası kullanıyorsanız, müdahale olmadan kullanıcılar kuruluşlarının isteyebilirsiniz.

[Azure Stack'te teklifler için temsilci seçme](../operator/azure-stack-delegated-provider.md) erişmek ve doğrudan daha fazla kullanıcı yönetmek mümkün hale getirerek bu görevleri yardımcı olur.

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL veritabanları, Azure Stack kullanıcıları için kullanılabilir yap
Azure Stack operatör, kullanıcılarınızın sağlayan teklifler oluşturabilirsiniz (kiracılar), bulutta çalışan uygulamalar, Web siteleri ve iş yüklerini kullanabileceğiniz SQL veritabanları oluşturun. Bu özel, isteğe bağlı, bulut tabanlı veritabanları kullanıcılarınıza sağlayarak bunları zamandan ve kaynaklardan tasarruf sağlayabilirsiniz.

İçin SQL Server Kaynak sağlayıcısı bağdaştırıcısını kullanmak [SQL veritabanları, Azure Stack kullanıcılarına kullandırmak](../operator/azure-stack-tutorial-sql-server.md) Azure Stack, hizmet olarak. Kaynak sağlayıcısını yükledikten sonra bir veya daha fazla SQL Server örneklerine bağlanabilirsiniz.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web ve API apps, Azure Stack kullanıcılar için kullanılabilir yap
Azure Stack operatör, kullanıcılarınızın sağlayan teklifler oluşturabilirsiniz (kiracılar), Azure işlevleri ve web ve API uygulamaları oluşturun. Kullanıcılarınız için bu isteğe bağlı, bulut tabanlı uygulamalara erişim sağlayarak bunları zamandan ve kaynaklardan tasarruf sağlayabilirsiniz.

App Service kaynak sağlayıcısı için dağıtma [web ve API apps, Azure Stack kullanıcılar için kullanılabilir yap](../operator/azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack tümleşik sistemleri ile hizmetleri sunan hakkında daha fazla bilgi edinin](../operator/azure-stack-offer-services-overview.md)
