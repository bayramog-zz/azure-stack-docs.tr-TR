---
title: PowerShell Azure Stack | Microsoft Docs
description: Azure Stack içindeki PowerShell, birkaç modül ve bağlama sahiptir.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 1c2727562fe287c5450a4977803a7c6fce14f27a
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824281"
---
# <a name="get-started-with-powershell-in-azure-stack"></a>Azure Stack 'de PowerShell 'i kullanmaya başlama

PowerShell, kaynakları komut satırından yönetmek ve yönetmek için tasarlanmıştır. Azure Resource Manager modelini kullanan otomatikleştirilmiş araçları derlemek istediğinizde PowerShell 'i kullanabilirsiniz. Bir PowerShell modülü, belirli bir alanın tüm yönlerini yönetmek üzere gruplanmış bir PowerShell işlevleri kümesi olarak tanımlanabilir. Azure Stack çalışmak için çeşitli PowerShell cmdlet 'leri kümelerini kullanmanız gerekir.

Bu makale, kendinizi Azure Stack ' de kullanılan çeşitli PowerShell modüllerine yönemenize yardımcı olur. PowerShell 'i Azure Stack kullandığınızda, aşağıdaki tabloda gösterildiği gibi dört tür API kümesiyle etkileşime geçebilirsiniz:

| API | PowerShell başvurusu | REST başvurusu |
| --- | --- | --- |
| Genel Azure Resource Manager | [Azure PowerShell modüller](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API tarayıcısı](https://docs.microsoft.com/rest/api/) |
| Azure Stack Kaynak Yöneticisi | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) |
| Azure Stack yönetici uç noktaları | [Azure Stack Yönetici modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [REST API Browser-Azure Stack](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| Azure Stack ayrıcalıklı uç nokta | [Azure Stack 'da ayrıcalıklı uç noktayı kullanın](../operator/azure-stack-privileged-endpoint.md) | |

Her arabirim, genel Azure veya Azure Stack kaynak sağlayıcıları ile iletişim kurar. Kaynak sağlayıcıları, Azure yeteneklerini etkinleştirir. Örneğin, Azure Işlem kaynak sağlayıcısı, sanal makinelerin ve destekleyici kaynakların oluşturulması ve yönetilmesi için programlı erişim sağlar.

Kaynak sağlayıcıları, kaynağı yönetmek ve yapılandırmak için hem işlevleri hem de denetimleri sağlar. Kaynak sağlayıcılarına Azure Resource Manager kullanarak programlı bir şekilde erişebilirsiniz. Sırasıyla, arabirim PowerShell, Azure CLı ve kendi REST istemcileriniz için bir yüzey sağlar.

## <a name="where-to-find-azure-stack-powershell"></a>Azure Stack PowerShell nerede bulunur?

Aşağıdaki blok diyagramı, PowerShell modülleri kümeleri arasındaki ilişkileri gösterir. Makinenizden, PowerShell modüllerini yükleyebilir ve hem küresel Azure hem de Azure Stack yönetebilirsiniz.

![Azure Stack PowerShell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="global-azure"></a>Küresel Azure

Azure PowerShell, Azure kaynaklarınızla çalışmak için geçerli Azure Resource Manager sürümünü kullanan bir cmdlet kümesi içerir. Azure PowerShell, Windows, macOS ve Linux ile PowerShell sürümlerini kullanabileceğiniz anlamına gelen .NET Standard sürümünü kullanır. Azure PowerShell, Azure Cloud Shell üzerinde de kullanılabilir. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi

Azure Stack PowerShell, önceki Azure Resource Manager sürümlerini kullanan bir cmdlet kümesi sağlar. Bu cmdlet 'ler Azure Stack içindeki kaynak sağlayıcılarıyla uyumludur. Azure Stack içindeki her kaynak sağlayıcısı, genel Azure 'da bulunan sağlayıcının daha eski bir sürümünü kullanır. Azure Stack tarafından desteklenen her sağlayıcının sürümünü koordine etmenize yardımcı olmak için API profillerini kullanabilirsiniz. Azure Stack PowerShell, PowerShell 5,1 kullanır ve yalnızca Windows 'da kullanılabilir. Daha fazla bilgi için bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md).

### <a name="azure-stack-administrator"></a>Azure Stack Yöneticisi

Azure Stack, işlecin Azure Stack yükleyip koruyabilmesi için bulut işlecine bir kaynak sağlayıcıları kümesi sunar. Küresel Azure 'da bu etkileşim, kullanıcıdan soyutlanmıştır ve Azure 'un bir parçası olarak arka planda işlenir. Ancak Azure Stack, kuruluşlar özel bir bulutu destekleyebilir. Bu görevleri yapmak için, işleç Azure Stack yönetici API 'Leri ile etkileşime girer. Daha fazla bilgi için bkz. [Azure Stack Için PowerShell 'ı Install](../operator/azure-stack-powershell-install.md).

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack ayrıcalıklı uç nokta

Yüklemeyi test etme ve günlüklere erişme gibi Azure Stack operatör etkinlikleri için, işleçler ayrıcalıklı uç nokta (PEP) ile etkileşime geçebilir. PEP, operatörlere belirli görevleri gerçekleştirmek için yeterli erişim sağlayan, önceden yapılandırılmış bir uzak PowerShell konsoludur. Uç nokta, kısıtlanmış bir cmdlet kümesini kullanıma sunmak için PowerShell 'i yeterli yönetim (JEA) kullanır. Daha fazla bilgi için bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](../operator/azure-stack-privileged-endpoint.md).

### <a name="azure-stack-tools"></a>Azure Stack araçları

Azure Stack bir GitHub deposunda, *Azurestack-Tools*komut dosyalarını ve ek cmdlet 'leri kullanılabilir hale getirir. AzureStack-Tools, kaynakları yönetmek ve Azure Stack dağıtmak için PowerShell modülleri barındırır. VPN bağlantısı kurmayı planlıyorsanız, bu PowerShell modüllerini Azure Stack Geliştirme Seti veya Windows tabanlı bir dış istemciye indirebilirsiniz. Daha fazla bilgi için [Azurestack-Tools](https://github.com/Azure/AzureStack-Tools) sayfasına gidin.

## <a name="work-with-powershell-in-azure-stack"></a>Azure Stack 'de PowerShell ile çalışma

PowerShell Azure Resource Manager etkileşimde bulunmak için programlı bir yol sağlar. Etkileşimli bir komut istemiyle çalışabilirsiniz veya görevleri otomatikleştirmeye başladıysanız komut dosyaları yazabilirsiniz.

Azure Stack PowerShell ile çalışmaya çok zaman harcadıysanız, daha sonra modülleri yükleyip yeniden yüklediğinize ulaşabilirsiniz. Global Azure ile aynı anda çalışıyorsanız, bu yordam zor olabilir, çünkü modüllerinize bağlı olarak modüllerinizi kaldırmanız ve yeniden yüklemeniz gerekir. 

Her bir PowerShell sürümünü yerel makinenizde yalıtmak için Docker Kapsayıcıları kullanabilirsiniz. PowerShell modülünden PowerShell modülü kümesine geçiş yapabilmeniz için Docker kapsayıcılarını kullanmak üzere, bkz. [PowerShell 'i çalıştırmak Için Docker kullanma](azure-stack-powershell-user-docker.md).


## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack 'de [PowerShell Için API profilleri](azure-stack-version-profiles.md) hakkında bilgi edinin.
- [Azure Stack PowerShell](../operator/azure-stack-powershell-install.md)'i yükler.
- Bulut tutarlılığı için [Azure Resource Manager şablonları](azure-stack-develop-templates.md) oluşturma hakkında bilgi edinin.
