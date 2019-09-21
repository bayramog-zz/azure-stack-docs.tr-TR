---
title: Azure Stack, AKS altyapısının önkoşullarını ayarlama | Microsoft Docs
description: Azure Stack ASK altyapısını çalıştırmaya yönelik gereksinimleri oluşturun.
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
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: cc278020bb4d2ba530e20b6f2f56bcb841207bbc
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159732"
---
# <a name="set-up-the-prerequisites-for-the-aks-engine-on-azure-stack"></a>Azure Stack AKS altyapısı için önkoşulları ayarlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

AKS altyapısını ortamınızdaki bir VM 'ye veya Azure Stack Kaynak Yöneticisi uç noktanıza erişimi olan herhangi bir istemci makinesine yükleyebilirsiniz. Altyapıyı çalıştırmadan önce aşağıdaki öğelere sahip olmanız gerekir: aboneliğinizde bulunan AKS Base Ubuntu Server ve Linux özel Betik uzantısı, katkıda bulunan bir role atanan hizmet sorumlusu kimliği ve özel/ortak anahtar çifti Ubuntu sunucunuza SSH erişimi için. Ayrıca, Azure Stack Geliştirme Seti kullanıyorsanız makinenizin uygun sertifikalara güvenmesi gerekir.

Ön koşullar varsa, [kümenizi tanımlamaya](azure-stack-kubernetes-aks-engine-deploy-cluster.md)başlayabilirsiniz.

Azure Stack için bulut operatörünüz varsa ve AKS altyapısını sunmak istiyorsanız, [AKS altyapısını Azure Stack Market 'e ekleme](../operator/azure-stack-aks-engine.md)bölümündeki yönergeleri izleyin.

## <a name="prerequisites-for-the-aks-engine"></a>AKS altyapısının önkoşulları

AKS altyapısını kullanmak için aşağıdaki kaynaklara sahip olmanız gerekir. AKS altyapısının, Kubernetes kümelerini kiracı aboneliğine dağıtmak üzere Azure Stack kiracılar tarafından kullanılması gerektiğini aklınızda bulundurun. Azure Stack işlecinin katılımın gerekli olabileceği tek bölüm Market öğelerini indirmek ve hizmet sorumlusu kimliği oluşturmak içindir. Aşağıdaki tabloda ayrıntıları bulabilirsiniz.

| Önkoşul | Açıklama | Gerekli | Yönergeler |
| --- | --- | --- | --- |
| Linux özel Betik uzantısı | Linux özel Betik uzantısı 2,0<br>Sunar Linux 2,0 için özel betik<br>Sürüm: 2.0.6 (veya en son sürüm)<br>'In Microsoft Corp | Gerekli | Aboneliğinizde bu öğe yoksa, bulut işleçle iletişim kurun. |
| AKS Base Ubuntu görüntüsü | AKS temel görüntüsü<br>Teklif: aks<br>Sürüm: 2019.07.30 (veya daha yeni sürüm)<br>Yayımcı: Microsoft-aks<br>SKU: aks-Ubuntu-1604-201907 | Gerekli | Aboneliğinizde bu öğe yoksa, bulut işleçle iletişim kurun. Sürüm bağımlılığı hakkında daha fazla bilgi için bkz. [temel görüntü sürümüne eşleşen altyapı](#matching-engine-to-base-image-version). |
| Azure Stack aboneliği | Azure Stack tekliflere, aboneliklerle erişebilirsiniz. Teklif, sizin için kullanabileceğiniz hizmetleri içerir. | Gerekli | Azure Stack hiçbir kiracı iş yükünü dağıtabilmek için önce bir [Azure Stack aboneliği](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services)almanız gerekir. |
| Hizmet sorumlusu kimliği (SPN) |  Kaynakları Azure Resource Manager aracılığıyla dağıtması veya yapılandırması gereken bir uygulama, bir hizmet sorumlusu tarafından temsil etmelidir. | Gerekli | Bu öğe için Azure Stack işleçle iletişim kurmanız gerekebilir.  Yönergeler için bkz. [kaynaklara erişmek için uygulama kimliği kullanma](https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals) |
| (SPN) atanmış **katkıda bulunan** rolü | Bir uygulamanın hizmet sorumlusunu kullanarak aboneliğinizdeki kaynaklara erişmesine izin vermek için, hizmet sorumlusunu belirli bir kaynak için bir role atamanız gerekir. | Gerekli | Yönergeler için bkz. [rol atama](https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals#assign-a-role) |
| Resource group | Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Mevcut bir kaynak grubunu belirtmezseniz araç sizin için bir tane oluşturur. | İsteğe Bağlı | [Azure portal kullanarak Azure Resource Manager kaynak gruplarını yönetme](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal) |
| Özel ortak anahtar | Geliştirme makinenizden Web uygulamanızı barındıran Azure Stack örneğindeki sunucu VM 'sine açık bir SSH bağlantısı kullanmak için, bir Secure Shell (SSH) ortak ve özel anahtar çifti oluşturmanız gerekir. | Gerekli | Anahtar oluşturma yönergeleri için bkz. [SSH anahtar üretimi](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-ssh-public-key).|

> [!Note]  
> Ayrıca, Azure Stack veya [Azure Stack PowerShell](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install) [için Azure CLI](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2) ile aks altyapısına yönelik önkoşulları da oluşturabilirsiniz.

## <a name="matching-engine-to-base-image-version"></a>Temel görüntü sürümüne eşleşen altyapı

AKS altyapısı, **aks temel görüntüsünü**kullanan oluşturulmuş bir görüntü kullanır. Herhangi bir AKS motoru sürümü, Azure Stack operatörünüz tarafından Azure Stack kullanılabilir hale getirilen belirli bir görüntü sürümüne bağımlıdır. AKS altyapısı sürümlerini ve [desteklenen Kubernetes sürümlerinde](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)karşılık gelen desteklenen Kubernetes sürümlerini listeleyerek bir tablo bulabilirsiniz. Örneğin, aks motoru sürümü `v0.40.0` aks temel görüntüsünün sürümüne `2019.08.21` bağlıdır. Belirli görüntü sürümünü Azure Marketi 'nden Azure Stack Market 'e indirmek için Azure Stack işleçinizden sorun.

Görüntü Azure Stack marketi 'nde yoksa tetiklenir ve hata alırsınız. Örneğin, şu anda aks altyapısı sürüm v 0.39.1 kullanıyorsanız ve aks temel görüntü sürümü `2019.08.09` yoksa, aks altyapısını çalıştırırken şu hatayı görürsünüz: 

```Text  
The platform image 'microsoft-aks:aks:aks-ubuntu-1604-201908:2019.08.09' is not available. 
Verify that all fields in the storage profile are correct.
```

Aşağıdaki komutu çalıştırarak AKS motorunuz için geçerli sürümü kontrol edebilirsiniz:

```bash  
$ aks-engine version
Version: v0.39.1
GitCommit: 6fff62731
GitTreeState: clean
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [AKS altyapısını Windows üzerinde dağıtma Azure Stack](azure-stack-kubernetes-aks-engine-deploy-windows.md)  
> [Linux üzerinde AKS altyapısını dağıtma Azure Stack](azure-stack-kubernetes-aks-engine-deploy-linux.md)