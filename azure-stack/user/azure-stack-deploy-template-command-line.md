---
title: Azure Stack | komut satırı ile şablon dağıtma | Microsoft Docs
description: Azure Stack için şablonları dağıtmak üzere Azure platformlar arası komut satırı arabirimi 'ni (CLı) nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 10/07/2019
ms.author: mabrigg
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 7b3daaefd8fa7e7bce9c6d5708e664911fc906fe
ms.sourcegitcommit: 7226979ece29d9619c959b11352be601562b41d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72304091"
---
# <a name="deploy-a-template-with-the-command-line-in-azure-stack"></a>Azure Stack içinde komut satırı ile şablon dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Azure Resource Manager şablonlarını dağıtmak için Azure komut satırı arabirimi 'ni (CLı) kullanabilirsiniz. Azure Resource Manager şablonlar, uygulamanıza yönelik kaynakları tek ve eşgüdümlü bir eylemde dağıtıp ayarlar.

## <a name="deploy-template"></a>Şablon dağıtma

1. [Azurestack-hızlı başlangıç-şablonlar](https://aka.ms/AzureStackGitHub) deposuna gözatıp **101-Create-Storage-Account** şablonunu bulun. Şablonu (`azuredeploy.json`) ve parametre dosyalarını `(azuredeploy.parameters.json`) Yerel sürücünüzdeki bir konuma `C:\templates\` gibi kaydedin
2. Dosyalarını indirdiğiniz klasöre gidin. 
3. Azure CLı ile Azure Stack [yükleyip bağlayın](azure-stack-version-profiles-azurecli2.md) .
4. Aşağıdaki komutta yer alan bölgeyi ve konumu güncelleştirin. ASDK kullanıyorsanız konum parametresi için `local` kullanın. Şablonu dağıtmak için:
    ```azurecli
    az group create --name testDeploy --location local
    az group deployment create --resource-group testDeploy --template-file ./azuredeploy.json --parameters ./azuredeploy.parameters.json
    ```

Bu komut, şablonu Azure Stack örneğiniz için **Testdeploy** kaynak grubuna dağıtır.

## <a name="validate-template-deployment"></a>Şablon dağıtımını doğrula

Kaynak grubunu ve depolama hesabını gözden geçirmek için aşağıdaki CLı komutlarını çalıştırın:

```azurecli
az group list
az storage account list
```

## <a name="next-steps"></a>Sonraki adımlar

[PowerShell kullanarak şablonları dağıtmayı](azure-stack-deploy-template-powershell.md)öğrenin.
