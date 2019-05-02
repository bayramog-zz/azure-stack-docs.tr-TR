---
title: Azure Stack araçları Github'dan indirin. | Microsoft Docs
description: Azure Stack ile çalışmak için gereken araçları indirme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
origin.date: 01/14/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: e7c24431a508e45fea0a61025b62f9bc3443d97b
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64293844"
---
# <a name="download-azure-stack-tools-from-github"></a>Github'dan Azure Stack araçları indirin

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

**AzureStack Araçları** olduğu bir [GitHub deposu](https://github.com/Azure/AzureStack-Tools) yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındırır. VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci indirebilirsiniz. Bu araçları edinmek için GitHub deposunu kopyalayın veya indirme **AzureStack Araçları** aşağıdaki betiği çalıştırarak klasörü:

```powershell
# Change directory to the root directory. 
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Modüller tarafından sağlanan işlevselliği

**AzureStack Araçları** deposu, aşağıdaki işlevleri desteklemek için Azure Stack PowerShell modüllerini içerir:  

| İşlev | Açıklama | Kimler bu modül kullanabilir miyim? |
| --- | --- | --- |
| [Bulut özellikleri](../user/azure-stack-validate-templates.md) | Bulut'u bulut özelliklerini almak için bu modülü kullanın. Örneğin, bu modül kullanılarak API sürümü ve Azure Resource Manager kaynakları gibi bulut özellikleri elde edebilirsiniz. Ayrıca VM uzantıları Azure Stack ve Azure bulut için bu modülü kullanarak alabilirsiniz. | Bulut operatörleri ve kullanıcıları |
| [Azure Stack için Resource Manager İlkesi](../user/azure-stack-policy-module.md) | Azure Stack olarak aynı sürüm oluşturma ve hizmet kullanılabilirliği olan bir Azure aboneliği veya bir Azure kaynak grubu yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcıları |
| [Azure ile kaydedin](azure-stack-registration.md ) | Azure ile Geliştirme Seti örneğinizin kaydetmek için bu modülü kullanın. Kaydolduktan sonra Azure Market öğelerini indirme ve bunları Azure Stack'te kullanın. | Bulut operatörleri |
| [Azure Stack dağıtımı](../asdk/asdk-install.md) | Dağıtma ve Azure Stack sanal sabit disk (VHD) görüntüsü kullanarak yeniden dağıtmak için Azure Stack ana bilgisayarı hazırlamak için bu modülü kullanın. | Bulut operatörleri|
| [Azure Stack'e bağlanma](azure-stack-powershell-install.md) | Azure Stack için VPN bağlantısı yapılandırmak için bu modülü kullanın. | Bulut operatörleri ve kullanıcıları |
| [Şablon Doğrulayıcı](../user/azure-stack-validate-templates.md) | Var olan veya yeni bir şablonu Azure Stack'e yerleştirilip yerleştirilemediğini doğrulamak için bu modülü kullanın. | Bulut operatörleri ve kullanıcıları|


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](../user/azure-stack-powershell-configure-user.md)   
* [Azure Stack Geliştirme Seti için bir VPN üzerinden bağlanan](../asdk/asdk-connect.md)  
