---
title: Azure Stack bir geliştirme ortamı ayarlama | Microsoft Docs
description: Azure Stack için uygulama geliştirmeye başlayın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 437963355c8077c31ca5f0f55acdd344bb0060e2
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955716"
---
# <a name="set-up-a-development-environment-in-azure-stack"></a>Azure Stack bir geliştirme ortamı ayarlama 

Windows 10, Linux veya macOS Workstation kullanarak Azure Stack uygulamalar geliştirebilirsiniz. Bu makalede şu makaleye göz atacağız: 

- Uygulamanızın Azure Stack çalıştığı çeşitli bağlamlar. 
- Windows 10, Linux veya macOS iş istasyonu ile kurulumunu sizin için izlemeniz gereken adımlar. 
- Azure Stack içinde kaynak oluşturma ve bunları bir uygulamaya dağıtma adımları. 

## <a name="azure-stack-context-and-your-code"></a>Azure Stack bağlamı ve kodunuz 

Azure Stack çok sayıda görevi gerçekleştirmek için betikler ve uygulamalar yazabilirsiniz. Bununla birlikte, kapsamınızı aşağıdaki üç moddan sınırlamak yararlı olur: 

1. İlk modda, Azure Resource Manager şablonları kullanarak Azure Stack kaynak sağlayan uygulamalar oluşturabilirsiniz. Örneğin, bir sanal ağ ve uygulamanızı barındıracak VM 'Ler oluşturan bir Azure Resource Manager şablonu oluşturan bir betik yazabilirsiniz. 

2. İkinci modda, REST API ve kodunuzda oluşturulan bir REST istemcisini kullanarak doğrudan uç noktalarla çalışırsınız. Bu modda, API 'lere istek göndererek sanal ağ ve VM 'Ler oluşturan bir betik yazacaksınız. 

3. Üçüncü modda, Azure Stack ' de barındırılan bir uygulama oluşturmak için kodunuzu kullanabilirsiniz. Uygulamanızı barındırmak için Azure Stack altyapıyı oluşturduktan sonra, uygulamanızı altyapıya dağıtırsınız. Normalde, ortamınızı hazırlarsınız ve uygulamanızı bu ortama dağıtırsınız. 

###  <a name="infrastructure-as-a-service-and-platform-as-a-service"></a>Hizmet olarak altyapı ve hizmet olarak platform 

Bulut platformu ürünü olarak Azure Stack her ikisini de destekler: 

- Hizmet olarak altyapı (IaaS) 
- Hizmet olarak platform (PaaS) 

IaaS ve PaaS, geliştirme makinenizi nasıl ayarlayabileceğini bildirir. 

IaaS, ağ dişli, ağ ve sunuculardan gelen veri merkezinin parçalarını sanallaştırmadır. Bir uygulamayı Web sunucusunu barındıran bir VM 'ye dağıtırken, bir IaaS modelinde çalışıyorsunuz. Bu modelde, sanal dişli 'yi Azure Stack yönetir ve uygulamanız bir sanal sunucu üzerinde. Azure Stack kaynak sağlayıcıları ağ bileşenlerini ve sanal sunucuları destekler. 

PaaS, uygulamanızı çalıştıran bir uç noktaya dağıtmanız için altyapı katmanını soyutlar. PaaS modelinde, uygulamanızı barındırmak için kapsayıcıları kullanabilir ve Kapsayıcılı uygulamayı kapsayıcıyı çalıştıran bir hizmete dağıtabilirsiniz. Ya da uygulamanızı, uygulamayı çalıştıran bir hizmete doğrudan gönderebilirsiniz. Azure App Service ve Kubernetes 'i çalıştırmak için Azure Stack kullanabilirsiniz. 

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi 

Daha önce bahsedilen üç mod ve PaaS veya IaaS, Azure Resource Manager Azure Stack sürümü tarafından etkinleştirilir. Bu yönetim çerçevesi Azure Stack kaynaklarını dağıtmanıza, yönetmenize ve izlemenize olanak tanır. Tek bir işlemde kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Azure Stack Kaynak Yöneticisi çalışma hakkında daha fazla bilgi için bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md). 

### <a name="azure-stack-sdks"></a>Azure Stack SDK 'lar 

Azure Stack, Azure Resource Manager Azure Stack bir sürümünü kullanır. Seçtiğiniz kodu kullanarak Azure Stack Kaynak Yöneticisi çalışmanıza yardımcı olmak için aşağıdakiler dahil olmak üzere birkaç SDK sunuyoruz: 

- [NETC#](azure-stack-version-profiles-net.md)
- [Java](azure-stack-version-profiles-java.md)
- [Go](azure-stack-version-profiles-go.md)
- [Ruby](azure-stack-version-profiles-ruby.md)
- [Python](azure-stack-version-profiles-python.md)
- [Node.js](azure-stack-version-profile-nodejs.md)

## <a name="before-you-start"></a>Başlamadan önce 

Ortamınızı ayarlamaya başlamadan önce şunları yapmanız gerekir: 

- Azure Stack Kullanıcı portalına erişim. 
- Kiracınızın adı. 
- Kimlik yöneticiniz olarak Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanıp kullanmayacağınızı öğrenmek için. 

Azure Stack hakkında sorularınız varsa, bulut işleçle iletişim kurun. 

## <a name="windows-10"></a>Windows 10 

Bir Windows 10 makinesi kullanıyorsanız, PowerShell 5,0 ve Visual Studio ile çalışabilirsiniz. Azure Stack Geliştirme Seti (ASDK) ile çalışıyorsanız, ortamınıza VPN bağlantısıyla bağlanabilirsiniz. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlama 

1. PowerShell ile ayarlama. Yönergeler için bkz. [ınstall Azure Stack PowerShell](../operator/azure-stack-powershell-install.md). 

2. Azure Stack araçlarını indirin. Yönergeler için bkz. [GitHub 'dan Azure Stack araçları indirme](../operator/azure-stack-powershell-download.md). 

3. Bir ASDK kullanıyorsanız, [Azure Stack için bir VPN bağlantısı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)yükleyip yapılandırın. 

4. Azure CLı 'yı yükleyip yapılandırın. Yönergeler için bkz. [Azure Stack Azure CLI Ile API sürüm profillerini kullanma](azure-stack-version-profiles-azurecli2.md). 

5. Azure Depolama Gezgini yükleyip yapılandırın. Depolama Gezgini, Azure Stack Depolama verileriyle çalışmanıza olanak sağlayan tek başına bir uygulamadır. Yönergeler için bkz. [Azure Stack aboneliğine veya depolama hesabına bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınızı yükler 

1. Kod tabanınıza ve tercihlerinize bağlı olarak tümleşik geliştirme ortamınızı (IDE) yükler. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code [Code.VisualStudio.com](https://code.visualstudio.com/Download)adresinden indirin. 
     - Visual Studio (.NET/C#). Visual Studio Community Edition 'ı [VisualStudio.Microsoft.com](https://visualstudio.microsoft.com/vs/community/)adresinden indirin. 
     - Tutulma (Java). [Eclipse.org](https://www.eclipse.org/downloads/)'ten tutulma indirin. 

2. Kodunuz için SDK 'Yı yükler: 

     - [NETC#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="linux"></a>Linux 

Bir Linux makinesi kullanıyorsanız, Azure CLı, Visual Studio Code veya kendi tercih ettiğiniz tümleşik geliştirme ortamınızla çalışabilirsiniz. 

> [!Note]   
> ASDK ile bir Linux makinesi kullanıyorsanız, uzak makinenizin ASDK ile aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanamazsınız. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlama 

1. Azure CLı 'yı yükleyip yapılandırın. Yönergeler için bkz. [Azure Stack Azure CLI Ile API sürüm profillerini kullanma](azure-stack-version-profiles-azurecli2.md). 

2. Azure Depolama Gezgini yükleyip yapılandırın. Depolama Gezgini, Azure Stack Depolama verileriyle çalışmanıza olanak sağlayan tek başına bir uygulamadır. Yönergeler için bkz. [Azure Stack aboneliğine veya depolama hesabına bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınızı yükler 

1. Kod tabanınıza ve tercihlerinize bağlı olarak tümleşik geliştirme ortamınızı (IDE) yükler. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code [Code.VisualStudio.com](https://code.visualstudio.com/Download)adresinden indirin. 
     - Visual Studio (.NET/C#). Visual Studio Community Edition 'ı [VisualStudio.Microsoft.com](https://visualstudio.microsoft.com/vs/community/)adresinden indirin. 
     - Tutulma (Java). [Eclipse.org](https://www.eclipse.org/downloads/)'ten tutulma indirin. 

2. Kodunuz için SDK 'Yı yükler: 

     - [NETC#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="macos"></a>macOS 

MacOS makinesi, Azure CLı ve Visual Studio Code veya kendi tercih ettiğiniz tümleşik geliştirme ortamınız ile çalışmanıza olanak sağlayacak. 

> [!Note]   
> ASDK ile bir macOS makinesi kullanıyorsanız, uzak makinenizin ASDK ile aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanamazsınız. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlama 

1. Azure CLı 'yı yükleyip yapılandırın. Yönergeler için bkz. [Azure Stack Azure CLI Ile API sürüm profillerini kullanma](azure-stack-version-profiles-azurecli2.md). 

2. Azure Depolama Gezgini yükleyip yapılandırın. Depolama Gezgini, Azure Stack Depolama verileriyle çalışmanıza olanak sağlayan tek başına bir uygulamadır. Yönergeler için bkz. [Azure Stack aboneliğine veya depolama hesabına bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınızı yükler 

1. Kod tabanınıza ve tercihlerinize bağlı olarak tümleşik geliştirme ortamınızı (IDE) yükler. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code [Code.VisualStudio.com](https://code.visualstudio.com/Download)adresinden indirin. 
     - Visual Studio (.NET/C#). Visual Studio Community Edition 'ı [VisualStudio.Microsoft.com](https://visualstudio.microsoft.com/vs/community/)adresinden indirin. 
     - Tutulma (Java). [Eclipse.org](https://www.eclipse.org/downloads/)'ten tutulma indirin. 

2. Kodunuz için SDK 'Yı yükler: 

     - [NETC#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md)
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="next-steps"></a>Sonraki adımlar 

Azure Stack bir uygulamayı kaynaklara dağıtmak için, bkz. [Azure Stack Için ortak dağıtımlar](azure-stack-dev-start-deploy-app.md).
