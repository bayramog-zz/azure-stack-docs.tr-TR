---
title: GitHub 'dan Azure Stack araçları indirin | Microsoft Docs
description: Azure Stack çalışmak için gereken araçların nasıl indirileceği hakkında bilgi edinin.
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
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: 9ca2e25217886d6b8a55bc394f4ee05df1bea2d6
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298775"
---
# <a name="download-azure-stack-tools-from-github"></a>GitHub 'dan Azure Stack araçları indirin

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

**Azurestack-Tools** , Azure Stack kaynakları yönetmek ve dağıtmak için PowerShell modüllerini barındıran bir [GitHub deposudur](https://github.com/Azure/AzureStack-Tools) . VPN bağlantısı kurmayı planlıyorsanız, bu PowerShell modüllerini Azure Stack Geliştirme Seti (ASDK) veya Windows tabanlı bir dış istemciye indirebilirsiniz. Bu araçları almak için GitHub deposunu kopyalayın veya aşağıdaki betiği çalıştırarak **Azurestack-Tools** klasörünü indirin:

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

**Azurestack-Tools** deposunda, Azure Stack için aşağıdaki Işlevleri destekleyen PowerShell modülleri vardır:  

| İşlev | Açıklama | Bu modülü kimler kullanabilir? |
| --- | --- | --- |
| [Bulut özellikleri](../user/azure-stack-validate-templates.md) | Bir bulutun bulut yeteneklerini almak için bu modülü kullanın. Örneğin, API sürümü ve Azure Resource Manager kaynakları gibi bulut olanakları edinebilirsiniz. Ayrıca, Azure Stack ve Azure bulutları için VM uzantılarını da alabilirsiniz. | Bulut İşletmenleri ve kullanıcıları |
| [Azure Stack için Kaynak Yöneticisi ilkesi](../user/azure-stack-policy-module.md) | Aynı sürüm oluşturma ve hizmet kullanılabilirliğine sahip bir Azure aboneliği veya Azure kaynak grubunu Azure Stack olarak yapılandırmak için bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları |
| [Azure 'a kaydolun](azure-stack-registration.md ) | ASDK örneğinizi Azure ile kaydetmek için bu modülü kullanın. Kaydolduktan sonra Azure Marketi öğelerini indirebilirsiniz Azure Stack ' de kullanabilirsiniz. | Bulut işleçleri |
| [Azure Stack dağıtımı](../asdk/asdk-install.md) | Bu modülü, Azure Stack sanal sabit disk (VHD) görüntüsünü kullanarak Azure Stack ana bilgisayarı dağıtmak ve yeniden dağıtmak üzere hazırlamak için kullanın. | Bulut işleçleri|
| [Azure Stack bağlanılıyor](azure-stack-powershell-install.md) | Azure Stack için VPN bağlantısını yapılandırmak üzere bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları |
| [Şablon doğrulayıcısı](../user/azure-stack-validate-templates.md) | Azure Stack için mevcut veya yeni bir şablon dağıtılıp dağıtılmadığını doğrulamak için bu modülü kullanın. | Bulut İşletmenleri ve kullanıcıları|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerinde PowerShell 'i kullanmaya](../user/azure-stack-powershell-overview.md)başlayın.
- [Azure Stack kullanıcının PowerShell ortamını yapılandırın](../user/azure-stack-powershell-configure-user.md).
- [VPN üzerinden Azure Stack geliştirme seti bağlama](../asdk/asdk-connect.md).
