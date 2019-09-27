---
title: ASDK sorunlarını giderme | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) sorunlarını giderme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: ab43d94c2e65032e5e525ec000e38cacb01b2980
ms.sourcegitcommit: 1bae55e754d7be75e03af7a4db3ec43fd7ff3e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71319098"
---
# <a name="troubleshoot-the-asdk"></a>ASDK sorunlarını giderme
Bu makalede Azure Stack Geliştirme Seti (ASDK) için genel sorun giderme bilgileri sağlanmaktadır. Azure Stack tümleşik sistemlerle ilgili yardım için bkz. [Microsoft Azure Stack sorunlarını giderme](../operator/azure-stack-troubleshooting.md). 

ASDK bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) destek sağlamaz. Belgelenmemiş bir sorun yaşıyorsanız, [Azure Stack MSDN forumundan](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)uzmanlardan yardım alabilirsiniz. 


## <a name="deployment"></a>Dağıtım
### <a name="deployment-failure"></a>Dağıtım hatası
Yükleme sırasında bir hata yaşarsanız dağıtım betiğinin-yeniden çalıştır seçeneğini kullanarak dağıtımı başarısız adımdan yeniden başlatabilirsiniz. Örneğin:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Dağıtımın sonunda, PowerShell oturumu hala açık ve herhangi bir çıkış göstermez
Bu davranış, bir PowerShell komut penceresi seçildiğinde yalnızca varsayılan davranışın sonucudur. ASDK dağıtımı başarılı oldu ancak pencere seçilirken betik duraklatıldı. Komut penceresinin başlık çubuğunda "Seç" sözcüğünü arayarak kurulumun tamamlandığını doğrulayabilirsiniz. Bu seçeneğin seçimini kaldırmak için ESC tuşuna basın ve sonra tamamlanma iletisi gösterilmelidir.

## <a name="virtual-machines"></a>Sanal makineler
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Azure Stack ' de VM 'Ler dağıtılmadan önce bir Windows Server görüntüsü ve galeri öğesi eklenmelidir.

### <a name="after-restarting-my-azure-stack-host-some-vms-dont-automatically-start"></a>Azure Stack ana makinelerimi yeniden başlattıktan sonra bazı VM 'Ler otomatik olarak başlatılmaz
Ana bilgisayarınızı yeniden başlattıktan sonra Azure Stack hizmetleri hemen kullanılabilir olmayabilir. Bunun nedeni, Azure Stack [altyapı VM](asdk-architecture.md#virtual-machine-roles) 'Lerinin ve RPS 'nin tutarlılığı denetlemek için biraz zaman sürmesine, ancak sonunda otomatik olarak başlayacak.

Ayrıca, Kiracı VM 'lerinin, ASDK konağının yeniden başlatıldıktan sonra otomatik olarak başlamadığına da dikkat edebilirsiniz. Birkaç el ile adımları izleyerek bunları çevrimiçi duruma getirebilirsiniz:

1.  ASDK ana bilgisayarında başlangıç menüsünden **Yük devretme kümesi Yöneticisi** başlatın.
2.  **S-Cluster. azurestack. Local**kümesini seçin.
3.  **Rolleri**seçin.
4.  Kiracı VM 'Leri *kaydedilmiş* durumda görüntülenir. Tüm altyapı VM 'Leri çalışır olduktan sonra, Kiracı VM 'lerine sağ tıklayıp **Başlat** ' ı seçerek VM 'yi sürdürebilirsiniz.

### <a name="ive-deleted-some-vms-but-still-see-the-vhd-files-on-disk"></a>Bazı VM 'Leri sildim, ancak yine de diskte VHD dosyaları görüyorum 
Bu davranış tasarıma göre belirlenir:

* Bir VM 'yi sildiğinizde, VHD 'ler silinmez. Diskler, kaynak grubundaki ayrı kaynaklardır.
* Bir depolama hesabı silindiğinde, silme işlemi hemen Azure Resource Manager aracılığıyla görünür, ancak içerebileceği diskler hala çöp toplama çalışana kadar depolama alanında tutulur.

"Artık" VHD 'ler görürseniz, silinen bir depolama hesabının klasörünün bir parçası olup olmadığını bilmeniz önemlidir. Depolama hesabı silinmediyse, VHD 'Lerin kalması normaldir.

[Depolama hesaplarını yönetme](../operator/azure-stack-manage-storage-accounts.md)bölümünde bekletme eşiğini ve isteğe bağlı geri kazanma yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="storage"></a>Depolama
### <a name="storage-reclamation"></a>Depolama geri kazanma
Geri kazanılan kapasitenin portalda gösterilmesi 14 saate kadar sürebilir. Space geri kazanma, Blok Blobu deposundaki iç kapsayıcı dosyalarının kullanım yüzdesi gibi çeşitli faktörlere bağlıdır. Bu nedenle, ne kadar veri silindiğine bağlı olarak, çöp toplayıcı çalıştırıldığında geri kazanılabilecek alan miktarı garantisi yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack destek forumunu ziyaret edin](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)
