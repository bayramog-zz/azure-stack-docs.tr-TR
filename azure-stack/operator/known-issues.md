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
ms.openlocfilehash: 367a2cb5a07c0db2b272b8ffc951f51e5ed9cc0e
ms.sourcegitcommit: bb2bbfad8061f7677954f6ce5a435b4e6f9299b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74100039"
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

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur.
- Düzeltme: sürüm 1901 ' den (1.1901.0.95) başlayarak, **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri yükleme bölümünü](azure-stack-apply-updates.md#install-updates-and-monitor-progress)izleyerek güncelleştirme paketini el Ile karşıya yüklemeniz önerilir.
- Oluşum: ortak

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1804 sürümü ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltme: Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum: ortak

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmedi
- Düzeltme: Bu abonelik özelliklerini, **Abonelikler genel bakış** dikey penceresinin **temel** bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum: ortak

### <a name="subscriptions-lock-blade"></a>Abonelikler kilit dikey penceresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yönetici portalında, Kullanıcı aboneliklerinin **kilit** dikey penceresinde **abonelik**belirten iki buton vardır.
- Oluşum: ortak

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portallarını kullanarak aboneliğinize ilişkin izinleri görüntüleyemezsiniz.
- Düzeltme: [izinleri doğrulamak Için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum: ortak

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltme: SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum: ortak

## <a name="networking"></a>Networking (Ağ İletişimi)

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: bir NSG 'de açık bir **Denyalloutbağlanacak** kural oluşturulamıyor çünkü bu, VM dağıtımının tamamlanabilmesi için gereken altyapıya yönelik tüm iç iletişimin tamamlanmasını engelleyecek.
- Oluşum: ortak

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: bir VM 'nin birincil NIC 'si değiştirilemez. Birincil NIC 'nin silinmesi/ayrılması, sanal makinenin başlatılmasına neden olur.
- Oluşum: ortak

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="local-network-gateway-deletion"></a>Yerel ağ geçidi silme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **yerel ağ geçidini** silmek şu hata iletisini görüntüler: etkin bir bağlantı olmasa bile, **etkin bir bağlantıyla yerel ağ geçidi silinemez**.
- Risk azaltma: Bu sorunun düzeltilmesi 1907 ' de yayımlanacak. Bu soruna yönelik bir geçici çözüm olarak, aynı IP adresi, adres alanı ve yapılandırma ayrıntıları farklı bir adla yeni bir yerel ağ geçidi oluşturmaktır. Ortam 1907 olarak güncelleştirildikten sonra eski LNG silinebilir.
- Oluşum: ortak

#### <a name="alerts"></a>Uyarılar

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="active-active"></a>Etkin-Etkin

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="documentation"></a>Belgeler

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: sanal ağ geçidinin Genel Bakış sayfasında bulunan belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeleri için aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yeni bir Windows sanal makinesi (VM) oluştururken şu hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı ' için seri çıkış ayarları güncellenemedi**. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltme: daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum: ortak

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve bir sanal makine ölçek kümesi örneği oluşturma işlemi, 4 düğümlü Azure Stack ortamında güncelleştirme işlemi sırasında **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltme: bir kullanılabilirlik kümesinde 2 hata etki alanı başarıyla bulunan tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltme: sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum: ortak

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.    
- Neden: sanal makine Tanılama ayarları dikey penceresinde, bir **Application Insights hesabı**ısteyen bir **Havuz** sekmesi vardır. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltme: yok.
- Oluşum: ortak

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-update-process"></a>1907 güncelleştirme işlemi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1907 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur.
- Düzeltme: sürüm 1901 ' den (1.1901.0.95) başlayarak, **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum: ortak

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1804 sürümü ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltme: Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum: ortak

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmedi
- Düzeltme: Bu abonelik özelliklerini, **Abonelikler genel bakış** dikey penceresinin **temel** bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum: ortak

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portallarını kullanarak aboneliğinize ilişkin izinleri görüntüleyemezsiniz.
- Düzeltme: [izinleri doğrulamak Için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum: ortak

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltme: SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum: ortak

## <a name="networking"></a>Networking (Ağ İletişimi)

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="alerts"></a>Uyarılar

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="active-active"></a>Etkin-Etkin

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-connection-type"></a>Ağ bağlantısı türü

- Uygulanabilir: Bu sorun, 1906 veya 1907 ortamları için geçerlidir. 
- Neden: Kullanıcı portalında, **AddConnection** dikey penceresinde **VNET-VNET**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor. 
- Oluşum: ortak 

#### <a name="documentation"></a>Belgeler

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: sanal ağ geçidinin Genel Bakış sayfasında bulunan belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeleri için aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yeni bir Windows sanal makinesi (VM) oluştururken şu hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı ' için seri çıkış ayarları güncellenemedi**. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltme: daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum: ortak

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve bir sanal makine ölçek kümesi örneği oluşturma işlemi, 4 düğümlü Azure Stack ortamında güncelleştirme işlemi sırasında **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltme: bir kullanılabilirlik kümesinde 2 hata etki alanı başarıyla bulunan tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltme: sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum: ortak

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygulanabilir: Bu sorun 1906 ve 1907 sürümleri için geçerlidir.
- Neden: ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygulanabilir: Bu sorun 1906 ve 1907 sürümleri için geçerlidir.
- Neden: sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygulanabilir: Bu sorun 1906 ve 1907 sürümleri için geçerlidir.    
- Neden: sanal makine Tanılama ayarları dikey penceresinde, bir **Application Insights hesabı**ısteyen bir **Havuz** sekmesi vardır. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltme: yok.
- Oluşum: ortak

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-update-process"></a>1906 güncelleştirme işlemi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1906 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 
- Düzeltme: sürüm 1901 ' den (1.1901.0.95) başlayarak, **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum: ortak

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1804 sürümü ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltme: Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum: ortak

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı aboneliklerinin silinmesi yalnız bırakılmış kaynaklarda sonuçlanır.
- Düzeltme: önce kullanıcı kaynaklarını veya tüm kaynak grubunu silin ve ardından Kullanıcı aboneliklerini silin.
- Oluşum: ortak

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portallarını kullanarak aboneliğinize ilişkin izinleri görüntüleyemezsiniz.
- Düzeltme: [izinleri doğrulamak Için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum: ortak

### <a name="subscriptions-properties-blade"></a>Abonelikler Özellikler dikey penceresi
- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yönetici portalında, abonelikler için **Özellikler** dikey penceresi doğru yüklenmedi
- Düzeltme: bu abonelikler özelliklerini abonelikler genel bakış dikey penceresinin temel bileşenler bölmesinde görüntüleyebilirsiniz.
- Oluşum: ortak

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, depolama hesabı **yapılandırma** dikey penceresinde **güvenlik aktarım türünü**değiştirme seçeneği gösterilir. Özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltme: SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum: ortak

### <a name="update"></a>Güncelleştir

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: operatör portalında, düzeltmenin güncelleştirme durumu güncelleştirme için yanlış bir durum gösterir. İlk durum, güncelleştirme devam ediyor olsa bile güncelleştirmenin yüklenemediğini belirtir.
- Düzeltme: portalı yenileyin ve durum "sürüyor" olarak güncelleşecek.
- Oluşum: aralıklı

## <a name="networking"></a>Networking (Ağ İletişimi)

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="alerts"></a>Uyarılar

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ geçidi** dikey penceresinde **uyarıları**kullanma seçeneği gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="active-active"></a>Etkin-Etkin

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, oluştururken ve **sanal ağ geçidinin**kaynak menüsünde, **etkin-etkin** yapılandırmayı etkinleştirme seçeneği görüntülenir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **Bağlantılar** dikey penceresinde **VPN sorun giderici**adlı bir özellik gösterilir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

#### <a name="documentation"></a>Belgeler

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: sanal ağ geçidinin Genel Bakış sayfasında bulunan belge bağlantıları, Azure Stack yerine Azure 'a özgü belgelere bağlanır. Azure Stack belgeler için lütfen aşağıdaki bağlantıları kullanın:

  - [Ağ Geçidi SKU 'Ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack BGP 'yi yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/ıKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç Havuzu Ekle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında bir **Load Balancer** **arka uç havuzu** eklemeye çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltme: arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum: ortak

#### <a name="create-inbound-nat"></a>Gelen NAT oluşturma

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir **Load Balancer**Için BIR **gelen NAT kuralı** oluşturmaya çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltme: arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum: ortak

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yeni bir Windows sanal makinesi (VM) oluştururken şu hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı ' için seri çıkış ayarları güncellenemedi**. Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltme: daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum: ortak

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi


#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve bir sanal makine ölçek kümesi örneği oluşturma işlemi, 4 düğümlü Azure Stack ortamında güncelleştirme işlemi sırasında **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltme: bir kullanılabilirlik kümesinde 2 hata etki alanı başarıyla bulunan tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltme: sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum: ortak

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi sıfırlama parolası çalışmıyor

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: ölçek kümesi Kullanıcı arabiriminde yeni bir sıfırlama parolası dikey penceresi görünür ancak Azure Stack, henüz bir ölçek kümesindeki parolanın sıfırlanmasını desteklemez.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Ölçek kümesi tanılamasında Rainy bulutu

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: sanal makine ölçek kümesine genel bakış sayfası boş bir grafik gösterir. Boş grafiğe tıkladığınızda bir "Rainy Cloud" dikey penceresi açılır. Bu, CPU yüzdesi gibi ölçek kümesi tanılama bilgilerine yönelik bir grafiktir ve geçerli Azure Stack derlemesinde desteklenen bir özellik değildir.
- Düzeltme: yok.
- Oluşum: ortak

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine Tanılama ayarları dikey penceresi

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: sanal makine Tanılama ayarları dikey penceresinde, bir **Application Insights hesabı**ısteyen bir **Havuz** sekmesi vardır. Bu, yeni bir dikey pencerenin sonucudur ve Azure Stack henüz desteklenmemiştir.
- Düzeltme: yok.
- Oluşum: ortak

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

- Uygulanabilir: Bu sorun 1905 güncelleştirmesi için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında, **ana bilgisayar düğümü güncelleştirme önkoşulu**nedeniyle güncelleştirme durumu başarısız olabilir. Bu genellikle, yetersiz boş disk alanına sahip bir ana bilgisayar düğümü nedeniyle oluşur.
- Düzeltme: konak düğümündeki disk alanını temizleme hakkında yardım almak için Azure Stack desteğe başvurun.
- Oluşum: seyrek

### <a name="preparation-failed"></a>Hazırlama başarısız oldu

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 1905 güncelleştirme paketi, bu sorunun oluşma olasılığını daha büyük hale getirmek için önceki güncelleştirme paketlerinden daha büyüktür.
- Düzeltme: sürüm 1901 ' den (1.1901.0.95) başlayarak, **Şimdi Güncelleştir** ' i ( **sürdürülmez**) tıklayarak bu soruna geçici bir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
- Oluşum: ortak

## <a name="portal"></a>Portal

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı aboneliklerinin silinmesi yalnız bırakılmış kaynaklarda sonuçlanır.
- Düzeltme: önce kullanıcı kaynaklarını veya tüm kaynak grubunu silin ve ardından Kullanıcı aboneliklerini silin.
- Oluşum: ortak

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Azure Stack portallarını kullanarak aboneliğinize ilişkin izinleri görüntüleyemezsiniz.
- Düzeltme: [izinleri doğrulamak Için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment)kullanın.
- Oluşum: ortak

### <a name="marketplace-management"></a>Market yönetimi

- Uygulanabilir: Bu sorun 1904 ve 1905 için geçerlidir
- Neden: yönetici portalında oturum açtığınızda Market yönetim ekranı görünür değildir.
- Düzeltme: Tarayıcıyı yenileyin veya **Ayarlar** ' a gidin ve **varsayılan ayarlara Sıfırla**seçeneğini belirleyin.
- Oluşum: aralıklı

### <a name="docker-extension"></a>Docker uzantısı

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: her iki yönetici ve Kullanıcı portalında, **Docker**için arama yaparsanız öğe yanlış döndürülür. Azure Stack ' de kullanılamaz. Oluşturmayı denerseniz bir hata görüntülenir.
- Düzeltme: azaltma yok.
- Oluşum: ortak

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltme: SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum: ortak

### <a name="template"></a>Şablon

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, şablon dağıtımı Kullanıcı arabirimi, "_" (alt çizgi karakteri) ile başlayan şablon adları için parametreleri doldurmaz.
- Düzeltme: şablon adından "_" (alt çizgi karakteri) öğesini kaldırın.
- Oluşum: ortak

## <a name="networking"></a>Networking (Ağ İletişimi)

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç Havuzu Ekle

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında bir **Load Balancer** **arka uç havuzu** eklemeye çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltme: arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum: ortak

#### <a name="create-inbound-nat"></a>Gelen NAT oluşturma

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir **Load Balancer**Için BIR **gelen NAT kuralı** oluşturmaya çalışırsanız, işlem hata iletisiyle başarısız olur **Load Balancer güncelleştirilemedi...**
- Düzeltme: arka uç havuzunu bir yük dengeleyici kaynağıyla ilişkilendirmek için PowerShell, CLı veya Kaynak Yöneticisi şablonu kullanın.
- Oluşum: ortak

#### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **oluştur Load Balancer** penceresinde **Standart** yük dengeleyici SKU 'su oluşturma seçeneği gösterilir. Bu seçenek Azure Stack desteklenmez.
- Düzeltme: bunun yerine **temel** yük dengeleyici seçeneklerini kullanın.
- Oluşum: ortak

### <a name="public-ip-address"></a>Genel IP adresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında **genel IP adresi oluştur** penceresinde **Standart** SKU oluşturma seçeneği gösterilir. **Standart** SKU Azure Stack desteklenmez.
- Düzeltme: genel IP adresi için **temel** SKU 'yu kullanın.
- Oluşum: ortak

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yeni bir Windows sanal makinesi (VM) oluştururken şu hata görüntülenebilir: **' VM-adı ' sanal makinesi başlatılamadı. Hata: VM ' VM-adı ' için seri çıkış ayarları güncellenemedi**.
Bir VM 'de önyükleme tanılamayı etkinleştirip önyükleme tanılama depolama hesabınızı sildiğinizde hata oluşur.
- Düzeltme: daha önce kullandığınız adla depolama hesabını yeniden oluşturun.
- Oluşum: ortak

### <a name="vm-resize"></a>VM yeniden boyutlandırma

- Uygulanabilir: Bu sorun 1905 sürümü için geçerlidir.
- Neden: yönetilen bir disk VM 'si başarıyla yeniden boyutlandırılamıyor. Sanal makineyi yeniden boyutlandırmaya çalışmak "Code": "ınternaloperationerror", "Message": "işlemde bir iç hata oluştu." hatası oluşturur.
- Düzeltme: sonraki sürümde bunu düzeltmek için çalışıyoruz. Şu anda VM 'yi yeni VM boyutuyla yeniden oluşturmanız gerekir.
- Oluşum: ortak

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="centos"></a>CentOS

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: sanal makine ölçek kümesi oluşturma deneyimi, dağıtım için bir seçenek olarak CentOS tabanlı 7,2 sağlar. CentOS 7,2, görüntünün bulunamadığını arayan dağıtım hatalarının oluşmasına neden olacak Azure Stack marketi 'nde kullanılamaz.
- Düzeltme: dağıtımınız için başka bir işletim sistemi seçin ya da Market 'ten bir operatör aracılığıyla dağıtımdan önce indirilen başka bir CentOS görüntüsünü belirten Azure Resource Manager şablonu kullanın.
- Oluşum: ortak

#### <a name="remove-scale-set"></a>Ölçek kümesini kaldır

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **Sanal Makine Ölçek Kümeleri** dikey penceresinden ölçek kümesini kaldıramazsınız.
- Düzeltme: kaldırmak istediğiniz ölçek kümesini seçin, sonra **genel bakış** bölmesinden **Sil** düğmesine tıklayın.
- Oluşum: ortak

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve bir sanal makine ölçek kümesi örneği oluşturma işlemi, 4 düğümlü Azure Stack ortamında güncelleştirme işlemi sırasında **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltme: bir kullanılabilirlik kümesinde 2 hata etki alanı başarıyla bulunan tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

#### <a name="scale-set-instance-view-blade-doesnt-load"></a>Ölçek kümesi örneği görünümü dikey penceresi yüklenmedi

- Uygulanabilir: Bu sorun 1904 ve 1905 sürümü için geçerlidir.
- Neden: Azure Stack Portal-> panosunda bulunan bir sanal makine ölçek kümesinin örnek görünümü dikey penceresi-> Sanal Makine Ölçek Kümeleri-> AnyScaleSet-Instances-> AnyScaleSetInstance yüklenemez ve bir ağlayan bulut görüntüsü görüntüler.
- Düzeltme: Şu anda bir düzeltme yoktur ve bir düzeltme üzerinde çalışıyoruz. Bundan sonra, bir ölçek kümesinin örnek görünümünü almak için lütfen CLı komutunu `az vmss get-instance-view` kullanın.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH erişimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SSH yetkilendirmesi etkinken oluşturulan bir Ubuntu 18,04 VM, oturum açmak için SSH anahtarlarını kullanmanıza izin vermez.
- Düzeltme: sağlama sonrasında SSH anahtarları uygulamak veya parola tabanlı kimlik doğrulaması kullanmak için Linux uzantısı için VM erişimi kullanın.
- Oluşum: ortak

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
