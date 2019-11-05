---
title: Azure Stack bir Kubernetes kümesi ile AKS altyapısını dağıtma | Microsoft Docs
description: AKS altyapısını çalıştıran bir istemci VM 'sinden Azure Stack bir Kubernetes kümesi dağıtma.
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
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 10/10/2019
ms.openlocfilehash: e4f10cb3e5d96942e5fe32b0d8fe3a04cf921521
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73595192"
---
# <a name="deploy-a-kubernetes-cluster-with-the-aks-engine-on-azure-stack"></a>Bir Kubernetes kümesini AKS altyapısı ile Azure Stack dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

AKS altyapısını çalıştıran bir istemci VM 'sinden Azure Stack bir Kubernetes kümesi dağıtabilirsiniz. Bu makalede, bir küme belirtimi yazma, `apimodel.json` dosyası ile küme dağıtma ve Held ile MySQL dağıtarak kümenizi denetleme konusuna baktık.

## <a name="define-a-cluster-specification"></a>Küme belirtimi tanımlama

Bir belge dosyasında, [API modeli](https://github.com/Azure/aks-engine/blob/master/docs/topics/architecture.md#architecture-diagram)olarak adlandırılan JSON biçimini kullanarak bir küme belirtimi belirtebilirsiniz. AKS altyapısı, kümenizi oluşturmak için API modelinde bir küme belirtimi kullanır. 

### <a name="update-the-api-model"></a>API modelini güncelleştirme

Bu bölüm kümeniz için bir API modeli oluşturmaya bakar.

1.  Azure Stack [örnek](https://github.com/Azure/aks-engine/tree/master/examples/azure-stack) bir API modeli dosyası kullanarak başlatın ve dağıtımınız için yerel bir kopya oluşturun. Makinesinden, AKS altyapısını yükleyeceğinizden şunu çalıştırın:

    ```bash
    curl -o kubernetes-azurestack.json https://raw.githubusercontent.com/Azure/aks-engine/master/examples/azure-stack/kubernetes-azurestack.json
    ```

    > [!Note]  
    > Bağlantınız kesildiyse, dosyayı indirebilir ve onu düzenlemeyi planladığınız bağlantısı kesilen makineye el ile kopyalayabilirsiniz. [Putty veya WinSCP](https://www.suse.com/documentation/opensuse103/opensuse103_startup/data/sec_filetrans_winssh.html)gibi araçları kullanarak dosyayı Linux makinenize kopyalayabilirsiniz.

2.  Düzenleyicisini bir düzenleyicide açmak için nano kullanabilirsiniz:

    ```bash
    nano ./kubernetes-azurestack.json
    ```

    > [!Note]  
    > Nano yüklü değilse, Ubuntu: `sudo apt-get install nano`'e nano yükleme yapabilirsiniz.

3.  Kubernetes-azurestack. json dosyasında `orchestratorRelease`bulun. Desteklenen Kubernetes sürümlerinden birini seçin. Örneğin, 1,14, 1,15. Sürümler genellikle güncelleştirmelerdir. X. xx. x yerine x. xx olarak sürümü belirtin. Geçerli sürümlerin bir listesi için bkz. [desteklenen Kubernetes sürümleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions). Aşağıdaki AKS motoru komutunu çalıştırarak desteklenen sürümü bulabilirsiniz:

    ```bash
    aks-engine get-versions
    ```

4.  `customCloudProfile` bulun ve kiracı portalının URL 'sini sağlayın. Örneğin, `https://portal.local.azurestack.external`. 

5. AD FS kullanıyorsanız `"identitySystem":"adfs"` ekleyin. Örneğin,

    ```JSON  
        "customCloudProfile": {
            "portalURL": "https://portal.local.azurestack.external",
            "identitySystem": "adfs"
        },
    ```

    > [!Note]  
    > Kimlik sisteminiz için Azure AD kullanıyorsanız, **ıdentitysystem** alanını eklemeniz gerekmez.

6. `portalURL` bulun ve kiracı portalının URL 'sini sağlayın. Örneğin, `https://portal.local.azurestack.external`.

7.  Dizi `masterProfile`, aşağıdaki alanları ayarlayın:

    | Alan | Açıklama |
    | --- | --- |
    | dnsPrefix | VM 'lerin ana bilgisayar adını belirlemek için kullanılacak benzersiz bir dize girin. Örneğin, kaynak grubu adını temel alan bir ad. |
    | count |  Dağıtımınız için istediğiniz ana öğe sayısını girin. Bir HA dağıtımı için en az 3, ancak HA olmayan dağıtımlar için 1 öğesine izin verilir. |
    | vmSize |  [Azure Stack tarafından desteklenen bir boyut](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes)girin, örnek `Standard_D2_v2`. |
    | distro | `aks-ubuntu-16.04` yazın. |

8.  Dizi `agentPoolProfiles` güncelleştirmesi:

    | Alan | Açıklama |
    | --- | --- |
    | count | Dağıtımınız için istediğiniz aracıların sayısını girin. |
    | vmSize | [Azure Stack tarafından desteklenen bir boyut](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes)girin, örnek `Standard_D2_v2`. |
    | distro | `aks-ubuntu-16.04` yazın. |

9.  Dizi `linuxProfile` güncelleştirmesi:

    | Alan | Açıklama |
    | --- | --- |
    | adminUsername | VM yönetici kullanıcı adını girin. |
    | SSH | VM 'Ler ile SSH kimlik doğrulaması için kullanılacak ortak anahtarı girin. |

### <a name="more-information-about-the-api-model"></a>API modeli hakkında daha fazla bilgi

- API modelindeki tüm kullanılabilir seçeneklerin tamamı için [küme tanımlarına](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md)bakın.  
- Azure Stack özgü seçeneklere yönelik vurgular için [Azure Stack kümesi tanımı özelliklerine](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#cluster-definition-aka-api-model)bakın.  

## <a name="deploy-a-kubernetes-cluster"></a>Bir Kubernetes kümesi dağıtın

API modelinizde gerekli tüm değerleri topladıktan sonra kümenizi oluşturabilirsiniz. Bu noktada şunları yapmalısınız:

Azure Stack operatörünüzden şunları yapın:

- Sistemin sistem durumunu doğrulayın, `Test-AzureStack` çalıştırmayı ve OEM satıcınızın donanım izleme aracını önerin.
- Bellek, depolama ve genel IP 'Ler gibi kaynaklar dahil sistem kapasitesini doğrulayın.
- Kullanmayı planladığınız VM sayısı için hala yeterli alan olduğunu doğrulayabilmeniz için aboneliğinizle ilişkili kotanın ayrıntılarını sağlayın.

Bir kümeyi dağıtmaya devam edin:

1.  Azure Stack [CLI bayrakları](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#cli-flags)üzerinde aks altyapısının kullanılabilir parametrelerini gözden geçirin.

    | Parametre | Örnek | Açıklama |
    | --- | --- | --- |
    | Azure-env | AzureStackCloud | Hedef platformunuzun Azure Stack `AzureStackCloud`kullanılacak AKS altyapısını göstermek için. |
    | kimlik sistemi | FS | İsteğe bağlı. Active Directory Federasyon Hizmetleri kullanıyorsanız kimlik yönetimi çözümünüzü belirtin (AD FS). |
    | location | Yerel | Azure Stack için bölge adı. ASDK için bölge `local`olarak ayarlanır. |
    | resource-group | kuin-RG | Yeni bir kaynak grubunun adını girin veya var olan bir kaynak grubunu seçin. Kaynak adının alfasayısal ve küçük harf olması gerekir. |
    | api modeli | ./Kubernetes-azurestack.exe JSON | Küme yapılandırma dosyasının yolu veya API modeli. |
    | çıkış dizini | kuin-RG | Çıkış dosyası `apimodel.json` ve diğer oluşturulan dosyaları içerecek şekilde dizin adını girin. |
    | istemci kimliği | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Hizmet sorumlusu GUID 'INI girin. Azure Stack yöneticiniz hizmet sorumlusunu oluştururken uygulama KIMLIĞI olarak tanımlanan Istemci KIMLIĞI. |
    | istemci parolası | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Hizmet sorumlusu gizli anahtarını girin. Bu, hizmetinizi oluştururken ayarladığınız istemci sırrı. |
    | abonelik kimliği | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Abonelik KIMLIĞINIZI girin. Daha fazla bilgi için bkz. [teklife abone olma](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services#subscribe-to-an-offer) |

    Örnek aşağıda verilmiştir:

    ```bash  
    aks-engine deploy \
    --azure-env AzureStackCloud \
    --location <for asdk is local> \
    --resource-group kube-rg \
    --api-model ./kubernetes-azurestack.json \
    --output-directory kube-rg \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --identity-system adfs # required if using AD FS
    ```

2.  Bir nedenden dolayı, çıkış dizini oluşturulduktan sonra yürütme başarısız olursa, sorunu düzeltebilir ve komutunu yeniden çalıştırabilirsiniz. Dağıtımı yeniden çalıştırıyorsanız ve daha önce aynı çıkış dizinini kullandıysanız, AKS altyapısı dizinin zaten var olduğunu belirten bir hata döndürür. Şu bayrağı kullanarak var olan dizinin üzerine yazabilirsiniz: `--force-overwrite`.

3.  AKS motoru küme yapılandırmasını güvenli, şifrelenmiş bir konuma kaydedin.

    `apimodel.json`dosyasını bulun. Güvenli bir konuma kaydedin. Bu dosya, diğer tüm AKS motoru işlemlerinizin içinde girdi olarak kullanılacaktır.

    Oluşturulan `apimodel.json`, giriş API modelinde kullandığınız hizmet sorumlusu, gizli anahtar ve SSH ortak anahtarını içerir. Ayrıca, diğer tüm işlemleri gerçekleştirmek için AKS altyapısının gerek duyduğu tüm diğer meta verilere sahiptir. Kaybederseniz, AKS motoru kümeyi yapılandıramaz.

    Gizlilikler **şifrelenmemiş**. Dosyayı şifreli, güvenli bir yerde tutun. 

## <a name="verify-your-cluster"></a>Kümenizi doğrulama

Kümenizi denetlemek için Held ile MySQL dağıtarak kümenizi doğrulayın.

1. Azure Stack portalını kullanarak ana düğümlerinden birinin genel IP adresini alın.

2. Azure Stack örneğine erişimi olan bir makineden, PuTTY veya MobaXterm gibi bir istemciyi kullanarak SSH aracılığıyla yeni ana düğüme bağlanın. 

3. SSH Kullanıcı adı için, kümenin dağıtımı için belirttiğiniz anahtar çiftinin "azureuser" ve özel anahtar dosyasını kullanırsınız.

4.  Aşağıdaki komutları çalıştırın:

    ```bash
    sudo snap install helm --classic
    kubectl -n kube-system create serviceaccount tiller
    kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    helm init --service-account=tiller
    helm repo update
    helm install stable/mysql
    ```

5.  Testi temizlemek için MySQL dağıtımı için kullanılan adı bulun. Aşağıdaki örnekte ad `wintering-rodent`. Sonra silin. 

    Aşağıdaki komutları çalıştırın:

    ```bash
    helm ls
    NAME REVISION UPDATED STATUS CHART APP VERSION NAMESPACE
    wintering-rodent 1 Thu Oct 18 15:06:58 2018 DEPLOYED mysql-0.10.1 5.7.14 default
    helm delete wintering-rodent
    ```

    CLı görüntülenir:
    ```bash
    release "wintering-rodent" deleted
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Stack üzerindeki AKS altyapısının sorunlarını giderme](azure-stack-kubernetes-aks-engine-troubleshoot.md)