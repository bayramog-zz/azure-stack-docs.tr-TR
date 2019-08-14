---
title: Azure Stack | komut satırı ile şablon dağıtma | Microsoft Docs
description: Azure Stack için şablonları dağıtmak üzere Azure platformlar arası komut satırı arabirimi 'ni (CLı) nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: da17e80c802e210d53effbad8f264b1a4019e6e0
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991841"
---
# <a name="deploy-a-template-with-the-command-line-in-azure-stack"></a>Azure Stack içinde komut satırı ile şablon dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Azure Resource Manager şablonlarını dağıtmak için Azure komut satırı arabirimi 'ni (CLı) kullanabilirsiniz. Azure Resource Manager şablonlar, uygulamanıza yönelik kaynakları tek ve eşgüdümlü bir işlem halinde dağıtıp temin ediyor.

## <a name="before-you-begin"></a>Başlamadan önce

- Azure CLı ile Azure Stack [yükleyip bağlayın](azure-stack-version-profiles-azurecli2.md) .
- [Depolama hesabı oluşturma örnek şablonundan](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account) *azuredeploy. JSON* ve *azuredeploy. Parameters. JSON* dosyalarını indirin.

## <a name="deploy-template"></a>Şablon dağıtma

Bu dosyaların indirileceği klasöre gidin ve şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
az group create "cliRG" "local" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json
```

Bu komut, şablonu Azure Stack POC varsayılan konumundaki **cliRG** kaynak grubuna dağıtır.

## <a name="validate-template-deployment"></a>Şablon dağıtımını doğrula

Bu kaynak grubunu ve depolama hesabını görmek için aşağıdaki CLı komutlarını kullanın:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Sonraki adımlar

[PowerShell kullanarak şablonları dağıtmayı](azure-stack-deploy-template-powershell.md)öğrenin.
