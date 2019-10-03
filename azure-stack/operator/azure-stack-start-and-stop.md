---
title: Başlatma ve durdurma Azure Stack | Microsoft Docs
description: Azure Stack başlatma ve kapatma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 5072c40f4e8eba787e0a18c7c73a51111d0c847b
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829260"
---
# <a name="start-and-stop-azure-stack"></a>Azure Stack başlatıp
Azure Stack hizmetlerini doğru şekilde kapatmak ve yeniden başlatmak için bu makaledeki yordamları izlemeniz gerekir. Kapatma, tüm Azure Stack ortamını fiziksel olarak kapatır. Tüm altyapı rollerinde üsler başlatın ve kiracı kaynaklarını kapanmadan önce içinde oldukları güç durumuna geri döndürür.

## <a name="stop-azure-stack"></a>Azure Stack durdur 

Aşağıdaki adımlarla Azure Stack kapatın:

1. Azure Stack ortamınızın kiracı kaynakları üzerinde çalışan tüm iş yüklerini yakında kapatılacak şekilde hazırlayın. 

2. Azure Stack ERCS VM 'lerine ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturumu (PEP) açın. Yönergeler için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](azure-stack-privileged-endpoint.md).

3. PEP 'den şunu çalıştırın:

    ```powershell
      Stop-AzureStack
    ```

4. Tüm fiziksel Azure Stack düğümlerin kapatılmasını bekleyin.

> [!Note]  
> Azure Stack donanımınızı sağlayan özgün ekipman üreticisi (OEM) yönergelerini izleyerek bir fiziksel düğümün güç durumunu doğrulayabilirsiniz. 

## <a name="start-azure-stack"></a>Azure Stack Başlat 

Aşağıdaki adımlarla Azure Stack başlatın. Azure Stack durdurulma şeklinden bağımsız olarak aşağıdaki adımları izleyin.

1. Azure Stack ortamınızdaki fiziksel düğümlerin her birinde güç. Azure Stack donanımını sağlayan özgün ekipman üreticisi (OEM) yönergelerini izleyerek fiziksel düğümlerin güç açma yönergelerini doğrulayın.

2. Azure Stack Altyapı Hizmetleri başlatılana kadar bekleyin. Azure Stack altyapı hizmetleri, başlatma işleminin tamamlanması için iki saat gerektirebilir. Azure Stack başlangıç durumunu [ **Get-actionstatus** cmdlet 'i](#get-the-startup-status-for-azure-stack)ile doğrulayabilirsiniz.

3. Tüm kiracı kaynaklarınızın kapanmadan önce bulunduğu duruma döndüğünden emin olun. Kiracı kaynaklarında çalışan iş yüklerinin, iş yükü Yöneticisi tarafından başlatıldıktan sonra yeniden yapılandırılması gerekebilir.

## <a name="get-the-startup-status-for-azure-stack"></a>Azure Stack için başlatma durumunu al

Aşağıdaki adımlarla Azure Stack başlangıç yordamının başlangıcını alın:

1. Azure Stack ERCS VM 'lerine ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturumu açın.

2. PEP 'den şunu çalıştırın:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Azure Stack başlatma ve kapatmadan ilgili sorunları giderme

Altyapı ve kiracı Hizmetleri, Azure Stack ortamınızda güç kullandıktan 2 saat sonra başarıyla başlamazsa, aşağıdaki adımları gerçekleştirin. 

1. Azure Stack ERCS VM 'lerine ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturumu açın.

2. Çalıştırın: 

    ```powershell
      Test-AzureStack
      ```

3. Çıktıyı gözden geçirin ve sistem durumu hatalarını çözün. Daha fazla bilgi için bkz. [Azure Stack doğrulama testi çalıştırma](azure-stack-diagnostic-test.md).

4. Çalıştırın:

    ```powershell
      Start-AzureStack
    ```

5. **Start-AzureStack** çalıştırmak bir hata ile sonuçlanırsa, Microsoft Müşteri Hizmetleri desteği ' ne başvurun. 

## <a name="next-steps"></a>Sonraki adımlar 

[Azure Stack tanılama araçları](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs) hakkında daha fazla bilgi edinin
