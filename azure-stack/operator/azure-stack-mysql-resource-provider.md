---
title: PaaS Azure Stack'te MySQL veritabanlarını kullanın | Microsoft Docs
description: MySQL kaynak sağlayıcısı dağıtma ve Azure Stack'te bir hizmet olarak MySQL veritabanları sağlamak nasıl öğrenin.
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
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: e2d2df6c5791b6a05c31950e69fb58b5d48360b2
ms.sourcegitcommit: 104ccafcb72a16ae7e91b154116f3f312321cff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67308495"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack'te MySQL veritabanlarını kullanma

MySQL veritabanları, Web siteleri ile yaygın olarak kullanılır ve birçok Web sitesi platformlarını destekler. Örneğin, uygulama hizmetleri kaynak sağlayıcı (PaaS) eklentisini kullanarak bir WordPress Web siteleri oluşturabilirsiniz.

Kaynak sağlayıcısı dağıttıktan sonra şunları yapabilirsiniz:

* MySQL sunucusu ve Azure Resource Manager dağıtım şablonlarını kullanarak veritabanları oluşturun.
* Hizmet olarak MySQL veritabanı sağlar.  

## <a name="mysql-resource-provider-adapter-architecture"></a>MySQL kaynak sağlayıcısı bağdaştırıcısını mimarisi

Kaynak sağlayıcısı, aşağıdaki bileşenlere sahiptir:

* **MySQL kaynak sağlayıcısı bağdaştırıcısını sanal makine (VM)** , sağlayıcı hizmetlerini çalıştıran bir Windows Server VM olduğu.
* **Kaynak sağlayıcısı**isteklerini işler ve veritabanı kaynaklara erişir.
* **MySQL Server'ı barındıran sunucular**, barındırma sunucuları adı verilen veritabanları için kapasite sağlayın. MySQL örneği kendiniz oluşturmanız veya dış MySQL örneklerini erişim sağlar. [Azure Stack hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) için kullanabileceğiniz bir örnek şablonu vardır:

  * MySQL sunucusu için oluşturun.
  * İndirmek ve bir MySQL sunucusu Azure Market'ten dağıtın.

> [!NOTE]
> Barındıran Azure Stack üzerinde yüklü bir sunucu tarafından sunulan tümleşik sistemler Kiracı abonelikten oluşturulması gerekir. Varsayılan sağlayıcı aboneliği oluşturulamıyor. Bunlar, Kiracı portalı veya bir PowerShell oturumu bir uygun oturum açma ile oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir Vm'leri ve lisansına sahip olması gerekir. Hizmet Yöneticisi, Kiracı aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

Sistem hesabı aşağıdaki ayrıcalıklara sahip olmanız gerekir:

* **Veritabanı:** oluşturma, bırakma
* **Oturum açma:** oluşturmak, ayarlama, bırakın, vermek, iptal etme  

## <a name="next-steps"></a>Sonraki adımlar

[MySQL kaynak sağlayıcısı dağıtma](azure-stack-mysql-resource-provider-deploy.md)
