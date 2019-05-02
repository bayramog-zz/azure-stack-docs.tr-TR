---
title: İlk test zamanlamak için Azure Stack portalı için bir hizmet olarak doğrulama kullanın | Microsoft Docs
description: İlk test zamanlamak için Azure Stack portalı için bir hizmet olarak doğrulama kullanın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 3fb5c3deeddb6f3ee381ca45df76feebf3405b21
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64300510"
---
# <a name="scheduling-a-test"></a>Test planlama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama Hizmeti (VaaS) portalı olarak Azure Stack çözümünüz için bir test zamanlayın. VaaS çözüm belirli donanım ürün reçetesi (BoM) ile bir Azure Stack çözüm temsil eder. Azure Stack, donanım çalıştırabileceğiniz denetlemek için bir test zamanlayabilirsiniz.

Çözümünüzü denetlemek için iş akışı için bir test oluşturun. VaaS iş akışı VaaS çözüm bağlamında çalışır. Bu test paketleri, bir Azure Stack dağıtımı donanımınıza işlevselliğini çalışma kümesini temsil eder. Çözümünüzün ortam parametreleri ekleyin ve çözümünüzü çalıştırmak için bir veya daha fazla test seçin.

Test geçiş iş akışı VaaS testlerden doğrulama iş akışları dahil olmak üzere tarafından sağlanan herhangi bir testi çalıştırmak için kullanılabilse sonuçları Test geçiş iş akışı değil olarak kabul edilir *resmi*. Resmi doğrulama iş akışları hakkında daha fazla bilgi için bkz: [iş akışları](azure-stack-vaas-key-concepts.md#workflows).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta izlemeden önce aşağıdaki öğeleri tamamlanmalıdır:

- [Hizmet kaynakları olarak doğrulama ayarlayın](azure-stack-vaas-set-up-resources.md)
- [Yerel aracı dağıtma](azure-stack-vaas-local-agent.md) (gerekli)
- [Doğrulama Hizmeti temel kavramları olarak](azure-stack-vaas-key-concepts.md) (gerekli)

## <a name="start-a-workflow"></a>Bir iş akışını Başlat

![VaaS portalında oturum](media/vaas_portalsignin.png)

Portal, seçin veya bir çözüm oluşturma için oturum açın ve ardından çözümü seçin.

1. Oturum [VaaS portalı](https://azurestackvalidation.com).
2. Varolan bir çözüm adı yazın veya seçin **yeni çözüm** yeni bir çözüm oluşturmak için. Yönergeler için [VaaS portalda bir çözüm oluşturma](azure-stack-vaas-key-concepts.md#create-a-solution-in-the-vaas-portal).
3. Seçin **Başlat** üzerinde **Test geçişleri** Döşe.

## <a name="specify-parameters"></a>Parametreleri belirtin

![Alternatif metin](media/vaas_test_pass_parameters.png)

İş akışı içindeki tüm testler için geçerli parametreler belirtin.

1. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
2. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]
3. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]
4. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
5. Seçin **sonraki** zamanlamak için testleri seçmek için.

## <a name="select-tests-to-run"></a>Çalıştırılacak testleri seçin

İş akışını oluşturduktan sonra seçtiğiniz testleri zamanlanacak.

1. İş akışınızı çalıştırmak istediğiniz test seçin.

    Ortak parametreleri (diğer bir deyişle, önceki bölümde sağlanan parametreleri) herhangi bir test geçersiz kılmak istiyorsanız, seçin **Düzenle** bağlantı sonraki yeni değerleri belirtin.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

1. Seçin **sonraki** iş akışını gözden geçirmek için.

## <a name="review-and-submit"></a>Gözden geçir ve Gönder

İş akışı oluşturmayı tamamlayın.

1. Görüntülenen bilgileri gözden geçirin.

    Hizmet, iş akışınızı sağlanan bilgilerle oluşturur ve seçili testleri zamanlanacak.

    Herhangi bir şey yanlış görünür kullanırsanız **önceki** önceki bir bölüme gitmek için düğmeler.

1. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
