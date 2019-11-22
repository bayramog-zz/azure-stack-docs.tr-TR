---
title: Sürüm notlarını Azure Stack | Microsoft Docs
description: Yenilikler ve güncelleştirmenin indirileceği dahil Azure Stack tümleşik sistemler için güncelleştirmeler hakkında bilgi edinin.
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
ms.openlocfilehash: 81a454fbe2db7d72d94eb499ad276ff28d33f048
ms.sourcegitcommit: 0b783e262ac87ae67929dbd4c366b19bf36740f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310079"
---
# <a name="azure-stack-updates-release-notes"></a>Azure Stack güncelleştirmeler: sürüm notları

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede Azure Stack güncelleştirme paketlerinin içerikleri açıklanır. Güncelleştirme yenilikleri ve bu Azure Stack sürümü için düzeltmeler içerir.

Farklı bir sürüme yönelik sürüm notlarına erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın.

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> Azure Stack örneğiniz ikiden fazla güncelleştirme tarafından arkasındaysa uyumsuz olarak kabul edilir. [Destek almak için en az desteklenen sürüme güncelleştirmeniz](azure-stack-servicing-policy.md#keep-your-system-under-support)gerekir.
::: moniker-end

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

Güncelleştirmeler ve güncelleştirme sürecinde sorun giderme hakkında yardım için bkz. [Azure Stack için düzeltme eki ve güncelleştirme sorunlarını giderme](azure-stack-updates-troubleshoot.md).

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->
::: moniker range="azs-1910"
## <a name="1910-build-reference"></a>1910 derleme başvurusu

Azure Stack 1910 güncelleştirme yapı numarası **1.1910.0.58**.

### <a name="update-type"></a>Güncelleştirme türü

1908 ile başlayarak, Azure Stack çalıştığı temel işletim sistemi Windows Server 2019 ' e güncelleştirildi. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.

Azure Stack 1910 güncelleştirme yapı türü **Express**'dir.

1910 güncelleştirme paketi önceki güncelleştirmelerle karşılaştırıldığında boyut olarak daha büyük. Artan boyut, daha uzun karşıdan yükleme süreleriyle sonuçlanır. Güncelleştirme, **hazırlama** durumunda uzun bir süre kalacak ve işleçler bu işlemin önceki güncelleştirmelerden daha uzun sürme süresini bekleyebilir. 1910 güncelleştirmesinin tamamlaması için gereken süre, Azure Stack ortamınızdaki fiziksel düğümlerin sayısından bağımsız olarak yaklaşık 10 saattir. Tam güncelleştirme çalışma zamanları genellikle, kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanımınızın belirtimleri tarafından sisteminizde kullanılan kapasiteye bağlıdır. Beklenen değerden daha uzun süre içinde yer kalmayan çalışma zamanları, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir. Bu çalışma zamanı yaklaşık 1910 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- Yönetici portalı artık daha kolay bulma için bölge özellikleri menüsünde ayrıcalıklı uç nokta IP adreslerini gösterir. Ayrıca, geçerli yapılandırılmış saat sunucusunu ve DNS ileticilerini gösterir.

- Azure Stack sistem durumu ve izleme sistemi artık bir hata oluşursa çeşitli donanım bileşenlerine yönelik uyarılar oluşturabilir. Bunun için ek yapılandırma gerekir. Daha fazla bilgi için bkz. [Azure Stack donanım bileşenlerini izleme](azure-stack-hardware-monitoring.md).

- [Azure Stack Için Cloud-init desteği](/azure/virtual-machines/linux/using-cloud-init): Cloud-Init, Linux VM 'yi ilk kez önyüklediğinde özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Cloud-init önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanıza uygulayabileceğiniz gerekli aracı yoktur. Market 'teki Ubuntu görüntüleri, sağlama için Cloud-init ' i destekleyecek şekilde güncelleştirilmiştir.

- Azure Stack artık Azure olarak tüm Windows Azure Linux Aracısı sürümlerini desteklemektedir.

- Azure Stack admin PowerShell modüllerinin yeni bir sürümü kullanılabilir. <!-- For more information, see -->

- Azure Stack altyapısında Windows Defender tanımları için el ile güncelleştirmeyi yapılandırmak üzere ayrıcalıklı uç noktaya (PEP) **set-Azssavunma Dermanualupdate** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack Windows Defender virüsten koruma 'Yı güncelleştirme](azure-stack-security-av.md).

- Azure Stack altyapısında Windows Defender tanımlarının el ile güncelleştirme tanımlarını almak için, ayrıcalıklı uç noktaya (PEP) **Get-Azssavundermanualupdate** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack Windows Defender virüsten koruma 'Yı güncelleştirme](azure-stack-security-av.md).

- Azure Stack içindeki DNS sunucularının iletici ayarlarını değiştirmek için, ayrıcalıklı uç noktaya (PEP) **set-AzSDnsForwarder** cmdlet 'i eklendi. DNS yapılandırması hakkında daha fazla bilgi için bkz. [Azure Stack Datacenter DNS tümleştirmesi](azure-stack-integrate-dns.md).

- Azure Stack içindeki DNS sunucularının iletici ayarlarını almak için, ayrıcalıklı uç noktaya (PEP) **Get-AzSDnsForwarder** cmdlet 'i eklendi. DNS yapılandırması hakkında daha fazla bilgi için bkz. [Azure Stack Datacenter DNS tümleştirmesi](azure-stack-integrate-dns.md).


### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Bir dış cihaz (örneğin, USB anahtarı) Azure Stack altyapısının bir düğümüne bağlandığında raporlamak için denetim kuralı eklendi. Denetim günlüğü Syslog aracılığıyla yayınlanır ve **Microsoft-Windows-Security-Auditing olarak görüntülenecektir: 6416 | Olayları Tak ve Kullan**. Syslog istemcisinin nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [Syslog iletme](azure-stack-integrate-security.md).

- Azure Stack iç sertifikalar için 4096 bit RSA anahtarına taşınıyor. İç gizli dizi döndürmeyi çalıştırmak, eski 2048 bit sertifikalarını 4096 bit uzun sertifikalarla değiştirecek. Azure Stack 'de gizli anahtar döndürme hakkında daha fazla bilgi için bkz. [gizli dizileri Azure Stack döndürme](azure-stack-rotate-secrets.md).

- Birçok iç bileşen için şifreleme algoritmalarının karmaşıklığına ve anahtar gücüne, genel standartları güvenli hale getirmek için en iyi yöntemleri sağlayan, bu arada, ulusal güvenlik sistemleri-Policy 15 (CNSSP-15) üzerindeki komite ile uyum sağlamak için yükseltmeler bilgi paylaşımı. Geliştirmeler arasında Kerberos kimlik doğrulaması için AES256 ve VPN şifrelemesi için SHA384 vardır. CNSSP-15 hakkında daha fazla bilgi için, [ulusal güvenlik sistemleri, ilkeler sayfasında komite 'ye](http://www.cnss.gov/CNSS/issuances/Policies.cfm)bakın.

- Yukarıdaki yükseltmenin sonucu olarak, Azure Stack artık IPSec/Ikev2 yapılandırmalarına yönelik yeni varsayılan değerlere sahiptir. Azure Stack tarafında kullanılan yeni varsayılan değerler aşağıdaki gibidir:

   **IKE Aşama 1 (ana mod) parametreleri**

   | Özellik              | Değer|
   |-|-|
   | IKE Sürümü           | IKEv2 |
   |Diffie-Hellman Grubu   | ECP384 |
   | Kimlik doğrulama Yöntemi | Önceden paylaşılan anahtar |
   |Şifreleme ve Karma Algoritmaları | AES256, SHA384 |
   |SA Yaşam Süresi (Zaman)     | 28.800 saniye|

   **IKE aşama 2 (hızlı mod) parametreleri**

   | Özellik| Değer|
   |-|-|
   |IKE Sürümü |IKEv2 |
   |Şifreleme & karma algoritmaları (şifreleme)     | GCMAES256|
   |Şifreleme & karma algoritmaları (kimlik doğrulaması) | GCMAES256|
   |SA Yaşam Süresi (Zaman)  | 27.000 saniye  |
   |SA yaşam süresi (kilobayt) | 33.553.408     |
   |Kusursuz İletme Gizliliği (PFS) | ECP384 |
   |Kullanılmayan Eş Algılama | Desteklenen|

   Bu değişiklikler [varsayılan IPSec/IKE teklif](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters) belgelerinde de yansıtılır.

- Altyapı yedekleme hizmeti, sabit bir eşiğe güvenmek yerine, yedeklemeler için istenen boş alanı hesaplayan mantığı geliştirir. Hizmet, bir uyarının çalışır duruma getirilmesi gerekip gerekmediğini öğrenmek için yedekleme, bekletme ilkesi, ayırma ve dış depolama konumunun geçerli kullanım boyutunu kullanır. 

### <a name="changes"></a>Değişiklikler

- Market öğelerini Azure 'dan Azure Stack 'e indirirken, birden fazla sürüm mevcut olduğunda öğenin bir sürümünü belirtmenizi sağlayan yeni bir kullanıcı arabirimi vardır. Yeni Kullanıcı arabirimi hem bağlı hem de bağlantısı kesik senaryolarda kullanılabilir. Daha fazla bilgi için bkz. [Azure 'dan Market öğelerini indirme Azure Stack](azure-stack-download-azure-marketplace-item.md).  

- 1910 ' den başlayarak Azure Stack sistem ek/20 özel iç IP alanı gerektirir. Bu ağ Azure Stack sistemine özeldir ve veri merkezinizdeki birden çok Azure Stack sisteminde yeniden kullanılabilir. Ağ Azure Stack için özel olsa da, veri merkezinizdeki bir ağla çakışmaması gerekir. /20 özel IP alanı, kapsayıcılar üzerinde Azure Stack altyapısını çalıştırmayı etkinleştiren birden çok ağa bölünür (daha önce [1905 sürüm notlarında](release-notes.md?view=azs-1905)bahsedildiği gibi). Kapsayıcılarda Azure Stack altyapısını çalıştırmanın hedefi, kullanımı iyileştirmek ve performansı artırmaktır. Ayrıca,/20 özel IP alanı, dağıtımdan önce gerekli yönlendirilebilir IP alanını azaltacak devam eden çabaları etkinleştirmek için de kullanılır.

  - /20 girişinin bir sonraki Azure Stack güncelleştirmesine önkoşul olarak işlev gördüğüne lütfen unutmayın. Sonraki Azure Stack güncelleştirmesi yayınlandığında ve bu eklentiyi yüklemek için bir girişim yapıldığında, düzeltme adımlarında aşağıda açıklanan şekilde/20 girişi tamamlamadıysanız güncelleştirme başarısız olur. Yukarıdaki düzeltme adımları tamamlanana kadar yönetici portalında bir uyarı sunulacaktır. Bu yeni özel alanın nasıl kullanılacağını anlamak için lütfen [veri merkezi ağ tümleştirme](azure-stack-network.md#private-network) makalesine bakın. 

  - Düzeltme adımları: [BIR PEP oturumu açmak](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)için yönergeleri izleyin. Özel bir [Iç IP aralığı](azure-stack-network.md#logical-networks) /20 hazırlayın ve şu biçimi kullanarak Pep oturumunda Şu cmdlet 'i çalıştırın: `Set-AzsPrivateNetwork -UserSubnet 100.87.0.0/20`. İşlem başarıyla gerçekleştirilirse, **yapılandırmaya eklenen AZS Iç ağ aralığı**iletisini alırsınız. Başarıyla tamamlanırsa, uyarı yönetici portalında kapanır. Azure Stack sistem artık sonraki sürüme güncelleştirebilecektir.
  
- Altyapı yedekleme hizmeti, karşıya yükleme yordamı sırasında dış depolama konumunun kapasitesi tükeniyorsa kısmen karşıya yüklenen yedekleme verilerini siler.  

- Altyapı yedekleme hizmeti, AAD dağıtımları için yedekleme yüküne kimlik hizmeti ekler.  

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Azure Stack 1904 sürümünden önce dağıtılan ortamlarda TLS 1,2 ilkesinin zorlanmasının önlendiği bir sorun düzeltildi.
- SSH yetkilendirmesi etkin olarak oluşturulan bir Ubuntu 18,04 VM 'sinin, oturum açmak için SSH anahtarlarını kullanmanıza izin vermediği bir sorun düzeltildi. 
- Sanal makine ölçek kümesi kullanıcı arabiriminden **sıfırlama parolası** kaldırıldı.
- Yük dengeleyiciyi portaldan silmenin, altyapı katmanındaki nesnenin silinmesine neden olmadığı bir sorun düzeltildi.
- Yönetici portalında ağ geçidi havuzu kullanım uyarısının yanlış bir yüzdesini gösteren bir sorun düzeltildi.
- Bir sanal makinede aynı NIC 'de birden fazla genel IP eklemenin bir sorunu düzeltildi Internet bağlantısı sorunları. Şimdi, iki genel IP içeren bir NIC beklendiği gibi çalışmalıdır.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1910 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1910 ' e güncelleştirmeden önce 1908 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="prerequisites-before-applying-the-1910-update"></a>Önkoşullar: 1910 güncelleştirmesini uygulamadan önce

Azure Stack 1910 sürümü aşağıdaki düzeltmelere sahip 1908 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1908.9.43](https://support.microsoft.com/help/4531007)

### <a name="after-successfully-applying-the-1910-update"></a>1910 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1910 için kullanılabilir düzeltme yok.
::: moniker-end

::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 derleme başvurusu

Azure Stack 1908 güncelleştirme yapı numarası **1.1908.4.33**.

### <a name="update-type"></a>Güncelleştirme türü

1908 için, Azure Stack çalıştığı temel işletim sistemi Windows Server 2019 ' e güncelleştirilmiştir. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.

Azure Stack 1908 güncelleştirme yapı türü **dolu**. Sonuç olarak, 1908 güncelleştirmesinin 1906 ve 1907 gibi hızlı güncelleştirmelerden daha uzun bir çalışma zamanı vardır. Tam güncelleştirmeler için tam çalışma zamanları genellikle Azure Stack örneğinizin içerdiği düğüm sayısına, kiracı iş yükleri tarafından sisteminizde kullanılan kapasiteye, sisteminizin ağ bağlantısına (internet 'e bağlıysa) ve sistem donanımınıza bağlıdır yapılandırmada. 1908 güncelleştirmesi, iç sınamamız içinde şu beklenen çalışma zamanlarını içeriyordu: 4 düğüm-42 saat, 8 düğüm-50 saat, 12 düğüm-60 saat, 16 düğüm-70 saat. Bu beklenen değerlerin daha uzun süre içinde güncelleştirilmesi, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir.

Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için bkz. [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md).

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1908 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- 1908 için, Azure Stack çalıştığı temeldeki işletim sisteminin Windows Server 2019 ' e güncelleştirildiğini unutmayın. Bu, temel temel geliştirmelerin yanı sıra, yakın gelecekte Azure Stack için ek özellikler getirme özelliğini sağlar.
- Azure Stack altyapısının tüm bileşenleri artık FIPS 140-2 modunda çalışır.
- Azure Stack işleçler artık Portal Kullanıcı verilerini kaldırabilir. Daha fazla bilgi için bkz. [Azure Stack Portal Kullanıcı verilerini temizleme](azure-stack-portal-clear.md).

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->
- Fiziksel düğümlerin donanım Güvenilir Platform Modülü (TPM) ile gizli dizileri kalıcı hale getirmek için Azure Stack Rest şifrelemesinde veri geliştirmeleri.

### <a name="changes"></a>Değişiklikler

- Donanım sağlayıcıları, Azure Stack sürüm 1908 ile aynı anda OEM Uzantı paketi 2,1 veya üstünü serbest bırakır. OEM uzantısı paketi 2,1 veya üzeri, Azure Stack sürüm 1908 için bir önkoşuldur. OEM Uzantı paketi 2,1 veya sonraki bir sürümünü indirme hakkında daha fazla bilgi için sisteminizin donanım sağlayıcısına başvurun ve [OEM güncelleştirmeleri](azure-stack-update-oem.md#oem-contact-information) makalesine bakın.  

### <a name="fixes"></a>Düzeltmeleri

- Gelecekteki Azure Stack OEM güncelleştirmeleriyle uyumlulukla ilgili bir sorun ve müşteri Kullanıcı görüntülerini kullanarak VM dağıtımıyla ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) düzeltildi  
- Yapı halka durumu için test-AzureStack 'te OEM üretici yazılımı güncelleştirmesiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) düzeltildi
- OEM üretici yazılımı güncelleştirme işlemiyle ilgili bir sorun düzeltildi. Bu sorun 1907 sürümünde bulunur ve düzeltme [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) düzeltildi

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="download-the-update-1908"></a>Güncelleştirmeyi indir

Azure Stack 1908 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1908 ' e güncelleştirmeden önce 1907 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="prerequisites-before-applying-the-1908-update"></a>Önkoşullar: 1908 güncelleştirmesini uygulamadan önce

Azure Stack 1908 sürümü aşağıdaki düzeltmelere sahip 1907 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.18.56](https://support.microsoft.com/help/4528552)

Azure Stack 1908 güncelleştirmesi, sisteminizin donanım sağlayıcısından **Azure Stack OEM sürüm 2,1 veya üstünü** gerektirir. OEM güncelleştirmeleri, Azure Stack sistem Donanımınıza yönelik sürücü ve bellenim güncelleştirmelerini içerir. OEM güncelleştirmelerini uygulama hakkında daha fazla bilgi için bkz. [Apply Azure Stack özgün ekipman üreticisi güncelleştirmeleri](azure-stack-update-oem.md)

### <a name="after-successfully-applying-the-1908-update"></a>1908 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1908.9.43](https://support.microsoft.com/help/4531007)
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-build-reference"></a>1907 derleme başvurusu

Azure Stack 1907 güncelleştirme yapı numarası **1.1907.0.20**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1907 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. İç teste bağlı olarak, 1907 güncelleştirmesinin tamamlaması için gereken süre yaklaşık 13 saattir.

- Tam güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanım yapılandırmanız tarafından sisteminizde kullanılan kapasiteye bağlıdır.
- Beklenenden uzun süre içinde olan çalışma zamanları seyrek değildir ve güncelleştirme başarısız olmazsa Azure Stack işleçlere göre eylem gerektirmez.
- Bu çalışma zamanı yaklaşık 1907 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>Yenilikler

<!-- What's new, also net new experiences and features. -->

- Tanılama günlüğü toplamayı kolaylaştırmak ve geliştirmek için Azure Stack tanılama günlüğü toplama hizmetinin genel kullanım sürümü. Azure Stack tanılama günlüğü toplama hizmeti, Microsoft Müşteri Destek Hizmetleri (CSS) ile tanılama günlüklerini toplayıp paylaşmak için basitleştirilmiş bir yöntem sağlar. Bu tanılama günlüğü toplama hizmeti, operatörlerin bir depolama blobuna belirli kritik uyarıların ne zaman yapıldığında otomatik olarak yüklenmesini veya aynı işlemi gerçekleştirmesini sağlayan Azure Stack yönetici portalında yeni bir kullanıcı deneyimi sağlar. isteğe bağlı işlem. Daha fazla bilgi için bkz. [tanılama günlüğü toplama](azure-stack-diagnostic-log-collection-overview.md) makalesi.

- Azure Stack doğrulama aracının **Test-AzureStack**'in bir parçası olarak Azure Stack ağ altyapısı doğrulamasının genel kullanım sürümü. Azure Stack ağ altyapısı, Azure Stack ağ altyapısında bir hata oluşup oluşmadığını belirlemek için **Test-AzureStack**'in bir parçası olacaktır. Test, yazılım tanımlı Azure Stack ağı atlayarak ağ altyapısının bağlantısını denetler. Ortak bir VIP 'den yapılandırılmış DNS ileticilerine, NTP sunucularına ve kimlik uç noktalarına olan bağlantıyı gösterir. Ayrıca, kimlik sağlayıcısı olarak Azure AD 'yi kullanırken veya ADFS kullanılırken Federasyon sunucusu ile Azure 'a bağlantıyı denetler. Daha fazla bilgi için [Azure Stack doğrulama aracı](azure-stack-diagnostic-test.md) makalesine bakın.

- Bir sistem güncelleştirmesi sırasında, iç SQL TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli dizi döndürme yordamı eklenmiştir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack güncelleştirme dikey penceresinde artık etkin güncelleştirmeler için **son adım tamamlanmış** bir zaman görüntülenir. Bu, güncelleştirme dikey penceresine gidip çalışan bir güncelleştirmeye tıklanarak görülebilir. **Son adım tamamlandığında** **güncelleştirme çalıştırması ayrıntıları** bölümünde kullanılabilir.

- **Start-AzureStack** ve **stop-azurestack** işleç eylemlerine yönelik iyileştirmeler. Azure Stack başlama zamanı, ortalama %50 oranında azaltılmıştır. Azure Stack kapatma süresi, ortalama %30 oranında azaltılmıştır. Ortalama başlatma ve kapatılma süreleri, bir ölçek birimindeki düğümlerin sayısı arttıkça aynı kalır.

- Bağlantısı kesilen Market aracı için geliştirilmiş hata işleme. **Export-AzSOfflineMarketplaceItem**kullanılırken bir indirme başarısız olursa veya kısmen başarılı olursa, hata ve risk azaltma adımları hakkında daha fazla ayrıntı içeren ayrıntılı bir hata iletisi görüntülenir.

- Büyük bir sayfa blobundan/anlık görüntüsünden yönetilen disk oluşturma performansı geliştirilmiştir. Daha önce, büyük bir disk oluştururken bir zaman aşımı tetikledi.  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- Beklenmedik sanal disk ayırmayı önlemek için bir düğümü kapatmadan önce sanal disk sistem durumu denetimi geliştirildi.

- Yönetici işlemlerine yönelik iç günlüklerin depolanması geliştirildi. Bu durum, iç günlük işlemlerinin bellek ve depolama tüketimini en aza indirerek yönetici işlemleri sırasında performansı ve güvenilirliği elde ediyor. Yönetici portalındaki güncelleştirme dikey penceresinin Gelişmiş sayfa yükleme sürelerini de fark edebilirsiniz. Bu iyileştirmesinin bir parçası olarak, 6 aydan eski olan güncelleştirme günlükleri sistemde artık kullanılamaz. Bu güncelleştirmeler için günlüklere ihtiyacınız varsa, 1907 güncelleştirmesini gerçekleştirmeden önce 6 aydan eski tüm güncelleştirme çalıştırmaları için [Özeti indirdiğinizden](azure-stack-apply-updates.md) emin olun.

### <a name="changes"></a>Değişiklikler

- Azure Stack sürüm 1907, operatörlere 1908 güncelleştirme yapmadan önce işletmenlerinin sistem OEM paketini 2,1 veya sonraki bir sürüme güncelleştirdiğinizden emin olan bir uyarı uyarısı içerir. Azure Stack OEM güncelleştirmelerinin nasıl uygulanacağı hakkında daha fazla bilgi için, bkz. [Azure Stack orijinal donanım üreticisi güncelleştirmesi uygulama](azure-stack-update-oem.md).

- Azure Stack tanılama günlüğü toplama hizmeti için iletişimi etkinleştirmek üzere yeni bir giden kuralı (HTTPS) eklendi. Daha fazla bilgi için bkz. [Azure Stack Datacenter Integration-son nokta yayımlama](azure-stack-integrate-endpoints.md#ports-and-urls-outbound).

- Altyapı yedekleme hizmeti artık dış depolama konumu kapasiteyi tükeniyorsa kısmen karşıya yüklenen yedeklemeleri siler.

- Altyapı yedeklemeleri artık etki alanı Hizmetleri verilerinin bir yedeklemesini içermez. Bu, yalnızca kimlik sağlayıcısı olarak Azure Active Directory kullanan sistemler için geçerlidir.

- Artık, **işlem > VM görüntüleri** dikey penceresine alınan bir görüntünün Sayfa Blobu türünde olduğunu doğrulayacağız.

- Ayrıcalıklı uç nokta komutu **set-BmcCredential** artık temel kart yönetim denetleyicisindeki kimlik bilgilerini güncelleştirir.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->
- Yayımcı, teklif ve SKU 'nun bir Kaynak Yöneticisi şablonunda büyük/küçük harfe duyarlı olduğu bir sorun düzeltildi: görüntü parametreleri Yayımcı, teklif ve SKU ile aynı durumda değilse, görüntü dağıtım için getirilmedi.

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/129536438/home -->
- Depolama hizmeti meta verilerinin yedeklenmesi sırasında zaman aşımları nedeniyle, bir **Partialsucceeded** hata iletisiyle başarısız olan yedeklemelerle ilgili bir sorun düzeltildi.  

- Kullanıcı aboneliklerinin silinmesinin yalnız bırakılmış kaynaklarla sonuçlandığı bir sorun düzeltildi.

- Bir teklif oluşturulurken açıklama alanı kaydedilmemiş bir sorun düzeltildi.

- **Salt okuma** izinlerine sahip bir kullanıcının kaynakları oluşturabildiği, düzenleyebilen ve silebileceği bir sorun düzeltildi. Artık Kullanıcı, **katkıda** bulunan izin atandığında yalnızca kaynak oluşturabiliyor. 

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127772311/home -->
- WMI sağlayıcısı ana bilgisayarı tarafından kilitlenen bir DLL dosyası nedeniyle güncelleştirmenin başarısız olduğu bir sorun düzeltildi.

- Güncelleştirme hizmetinde, güncelleştirme kutucuğunda veya kaynak sağlayıcısında kullanılabilir güncelleştirmelerin görüntülenmesini önleyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4511282](https://support.microsoft.com/help/4511282/)içinde düzeltildi.

- Hatalı bir yapılandırma nedeniyle yönetim düzlemi sağlıksız olduğundan güncelleştirmelerin başarısız olmasına neden olabilecek bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794/)içinde düzeltildi.

- Kullanıcıların Market 'ten üçüncü taraf görüntülerin dağıtımını tamamlamasını engelleyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4511259](https://support.microsoft.com/help/4511259/)içinde düzeltildi.

- Kullanıcı görüntüsü Yöneticisi hizmeti kilitlenmekte olduğundan yönetilen görüntülerden VM oluşturma 'nın başarısız olmasına neden olabilecek bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794/) düzeltildi

- Uygulama ağ geçidi önbelleğinin beklendiği şekilde yenilenmediği için VM CRUD işlemlerinin başarısız olmasına neden olan bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4513119](https://support.microsoft.com/en-us/help/4513119/) düzeltildi

- Sistem durumu kaynak sağlayıcısında, yönetici portalındaki bölge ve uyarı dikey pencerelerinin kullanılabilirliğini etkileyen bir sorun düzeltildi. Bu sorun 1906 sürümünde bulunur ve düzeltme [KB4512794](https://support.microsoft.com/help/4512794)içinde düzeltildi.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1907 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1907 ' e güncelleştirmeden önce 1906 için en son Azure Stack düzeltmesini yüklediğinizden emin olun.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1907-update"></a>1907 güncelleştirmesini uygulamadan önce

Azure Stack 1907 sürümü aşağıdaki düzeltmelere sahip 1906 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.15.60](https://support.microsoft.com/help/4524559)

### <a name="after-successfully-applying-the-1907-update"></a>1907 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1907.18.56](https://support.microsoft.com/help/4528552)
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-build-reference"></a>1906 derleme başvurusu

Azure Stack 1906 güncelleştirme yapı numarası **1.1906.0.30**.

### <a name="update-type"></a>Güncelleştirme türü

Azure Stack 1906 güncelleştirme yapı türü **Express**'dir. Güncelleştirme oluşturma türleri hakkında daha fazla bilgi için [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md) makalesine bakın. 1906 güncelleştirmesinin tamamlaması için gereken süre, Azure Stack ortamınızdaki fiziksel düğümlerin sayısından bağımsız olarak yaklaşık 10 saattir. Tamamen güncelleştirme çalışma zamanları, genellikle kiracı iş yükleri, sistem ağ bağlantınız (internet 'e bağlıysa) ve sistem donanımınızın belirtimleri tarafından sisteminizde kullanılan kapasiteye bağlıdır. Beklenen değerden daha uzun süre içinde yer kalmayan çalışma zamanları, güncelleştirme başarısız olmadığı için Azure Stack işleçlere göre işlem gerektirmemektedir. Bu çalışma zamanı yaklaşık 1906 güncelleştirmesine özeldir ve diğer Azure Stack güncelleştirmeleriyle karşılaştırılmamalıdır.

## <a name="whats-in-this-update"></a>Bu güncelleştirmede neler vardır

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- Tüm uç noktalarında TLS 1,2 zorlamak için ayrıcalıklı uç noktaya (PEP) bir **set-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Uygulanan TLS ilkesini almak için ayrıcalıklı uç noktaya (PEP) bir **Get-TLSPolicy** cmdlet 'i eklendi. Daha fazla bilgi için bkz. [Azure Stack güvenlik denetimleri](azure-stack-security-configuration.md).

- Bir sistem güncelleştirmesi sırasında iç TLS sertifikalarını gereken şekilde döndürmek için bir iç gizli anahtar prosedürü eklendi.

- Süresi dolan gizli dizi uyarısı yok sayıldığında iç gizli dizileri zorlayarak iç Parolaların süresinin dolmasını önlemek için bir koruma eklendi. Bu, normal bir işletim yordamı olarak güvenilmemelidir. Gizli dizi yönü, bakım penceresi sırasında planlanmalıdır. Daha fazla bilgi için bkz. [Azure Stack gizli döndürme](azure-stack-rotate-secrets.md).

- Visual Studio Code artık AD FS kullanarak Azure Stack dağıtımı ile desteklenmektedir.

### <a name="improvements"></a>Geliştirmeleri

<!-- Changes and product improvements with tangible customer-facing value. -->

- Ayrıcalıklı uç noktasındaki **Get-GraphApplication** cmdlet 'i artık kullanılmakta olan sertifikanın parmak izini görüntüler. Bu, Azure Stack AD FS dağıtıldığında hizmet sorumluları için sertifika yönetimini geliştirir.

- Uyarı verme özelliği de dahil olmak üzere AD grafiğinin ve AD FS kullanılabilirliğini doğrulamak için yeni sistem durumu izleme kuralları eklenmiştir.

- Altyapı yedekleme hizmeti başka bir örneğe taşırken, yedekleme kaynak sağlayıcısının güvenilirliğini artırmaya yönelik geliştirmeler.

- Bakım penceresinin planlanmasını kolaylaştırmak için Tekdüzen yürütme süresi sağlamak üzere dış gizli anahtar döndürme yordamının performans iyileştirmesi.

- **Test-AzureStack** cmdlet 'i artık sona ermek üzere olan iç gizli dizileri (kritik uyarılar) bildiriyor.

- AD FS için Federasyon güveni yapılandırılırken, sertifika iptal listesi denetimini atlamayı sağlayan ayrıcalıklı uç noktada **register-CustomAdfs** cmdlet 'i için yeni bir parametre mevcuttur.

- 1906 sürümü güncelleştirme ilerleme durumunun daha büyük bir görünürlüğünü sunarak güncelleştirmelerin duraklatılmadığından emin olabilirsiniz. Bu, **güncelleştirme** dikey penceresindeki işleçlere gösterilen güncelleştirme adımlarının toplam sayısının artmasına neden olur. Ayrıca, önceki güncelleştirmelerden daha fazla güncelleştirme adımı olduğunu fark edebilirsiniz.

#### <a name="networking-updates"></a>Ağ güncelleştirmeleri

- DHCP Yanıtlayıcının Azure ile tutarlı olması için ayarlanan kira süresi güncelleştirildi.

- Kaynak sağlayıcıya, başarısız kaynakların dağıtımı senaryosunda geliştirilmiş yeniden deneme oranları.

- **Standart** SKU seçeneği şu anda desteklenmeyen şekilde hem yük dengeleyiciden hem de genel IP 'den kaldırılmıştır.

### <a name="changes"></a>Değişiklikler

- Depolama hesabı deneyimi oluşturmak artık Azure ile tutarlıdır.

- İç gizli dizi süresi sonu için uyarı Tetikleyicileri değiştirildi:
  - Uyarı uyarıları artık gizli dizi süresi dolmadan 90 gün önce tetiklenir.
  - Kritik uyarılar artık gizli dizileri sona ermeden önce 30 gün sonra tetiklenir.

- Tutarlı terminoloji için altyapı yedekleme kaynak sağlayıcısındaki dizeler güncelleştirildi.

### <a name="fixes"></a>Düzeltmeleri

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Yönetilen bir disk VM 'sinin yeniden boyutlandırılırken bir **Iç Işlem hatasıyla**başarısız olan bir sorun düzeltildi.

- Başarısız bir Kullanıcı görüntüsü oluşturmanın, görüntüleri yöneten hizmeti kötü durumda olduğu bir sorun düzeltildi; Bu, başarısız olan görüntünün silinmesini ve yeni görüntülerin oluşturulmasını engeller. Bu, 1905 düzeltmesinde de düzeltilir.

- Süresi dolan iç gizliliklerin etkin uyarıları, iç gizli döndürmenin başarılı bir şekilde yürütülmesinden sonra otomatik olarak kapatılır.

- Güncelleştirme geçmişi sekmesindeki güncelleştirme süresinin 99 saatten uzun bir süredir çalışıyor olması durumunda ilk basamağı kırpabileceği bir sorun düzeltildi.

- **Güncelleştirme** dikey penceresinde, başarısız güncelleştirmeler Için bir **özgeçmişi** seçeneği bulunur.

- Yönetici ve Kullanıcı portalları ' nde, Docker uzantısının aramadan hatalı şekilde döndürüldüğü, ancak başka bir eylem gerçekleştirilmediğinden, Azure Stack ' de kullanılamadığından Market 'teki sorunu düzeltildi.

- Şablon adı ' _ ' alt çizgiyle başlıyorsa parametreleri doldurmayan şablon dağıtım Kullanıcı arabirimindeki bir sorun düzeltildi.

- Sanal makine ölçek kümesi oluşturma deneyiminin dağıtım seçeneği olarak CentOS tabanlı 7,2 sağladığı bir sorun düzeltildi. CentOS 7,2 Azure Stack kullanılamıyor. Şimdi dağıtım için bir seçenek olarak CentOS 7,5 sunuyoruz

- Artık **Sanal Makine Ölçek Kümeleri** dikey penceresinden bir ölçek kümesini kaldırabilirsiniz.

## <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Bu Azure Stack güncelleştirmesinde güvenlik güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Stack Security Updates](release-notes-security-updates.md).

## <a name="update-planning"></a>Planlamayı Güncelleştir

Güncelleştirmeyi uygulamadan önce, aşağıdaki bilgileri gözden geçirdiğinizden emin olun:

- [Bilinen sorunlar](known-issues.md)
- [Güvenlik güncelleştirmeleri](release-notes-security-updates.md)
- [Güncelleştirme uygulandıktan önceki ve sonraki etkinliklerin denetim listesi](release-notes-checklist.md)

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1906 güncelleştirme paketini [Azure Stack indirme sayfasından](https://aka.ms/azurestackupdatedownload)indirebilirsiniz.

## <a name="hotfixes"></a>Düzeltmeler

Düzeltmeleri düzenli aralıklarla yayınlar Azure Stack. Azure Stack 1906 ' e güncelleştirmeden önce 1905 için en son Azure Stack düzeltmesini yüklediğinizden emin olun. Güncelleştirme sonrasında, [1906 için kullanılabilir düzeltmeleri](#after-successfully-applying-the-1906-update)yüklersiniz.

Azure Stack düzeltmeler yalnızca Azure Stack tümleşik sistemler için geçerlidir; ASDK 'ye düzeltme yüklemeyi denemeyin.

### <a name="before-applying-the-1906-update"></a>1906 güncelleştirmesini uygulamadan önce

Azure Stack 1906 sürümü aşağıdaki düzeltmelere sahip 1905 sürümüne uygulanmalıdır:

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 güncelleştirmesini başarıyla uyguladıktan sonra

Bu güncelleştirmenin yüklenmesinden sonra, geçerli düzeltmeleri yükleme. Daha fazla bilgi için bkz. [hizmet ilkeniz](azure-stack-servicing-policy.md).

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hotfix 1.1906.15.60](https://support.microsoft.com/help/4524559)
::: moniker-end

::: moniker range=">=azs-1906"
## <a name="automatic-update-notifications"></a>Otomatik güncelleştirme bildirimleri

Altyapı ağından internet 'e erişebilen sistemler, operatör portalındaki **güncelleştirme kullanılabilir** iletisini görür. İnternet erişimi olmayan sistemler. zip dosyasını indirebilir ve ilgili. xml ile içe aktarabilir.

> [!TIP]  
> Azure Stack düzeltmelere devam etmek için aşağıdaki *RSS* veya *atom* akışlarına abone olun:
>
> - ['YI](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archive"></a>Arşiv

Daha eski bir sürüm için arşivlenmiş sürüm notlarına erişmek için, sol taraftaki içindekiler tablosunun üzerindeki sürüm Seçicisi açılan listesini kullanın ve görmek istediğiniz sürümü seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-release-notes"></a>1905 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1904"
## <a name="1904-archived-release-notes"></a>1904 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-release-notes"></a>1903 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-release-notes"></a>1902 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-release-notes"></a>1901 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-release-notes"></a>1811 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-release-notes"></a>1809 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-release-notes"></a>1808 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-release-notes"></a>1807 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-release-notes"></a>1805 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-release-notes"></a>1804 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-release-notes"></a>1803 arşivlenmiş sürüm notları
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-release-notes"></a>1802 arşivlenmiş sürüm notları
::: moniker-end

::: moniker range="<azs-1906"
[Azure Stack sürüm notlarının eski sürümlerine TechNet Gallery ' de](https://aka.ms/azsarchivedrelnotes)erişebilirsiniz. Bu arşivlenmiş belgeler yalnızca başvuru amacıyla sağlanır ve bu sürümler için destek göstermez. Azure Stack desteği hakkında bilgi için bkz. [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md). Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri 'ne başvurun.
::: moniker-end


