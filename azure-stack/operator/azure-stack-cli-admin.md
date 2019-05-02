---
title: Azure Stack kullanıcıları için Azure CLI'yi etkinleştirme | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) yönetmek ve Azure Stack'te kaynakları dağıtmak için kullanmayı öğrenin
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
origin.date: 01/15/2019
ms.date: 04/29/2019
ms.author: v-jay
ms.lastreviewed: 01/15/2019
ms.openlocfilehash: efec1cc58b263c9580ab4a8ab3d7973a8f230d93
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64290357"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Azure Stack kullanıcıları için Azure CLI'yi etkinleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Böylece kendi geliştirme makinelerinde Azure CLI'yi kullanabilirsiniz, Azure Stack kullanıcıları için CA kök sertifikasını sağlayabilirsiniz. Kullanıcılarınızın CLI aracılığıyla kaynaklarını yönetmek için sertifika gerekir.

 - **Azure Stack CA kök sertifikasını** kullanıcılar Azure Stack geliştirme Seti'ni dışında bir iş istasyonundan CLI kullanıyorsanız gereklidir.  

 - **Sanal makine diğer uç nokta** "UbuntuLTS" veya "bir görüntü yayımcısı, teklif, SKU ve sürüm Vm'leri dağıtırken tek bir parametre başvuran Win2012Datacenter," gibi bir diğer ad sağlar.  

Aşağıdaki bölümlerde, bu değerleri almak nasıl açıklanmaktadır.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını dışarı aktarma

Tümleşik bir sistem kullanıyorsanız, CA kök sertifikasını dışarı aktarmanız gerekmez. Bir Azure Stack geliştirme Seti'ni (ASDK) üzerinde CA kök sertifikasını dışarı aktarmanız gerekir.

PEM biçiminde ASDK kök sertifikasını dışarı aktarmak için oturum açın ve aşağıdaki betiği çalıştırın:

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

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer uç nokta ayarlamayı

Azure Stack operatörlerinin sanal makine diğer dosyasını barındıran bir genel olarak erişilebilir uç ayarlamanız gerekir. Sanal makine diğer dosyasının bir görüntü için ortak bir ad sağlar bir JSON dosyasıdır. Azure CLI parametresi olarak bir VM dağıtırken adı kullanacaksınız.  

Bir diğer ad dosyasına bir giriş eklemeden önce emin olun, [görüntüleri Azure Market'te indirme](azure-stack-download-azure-marketplace-item.md) veya [kendi özel görüntünüzü yayımlanan](azure-stack-add-vm-image.md). Özel bir görüntü yayımlarsanız, yayımlama sırasında belirtilen yayımcı, teklif, SKU ve sürüm bilgileri not edin. Marketten bir görüntü varsa kullanarak bilgileri görüntüleyebilir ```Get-AzureVMImage``` cmdlet'i.  

A [örnek diğer ad dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) birçok ortak görüntü ile diğer adları kullanılabilir. Bu bir başlangıç noktası olarak kullanabilirsiniz. Bu dosyada burada CLI istemcilerinize erişebileceği bir boşluk barındırın. Bir dosyayı blob depolama hesabında barındırmak ve URL'yi Kullanıcılarınızla paylaşın yoludur:

1. İndirme [örnek dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) github'dan.
2. Azure Stack'te bir depolama hesabı oluşturun. Tamamlandığında, bir blob kapsayıcı oluşturun. "Genel" erişim ilkesi ayarlayın  
3. JSON dosyasını yeni kapsayıcısına yükleyin. Bu yapıldığında blobun URL'sini görüntüleyebilirsiniz. Blob adı belirledikten sonra URL'yi blob özelliklerini seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure CLI ile dağıtma](../user/azure-stack-deploy-template-command-line.md )
- [PowerShell ile bağlanma](azure-stack-powershell-install.md)
- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
