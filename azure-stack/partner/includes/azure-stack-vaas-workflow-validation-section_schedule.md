---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ms.openlocfilehash: c3a85e866acc615d2d33219232f7890b32df50f5
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71938991"
---
Doğrulama iş akışlarında, bir testin **zamanlanması** , iş akışı oluşturma sırasında belirttiğiniz iş akışı düzeyi ortak parametrelerini kullanır (bkz. [hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri](../azure-stack-vaas-parameters.md)). Herhangi bir test parametresi değeri geçersiz hale gelirse, [iş akışı parametrelerini değiştirme](../azure-stack-vaas-monitor-test.md#change-workflow-parameters)bölümünde belirtildiği gibi onları yeniden silmelisiniz.

> [!NOTE]
> Mevcut bir örnek üzerinde bir doğrulama testinin zamanlanması, portalda eski örnek yerine yeni bir örnek oluşturur. Eski örnek için Günlükler korunur, ancak portaldan erişilebilir olmayacaktır.  
Bir sınama başarıyla tamamlandıktan sonra, **zamanlama** eylemi devre dışı bırakılır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Test örneğini zamanlamaya yönelik bir istem açmak için bağlam menüsünden **zamanlama** ' yı seçin.

1. Test parametrelerini gözden geçirin ve ardından testleri yürütmeye zamanlamak için **Gönder** ' i seçin.