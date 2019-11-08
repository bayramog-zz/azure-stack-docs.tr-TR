---
title: Azure Stack Market 'e Commkasasını ekleme | Microsoft Docs
description: Azure Stack Market 'e Commkasasını eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: 787453550e9a8ed69aa9dfda0a03ea5e57c49d7a
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802318"
---
# <a name="add-commvault-to-the-azure-stack-marketplace"></a>Azure Stack Market 'e Commkasasını ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, ayrı bir Azure Stack ölçek biriminde bulunan bir kurtarma VM 'sini güncelleştirmek üzere Commkasak canlı eşitlemesi sunumu gösterilmektedir. BT kasasını, kullanıcılarınız için bir yedekleme ve çoğaltma çözümü olarak indirebilir ve sunabilirsiniz. 

## <a name="notes-for-commvault"></a>Commkasası notları

- Kullanıcılarınızın, kaynak Azure Stack aboneliğindeki bir VM 'ye yedekleme ve çoğaltma yazılımını yüklemesi gerekir. Azure Site Recovery ve Azure Backup, yedeklemelerinizi ve kurtarma görüntülerinizi depolamak için yığın dışı bir konum sunabilir. Her ikisi de, aşağıdaki konumlardan Azure Stack yüklenecek yazılım görüntülerini indirmeden önce Azure 'da bir kurtarma hizmetleri Kasası oluşturulmasını gerektirir: [Azure Backup sunucusu](https://go.microsoft.com/fwLink/?LinkId=626082&clcid=0x0409) ve [Azure Site Recovery](https://aka.ms/unifiedinstaller_eus).  
    
- Üçüncü taraf yazılım (seçilmişse) lisanslarına ihtiyacınız olabilir.
- Kullanıcılarınız, kaynak ve hedefi bir sanal ağ geçidi (VPN) veya genel IP aracılığıyla yedekleme ve çoğaltma ana bilgisayarında bağlarken yardımcı olabilir.
- Bir kurtarma hedefi Azure Stack Azure bulut aboneliğini veya aboneliğini hedefleyin.
- Hedef kaynak grubu ve bir kurtarma hedefi Azure Stack BLOB depolama hesabı.
- Bazı çözümler, kaynak sunucudan değişikliklerin alabilmesi için 24x7x365 ' i çalıştırması gereken hedef abonelikte sanal makineler oluşturmanızı gerektirir. [Sanal makinelerinizi commkasasıyla Azure Stack yedekleme](../user/azure-stack-network-howto-backup-commvault.md)işleminde, Commkasali canlı eşitleme, ilk yapılandırma sırasında hedef kurtarma VM 'lerini oluşturur ve değişikliklerin bir çoğaltma çevrimi sırasında uygulanması gerekene kadar boşta (çalışmıyor, faturalandırılabilir değil) olarak kalır.


## <a name="get-commvault-for-your-marketplace"></a>Market için Commkasadan yararlanın

1. Azure Stack Yönetim Portalı ' nı açın.
2. **Azure 'Dan Add** **Market yönetimi** > seçin.

    ![Azure Stack için commkasakasası](./media/azure-stack-network-offer-backup-commvault/get-commvault-for-marketplace.png)

3. `commvault` yazın.
4. **Commkasasının deneme sürümünü**seçin. Ardından **İndir**' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

[Sanal makineyi Commkasasıyla Azure Stack yedekleme](../user/azure-stack-network-howto-backup-commvault.md)

[Azure Stack hizmetleri sunma hakkında genel bakış](service-plan-offer-subscription-overview.md)
