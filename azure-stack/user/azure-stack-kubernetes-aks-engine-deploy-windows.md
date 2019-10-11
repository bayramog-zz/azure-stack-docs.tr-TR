---
title: AKS altyapısını Windows üzerinde dağıtma Azure Stack | Microsoft Docs
description: Bir Kubernetes kümesini dağıtmak ve yönetmek üzere AKS altyapısını barındırmak için Azure Stack bir Windows makinesini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 10/09/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 10/09/2019
ms.openlocfilehash: 9c19047629d95ba1ffe7c0fe6ab2da5c745576a0
ms.sourcegitcommit: 12034a1190d52ca2c7d3f05c8c096416120d8392
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037933"
---
# <a name="install-the-aks-engine-on-windows-in-azure-stack"></a>AKS altyapısını Windows üzerinde Azure Stack

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0

Bir Kubernetes kümesini dağıtmak ve yönetmek üzere AKS altyapısını barındırmak için Azure Stack bir Windows makinesini kullanabilirsiniz. Bu makalede, hem bağlı hem de bağlantısı kesilen Azure Stack örnekleri için, istemci sanal makinesini hazırlama, yüklemeyi denetleme ve istemci sanal makinesini ASDK üzerinde ayarlama hakkında inceliyoruz.

## <a name="prepare-the-client-vm"></a>İstemci VM 'sini hazırlama

AKS motoru, Kubernetes kümenizi dağıtmak ve yönetmek için kullanılan bir komut satırı aracıdır. Altyapıyı Azure Stack bir makinede çalıştırabilirsiniz. Bu makineden, kümelerinizi çalıştırmak için gereken IaaS kaynaklarını ve yazılımlarını dağıtmak üzere AKS altyapısını çalıştıracaksınız. Daha sonra, kümelerinizde yönetim görevlerini gerçekleştirmek için altyapıyı çalıştıran makineyi kullanabilirsiniz.

İstemci makinenizi seçerken şunları göz önünde bulundurun:

1. İstemci makinenin bir olağanüstü durum oluşması halinde kurtarılabilir olması gerekir.
3. İstemci makinesine nasıl bağlanacağınızı ve makinenin kümenizle nasıl etkileşime gireceğini öğrenin.

## <a name="install-in-a-connected-environment"></a>Bağlı bir ortama yüklensin

Kubernetes kümenizi Internet 'e bağlı bir Azure Stack yönetmek için istemci sanal makinesini yükleyebilirsiniz.

1. Azure Stack bir Windows sanal makinesi oluşturun. Yönergeler için bkz. [Hızlı başlangıç: Azure Stack portalını kullanarak bir Windows Server VM oluşturun @ no__t-0.
2. Sanal makinenize bağlanın.
3. [PowerShell yönergelerini kullanarak Chocolatey 'Yi yükler.](https://chocolatey.org/install#install-with-powershellexe).. 

    Chocolaty Web sitesine göre: Chocolatey, Windows için bir paket yöneticisi olan apt-get veya yıum, ancak Windows için. İhtiyacınız olan uygulamaları ve araçları hızlı bir şekilde yüklemek için merkezi olmayan bir çerçeve olacak şekilde tasarlanmıştır. Bu, şu anda PowerShell kullanan NuGet altyapısına kurulmuştur. Bu, paketleri distroden kapıya, hata, bilgisayar adına teslim etmek için odaklanmaktadır.
4. [Desteklenen Kubernetes sürümleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions) tablosunda aks altyapısının sürümünü bulun. AKS temel altyapısı, Azure Stack marketi 'nde kullanılabilir olmalıdır. Komutu çalıştırırken, `--version v0.41.2` sürümünü belirtmeniz gerekir. Sürümü belirtmezseniz, komut Market 'te kullanılamayan bir VHD görüntüsü gerektirebilecek en son sürümü yükler.
5. Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın ve sürüm numarasını ekleyin:

    ```PowerShell  
        choco install aks-engine --version v0.41.2 -y
    ```

> [!Note]  
> Yükleme için bu yöntem başarısız olursa, [bağlantısı kesilen ortamda](#install-in-a-disconnected-environment)adımları deneyebilir veya alternatif bir paket yöneticisi olan [gofsi 'yi deneyebilirsiniz](azure-stack-kubernetes-aks-engine-troubleshoot.md#try-gofish).

## <a name="install-in-a-disconnected-environment"></a>Bağlantısı kesilmiş bir ortama yüklensin

Kubernetes kümenizi Internet bağlantısı kesilen bir Azure Stack yönetmek için istemci sanal makinesini yükleyebilirsiniz.

1.  Internet erişimi olan bir makineden GitHub [Azure/aks-Engine](https://github.com/Azure/aks-engine/releases/latest)sayfasına gidin. Bir Windows makinesi için Arşiv (*. tar. gz) indirin, örneğin, `aks-engine-v0.38.8-windows-amd64.tar.gz`.

2.  AKS altyapısı ikilisini içeren arşiv dosyasını (*. tar. gz) karşıya yüklemek için Azure Stack örneğiniz için bir depolama hesabı oluşturun. Azure Depolama Gezgini kullanımı hakkında yönergeler için, bkz. [Azure Stack Azure Depolama Gezgini](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-connect-se).

3. Azure Stack bir Windows sanal makinesi oluşturun. Yönergeler için bkz. [Hızlı başlangıç: Azure Stack portalını kullanarak bir Windows Server VM oluşturma @ no__t-0

4.  Arşiv dosyasını (*. tar. gz) karşıya yüklediğiniz Azure Stack depolama hesabı blobu URL 'sinden, dosyayı Yönetim sanal makinenize indirin. Komut isteminizden erişimi olan bir dizine Arşivi ayıklayın.

5. Sanal makinenize bağlanın.

6. [PowerShell yönergelerini kullanarak Chocolatey 'Yi yükler.](https://chocolatey.org/install#install-with-powershellexe).. 

7.  Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın. Doğru sürüm numarasını ekleyin:

    ```PowerShell  
        choco install aks-engine --version v0.41.2 -y
    ```

## <a name="verify-the-installation"></a>Yüklemeyi doğrulama

İstemci VM 'niz kurulduktan sonra, AKS altyapısını yüklediğinizden emin olun.

1. İstemci sanal makinenize bağlanın.
2. Şu komutu çalıştırın:

    ```PowerShell  
    aks-engine version
    ```

AKS altyapısını istemci sanal makinenize yüklediğinizi doğrulayadıysanız bkz. [aks altyapısı yükleme sorunlarını giderme](azure-stack-kubernetes-aks-engine-troubleshoot.md)


## <a name="asdk-installation"></a>ASDK yüklemesi

AKS altyapısının istemci VM 'sini, ASDK dışındaki bir makinede ASDK 'de çalıştırırken bir sertifika eklemeniz gerekir. ASDK ortamının içinde bir Windows sanal makinesi kullanıyorsanız, makine zaten ASDK sertifikasına güvenir. İstemci makineniz ASDK dışındaysa, sertifikayı ASDK 'den ayıklamanız ve Windows makinenize eklemeniz gerekir.

Bir ASDK kullandığınızda Azure Resource Manager uç noktası otomatik olarak imzalanan bir sertifika kullanıyorsa, bu sertifikayı makinenin güvenilen sertifika deposuna eklemeniz gerekir. Asdk kök sertifikasını, ASDK 'de dağıttığınız herhangi bir VM 'de bulabilirsiniz.

1. CA kök sertifikasını dışarı aktarın. Yönergeler için bkz [. Azure Stack CA kök sertifikasını dışarı aktarma](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2#export-the-azure-stack-ca-root-certificate)
2. Azure Stack CA kök sertifikasına güvenin. Yönergeler için bkz. [Azure Stack CA kök sertifikasına güvenin](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2#trust-the-azure-stack-ca-root-certificate).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Kubernetes kümesini AKS altyapısı ile Azure Stack dağıtma](azure-stack-kubernetes-aks-engine-deploy-cluster.md)
