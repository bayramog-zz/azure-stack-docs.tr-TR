---
title: ASDK gereksinimleri ve konuları | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) için donanım, yazılım ve ortam gereksinimleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 05/13/2019
ms.openlocfilehash: 691eac0c76763dcaae60aa6946c5dcbc479486a6
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579010"
---
# <a name="asdk-requirements-and-considerations"></a>ASDK gereksinimleri ve konuları

Azure Stack Geliştirme Seti (ASDK) dağıtmadan önce, ASDK ana bilgisayarınızın bu makalede açıklanan gereksinimleri karşıladığından emin olun.

## <a name="hardware"></a>Donanım

| Bileşen | Minimum | Önerilen |
| --- | --- | --- |
| Disk sürücüleri: İşletim sistemi |en az 200 GB işletim sistemi diski sistem bölümü (SSD veya HDD) için kullanılabilir. |Sistem bölümü (SSD veya HDD) için en az 200 GB kullanılabilir olan 1 işletim sistemi diski. |
| Disk sürücüleri: Genel Geliştirme Seti verileri<sup>*</sup>  |4 disk. Her disk en az 240 GB kapasite (SSD veya HDD) sağlar. Kullanılabilir tüm diskler kullanılır. |4 disk. Her disk en az 400 GB kapasite (SSD veya HDD) sağlar. Kullanılabilir tüm diskler kullanılır. |
| Hesaplamasını CPU |Çift Soketli: 16 fiziksel çekirdek (Toplam). |Çift Soketli: 20 fiziksel çekirdek (Toplam). |
| Hesaplamasını Bellek |192 GB RAM. |256 GB RAM. |
| Hesaplamasını 'TAKI |Hyper-V etkin (SLAT desteği ile). |Hyper-V etkin (SLAT desteği ile). |
| Network NIC |Windows Server 2012 R2 sertifikası. Özelleştirilmiş özellik gerekmez. | Windows Server 2012 R2 sertifikası. Özelleştirilmiş özellik gerekmez. |
| Donanım logosu sertifikası |[Windows Server 2012 R2 Için sertifikalıdır](https://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0). |[Windows Server 2016 Için sertifikalıdır](https://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0). |

<sup>*</sup>Azure 'dan birçok [Market öğesi](../operator/azure-stack-create-and-publish-marketplace-item.md) eklemeyi planlıyorsanız, bu önerilen kapasitenin daha fazlasına ihtiyacınız vardır.

### <a name="hardware-notes"></a>Donanım notları

**Veri diski sürücüsü yapılandırması:** Tüm veri sürücüleri aynı türde (tüm SAS, tüm SATA veya tüm NVMe) ve kapasitede olmalıdır. SAS disk sürücüleri kullanılıyorsa disk sürücülerinin (MPIO kullanılmaz, çok yollu destek sağlanır) tek bir yolla bağlanması gerekir.

**HBA yapılandırma seçenekleri**

* Sındadır Basit HBA.
* RAID HBA-bağdaştırıcı "geçiş" modunda yapılandırılmalıdır.
* RAID HBA-diskler tek disk, RAID-0 olarak yapılandırılmalıdır.

**Desteklenen veri yolu ve medya türü birleşimleri**

* SATA HDD
* SAS HDD
* RAID HDD
* RAID SSD (medya türü belirtilmemişse/bilinmiyorsa<sup>*</sup>)
* SATA SSD + SATA HDD
* SAS SSD + SAS HDD
* NVMe

<sup>*</sup>Geçişli özelliği olmayan RAID denetleyicileri medya türünü algılayamaz. Bu tür denetleyiciler hem HDD hem de SSD 'yi belirtilmemiş olarak işaretler. Bu durumda, SSD, cihazları önbelleğe almak yerine kalıcı depolama olarak kullanılır. Bu nedenle, ASDK 'yi bu SSD 'Ler üzerinde dağıtabilirsiniz.

**Örnek HBA**'lar: LSI 9207-8i, LSI-9300-8i veya LSI-9265-8ı geçişli modda.

Örnek OEM yapılandırmaları kullanılabilir.

### <a name="storage-resiliency-for-the-asdk"></a>ASDK için depolama dayanıklılığı

Tek düğümlü bir sistem olarak, ASDK Azure Stack tümleşik bir sistemin üretim yedekliliği doğrulamak için tasarlanmamıştır. Ancak, ASDK 'nin temeldeki depolama yedekliliği düzeyini HDD ve SSD sürücülerinin en uygun karışımı aracılığıyla artırabilirsiniz. Bir RAID0 benzer bir basit dayanıklılık yapılandırması yerine, RAID1 benzer bir iki yönlü yansıtma yapılandırması dağıtabilirsiniz. Temel Depolama Alanları Doğrudan yapılandırması için yeterli kapasite, tür ve sürücü sayısını kullanın.

Depolama esnekliği için iki yönlü bir yansıtma yapılandırması kullanmak için:

- İkiden büyük terabayt sisteminde HDD kapasiteniz olması gerekir.
- ASDK 'de SSD 'niz yoksa, iki yönlü yansıtma yapılandırması için en az sekiz HDD 'niz olması gerekir.
- ASDK 'de SSD 'niz varsa, HDD 'ler ile birlikte en az beş HDD 'niz olması gerekir. Ancak altı ssds önerilir. Altı HDD için, iki kapasite sürücüsüne (HDD) hizmeti sağlamak üzere bir önbellek diskine (SSD) sahip olmanız için sistemde en az üç karşılık gelen SSD 'nin olması da önerilir.

Örnek iki yönlü yansıtma yapılandırması:

- Sekiz HDD
- Üç SSD/altı HDD
- Dört SSD/sekiz HDD

## <a name="operating-system"></a>İşletim sistemi
|  | **Gereksinimler** |
| --- | --- |
| **İşletim sistemi sürümü** |Windows Server 2016 veya üzeri. Ana bilgisayarı Azure Stack yüklemesinde bulunan VHD 'ye önyüklediğiniz için, işletim sistemi sürümü dağıtım başlamadan önce kritik değildir. İşletim sistemi ve gerekli tüm düzeltme ekleri görüntüyle zaten tümleşiktir. ASDK 'de kullanılan herhangi bir Windows Server örneğini etkinleştirmek için herhangi bir anahtar kullanmayın. |

> [!TIP]
> İşletim sistemini yükledikten sonra, donanımınızın tüm gereksinimleri karşıladığından emin olmak için [Azure Stack Için dağıtım denetleyicisi](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) ' ni kullanabilirsiniz.

## <a name="account-requirements"></a>Hesap gereksinimleri
Genellikle, Microsoft Azure bağlandığınız internet bağlantısı ile ASDK 'yi dağıtırsınız. Bu durumda, ASDK 'yi dağıtmak için bir Azure Active Directory (Azure AD) hesabı yapılandırmanız gerekir.

Ortamınız internet 'e bağlı değilse veya Azure AD 'yi kullanmak istemiyorsanız, Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanarak Azure Stack dağıtabilirsiniz. ASDK, kendi AD FS ve Active Directory Domain Services örneklerini içerir. Bu seçeneği kullanarak dağıtıyorsanız, hesapları önceden ayarlamanız gerekmez.

> [!NOTE]
> AD FS seçeneğini kullanarak dağıtırsanız, Azure AD 'ye geçiş yapmak için Azure Stack yeniden dağıtmanız gerekir.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory hesapları
Azure Stack bir Azure AD hesabı kullanarak dağıtmak için, dağıtım PowerShell betiğini çalıştırmadan önce bir Azure AD hesabı hazırlamanız gerekir. Bu hesap, Azure AD kiracısı için genel yönetici olur. Azure AD ve Graph API etkileşime geçen tüm Azure Stack Hizmetleri için uygulama ve hizmet sorumlularını sağlamak ve devretmek için kullanılır. Bu, varsayılan sağlayıcı Aboneliğinin sahibi olarak da kullanılır (daha sonra değiştirebilirsiniz). Bu hesabı kullanarak Azure Stack sisteminizin yönetim portalında oturum açabilirsiniz.

1. En az bir Azure AD için Dizin Yöneticisi olan bir Azure AD hesabı oluşturun. Zaten bir hesabınız varsa bu hesabı kullanabilirsiniz. Aksi takdirde, adresinden [https://azure.microsoft.com/free/](https://azure.microsoft.com/free/) ücretsiz olarak bir tane oluşturabilirsiniz (Çin 'de, bunun yerine adresini ziyaret edin <https://go.microsoft.com/fwlink/?LinkID=717821> ). Daha sonra [Azure Stack Azure ile kaydetmeyi](asdk-register.md)planlıyorsanız, bu yeni oluşturulan hesapta da bir aboneliğiniz olması gerekir.
   
    Bu kimlik bilgilerini hizmet yöneticisi olarak kullanmak üzere kaydedin. Bu hesap, kaynak bulutlarını, Kullanıcı hesaplarını, kiracı planlarını, kotaları ve fiyatlandırmayı yapılandırabilir ve yönetebilir. Portalda, Web sitesi bulutları, VM özel bulutları oluşturabilir, planlar oluşturabilir ve Kullanıcı aboneliklerini yönetebilir.
1. Azure AD 'de, kiracı olarak ASDK 'de oturum açabilmeniz için en az bir test Kullanıcı hesabı oluşturun.
   
   | **Azure Active Directory hesabı** | **Destek?** |
   | --- | --- |
   | Geçerli genel Azure aboneliğiyle iş veya okul hesabı |Evet |
   | Geçerli genel Azure aboneliğine sahip Microsoft hesabı |Evet |
   | Geçerli Çin Azure aboneliğiyle iş veya okul hesabı |Evet |
   | Geçerli ABD kamu Azure aboneliğiyle iş veya okul hesabı |Evet |

Dağıtımdan sonra Azure AD Genel yönetici izni gerekli değildir. Ancak, bazı işlemler genel yönetici kimlik bilgileri gerektirebilir. Bu tür işlemlere örnek olarak bir kaynak sağlayıcısı yükleyici betiği veya izin verilmesi gereken yeni bir özellik dahildir. Hesabın genel yönetici izinlerini geçici olarak yeniden başlatabilir ya da *varsayılan sağlayıcı aboneliğinin*sahibi olan ayrı bir genel yönetici hesabı kullanabilirsiniz.

## <a name="network"></a>Ağ
### <a name="switch"></a>Anahtar
ASDK makinesi için bir anahtarda kullanılabilir bir bağlantı noktası.  

ASDK makinesi, anahtar erişim bağlantı noktasına veya santral bağlantı noktasına bağlanmayı destekler. Anahtar üzerinde özelleştirilmiş herhangi bir özellik gerekmez. Bir santral bağlantı noktası kullanıyorsanız veya bir VLAN KIMLIĞI yapılandırmanız gerekiyorsa, VLAN KIMLIĞINI bir dağıtım parametresi olarak sağlamanız gerekir.

### <a name="subnet"></a>Subnet
ASDK makinesini aşağıdaki alt ağlara bağlama:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Bu alt ağlar, ASDK ortamındaki iç ağlar için ayrılmıştır.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Yalnızca IPv4 desteklenir. IPv6 ağları oluşturamazsınız.

### <a name="dhcp"></a>DHCP
NIC 'nin bağlandığı ağda kullanılabilir bir DHCP sunucusu olduğundan emin olun. DHCP yoksa, ana bilgisayar tarafından kullanılan ek bir statik IPv4 ağı hazırlamanız gerekir. Dağıtım parametresi olarak bu IP adresini ve ağ geçidini sağlamanız gerekir.

### <a name="internet-access"></a>İnternet erişimi
Azure Stack, doğrudan veya saydam bir ara sunucu üzerinden internet erişimi gerektirir. Azure Stack, internet erişimini etkinleştirmek için bir Web proxy 'sinin yapılandırmasını desteklemez. Hem ana bilgisayar IP 'si hem de AzS-BGPNAT01 (DHCP veya statik IP tarafından) atanan yeni IP, internet 'e erişebilmelidir. 80 ve 443 bağlantı noktaları graph.windows.net ve login.microsoftonline.com etki alanları altında kullanılır.


## <a name="next-steps"></a>Sonraki adımlar

- [ASDK dağıtım paketini indirin](asdk-download.md).
- Depolama Alanları Doğrudan hakkında daha fazla bilgi edinmek için bkz. [depolama alanları doğrudan genel bakış](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview).

