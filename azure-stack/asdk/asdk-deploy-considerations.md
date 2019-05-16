---
title: Azure Stack geliştirme Seti'ni (ASDK) dağıtım önkoşulları | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK için) ortam ve donanım gereksinimlerini gözden geçirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2019
ms.author: mabrigg
ms.reviewer: misainat
ms.lastreviewed: 05/13/2019
ms.openlocfilehash: 9cb349ec19edd493ca994b406b9311fe27bed242
ms.sourcegitcommit: 87d93cdcdb6efb06e894f56c2f09cad594e1a8b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65712237"
---
# <a name="azure-stack-deployment-planning-considerations"></a>Azure Stack dağıtım planlama konuları

Azure Stack geliştirme Seti'ni (ASDK) dağıtmadan önce Geliştirme Seti ana bilgisayarınız bu makalede açıklanan gereksinimleri karşıladığından emin olun.

## <a name="hardware"></a>Donanım

| Bileşen | Minimum | Önerilen |
| --- | --- | --- |
| Disk sürücüler: İşletim Sistemi |1 işletim sistemi diski ile en az 200 GB (SSD veya HDD) sistem bölümü için kullanılabilir |Sistem bölümü için en az 200 GB'lık kullanılabilir alana sahip 1 işletim sistemi diski (SSD veya HDD) |
| Disk sürücüler: Genel Geliştirme Seti veri<sup>*</sup>  |4 disk. Her disk en az 240 GB'lık kapasiteye (SSD veya HDD) sahiptir. Kullanılabilir tüm diskler kullanılır. |4 disk. Her disk en az 400 GB'lık kapasiteye (SSD veya HDD) sahiptir. Kullanılabilir tüm diskler kullanılır. |
| İşlem: CPU |Çift yuvalı: 16 fiziksel çekirdek (toplam) |Çift yuvalı: 20 fiziksel çekirdek (toplam) |
| İşlem: Bellek |192 GB RAM |256 GB RAM |
| İşlem: BIOS |Hyper-V Etkin (SLAT desteğiyle) |Hyper-V Etkin (SLAT desteğiyle) |
| Ağ: NIC |Windows Server 2012 R2 sertifikası'ı seçin. Özelleştirilmiş gerekli herhangi bir özellik |Windows Server 2012 R2 sertifikası'ı seçin. Özelleştirilmiş gerekli herhangi bir özellik |
| Donanım logosu sertifikası |[Windows Server 2012 R2 için sertifikalıdır](https://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Windows Server 2016 için sertifikalı](https://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

<sup>*</sup> Daha fazla kapasite birçok eklemeyi planlıyorsanız bu önerilir ihtiyacınız [Market öğesi](../operator/azure-stack-create-and-publish-marketplace-item.md) azure'dan.

### <a name="hardware-notes"></a>Donanım notları

**Veri disk sürücüsü yapılandırması:** Tüm veri sürücüleri aynı türü (tümü SAS, tüm SATA veya tüm NVMe) ve kapasitede olmalıdır. SAS disk sürücüleri kullanılıyorsa disk sürücülerinin (MPIO kullanılmaz, çok yollu destek sağlanır) tek bir yolla bağlanması gerekir.

**HBA yapılandırma seçenekleri**

* (Tercih edilen) Basit HBA
* RAID HBA - bağdaştırıcısı "geçiş" modunda yapılandırılmış olması gerekir
* RAID HBA - disklerin yapılandırılmalıdır olarak tek Disk ve RAID-0

**Desteklenen veri yolu ve medya türü bileşimleri**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (medya türü belirtilmemişse/bilinmiyorsa<sup>*</sup>)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

<sup>*</sup> Geçiş özelliği olmayan RAID denetleyicileri, medya türünü tanıyamaz. Bu tür denetleyiciler, hem HDD ve SSD belirtilmemiş olarak işaretler. Bu durumda, önbelleğe alma cihazları yerine kalıcı depolama olarak SSD kullanılır. Bu nedenle, Geliştirme Seti Bu Sdd'ler üzerinde dağıtabilirsiniz.

**Örnek HBA'lar**: LSI 9207-8i, LSI-9300-8i veya LSI-9265-8i modunda

Örnek OEM yapılandırmaları kullanılabilir.

### <a name="storage-resiliency-for-the-asdk"></a>ASDK için depolama esnekliği

Bir tek düğümlü sistem ASDK üretim yedeklilik Azure Stack tümleşik sistemi doğrulamak için tasarlanmamıştır. Ancak, temel alınan ASDK aracılığıyla optimum HDD ve SSD sürücülerin depolama fazlalık düzeyini artırabilirsiniz. Bir RAID1 için benzer bir iki yönlü yansıtma yapılandırması yerine bir RADI0 için benzer bir basit dayanıklılık yapılandırması dağıtabilirsiniz. Yeterli kapasite, türü ve sayısı için temel alınan depolama alanları doğrudan yapılandırması kullanın.

Depolama dayanıklılık için iki yönlü yansıtma yapılandırmasını kullanmak için:

- HDD kapasitesi sisteminde iki terabayttan daha büyük.
- İçinde ASDK SSD yoksa, bir iki yönlü yansıtma yapılandırması için en az sekiz HDD'ler gerekir.
- SSD'ler, HDD'ler ile birlikte, ASDK varsa, en az beş HDD'ler gerekir. Ancak, altı HHDs önerilir. Altı HDD için de karşılık gelen en az üç SSD sistemde sahip bir önbellek sürücüsü (SSD) hizmet vermemesini iki kapasite sürücüsü (HDD) sahip olduğunuz için tavsiye.

Örnekte iki yönlü yansıtma yapılandırması:

- Sekiz HDD
- Üç SSD / altı HDD
- Dört SSD / sekiz HDD

## <a name="operating-system"></a>İşletim sistemi
|  | **Gereksinimler** |
| --- | --- |
| **İşletim sistemi sürümü** |Windows Server 2016 veya sonraki sürümleri. Dağıtım başlamadan önce Azure Stack yüklemede VHD'ye önyüklemeden ana bilgisayar önyükleme kritik işletim sistemi sürümü değildir. İşletim sistemini ve gerekli tüm düzeltme eklerini görüntüye önceden tümleştirilmiştir. Tüm anahtarları Geliştirme Seti kullanılan herhangi bir Windows Server örneklerine etkinleştirmek için kullanmayın. |

> [!TIP]
> Kullanabileceğiniz işletim sistemini yükledikten sonra [Azure Stack için dağıtım denetleyicisi](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız gereksinimleri karşıladığını onaylamak için.

## <a name="account-requirements"></a>Hesap gereksinimleri
Genellikle, burada, Microsoft Azure'a bağlanabilirsiniz, internet bağlantısı ile geliştirme Seti'ni dağıtın. Bu durumda, Geliştirme Seti dağıtmak için bir Azure Active Directory (Azure AD) hesabını yapılandırmanız gerekir.

Ortamınız internet'e bağlı değil veya Azure AD kullanmak istemiyorsanız, Azure Stack, Active Directory Federasyon Hizmetleri (AD FS) kullanarak dağıtabilirsiniz. Geliştirme Seti, kendi AD FS ve Active Directory Domain Services örneklerini içerir. Bu seçeneği kullanarak dağıtırsanız, önceden hesapları ayarlamanız gerekmez.

> [!NOTE]
> AD FS seçeneğini kullanarak dağıtırsanız, Azure Stack, Azure AD'ye geçiş yapmak için yeniden dağıtmanız gerekir.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory hesapları
Azure Stack, Azure AD hesabı kullanarak dağıtmak için dağıtım PowerShell Betiği çalıştırmadan önce bir Azure AD hesap hazırlamanız gerekir. Bu hesap, Azure AD kiracınız için genel yönetici olur. Bu, sağlamak ve uygulama ve hizmet sorumluları için Azure Active Directory Graph API'si ile etkileşimde bulunan tüm Azure Stack Hizmetleri temsilci seçmek için kullanılır. (Daha sonra değiştirebilirsiniz) varsayılan sağlayıcı aboneliği sahibi olarak da kullanılır. Azure Stack sisteminizin Yönetici portalına bu hesabı kullanarak oturum açabilirsiniz.

1. İçin en az bir Azure AD dizin yöneticisi olan bir Azure AD hesabı oluşturun. Zaten bir hesabınız varsa bu hesabı kullanabilirsiniz. Aksi takdirde, ücretsiz, oluşturabilirsiniz [ https://azure.microsoft.com/free/ ](https://azure.microsoft.com/free/) (Çin'de ziyaret <https://go.microsoft.com/fwlink/?LinkID=717821> yerine). Daha sonra planlıyorsanız [Azure Stack Azure ile kaydedin](asdk-register.md), ayrıca bir aboneliği bu hesabı yeni oluşturulmuş olması gerekir.
   
    Bu kimlik bilgilerini kullanmak için Hizmet Yöneticisi olarak kaydedin. Bu hesap, yapılandırma ve kaynak bulutlarını, kullanıcı hesaplarını, Kiracı planlarını, kotalar ve fiyatlandırma yönetin. Portalda web sitesi bulutları, sanal makine özel bulutları ve planlar oluşturup kullanıcı aboneliklerini yönetebilir.
1. Böylece, Geliştirme Seti Kiracı olarak oturum açarak Azure AD'NİZDE en az bir test kullanıcı hesabı oluşturun.
   
   | **Azure Active Directory hesabı** | **Destekleniyor mu?** |
   | --- | --- |
   | Geçerli genel Azure aboneliğine sahip iş veya Okul hesabı |Evet |
   | Geçerli genel Azure aboneliğine sahip Microsoft Account |Evet |
   | Geçerli Çin Azure aboneliğine sahip iş veya Okul hesabı |Evet |
   | Geçerli ABD Azure aboneliğine sahip iş veya Okul hesabı |Evet |

Dağıtımdan sonra Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler, genel yönetici kimlik bilgileri gerektirebilir. Örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilecek gerektiren yeni bir özelliktir. Geçici olarak hesap genel yönetici izinleri yeniden devreye sokmanız veya sahiplerinden biri olan ayrı bir genel yönetici hesabı kullanın *varsayılan sağlayıcı aboneliği*.

## <a name="network"></a>Ağ
### <a name="switch"></a>Switch
Mevcut bir bağlantı noktası üzerinde Geliştirme Seti makine için bir anahtar.  

Geliştirme Seti makine, bir anahtar erişimi bağlantı noktasına veya santral bağlantı noktasına bağlanmayı destekler. Anahtar üzerinde özelleştirilmiş herhangi bir özellik gerekmez. Santral bağlantı noktası kullanıyorsanız veya VLAN kimliği yapılandırmanız gerekiyorsa dağıtım parametresi olarak VLAN kimliğini sağlamanız gerekir.

### <a name="subnet"></a>Alt ağ
Geliştirme Seti makine aşağıdaki alt ağlara bağlanma:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Bu alt ağlara Geliştirme Seti ortamındaki iç ağlar için ayrılmıştır.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Yalnızca IPv4 desteklenir. IPv6 ağı oluşturamazsınız.

### <a name="dhcp"></a>DHCP
Ağ üzerinde NIC'nin bağlanabileceği kullanılabilir bir DHCP sunucusunun olduğundan emin olun. Kullanılabilir bir DHCP sunucusu yoksa ana bilgisayar tarafından kullanılanın dışında ek bir statik IPv4 ağı hazırlamanız gerekir. Dağıtım parametresi olarak bu IP adresini ve ağ geçidini sağlamanız gerekir.

### <a name="internet-access"></a>İnternet erişimi
Azure Stack, doğrudan veya saydam bir ara sunucu üzerinden Internet erişimi gerektirir. Azure Stack, Internet erişimi etkinleştirmek için bir web proxy yapılandırmasını desteklemiyor. Hem ana bilgisayar IP'SİNİN hem de (DHCP veya statik IP tarafından) AzS-BGPNAT01 atanan yeni IP'nin Internet erişimine sahip olmalıdır. Bağlantı noktaları 80 ve 443, graph.windows.net ve login.microsoftonline.com etki alanlarının altında kullanılır.


## <a name="next-steps"></a>Sonraki adımlar

- [ASDK dağıtım paketini indirme](asdk-download.md)
- Depolama alanları doğrudan ile ilgili daha fazla bilgi için bkz. [depolama alanları doğrudan genel bakış](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview).

