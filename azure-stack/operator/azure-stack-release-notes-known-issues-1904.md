---
title: Azure Stack 1904 bilinen sorunlar | Microsoft Docs
description: Azure Stack 1904'de bilinen sorunlar hakkında bilgi edinin.
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
ms.date: 05/07/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 4438fdb5b97a4e08632b9c4f55fc03c91ef3aa97
ms.sourcegitcommit: 405075e7826ba2fa545dbf5498160219ef8d2db5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65453710"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 bilinen sorunlar

Bu makalede, Azure Stack 1904 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlanan liste güncelleştirilir.

> [!IMPORTANT]  
> Bu bölümde, güncelleştirmeyi uygulamadan önce gözden geçirin.

## <a name="portal"></a>Portal

### <a name="add-on-plans"></a>Eklenti planları

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikler de silinir kadar plan kalır.
- Düzeltme: Risk azaltma.
- Örneği: Ortak

### <a name="administrative-subscriptions"></a>Yönetim abonelikler

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: 1804 sürümü ile sunulan iki yönetici aboneliğin kullanılmamalıdır. Abonelik türleridir **ölçüm** aboneliği ve **tüketim** abonelik.
- Düzeltme: Bu abonelikler 1905 ile başlayan ve sonunda silinen askıya alınır. Bu iki abonelik üzerinde çalışan kaynaklarınız varsa, kullanıcı Aboneliklerdeki 1905 önce yeniden oluşturun.
- Örneği: Ortak

### <a name="subscription-resources"></a>Abonelik kaynakları

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor.
- Düzeltme: Kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.
- Örneği: Ortak

### <a name="subscription-permissions"></a>Abonelik izinleri

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz.
- Düzeltme: Kullanım [izinleri doğrulamak için PowerShell](/powershell/module/azurerm.resources/get-azurermroleassignment).
- Örneği: Ortak

### <a name="marketplace-management"></a>Market Yönetimi

- Uygulanabilir: Bu sorun 1904 için geçerlidir.
- Neden: Yönetici portalında oturum açtığınızda Market yönetim ekran görünür değil.
- Düzeltme: Tarayıcıyı yenileyin.
- Örneği: Aralıklı

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda görev OAuth(preview) seçeneğini kullanarak bir blob karşıya yüklemek çalıştığınızda bir hata iletisiyle başarısız olur.
- Düzeltme: Blob SAS seçeneğini kullanarak yükleyin.
- Örneği: Ortak

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Yük Dengeleyici

#### <a name="add-backend-pool"></a>Arka uç havuzu Ekle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı portalında eklemeye çalışırsa bir **arka uç havuzu** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: PowerShell, CLI veya Resource Manager şablonu ile bir yük dengeleyici kaynağını arka uç havuzunu ilişkilendirmek için kullanın.
- Örneği: Ortak

#### <a name="create-inbound-nat"></a>Gelen NAT'ı oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Oluşturmayı denerseniz, Kullanıcı Portalı'nda bir **gelen NAT kuralı** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: PowerShell, CLI veya Resource Manager şablonu ile bir yük dengeleyici kaynağını arka uç havuzunu ilişkilendirmek için kullanın.
- Örneği: Ortak

#### <a name="create-load-balancer"></a>Yük Dengeleyici oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **yük dengeleyici oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** yük dengeleyici SKU. Bu seçenek, Azure Stack'te desteklenmiyor.
- Düzeltme: Temel yük dengeleyici seçeneklerini kullanın.
- Örneği: Ortak

#### <a name="public-ip-address"></a>Genel IP Adresi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **genel IP adresi oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** SKU. **Standart** SKU Azure Stack'te desteklenmez.
- Düzeltme: Temel SKU genel IP adresini kullanın.
- Örneği: Ortak

## <a name="compute"></a>İşlem

### <a name="vm-boot-diagnostics"></a>VM önyükleme tanılaması

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir: **Sanal makine 'vm-adı' başlatılamadı. Hata: VM 'vm-adı' için seri çıkış ayarları güncelleştirilemedi**.
Bir VM'de önyükleme tanılamalarını etkinleştirme, ancak önyükleme tanılama depolama hesabınızı silerseniz, bir hata meydana gelir.
- Düzeltme: Daha önce kullandığınız aynı ada sahip bir depolama hesabını yeniden oluşturun.
- Örneği: Ortak

### <a name="virtual-machine-scale-set"></a>Sanal Makine Ölçek Kümesi

#### <a name="centos"></a>CentOS

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi (VMSS) oluşturma deneyimi sağlar. 7.2 centOS Azure Stack üzerinde kullanılabilir değil.
- Düzeltme: Dağıtımınız için başka bir işletim sistemi veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanın.
- Örneği: Ortak

#### <a name="remove-scale-set"></a>Ölçek kümesi Kaldır

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Bir ölçek kümesi kaldırılamıyor **sanal makine ölçek kümeleri** dikey penceresi.
- Düzeltme: Ölçek kümesini kaldırmak isteyip istemediğiniz seçin ardından **Sil** düğmesini **genel bakış** bölmesi.
- Örneği: Ortak

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH access

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Etkinleştirilmiş SSH yetkilendirme ile oluşturulan bir Ubuntu 18.04 VM oturum açmak için SSH anahtarları kullanmak izin vermez.
- Düzeltme: VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için veya parola tabanlı kimlik doğrulaması kullanın.
- Örneği: Ortak

## <a name="infrastructure-backup"></a>Altyapı yedekleme

<!--Bug 3615401 - scheduler config lost; new issue in YYMM;  hectorl-->
Otomatik yedeklemeler etkinleştirdikten sonra Zamanlayıcı hizmeti beklenmedik bir şekilde devre dışı durumuna geçtiğinde. Yedekleme denetleyici hizmeti, otomatik yedeklemeler devre dışı bırakılır ve bir Yönetici portalını uyarısı algılar. Bu uyarı, otomatik yedeklemeler devre dışı bırakıldığında beklenmektedir.

- Uygulanabilir: Bu sürümde 1904 yeni bir sorundur.
- Neden: Zamanlayıcı yapılandırmasının kaybı ile sonuçlanır hizmetinde bir hata nedeniyle bu sorunudur. Bu hata, depolama konumu, kullanıcı adı, parola ve şifreleme anahtarı değiştirmez.
- Düzeltme: Bu sorunu gidermek için altyapı yedekleme kaynak sağlayıcısındaki yedekleme denetleyicisi ayarlar dikey penceresi açın ve seçin **etkinleştirmek otomatik yedeklemeler**. İstenen sıklığı ve Bekletme dönemi ayarladığınızdan emin olun.
- Örneği: Düşük

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme güncelleştirme etkinlik denetim listesi](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1904.md)
