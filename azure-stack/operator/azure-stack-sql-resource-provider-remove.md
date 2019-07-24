---
title: Azure Stack SQL kaynak sağlayıcısı kaldırılıyor | Microsoft Docs
description: Azure Stack dağıtımından SQL kaynak sağlayıcısını nasıl kaldıracağınızı öğrenin.
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
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: e753e4bbbf3dbaac8473667a0422a3ad49329bac
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418023"
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldır

SQL kaynak sağlayıcısını kaldırmadan önce tüm sağlayıcı bağımlılıklarını kaldırmanız gerekir. Ayrıca, kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasına ihtiyacınız olacaktır.

> [!NOTE]
> Kaynak sağlayıcısı [önkoşullarını dağıtma](./azure-stack-sql-resource-provider-deploy.md#prerequisites)' daki kaynak sağlayıcısı yükleyicilerinin indirme bağlantılarını bulabilirsiniz.

SQL kaynak sağlayıcısı 'nı kaldırmak, kiracı veritabanlarını barındırma sunucularından silmez.

## <a name="dependency-cleanup"></a>Bağımlılık Temizleme

Kaynak sağlayıcısını kaldırmak için DeploySqlProvider. ps1 betiğini çalıştırmadan önce yapılacak birkaç temizleme görevi vardır.

Azure Stack Işleci aşağıdaki Temizleme görevlerinden sorumludur:

* SQL bağdaştırıcısına başvuran tüm planları silin.
* SQL bağdaştırıcısıyla ilişkili tüm kotaları silin.

## <a name="to-remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı 'nı kaldırmak için

1. Var olan tüm SQL kaynak sağlayıcısı bağımlılıklarını kaldırdığınızı doğrulayın.

   > [!NOTE]
   > SQL kaynak sağlayıcısı 'nı kaldırmak, bağımlı kaynaklar şu anda kaynak sağlayıcısını kullanıyor olsa bile devam edecektir.
  
2. SQL kaynak sağlayıcısı yükleme paketinin bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklamak için Self-Extractor ' i çalıştırın.

3. Yeni bir yükseltilmiş PowerShell konsol penceresi açın ve SQL kaynak sağlayıcısı yükleme dosyalarını ayıkladığınız dizine geçin.

4. Aşağıdaki parametreleri kullanarak DeploySqlProvider. ps1 betiğini çalıştırın:

    * **Uygulamasını kaldırın**. Kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır.
    * **Privilegedendpoint**. Ayrıcalıklı uç noktanın IP adresi veya DNS adı.
    * **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    * **Cloudadmincredential**. Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri.
    * **Azcredential**. Azure Stack hizmeti yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmetlerini PaaS olarak sunun](azure-stack-app-service-overview.md)
