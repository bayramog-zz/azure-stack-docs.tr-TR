---
title: Azure Stack için şablonlar geliştir | Microsoft Docs
description: Azure ile Azure Stack arasında uygulama taşınabilirliği için Azure Resource Manager şablonları geliştirmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: bedc4c3971c5d4a177f4d8ac804878babebaa9b6
ms.sourcegitcommit: b3dac698f2e1834491c2f9af56a80e95654f11f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658658"
---
# <a name="develop-templates-for-azure-stack-with-azure-resource-manager"></a>Azure Resource Manager Azure Stack için Şablonlar geliştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Uygulamanızı geliştirirken, Azure ile Azure Stack arasında şablon taşınabilirliği olması önemlidir. Bu makalede [Azure Resource Manager şablonları](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf)geliştirmeye yönelik konular sunulmaktadır. Bu şablonlar sayesinde uygulamanızı ve test dağıtımınızı bir Azure Stack ortamına erişim olmadan Azure 'da prototip oluşturabilirsiniz.

## <a name="resource-provider-availability"></a>Kaynak sağlayıcısı kullanılabilirliği

Dağıtmayı planladığınız şablon yalnızca, Azure Stack ' de zaten mevcut olan veya Önizlemedeki Microsoft Azure hizmetleri kullanmalıdır.

## <a name="public-namespaces"></a>Ortak ad alanları

Azure Stack veri merkezinizde barındırıldığından Azure genel bulutinden farklı hizmet uç noktası ad alanları vardır. Sonuç olarak, Azure Resource Manager şablonlarda sabit kodlanmış ortak uç noktalar, Azure Stack dağıtmaya çalıştığınızda başarısız olur. Dağıtım sırasında kaynak sağlayıcıdan değerleri almak için `reference` ve `concatenate` işlevlerini kullanarak hizmet uç noktalarını dinamik olarak oluşturabilirsiniz. Örneğin, şablonunuzda sabit kodlama `blob.core.windows.net` yerine, *OSDisk. URI* uç noktasını dinamik olarak ayarlamak için en düşük olan yenizsel [. blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-vm-windows-create/azuredeploy.json#L175) ' u alın:

```json
"osDisk": {"name": "osdisk","vhd": {"uri":
"[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
 '/',variables('OSDiskName'),'.vhd')]"}}
```

## <a name="api-versioning"></a>API sürümü oluşturma

Azure hizmet sürümleri, Azure ile Azure Stack arasında farklılık gösterebilir. Her kaynak için sunulan özellikleri tanımlayan **Apiversion** özniteliği gerekir. Azure Stack ' de API sürümü uyumluluğunu sağlamak için, aşağıdaki API sürümleri her kaynak sağlayıcısı için geçerlidir:

| Kaynak Sağlayıcısı | apiVersion |
| --- | --- |
| İşlem |**2015-06-15** |
| Ağ |**2015-06-15**, **2015-05-01-önizleme** |
| Depolama |**2016-01-01**, **2015-06-15**, **2015-05-01-önizleme** |
| KeyVault | **2015-06-01** |
| App Service |**2015-08-01** |

## <a name="template-functions"></a>Şablon işlevleri

Azure Resource Manager [işlevler](/azure/azure-resource-manager/resource-group-template-functions) , dinamik şablonlar oluşturmak için gereken özellikleri sağlar. Örnek olarak, şu gibi görevler için işlevleri kullanabilirsiniz:

* Dizeleri bitiştirme veya kırpma.
* Diğer kaynaklardaki değerlere başvuru.
* Birden çok örneği dağıtmak için kaynaklar üzerinde yineleme.

Bu işlevler Azure Stack kullanılamaz:

* Atla
* Almanız

## <a name="resource-location"></a>Kaynak konumu

Azure Resource Manager şablonlar, dağıtım `location` sırasında kaynakları yerleştirmek için bir özniteliği kullanır. Azure 'da konumlar, Batı ABD veya Güney Amerika gibi bir bölgeye başvurur. Azure Stack, veri merkezinizde Azure Stack olduğundan konumlar farklıdır. Şablonların Azure ile Azure Stack arasında aktarılabilir olmasını sağlamak için, tek tek kaynakları dağıtırken kaynak grubu konumuna başvurmanız gerekir. Tüm kaynakların kaynak grubu konumunu `[resourceGroup().Location]` devralmasını sağlamak için bunu kullanarak yapabilirsiniz. Aşağıdaki kod, bir depolama hesabı dağıtıldığında bu işlevi kullanmanın bir örneğidir:

```json
"resources": [
{
  "name": "[variables('storageAccountName')]",
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "[variables('apiVersionStorage')]",
  "location": "[resourceGroup().location]",
  "comments": "This storage account is used to store the VM disks",
  "properties": {
  "accountType": "Standard_GRS"
  }
}
]
```

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Azure CLı ile şablonları dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
