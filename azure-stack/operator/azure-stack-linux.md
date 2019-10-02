---
title: Azure Stack Linux görüntüleri ekleme
description: Azure Stack Linux görüntülerini eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 309d3d7185bd225f58691d4996ba649e8df7b97a
ms.sourcegitcommit: bbf3edbfc07603d2c23de44240933c07976ea550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714623"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack Linux görüntüleri ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Market'e Linux tabanlı bir görüntü ekleyerek Azure Stack üzerinde Linux sanal makineleri (VM) dağıtabilirsiniz. Azure Stack'e Linux görüntüsü eklemenin en kolay yolu, Market Yönetimi sayfasıdır. Bu görüntüler, Azure Stack ile uyumlu olacak şekilde hazırlanmış ve test edilmiştir.

## <a name="marketplace-management"></a>Market yönetimi

Azure Marketi 'nden Linux görüntülerini indirmek için [Azure 'dan Market öğelerini indirme Azure Stack makalesine yönelik](azure-stack-download-azure-marketplace-item.md) yordamları kullanın. Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin.

Bu görüntülere sık sık güncelleştirmeler vardır. bu nedenle, güncel tutmak için Market yönetimini sık sık denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

Mümkün olduğunda Market Yönetimi sayfasından görüntüleri indirin. Bu görüntüler, Azure Stack için hazırlanmış ve test edilmiştir.

### <a name="azure-linux-agent"></a>Azure Linux Aracısı

Azure Linux Aracısı (genellikle **walınuxagent** veya **walınuxagent**olarak adlandırılır) gereklidir ve aracının tüm sürümleri Azure Stack üzerinde çalışır. 2\.2.20 ve 2.2.35 arasındaki sürümler Azure Stack desteklenmez. En son aracı sürümlerini 2.2.35 üzerinde kullanmak için, 1901 düzeltmesini/1902 düzeltmesini uygulayın veya Azure Stack 1903 sürümüne (veya üzeri) güncelleştirin. [Cloud-init](https://cloud-init.io/) 'in şu anda Azure Stack desteklenmediğini unutmayın.

| Azure Stack derlemesi | Azure Linux Aracısı derlemesi |
| ------------- | ------------- |
| 1.1901.0.99 veya önceki sürümler | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 veya daha yeni |
| 1.1902.2.73  | 2.2.35 veya daha yeni |
| 1.1903.0.35  | 2.2.35 veya daha yeni |
| 1903 sonrasında derleme | 2.2.35 veya daha yeni |
| Desteklenmiyor | 2.2.21-2.2.34 |

Aşağıdaki yönergeleri kullanarak kendi Linux görüntünüzü hazırlayabilirsiniz:

* [CentOS Tabanlı Dağıtımlar](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES ve openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Görüntünüzü Market 'e ekleyin

[Görüntüyü Market 'e ekleyin](azure-stack-add-vm-image.md). @No__t-0 parametresinin `Linux` olarak ayarlandığından emin olun.

Görüntüyü Market 'e ekledikten sonra bir Market öğesi oluşturulur ve kullanıcılar bir Linux sanal makinesini dağıtabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
* [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md)
