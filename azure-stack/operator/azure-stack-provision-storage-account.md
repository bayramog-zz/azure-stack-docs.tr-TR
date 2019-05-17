---
title: Azure stack'teki depolama hesapları | Microsoft Docs
description: Bir Azure Stack depolama hesabı oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 77d4963d2e3e468cb2de1e41a5c483e0339a3449
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782454"
---
# <a name="storage-accounts-in-azure-stack"></a>Azure Stack'teki depolama hesapları

Azure stack'teki depolama hesapları, Blob ve tablo hizmetlerinin yanı sıra depolama verisi nesneleriniz için benzersiz ad alanı içerir. Varsayılan olarak hesabınızdaki veriler, depolama hesabı sahibi olarak sizin kullanımınıza sunulur.

1. Azure Stack POC bilgisayarda oturum açın `https://adminportal.local.azurestack.external` olarak [yönetici](../asdk/asdk-connect.md)ve ardından **+ kaynak Oluştur** > **veri + depolama**  >  **Depolama hesabı**.

   ![Depolama hesabınızı oluşturun](media/azure-stack-provision-storage-account/image01.png)
2. İçinde **depolama hesabı oluşturma** dikey penceresinde, depolama hesabınız için bir ad yazın. Yeni bir **kaynak grubu**, veya varolan bir tanesini seçin ve ardından tıklayın **Oluştur** depolama hesabı oluşturmak için.

   ![Depolama hesabınızı gözden geçirin](media/azure-stack-provision-storage-account/image02.png)
3. Yeni depolama hesabınızı görmek için tıklayın **tüm kaynakları**, ardından depolama hesabı için arama yapın ve onun adına tıklayın.

    ![Depolama hesabınızın adını](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Sonraki adımlar
[Azure Resource Manager şablonlarını kullanma](../user/azure-stack-arm-templates.md)

[Azure depolama hesapları hakkında bilgi edinin](/azure/storage/common/storage-create-storage-account)

[Azure Stack Azure ile tutarlı depolama doğrulama kılavuzunu indirin](https://aka.ms/azurestacktp1doc)
