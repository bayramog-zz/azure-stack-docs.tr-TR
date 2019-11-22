---
title: Azure Stack AKS Altyapısı nedir? | Microsoft Docs
description: Azure ve Azure Stack bir Kubernetes kümesi dağıtmak ve yönetmek için AKS Altyapısı komut satırı aracını nasıl kullanacağınızı öğrenin.
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
ms.date: 11/21/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: cb531570191216ea13a0b1a7a6d36738c0765cd3
ms.sourcegitcommit: 0b783e262ac87ae67929dbd4c366b19bf36740f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310247"
---
# <a name="what-is-the-aks-engine-on-azure-stack"></a>Azure Stack AKS Altyapısı nedir?

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure ve Azure Stack bir Kubernetes kümesi dağıtmak ve yönetmek için AKS Altyapısı komut satırı aracını kullanabilirsiniz. Yerel kümeler Azure Resource Manager oluşturmak, yükseltmek ve ölçeklendirmek için AKS altyapısını kullanın. Bir kümeyi hem bağlı hem de bağlantısı kesik ortamlarda dağıtmak için altyapısını kullanabilirsiniz. Bu makalede, AKS altyapısına genel bakış, Azure Stack ile altyapıyı kullanmaya yönelik desteklenen senaryolar ve dağıtma, yükseltme ve ölçeklendirme gibi işlemlere giriş sunulmaktadır.

## <a name="overview-of-the-aks-engine"></a>AKS altyapısına genel bakış

[Aks altyapısı](https://github.com/Azure/aks-engine) , Azure 'Da Kubernetes kümelerini önyüklemek için bir komut satırı aracı sağlar ve Azure Stack. AKS motoru, Azure Resource Manager kullanarak VM 'lerde, sanal ağlarda ve Azure Stack hizmet olarak altyapı (IaaS) kaynaklarında çalışan kümeler oluşturmanıza ve bakımını yapmanıza yardımcı olur.

## <a name="aks-engine-on-azure-stack-considerations"></a>AKS altyapısı Azure Stack konuları

Azure Stack AKS altyapısını kullanmadan önce, Azure Stack ve Azure arasındaki farkları anlamak önemlidir. Bu bölüm, Kubernetes kümenizi yönetmek için AKS altyapısıyla Azure Stack kullanırken farklı özellikleri ve önemli konuları tanımlar.

Azure Stack üzerindeki AKS altyapısının özellikleri ve Azure 'a yönelik farklılıklar hakkında daha fazla bilgi için [Azure Stack adresindeki aks altyapısına](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md)bakın.

## <a name="supported-scenarios-with-the-aks-engine"></a>AKS altyapısıyla desteklenen senaryolar

Aşağıdaki senaryolar Azure Stack destek ekibi tarafından desteklenir:

1.  AKS altyapısı, bu belgelerdeki yönergeleri izleyerek ve [Aşağıdaki şablonu](https://github.com/Azure/aks-engine/tree/master/examples/azure-stack)kullanarak tüm küme yapıtları dağıtır.
2.  AKS altyapısı, kümeyi mevcut bir VNET üzerinde dağıtır. Daha fazla bilgi için bkz. [AKS altyapısı ile özel bir sanal ağ kullanma](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/custom-vnet.md).
3.  [Yükseltme](azure-stack-kubernetes-aks-engine-upgrade.md) ve [ölçeklendirme](azure-stack-kubernetes-aks-engine-scale.md) işlemleri.

AKS altyapısı ve Azure Stack hakkında daha fazla bilgi için bkz. [Azure Stack aks motoru Için destek ilkeleri](azure-stack-kubernetes-aks-engine-support.md).

## <a name="install-the-aks-engine-and-deploy-a-kubernetes-cluster"></a>AKS altyapısını yükleyip bir Kubernetes kümesi dağıtma

Bir Kubernetes kümesini, Azure Stack AKS altyapısıyla birlikte dağıtmak için:

1. [AKS altyapısı için önkoşulları ayarlama](azure-stack-kubernetes-aks-engine-set-up.md)
2. AKS altyapısını Azure Stack ortamınıza erişimi olan bir makineye yükler.
     - [AKS altyapısını Windows üzerinde Azure Stack](azure-stack-kubernetes-aks-engine-deploy-windows.md)
     - [Linux 'ta AKS altyapısını Azure Stack ' de yükler](azure-stack-kubernetes-aks-engine-deploy-linux.md)
3. [Bir Kubernetes kümesini AKS altyapısı ile Azure Stack dağıtma](azure-stack-kubernetes-aks-engine-deploy-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [AKS altyapısı için önkoşulları ayarlama](azure-stack-kubernetes-aks-engine-set-up.md)