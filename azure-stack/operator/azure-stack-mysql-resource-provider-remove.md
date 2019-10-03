---
title: Azure Stack MySQL kaynak sağlayıcısı kaldırılıyor | Microsoft Docs
description: Azure Stack dağıtımınızdan MySQL kaynak sağlayıcısını nasıl kaldırabileceğinizi öğrenin.
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
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: 3a48ded6a9a21650bae488e0a858086a3575235e
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829397"
---
# <a name="remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldır

MySQL kaynak sağlayıcısını kaldırmadan önce tüm sağlayıcı bağımlılıklarını kaldırmanız gerekir. Ayrıca, kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasına ihtiyacınız olacaktır.

> [!NOTE]
> Kaynak sağlayıcısı [önkoşullarını dağıtma](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)' daki kaynak sağlayıcısı yükleyicilerinin indirme bağlantılarını bulabilirsiniz.

MySQL kaynak sağlayıcısı 'nın kaldırılması, kiracı veritabanlarını barındırma sunucularından silmez.

## <a name="dependency-cleanup"></a>Bağımlılık Temizleme

Kaynak sağlayıcıyı kaldırmak için DeployMySqlProvider. ps1 betiğini çalıştırmadan önce yapılacak birkaç temizleme görevi vardır.

Azure Stack Işleci aşağıdaki Temizleme görevlerinden sorumludur:

* MySQL bağdaştırıcısına başvuran tüm planları silin.
* MySQL bağdaştırıcısıyla ilişkili tüm kotaları silin.

## <a name="to-remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldırmak için

1. Var olan tüm MySQL kaynak sağlayıcısı bağımlılıklarını kaldırdığınızı doğrulayın.

   > [!NOTE]
   > MySQL kaynak sağlayıcısını kaldırmak, bağımlı kaynaklar şu anda kaynak sağlayıcısını kullanıyor olsa bile devam edecektir.
  
2. MySQL kaynak sağlayıcısı yükleme paketinin bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklamak için Self-Extractor ' i çalıştırın.
3. Yeni bir yükseltilmiş PowerShell konsol penceresi açın ve MySQL kaynak sağlayıcısı yükleme dosyalarını ayıkladığınız dizine geçin.
4. Aşağıdaki parametreleri kullanarak DeployMySqlProvider. ps1 betiğini çalıştırın:
    - **Uygulamasını kaldırın**. Kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır.
    - **Privilegedendpoint**. Ayrıcalıklı uç noktanın IP adresi veya DNS adı.
    - **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    - **Cloudadmincredential**. Ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri.
    - **Directorytenantıd**
    - **Azcredential**. Azure Stack hizmeti yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmetlerini PaaS olarak sunun](azure-stack-app-service-overview.md)
