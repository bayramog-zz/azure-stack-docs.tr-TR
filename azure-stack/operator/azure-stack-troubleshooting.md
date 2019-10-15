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
ms.date: 10/09/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 10/09/2019
ms.openlocfilehash: 45600ee6e35c7e80a1dd74c6c6d456b3bf68ed3f
ms.sourcegitcommit: 5eae057cb815f151e6b8af07e3ccaca4d8e4490e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310529"
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

### <a name="azure-marketplace"></a>Azure Marketi

* [Azure Stack için kullanılabilen Azure Market öğeleri](azure-stack-marketplace-azure-items.md)

### <a name="manage-capacity"></a>Kapasiteyi yönetme

#### <a name="memory"></a>Hafıza

Azure Stack'te kullanılabilir toplam bellek kapasitesini artırmak için bellek ekleyebilirsiniz. Azure Stack'te fiziksel sunucunuz ölçek birimi düğümü olarak da adlandırılır. Tek bir ölçek biriminin üyesi olan tüm ölçek birimi düğümlerinin [bellek miktarı aynı olmalıdır](azure-stack-manage-storage-physical-memory-capacity.md).

#### <a name="retention-period"></a>Bekletme dönemi

Saklama süresi ayarı bulut operatörünün silinen hesaplar için olası kurtarma süresi olarak gün cinsinden bir süre (0 ile 9999 gün arası) belirtmesine olanak tanır. Varsayılan saklama süresi **0** gün olarak ayarlanır. Değerin **0** olarak ayarlanması, silinen her hesabın hemen bekletme dışı ve düzenli çöp toplama için işaretlenme anlamına gelir.

* [Saklama süresini ayarlama](azure-stack-manage-storage-accounts.md#set-the-retention-period)

### <a name="security-compliance-and-identity"></a>Güvenlik, uyumluluk ve kimlik  

#### <a name="manage-rbac"></a>RBAC’yi yönetme

Azure Stack'teki kullanıcılar her abonelik, kaynak grubu veya hizmet örneği için okuyucu, sahip veya katkıda bulunan olabilir.

* [Azure Stack RBAC Yönetimi](azure-stack-manage-permissions.md)

Azure kaynaklarına yönelik yerleşik roller kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Bu öğretici için Azure PowerShell'i kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız.

* [Öğretici: Azure PowerShell kullanarak Azure kaynakları için özel bir rol oluşturma](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-powershell)

### <a name="manage-usage-and-billing-as-a-csp"></a>CSP olarak kullanım ve faturalamayı yönetme

* [CSP olarak kullanım ve faturalamayı yönetme](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)
* [CSP veya APSS aboneliği oluşturma](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)

Azure Stack için kullandığınız paylaşılan hizmetler hesabının türünü seçin. Çok kiracılı bir Azure Stack'in kaydında kullanılabilecek abonelik türleri şunlardır:

* Bulut Çözümü Sağlayıcısı
* İş Ortağı Paylaşılan Hizmetler aboneliği

### <a name="get-scale-unit-metrics"></a>Ölçek birimi ölçümlerini al

PowerShell 'i kullanarak damga kullanım bilgilerini CSS 'den yardım etmeden alabilirsiniz. Damga kullanımını almak için: 

1. PEP oturumu oluşturma
2. Test-azurestack Çalıştır
3. PEP oturumundan çık
4. Invoke komutu çağrısı kullanarak Get-azurestacklog-filterbyrole seedring komutunu çalıştırın
5. Seedring. zip dosyasını ayıklayın ve test-azurestack komutunu çalıştırdığınız ERCS klasöründen doğrulama raporunu elde edebilirsiniz

Daha fazla bilgi için bkz. [tanılama Azure Stack](azure-stack-configure-on-demand-diagnostic-log-collection.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems).

## <a name="troubleshoot-deployment"></a>Dağıtım sorunlarını giderme 
### <a name="general-deployment-failure"></a>Genel dağıtım hatası
Yükleme sırasında bir hata yaşarsanız dağıtım betiğinin-yeniden çalıştır seçeneğini kullanarak dağıtımı başarısız adımdan yeniden başlatabilirsiniz.  

### <a name="template-validation-error-parameter-osprofile-is-not-allowed"></a>Şablon doğrulama hatası parametre osProfile izin verilmiyor

Şablon doğrulaması sırasında ' osProfile ' parametresine izin verilmediğinden bir hata mesajı alırsanız, bu bileşenler için API 'lerin doğru sürümlerini kullandığınızdan emin olun:

- [İşlem](https://docs.microsoft.com/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftcompute)
- [Ağ](https://docs.microsoft.com/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftnetwork)

Azure 'dan Azure Stack bir VHD 'yi kopyalamak için [AzCopy 7.3.0](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#download-and-install-azcopy)kullanın. Görüntüyle birlikte çalışarak görüntünün kendisiyle ilgili sorunları giderin. Azure Stack için Walınuxagent gereksinimleri hakkında daha fazla bilgi için bkz. [Azure Linux Aracısı](azure-stack-linux.md#azure-linux-agent).

### <a name="deployment-fails-due-to-lack-of-external-access"></a>Dış erişim olmaması nedeniyle dağıtım başarısız oluyor
Dış erişimin gerekli olduğu aşamalardan dağıtım başarısız olduğunda, aşağıdaki örnek gibi bir özel durum döndürülür:

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
Bu hata oluşursa, [dağıtım ağ trafiği belgelerini](deployment-networking.md)inceleyerek tüm en düşük ağ gereksinimlerinin karşılandığından emin olun. İş ortağı araç setinin bir parçası olarak iş ortakları için de bir ağ denetleyicisi aracı vardır.

Diğer dağıtım hataları genellikle Internet üzerindeki kaynaklara bağlanma sorunlarından kaynaklanır.

Internet 'teki kaynakların bağlantısını doğrulamak için aşağıdaki adımları gerçekleştirebilirsiniz:

1. PowerShell’i açın.
2. WAS01 veya ERCs VM 'lerinden herhangi birine-PSSession yazın.
3. Aşağıdaki cmdlet'i çalıştırın: 
   ```powershell
   Test-NetConnection login.windows.net -port 443
   ```

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

## <a name="troubleshooting-app-service"></a>Sorun giderme App Service
### <a name="create-aadidentityappps1-script-fails"></a>Create-AADIdentityApp. ps1 betiği başarısız oluyor

App Service için gerekli olan Create-AADIdentityApp. ps1 betiği başarısız olursa, betiği çalıştırırken gerekli-AzureStackAdminCredential parametresini eklediğinizden emin olun. Daha fazla bilgi için, [Azure Stack App Service dağıtmaya yönelik önkoşullar](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app)bölümüne bakın.

