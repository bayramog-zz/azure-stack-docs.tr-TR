---
title: Azure Stack'e Linux görüntüleri ekleme
description: Linux görüntüleri için Azure Stack eklemeyi öğrenin.
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
ms.date: 05/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 8e9617974984afac55c2b1542065ea0455da0acc
ms.sourcegitcommit: 5a720b17bd6a5aab44929c0247db8d512e0669ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67197143"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack'e Linux görüntüleri ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Market'e Linux tabanlı bir görüntü ekleyerek Azure Stack üzerinde Linux sanal makineleri (VM) dağıtabilirsiniz. Azure Stack'e Linux görüntüsü eklemenin en kolay yolu, Market Yönetimi sayfasıdır. Bu görüntüler, Azure Stack ile uyumlu olacak şekilde hazırlanmış ve test edilmiştir.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Market'teki Linux görüntüleri indirmek için yordamları kullanın. [indirme Market öğesi Azure'dan Azure Stack'e](azure-stack-download-azure-marketplace-item.md) makalesi. Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin.

Bu görüntüler için sık sık güncelleştirme vardır, bu nedenle Market genellikle güncel tutmak için yönetim denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

Mümkün olduğunda, Market yönetim sağlanan görüntülerin indirin. Bu görüntüleri hazırlanan ve Azure Stack için test.

### <a name="azure-linux-agent"></a>Azure Linux Aracısı

Azure Linux Aracısı (genellikle adlı `WALinuxAgent` veya `walinuxagent`) gerekli ve tüm sürümler Azure Stack'te aracı iş. 2\.2.20 2.2.35 arasındaki sürümleri, Azure Stack üzerinde desteklenmez. En son Aracı sürümleri 2.2.35 üstünde kullanılacak 1901 düzeltme/1902 düzeltmeyi veya Azure Stack 1903 sürümüne (veya üzeri) güncelleştirin. Unutmayın [cloud-init](https://cloud-init.io/) Azure Stack üzerinde şu anda desteklenmiyor.

| Azure Stack derleme | Azure Linux Aracısı oluşturma |
| ------------- | ------------- |
| 1.1901.0.99 veya önceki sürümleri | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 ya da daha yeni |
| 1.1902.2.73  | 2.2.35 ya da daha yeni |
| 1.1903.0.35  | 2.2.35 ya da daha yeni |
| Sonra 1903 oluşturur | 2.2.35 ya da daha yeni |
| Desteklenmiyor | 2.2.21-2.2.34 |

Aşağıdaki yönergeleri kullanarak kendi Linux görüntünüzü hazırlayabilirsiniz:

* [CentOS Tabanlı Dağıtımlar](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES ve openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Market'te görüntünüzü ekleme

İzleyin [ve Market görüntüsü ekleme](azure-stack-add-vm-image.md). Emin olun `OSType` parametrenin ayarlanmış `Linux`.

Görüntü Marketi'nde ekledikten sonra bir Market öğesi oluşturulur ve kullanıcıların bir Linux sanal makinesi olarak dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
* [Azure Stack Marketini genel bakış](azure-stack-marketplace.md)
