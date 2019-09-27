---
title: Microsoft Azure Stack sorunlarını giderme | Microsoft Docs
description: Sorun giderme Azure Stack.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 09/26/2019
ms.openlocfilehash: 865592d476eadaa847c4b46ff2a802f5fa0cc63e
ms.sourcegitcommit: 1bae55e754d7be75e03af7a4db3ec43fd7ff3e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71319084"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure Stack sorunlarını giderme

Bu belge Azure Stack tümleşik ortamlar için sorun giderme bilgileri sağlar. Azure Stack Geliştirme Seti yardım için bkz. [asdk sorun giderme](../asdk/asdk-troubleshooting.md) veya [Azure Stack MSDN forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)uzmanlardan yardım alın. 


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümler, Microsoft Müşteri Destek Hizmetleri 'ne (CSS) gönderilen genel soruları kapsayan docs bağlantılarını içerir.

### <a name="purchase-considerations"></a>Satın alma konuları

* [Nasıl satın alınır?](https://azure.microsoft.com/overview/azure-stack/how-to-buy/)
* [Azure Stack genel bakış](azure-stack-overview.md)

### <a name="updates-and-diagnostics"></a>Güncelleştirmeler ve Tanılamalar

* [Azure Stack 'de tanılama araçlarını kullanma](azure-stack-diagnostics.md)
* [Azure Stack sistem durumunu doğrulama](azure-stack-diagnostic-test.md)
* [Güncelleştirme paketi sürümü temposunda](azure-stack-servicing-policy.md#update-package-release-cadence)

### <a name="supported-operating-systems-and-sizes-for-guest-vms"></a>Konuk VM 'Ler için desteklenen işletim sistemleri ve boyutlar

* [Azure Stack üzerinde desteklenen konuk işletim sistemleri](azure-stack-supported-os.md)
* [Azure Stack desteklenen VM boyutları](../user/azure-stack-vm-sizes.md)

### <a name="azure-marketplace"></a>Azure Market

* [Azure Stack için kullanılabilen Azure Market öğeleri](azure-stack-marketplace-azure-items.md)

### <a name="manage-capacity"></a>Kapasiteyi yönetme

#### <a name="memory"></a>Bellek

Azure Stack'te kullanılabilir toplam bellek kapasitesini artırmak için bellek ekleyebilirsiniz. Azure Stack'te fiziksel sunucunuz ölçek birimi düğümü olarak da adlandırılır. Tek bir ölçek biriminin üyesi olan tüm ölçek birimi düğümlerinin [bellek miktarı aynı olmalıdır](azure-stack-manage-storage-physical-memory-capacity.md).

#### <a name="retention-period"></a>Elde tutma süresi

Saklama süresi ayarı bulut operatörünün silinen hesaplar için olası kurtarma süresi olarak gün cinsinden bir süre (0 ile 9999 gün arası) belirtmesine olanak tanır. Varsayılan saklama süresi **0** gün olarak ayarlanır. Değerin **0** olarak ayarlanması, silinen her hesabın hemen bekletme dışı ve düzenli çöp toplama için işaretlenme anlamına gelir.

* [Saklama süresini ayarlama](azure-stack-manage-storage-accounts.md#set-the-retention-period)

### <a name="security-compliance-and-identity"></a>Güvenlik, uyumluluk ve kimlik  

#### <a name="manage-rbac"></a>RBAC’yi yönetme

Azure Stack'teki kullanıcılar her abonelik, kaynak grubu veya hizmet örneği için okuyucu, sahip veya katkıda bulunan olabilir.

* [Azure Stack RBAC Yönetimi](azure-stack-manage-permissions.md)

Azure kaynaklarına yönelik yerleşik roller kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Bu öğretici için Azure PowerShell'i kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız.

* [Öğretici: Azure PowerShell kullanarak Azure kaynakları için özel rol oluşturma](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-powershell)

### <a name="manage-usage-and-billing-as-a-csp"></a>Kullanımını yönetmenize ve CSP olarak faturalama

* [CSP olarak kullanım ve faturalamayı yönetme](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)
* [CSP veya APSS aboneliği oluşturma](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)

Azure Stack için kullandığınız paylaşılan hizmetler hesabının türünü seçin. Çok kiracılı bir Azure Stack'in kaydında kullanılabilecek abonelik türleri şunlardır:

* Bulut Hizmeti Sağlayıcısı
* İş Ortağı Paylaşılan Hizmetler aboneliği


## <a name="troubleshoot-deployment"></a>Dağıtım sorunlarını giderme 
### <a name="general-deployment-failure"></a>Genel dağıtım hatası
Yükleme sırasında bir hata yaşarsanız dağıtım betiğinin-yeniden çalıştır seçeneğini kullanarak dağıtımı başarısız adımdan yeniden başlatabilirsiniz.  

### <a name="deployment-fails-due-to-lack-of-external-access"></a>Dış erişim olmaması nedeniyle dağıtım başarısız oluyor
Dış erişimin gerekli olduğu aşamalardan dağıtım başarısız olduğunda, aşağıdaki örnek gibi bir özel durum döndürülür:

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
Bu hata oluşursa, [dağıtım ağ trafiği belgelerini](deployment-networking.md)inceleyerek en düşük ağ gereksinimlerinin karşılandığından emin olun. İş ortağı araç setinin bir parçası olarak iş ortakları için de bir ağ denetleyicisi aracı vardır.

Diğer dağıtım hataları genellikle Internet üzerindeki kaynaklara bağlanma sorunlarından kaynaklanır.

Internet 'teki kaynakların bağlantısını doğrulamak için aşağıdaki adımları gerçekleştirebilirsiniz:

1. PowerShell 'i aç
2. WAS01 veya ERCs VM 'lerinden herhangi birine-PSSession girin
3. Commandlet 'i çalıştırın: Test-NetConnection login.windows.net-bağlantı noktası 443

Bu komut başarısız olursa, TOR anahtarını ve diğer tüm ağ aygıtlarını [ağ trafiğine izin verecek](azure-stack-network.md)şekilde yapılandırıldığını doğrulayın.

## <a name="troubleshoot-virtual-machines"></a>Sanal makinelerde sorun giderme
### <a name="default-image-and-gallery-item"></a>Varsayılan görüntü ve galeri öğesi
Azure Stack ' de VM 'Ler dağıtılmadan önce bir Windows Server görüntüsü ve galeri öğesi eklenmelidir.


### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk"></a>Bazı sanal makineleri sildim, ancak yine de diskte VHD dosyaları görüyorum
Bu davranış tasarıma göre belirlenir:

* Bir VM 'yi sildiğinizde, VHD 'ler silinmez. Diskler, kaynak grubundaki ayrı kaynaklardır.
* Bir depolama hesabı silindiğinde, silme işlemi hemen Azure Resource Manager aracılığıyla görünür, ancak içerebileceği diskler hala çöp toplama çalışana kadar depolama alanında tutulur.

"Artık" VHD 'ler görürseniz, silinen bir depolama hesabının klasörünün bir parçası olup olmadığını bilmeniz önemlidir. Depolama hesabı silinmediği için bunlar normaldir.

[Depolama hesaplarını yönetme](azure-stack-manage-storage-accounts.md)bölümünde bekletme eşiğini ve isteğe bağlı geri kazanma yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="troubleshoot-storage"></a>Depolama sorunlarını giderme
### <a name="storage-reclamation"></a>Depolama geri kazanma
Geri kazanılabilir kapasitenin portalda gösterilmesi 14 saate kadar sürebilir. Space geri kazanma, Blok Blobu deposundaki iç kapsayıcı dosyalarının kullanım yüzdesi gibi çeşitli faktörlere bağlıdır. Bu nedenle, ne kadar veri silindiğine bağlı olarak, çöp toplayıcı çalıştırıldığında geri kazanılabilecek alan miktarı garantisi yoktur.

