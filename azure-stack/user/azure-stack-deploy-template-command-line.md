---
title: Azure Stack komut satırında bir şablonla dağıtma | Microsoft Docs
description: Şablonları Azure Stack'e dağıtma için platformlar arası komut satırı arabirimi (CLI) kullanmayı öğrenin.
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
ms.date: 05/09/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 92c9189f8144804f36e551ab89d8b4fc4c1f8598
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691374"
---
# <a name="deploy-a-template-with-the-command-line-in-azure-stack"></a>Şablon Azure Stack'te komut satırı ile dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure Resource Manager şablonlarını dağıtmak için Azure komut satırı arabirimi (CLI) kullanın. Azure Resource Manager şablonları, dağıtın ve kaynakları tek ve eşgüdümlü bir işlemle uygulamanızda sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce

- [Yükleme ve bağlanma](azure-stack-version-profiles-azurecli2.md) Azure CLI ile Azure Stack için.
- Dosyaları indirmek *azuredeploy.json* ve *azuredeploy.parameters.json* gelen [depolama hesabı örnek şablonu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Şablon dağıtma

Bu dosyalar indirilir ve şablonu dağıtmak için aşağıdaki komutu çalıştırın klasörüne gidin:

```azurecli
az group create "cliRG" "local" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json
```

Bu komut kaynak grubuna şablon dağıtır **cliRG** Azure Stack POC varsayılan konumda.

## <a name="validate-template-deployment"></a>Şablon dağıtımı doğrulama

Bu kaynak grubu ve depolama hesabı görmek için aşağıdaki CLI komutları kullanın:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [şablonlar, PowerShell kullanarak dağıtma](azure-stack-deploy-template-powershell.md).
