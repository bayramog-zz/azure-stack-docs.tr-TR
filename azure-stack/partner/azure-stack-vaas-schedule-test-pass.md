---
title: İlk testinizi zamanlamak için Azure Stack Portal için hizmet olarak doğrulamayı kullanın | Microsoft Docs
description: İlk testinizi zamanlamak için Azure Stack Portal için bir hizmet olarak doğrulama kullanın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 8843b6791876b4e0362d7aee3aa738453a8c3b88
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955918"
---
# <a name="scheduling-a-test"></a>Test zamanlama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack çözümünüz için hizmet olarak doğrulama (VaaS) portalında bir test zamanlayın. Bir VaaS çözümü, belirli bir donanım ürün reçetesinden (BoM) Azure Stack bir çözümü temsil eder. Donanımınızın Azure Stack çalıştırıp çalıştıracağınızı denetlemek için bir test zamanlayabilirsiniz.

Çözümünüzü denetlemek için bir test iş akışı oluşturun. Bir VaaS iş akışı, VaaS çözümünün bağlamı içinde çalışır. Donanımınıza Azure Stack dağıtımının işlevlerini gerçekleştiren bir test paketi kümesini temsil eder. Çözümünüzün çevresel parametrelerini ekleyin ve çözümünüzde çalıştırmak için bir veya daha fazla test seçin.

Test geçişi iş akışı, VaaS tarafından sunulan herhangi bir testi çalıştırmak için, doğrulama iş akışlarının testleri de dahil olmak üzere kullanılıyorsa, test geçiş iş akışının sonuçları *resmi*olarak değerlendirilmez. Resmi doğrulama iş akışları hakkında daha fazla bilgi için bkz. [Iş akışları](azure-stack-vaas-key-concepts.md#workflows).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı izlemeden önce aşağıdaki öğeleri bitirebilmelisiniz:

- [Doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)
- [Yerel aracıyı dağıtma](azure-stack-vaas-local-agent.md) (gerekli)
- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md) (gerekli)

## <a name="start-a-workflow"></a>Bir iş akışı Başlat

![VaaS portalında oturum açın](media/vaas_portalsignin.png)

Portalda oturum açın, bir çözüm seçin veya oluşturma ve ardından çözümü seçme.

1. [Vaas portalında](https://azurestackvalidation.com)oturum açın.
2. Mevcut bir çözümün adını yazın veya yeni çözüm oluşturmak için **yeni çözüm** ' i seçin. Yönergeler için bkz. [VaaS portalında çözüm oluşturma](azure-stack-vaas-key-concepts.md#create-a-solution-in-the-vaas-portal).
3. **Test geçişleri** kutucuğunda **Başlat** ' ı seçin.

## <a name="specify-parameters"></a>Parametreleri belirtin

![Alternatif metin](media/vaas_test_pass_parameters.png)

İş akışı içindeki tüm testlere uygulanan parametreleri sağlayın.

1. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
2. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]
3. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]
4. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
5. Zamanlamaya göre testleri seçmek için **İleri ' yi** seçin.

## <a name="select-tests-to-run"></a>Çalıştırılacak testleri seçin

Seçtiğiniz testler, iş akışı oluşturulduktan sonra zamanlanır.

1. İş akışınızda çalıştırmak istediğiniz test (ler) i seçin.

    Herhangi bir test için ortak parametreleri (önceki bölümde belirtilen parametreler) geçersiz kılmak istiyorsanız, yeni değerleri belirtin ' in yanındaki **Düzenle** bağlantısını seçin.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

1. İş akışını gözden geçirmek için **İleri ' yi** seçin.

## <a name="review-and-submit"></a>Gözden geçir ve Gönder

İş akışı oluşturmayı tamamlama.

1. Görünen bilgileri gözden geçirin.

    Hizmet, iş akışınızı belirtilen bilgilerle oluşturur ve seçilen testler zamanlanır.

    Bir şeyler yanlış görünüyorsa **önceki düğmeleri kullanarak önceki bir** bölüme gidebilirsiniz.

1. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)
