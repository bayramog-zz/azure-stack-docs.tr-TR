---
title: SQL veritabanları, Azure Stack kullanıcılarına kullandırmak | Microsoft Docs
description: SQL Server Kaynak sağlayıcısını yüklemek ve oluşturmak için öğretici, Azure Stack kullanıcıları SQL veritabanı oluşturma sağlayan sunar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: mabrigg
ms.reviewer: quying
ms.lastreviewed: 11/05/2018
ms.custom: mvc
ms.openlocfilehash: e65bebf5b6126455ef1294fabeb1450f91689d5d
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65618373"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Öğretici: SQL veritabanları, Azure Stack kullanıcılar için kullanılabilir yap

Azure Stack bulut yönetici olarak, kullanıcılarınızın sağlayan teklifler oluşturabilirsiniz (kiracılar), bulutta çalışan uygulamalar, Web siteleri ve iş yüklerini kullanabileceğiniz SQL veritabanları oluşturun. Bu özel, isteğe bağlı, bulut tabanlı veritabanları kullanıcılarınıza sağlayarak bunları zamandan ve kaynaklardan tasarruf sağlayabilirsiniz. Bunu ayarlamak için yapacaklarınız:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Test teklifini

## <a name="deploy-the-sql-server-resource-provider"></a>SQL Server Kaynak sağlayıcısı dağıtma

Dağıtım işlemi ayrıntılı olarak açıklanan [Azure Stack makale üzerinde SQL veritabanlarını kullanmayı](azure-stack-sql-resource-provider-deploy.md)ve birincil aşağıdaki adımlardan oluşur:

1. [SQL kaynak sağlayıcısı dağıtma](azure-stack-sql-resource-provider-deploy.md).
2. [Dağıtımı doğrulama](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Bir barındırma SQL sunucusuna bağlanarak kapasitesi sağlar. Daha fazla bilgi için [barındırma sunucuları ekleme](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Teklif oluşturma

1.  [Kota ayarlamak](azure-stack-plan-offer-quota-overview.md ) ve adlandırın *SQLServerQuota*. Seçin **Microsoft.SQLAdapter** için **Namespace** alan.
2.  [Bir plan oluşturmanız](azure-stack-create-plan.md). Adlandırın *TestSQLServerPlan*seçin **Microsoft.SQLAdapter** hizmeti ve **SQLServerQuota** kota.

    > [!NOTE]
    > Kullanıcıların diğer uygulamalar için diğer hizmetleri planında gerekli olabilir. Örneğin, Azure işlevleri'ni gerektirir **Microsoft.Storage** Wordpress gerekirken hizmet planda **Microsoft.MySQLAdapter**.

3.  [Teklif oluşturma](azure-stack-create-offer.md), adlandırın **TestSQLServerOffer** seçip **TestSQLServerPlan** planı.

## <a name="test-the-offer"></a>Test teklifini

SQL Server Kaynak sağlayıcısı dağıtılan ve teklif oluşturduğunuza göre bir kullanıcı olarak oturum açın, teklife abone olma ve bir veritabanı oluşturun.

### <a name="subscribe-to-the-offer"></a>Teklife abone olma

1. Azure Stack portalında oturum açın (https://portal.local.azurestack.external) Kiracı olarak.
2. Seçin **bir abonelik edinmeniz** ve enter **TestSQLServerSubscription** altında **görünen ad**.
3. Seçin **bir teklif seçin** > **TestSQLServerOffer** > **Oluştur**.
4. Seçin **tüm hizmetleri** > **abonelikleri** > **TestSQLServerSubscription** > **kaynak sağlayıcıları**.
5. Seçin **kaydetme** yanındaki **Microsoft.SQLAdapter** sağlayıcısı.

### <a name="create-a-sql-database"></a>SQL veritabanı oluşturun

1. Seçin **+**  >  **veri + depolama** > **SQL veritabanı**.
2. Varsayılan değerleri tutabilir veya bu örnekler için aşağıdaki alanları kullanın:
    - **Veritabanı adı**: SQLdb
    - **MB cinsinden en büyük boyutu**: 100
    - **Abonelik**: TestSQLOffer
    - **Kaynak grubu**: SQL-RG
3. Seçin **oturum açma ayarları**veritabanı için kimlik bilgilerini girin ve ardından **Tamam**.
4. Seçin **SKU** > SQL barındıran SQL Server için oluşturduğunuz SKU'ları seçin > ve ardından **Tamam**.
5. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * SQL Server Kaynak sağlayıcısı dağıtma
> * Teklif oluşturma
> * Test teklifini

Bilgi edinmek için sonraki öğreticiye ilerleyin nasıl yapılır:

> [!div class="nextstepaction"]
> [Web, mobil ve API apps, kullanıcılar için kullanılabilir yap]( azure-stack-tutorial-app-service.md)