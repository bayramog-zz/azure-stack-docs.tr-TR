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
ms.date: 05/10/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 05/10/2019
ms.openlocfilehash: f2e20377a976c5dba7a63d9f8cf8b3e2d100e060
ms.sourcegitcommit: 426380a3a27954cd609ba52d1066d9d69f5267fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65532256"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 bilinen sorunlar

Bu makalede, Azure Stack 1904 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlanan liste güncelleştirilir.

> [!IMPORTANT]  
> Bu bölümde, güncelleştirmeyi uygulamadan önce gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Azure Stack güncelleştirmesi yüklenmeye çalışılırken olmadığında güncelleştirme durumu ve durumuna değiştirin **PreparationFailed**. Bu, düzgün bir şekilde işlemek için bir iç altyapı paylaşımına depolama kapsayıcısından dosya aktarımı erişememe güncelleştirme kaynağı sağlayıcısı tarafından (URP) kaynaklanır.
- Düzeltme: 1901 (1.1901.0.95) sürümünden itibaren kullanarak bu sorunu geçici olarak tıklayarak çalışabilirsiniz **Şimdi Güncelleştir** yeniden (değil **sürdürme**). URP ardından önceki girişim dosyalarından temizler ve indirmeyi yeniden başlatır.
- Örneği: Ortak

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

### <a name="compute-host-agent-alert"></a>Konak Aracısı uyarı işlem

- Uygulanabilir: Bu sürümde 1904 yeni bir sorundur.
- Neden: "İşlem konak Aracısı uyarı görünür bir düğümü yeniden başlatıldıktan sonra ölçek birimi". Yeniden başlatma işlem konak Aracısı hizmeti için varsayılan başlangıç ayarını değiştirir.
- Düzeltme:
  - Bu uyarı yoksayılabilir. Aracı yanıt vermiyor, işleci ve kullanıcı işlemleri veya kullanıcı uygulamaları üzerinde hiçbir etkisi yok. El ile kapalıysa uyarı 24 saat sonra yeniden görünür.
  - Microsoft destek hizmeti için başlatma ayarı değiştirerek bu sorunu düzeltebilir. Bu, bir destek çağrısının açılmasını gerektirir. Düğümü yeniden başlatıldıysa, yeni bir uyarı görüntülenir.
- Örneği: Ortak

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme güncelleştirme etkinlik denetim listesi](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1904.md)
