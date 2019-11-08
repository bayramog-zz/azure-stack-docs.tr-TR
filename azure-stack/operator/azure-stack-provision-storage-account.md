---
title: Azure Stack depolama hesapları oluşturun | Microsoft Docs
titleSuffix: Azure Stack
description: Azure Stack 'de depolama hesapları oluşturmayı öğrenin.
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
ms.openlocfilehash: 449d9e39b650e6f7ccd91f4703709ea033e7a5dc
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802344"
---
# <a name="create-storage-accounts-in-azure-stack"></a>Azure Stack depolama hesapları oluşturma

Azure Stack'te depolama hesapları, Blob ve Tablo hizmetlerinin yanı sıra depolama verisi nesneleriniz için benzersiz ad alanı içerir. Varsayılan olarak hesabınızdaki veriler, depolama hesabı sahibi olarak sizin kullanımınıza sunulur.

1. Azure Stack POC bilgisayarında, [yönetici](../asdk/asdk-connect.md)olarak `https://adminportal.local.azurestack.external` oturum açın ve **+ kaynak oluştur** > **veri + depolama** > **depolama hesabı**' na tıklayın.

   ![Azure Stack Yönetici portalı 'nda depolama hesabınızı oluşturun](media/azure-stack-provision-storage-account/image01.png)

2. **Depolama hesabı oluştur** dikey penceresinde, depolama hesabınız için bir ad yazın. Yeni bir kaynak grubu oluşturun veya var olan bir **kaynak grubunu**seçin ve ardından **Oluştur** ' a tıklayarak depolama hesabı oluşturun.

   ![Azure Stack Yönetici portalı 'nda depolama hesabınızı gözden geçirin](media/azure-stack-provision-storage-account/image02.png)

3. Yeni depolama hesabınızı görmek için **tüm kaynaklar**' a tıklayın, ardından Depolama hesabını arayın ve adına tıklayın.

    ![Azure Stack Yönetici portalı 'nda depolama hesabınızın adı](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager şablonlarını kullanma](../user/azure-stack-arm-templates.md)
- [Azure depolama hesapları hakkında bilgi edinin](/azure/storage/common/storage-create-storage-account)
- [Azure Stack Azure ile tutarlı depolama doğrulama kılavuzunu indirin](https://aka.ms/azurestacktp1doc)
