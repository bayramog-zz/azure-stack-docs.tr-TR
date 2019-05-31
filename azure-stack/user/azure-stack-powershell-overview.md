---
title: Azure Stack'te PowerShell | Microsoft Docs
description: Azure stack'teki PowerShell modülleri ve bağlamları sahiptir.
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
ms.openlocfilehash: 8f5d97969c96e8f3546d37ffed28b6332d80dc05
ms.sourcegitcommit: 2ee75ded704e8cfb900d9ac302d269c54a5dd9a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394416"
---
# <a name="get-started-with-powershell-in-azure-stack"></a>Azure stack'teki PowerShell ile çalışmaya başlama

PowerShell komut satırından kaynaklarını yönetmek ve için tasarlanmıştır. Azure Resource Manager modelini kullanan Otomatikleştirilmiş araçları oluşturmak istediğinizde PowerShell kullanabilirsiniz. Bir PowerShell modülü, belirli bir alandaki tüm yönlerini yönetmek için gruplandırılır PowerShell işlevler bir dizi tanımlanabilir. Azure Stack ile çalışmak için çeşitli PowerShell cmdlet'leri kümesi juggle gerekir.

Bu makale kendiniz için çeşitli Azure Stack'te kullanılan PowerShell modüllerinin yönlendirmenize yardımcı olur. Azure Stack'te PowerShell kullandığınızda, tüm API'leri, dört kümesi aşağıdaki tabloda gösterildiği gibi etkileşim kurabilirsiniz:

| API | PowerShell başvurusu | REST başvurusu |
| --- | --- | --- |
| Küresel Azure Resource Manager | [Azure PowerShell modülleri](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API tarayıcısı](https://docs.microsoft.com/rest/api/) |
| Azure Stack Kaynak Yöneticisi | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) | [Azure stack'teki API sürümü profillerini yönetme](azure-stack-version-profiles.md) |
| Azure Stack yönetici uç noktaları | [Azure Stack yönetici Modülü](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [REST API tarayıcısı - Azure Stack](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| Azure Stack ayrıcalıklı uç noktası | [Azure Stack'te ayrıcalıklı uç noktası kullanma](../operator/azure-stack-privileged-endpoint.md) | |

Her arabirim kaynak sağlayıcıları genel Azure'da veya Azure Stack ile iletişim kurar. Kaynak sağlayıcıları, Azure özelliklerini etkinleştirin. Örneğin, Azure bilgi işlem kaynak sağlayıcısı, programlı erişim oluşturma ve Yönetim sanal makineleri ve destekleyici kaynakları sağlar.

Kaynak sağlayıcıları, kaynak yapılandırma ve yönetme için hem işlevselliği ve denetimleri sağlar. Kaynak sağlayıcıları, Azure Resource Manager kullanarak program aracılığıyla erişebilir. Buna karşılık arabirimi, PowerShell, Azure CLI ve kendi REST istemcileri için bir yüzeyi sağlar.

## <a name="where-to-find-azure-stack-powershell"></a>Azure Stack PowerShell nerede bulacağını

Aşağıdaki blok diyagramını PowerShell modülleri kümeleri arasındaki ilişkiler gösterilmektedir. Şunu PowerShell modüllerini yükleyin ve genel Azure ve Azure Stack yönetin.

![Azure Stack Powershell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="global-azure"></a>Genel Azure

Azure PowerShell, Azure kaynaklarınızla çalışmak için Azure Resource Manager'ın geçerli sürümü kullanan cmdlet'ler kümesi içerir. Azure PowerShell ile Windows, macOS ve Linux'ta PowerShell sürümlerinin kullanabileceğiniz anlamına gelir .NET Standard sürümünü kullanır. Azure PowerShell, Azure Cloud Shell üzerinde de kullanılabilir. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi

Azure Stack PowerShell'i Azure Resource Manager'ın önceki sürümlerini kullanan cmdlet'ler kümesi sağlar. Bu cmdlet'ler, Azure Stack kaynak sağlayıcıları ile uyumludur. Her kaynak sağlayıcısı Azure Stack'te genel Azure'da bulunan sağlayıcı daha eski bir sürümünü kullanır. Azure yığını tarafından desteklenen her bir sağlayıcı sürümünü koordine yardımcı olmak için API profillerini kullanabilirsiniz. Azure Stack PowerShell PowerShell 5.1 kullanır ve yalnızca Windows üzerinde kullanılabilir. Daha fazla bilgi için [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

### <a name="azure-stack-administrator"></a>Azure Stack yönetici

Azure Stack operatörü yükleyebilir ve Azure Stack korumak kaynak sağlayıcıları için bulut işlecini bir dizi kullanıma sunar. Genel Azure, bu etkileşim kullanıcıdan soyutlanır ve Azure'nın bir parçası olarak arka planda işlenen. Azure Stack ile ancak bir özel bulut kuruluşlar destekleyebilir. Bu görevleri gerçekleştirmek için işleç Azure Stack yönetici API'leri ile etkileşime girer. Daha fazla bilgi için [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md).

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack ayrıcalıklı uç noktası

Yükleme testi ve günlükleri, erişim gibi Azure Stack operatörü etkinlikler için işleçleri (CESARETLENDİRİCİ) ayrıcalıklı uç ile etkileşim kurabilir. CESARETLENDİRİCİ işleçleri belirli görevleri gerçekleştirmek için yeterli erişim sağlayan bir önceden yapılandırılmış Uzaktan PowerShell konsoludur. Uç nokta, kısıtlı bir dizi cmdlet kullanıma sunmak için PowerShell yeterli yönetim (JEA) kullanır. Daha fazla bilgi için [ayrıcalıklı uç nokta Azure Stack'te kullanmak](../operator/azure-stack-privileged-endpoint.md).

### <a name="azure-stack-tools"></a>Azure Stack araçları

Azure Stack kullanımınıza betikleri ve diğer cmdlet'lerle bir GitHub deposunda *AzureStack Araçları*. AzureStack araçları, yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındırır. VPN bağlantısı kurmak planlıyorsanız, bu PowerShell modülleri Azure Stack geliştirme Seti'ni veya Windows tabanlı bir dış istemci indirebilirsiniz. Daha fazla bilgi için Git [AzureStack Araçları](https://github.com/Azure/AzureStack-Tools) sayfası.

## <a name="work-with-powershell-in-azure-stack"></a>Azure Stack PowerShell'de çalışın

PowerShell, Azure Resource Manager ile etkileşim kurmak için programlı bir şekilde sağlar. Etkileşimli bir komut istemi ile çalışabilir veya görevleri otomatik hale getirme, komut dosyaları yazabilirsiniz.

Azure Stack PowerShell ile çalışmayı çok vakit, kendiniz yükleme ve modüllerin yeniden bulabilirsiniz. Aynı zamanda genel Azure ile çalışıyorsanız, hedef bağlı olarak, modülleri kaldırıp gerekeceği için bu yordamı zorlu olabilir. 

Docker kapsayıcıları her PowerShell sürümünü yerel makinenizde yalıtmak için kullanabilirsiniz. PowerShell modülü set için PowerShell modülünden geçiş yapabilir, böylece Docker kapsayıcılarının kullanmak için bkz [PowerShell'i çalıştırmak için Docker kullanan](azure-stack-powershell-user-docker.md).


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [PowerShell için API profillerini](azure-stack-version-profiles.md) Azure Stack'te.
- Yükleme [Azure Stack PowerShell'e](../operator/azure-stack-powershell-install.md).
- Oluşturma hakkında bilgi [Azure Resource Manager şablonları](azure-stack-develop-templates.md) bulut tutarlılık için.
