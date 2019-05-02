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
ms.openlocfilehash: 9b3d58e67d7c6ca7016b3ecb51c09171aae7c06a
ms.sourcegitcommit: 0d8ccf2a32b08ab9bcbe13d54c7c3dce2379757f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64490027"
---
# <a name="set-up-a-development-environment-on-azure-stack"></a>Azure Stack'te bir geliştirme ortamı ayarlama 

Windows 10, Linux veya macOS iş istasyonu kullanarak Azure Stack için uygulamalar geliştirebilirsiniz. Bu makalede atacağız: 

- Uygulamanızı Azure Stack üzerinde çalıştığı çeşitli bağlamı. 
- Windows 10, Linux veya macOS iş istasyonu ile başlamanız için izleyebileceğiniz adımlar ayarlayın. 
- Azure Stack'te kaynak oluşturma adımları ve bir uygulama dağıttınız. 

## <a name="azure-stack-context-and-your-code"></a>Azure Stack bağlam ve kodunuz 

Betikleri ve Azure stack'teki her şeyi gerçekleştirebilirsiniz uygulamalar yazabilirsiniz. Bununla birlikte, üç farklı modlarını etmeye çalıştığınız sınırlamak yararlı olabilir. 

1. İlk modunda Azure Stack'te Azure Resource Manager şablonları kullanarak kaynakları sağlayacak uygulamalar oluşturabilirsiniz. Örneğin, bir sanal ağ ve uygulamanızı barındıracak Vm'leri oluşturacak bir Azure Resource Manager şablonu oluşturma yapacak bir betik oluşturabilirsiniz. 

2. İkinci modunda doğrudan REST API ve kodunuzda oluşturulan bir REST istemcisi kullanarak uç noktaları ile çalışırsınız. Bu modda, bir sanal ağ ve sanal makineleri için API'lerini istek göndererek oluşturan bir komut dosyası oluşturursunuz. 

3. Üçüncü modunda Azure Stack'te barındırılan bir uygulama oluşturmak için kodunuzu kullanabilirsiniz. Azure Stack'te uygulamanızı barındırmak için altyapı oluşturduktan sonra uygulamanızı altyapısını dağıtın. Genellikle, ortamınızı hazırlayın ve ardından söz konusu ortama uygulamanızı dağıtın. 

###  <a name="infrastructure-as-a-service-and-platform-as-a-service"></a>Hizmet olarak altyapı ve hizmet olarak Platform 

Bir bulut olarak Azure Stack her ikisini de destekler: 

- Hizmet olarak Altyapı (IaaS) 
- Hizmet Olarak Platform (PaaS) 

Hem Iaas ve PaaS geliştirme makinenizi kurmak nasıl istediğinizi bildirin. 

Iaas parçalarından biri ağ dişli, ağ ve sunuculardan veri merkezi sanallaştırma ' dir. Web sunucusunu barındıran bir VM için bir uygulamayı dağıttığınızda, bir Iaas programlamada çalışıyoruz. Bu paradigma, Azure Stack sanal dişli yönetir ve uygulamanızı bir sanal sunucuda olduğu. Azure Stack kaynak sağlayıcıları, ağ bileşenleriniz ve sanal sunucuları destekler. 

Uygulamanızı, sonra da uygulamanızın çalışacağı bir uç noktasına dağıtın, PaaS altyapısı katman soyutlar. PaaS programlamada olabileceğiniz uygulamanızı barındırmak için kapsayıcılar kullanın ve ardından kapsayıcıya alınmış uygulamanızı kapsayıcı çalışan bir hizmet dağıtmak veya doğrudan uygulamanız çalışırken bir hizmet için uygulamanızı göndermek olabilir. Azure Stack, Azure App service ve Kubernetes çalıştırmak için kullanılabilir. 

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi 

Bu üç moddan hem de PaaS veya Iaas Azure Stack sürümü, Azure Resource Manager tarafından etkinleştirilir. Yönetim çerçevesi, dağıtma, yönetme ve Azure Stack kaynakları izleme sağlar. Yönetici, bu öğeler ile tek bir işlemde bir grup olarak çalışmanıza olanak sağlar. Hakkında daha fazla bilgi için bkz. Azure Stack Kaynak Yöneticisi ile çalışma hakkında [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md). 

### <a name="azure-stack-development-kits"></a>Azure Stack geliştirme setleri 

Azure Stack, Azure Resource Manager'ın bir Azure Stack sürümünü kullanır.  Azure Stack kaynak kodunuzu tercih ettiğiniz kullanarak Yöneticisi ile çalışmanıza yardımcı olmak için birkaç yazılım geliştirme setleri sağladık. Bunlar: 

- [.NET /C#](azure-stack-version-profiles-net.md)
- [Java](azure-stack-version-profiles-java.md)
- [Go](azure-stack-version-profiles-go.md)
- [Ruby](azure-stack-version-profiles-ruby.md)
- [Python](azure-stack-version-profiles-python.md)

## <a name="before-you-start"></a>Başlamadan önce 

Şunlar gerekir: 

- Azure Stack kullanıcı portalına erişim. 
- Kiracınızın adını. 
- Azure Active directory (Azure AD) veya Active Directory Federasyon Hizmetleri'nde (AD FS), kimlik yöneticisi kullanıp kullanmadığınızı edinmek için. 

Azure Stack hakkında sorularınız varsa, bulut operatörünüze başvurun. 

## <a name="windows-10"></a>Windows 10 

Bir Windows 10 makinesi PowerShell 5.0, Visual Studio, iş ve bir ASDK ile çalışıyorsanız ortamınızı VPN bağlantısıyla bağlanmak için izin verir. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. PowerShell ile ayarlama yapın. Bağlantısındaki talimat için [Azure Stack Powershell yükleme](../operator/azure-stack-powershell-install.md). 

2. Azure Stack Araçları'nı indirin. Yönergeler için adımları [github'dan indirin, Azure Stack Araçları](../operator/azure-stack-powershell-download.md). 

3. Bir ASDK kullanıyorsanız, yükleme ve yapılandırma bir [Azure Stack için VPN bağlantısı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

4. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için adımları [Azure Stack'te Azure CLI ile kullanımı API Sürüm profillerini](azure-stack-version-profiles-azurecli2.md). 

5. Yükleyin ve Azure Depolama Gezgini yapılandırın. Azure Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır.  Yönergeler için adımları [Connect Depolama Gezgini'ni Azure Stack aboneliğine veya bir depolama hesabına](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.Net /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için Yazılım Geliştirme Seti (SDK) yükleyin. 

     - [.NET /C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="linux"></a>Linux 

Linux makinesi, Azure CLI ve Visual Studio Code veya kendi tercih ettiğiniz tümleşik geliştirme ortamı ile çalışmanıza olanak sağlar. 

> [!Note]   
> ASDK ile Linux makinesi kullanıyorsanız, uzak makinenizi ASDK aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanmak mümkün olmayacaktır. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için adımları [Azure Stack'te Azure CLI ile kullanımı API Sürüm profillerini](azure-stack-version-profiles-azurecli2.md). 

2. Yükleyin ve Azure Depolama Gezgini yapılandırın. Azure Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır.  Yönergeler için adımları [Connect Depolama Gezgini'ni Azure Stack aboneliğine veya bir depolama hesabına](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.Net /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için Yazılım Geliştirme Seti (SDK) yükleyin. 

     - [.NET /C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="macos"></a>macOs 

Bir macOS makinenizde Azure CLI ve Visual Studio Code veya kendi tercih ettiğiniz tümleşik geliştirme ortamı ile çalışmanıza olanak sağlar. 

> [!Note]   
> Bir macOS makinenizde ASDK ile kullanıyorsanız, uzak makinenizi ASDK aynı ağda olması gerekir. Bir sanal özel ağ bağlantısı kullanarak bağlanmak mümkün olmayacaktır. 

### <a name="set-up-your-tools"></a>Araçlarınızı ayarlayın 

1. Yükleyin ve Azure CLI'yı yapılandırın. Yönergeler için adımları [Azure Stack'te Azure CLI ile kullanımı API Sürüm profillerini](azure-stack-version-profiles-azurecli2.md). 

2. Yükleyin ve Azure Depolama Gezgini yapılandırın. Azure Depolama Gezgini ile Azure Stack depolama çalışmanızı sağlayan bir tek başına uygulamadır. Yönergeler için adımları [Connect Depolama Gezgini'ni Azure Stack aboneliğine veya bir depolama hesabına](azure-stack-storage-connect-se.md). 

### <a name="install-your-integrated-development-environment"></a>Tümleşik geliştirme ortamınıza yükleyin 

1. Tümleşik geliştirme ortamınız (IDE), kod tabanı ve tercihine bağlı olarak yükleyin. 

     - Visual Studio Code (Python, Go, NodeJS). Makinenizde Visual Studio Code'u indirin [code.visualstudio.com](https://code.visualstudio.com/Download). 
     - Visual Studio (.Net /C#). Visual Studio Community edition'ı indirin [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/). 
     - Eclipse (Java). Eclipse'ten indirme [eclipse.org](https://www.eclipse.org/downloads/). 

2. Kodunuz için Yazılım Geliştirme Seti (SDK) yükleyin. 

     - [.NET /C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="next-steps"></a>Sonraki adımlar 

Uygulama kaynakları Azure Stack'te dağıtın. Adımları bulabilirsiniz [ortak dağıtımları için Azure Stack](azure-stack-dev-start-deploy-app.md).