---
title: Bilinen sorunları Azure Stack
description: Azure Stack sürümlerindeki bilinen sorunlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 09/13/2019
ms.openlocfilehash: ba5b8358dced378f499c30f9b8d409497319fc86
ms.sourcegitcommit: 2c37ac8e88de19430080128bac1b70e33557d354
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164244"
---
# <a name="azure-stack-known-issues"></a>Bilinen sorunları Azure Stack

Bu makalede Azure Stack sürümlerindeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlandıkları için liste güncellenir.

Farklı bir sürüme yönelik bilinen sorunlara erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın.

::: moniker range=">=azs-1905"
> [!IMPORTANT]  
> Güncelleştirmeyi uygulamadan önce bu bölümü gözden geçirin.
::: moniker-end
::: moniker range="<azs-1905"
> [!IMPORTANT]  
> Azure Stack örneğiniz ikiden fazla güncelleştirme tarafından arkasındaysa uyumsuz olarak kabul edilir. [Destek almak için en az desteklenen sürüme güncelleştirmeniz](azure-stack-servicing-policy.md#keep-your-system-under-support)gerekir. 
::: moniker-end

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->

::: moniker range="azs-1908"
## <a name="1908-update-process"></a>1908 güncelleştirme işlemi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur.
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri yükleme bölümünü](azure-stack-apply-updates.md#install-updates-and-monitor-progress)izleyerek güncelleştirme paketini el Ile karşıya yüklemeniz önerilir.
- Oluşum Common

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sürüm 1804 ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltmesi Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum Common

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmez
- Düzeltmesi Bu abonelik özelliklerini, **Abonelikler genel bakış** dikey penceresinin **temel** bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum Common

### <a name="subscriptions-lock-blade"></a>Abonelikler kilit dikey penceresi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yönetici portalında, Kullanıcı aboneliklerinin **kilit** dikey penceresinde **abonelik**belirten iki buton vardır.
- Oluşum Common

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltmesi [İzinleri doğrulamak Için PowerShell 'i](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum Common

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltmesi SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum Common

## <a name="networking"></a>Ağ

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresi **hizmet uç noktalarını**kullanma seçeneğini gösterir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="network-interface"></a>Ağ arabirimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **Çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltmesi Ağ arabirimi eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum Common

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="local-network-gateway-deletion"></a>Yerel ağ geçidi silme

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **yerel ağ geçidini** silmek şu hata iletisini görüntüler: Etkin bir bağlantı olmasa bile, **etkin bir bağlantıyla yerel ağ geçidi**silinemez.
- Mayı Bu sorunun düzeltilmesi 1907 ' de yayımlanacak. Bu soruna yönelik bir geçici çözüm olarak, aynı IP adresi, adres alanı ve yapılandırma ayrıntıları farklı bir adla yeni bir yerel ağ geçidi oluşturmaktır. Ortam 1907 olarak güncelleştirildikten sonra eski LNG silinebilir.
- Oluşum Common

#### <a name="alerts"></a>Uyarılar

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="active-active"></a>Etkin-Etkin

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="documentation"></a>Belgeler

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sanal ağ geçidinin Genel Bakış sayfasındaki belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeleri için aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yeni bir Windows sanal makinesi (VM) oluştururken aşağıdaki hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı '** için seri çıkış ayarları güncelleştirilemedi. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltmesi Daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 4 düğümlü bir Azure Stack ortamında güncelleştirme işlemi sırasında, 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve sanal makine ölçek kümesi örneği oluşturma işlemi **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltmesi 2 hata etki alanı içeren bir kullanılabilirlik kümesinde tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltmesi Sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum Common

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.    
- Neden: Sanal makine Tanılama ayarları dikey penceresinde bir **Havuz** sekmesi bulunur ve bu bir Application Insights **hesabı**ister. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltmesi Yok.
- Oluşum Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-update-process"></a>1907 güncelleştirme işlemi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1907 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur.
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum Common

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sürüm 1804 ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltmesi Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum Common

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmez
- Düzeltmesi Bu abonelik özelliklerini, **Abonelikler genel bakış** dikey penceresinin **temel** bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum Common

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltmesi [İzinleri doğrulamak Için PowerShell 'i](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum Common

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltmesi SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum Common

## <a name="networking"></a>Ağ

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresi **hizmet uç noktalarını**kullanma seçeneğini gösterir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="network-interface"></a>Ağ arabirimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **Çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltmesi Ağ arabirimi eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum Common

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="alerts"></a>Uyarılar

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="active-active"></a>Etkin-Etkin

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="network-connection-type"></a>Ağ bağlantısı türü

- Uygunsa Bu sorun, 1906 veya 1907 ortamları için geçerlidir. 
- Neden: Kullanıcı portalında, **AddConnection** dikey penceresinde **VNET-VNET**' i kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor. 
- Oluşum Common 

#### <a name="documentation"></a>Belgeler

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sanal ağ geçidinin Genel Bakış sayfasındaki belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeleri için aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yeni bir Windows sanal makinesi (VM) oluştururken aşağıdaki hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı '** için seri çıkış ayarları güncelleştirilemedi. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltmesi Daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 4 düğümlü bir Azure Stack ortamında güncelleştirme işlemi sırasında, 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve sanal makine ölçek kümesi örneği oluşturma işlemi **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltmesi 2 hata etki alanı içeren bir kullanılabilirlik kümesinde tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltmesi Sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum Common

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygunsa Bu sorun 1906 ve 1907 sürümleri için geçerlidir.
- Neden: Ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygunsa Bu sorun 1906 ve 1907 sürümleri için geçerlidir.
- Neden: Sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygunsa Bu sorun 1906 ve 1907 sürümleri için geçerlidir.    
- Neden: Sanal makine Tanılama ayarları dikey penceresinde bir **Havuz** sekmesi bulunur ve bu bir Application Insights **hesabı**ister. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltmesi Yok.
- Oluşum Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-update-process"></a>1906 güncelleştirme işlemi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1906 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum Common

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sürüm 1804 ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltmesi Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum Common

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor.
- Düzeltmesi Önce kullanıcı kaynaklarını veya tüm kaynak grubunu silin ve ardından Kullanıcı aboneliklerini silin.
- Oluşum Common

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltmesi [İzinleri doğrulamak Için PowerShell 'i](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum Common

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi
- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmez
- Düzeltmesi Bu abonelikler özelliklerini, abonelikler genel bakış dikey penceresinin temel bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum Common

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltmesi SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum Common

### <a name="update"></a>Güncelleştirme

- Uygunsa Bu sorun 1906 sürümü için geçerlidir.
- Neden: Operatör portalında, düzeltmenin güncelleştirme durumu güncelleştirme için yanlış bir durum gösterir. İlk durum, güncelleştirme devam ediyor olsa bile güncelleştirmenin yüklenemediğini belirtir.
- Düzeltmesi Portalı yenileyin ve durum "sürüyor" olarak güncelleşecek.
- Oluşum Aralıklı

## <a name="networking"></a>Ağ

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresi **hizmet uç noktalarını**kullanma seçeneğini gösterir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

### <a name="network-interface"></a>Ağ arabirimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **Çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltmesi Ağ arabirimi eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum Common

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="alerts"></a>Uyarılar

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="active-active"></a>Etkin-Etkin

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum Common

#### <a name="documentation"></a>Belgeler

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sanal ağ geçidinin Genel Bakış sayfasındaki belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeler için lütfen aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç havuzu ekle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında bir **Load Balancer** **arka uç havuzu** eklemeye çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltmesi Arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum Common

#### <a name="create-inbound-nat"></a>Gelen NAT oluşturma

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir **Load Balancer**Için BIR **gelen NAT kuralı** oluşturmaya çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltmesi Arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum Common

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yeni bir Windows sanal makinesi (VM) oluştururken aşağıdaki hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı '** için seri çıkış ayarları güncelleştirilemedi. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltmesi Daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi


#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 4 düğümlü bir Azure Stack ortamında güncelleştirme işlemi sırasında, 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve sanal makine ölçek kümesi örneği oluşturma işlemi **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltmesi 2 hata etki alanı içeren bir kullanılabilirlik kümesinde tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltmesi Sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum Common

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygunsa Bu sorun 1906 sürümü için geçerlidir.
- Neden: Ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygunsa Bu sorun 1906 sürümü için geçerlidir.
- Neden: Sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltmesi Yok.
- Oluşum Common

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygunsa Bu sorun 1906 sürümü için geçerlidir.
- Neden: Sanal makine Tanılama ayarları dikey penceresinde bir **Havuz** sekmesi bulunur ve bu bir Application Insights **hesabı**ister. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltmesi Yok.
- Oluşum Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1905"
## <a name="1905-update-process"></a>1905 güncelleştirme işlemi

### <a name="host-node-update-prerequisite-failure"></a>Ana bilgisayar düğümü güncelleştirme önkoşulu hatası

- Uygunsa Bu sorun 1905 güncelleştirmesi için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında güncelleştirme durumu, **ana bilgisayar düğümü güncelleştirme önkoşulu**nedeniyle başarısız olabilir. Bu genellikle, yetersiz boş disk alanına sahip bir ana bilgisayar düğümü nedeniyle oluşur.
- Düzeltmesi Konak düğümündeki disk alanını temizleme hakkında yardım almak için Azure Stack desteğe başvurun.
- Oluşum Genel olmayan

### <a name="preparation-failed"></a>Hazırlama başarısız oldu

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 1905 güncelleştirme paketi, bu sorunun oluşma olasılığını daha büyük hale getirmek için önceki güncelleştirme paketlerinden daha büyüktür.
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum Common

## <a name="portal"></a>Portal

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor.
- Düzeltmesi Önce kullanıcı kaynaklarını veya tüm kaynak grubunu silin ve ardından Kullanıcı aboneliklerini silin.
- Oluşum Common

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltmesi [İzinleri doğrulamak Için PowerShell 'i](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum Common

### <a name="marketplace-management"></a>Market yönetimi

- Uygunsa Bu sorun 1904 ve 1905 için geçerlidir
- Neden: Yönetici portalında oturum açtığınızda Market yönetim ekranı görünür değildir.
- Düzeltmesi Tarayıcıyı yenileyin veya **Ayarlar** ' a gidin ve **varsayılan ayarlara Sıfırla**seçeneğini belirleyin.
- Oluşum Aralıklı

### <a name="docker-extension"></a>Docker uzantısı

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Hem yönetici hem de Kullanıcı portalında, **Docker**için arama yaparsanız öğe yanlış döndürülür. Azure Stack'te kullanılamıyor. Oluşturmayı denerseniz bir hata görüntülenir.
- Düzeltmesi Risk azaltma yok.
- Oluşum Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltmesi SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum Common

### <a name="template"></a>Şablon

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, şablon dağıtımı Kullanıcı arabirimi, "_" (alt çizgi karakteri) ile başlayan şablon adları için parametreleri doldurmaz.
- Düzeltmesi Şablon adından "_" (alt çizgi karakterini) kaldırın.
- Oluşum Common

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç havuzu ekle

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında bir **Load Balancer** **arka uç havuzu** eklemeye çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltmesi Arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum Common

#### <a name="create-inbound-nat"></a>Gelen NAT oluşturma

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir **Load Balancer**Için BIR **gelen NAT kuralı** oluşturmaya çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltmesi Arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum Common

#### <a name="create-load-balancer"></a>Yük dengeleyici oluştur

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **oluştur Load Balancer** penceresinde **Standart** yük dengeleyici SKU 'su oluşturma seçeneği gösterilir. Bu seçenek Azure Stack desteklenmez.
- Düzeltmesi Bunun yerine **temel** yük dengeleyici seçeneklerini kullanın.
- Oluşum Common

### <a name="public-ip-address"></a>Ortak IP adresi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında **genel IP adresi oluştur** penceresinde **Standart** SKU oluşturma seçeneği gösterilir. **Standart** SKU Azure Stack desteklenmez.
- Düzeltmesi Genel IP adresi için **temel** SKU 'yu kullanın.
- Oluşum Common

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Yeni bir Windows sanal makinesi (VM) oluştururken aşağıdaki hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı '** için seri çıkış ayarları güncelleştirilemedi.
Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltmesi Daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum Common

### <a name="vm-resize"></a>VM yeniden boyutlandırma

- Uygunsa Bu sorun 1905 sürümü için geçerlidir.
- Neden: Yönetilen bir disk VM 'si başarıyla yeniden boyutlandırılamıyor. Sanal makineyi yeniden boyutlandırmaya çalışmak "Code" ile bir hata oluşturur: "Internaloperationerror", "Message": "İşlemde bir iç hata oluştu."
- Düzeltmesi Sonraki sürümde bunu düzeltmek için çalışıyoruz. Şu anda VM 'yi yeni VM boyutuyla yeniden oluşturmanız gerekir.
- Oluşum Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="centos"></a>CentOS

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Sanal makine ölçek kümesi oluşturma deneyimi, dağıtım için bir seçenek olarak CentOS tabanlı 7,2 sağlar. CentOS 7,2, görüntünün bulunamadığını arayan dağıtım hatalarının oluşmasına neden olacak Azure Stack marketi 'nde kullanılamaz.
- Düzeltmesi Dağıtımınız için başka bir işletim sistemi seçin ya da Market 'ten bir operatör aracılığıyla dağıtımdan önce indirilen başka bir CentOS görüntüsünü belirten Azure Resource Manager şablonu kullanın.
- Oluşum Common

#### <a name="remove-scale-set"></a>Ölçek kümesini kaldır

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Ölçek kümesini **Sanal Makine Ölçek Kümeleri** dikey penceresinden kaldıramazsınız.
- Düzeltmesi Kaldırmak istediğiniz ölçek kümesini seçin, sonra **genel bakış** bölmesinden **Sil** düğmesine tıklayın.
- Oluşum Common

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 4 düğümlü bir Azure Stack ortamında güncelleştirme işlemi sırasında, 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve sanal makine ölçek kümesi örneği oluşturma işlemi **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltmesi 2 hata etki alanı içeren bir kullanılabilirlik kümesinde tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

#### <a name="scale-set-instance-view-blade-doesnt-load"></a>Ölçek kümesi örneği görünümü dikey penceresi yüklenmedi

- Uygunsa Bu sorun 1904 ve 1905 sürümü için geçerlidir.
- Neden: Azure Stack Portal-> Panosu-> Sanal Makine Ölçek Kümeleri-> AnyScaleSet-Instances-> AnyScaleSetInstance konumunda bulunan bir sanal makine ölçek kümesinin örnek görünümü dikey penceresi yükleme başarısız olur ve bir ağlayan bulut görüntüsünü görüntüler.
- Düzeltmesi Şu anda bir düzeltme yoktur ve bir düzeltme üzerinde çalışıyoruz. Bundan sonra, bir ölçek kümesinin örnek görünümünü `az vmss get-instance-view` almak için lütfen CLI komutunu kullanın.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltmesi Sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range=">=azs-1905"
## <a name="archive"></a>Arşiv

Daha eski bir sürüm için arşivlenmiş bilinen sorunlara erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın ve görmek istediğiniz sürümü seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme etkinliği denetim listesini gözden geçir](release-notes-checklist.md)
- [Güvenlik güncelleştirmelerinin listesini gözden geçirin](release-notes-security-updates.md)
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1904"
## <a name="1904-archived-known-issues"></a>1904 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-known-issues"></a>1903 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-known-issues"></a>1902 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-known-issues"></a>1901 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-known-issues"></a>1811 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-known-issues"></a>1809 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-known-issues"></a>1808 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-known-issues"></a>1807 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-known-issues"></a>1805 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-known-issues"></a>1804 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-known-issues"></a>1803 arşivlenmiş bilinen sorunlar
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-known-issues"></a>1802 arşivlenmiş bilinen sorunlar
::: moniker-end

::: moniker range="<azs-1905"
[Azure Stack bilinen sorunların eski sürümlerine TechNet Gallery ' de](https://aka.ms/azsarchivedrelnotes)erişebilirsiniz. Bu arşivlenmiş belgeler yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.
::: moniker-end