---
title: Azure Stack için saat sunucusunu yapılandırma | Microsoft Docs
description: Azure Stack için saat sunucusunu yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 10/10/2019
ms.openlocfilehash: a70eaaf46988524f5323052a3f2ca90f5b7719e1
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73636806"
---
# <a name="configure-the-time-server-for-azure-stack"></a>Azure Stack için saat sunucusunu yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Azure Stack saat sunucusunu güncelleştirmek için ayrıcalıklı uç noktası (PEP) kullanabilirsiniz. İki veya daha fazla NTP sunucusu IP adresine çözümlenen bir ana bilgisayar adı kullanın.

Azure Stack, Internet 'teki saat sunucularına bağlanmak için ağ saati protokolünü (NTP) kullanır. NTP sunucuları doğru sistem saatini sağlar. Süre, Azure Stack fiziksel ağ anahtarları, donanım yaşam döngüsü Konağı, altyapı hizmeti ve sanal makineler arasında kullanılır. Saat eşitlenmemişse, Azure Stack ağ ve kimlik doğrulamasıyla ilgili ciddi sorunlarla karşılaşabilir. Günlük dosyaları, belgeler ve diğer dosyalar hatalı zaman damgaları ile oluşturulabilir.

Azure Stack zaman eşitlemesini sağlamak için bir kerelik sunucu (NTP) sağlanması gerekir. Azure Stack dağıtırken, bir NTP sunucusunun adresini sağlarsınız. Zaman, kritik bir veri merkezi altyapı hizmetidir. Hizmet değişirse saati güncelleştirmeniz gerekecektir.

> [!NOTE]
> Azure Stack, yalnızca bir kerelik sunucu (NTP) ile eşitleme süresini destekler. Azure Stack ile zaman eşitlemesini sağlamak için birden çok NTPs belirtemezsiniz.

## <a name="configure-time"></a>Saati Yapılandır

1. [PEP 'ye bağlanın](azure-stack-privileged-endpoint.md). 
    > [!Note]  
    > Destek bileti açarak ayrıcalıklı uç noktanın kilidini açmak gerekli değildir.

2. Geçerli yapılandırılmış NTP sunucusunu gözden geçirmek için aşağıdaki komutu çalıştırın:

    ```PowerShell
    Get-AzsTimeSource
    ```

3. Yeni NTP sunucusunu kullanmak ve zamanı anında eşitlemesini Azure Stack güncelleştirmek için aşağıdaki komutu çalıştırın.

    > [!Note]  
    > Bu yordam, fiziksel anahtarlarda saat sunucusunu güncelleştirmeyin

    ```PowerShell
    Set-AzsTimeSource -TimeServer NEWTIMESERVERIP -resync
    ```

4. Tüm hatalar için komutun çıkışını gözden geçirin.


## <a name="next-steps"></a>Sonraki adımlar

[Hazırlık raporunu görüntüleme](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  
