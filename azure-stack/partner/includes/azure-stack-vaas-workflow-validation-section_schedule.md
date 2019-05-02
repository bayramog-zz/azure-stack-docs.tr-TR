---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 03/04/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ms.openlocfilehash: e9d59fadb9cbfeb7463f799bdab2e6da49208fe9
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64299069"
---
Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](../azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](../azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

1. Test parametreleri gözden geçirin ve ardından **Gönder** test yürütme için zamanlamak için.