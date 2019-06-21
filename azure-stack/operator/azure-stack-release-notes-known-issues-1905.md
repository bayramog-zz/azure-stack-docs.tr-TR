---
title: Azure Stack 1905 bilinen sorunlar | Microsoft Docs
description: Azure Stack 1905'de bilinen sorunlar hakkında bilgi edinin.
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
ms.date: 06/05/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/05/2019
ms.openlocfilehash: 48a97beac6b43e95b52d8b23f20369318e53ebee
ms.sourcegitcommit: 759a01b566597a71b18fca25902cacc983a5a63b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297984"
---
# <a name="azure-stack-1905-known-issues"></a>Azure Stack 1905 bilinen sorunlar

Bu makalede, Azure Stack 1905 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlanan liste güncelleştirilir.

> [!IMPORTANT]  
> Bu bölümde, güncelleştirmeyi uygulamadan önce gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

### <a name="host-node-update-prerequisite-failure"></a>Ana bilgisayar düğümünü güncelleştirmeyi önkoşul hatası

- Uygulanabilir: Bu sorun 1905 güncelleştirmesi için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirmeyi yüklemeye çalışırken, güncelleştirme durumu nedeniyle başarısız olabilir **ana düğüm güncelleştirme önkoşul**. Bunun nedeni genellikle yeterli boş disk alanı olan bir konak düğümü tarafından.
- Düzeltme: Ana bilgisayar düğümü disk alanı temizleme Yardım almak için Azure Stack desteği'ne başvurun.
- Örneği: Seyrek

### <a name="preparation-failed"></a>Hazırlama başarısız

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 1905 Azure yığını güncelleştirmesi yüklenmeye çalışılırken olmadığında güncelleştirme durumu ve durumuna değiştirin **PreparationFailed**. Bu, düzgün bir şekilde işlemek için bir iç altyapı paylaşımına depolama kapsayıcısından dosya aktarımı erişememe güncelleştirme kaynağı sağlayıcısı tarafından (URP) kaynaklanır. 1905 güncelleştirme paketi, bu sorun büyük olasılıkla yapabilir, önceki güncelleştirme paketlerini büyüktür.
- Düzeltme: 1901 (1.1901.0.95) sürümünden itibaren kullanarak bu sorunu geçici olarak tıklayarak çalışabilirsiniz **Şimdi Güncelleştir** yeniden (değil **sürdürme**). URP ardından önceki girişim dosyalarından temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, aşağıdaki güncelleştirme paketini el ile karşıya öneririz [alma ve yükleme güncelleştirmelerini bölümüne](azure-stack-apply-updates.md#import-and-install-updates).
- Örneği: Common

## <a name="portal"></a>Portal

### <a name="administrative-subscriptions"></a>Yönetim abonelikler

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 1804 sürümü ile sunulan iki yönetici aboneliğin kullanılmamalıdır. Abonelik türleridir **ölçüm** aboneliği ve **tüketim** abonelik.
- Düzeltme: Bu abonelikler 1906 ile başlayan ve sonunda silinen askıya alınır. Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, kullanıcı Aboneliklerdeki 1906 önce yeniden oluşturun.
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

### <a name="marketplace-management"></a>Market Yönetimi

- Uygulanabilir: Bu sorun 1904 ve 1905 için geçerlidir
- Neden: Yönetici portalında oturum açtığınızda Market yönetim ekran görünür değil.
- Düzeltme: Tarayıcıyı yenileyin veya Git **ayarları** ve seçeneğini **varsayılan ayarlarına geri döndürmeyi**.
- Örneği: Aralıklı

### <a name="docker-extension"></a>Docker uzantısı

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yönetici ve kullanıcı portalı için arama yaparsanız, **Docker**, öğenin yanlış döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata görüntülenir.
- Düzeltme: Risk azaltma.
- Örneği: Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı portalında bir blobu kullanarak yüklemeye çalıştığınızda **OAuth(preview)** seçeneği, görev bir hata iletisiyle başarısız olur.
- Düzeltme: Blob SAS seçeneğini kullanarak yükleyin.
- Örneği: Common

## <a name="networking"></a>Ağ

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

#### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **Load Balancer oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** yük dengeleyici SKU. Bu seçenek, Azure Stack'te desteklenmiyor.
- Düzeltme: Kullanım **temel** bunun yerine yük dengeleyici seçenekleri.
- Örneği: Common

### <a name="public-ip-address"></a>Genel IP adresi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **genel IP adresi oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** SKU. **Standart** SKU Azure Stack'te desteklenmez.
- Düzeltme: Kullanım **temel** SKU genel IP adresi için.
- Örneği: Common

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir: **Sanal makine 'vm-adı' başlatılamadı. Hata: VM 'vm-adı' için seri çıkış ayarları güncelleştirilemedi**.
Bir VM'de önyükleme tanılamalarını etkinleştirme, ancak önyükleme tanılama depolama hesabınızı silerseniz, bir hata meydana gelir.
- Düzeltme: Daha önce kullandığınız aynı ada sahip bir depolama hesabını yeniden oluşturun.
- Örneği: Common

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi

#### <a name="centos"></a>CentOS

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi oluşturma deneyimi sağlar. 7\.2 centOS kullanılabilir değil Azure Stack Marketini üzerinde dağıtım hatalarını dışarı görüntü bulunamadı aramalar neden olur.
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

#### <a name="vmss-instance-view-blade-doesnt-load"></a>VMSS örnek görünümü dikey yüklenmiyor

- Uygulanabilir: Bu sorun, yayın 1904 ve 1905 için geçerlidir.
- Neden: Azure Stack portalında yer alan bir ölçek kümesi örnek görünümü dikey Pano -> sanal makine Ölçek -> Ayarlar -> AnyScaleSet - örnekleri ->'nı yüklemek için AnyScaleSetInstance başarısız ve crying bir bulut görüntüsünü görüntüler.
- Düzeltme: Şu anda düzeltme yoktur ve bir düzeltme üzerinde çalışıyoruz. O zamana kadar lütfen CLI komutunu `az vmss get-instance-view` bir ölçek kümesi örnek görünümünü elde etmek için.

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH access

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Etkinleştirilmiş SSH yetkilendirme ile oluşturulan bir Ubuntu 18.04 VM oturum açmak için SSH anahtarları kullanmak izin vermez.
- Düzeltme: VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için veya parola tabanlı kimlik doğrulaması kullanın.
- Örneği: Common

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme güncelleştirme etkinlik denetim listesi](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1905.md)
