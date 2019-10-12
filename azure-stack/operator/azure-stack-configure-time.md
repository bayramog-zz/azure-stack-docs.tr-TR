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
ms.openlocfilehash: cc432538715c1c990a9efe6473b33303deb78734
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72280539"
---
# <a name="configure-the-time-server-for-azure-stack"></a>Azure Stack için saat sunucusunu yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Azure Stack saat sunucusunu güncelleştirmek için ayrıcalıklı uç noktası (PEP) kullanabilirsiniz. İki veya daha fazla NTP sunucusu IP adresine çözümlenen bir ana bilgisayar adı kullanın.

Azure Stack, Internet 'teki saat sunucularına bağlanmak için ağ saati protokolünü (NTP) kullanır. NTP sunucuları doğru sistem saatini sağlar. Süre, Azure Stack fiziksel ağ anahtarları, donanım yaşam döngüsü Konağı, altyapı hizmeti ve sanal makineler arasında kullanılır. Saat eşitlenmemişse, Azure Stack ağ ve kimlik doğrulamasıyla ilgili ciddi sorunlarla karşılaşabilir. Günlük dosyaları, belgeler ve diğer dosyalar hatalı zaman damgaları ile oluşturulabilir.

Azure Stack zaman eşitlemesini sağlamak için en az bir kerelik sunucu (NTP) gereklidir. Azure Stack dağıtırken, bir NTP sunucusunun adresini sağlarsınız. Zaman, kritik bir veri merkezi altyapı hizmetidir. Hizmet değişirse saati güncelleştirmeniz gerekecektir.

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
