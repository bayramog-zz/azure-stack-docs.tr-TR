---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ms.openlocfilehash: 6cb45b58122c3e1c56098a90e294a3d67e9eb843
ms.sourcegitcommit: b79a6ec12641d258b9f199da0a35365898ae55ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133885"
---
Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](../azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](../azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

1. Test parametreleri gözden geçirin ve ardından **Gönder** test yürütme için zamanlamak için.