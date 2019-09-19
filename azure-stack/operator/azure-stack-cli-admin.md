---
title: Azure Stack kullanıcılar için Azure CLı 'yi etkinleştirme | Microsoft Docs
description: Platformlar arası komut satırı arabirimi 'ni (CLı) Azure Stack yönetmek ve dağıtmak için nasıl etkinleştirebileceğinizi öğrenin.
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
ms.date: 05/16/2019
ms.author: mabrigg
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 6e901b2d806e85f7bc394dc9bee6412270753649
ms.sourcegitcommit: c196463492732218d2474d3a964f88e995272c80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71094295"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure Stack kullanıcıları için Azure CLı 'yi etkinleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

CA kök sertifikasını, Azure Stack kullanıcılarına, geliştirme makinelerinde Azure CLı 'yi etkinleştirebilecekleri şekilde sağlayabilirsiniz. Kullanıcılarınızın CLı aracılığıyla kaynakları yönetmesi için sertifikaya ihtiyacı vardır.

 - Kullanıcılar, Azure Stack Geliştirme Seti (ASDK) dışında bir iş istasyonundan CLı kullanıyorsa, **Azure Stack CA kök sertifikası** gereklidir.  

 - **Sanal makine (VM) diğer adları uç noktası** , "UbuntuLTS" veya "Win2012Datacenter" gibi bir diğer ad sağlar, bu da VM 'leri dağıtımında tek bir parametre olarak bir görüntü yayımcısı, TEKLIF, SKU ve sürüme başvurur.  

Aşağıdaki bölümlerde bu değerlerin nasıl alınacağı açıklanır.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını dışarı aktarma

Tümleşik bir sistem kullanıyorsanız, CA kök sertifikasını dışarı aktarmanız gerekmez. CA kök sertifikasını ASDK 'ye dışarı aktarmanız gerekir.

ASK biçimindeki kök sertifikayı dışarı aktarmak için oturum açın ve aşağıdaki betiği çalıştırın:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Write-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-vm-aliases-endpoint"></a>VM diğer adları uç noktasını ayarlama

Azure Stack işleçler, bir VM diğer adı dosyasını barındıran, genel olarak erişilebilen bir uç nokta ayarlamalıdır. VM diğer adı dosyası, bir görüntü için ortak ad sağlayan bir JSON dosyasıdır. Bir sanal makineyi Azure CLı parametresi olarak dağıtırken bu adı kullanırsınız.  

Bir diğer ad dosyasına bir giriş eklemeden önce, [Azure Marketi 'nden görüntüleri indirdiğinizden](azure-stack-download-azure-marketplace-item.md) veya [kendi özel görüntünüzü yayımladığınızdan](azure-stack-add-vm-image.md)emin olun. Özel bir görüntü yayımlarsanız, yayımlama sırasında belirttiğiniz Yayımcı, teklif, SKU ve sürüm bilgilerini unutmayın. Market 'ten bir görüntüdür, bu bilgileri `Get-AzureVMImage` cmdlet 'ini kullanarak görüntüleyebilirsiniz.  

Birçok ortak görüntü diğer [adı içeren örnek bir diğer ad dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) kullanılabilir. Bunu, başlangıç noktası olarak kullanabilirsiniz. Bu dosyayı CLı istemcilerinizin ulaşabilmesi için bir alanda barındırın. Tek yol, dosyayı bir BLOB depolama hesabında barındırmanız ve URL 'YI kullanıcılarınız ile paylaşmalıdır:

1. [Örnek dosyayı](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) GitHub 'dan indirin.
2. Azure Stack ' de bir depolama hesabı oluşturun. Bu tamamlandığında bir blob kapsayıcısı oluşturun. Erişim ilkesini "genel" olarak ayarlayın.  
3. JSON dosyasını yeni kapsayıcıya yükleyin. Bu tamamlandığında, Blobun URL 'sini görüntüleyebilirsiniz. Blob adını seçin ve ardından blob özelliklerinden URL 'YI seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLı ile şablonları dağıtma](../user/azure-stack-deploy-template-command-line.md )
- [PowerShell ile bağlanma](azure-stack-powershell-install.md)
- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
