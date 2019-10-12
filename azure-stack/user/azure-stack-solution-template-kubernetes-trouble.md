---
title: Azure Stack için Kubernetes dağıtımıyla ilgili sorun giderme | Microsoft Docs
description: Azure Stack için Kubernetes dağıtımıyla ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.author: mabrigg
ms.date: 10/10/2019
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 14a32696a3e46782b8990ba57f9510976200f7d3
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277559"
---
# <a name="troubleshoot-kubernetes-deployment-to-azure-stack"></a>Azure Stack için Kubernetes dağıtımıyla ilgili sorunları giderme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack Kubernetes önizleme aşamasındadır. Azure Stack bağlantısı kesik bir senaryo şu anda önizleme tarafından desteklenmiyor. Yalnızca geliştirme ve test senaryoları için Market öğesini kullanın.

Bu makalede Kubernetes kümenizde nasıl sorun giderileceği incelenemez. Sorun gidermeye başlamak için, dağıtım için gereken öğeleri gözden geçirin. Kubernetes 'i barındıran Azure Stack veya Linux VM 'lerinden dağıtım günlüklerini toplamanız gerekebilir. Yönetim uç noktasından günlükleri almak için Azure Stack yöneticinize başvurun.

## <a name="overview-of-kubernetes-deployment"></a>Kubernetes dağıtımına genel bakış

Kümenizin sorunlarını giderebilmeniz için, Kubernetes küme dağıtım işlemini Azure Stack gözden geçirin. Dağıtım, VM 'Ler oluşturmak ve kümeniz için AKS-Engine yüklemek üzere bir Azure Resource Manager çözüm şablonu kullanır.

### <a name="kubernetes-deployment-workflow"></a>Kubernetes dağıtım iş akışı

Aşağıdaki diyagramda, kümeyi dağıtmaya yönelik genel işlem gösterilmektedir.

![Kubernetes işlemini dağıtma](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Dağıtım adımları

1. Market öğesinden giriş parametrelerini toplayın.

    Kubernetes kümesini ayarlamak için gereken değerleri girin, örneğin:
    -  **Kullanıcı adı**: Kubernetes kümesinin ve DVD 'nin parçası olan Linux sanal makinelerinin (VM 'ler) Kullanıcı adı.
    -  **SSH ortak anahtarı**: Kubernetes kümesi ve DVD 'nin bir parçası olarak oluşturulan tüm Linux makinelerin yetkilendirmesi için kullanılan anahtar.
    -  **Hizmet sorumlusu**: Kubernetes Azure bulut sağlayıcısı tarafından kullanılan kimlik. Hizmet sorumlunuzu oluştururken uygulama KIMLIĞI olarak tanımlanan istemci KIMLIĞI. 
    -  **İstemci gizli**dizisi: hizmet sorumlunuzu oluştururken oluşturduğunuz anahtar.

2. Dağıtım sanal makinesi ve özel Betik uzantısı oluşturun.
    -  Market Linux Image **Ubuntu Server 16,04-LTS**kullanarak DAĞıTıM Linux VM 'sini oluşturun.
    -  Özel Betik uzantısı 'nı Market 'ten indirip çalıştırın. Betik, **Linux 2,0 Için özel bir betiktir**.
    -  DVA özel betiğini çalıştırın. Komut dosyası aşağıdaki görevleri yapar:
        1. Azure Resource Manager meta veri uç noktasından Galeri uç noktasını alır.
        2. Azure Resource Manager meta veri uç noktasından Active Directory kaynak KIMLIĞINI alır.
        3. AKS altyapısının API modelini yükler.
        4. AKS altyapısını Kubernetes kümesine dağıtır ve Azure Stack bulut profilini `/etc/kubernetes/azurestackcloud.json` ' a kaydeder.
3. Ana VM 'Leri oluşturun.

4. Özel Betik uzantılarını indirip çalıştırın.

5. Ana betiği çalıştırın.

    Komut dosyası aşağıdaki görevleri yapar:
    - Kubelet gibi etcd, Docker ve Kubernetes kaynaklarını kurar. etcd, bir makine kümesi genelinde veri depolamak için bir yol sağlayan dağıtılmış bir anahtar değeri deposudur. Docker, kapsayıcılar olarak bilinen çıplak işletim sistemi düzeyi sanallaştırmeleri destekler. Kubelet, her bir Kubernetes düğümünde çalışan düğüm aracısıdır.
    - **Etcd** hizmetini ayarlar.
    - **Kubelet** hizmetini ayarlar.
    - Kubelet 'i başlatır. Bu görev aşağıdaki adımları içerir:
        1. API hizmetini başlatır.
        2. Denetleyici hizmetini başlatır.
        3. Zamanlayıcı hizmetini başlatır.
6. Aracı VM 'Leri oluşturun.

7. Özel Betik uzantısını indirip çalıştırın.

7. Aracı betiğini çalıştırın. Aracı özel komut dosyası aşağıdaki görevleri yapar:
    - **Etcd 'yi**yüklüyor.
    - **Kubelet** hizmetini ayarlar.
    - Kubernetes kümesini birleştirir.

## <a name="steps-to-troubleshoot-kubernetes"></a>Kubernetes sorunlarını giderme adımları

Kubernetes kümenizi destekleyen VM 'lerde dağıtım günlüklerini toplayıp gözden geçirebilirsiniz. Kullanmanız gereken Azure Stack sürümünü doğrulamak ve dağıtımınızla ilgili Azure Stack günlükleri almak için Azure Stack yöneticinizle görüşün.

1. [Dağıtım durumunu](#review-deployment-status) gözden geçirin ve Kubernetes kümenizdeki ana düğümden günlükleri alın.
2. Azure Stack en son sürümünü kullandığınızdan emin olun. Kullanmakta olduğunuz sürümü bilmiyorsanız Azure Stack yöneticinize başvurun.
3.  VM oluşturma dosyalarınızı gözden geçirin. Aşağıdaki sorunlarla karşılaşmışsınız olabilirsiniz:  
    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - VM oluşturma bir iç hata tetikledi veya bir oluşturma hatası tetiklemiş olabilir. Azure Stack Aboneliğinizle ilgili kapasite sınırlamaları da dahil olmak üzere bir dizi etken hatalara neden olabilir.
    - Sanal makine için tam etki alanı adının (FQDN) yinelenen bir ön ek ile başladığından emin olun.
4.  VM **Tamam**Ise, DVD 'yi değerlendirin. DVD 'nin bir hata iletisi varsa:

    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - Ayrıcalıklı uç noktaları kullanarak Azure Stack günlüklerini almak için Azure Stack yöneticinize başvurun. Daha fazla bilgi için bkz. [Azure Stack tanılama araçları](../operator/azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs).
5. Dağıtımınız hakkında sorularınız varsa, bunu gönderebilir veya birisinin [Azure Stack forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)soruyu zaten cevaplamış olup olmadığını görebilirsiniz. 

## <a name="review-deployment-status"></a>Dağıtım durumunu gözden geçir

Kubernetes kümenizi dağıtırken, herhangi bir sorunu denetlemek için dağıtım durumunu gözden geçirebilirsiniz.

1. [Azure Stack portalını](https://portal.local.azurestack.external)açın.
2. **Kaynak grupları**' nı seçin ve Kubernetes kümesini dağıttığınızda kullandığınız kaynak grubunun adını seçin.
3. **Dağıtımlar**' ı seçin ve ardından **dağıtım adını**seçin.

    ![Kubernetes sorunlarını giderme: dağıtım seçin](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sorun giderme penceresine bakın. Dağıtılan her kaynak aşağıdaki bilgileri sağlar:
    
    | Özellik | Açıklama |
    | ----     | ----        |
    | Kaynak | Kaynağın adı. |
    | Tür | Kaynak sağlayıcısı ve kaynak türü. |
    | Durum | Öğenin durumu. |
    | Ilişkin | Zamanın UTC zaman damgası. |
    | İşlem ayrıntıları | İşlem ayrıntıları, işleme dahil olan kaynak sağlayıcısı, kaynak uç noktası ve kaynağın adı. |

    Her öğenin bir yeşil veya kırmızı durum simgesi vardır.

## <a name="review-deployment-logs"></a>Dağıtım günlüklerini gözden geçirme

Azure Stack portalı bir dağıtım hatasını gidermek veya bir dağıtım başarısızlığından kaçınmak için yeterli bilgi sağlamıyorsa, sonraki adım küme günlüklerini inceleyin. Dağıtım günlüklerini el ile almak için, genellikle kümenin ana VM 'lerinden birine bağlanmanız gerekir. Daha basit bir alternatif yaklaşım, Azure Stack ekibi tarafından sunulan aşağıdaki [Bash betiğini](https://aka.ms/AzsK8sLogCollectorScript) indirmek ve çalıştırmak olacaktır. Bu betik, DVı ve kümenin VM 'lerine bağlanır, ilgili sistem ve küme günlüklerini toplar ve bunları iş istasyonunuza geri yükler.

### <a name="prerequisites"></a>Prerequisites

Azure Stack yönetmek için kullandığınız makinede bir bash istemi gerekir. Windows makinesinde, [Windows Için git](https://git-scm.com/downloads)'i yükleyerek bir bash istemi alabilirsiniz. Yüklendikten sonra başlangıç menünüzde _Git Bash_ ' i arayın.

### <a name="retrieving-the-logs"></a>Günlükler alınıyor

Küme günlüklerini toplamak ve indirmek için şu adımları izleyin:

1. Bash istemi açın. Bir Windows makinesinden _Git Bash_ ' i açın veya şunu çalıştırın: `C:\Program Files\Git\git-bash.exe`.

2. Bash isteminizdeki aşağıdaki komutları çalıştırarak günlük Toplayıcı betiğini indirin:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    chmod 744 getkuberneteslogs.sh
    ```

3. Betiği için gereken bilgileri arayın ve çalıştırın:

    | Parametre           | Açıklama                                                                                                      | Örnek                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -d,--VMD-konak      | DVD 'nin genel IP veya tam etki alanı adı (FQDN). VM adı `vmd-` ile başlar. | IP: 192.168.102.38<br>DNS: VMD-myk8s. Local. cloudapp. azurestack. External |
    | -h,--yardım  | Komut kullanımını yazdır. | |
    | -i,--Identity-File | Kubernetes kümesi oluşturulurken Market öğesine geçirilen RSA özel anahtar dosyasının yolu. İle Kubernetes düğümlerine uzak için gereklidir. | C:\data\ıd_rsa.exe (PuTTY)<br>~/nssh/id_rsa (SSH)
    | -a,--ana-konak   | Bir Kubernetes ana düğümünün genel IP veya tam etki alanı adı (FQDN). VM adı `k8s-master-` ile başlar. | IP: 192.168.102.37<br>FQDN: k8s-12345. Local. cloudapp. azurestack. External      |
    | -u,--Kullanıcı          | Kubernetes kümesi oluşturulurken Market öğesine geçirilen Kullanıcı adı. İle Kubernetes düğümlerine uzak için gereklidir. | azureuser (varsayılan değer) |


   Parametre değerlerinizi eklediğinizde, komutunuz Şu örneğe benzer bir şekilde görünebilir:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\id_rsa.pem" --user azureuser --vmd-host 192.168.102.37
     ```

4. Birkaç dakika sonra, komut dosyası toplanan günlükleri `KubernetesLogs_{{time-stamp}}` adlı bir dizine çıktı olarak kaydeder. Kümeye ait olan her VM için bir dizin bulacaksınız.

    Günlük Toplayıcı betiği, günlük dosyalarındaki hataları da arar ve bilinen bir sorunu bulursa sorun giderme adımlarını dahil eder. Bilinen sorunları bulma olasılığınızı artırmak için betiğin en son sürümünü çalıştırdığınızdan emin olun.

> [!Note]  
> Günlük Toplayıcı betiği hakkında daha fazla bilgi edinmek için bu GitHub [deposuna](https://github.com/msazurestackworkloads/azurestack-gallery/tree/master/diagnosis) göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes 'i Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)

[Market 'e bir Kubernetes kümesi ekleme (Azure Stack işleci için)](../operator/azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure 'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
