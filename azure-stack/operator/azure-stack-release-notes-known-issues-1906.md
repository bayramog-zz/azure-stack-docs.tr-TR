---
title: Azure Stack 1906 bilinen sorunlar | Microsoft Docs
description: Azure Stack 1906 ' de bilinen sorunlar hakkında bilgi edinin.
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
ms.date: 07/12/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/28/2019
ms.openlocfilehash: 9eca3112085d68c6a35378f8309dfd8ac5a1bafc
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70008627"
---
# <a name="azure-stack-1906-known-issues"></a>Azure Stack 1906 bilinen sorunlar

Bu makalede Azure Stack 1906 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlandıkları için liste güncellenir.

> [!IMPORTANT]  
> Güncelleştirmeyi uygulamadan önce bu bölümü gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1906 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i (sürdürülmez) tıklayarak bu soruna geçicibir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
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

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme etkinliği denetim listesini gözden geçir](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmelerinin listesini gözden geçirin](azure-stack-release-notes-security-updates-1906.md)
