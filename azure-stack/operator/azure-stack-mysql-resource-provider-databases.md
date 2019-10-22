---
title: Azure Stack MySQL veritabanları oluştur | Microsoft Docs
description: Azure Stack ' de MySQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan MySQL veritabanlarını oluşturmayı ve yönetmeyi öğrenin.
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
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 9daaea395ee28da79da1d6fd553d2e0b30e28156
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682243"
---
# <a name="create-mysql-databases-in-azure-stack"></a>Azure Stack MySQL veritabanları oluşturma
MySQL veritabanı hizmetini içeren bir teklifine abone olan Azure Stack Kullanıcı, self servis MySQL veritabanlarını Kullanıcı portalında oluşturup yönetebilir.

## <a name="create-a-mysql-database"></a>MySQL veritabanı oluşturma

1. Azure Stack Kullanıcı portalında oturum açın.
2. **+ Kaynak oluştur** > **veri + depolama** > **MySQL veritabanı** > **Ekle**' yi seçin.
3. **MySQL veritabanı oluştur**altında, veritabanı adını girin ve ortamınız için gereken diğer ayarları yapılandırın.

    ![Test MySQL veritabanı oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. **Veritabanı oluştur**altında **SKU**' yı seçin. **MySQL SKU 'Su seçin**altında VERITABANıNıZ için SKU 'yu seçin.

    ![MySQL SKU 'SU seçin](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >Barındırma sunucuları Azure Stack eklendikçe, bunlara bir SKU atanır. Veritabanları, bir SKU 'daki barındırma sunucuları havuzunda oluşturulur.

5. **Oturum aç**altında, ***gerekli ayarları Yapılandır***' ı seçin.
6. **Oturum aç**' ın altında, mevcut bir oturum açma veya + yeni bir oturum açma **Oluştur** ' u seçerek yeni bir oturum açmayı seçebilirsiniz.  Bir **veritabanı oturum açma** adı ve **parolası**girip **Tamam**' ı seçin.

    ![Yeni bir veritabanı oturumu oluştur](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >Veritabanı oturum açma adının uzunluğu MySQL 5,7 ' de 32 karakteri aşamaz. Önceki sürümlerde, 16 karakterden uzun olamaz.

7. Veritabanının kurulumunu yapmak için **Oluştur** ' u seçin.

Veritabanı dağıtıldıktan sonra, **temel parçalar**altındaki **bağlantı dizesini** göz önünde atın. Bu dizeyi, MySQL veritabanına erişmesi gereken herhangi bir uygulamada kullanabilirsiniz.

![MySQL veritabanı için bağlantı dizesini al](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Yönetici parolasını güncelleştirme

Parolayı MySQL Server örneğinde değiştirerek değiştirebilirsiniz.

1. **MySQL barındırma sunucuları** >  **Yönetim kaynakları** ' nı seçin. Barındırma sunucusunu seçin.
2. **Ayarlar**altında, **parola**' yı seçin.
3. **Parola**alanına yeni parolayı girin ve ardından **Kaydet**' i seçin.

![Yönetici parolasını güncelleştirme](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir MySQL veritabanları sunmayı](azure-stack-tutorial-mysql.md)öğrenin.
