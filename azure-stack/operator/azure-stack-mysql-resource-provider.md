---
title: MySQL veritabanlarını Azure Stack PaaS olarak kullanma | Microsoft Docs
description: MySQL kaynak sağlayıcısını nasıl dağıtabileceğinizi ve Azure Stack bir hizmet olarak MySQL veritabanları sağlayabileceğinizi öğrenin.
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
ms.openlocfilehash: 7c429fac7a24af3a1488687a79dd822fa4bccd24
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71908956"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack MySQL veritabanlarını kullanma

MySQL veritabanları yaygın olarak Web siteleri ile kullanılır ve birçok Web sitesi platformunu destekler. Örneğin, uygulama Hizmetleri kaynak sağlayıcısı (PaaS) eklentisini kullanarak WordPress web siteleri oluşturabilirsiniz.

Kaynak sağlayıcısını dağıttıktan sonra şunları yapabilirsiniz:

* Azure Resource Manager dağıtım şablonlarını kullanarak MySQL sunucuları ve veritabanları oluşturun.
* MySQL veritabanlarını hizmet olarak sağlayın.  

## <a name="mysql-resource-provider-adapter-architecture"></a>MySQL kaynak sağlayıcısı bağdaştırıcı mimarisi

Kaynak sağlayıcısı aşağıdaki bileşenlere sahiptir:

* Sağlayıcı hizmetlerini çalıştıran bir Windows Server VM 'si olan **MySQL kaynak sağlayıcısı bağdaştırıcısı sanal makinesi (VM)** .
* İstekleri işleyen ve veritabanı kaynaklarına erişen **kaynak sağlayıcısı**.
* Barındırma sunucusu olarak adlandırılan veritabanları için kapasite sağlayan **MySQL Server**'ı barındıran sunucular. MySQL örneklerini kendiniz oluşturabilir veya dış MySQL örneklerine erişim sağlayabilirsiniz. [Azure Stack hızlı başlangıç galerisinde](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) , aşağıdakileri yapmak için kullanabileceğiniz örnek bir şablon vardır:

  * Sizin için bir MySQL sunucusu oluşturun.
  * Azure Marketi 'nden bir MySQL sunucusu indirin ve dağıtın.

> [!NOTE]
> Azure Stack tümleşik sistemlere yüklenen barındırma sunucularının bir kiracı aboneliğinden oluşturulması gerekir. Varsayılan sağlayıcı aboneliğinden oluşturuamazlar. Bunlar, kiracı portalından veya uygun bir oturum açma ile bir PowerShell oturumundan oluşturulmalıdır. Tüm barındırma sunucuları faturalandırılabilir sanal makineler ve lisanslarına sahip olmalıdır. Hizmet Yöneticisi, kiracı Aboneliğinin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

Sistem hesabı aşağıdaki ayrıcalıklara sahip olmalıdır:

* **Veritabanı:** oluştur, bırak
* **Oturum açma:** oluşturma, ayarlama, bırakma, verme, iptal etme  

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısını dağıtma](azure-stack-mysql-resource-provider-deploy.md)
