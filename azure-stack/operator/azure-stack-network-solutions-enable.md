---
title: Fortinet FortiGate ile Azure Stack bir ağ çözümü sunun | Microsoft Docs
description: Fortinet FortiGate ile Azure Stack ağ çözümünü nasıl etkinleştirebileceğinizi öğrenin
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 09/30/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/30/2019
ms.openlocfilehash: 27012c491054043f45004d76787538091864577c
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962446"
---
# <a name="offer-a-network-solution-in-azure-stack-with-fortinet-fortigate"></a>Fortinet FortiGate ile Azure Stack bir ağ çözümü sunun

Yeni nesil güvenlik duvarını Azure Stack Market 'e ekleyebilirsiniz. FortiGate, kullanıcılarınızın Azure Stack ve VNET eşlemesi için sanal özel ağ (VPN) gibi ağ çözümleri oluşturmalarına olanak sağlar. Ağ sanal gereci (NVA), ağ trafiğinin bir çevre ağdan diğer ağlara veya alt ağlara akışını denetler. 

Azure Marketi 'nde FortiGate hakkında daha fazla bilgi için bkz. [Fortinet FortiGate yeni nesil güvenlik duvarı tek VM çözümü](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm).

## <a name="download-the-required-azure-stack-marketplace-items"></a>Gerekli Azure Stack Market öğelerini indirin

1.  Azure Stack Yönetici portalı ' nı açın.

2.  **Market yönetimi** ' ni seçin ve **Azure 'dan Ekle**' yi seçin.

3. Arama kutusuna `Forti` yazın ve aşağıdaki öğelerin kullanılabilir en son sürümlerini **almak için >** çift tıklayın. 
    - Fortinet FortiGate-Azure KLG Için VM
    - FortiGate NGFW-tek VM dağıtımı (KLG)

    ![Azure Stack FortiGate Fortinet](./media/azure-stack-network-solutions-enable/azure-stack-marketplace-FortiGate-fortinet.png)

2.  Market öğelerinizin durumu **indirilene**kadar bekleyin. Öğelerin yüklenmesi birkaç dakika sürebilir.

    ![Azure Stack FortiGate Fortinet](./media/azure-stack-network-solutions-enable/image4.png)

## <a name="next-steps"></a>Sonraki adımlar

[FortiGate NVA kullanarak Azure Stack için VPN kurma](../user/azure-stack-network-howto-vnet-to-onprem.md)  
[Eşleme ile iki sanal ağı bağlama](../user/azure-stack-network-howto-vnet-to-vnet.md)  
[Fortinet FortiGate NVA ile VNET 'ten VNET 'e bağlantı kurma](../user/azure-stack-network-howto-vnet-to-vnet-stacks.md)  
