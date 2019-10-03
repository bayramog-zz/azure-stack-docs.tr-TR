---
title: Azure Stack SQL veritabanlarını kullanma | Microsoft Docs
description: SQL veritabanlarını Azure Stack bir hizmet olarak nasıl dağıtabileceğinizi ve SQL Server kaynak sağlayıcısı bağdaştırıcısını dağıtmaya yönelik hızlı adımları öğrenin.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 210d8e074cd8c0d62567b33b70cd75984f72d149
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829291"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack SQL veritabanlarını kullanma

SQL veritabanlarını [Azure Stack](azure-stack-overview.md)bir hizmet olarak sunmak için SQL Server kaynak sağlayıcısını kullanın. Kaynak sağlayıcısını yükledikten ve bir veya daha fazla SQL Server örneğine bağladığınızda, siz ve kullanıcılarınızın şunları oluşturaktarabilirsiniz:

- Bulutta yerel uygulamalar için veritabanları.
- SQL kullanan Web siteleri.
- SQL kullanan iş yükleri.

Kaynak sağlayıcı, [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/)'nın tüm veritabanı yönetim yeteneklerini sağlamıyor. Örneğin, kaynakları otomatik olarak ayıran esnek havuzlar desteklenmez. Ancak, kaynak sağlayıcısı SQL Server veritabanında benzer oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini destekler. 

## <a name="sql-resource-provider-adapter-architecture"></a>SQL kaynak sağlayıcısı bağdaştırıcı mimarisi

Kaynak sağlayıcı aşağıdaki bileşenlerden oluşur:

- Sağlayıcı hizmetlerini çalıştıran bir Windows Server VM 'si olan **SQL kaynak sağlayıcısı bağdaştırıcısı sanal makinesi (VM)** .
- İstekleri işleyen ve veritabanı kaynaklarına erişen **kaynak sağlayıcısı**.
- Barındırma sunucuları adlı veritabanları için kapasite sağlayan **SQL Server barındıran sunucular**.

SQL Server en az bir örneğini oluşturmanız veya dış SQL Server örneklerine erişim sağlamanız gerekir.

> [!NOTE]
> Azure Stack tümleşik sistemlere yüklenen barındırma sunucularının bir kiracı aboneliğinden oluşturulması gerekir. Varsayılan sağlayıcı aboneliğinden oluşturuamazlar. Bunlar, kiracı portalından oluşturulmalıdır veya uygun oturum açma ile PowerShell kullanılarak kullanılmalıdır. Tüm barındırma sunucuları faturalandırılabilir sanal makinelerdir ve lisanslarına sahip olmalıdır. Hizmet Yöneticisi, kiracı Aboneliğinin sahibi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server kaynak sağlayıcısını dağıtma](azure-stack-sql-resource-provider-deploy.md)
