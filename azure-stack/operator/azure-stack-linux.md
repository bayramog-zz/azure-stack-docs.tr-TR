---
title: Azure Stack'e Linux görüntüleri ekleme
description: Bilgi nasıl Linux görüntüleri Azure Stack'e ekleme.
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
ms.date: 02/15/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 2f7f0c55f02fd99a419619d878be8300d7326303
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64296792"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack'e Linux görüntüleri ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Market'e Linux tabanlı bir görüntü ekleyerek Azure Stack üzerinde Linux sanal makineleri (VM) dağıtabilirsiniz. Azure Stack'e Linux görüntüsü eklemenin en kolay yolu, Market Yönetimi sayfasıdır. Bu görüntüler, Azure Stack ile uyumlu olacak şekilde hazırlanmış ve test edilmiştir.

## <a name="marketplace-management"></a>Market Yönetimi

Azure Market'teki Linux görüntüleri indirmek için yordamları kullanın. [indirme Market öğesi Azure'dan Azure Stack'e](azure-stack-download-azure-marketplace-item.md) makalesi. Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin. 

Bu görüntüler için sık sık güncelleştirme olduğunu unutmayın, bu nedenle Market genellikle güncel tutmak için yönetim denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

Mümkün olduğunda Market Yönetimi sayfasından Azure Stack için hazırlanmış ve test edilmiş olan görüntüleri indirin.

### <a name="azure-linux-agent"></a>Azure Linux Aracısı
Azure Linux Aracısı (genellikle adlı `WALinuxAgent` veya `walinuxagent`) gereklidir ve aracıyı'nin tüm sürümleri Azure Stack'te çalışır. 2.2.20 2.2.35 arasındaki sürümleri, Azure Stack üzerinde desteklenmez. En son Aracı sürümleri 2.2.35 yukarıda kullanmak için lütfen 1901 düzeltme/1902 düzeltmeyi veya Azure Stack 1903 sürümüne (veya üzeri) güncelleştirin. Unutmayın [cloud-init](https://cloud-init.io/) Azure Stack üzerinde şu anda desteklenmiyor.

| Azure Stack derleme | Azure Linux Aracısı oluşturma |
| ------------- | ------------- |
| 1.1901.0.99 veya önceki sürümleri | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 ya da daha yeni |
| 1.1902.2.73  | 2.2.35 ya da daha yeni |
| 1.1903.0.35  | 2.2.35 ya da daha yeni |
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

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
- [Azure Stack Marketini genel bakış](azure-stack-marketplace.md)
