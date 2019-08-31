---
title: Azure Stack 1905 bilinen sorunlar | Microsoft Docs
description: Azure Stack 1905 ' de bilinen sorunlar hakkında bilgi edinin.
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
ms.date: 06/14/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/14/2019
monikerRange: azs-1905
ms.openlocfilehash: f5fdabf85e7fca25fbe341453c37c1072078b6f2
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188278"
---
# <a name="azure-stack-1905-known-issues"></a>Azure Stack 1905 bilinen sorunlar

Bu makalede Azure Stack 1905 sürümündeki bilinen sorunlar listelenmektedir. Yeni sorunlar tanımlandıkları için liste güncellenir.

> [!IMPORTANT]  
> Güncelleştirmeyi uygulamadan önce bu bölümü gözden geçirin.

## <a name="update-process"></a>Güncelleştirme işlemi

### <a name="host-node-update-prerequisite-failure"></a>Ana bilgisayar düğümü güncelleştirme önkoşulu hatası

- Uygunsa Bu sorun 1905 güncelleştirmesi için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında güncelleştirme durumu, **ana bilgisayar düğümü güncelleştirme önkoşulu**nedeniyle başarısız olabilir. Bu genellikle, yetersiz boş disk alanına sahip bir ana bilgisayar düğümü nedeniyle oluşur.
- Düzeltmesi Konak düğümündeki disk alanını temizleme hakkında yardım almak için Azure Stack desteğe başvurun.
- Oluşum Genel olmayan

### <a name="preparation-failed"></a>Hazırlama başarısız oldu

- Uygunsa Bu sorun desteklenen tüm yayınlar için geçerlidir.
- Neden: 1905 Azure Stack güncelleştirme yüklenmeye çalışıldığında, güncelleştirmenin durumu başarısız olabilir ve durumu **PreparationFailed**olarak değişebilir. Bu, güncelleştirme kaynak sağlayıcısı 'nın (URP) dosyaları depolama kapsayıcısından işlenmek üzere bir iç altyapı paylaşımında doğru bir şekilde aktarmaması nedeniyle oluşur. 1905 güncelleştirme paketi, bu sorunun oluşma olasılığını daha büyük hale getirmek için önceki güncelleştirme paketlerinden daha büyüktür.
- Düzeltmesi Sürüm 1901 ' den başlayarak (1.1901.0.95), **Şimdi Güncelleştir** ' i (sürdürülmez) tıklayarak bu soruna geçicibir çözüm bulabilirsiniz. URP daha sonra önceki girişimden dosyaları temizler ve indirmeyi yeniden başlatır. Sorun devam ederse, [güncelleştirmeleri içeri ve yükleme güncelleştirmelerini](azure-stack-apply-updates.md)izleyerek güncelleştirme paketini el ile karşıya yüklemeniz önerilir.
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

### <a name="public-ip-address"></a>Genel IP adresi

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

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirme etkinliği denetim listesini gözden geçir](azure-stack-release-notes-checklist.md)
- [Güvenlik güncelleştirmelerinin listesini gözden geçirin](azure-stack-release-notes-security-updates.md)
