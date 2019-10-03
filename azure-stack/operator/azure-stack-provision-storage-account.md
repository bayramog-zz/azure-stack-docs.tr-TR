---
title: Azure Stack 'de depolama hesapları | Microsoft Docs
description: Azure Stack depolama hesabı oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/2/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 60336477f1dec9618fd6cc439e9d2f5d098b3399
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829395"
---
# <a name="storage-accounts-in-azure-stack"></a>Azure Stack'teki depolama hesapları

Azure Stack'te depolama hesapları, Blob ve Tablo hizmetlerinin yanı sıra depolama verisi nesneleriniz için benzersiz ad alanı içerir. Varsayılan olarak hesabınızdaki veriler, depolama hesabı sahibi olarak sizin kullanımınıza sunulur.

1. Azure Stack POC bilgisayarında, [yönetici](../asdk/asdk-connect.md)olarak `https://adminportal.local.azurestack.external` ' da oturum açın ve **+ kaynak oluştur** > **veri + depolama** > **depolama hesabı**' na tıklayın.

   ![Depolama hesabınızı oluşturun](media/azure-stack-provision-storage-account/image01.png)
2. **Depolama hesabı oluştur** dikey penceresinde, depolama hesabınız için bir ad yazın. Yeni bir kaynak grubu oluşturun veya var olan bir **kaynak grubunu**seçin ve ardından **Oluştur** ' a tıklayarak depolama hesabı oluşturun.

   ![Depolama hesabınızı gözden geçirin](media/azure-stack-provision-storage-account/image02.png)
3. Yeni depolama hesabınızı görmek için **tüm kaynaklar**' a tıklayın, ardından Depolama hesabını arayın ve adına tıklayın.

    ![Depolama hesabınızın adı](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Sonraki adımlar
[Azure Resource Manager şablonlarını kullanma](../user/azure-stack-arm-templates.md)

[Azure depolama hesapları hakkında bilgi edinin](/azure/storage/common/storage-create-storage-account)

[Azure Stack Azure ile tutarlı depolama doğrulama kılavuzunu indirin](https://aka.ms/azurestacktp1doc)
