---
title: GitHub 'dan Azure Stack araçları indirin | Microsoft Docs
description: Azure Stack ile çalışmak için gereken araçların nasıl indirileceği hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/18/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/18/2019
ms.openlocfilehash: c6939a28b150073d08a4f8c8dbc2d15dfd153957
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100934"
---
# <a name="download-azure-stack-tools-from-github"></a>GitHub 'dan Azure Stack araçları indirin

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

**Azurestack-Tools** , Azure Stack kaynakları yönetmek ve dağıtmak için PowerShell modüllerini barındıran bir [GitHub deposudur](https://github.com/Azure/AzureStack-Tools) . VPN bağlantısı kurmayı planlıyorsanız, bu PowerShell modüllerini Azure Stack Geliştirme Seti veya Windows tabanlı bir dış istemciye indirebilirsiniz. Bu araçları edinmek için GitHub deposunu kopyalayın veya aşağıdaki betiği çalıştırarak **Azurestack-Tools** klasörünü indirin:

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

## <a name="functionality-provided-by-the-modules"></a>Modüller tarafından sunulan işlevsellik

**Azurestack-Tools** deposu, Azure Stack için aşağıdaki Işlevleri destekleyen PowerShell modülleri içerir:  

| İşlevi | Açıklama | Bu modülü kimler kullanabilir? |
| --- | --- | --- |
| [Bulut özellikleri](../user/azure-stack-validate-templates.md) | Bir bulutun bulut yeteneklerini almak için bu modülü kullanın. Örneğin, bu modülü kullanarak, API sürümü ve Azure Resource Manager kaynakları gibi bulut özelliklerini alabilirsiniz. Ayrıca, bu modülü kullanarak Azure Stack ve Azure bulutları için VM uzantılarını da edinebilirsiniz. | Bulut İşletmenleri ve kullanıcıları |
| [Azure Stack için Kaynak Yöneticisi ilkesi](../user/azure-stack-policy-module.md) | Aynı sürüm oluşturma ve hizmet kullanılabilirliğine sahip bir Azure aboneliği veya Azure kaynak grubunu Azure Stack olarak yapılandırmak için bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları |
| [Azure 'a kaydolun](azure-stack-registration.md ) | Geliştirme Seti örneğinizi Azure ile kaydetmek için bu modülü kullanın. Kaydolduktan sonra Azure 'dan Market öğelerini indirebilir ve bunları Azure Stack kullanabilirsiniz. | Bulut işleçleri |
| [Azure Stack dağıtımı](../asdk/asdk-install.md) | Bu modülü, Azure Stack sanal sabit disk (VHD) görüntüsünü kullanarak Azure Stack ana bilgisayarı dağıtmak ve yeniden dağıtmak üzere hazırlamak için kullanın. | Bulut işleçleri|
| [Azure Stack bağlanılıyor](azure-stack-powershell-install.md) | Azure Stack için VPN bağlantısını yapılandırmak üzere bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları |
| [Şablon doğrulayıcısı](../user/azure-stack-validate-templates.md) | Azure Stack için mevcut veya yeni bir şablon dağıtılıp dağıtılmadığını doğrulamak için bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları|


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack 'de PowerShell 'i kullanmaya başlama](../user/azure-stack-powershell-overview.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](../user/azure-stack-powershell-configure-user.md)   
- [VPN üzerinden Azure Stack Geliştirme Seti bağlanma](../asdk/asdk-connect.md)  
