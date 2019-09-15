---
title: Infrastructure Backup hizmeti ile Azure Stack verileri kurtarma | Microsoft Docs
description: Infrastructure Backup hizmetini kullanarak Azure Stack yapılandırma ve hizmet verilerini yedekleme ve geri yükleme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 832d2146e79f3724c5f1b3bf9da1776388636da8
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974714"
---
# <a name="recover-data-in-azure-stack-with-the-infrastructure-backup-service"></a>Infrastructure Backup hizmeti ile Azure Stack verileri kurtarma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack Infrastructure Backup hizmetini kullanarak yapılandırma ve hizmet verilerini yedekleyebilir ve geri yükleyebilirsiniz. Her Azure Stack yüklemesi hizmetin bir örneğini içerir. Kimlik, güvenlik ve Azure Resource Manager verilerini geri yüklemek için, hizmet tarafından oluşturulan yedeklemeleri Azure Stack bulutun yeniden dağıtımı için kullanabilirsiniz.

Bulutunuzu üretime koymaya hazırsanız yedeklemeyi etkinleştirin. Uzun bir süre için test ve doğrulama gerçekleştirmeyi planlıyorsanız yedeklemeyi etkinleştirmeyin.

Yedekleme hizmetinizi etkinleştirmeden önce, [gereksinimlerin yerinde](#verify-requirements-for-the-infrastructure-backup-service)olduğundan emin olun.

> [!Note]  
> Infrastructure Backup hizmeti kullanıcı verileri ve uygulamaları içermez. IaaS VM tabanlı uygulamaları koruma hakkında daha fazla bilgi için bkz. [Azure Stack dağıtılmış VM 'leri koruma](../user/azure-stack-manage-vm-protect.md). Azure Stack uygulamaları nasıl koruyabileceğiniz hakkında kapsamlı bilgi için, [iş sürekliliği ve olağanüstü durum kurtarma teknik incelemesi için Azure Stack konulara](https://aka.ms/azurestackbcdrconsiderationswp)bakın.

## <a name="the-infrastructure-backup-service"></a>Infrastructure Backup hizmeti

Hizmet aşağıdaki özellikleri içerir:

| Özellik                                            | Açıklama                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Altyapı hizmetlerini yedekleme                     | Azure Stack içindeki bir altyapı hizmetleri alt kümesi üzerinde yedeklemeyi koordine edin. Bir olağanüstü durum varsa, veriler yeniden dağıtım kapsamında geri yüklenebilir. |
| İçe aktarılmış yedekleme verilerinin sıkıştırılması ve şifrelenmesi | Yedekleme verileri, yönetici tarafından belirtilen dış depolama konumuna aktarılmadan önce sistem tarafından sıkıştırılır ve şifrelenir.                |
| Yedekleme işi izleme                              | Yedekleme işleri başarısız olduğunda ve sorunun nasıl düzeltileceğini sistem size bildirir.                                                                                                |
| Yedekleme yönetimi deneyimi                       | Backup RP, yedeklemeyi etkinleştirmeyi destekler.                                                                                                                         |
| Bulut kurtarma                                     | Çok önemli bir veri kaybı varsa, dağıtım kapsamında çekirdek Azure Stack bilgilerini geri yüklemek için yedeklemeler kullanılabilir.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Infrastructure Backup hizmeti için gereksinimleri doğrulama

- **Depolama konumu**  
  Yedi yedekleme içerebilen Azure Stack erişilebilir bir dosya paylaşımınız olması gerekir. Her yedekleme yaklaşık 10 GB 'tır. Paylaşımınızın 140 GB 'lık yedeklemeleri depolaması gerekir. Infrastructure Backup hizmeti için bir depolama konumu seçme hakkında daha fazla bilgi için bkz. [yedekleme denetleyicisi gereksinimleri](azure-stack-backup-reference.md#backup-controller-requirements).
- **Kimlik Bilgileri**  
  Bir etki alanı kullanıcı hesabı ve kimlik bilgileri gereklidir. Örneğin, Azure Stack yönetici kimlik bilgilerinizi kullanabilirsiniz.
- **Şifreleme sertifikası**  
  Yedekleme dosyaları, sertifikadaki ortak anahtar kullanılarak şifrelenir. Bu sertifikayı güvenli bir yerde sakladığınızdan emin olun. 


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack yedeklemesini yönetici portalından nasıl etkinleştirebileceğinizi](azure-stack-backup-enable-backup-console.md)öğrenin.

[Azure Stack Için yedeklemeyi PowerShell ile nasıl etkinleştireceğinizi](azure-stack-backup-enable-backup-powershell.md)öğrenin.

[Azure Stack nasıl yedekleyeceğinizi](azure-stack-backup-back-up-azure-stack.md)öğrenin.

Çok [önemli veri kaybını nasıl kurtaracağınızı](azure-stack-backup-recover-data.md)öğrenin.
