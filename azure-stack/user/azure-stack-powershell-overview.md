---
title: Azure Stack üzerinde PowerShell | Microsoft Docs
description: Azure Stack'te PowerShell modülleri ve bağlamları birçok vardır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 0cef39147fdbc62fe0652b1e387aa23f5ecb8487
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782879"
---
# <a name="get-started-with-powershell-on-azure-stack"></a>Azure Stack de PowerShell ile çalışmaya başlama

PowerShell komut satırından kaynaklarını yönetmek ve için tasarlanmıştır. Azure Resource Manager modelini kullanan Otomatikleştirilmiş araçları oluşturmak istediğinizde PowerShell kullanabilirsiniz. Bir PowerShell modülü, belirli bir alandaki tüm yönlerini yönetmek için gruplandırılır PowerShell işlevler bir dizi tanımlanabilir. Azure Stack ile çalışmak için PowerShell cmdlet'leri farklı kümesini juggle gerekecektir.

Bu makalede orient kendiniz Azure Stack üzerinde kullanılan farklı bu PowerShell modülleri için almanıza yardımcı olur. Azure Stack üzerinde PowerShell kullanırken etkileşim kurabileceğiniz API dört kümesi vardır.

| API | PowerShell başvurusu | REST başvurusu |
| --- | --- | --- |
| 1. Küresel Azure Resource Manager | [Azure PowerShell modülleri](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API tarayıcısı](https://docs.microsoft.com/rest/api/) |
| 2. Azure Stack Kaynak Yöneticisi | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) |
| 3. Azure Stack yönetici uç noktaları | [Azure Stack yönetici Modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [Azure Stack API Browser - REST](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| 4.  Azure Stack ayrıcalıklı uç noktası | [Azure Stack'te ayrıcalıklı uç noktayı (PEP) kullanma](../operator/azure-stack-privileged-endpoint.md) | |

Her arabirim kaynak sağlayıcıları genel Azure'da veya Azure Stack ile iletişim kurar. Kaynak sağlayıcıları, Azure özelliklerini etkinleştirin. Örneğin, Azure işlem kaynak sağlayıcısı, programlı erişim oluşturma ve Yönetim sanal makineleri ve destekleyici kaynakları sağlar.

Kaynak sağlayıcıları işlevselliği sağlar, ancak aynı zamanda kaynak yapılandırma ve yönetme için denetimleri sağlar. Kaynak sağlayıcılarını Azure Resource Manager kullanarak program aracılığıyla erişebileceğiniz ve sırasıyla arabirimi PowerShell, Azure CLI ve kendi REST istemcileri için bir yüzeyi sağlar.

## <a name="where-to-find-azure-stack-powershell"></a>Azure Stack PowerShell nerede bulacağını

Aşağıdaki blok diyagramını PowerShell modüllerinin her kümesi arasındaki ilişkiyi gösterir. Şunu PowerShell modüllerini yükleyin ve genel Azure ve Azure Stack yönetin.

![Azure Stack Powershell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="global-azure"></a>Küresel Azure

Azure PowerShell, Azure kaynaklarınızla çalışmak için Azure Resource Manager'ın geçerli sürümü kullanan cmdlet'ler kümesi içerir. Azure PowerShell, .NET Standard kullanır. Bu, Windows, macOS ve Linux sürümleri kullanabileceğiniz anlamına gelir. Azure PowerShell, Azure Cloud Shell üzerinde de kullanılabilir. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi

Azure Stack PowerShell'i Azure Resource Manager'ın önceki sürümlerini kullanan cmdlet'ler kümesi sağlar. Bu cmdlet'ler, Azure Stack kaynak sağlayıcıları ile uyumludur. Her kaynak sağlayıcısı Azure Stack'te genel Azure'da bulunan sağlayıcı daha eski bir sürümünü kullanır. Azure yığını tarafından desteklenen her bir sağlayıcı sürümünü koordine yardımcı olmak için API profillerini kullanabilirsiniz. Azure Stack PowerShell PowerShell 5.1 kullanır ve yalnızca Windows üzerinde kullanılabilir. Daha fazla bilgi için [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

### <a name="azure-stack-administrator"></a>Azure Stack yönetici

Azure Stack kaynak sağlayıcılarını yüklemek ve bakımını Azure Stack için bulut işleci bir dizi kullanıma sunar. Küresel Azure Bu etkileşim kullanıcıdan soyutlanır ve Azure'nın bir parçası olarak arka planda işlenen. Azure yığını, kuruluşlara ancak özel bir bulut destekleme özelliği sağlar. Bu görevleri gerçekleştirmek için işleç Azure Stack yönetici API'leri ile etkileşime girer. Daha fazla bilgi için [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md).

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack ayrıcalıklı uç noktası

Yükleme testi ve günlükleri, erişim gibi Azure Stack'te işleci etkinlikler için işleçleri (CESARETLENDİRİCİ) ayrıcalıklı uç ile etkileşim kurabilir. CESARETLENDİRİCİ belirli görevleri gerçekleştirmeye yardımcı olmak için yeterli erişimi yalnızca işleçleri sağlar bir önceden yapılandırılmış Uzaktan PowerShell Konsolu ' dir. Uç nokta, kısıtlı bir dizi cmdlet kullanıma sunmak için PowerShell JEA (yeterli yönetim) kullanır. Daha fazla bilgi için [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](../operator/azure-stack-privileged-endpoint.md).

### <a name="azurestack-tools"></a>AzureStack araçları

Ayrıca, Azure Stack, betikler ve ek cmdlet kullanılabilir bir GitHub deposu, Azure Stack araçları sağlar. AzureStack araçları, yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındırır. VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci indirebilirsiniz. Daha fazla bilgi için [AzureStack Araçları](https://github.com/Azure/AzureStack-Tools).

## <a name="working-with-powershell-on-azure-stack"></a>PowerShell ile Azure Stack üzerinde çalışma

PowerShell, Azure Resource Manager ile etkileşim kurmak için programlı bir şekilde sağlar. Etkileşimli bir komut istemi ile iş veya görevleri otomatikleştirme, betikler.

Azure Stack PowerShell ile çalışmayı çok vakit, kendiniz yükleme ve modüllerin yeniden bulabilirsiniz. Aynı zamanda genel Azure ile çalışıyorsanız kaldırıp modüllerinizi hedef bağlı olarak yeniden yüklemeniz gerekeceğinden bu zor olabilir. Docker kapsayıcıları her PowerShell sürümünü yerel makinenizde yalıtmak için kullanabilirsiniz. [PowerShell'i çalıştırmak için Docker'ı kullanma](azure-stack-powershell-user-docker.md) PowerShell modülü set için PowerShell modülünden geçiş yapabilir, böylece Docker kapsayıcılarını kullanarak arar.


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [PowerShell için API profillerini](azure-stack-version-profiles.md) Azure Stack üzerinde.
- [Azure Stack PowerShell'i yükleme](../operator/azure-stack-powershell-install.md)
- Oluşturma hakkında bilgi [Azure Resource Manager şablonları](azure-stack-develop-templates.md) bulut tutarlılık için.