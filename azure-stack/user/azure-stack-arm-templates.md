---
title: Azure Stack Azure Resource Manager şablonları kullanma | Microsoft Docs
description: Kaynakları sağlamak için Azure Stack Azure Resource Manager şablonlarını nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: sethm
ms.reviewer: justini
ms.lastreviewed: 11/14/2018
ms.openlocfilehash: 0b9aedb4a6b046755192b84e18e8c75a8d015c8f
ms.sourcegitcommit: f91979c1613ea1aa0e223c818fc208d902b81299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974065"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Stack Azure Resource Manager şablonları kullanma

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0

Uygulamanıza yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtmak ve sağlamak için Azure Resource Manager şablonlarını kullanabilirsiniz. Ayrıca, bir kaynak grubundaki kaynaklarda değişiklik yapmak için şablonları yeniden dağıtabilirsiniz.

Bu Şablonlar Microsoft Azure Stack portalı, PowerShell, komut satırı ve Visual Studio ile dağıtılabilir.

[GitHub 'da](https://aka.ms/azurestackgithub)aşağıdaki hızlı başlangıç şablonları mevcuttur:

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>SharePoint Server 'ı dağıtma (yüksek kullanılabilirliğe sahip olmayan dağıtım)

Aşağıdaki kaynakları içeren [bir SharePoint Server 2013 grubu oluşturmak](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha) Için PowerShell [istenen durum yapılandırması](/powershell/dsc/overview/overview) (DSC) uzantısını kullanın:

* Bir sanal ağ
* Üç depolama hesabı
* İki dış yük dengeleyiciler
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış bir sanal makine (VM)
* SQL Server 2014 tek başına sunucu olarak yapılandırılmış bir VM
* Tek makineli SharePoint Server 2013 grubu olarak yapılandırılmış bir VM

## <a name="deploy-ad-non-high-availability-deployment"></a>AD dağıtma (yüksek kullanılabilirlik olmayan dağıtım)

PowerShell DSC uzantısını kullanarak aşağıdaki kaynakları içeren [BIR ad etki alanı denetleyicisi sunucusu oluşturun](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha) :

* Bir sanal ağ
* Bir depolama hesabı
* Bir dış yük dengeleyici
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış bir sanal makine (VM)

## <a name="deploy-adsql-non-high-availability-deployment"></a>AD/SQL dağıtma (yüksek kullanılabilirliğe sahip olmayan dağıtım)

PowerShell DSC uzantısını kullanarak aşağıdaki kaynakları içeren [SQL Server 2014 tek başına sunucu oluşturun](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha) :

* Bir sanal ağ
* İki depolama hesabı
* Bir dış yük dengeleyici
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış bir sanal makine (VM)
* SQL Server 2014 tek başına sunucu olarak yapılandırılmış bir VM

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-uzantı-Azure-Automation-çekme-sunucu

Mevcut bir sanal makine yerel Configuration Manager (LCM) yapılandırmak ve Azure Otomasyonu hesabı DSC çekme sunucusuna kaydetmek için PowerShell DSC uzantısını kullanın.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Kullanıcı görüntüsünden sanal makine oluşturma

[Özel bir Kullanıcı görüntüsünden sanal makine oluşturun](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-create-from-customimage). Bu şablon Ayrıca bir sanal ağ (DNS ile), genel IP adresi ve ağ arabirimi dağıtır.

## <a name="basic-virtual-machine"></a>Temel sanal makine

Sanal ağ (DNS ile), genel IP adresi ve ağ arabirimi içeren [bir Windows sanal makinesi dağıtın](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm) .

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımını iptal etme

Çalışan bir şablon dağıtımını iptal etmek için, [stop-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/stop-azurermresourcegroupdeployment) PowerShell [cmdlet 'ini](/powershell/developer/cmdlet/cmdlet-overview)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları portal ile dağıtma](azure-stack-deploy-template-portal.md)
* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
* [Azure Resource Manager’a genel bakış](/azure/azure-resource-manager/resource-group-overview)
