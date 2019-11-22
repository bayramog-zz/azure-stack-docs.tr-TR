---
title: Azure Stack 'de DNS ileticisini güncelleştirin | Microsoft Docs
description: Azure Stack 'de DNS ileticisini güncelleştirme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: 22e49f28dee6b4aa97b9e84cf52950dd678450e4
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308207"
---
# <a name="update-the-dns-forwarder-in-azure-stack"></a>Azure Stack 'de DNS ileticisini güncelleştirme

Azure Stack altyapısının dış adları çözmesi için en az bir erişilebilir DNS ileticisi gereklidir. Azure Stack dağıtımı için bir DNS ileticisi sağlanması gerekir. Bu giriş, yönlendirici olarak Azure Stack iç DNS sunucuları için kullanılır ve kimlik doğrulaması, Market yönetimi veya kullanım gibi hizmetler için dış ad çözümlemesini sunar.

DNS, değişen önemli bir veri merkezi altyapı hizmetidir ve Azure Stack güncelleştirilmeleri gerekir.

Bu makalede, Azure Stack 'de DNS ileticisini güncelleştirmek için ayrıcalıklı uç nokta (PEP) kullanılması açıklanmaktadır. İki güvenilir DNS ileticisi IP adresi kullanmanız önerilir.

1. [Ayrıcalıklı uç noktaya](azure-stack-privileged-endpoint.md)bağlanın. Destek bileti açarak ayrıcalıklı uç noktanın kilidini açmak için gerekli değildir.

2. Geçerli yapılandırılmış DNS ileticisini gözden geçirmek için aşağıdaki komutu çalıştırın. Alternatif olarak, yönetim portalı bölge özelliklerini de kullanabilirsiniz:

   ```powershell
   Get-AzsDnsForwarder
   ```

3. Yeni DNS ileticisini kullanmak üzere Azure Stack güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```powershell
    Set-AzsDnsForwarder -IPAddress "IPAddress 1","IPAddress 2"
   ```

4. Tüm hatalar için komutun çıkışını gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)
