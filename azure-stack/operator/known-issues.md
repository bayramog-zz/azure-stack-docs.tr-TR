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
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: f2426699db940622f952809faa9de58b49065b56
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298879"
---
# <a name="azure-stack-known-issues"></a>Bilinen sorunları Azure Stack

Bu makalede Azure Stack sürümlerindeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlandıkları için liste güncellenir.

Farklı bir sürüme yönelik bilinen sorunlara erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın.

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> Güncelleştirmeyi uygulamadan önce bu bölümü gözden geçirin.
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> Azure Stack örneğiniz ikiden fazla güncelleştirme tarafından arkasındaysa uyumsuz olarak kabul edilir. [Destek almak için en az desteklenen sürüme güncelleştirmeniz](azure-stack-servicing-policy.md#keep-your-system-under-support)gerekir. 
::: moniker-end

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->

::: moniker range="azs-1910"
## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1804 sürümü ile tanıtılan iki yönetim aboneliği kullanılmamalıdır. Abonelik türleri **ölçüm** aboneliklerdir ve **Tüketim** aboneliğiydi.
- Düzeltme: Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları Kullanıcı aboneliklerinde yeniden oluşturun.
- Oluşum: ortak

### <a name="subscriptions-lock-blade"></a>Abonelikler kilit dikey penceresi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: yönetici portalında, Kullanıcı aboneliklerinin **kilit** dikey penceresinde **abonelik**belirten iki düğme vardır.
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

### <a name="upload-blob-with-oauth-error"></a>Blob 'u OAuth hatası ile karşıya yükleme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **OAuth (Önizleme)** seçeneğini kullanarak bir blobu karşıya yüklemeye çalıştığınızda, görev bir hata iletisiyle başarısız olur.
- Düzeltme: SAS seçeneğini kullanarak blobu karşıya yükleyin.
- Oluşum: ortak

### <a name="upload-blob-option-unsupported"></a>Karşıya yükleme blobu seçeneği desteklenmiyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, karşıya yükleme dikey penceresinde bir blob yüklemeye çalıştığınızda **AAD** veya **anahtar kimlik doğrulaması**seçme seçeneği vardır ancak **AAD** Azure Stack desteklenmez.
- Oluşum: ortak

### <a name="load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzu

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında **yük dengeleyici** arka uç havuzu eklenirken işlem, **yük dengeleyici arka uç havuzu kaydedilemedi**hata iletisiyle sonuçlanır; Ancak, işlem gerçekten başarılı oldu.
- Oluşum: ortak

### <a name="incorrect-tooltip-when-creating-vm"></a>VM oluşturulurken yanlış araç ipucu

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, disk türü Premium SSD ile yönetilen bir disk seçtiğinizde, açılan listede **Işletim sistemi diski**gösterilir. Bu seçeneğin yanındaki araç ipucu, **belirli işletim sistemi disk boyutlarının Azure Ücretsiz hesabı ile ücretsiz kullanılabilir olabileceğini**belirtir; Ancak, bu Azure Stack için geçerli değildir. Ayrıca, liste, Azure Stack için geçerli olmayan **ücretsiz hesap** içerir.
- Oluşum: ortak

### <a name="vpn-troubleshoot-and-metrics"></a>VPN sorun giderme ve ölçümler

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: bir VPN ağ geçidi kaynağında **VPN sorun giderme** özelliği ve **ölçümleri** , Kullanıcı portalında görüntülenir, ancak bu Azure Stack desteklenmez.
- Oluşum: ortak

### <a name="adding-extension-to-vm-scale-set"></a>Uzantı VM Ölçek kümesine ekleniyor

- Uygulanabilir: Bu sorun 1907 ve üzeri sürümler için geçerlidir.
- Neden: Kullanıcı portalında, bir sanal makine ölçek kümesi oluşturulduktan sonra, kullanıcı ARABIRIMI kullanıcının uzantı eklemesine izin vermez.
- Oluşum: ortak

### <a name="delete-a-storage-container"></a>Depolama kapsayıcısını silme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir Kullanıcı bir depolama kapsayıcısını silmeye çalıştığında, Kullanıcı **Azure ilkesi ve RBAC rol ayarlarını geçersiz kıl**seçeneğini değişmediğinde işlem başarısız olur.
- Düzeltme: **Azure ilkesi ve RBAC rol ayarlarını geçersiz kılmak**için kutunun Işaretli olduğundan emin olun.
- Oluşum: ortak

### <a name="refresh-button-on-virtual-machines-fails"></a>Sanal makinelerde yenileme düğmesi başarısız oluyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, sanal makinelere gidip en üstteki düğmeyi kullanarak yenilemeyi denediğinizde durumlar doğru şekilde güncelleştirilemez. 
- Düzeltme: yenileme düğmesinin tıklandığına bakılmaksızın durum her 5 dakikada bir otomatik olarak güncelleştirilir. 5 dakika bekleyin ve durumu denetleyin.
- Oluşum: ortak

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi 

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir yol tablosu oluşturduğunuzda, bir sonraki atlama türü seçeneğinden biri olarak **sanal ağ geçidi** görüntülenir; Ancak bu Azure Stack desteklenmez.
- Oluşum: ortak

### <a name="storage-account-options"></a>Depolama hesabı seçenekleri

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında depolama hesaplarının adı **depolama hesabı-blob, dosya, tablo, kuyruk**, ancak **Dosya** Azure Stack tarafından desteklenmez.
- Oluşum: ortak

### <a name="storage-account-configuration"></a>Depolama hesabı yapılandırması

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, bir depolama hesabı oluşturduğunuzda ve **yapılandırmasını**görüntülediğinizde, bir AJAX hatasına neden olduğu için yapılandırma değişikliklerini kaydedemezsiniz. 
- Oluşum: ortak

### <a name="capacity-mnitoring-in-sql-and-mysql-resource-provider-keeps-loading"></a>SQL ve MySQL kaynak sağlayıcısındaki kapasite mnitoring yükleme devam ediyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: SQL ve MySQL kaynak sağlayıcısının geçerli sürümleri, 1910 güncelleştirmesinde bulunan en son Portal değişiklikleriyle uyumlu değildir.
- Düzeltme: SQL ve MySQL kaynak sağlayıcısı düzeltmesini 1.1.47.0 uygulamak için kaynak sağlayıcısı güncelleştirme işlemini Izleyin. ([SQL RP sürüm 1.1.47.0](https://aka.ms/azurestacksqlrp11470) ve [MySQL RP sürüm 1.1.47.0](https://aka.ms/azurestackmysqlrp11470)).
- Oluşum: ortak

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Yük dengeleyici

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: bir yük dengeleyicinin arka uç havuzuna kullanılabilirlik kümesi VM 'Leri eklerken, portalda **yük dengeleyici arka uç havuzunu kaydedemediğini**belirten bir hata iletisi görüntülenir. Bu, portalda bir yüzeysel sorunudur; işlevsellik hala yerinde ve VM 'Ler dahili olarak arka uç havuzuna başarıyla eklendi.
- Oluşum: ortak

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: bir NSG 'de açık bir **Denyalloutbağlanacak** kural oluşturulamıyor çünkü bu, VM dağıtımının tamamlanabilmesi için gereken altyapıya yönelik tüm iç iletişimin tamamlanmasını engelleyecek.
- Oluşum: ortak

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

#### <a name="addingremoving-network-interface"></a>Ağ arabirimi ekleniyor/kaldırılıyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

#### <a name="primary-network-interface"></a>Birincil ağ arabirimi

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: bir VM 'nin birincil NIC 'si değiştirilemez. Birincil NIC 'nin silinmesi/ayrılması, VM 'yi başlatırken sorunlarla sonuçlanır.
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

### <a name="consumed-compute-quota"></a>Tüketilen işlem kotası

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Yeni bir sanal makine oluştururken bu **abonelik, bu konumdaki toplam bölgesel vCPU kapasitesinin kapasitesine sahip olduğu gibi bir hata alabilirsiniz. Bu abonelik tüm 50 toplam bölgesel sanal CPU 'ları kullanıyor.** Bu, kullanılabilir toplam çekirdek kotasına ulaşıldığını gösterir.
- Düzeltme: işleçinizden ek kota içeren bir eklenti planı eklemesini Isteyin. Geçerli planın kotasının düzenlenmesiyle çalışmaz veya artan kotayı yansıtacaksınız.
- Oluşum: nadir

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki sırasında oluşturma ve 4 düğümlü Azure Stack ortamlarında güncelleştirme

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 3 hata etki alanı kullanılabilirlik kümesinde VM oluşturma ve bir sanal makine ölçek kümesi örneği oluşturma işlemi, 4 düğümlü Azure Stack ortamında güncelleştirme işlemi sırasında **FabricVmPlacementErrorUnsupportedFaultDomainSize** hatasıyla başarısız oluyor.
- Düzeltme: bir kullanılabilirlik kümesinde 2 hata etki alanı başarıyla bulunan tek VM 'Ler oluşturabilirsiniz. Ancak, 4 düğümlü Azure Stack dağıtımında güncelleştirme işlemi sırasında ölçek kümesi örneği oluşturma yine de kullanılamaz.

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
- Neden: yönetici portalında, Kullanıcı aboneliklerinde **kilit** dikey penceresinde **abonelik**etiketli iki düğme vardır.
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

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Load Balancer

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: Load Balancer için bir arka uç havuzuna Avaibilme kümesi sanal makineleri eklenirken, portalda **yük dengeleyici arka uç havuzu kaydedilemedi**hatası bildiren bir hata iletisi görüntüleniyor. Bu, portalda bir yüzeysel sorunudur; işlevsellik hala yerinde ve VM 'Ler otomatik olarak arka uç havuzuna başarıyla eklendi. 
- Oluşum: ortak

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: bir NSG 'de açık bir **Denyalloutbağlanacak** kural oluşturulamıyor çünkü bu, VM dağıtımının tamamlanabilmesi için gereken altyapıya yönelik tüm iç iletişimin tamamlanmasını engelleyecek.
- Oluşum: ortak

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

#### <a name="addingremoving-network-interface"></a>Ağ arabirimi ekleniyor/kaldırılıyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

#### <a name="primary-network-interface"></a>Birincil ağ arabirimi

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

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Load Balancer

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: Load Balancer için bir arka uç havuzuna Avaibilme kümesi sanal makineleri eklenirken, portalda **yük dengeleyici arka uç havuzu kaydedilemedi**hatası bildiren bir hata iletisi görüntüleniyor. Bu, portalda bir yüzeysel sorunudur; işlevsellik hala yerinde ve VM 'Ler otomatik olarak arka uç havuzuna başarıyla eklendi. 
- Oluşum: ortak

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir. 
- Neden: bir NSG 'de açık bir **Denyalloutbağlanacak** kural oluşturulamıyor çünkü bu, VM dağıtımının tamamlanabilmesi için gereken altyapıya yönelik tüm iç iletişimin tamamlanmasını engelleyecek.
- Oluşum: ortak

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: Kullanıcı portalında, **sanal ağ** dikey penceresinde **hizmet uç noktalarını**kullanma seçeneği gösterilmektedir. Bu özellik şu anda Azure Stack desteklenmiyor.
- Oluşum: ortak

### <a name="network-interface"></a>Ağ arabirimi

#### <a name="addingremoving-network-interface"></a>Ağ arabirimi ekleniyor/kaldırılıyor

- Uygulanabilir: Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: **çalışır** DURUMDAKI bir sanal makineye yeni bir ağ arabirimi eklenemez.
- Düzeltme: ağ arabirimini eklemeden/kaldırmadan önce sanal makineyi durdurun.
- Oluşum: ortak

#### <a name="primary-network-interface"></a>Birincil ağ arabirimi

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

### <a name="update"></a>Güncelleştirme

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: operatör portalında, düzeltmenin güncelleştirme durumu güncelleştirme için yanlış bir durum gösterir. İlk durum, güncelleştirme devam ediyor olsa bile güncelleştirmenin yüklenemediğini belirtir.
- Düzeltme: portalı yenileyin ve durum "sürüyor" olarak güncelleşecek.
- Oluşum: aralıklı

## <a name="networking"></a>Ağ

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

::: moniker range=">=azs-1906"
## <a name="archive"></a>Arşiv

Daha eski bir sürüm için arşivlenmiş bilinen sorunlara erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın ve görmek istediğiniz sürümü seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme etkinliği denetim listesini gözden geçir](release-notes-checklist.md)
- [Güvenlik güncelleştirmelerinin listesini gözden geçirin](release-notes-security-updates.md)
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-known-issues"></a>1905 arşivlenmiş bilinen sorunlar
::: moniker-end
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

::: moniker range="<azs-1906"
[Azure Stack bilinen sorunların eski sürümlerine TechNet Gallery ' de](https://aka.ms/azsarchivedrelnotes)erişebilirsiniz. Bu arşivlenmiş belgeler yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.
::: moniker-end
