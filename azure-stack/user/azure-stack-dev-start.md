---
title: Azure Stack'te bir geliştirme ortamı ayarlama | Microsoft Docs
description: Azure Stack için uygulamalar geliştirmeye başlayın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 06c462ab46ce6bbae8d5c3bd6fcb757e14417edf
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65617629"
---
# <a name="set-up-a-development-environment-in-azure-stack"></a>Azure Stack geliştirme ortamında ayarlama 

Windows 10, Linux veya macOS iş istasyonu kullanarak Azure Stack için uygulamalar geliştirebilirsiniz. Bu makalede, şu konuları: 

- Uygulamanızı Azure Stack'te çalıştığı çeşitli bağlamı. 
- Windows 10, Linux veya macOS iş istasyonu ile almak için izlemeniz gereken adımları ayarlayın. 
- Azure Stack'te kaynakları oluşturmak için adımları ve uygulamayı dağıtırsınız. 

## <a name="azure-stack-context-and-your-code"></a>Azure Stack bağlam ve kodunuz 

Betikleri ve Azure Stack'te birçok görevi gerçekleştirmek için uygulamalar yazabilirsiniz. Ancak, aşağıdaki üç modun kapsamına sınırlamak yararlıdır: 

1. İlk modunda, Azure Resource Manager şablonları kullanarak Azure Stack'te kaynakları sağlamak uygulamalar oluşturabilirsiniz. Örneğin, bir sanal ağ ve uygulamanızı barındıracak Vm'leri sırayla oluşturan bir Azure Resource Manager şablonu oluşturan bir betik yazabilirsiniz. 

2. İkinci modunda, REST API ve kodunuzda oluşturulan bir REST istemcisi kullanarak doğrudan uç noktaları ile çalışır. Bu modda, bir sanal ağ ve sanal makineleri için API'lerini istek göndererek oluşturan bir betik yazmalısınız. 

3. Üçüncü modunda Azure Stack'te barındırılan bir uygulama oluşturmak için kodunuzu kullanabilirsiniz. Uygulamanızı barındırmak için Azure Stack'te altyapı oluşturduktan sonra uygulamanızı altyapısını dağıtın. Normalde, ortamınızı hazırlayın ve uygulamanızı dağıtın. 

###  <a name="infrastructure-as-a-service-and-platform-as-a-service"></a>Bir hizmet olarak platform ve hizmet olarak altyapı 

Azure Stack bulut platformu ürün olarak, her ikisini de destekler: 

- Hizmet olarak altyapı (IaaS) 
- Hizmet olarak platform (PaaS) 

Hem Iaas ve PaaS geliştirme makinenizi kurmak konusunda bilgilendirin. 

Iaas bölümlerinin ağ dişli, ağ ve sunucu gelen veri merkezi sanallaştırma ' dir. Web sunucusunu barındıran bir VM için bir uygulamayı dağıttığınızda, bir Iaas modeli çalışıyoruz. Bu modelde, Azure Stack sanal dişli yönetir ve uygulamanızı bir sanal sunucuda olduğu. Azure Stack kaynak sağlayıcıları, ağ bileşenleriniz ve sanal sunucuları destekler. 

Ardından uygulamayı çalıştıran bir uç noktaya uygulamanızı dağıtma, PaaS altyapısı katman soyutlar. PaaS modeli, uygulamanızı barındırmak ve ardından kapsayıcılı uygulama kapsayıcı çalışan bir hizmet dağıtmak için kapsayıcıları kullanabilirsiniz. Veya doğrudan uygulamanın çalışan hizmet için uygulamanızı göndermek olabilir. Azure Stack, Azure App Service ve Kubernetes çalıştırmak için kullanabilirsiniz. 

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi 

Üç modları, hem de PaaS veya Iaas, Azure Stack sürümü Azure Resource Manager tarafından etkinleştirilir daha önce bahsedilen. Bu yönetim çerçevesi, dağıtma, yönetme ve Azure Stack kaynakları izleme sağlar. Bu kaynaklarla tek bir işlemde bir grup olarak çalışmanıza olanak tanır. Azure Stack Kaynak Yöneticisi ile çalışma hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md). 

### <a name="azure-stack-sdks"></a>Azure Stack SDK'ları 

Azure Stack, Azure Resource Manager'ın bir Azure Stack sürümünü kullanır. Tercih ettiğiniz kodunuzu kullanarak Azure Stack Kaynak Yöneticisi ile çalışmanıza yardımcı olmak için bir dizi SDK sağladık: 

- [.NET/C#](azure-stack-version-profiles-net.md)
- [Java](azure-stack-version-profiles-java.md)
- [Go](azure-stack-version-profiles-go.md)
- [Ruby](azure-stack-version-profiles-ruby.md)
- [Python](azure-stack-version-profiles-python.md)

## <a name="before-you-start"></a>Başlamadan önce 

Ortamı ayarı başlamadan önce ihtiyacınız vardır: 

- Azure Stack kullanıcı portalına erişim. 
- Kiracınızın adını. 
- Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS), Identity manager kullandığınız olup olmadığını belirlemek için. 

Azure Stack hakkında sorularınız varsa, bulut operatörünüze başvurun. 

## <a name="windows-10"></a>Windows 10 

Bir Windows 10 makinesi kullanıyorsanız, PowerShell 5.0 ve Visual Studio ile çalışabilirsiniz. Ve bir Azure Stack geliştirme Seti'ni (ASDK) ile çalışıyorsanız, VPN bağlantısıyla ortamınıza bağlanabilirsiniz. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. PowerShell ile ayarlama yapın. Yönergeler için [Azure Stack Powershell yükleme](../operator/azure-stack-powershell-install.md). 

2. Azure Stack Araçları'nı indirin. Yönergeler için [github'dan indirin, Azure Stack Araçları](../operator/azure-stack-powershell-download.md). 

3. Bir ASDK kullanıyorsanız, yükleme ve yapılandırma bir [Azure Stack için VPN bağlantısı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

4. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için [Azure Stack'te Azure CLI ile kullanımı API sürümü profillerini](azure-stack-version-profiles-azurecli2.md). 

5. Yükleyin ve Azure Depolama Gezgini yapılandırın. Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır. Yönergeler için [bir depolama hesabına veya Azure Stack aboneliğine bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.NET /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için SDK'yı yükleyin: 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="linux"></a>Linux 

Linux makinesi kullanıyorsanız, Azure CLI, Visual Studio Code veya kendi tercih ettiğiniz tümleşik geliştirme ortamı ile çalışabilirsiniz. 

> [!Note]   
> ASDK ile Linux makinesi kullanıyorsanız, uzak makinenizi ASDK aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanmak mümkün olmayacaktır. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için [Azure Stack'te Azure CLI ile kullanımı API sürümü profillerini](azure-stack-version-profiles-azurecli2.md). 

2. Yükleyin ve Azure Depolama Gezgini yapılandırın. Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır. Yönergeler için [bir depolama hesabına veya Azure Stack aboneliğine bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.NET /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için SDK'yı yükleyin: 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="macos"></a>macOS 

MacOS makinenizde Visual Studio Code ve Azure CLI ile çalışmanıza olanak tanıyacak ya da kendi tercih edilen tümleşik geliştirme ortamı. 

> [!Note]   
> Bir macOS makinenizde ASDK ile kullanıyorsanız, uzak makinenizi ASDK aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanmak mümkün olmayacaktır. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için [Azure Stack'te Azure CLI ile kullanımı API sürümü profillerini](azure-stack-version-profiles-azurecli2.md). 

2. Yükleyin ve Azure Depolama Gezgini yapılandırın. Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır. Yönergeler için [bir depolama hesabına veya Azure Stack aboneliğine bağlanma Depolama Gezgini](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.NET /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için SDK'yı yükleyin: 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md)
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="next-steps"></a>Sonraki adımlar 

Kaynakları Azure Stack'te uygulama dağıtmak için bkz: [ortak dağıtımları için Azure Stack](azure-stack-dev-start-deploy-app.md).
