---
title: Azure Stack MySQL kaynak sağlayıcısını kaldır | Microsoft Docs
description: Azure Stack dağıtımınızdan MySQL kaynak sağlayıcısını kaldırmayı öğrenin.
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
ms.openlocfilehash: a2b55707bc05aecf8681cb866c58b0ed34fb87cd
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682171"
---
# <a name="remove-the-mysql-resource-provider-in-azure-stack"></a>Azure Stack MySQL kaynak sağlayıcısını kaldır

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
    - **Kaldır**: kaynak sağlayıcıyı ve tüm ilişkili kaynakları kaldırır.
    - **Privilegedendpoint**: ayrıcalıklı uç noktanın IP adresı veya DNS adı.
    - **AzureEnvironment**: Azure Stack dağıtmak Için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    - **Cloudadmincredential**: ayrıcalıklı uç noktaya erişmek için gerekli olan bulut yöneticisinin kimlik bilgileri.
    - **Directorytenantıd**
    - **Azcredential**: Azure Stack hizmeti yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullandığınız kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmetlerini PaaS olarak sunun](azure-stack-app-service-overview.md)
