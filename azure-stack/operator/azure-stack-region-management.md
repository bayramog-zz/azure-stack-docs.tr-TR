---
title: Azure Stack 'de bölge yönetimi | Microsoft Docs
titleSuffix: Azure Stack
description: Azure Stack 'de bölge yönetimine genel bakış.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 11/27/2018
ms.openlocfilehash: 38004b88f43ef59448ca99c3eb2762e5ca63e89c
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802272"
---
# <a name="region-management-in-azure-stack"></a>Azure Stack 'de bölge yönetimi

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, Azure Stack altyapısını oluşturan donanım kaynaklarından oluşan mantıksal varlıklar olan *bölge*kavramını kullanır. Bölge yönetimi 'nde, Azure Stack altyapısını başarıyla işletmek için gereken tüm kaynakları bulabilirsiniz.

Tümleşik bir sistem dağıtımı ( *Azure Stack bulutu*olarak adlandırılır) tek bir bölge oluşturur. Her Azure Stack Geliştirme Seti (ASDK), **Yerel**adlı bir bölgeye sahiptir. İkinci bir Azure Stack tümleşik sistem dağıtırsanız veya ayrı donanımda ASDK 'nin başka bir örneğini ayarlarsanız, bu Azure Stack bulut farklı bir bölgedir.

## <a name="information-available-through-the-region-management-tile"></a>Bölge yönetimi kutucuğu üzerinden kullanılabilir bilgiler

Azure Stack, **Bölge yönetimi** kutucuğunda kullanılabilen bir dizi bölge yönetimi özelliğine sahiptir. Bu kutucuk, yönetici portalındaki varsayılan panoda bir Azure Stack işleci ile kullanılabilir. Bu kutucuk sayesinde, bölgeye özgü olan Azure Stack bölgenizi ve bileşenlerini izleyebilir ve güncelleştirebilirsiniz.

![Azure Stack Yönetici portalı 'nda bölge yönetimi kutucuğu](media/azure-stack-region-management/image1.png)

**Bölge yönetimi** kutucuğunda bir bölgeye tıklarsanız, aşağıdaki bilgilere erişebilirsiniz:

[![Azure Stack Yönetici portalı 'nda bölge yönetimi dikey penceresinde bölmeler açıklaması](media/azure-stack-region-management/regionssm.png "Azure Stack Yönetici portalı 'nda bölge yönetimi dikey penceresi")](media/azure-stack-region-management/regions.png#lightbox)

1. **Kaynak menüsü**: farklı altyapı yönetimi bölümlerine erişin ve depolama hesapları ve sanal ağlar gibi kullanıcı kaynaklarını görüntüleyin ve yönetin.

2. **Uyarılar**: sistem genelinde uyarıları listeleyin ve bu uyarıların her biriyle ilgili ayrıntıları sağlayın.

3. **Güncelleştirmeler**: Azure Stack altyapınızın güncel sürümünü, kullanılabilir güncelleştirmeleri ve güncelleştirme geçmişini görüntüleyin. Ayrıca, tümleşik sisteminizi güncelleştirebilirsiniz.

4. **Kaynak sağlayıcıları**: Azure Stack çalıştırmak için gereken bileşenler tarafından sunulan Kullanıcı işlevselliğini yönetin. Her kaynak sağlayıcı bir yönetim deneyimiyle birlikte gelir. Bu deneyim, belirli sağlayıcı, ölçümler ve kaynak sağlayıcısına özgü diğer yönetim özelliklerine yönelik uyarıları içerebilir.

5. **Altyapı rolleri**: Azure Stack çalıştırmak için gereken bileşenler. Yalnızca uyarıları rapor eden altyapı rolleri listelenir. Rol seçerek, rolle ilişkili uyarıları ve bu rolün çalıştırıldığı rol örneklerini görebilirsiniz.

6. **Özellikler**: bölge yönetimi dikey penceresinde ortamınızın kayıt durumu ve ayrıntıları. Durum **kaydedilebilir**, **kaydettirilmemiş**veya **geçerliliği zaman aşımına**eklenebilir. Kaydolduysanız, kayıt kaynak grubu ve adıyla birlikte Azure Stack kaydetmek için kullandığınız Azure abonelik KIMLIĞINI de gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack’de işlevsel durumu ve uyarıları izleme](azure-stack-monitor-health.md)
- [Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)
