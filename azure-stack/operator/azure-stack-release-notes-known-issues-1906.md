---
title: Azure Stack 1906 bilinen sorunlar | Microsoft Docs
description: Azure Stack 1906'de bilinen sorunlar hakkında bilgi edinin.
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
ms.date: 06/28/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/28/2019
ms.openlocfilehash: d1d3b3c33652811964b16a35686df3e3a24eb59a
ms.sourcegitcommit: 7348876a97e8bed504b5f5d90690ec8d1d9472b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557875"
---
# <a name="azure-stack-1906-known-issues"></a>Azure Stack 1906 bilinen sorunlar

Bu makalede, Azure Stack 1906 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlanan liste güncelleştirilir.

> [!IMPORTANT]  
> Bu bölümde, güncelleştirmeyi uygulamadan önce gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 1906 Azure yığını güncelleştirmesi yüklenmeye çalışılırken olmadığında güncelleştirme durumu ve durumuna değiştirin **PreparationFailed**. Bu, düzgün bir şekilde işlemek için bir iç altyapı paylaşımına depolama kapsayıcısından dosya aktarımı erişememe güncelleştirme kaynağı sağlayıcısı tarafından (URP) kaynaklanır. 
- Düzeltme: 1901 (1.1901.0.95) sürümünden itibaren kullanarak bu sorunu geçici olarak tıklayarak çalışabilirsiniz **Şimdi Güncelleştir** yeniden (değil **sürdürme**). URP ardından önceki girişim dosyalarından temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, aşağıdaki güncelleştirme paketini el ile karşıya öneririz [alma ve yükleme güncelleştirmelerini bölümüne](azure-stack-apply-updates.md#import-and-install-updates).
- Örneği: Common

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikler

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 1804 sürümü ile sunulan iki yönetici aboneliğin kullanılmamalıdır. Abonelik türleridir **ölçüm** aboneliği ve **tüketim** abonelik.
- Düzeltme: Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, bunları kullanıcı aboneliklerini yeniden oluşturun.
- Örneği: Common

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor.
- Düzeltme: Kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.
- Örneği: Common

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltme: Kullanım [izinleri doğrulamak için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment).
- Örneği: Common

### <a name="storage-account-settings"></a>Depolama hesabı ayarları

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Depolama hesabı kullanıcı portalında **yapılandırma** dikey pencerede gösterilir değiştirmek için bir seçenek **güvenlik aktarım türü**. Azure Stack'te özelliği şu anda desteklenmiyor.
- Örneği: Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı portalında bir blobu kullanarak yüklemeye çalıştığınızda **OAuth(preview)** seçeneği, görev bir hata iletisiyle başarısız olur.
- Düzeltme: Blob SAS seçeneğini kullanarak yükleyin.
- Örneği: Common

## <a name="networking"></a>Ağ

### <a name="service-endpoints"></a>Hizmet uç noktaları

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **sanal ağ** dikey pencerede gösterilir kullanmak için bir seçenek **hizmet uç noktaları**. Bu özellik Azure Stack'te şu anda desteklenmiyor.
- Örneği: Common

### <a name="network-interface"></a>Ağ arabirimi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yeni bir ağ arabirimi olan bir sanal makineye eklenemez bir **çalıştıran** durumu.
- Düzeltme: Ekleme/ağ arabirimi kaldırmadan önce sanal makineyi durdurun.
- Örneği: Common

### <a name="virtual-network-gateway"></a>Sanal Ağ Geçidi

#### <a name="alerts"></a>Uyarılar

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **sanal ağ geçidi** dikey pencerede gösterilir kullanmak için bir seçenek **uyarılar**. Bu özellik Azure Stack'te şu anda desteklenmiyor.
- Örneği: Common

#### <a name="active-active"></a>Etkin-Etkin

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Oluşturulurken, Kullanıcı Portalı'nda ve kaynak menüsünde **sanal ağ geçidi**, etkinleştirmek için bir seçenek görürsünüz **etkin-etkin** yapılandırma. Bu özellik Azure Stack'te şu anda desteklenmiyor.
- Örneği: Common

#### <a name="vpn-troubleshooter"></a>VPN sorun giderici

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **bağlantıları** dikey pencerede gösterilir denilen bir özelliği **VPN sorun giderici**. Bu özellik Azure Stack'te şu anda desteklenmiyor.
- Örneği: Common

#### <a name="documentation"></a>Belgeler

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Azure Stack yerine Azure'a özel belgelere genel bakış sayfasında sanal ağ geçidinin aşağıdaki belge bağlantılarına bağlayın. Lütfen Azure Stack belgeleri için aşağıdaki bağlantıları kullanın:

  - [Ağ geçidi SKU'ları](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [Yüksek oranda kullanılabilir bağlantılar](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack üzerinde BGP yapılandırma](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute devreleri](azure-stack-connect-expressroute.md)
  - [Özel IPSec/IKE ilkeleri belirtme](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç havuzu Ekle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı portalında eklemeye çalışırsa bir **arka uç havuzu** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: PowerShell, CLI veya Resource Manager şablonu ile bir yük dengeleyici kaynağını arka uç havuzunu ilişkilendirmek için kullanın.
- Örneği: Common

#### <a name="create-inbound-nat"></a>Gelen NAT'ı oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Oluşturmayı denerseniz, Kullanıcı Portalı'nda bir **gelen NAT kuralı** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: PowerShell, CLI veya Resource Manager şablonu ile bir yük dengeleyici kaynağını arka uç havuzunu ilişkilendirmek için kullanın.
- Örneği: Common

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir: **Sanal makine 'vm-adı' başlatılamadı. Hata: VM 'vm-adı' için seri çıkış ayarları güncelleştirilemedi**. Bir VM'de önyükleme tanılamalarını etkinleştirme, ancak önyükleme tanılama depolama hesabınızı silerseniz, bir hata meydana gelir.
- Düzeltme: Daha önce kullandığınız aynı ada sahip bir depolama hesabını yeniden oluşturun.
- Örneği: Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="centos"></a>CentOS

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi oluşturma deneyimi sağlar. 7\.2 centOS Azure Stack üzerinde kullanılabilir değil.
- Düzeltme: Dağıtımınız için başka bir işletim sistemi veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanın.
- Örneği: Common

#### <a name="remove-scale-set"></a>Ölçek kümesi Kaldır

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Bir ölçek kümesi kaldırılamıyor **sanal makine ölçek kümeleri** dikey penceresi.
- Düzeltme: Ölçek kümesini kaldırmak isteyip istemediğiniz seçin ardından **Sil** düğmesini **genel bakış** bölmesi.
- Örneği: Common

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>Düzeltme eki ve güncelleştirme 4 düğümlü Azure Stack ortamlarında sırasında hatalar oluşturun

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 3 hata etki alanı'bir kullanılabilirlik kümesindeki Vm'leri oluşturma ve oluşturma bir sanal makine ölçek kümesi örneği başarısız oluyor bir **FabricVmPlacementErrorUnsupportedFaultDomainSize** bir 4 düğümlü Azure Stack'te güncelleştirme işlemi sırasında hata oluştu ortam.
- Düzeltme: Bir kullanılabilirlik ile 2 hata etki alanı başarıyla ayarlandı tek VM'ler oluşturabilirsiniz. Bununla birlikte, Ölçek kümesi örnek oluşturma bir 4 düğümlü Azure Stack'te güncelleştirme işlemi sırasında hala kullanılamıyor.

#### <a name="scale-set-instance-view-blade-does-not-load"></a>Ölçek kümesi örnek görünümü dikey yüklenmiyor

- Uygulanabilir: Bu sorun 1904 1905 ve 1906 sürümleri için geçerlidir.
- Neden: Bir sanal makine ölçek kümesi örnek görünümü dikey penceresinde bulunan Azure Stack portalında **Pano > sanal makine ölçek kümeleri > AnyScaleSet > örnekler > AnyScaleSetInstance** yüklemek başarısız olur ve "rainy görüntülenir Bulut"görüntüsü.
- Düzeltme: Şu anda düzeltme yoktur ve bir düzeltme üzerinde çalışıyoruz. O zamana kadar lütfen CLI komutunu `az vmss get-instance-view` bir ölçek kümesi örnek görünümünü elde etmek için.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH access

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Etkinleştirilmiş SSH yetkilendirme ile oluşturulan bir Ubuntu 18.04 VM oturum açmak için SSH anahtarları kullanmak izin vermez.
- Düzeltme: VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için veya parola tabanlı kimlik doğrulaması kullanın.
- Örneği: Common

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>Sanal makine ölçek kümesi parolayı Sıfırla çalışmıyor

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: Yeni bir parola sıfırlama dikey ölçek kümesindeki UI görünür, ancak henüz bir ölçekte Azure Stack parola sıfırlanırken desteklemez.
- Düzeltme: Yok.
- Örneği: Common

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>Tanılama rainy bulut ölçeğinde ayarlayın

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: Sanal makine ölçek kümesine genel bakış sayfasında boş bir grafik gösterir. Boş grafiğini tıklattığınızda, "rainy bulut" dikey penceresi açılır. Bu grafik için ölçek kümesi gibi tanılama bilgilerini, CPU yüzdesi ve geçerli Azure Stack yapı desteklenen bir özellik değil.
- Düzeltme: Yok.
- Örneği: Common

### <a name="virtual-machine-diagnostic-settings-blade"></a>Sanal makine tanılama ayarları dikey penceresi

- Uygulanabilir: Bu sorun 1906 sürümü için geçerlidir.
- Neden: Sanal makine tanılama ayarları dikey olan bir **havuz** ister için sekmesinde, bir **Application Insight hesabı**. Bu yeni bir dikey pencere sonucunu ve Azure Stack'te henüz desteklenmiyor.
- Düzeltme: Yok.
- Örneği: Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme güncelleştirme etkinlik denetim listesi](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1906.md)
