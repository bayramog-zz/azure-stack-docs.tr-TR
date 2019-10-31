---
title: Market öğe kümenizi Azure Stack AKS altyapısına taşıyın | Microsoft Docs
description: Market öğe kümenizi Azure Stack üzerindeki AKS altyapısına taşımayı öğrenin.
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
ms.date: 10/29/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 10/29/2019
ms.openlocfilehash: 41ffa9d9d9f96506d30a6a3c69a557cdba034843
ms.sourcegitcommit: 4d7611d81da6f2f8ef50adab3c09f9122a75bc9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145778"
---
# <a name="move-your-marketplace-item-cluster-to-the-aks-engine-on-azure-stack"></a>Market öğe kümenizi Azure Stack üzerindeki AKS altyapısına taşıyın

Kubernetes Azure Stack Market öğesi, AKS altyapısını indirmek ve yüklemek üzere bir dağıtım sanal makinesi (VM) dağıtmak ve bu AKS altyapısı VM 'de çalıştırıldıktan sonra kümeyi anlatmak için kullanılan giriş API modelini oluşturmak için bir Azure Resource Manager şablonu kullanır ve dağıtılmış küme. Bu makalede, AKS altyapısına ve bunlara karşılık gelen dosyalara nasıl erişebileceğiniz gösterilmektedir. böylece, Kubernetes kümenizde güncelleştirme ve ölçeklendirme işlemleri gerçekleştirmek için bunu kullanabilirsiniz.

## <a name="access-aks-engine-in-the-dvm"></a>DVD 'de AKS altyapısına erişme

Kubernetes Azure Stack Market öğesi tarafından başlatılan Dağıtım başarıyla tamamlandığında, küme için belirttiğiniz kaynak grubunda oluşturulan dağıtım sanal makinesine yüklenen kümeyi dağıtmak için kullanılan AKS altyapısını bulabilirsiniz, bu VM 'nin bir parçası değildir. Kubernetes kümesi, kendi VNet 'inde oluşturulur. Aşağıda, VM 'yi bulma ve içindeki AKS altyapısını bulma adımları verilmiştir:

1.  Azure Stack Kullanıcı portalını açın ve Kubernetes kümesi için belirttiğiniz kaynak grubunu bulun.
2.  Kaynak grubunda, dağıtım sanal makinesini bulun. Ad önekiyle başlar: **VMD-** .
3.  Dağıtım sanal makinesini seçin. Genel bakış bölümünde * * genel IP adresini bulun. VM 'ye bir SSH oturumu oluşturmak için bu adresi ve PuTTY gibi konsol uygulamanızı kullanın.
4.  Dağıtım VM 'sinin oturumunda, AKS altyapısını şu yolda bulabilirsiniz: `./var/lib/waagent/custom-script/download/0/bin/aks-engine`
5.  Aks-Engine ' e giriş olarak kullanılan kümeleri açıklayan `.json` dosyasını bulun. Dosya `/var/lib/waagent/custom-script/download/0/bin/azurestack.json`olarak. Dosyanın kümenizi dağıtmak için kullanılan hizmet sorumlusu kimlik bilgilerine sahip olduğunu unutmayın. Dosyayı korumaya karar verirseniz, dosyayı korumalı bir depoya aktarmayı dikkate alın.
6.  `/var/lib/waagent/custom-script/download/0/_output/<resource group name>`adresindeki AKS altyapısı tarafından oluşturulan çıkış dizinini bulun. Bu dizinde, `/var/lib/waagent/custom-script/download/0/bin/apimodel.json`yolundaki çıkış `apimodel.json` bulun. Dizin ve `apimodel.json` dosyası, Kubernetes kümesini dağıtmak için gereken tüm oluşturulan sertifikaları, anahtarları ve kimlik bilgilerini içerir. Bu kaynakları güvenli bir konuma depolayın.
7.  Genellikle **kubeconfig** dosyası olarak adlandırılan Kubernetes yapılandırma dosyasını bulun:, burada Azure Stack konum tanımınıza karşılık gelir. Bu dosya **kubectl** 'Yi Kubernetes kümenize erişmek için ayarlamayı planlıyorsanız yararlıdır.

## <a name="use-the-aks-engine-with-your-newly-created-cluster"></a>Yeni oluşturduğunuz kümeyle AKS altyapısını kullanma

Aks-Engine, giriş apimodel. json dosyasını, çıkış dizinini ve çıkış apimodel. json dosyasını bulduktan sonra, bunları güvenli bir yerde depolayın, tüm Linux sanal makineleri üzerinde AKS motoru ikili ve çıkış `apimodel.json` kullanabilirsiniz.

1.  AKS altyapısını kullanmaya devam etmek için, **yükseltme** ve **ölçeklendirme**gibi işlemleri gerçekleştirmek üzere **aks-Engine** ikili dosyasını hedef makineye kopyalayın. Aynı **VMD-** Machine ' i bir dizine kullanıyorsanız.

2.  Kümenin adı veya yeni kümeye başvuran diğer mnemotechnic adı ile bir dizin oluşturun ve çıktı apimodel. json dosyasını bu dosyaya kaydedin. Bu dosya kimlik bilgileri içerdiğinden korumalı bir yer olduğundan emin olun. Daha sonra, [Ölçek](azure-stack-kubernetes-aks-engine-scale.md) veya [yükseltme](azure-stack-kubernetes-aks-engine-upgrade.md) gibi işlemleri çalıştırmak için aks-Engine ' i çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerindeki AKS altyapısı](azure-stack-kubernetes-aks-engine-overview.md) hakkında bilgi edinin  
- [Azure Stack üzerindeki AKS altyapısının sorunlarını giderme](azure-stack-kubernetes-aks-engine-troubleshoot.md)  

