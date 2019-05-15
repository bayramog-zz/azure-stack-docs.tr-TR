---
title: Azure Stack'te SQL kaynak sağlayıcısı kaldırılıyor | Microsoft Docs
description: Bilgi nasıl, SQL kaynak sağlayıcısı, Azure Stack dağıtımdan kaldırın.
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
ms.date: 11/20/2018
ms.author: mabrigg
ms.reviewer: quying
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: d7b512315eb1631d37ed51a2dcb83015fad197be
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65618419"
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldırma

SQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcısı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

> [!NOTE]
> Kaynak sağlayıcısı yükleyiciler, indirme bağlantıları bulabilirsiniz [kaynak sağlayıcı önkoşulları dağıtma](./azure-stack-sql-resource-provider-deploy.md#prerequisites).

SQL kaynak sağlayıcısı kaldırıldığında, Kiracı veritabanlarını barındıran sunucu silinmez.

## <a name="dependency-cleanup"></a>Bağımlılık temizleme

Kaynak Sağlayıcısı'nı kaldırmak için DeploySqlProvider.ps1 betik çalıştırılmadan önce yapmak için birkaç temizleme görevi vardır.

Azure Stack operatörü için aşağıdaki temizleme görevlerini sorumludur:

* SQL bağdaştırıcı başvuru planları silin.
* SQL bağdaştırıcısı ile ilişkili olan herhangi bir kota silin.

## <a name="to-remove-the-sql-resource-provider"></a>SQL kaynak Sağlayıcısı'nı kaldırmak için

1. Tüm mevcut SQL kaynak sağlayıcısı bağımlılıkları kaldırmış olduğunuz doğrulayın.

   > [!NOTE]
   > Bağımlı kaynaklar halen kaynak sağlayıcısı kullanıyor olsanız bile, SQL kaynak sağlayıcısı kaldırma devam edecek.
  
2. SQL kaynak sağlayıcısı yükleme paketinin bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

3. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve SQL kaynak sağlayıcısı yükleme dosyaları ayıkladığınız dizine geçin.

4. Aşağıdaki parametreleri kullanarak DeploySqlProvider.ps1 betiği çalıştırın:

    * **Kaldırma**. Kaynak sağlayıcısı ile ilişkili tüm kaynakları kaldırır.
    * **PrivilegedEndpoint**. Ayrıcalıklı uç noktasının DNS adı veya IP adresi.
    * **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    * **CloudAdminCredential**. Ayrıcalıklı uç noktasına erişmek gerekli bulut Yöneticisi kimlik bilgileri.
    * **AzCredential**. Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[PaaS olarak App Services teklifi](azure-stack-app-service-overview.md)
