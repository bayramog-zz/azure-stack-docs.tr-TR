---
title: Yeni bir Azure Stack çözümü doğrulama | Microsoft Docs
description: Hizmet olarak doğrulama ile yeni bir Azure Stack çözümünü doğrulamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: fe744fc95c79cb7482e1baa213471335ebb6e1fd
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418301"
---
# <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözümünü doğrulama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Yeni Azure Stack çözümlerini onaylamak için **Çözüm doğrulama** iş akışını nasıl kullanacağınızı öğrenin.

Azure Stack çözümü, Windows Server logosu sertifika gereksinimleri yükseltildikten sonra Microsoft ile iş ortağı arasında ortaklaşa anlaşılabilecek bir donanım ürün reçetesinden (BoM). Donanım ürün reçetesinde değişiklik yapıldığında bir çözümün yeniden başlatılması gerekir. Çözümlerin ne zaman yeniden düzeltilme [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com)hakkında daha fazla bilgi için, konusundaki ekibine başvurun.

Çözümünüzü onaylamak için çözüm doğrulama iş akışını iki kez çalıştırın. En *düşük* düzeyde desteklenen yapılandırma için bunu bir kez çalıştırın. En *yüksek düzeyde* desteklenen yapılandırma için bu dosyayı ikinci kez çalıştırın. Her iki yapılandırma da tüm testleri iletmezse, Microsoft bu çözümü onaylar.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Çözüm doğrulama iş akışı oluşturma

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. **Çözüm doğrulamaları** kutucuğunda **Başlat** ' ı seçin.

    ![Çözüm doğrulamaları iş akışı kutucuğu](media/tile_validation-solution.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. **Çözüm yapılandırmasını**seçin.
    - **Minimum**: çözüm, desteklenen en az sayıda düğüm ile yapılandırılır.
    - **Maksimum**: çözüm desteklenen en fazla düğüm sayısıyla yapılandırılır.
6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Çözüm doğrulama bilgileri](media/workflow_validation-solution_info.png)

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Ortam parametreleri, iş akışı oluşturulduktan sonra değiştirilemez.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testler özet sayfasına yönlendirilirsiniz.

## <a name="run-solution-validation-tests"></a>Çözüm doğrulama testlerini Çalıştır

**Çözüm doğrulama sınamaları Özeti** sayfasında, doğrulamayı tamamlamak için gereken testlerin bir listesini görürsünüz.

Doğrulama iş akışlarında, bir testin **zamanlanması** , iş akışı oluşturma sırasında belirttiğiniz iş akışı düzeyi ortak parametrelerini kullanır (bkz. [hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametresi değeri geçersiz hale gelirse, [iş akışı parametrelerini değiştirme](azure-stack-vaas-monitor-test.md#change-workflow-parameters)bölümünde belirtildiği gibi onları yeniden silmelisiniz.

> [!NOTE]
> Mevcut bir örnek üzerinde bir doğrulama testinin zamanlanması, portalda eski örnek yerine yeni bir örnek oluşturur. Eski örnek için Günlükler korunur, ancak portaldan erişilebilir olmayacaktır.  
Bir sınama başarıyla tamamlandıktan sonra, **zamanlama** eylemi devre dışı bırakılır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

2. Aşağıdaki testleri seçin:
    - Bulut benzetimi altyapısı
    - İşlem SDK 'Sı Işlem paketi
    - Disk kimliği sınaması
    - Keykasauzantısı SDK 'Sı çalışma paketi
    - Keykasasdk Işlem paketi
    - Ağ SDK Işlem paketi
    - Depolama hesabı SDK 'Sı Işlem paketi

3. Test örneğini zamanlamaya yönelik bir istem açmak için bağlam menüsünden **zamanlama** ' yı seçin.

4. Test parametrelerini gözden geçirin ve ardından testleri yürütmeye zamanlamak için **Gönder** ' i seçin.

![Çözüm doğrulama testini zamanla](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)