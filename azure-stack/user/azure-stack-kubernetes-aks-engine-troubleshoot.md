---
title: Azure Stack 'de AKS altyapısının sorunlarını giderme | Microsoft Docs
description: Bu makale Azure Stack üzerindeki AKS altyapısına ilişkin sorun giderme adımlarını içerir.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femilav
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: 49684cb1821a5014e984a8e177f881be13123829
ms.sourcegitcommit: 0d27456332031ab98ba2277117395ae5ffcbb79f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73047137"
---
# <a name="troubleshoot-the-aks-engine-on-azure-stack"></a>Azure Stack üzerindeki AKS altyapısının sorunlarını giderme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack üzerinde AKS altyapısını dağıtma veya bunlarla çalışırken bir sorunla karşılaşabilirsiniz. Bu makale, AKS altyapısının dağıtımını sorun giderme, AKS altyapınız hakkında bilgi toplama, Kubernetes günlüklerini toplama, Özel Betik uzantısı hata kodlarını İnceleme ve AKS altyapısı için bir GitHub sorunu açma yönergelerini inceler.

> [!IMPORTANT]
> AKS altyapısı şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="troubleshoot-the-aks-engine-install"></a>AKS altyapısı yüklemesinin sorunlarını giderme

### <a name="try-gofish"></a>Gofsi kullanmayı deneyin

Yükleme adımları başarısız olursa, Gofsi paket yöneticisini kullanarak yüklemeyi deneyin. [Gofsi](https://gofi.sh) , kendisini platformlar arası Homebrew olarak açıklar.

#### <a name="install-the-aks-engine-with-gofish-on-linux"></a>Linux 'ta Gofsi ile AKS altyapısını yükler

[Yüklemesi](https://gofi.sh/#install) sayfasından Gofsi 'yi yükler.

1. Bash isteminde aşağıdaki komutu çalıştırın:

    ```bash
    curl -fsSL https://raw.githubusercontent.com/fishworks/gofish/master/scripts/install.sh | bash
    ```

2.  AKS altyapısını Gofsi ile yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    Run "gofish install aks-engine"
    ```

#### <a name="install-the-aks-engine-with-gofish-on-windows"></a>Windows 'a Gofsi ile AKS altyapısını yükler

[Yüklemesi](https://gofi.sh/#install) sayfasından Gofsi 'yi yükler.

1. Yükseltilmiş bir PowerShell isteminden aşağıdaki komutu çalıştırın:

    ```PowerShell
    Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/fishworks/gofish/master/scripts/install.ps1'))
    ```

2.  AKS altyapısını Gofsi ile yüklemek için aynı oturumda aşağıdaki komutu çalıştırın:

    ```PowerShell
    gofish install aks-engine
    ```

### <a name="checklist-for-common-deployment-issues"></a>Ortak dağıtım sorunları için denetim listesi

AKS altyapısını kullanarak bir Kubernetes kümesi dağıtılırken hatalarla karşılaşıldığında şunları kontrol edebilirsiniz:

1.  Doğru hizmet sorumlusu kimlik bilgilerini (SPN) kullanıyor musunuz?
2.  SPN Azure Stack aboneliğine bir "katkıda bulunanlar" rolü veriyor mu?
3. Azure Stack planınızda yeterince büyük bir kotası var mı?
4.  Azure Stack örneği bir yama veya yükseltmeye uygulanıyor mu?

Daha fazla bilgi için bkz. **Azure/aks-Engine** GitHub deposu 'Ndaki [sorun giderme](https://github.com/Azure/aks-engine/blob/master/docs/howto/troubleshooting.md) makalesi.

## <a name="collect-aks-engine-logs"></a>AKS motoru günlüklerini topla

AKS altyapısı tarafından oluşturulan gözden geçirme bilgilerine erişebilirsiniz. AKS altyapısı durumu ve uygulamanın çalıştırıldığı şekilde hataları raporlar. Çıktıyı bir metin dosyasına boru yapabilir veya doğrudan komut satırı konsolundan kopyalayabilirsiniz. [Özel Betik uzantısı hata kodlarını gözden geçirme](#review-custom-script-extension-error-codes)sırasında aks altyapısının tetiklediği hata kodlarının listesine bakın.

1.  AKS motoru komut satırı aracında görünen bilgilerden standart çıkış ve hata toplayın.

2. Yerel bir dosyadan günlükleri al. Çıkış dizinini **--output-Directory** parametresiyle ayarlayabilirsiniz.

    Günlüklerin yerel yolunu ayarlamak için:

    ```bash  
    aks-engine --output-directory <path to the directory>
    ```

## <a name="collect-kubernetes-logs"></a>Kubernetes günlüklerini toplayın

AKS motoru günlüklerine ek olarak, Kubernetes bileşenleri durum ve hata iletileri oluşturur. Bu günlükleri, [Getkuberneteslogs.sh](https://github.com/msazurestackworkloads/azurestack-gallery/releases/download/diagnosis-v0.1.0/diagnosis.zip)Bash betiğini kullanarak toplayabilirsiniz.

Bu betik aşağıdaki günlükleri toplama işlemini otomatikleştirir: 

 - Microsoft Azure Linux Aracısı (waagent) günlükleri
 - Özel Betik uzantısı günlükleri
 - Kuin sistem kapsayıcısı meta verilerini çalıştırma
 - Kuin sistem kapsayıcısı günlüklerini çalıştırma
 - Kubelet hizmeti durumu ve günlüğü
 - Etcd hizmeti durumu ve günlüğü
 - Galeri öğesinin DVZ günlükleri
 - kuin-sistem anlık görüntüsü

Bu komut dosyası olmadan, kümedeki her düğüme bağlanmanız gerekir ve günlükleri elle indirin. Ayrıca, komut dosyası isteğe bağlı olarak, toplanan günlükleri günlükleri başkalarıyla paylaşmak için kullanabileceğiniz bir depolama hesabına yükleyebilirsiniz.

Gereksinimler:

 - Bir Linux VM, git bash veya Windows üzerinde Bash.
 - [Azure CLI](azure-stack-version-profiles-azurecli2.md) , betiğinin çalıştırılacağı makineye yüklendi.
 - Azure Stack için bir Azure CLı oturumunda oturum açan hizmet sorumlusu kimliği. Betik, çalışmasını sağlamak için ARM kaynaklarını bulma ve oluşturma yeteneğine sahip olduğundan, Azure CLı ve hizmet sorumlusu kimliği gereklidir.
 - Ortamda Kubernetes kümesinin zaten seçildiği Kullanıcı hesabı (abonelik). 
1. Betik bataklık dosyasının en son sürümünü istemci sanal makinenize, Kubernetes kümenize erişimi olan bir makineye veya AKS altyapısı ile kümenizi dağıtmak için kullandığınız makineye indirin.

    Aşağıdaki komutları çalıştırın:

    ```bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    wget https://github.com/msazurestackworkloads/azurestack-gallery/releases/download/diagnosis-v0.1.0/diagnosis.tar.gz
    tar xvzf diagnosis.tar.gz -C ./
    ```

2. `getkuberneteslogs.sh` betiği için gereken parametreleri arayın. Betik aşağıdaki parametreleri kullanacaktır:

    | Parametre | Açıklama | Gereklidir | Örnek |
    | --- | --- | --- | --- |
    | -h,--yardım | Komut kullanımını yazdır. | hayır | 
    -u,--Kullanıcı | Küme VM 'Leri için Yönetici Kullanıcı adı | evet | azureuser<br>(varsayılan değer) |
    | -i,--Identity-File | RSA özel anahtarı Kubernetes kümesini oluşturmak için kullanılan ortak anahtara bağlı (bazen ' id_rsa ' olarak adlandırılır)  | evet | `./rsa.pem` (PuTTY)<br>`~/.ssh/id_rsa` (SSH) |
    |   -g,--Resource-Group    | Kubernetes kümesi kaynak grubu | evet | k8sresourcegroup |
    |   -n,--Kullanıcı-ad alanı               | Belirtilen ad alanlarındaki kapsayıcılardan günlükleri topla (Kuto-sistem günlükleri her zaman toplanır) | hayır |   izlemesinin |
    |       --api-model                    | Azure Stack Storage hesabında apimodel. json dosyasını devam ettirir. Apimodel. json dosyasını depolama hesabına yükle,--upload-logs parametresi de sağlandığında gerçekleşir. | hayır | `./apimodel.json` |
    | --tümü-ad alanları               | Tüm ad alanlarındaki kapsayıcılardan günlükleri toplayın. Geçersiz kılmalar--Kullanıcı-ad alanı | hayır | |
    | --karşıya yükleme-Günlükler                  | Azure Stack depolama hesabındaki Günlükler alındı. Günlükler, KubernetesLogs kaynak grubunda bulunabilir | hayır | |
    --Disable-Host-Key-checking    | Betik yürütülürken SSH 'nin StrictHostKeyChecking seçeneğini "Hayır" olarak ayarlar. Yalnızca güvenli bir ortamda kullanın. | hayır | |

3. Bilgilerinizi kullanarak aşağıdaki örnek komutlardan birini çalıştırın:

    ```bash
    ./getkuberneteslogs.sh -u azureuser -i private.key.1.pem -g k8s-rg
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --disable-host-key-checking
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg -n default -n monitoring
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --upload-logs --api-model clusterDefinition.json
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --upload-logs
    ```

## <a name="review-custom-script-extension-error-codes"></a>Özel Betik uzantısı hata kodlarını gözden geçirme

Kümenizi çalıştırırken özel Betik uzantısı (CSE) tarafından oluşturulan hata kodlarının listesine başvurabilirsiniz. CSE hatası, sorunun kök nedenini tanılamak için yararlı olabilir. Kubernetes Kümenizde kullanılan Ubuntu sunucusu için CSE, AKS altyapı işlemlerinin çoğunu destekler. CSE çıkış kodları hakkında daha fazla bilgi için bkz. [cse_helpers. sh](https://github.com/Azure/aks-engine/blob/master/parts/k8s/cloud-init/artifacts/cse_helpers.sh).

### <a name="providing-kubernetes-logs-to-a-microsoft-support-engineer"></a>Microsoft destek mühendisine Kubernetes günlükleri sağlama

Günlükleri toplayıp inceledikten sonra sorununuzu çözemezse, bir destek bileti oluşturma işlemini başlatmak ve `--upload-logs` parametresi kümesiyle `getkuberneteslogs.sh` çalıştırarak topladığınız günlükleri sağlamak isteyebilirsiniz. 

Azure Stack işleçle iletişim kurun. Operatörünüz, destek durumunu oluşturmak için günlüklerinizi testten olarak kullanır.

Herhangi bir destek sorununu giderme işlemi sırasında, Microsoft Destek Mühendisi Azure Stack operatörünüzün Azure Stack sistem günlüklerini toplamasını isteyebilir. `getkuberneteslogs.sh`çalıştırarak Kubernetes günlüklerini karşıya yüklediğiniz depolama hesabı bilgileri ile operatörüzü sağlamanız gerekebilir.

Operatörünüz **Get-AzureStackLog** PowerShell cmdlet 'ini çalıştırabilir. Bu komut, Kubernetes günlüklerini depoladığınız depolama hesabını belirten bir parametre (`-InputSaSUri`) kullanır.

İşletmenğiniz, Microsoft Destek ile ilgili diğer sistem günlüklerinin gerek duyduğu ve Microsoft tarafından kullanılabilmesini sağlamak için oluşturduğunuz günlükleri birleştirebilir.

## <a name="open-github-issues"></a>GitHub sorunlarını açma

Dağıtım hatayı çözemezse bir GitHub sorunu açabilirsiniz. 

1. AKS altyapısı deposunda bir [GitHub sorunu](https://github.com/Azure/aks-engine/issues/new) açın.
2. Şu biçimi kullanarak bir başlık ekleyin: C`SE error: exit code <INSERT_YOUR_EXIT_CODE>`.
3. Soruna aşağıdaki bilgileri ekleyin:

    - Kümeyi dağıtmak için kullanılan küme yapılandırma dosyası `apimodel json`. GitHub 'da nakletmeden önce tüm gizli dizileri ve anahtarları kaldırın.  
     - Aşağıdaki **kubectl** komutunun çıktısı `get nodes`.  
     - `/var/log/azure/cluster-provision.log` ve `/var/log/cloud-init-output.log` içeriği

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin
