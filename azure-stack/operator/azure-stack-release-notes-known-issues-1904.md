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
ms.date: 05/31/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 68a0d80f02e84affa551b296548a09768eba7e1a
ms.sourcegitcommit: 6876ccb85c20794969264a1b27e479f4e938f990
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406793"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 bilinen sorunlar

Bu makalede, Azure Stack 1904 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlanan liste güncelleştirilir.

> [!IMPORTANT]  
> Bu bölümde, güncelleştirmeyi uygulamadan önce gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Azure Stack güncelleştirmesi yüklenmeye çalışılırken olmadığında güncelleştirme durumu ve durumuna değiştirin **PreparationFailed**. Bu, düzgün bir şekilde işlemek için bir iç altyapı paylaşımına depolama kapsayıcısından dosya aktarımı erişememe güncelleştirme kaynağı sağlayıcısı tarafından (URP) kaynaklanır.
- Düzeltme: 1901 (1.1901.0.95) sürümünden itibaren kullanarak bu sorunu geçici olarak tıklayarak çalışabilirsiniz **Şimdi Güncelleştir** yeniden (değil **sürdürme**). URP ardından önceki girişim dosyalarından temizler ve indirmeyi yeniden başlatır.
- Örneği: Common

## <a name="portal"></a>Portal

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

### <a name="docker-extension"></a>Docker uzantısı

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yönetici ve kullanıcı portalı için arama yaparsanız, **Docker**, öğenin yanlış döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata görüntülenir.
- Düzeltme: Risk azaltma.
- Örneği: Common

### <a name="marketplace-management"></a>Market Yönetimi

- Uygulanabilir: Bu sürümde 1904 yeni bir sorundur.
- Neden: Yönetici portalında oturum açtığınızda Market yönetim ekran görünür değil.
- Düzeltme: Tarayıcıyı yenileyin.
- Örneği: Aralıklı

### <a name="marketplace-management"></a>Market Yönetimi

- Uygulanabilir: Bu sorun 1904 için geçerlidir.
- Neden: Sonuçlarda filtre uyguladığınızda **Ekle azure'dan** dikey penceresinde Yönetici portalı'nda Marketi yönetim sekmesinden, hatalı filtrelenmiş sonuçlar görebilirsiniz.
- Düzeltme: Adı sütuna göre sıralama sonuçları ve sonuçları düzeltilecektir.
- Örneği: Aralıklı

### <a name="marketplace-management"></a>Market Yönetimi

- Uygulanabilir: Bu sorun 1904 için geçerlidir.
- Neden: Market Yönetimi Yönetici portalı'nda sonuçlara filtre açılan yayımcı altında yinelenen yayımcı adlarını görürsünüz. 
- Düzeltme: Bu yayımcı altında kullanılabilir olan tüm Market ürünleri doğru listesine sahip tüm çoğaltmaları seçin.
- Örneği: Aralıklı

### <a name="docker-extension"></a>Docker uzantısı

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Yönetici ve kullanıcı portalı için arama yaparsanız, **Docker**, öğenin yanlış döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata görüntülenir.
- Düzeltme: Risk azaltma.
- Örneği: Common

### <a name="upload-blob"></a>Blobu karşıya yükle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda görev OAuth(preview) seçeneğini kullanarak bir blob karşıya yüklemek çalıştığınızda bir hata iletisiyle başarısız olur.
- Düzeltme: Blob SAS seçeneğini kullanarak yükleyin.
- Örneği: Common

## <a name="networking"></a>Ağ

### <a name="load-balancer"></a>Yük dengeleyici

#### <a name="add-backend-pool"></a>Arka uç havuzu Ekle

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı portalında eklemeye çalışırsa bir **arka uç havuzu** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: Arka uç havuzuna bir yük dengeleyici kaynağı ile ilişkilendirmek için PowerShell, CLI veya Azure Resource Manager şablonu kullanın.
- Örneği: Common

#### <a name="create-inbound-nat"></a>Gelen NAT'ı oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Oluşturmayı denerseniz, Kullanıcı Portalı'nda bir **gelen NAT kuralı** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...** .
- Düzeltme: Arka uç havuzuna bir yük dengeleyici kaynağı ile ilişkilendirmek için PowerShell, CLI veya Azure Resource Manager şablonu kullanın.
- Örneği: Common

#### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **Load Balancer oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** yük dengeleyici SKU. Bu seçenek, Azure Stack'te desteklenmiyor.
- Düzeltme: Temel yük dengeleyici seçeneklerini kullanın.
- Örneği: Common

#### <a name="public-ip-address"></a>Genel IP adresi

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Kullanıcı Portalı'nda **genel IP adresi oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** SKU. **Standart** SKU Azure Stack'te desteklenmez.
- Düzeltme: Kullanım **temel** SKU, bunun yerine için genel IP adresleri.
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

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH access

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: Etkinleştirilmiş SSH yetkilendirme ile oluşturulan bir Ubuntu 18.04 VM oturum açmak için SSH anahtarları kullanmak izin vermez.
- Düzeltme: VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için veya parola tabanlı kimlik doğrulaması kullanın.
- Örneği: Common

### <a name="compute-host-agent-alert"></a>Konak Aracısı uyarı işlem

- Uygulanabilir: Bu sürümde 1904 yeni bir sorundur.
- Neden: A **işlem konak Aracısı** uyarı ölçek birimi bir düğümü yeniden başlatıldıktan sonra görünür. Yeniden başlatma işlem konak Aracısı hizmeti için varsayılan başlangıç ayarını değiştirir. Bu uyarı, aşağıdaki örneğe benzer:

   ```shell
   NAME  
   Compute Host Agent is not responding to calls.
   SEVERITY  
   Warning
   STATE  
   Active
   CREATED TIME  
   5/16/2019, 10:08:23 AM
   UPDATED TIME  
   5/22/2019, 12:27:27 PM
   COMPONENT  
   M#####-NODE02
   DESCRIPTION  
   Could not communicate with the Compute Host Agent running on node: M#####-NODE02
   REMEDIATION  
   Please disable Compute Host Agent feature flag and collect logs for further diagnosis.
   ```

- Düzeltme:
  - Bu uyarı yoksayılabilir. Aracı yanıt vermiyor, işleci ve kullanıcı işlemleri veya kullanıcı uygulamaları üzerinde hiçbir etkisi yok. El ile kapalıysa uyarı 24 saat sonra yeniden görünür.
  - En son sorun çözüldüğünde [1904 için Azure Stack düzeltme](https://support.microsoft.com/help/4505688).
- Örneği: Common

### <a name="virtual-machine-scale-set-instance-view"></a>Sanal makine ölçek kümesi örnek görünümü

- Uygulanabilir: Bu sorun 1904 ve 1905 sürümleri için geçerlidir.
- Neden: Bir ölçek kümesi örnek görünümü dikey penceresinde bulunan Azure Stack portalında **Pano** > **sanal makine ölçek kümeleri**  >   **AnyScaleSet - örnekler** > **AnyScaleSetInstance** yüklenemiyor.
- Düzeltme: Şu anda düzeltme yoktur ve bir düzeltme üzerinde çalışıyoruz. O zamana kadar lütfen CLI cmdlet'ini kullanın `az vmss get-instance-view` bir sanal makine ölçek kümesi örnek görünümünü elde etmek için.

## <a name="storage"></a>Depolama

- Uygulanabilir: Bu sorun, tüm desteklenen sürümleri için geçerlidir.
- Neden: [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) Azure Stack'te desteklenmiyor ve sonuçları bir disk oluştururken **$null** kimliği Bu sanal makinede, başlatma ve durdurma gibi işlemleri gerçekleştirmesini engeller. Disk kullanıcı Arabiriminde görünmez ya da API aracılığıyla görünmüyor. Bu noktada VM onarılamıyor ve silinmesi gerekir.
- Düzeltme: Disklerinizi doğru şekilde dönüştürmek için izleyin [dönüştürmek için yönetilen diskler Kılavuzu](../user/azure-stack-managed-disk-considerations.md#convert-to-managed-disks).

## <a name="app-service"></a>App Service

- Kiracılar, abonelikte, ilk Azure işlevinizi oluşturmadan önce depolama kaynak sağlayıcısını kaydetmeniz gerekir.
- Bazı Kiracı portalı kullanıcı deneyimleri 1903 portal framework ile uyumsuzluk nedeniyle bozuk; Test üretim ve site uzantıları temelde, kullanıcı Deneyimini dağıtım yuvaları için. Bu sorunu geçici olarak çözmek için kullanın [Azure App Service PowerShell modülünü](/azure/app-service/deploy-staging-slots#automate-with-powershell) veya [Azure CLI](/cli/azure/webapp/deployment/slot?view=azure-cli-latest). Portal deneyimi dağıtımınızın yükselterek geri yüklenecek [1.6 (güncelleştirme 6) için Azure Stack'te Azure App Service](azure-stack-app-service-release-notes-update-six.md).

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme güncelleştirme etkinlik denetim listesi](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmeleri listesini gözden geçirin](azure-stack-release-notes-security-updates-1904.md)
