---
title: Hizmet başvurusunu Infrastructure Backup | Microsoft Docs
description: Azure Stack Infrastructure Backup hizmeti için başvuru malzemeleri.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 282d6f3a501550e49424c257b928e708f63ccadc
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974862"
---
# <a name="infrastructure-backup-service-reference"></a>Infrastructure Backup hizmet başvurusu

## <a name="azure-backup-infrastructure"></a>Azure Backup altyapısı

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, portalı (Azure Resource Manager) ve genel altyapı yönetimi deneyimini oluşturan birçok hizmetten oluşur. Azure Stack uygulama benzeri yönetim deneyimi, çözümün işlecine sunulan karmaşıklığı azaltmaya odaklanır.

Infrastructure Backup hizmet, altyapı hizmetleri için veri yedekleme ve geri yükleme karmaşıklığını internalize üzere tasarlanmıştır. Bu, işleçlerin çözümü yönetmeye ve kullanıcılara bir SLA 'yı korumaya odaklanabilmenizi sağlar.

Yedeklemelerin aynı sistemde depolanmasını önlemek için yedekleme verilerinin bir dış paylaşıma verilmesi gerekir. Dış bir paylaşımın gerekli olması, yöneticinin verileri mevcut şirket BC/DR ilkelerine göre nerede depolayaceğini belirleme esnekliği sağlar.

### <a name="infrastructure-backup-service-components"></a>Infrastructure Backup hizmet bileşenleri

Infrastructure Backup hizmeti aşağıdaki bileşenleri içerir:

 - **Infrastructure Backup denetleyicisi**  
 Infrastructure Backup denetleyicisi, ile birlikte oluşturulur ve her Azure Stack bulutta bulunur.
 - **Yedekleme kaynak sağlayıcısı**  
 Yedekleme kaynak sağlayıcısı (Backup RP), kullanıcı arabiriminden ve API 'lerin Azure Stack altyapısı için temel yedekleme işlevlerini açığa çıkaran API 'lerden oluşur.

#### <a name="infrastructure-backup-controller"></a>Infrastructure Backup denetleyicisi

Infrastructure Backup denetleyicisi, bir Azure Stack bulutu için oluşturulan Service Fabric bir hizmettir. Yedekleme kaynakları bölgesel düzeyde oluşturulur ve AD, CA, Azure Resource Manager, CRP, SRP, NRP, Key Vault, RBAC 'den bölgeye özgü hizmet verilerini yakalar.

### <a name="backup-resource-provider"></a>Yedekleme kaynak sağlayıcısı

Yedekleme kaynak sağlayıcısı, yedekleme kaynaklarını temel yapılandırma ve listeleme için Azure Stack portalında bir kullanıcı arabirimi sunar. İşleçler Kullanıcı arabiriminde aşağıdaki eylemleri gerçekleştirebilir:

 - Dış depolama konumu, kimlik bilgileri ve şifreleme anahtarı sağlayarak yedeklemeyi ilk kez etkinleştirin.
 - Oluşturulan tamamlanan yedekleme kaynaklarını ve durum kaynaklarını görüntüleme altında görüntüleyin.
 - Yedekleme denetleyicisinin yedekleme verilerini yerleştirdiği depolama konumunu değiştirin.
 - Yedekleme denetleyicisinin dış depolama konumuna erişmek için kullandığı kimlik bilgilerini değiştirin.
 - Yedekleme denetleyicisinin yedekleri şifrelemek için kullandığı şifreleme anahtarını değiştirin.


## <a name="backup-controller-requirements"></a>Yedekleme denetleyicisi gereksinimleri

Bu bölümde Infrastructure Backup hizmeti için önemli gereksinimler açıklanmaktadır. Azure Stack örneğiniz için yedeklemeyi etkinleştirmeden önce bilgileri dikkatle incelemenizi ve ardından dağıtım ve sonraki işlemler sırasında gerektiği şekilde geri başvurmalarını öneririz.

Gereksinimler şunlardır:

  - **Yazılım gereksinimleri** -desteklenen depolama konumlarını ve boyutlandırma kılavuzunu açıklar. 
  - **Ağ gereksinimleri** -farklı depolama konumları için ağ gereksinimlerini açıklar.  

### <a name="software-requirements"></a>Yazılım gereksinimleri

#### <a name="supported-storage-locations"></a>Desteklenen depolama konumları

| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazında barındırılan SMB dosya paylaşma. | Azure Stack dağıtıldığı veri merkezinde veya farklı bir veri merkezinde SMB paylaşma. Birden çok Azure Stack örneği aynı dosya paylaşımının kullanımını kullanabilir. |
| Azure 'da SMB dosya paylaşma.                                                          | Şu anda desteklenmiyor.                                                                                                                                 |
| Azure 'da BLOB depolama.                                                            | Şu anda desteklenmiyor.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Desteklenen SMB sürümleri

| SMB | Version |
|-----|---------|
| SMB | 3.x     |

#### <a name="smb-encryption"></a>SMB şifrelemesi

**1907 ve daha fazla**

Infrastructure Backup hizmeti, yedekleme verilerinin sunucu tarafında SMB Şifrelemesi etkin olan bir dış depolama konumuna aktarılmasını destekler. Sunucu, SMB şifrelemesini desteklemiyorsa veya özelliği etkinleştirilmemişse, Infrastructure Backup hizmet şifrelenmemiş veri aktarımına geri döner. Dış depolama konumuna yerleştirilmiş yedekleme verileri her zaman bekleyen olarak şifrelenir ve SMB şifrelemeye bağlı değildir.

#### <a name="storage-location-sizing"></a>Depolama konumu boyutlandırma

Günde son iki kez yedeklemeniz ve en fazla yedi gün tutulması önerilir. Bu, Azure Stack altyapı yedeklemelerini etkinleştirdiğinizde varsayılan davranıştır.

**1907 ve daha fazla**

***Azure AD Identity sağlayıcısına bağlı sistem***

| Ortam ölçeği | Yedeğin tahmini boyutu | Gereken toplam alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-16 düğüm/ASDK   | 1 GB                     | 20 GB                          |

***ADFS aracılığıyla şirket AD kimlik sağlayıcısına bağlı sistem***

| Ortam ölçeği | Yedeğin tahmini boyutu | Gereken toplam alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-16 düğümleri        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

**1907 öncesi**

| Ortam ölçeği | Yedeğin tahmini boyutu | Gereken toplam alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-16 düğümleri        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

### <a name="network-requirements"></a>Ağ gereksinimleri

| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazında barındırılan SMB dosya paylaşma. | Azure Stack örneği güvenlik duvarlı bir ortamda yer alıyorsa, bağlantı noktası 445 gereklidir. Infrastructure Backup denetleyicisi, 445 numaralı bağlantı noktası üzerinden SMB dosya sunucusuyla bağlantı başlatacak. |
| Dosya sunucusu FQDN 'SI kullanmak için adın PEP tarafından çözümlenebilmelidir.             |                                                                                                                                                                                         |

> [!Note]  
> Hiçbir gelen bağlantı noktasının açılması gerekmez.

### <a name="encryption-requirements"></a>Şifreleme gereksinimleri

1901 ' den başlayarak Infrastructure Backup hizmeti, ortak anahtara sahip bir sertifika kullanır (. CER) yedekleme verilerini ve özel anahtarla bir sertifikayı şifrelemek için (. PFX) bulut kurtarması sırasında yedekleme verilerinin şifresini çözün.

 - Sertifika, anahtarların taşınması için kullanılır ve güvenli kimlik doğrulamalı iletişim kurmak için kullanılmaz. Bu nedenle, sertifika otomatik olarak imzalanan bir sertifika olabilir. Azure Stack dış internet erişimi gerekmediği için bu sertifikanın kökünü veya güvenini doğrulaması gerekmez.

Otomatik olarak imzalanan sertifika, biri ortak anahtara ve diğeri de özel anahtara sahip iki bölüme gelir:

 - Yedekleme verilerini şifreleyin: Ortak anahtara sahip sertifika (öğesine aktarma. CER dosyası), yedekleme verilerini şifrelemek için kullanılır.
 - Yedekleme verilerinin şifresini çözün: Özel anahtara sahip sertifika (öğesine aktarma. PFX dosyası), yedekleme verilerinin şifresini çözmek için kullanılır.

Ortak anahtara sahip sertifika (. CER), iç gizli döndürme tarafından yönetilmez. Sertifikayı döndürmek için, otomatik olarak imzalanan yeni bir sertifika oluşturmanız ve yedekleme ayarlarını yeni dosyayla güncelleştirmeniz gerekir (. CER). 
 
 - Mevcut tüm yedeklemeler, önceki ortak anahtar kullanılarak şifrelenmiş olarak kalır. Yeni yedeklemeler yeni ortak anahtarı kullanır.
 
Özel anahtarla bulut kurtarma sırasında kullanılan sertifika (. PFX) güvenlik nedeniyle Azure Stack kalıcı değildir. Bu dosyanın, bulut kurtarma sırasında açıkça sağlanması gerekir.  

**Geriye dönük uyumluluk modu** 1901 ' den başlayarak şifreleme anahtarı desteği kullanım dışıdır ve gelecek sürümde kaldırılacaktır. Bir şifreleme anahtarı kullanılarak 1811 ' den yedekleme zaten etkinleştirilmiş olarak güncelleştirilirse, Azure Stack şifreleme anahtarını kullanmaya devam eder. Geriye doğru uyumluluk modu, en az üç sürüm için desteklenecektir. Bu süreden sonra bir sertifika gerekli olacaktır.

 * Şifreleme anahtarından sertifikaya güncelleştirme tek yönlü bir işlemdir.  
 * Mevcut tüm yedeklemeler şifreleme anahtarı kullanılarak şifreli olarak kalır. Yeni yedeklemeler sertifikayı kullanır. 

## <a name="infrastructure-backup-limits"></a>Infrastructure Backup sınırları

Microsoft Azure Stack örneklerinizi planlarken, dağıtırken ve işletirken bu limitleri göz önünde bulundurun. Aşağıdaki tabloda bu sınırlar açıklanmaktadır.

### <a name="infrastructure-backup-limits"></a>Infrastructure Backup sınırları

| Sınır tanımlayıcı                                                 | Sınır        | Açıklamalar                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme türü                                                      | Yalnızca tam    | Infrastructure Backup denetleyicisi yalnızca tam yedeklemeleri destekler. Artımlı yedeklemeler desteklenmez.                                          |
| Zamanlanan yedeklemeler                                                | Zamanlanmış ve el ile  | Yedekleme denetleyicisi zamanlanmış ve isteğe bağlı yedeklemeleri destekler.                                                                                 |
| Maksimum eşzamanlı yedekleme işi                                   | 1\.            | Yedekleme denetleyicisi örneği başına yalnızca bir etkin yedekleme işi desteklenir.                                                                  |
| Ağ anahtarı yapılandırması                                     | Kapsamda değil | Yöneticinin, OEM araçlarını kullanarak ağ anahtarı yapılandırmasını yedeklemesi gerekir. Her OEM satıcısı tarafından verilen Azure Stack için belgelere bakın. |
| Donanım yaşam döngüsü Konağı                                          | Kapsamda değil | Yöneticinin, OEM araçlarını kullanarak donanım yaşam döngüsü konağını yedeklemesi gerekir. Her OEM satıcısı tarafından verilen Azure Stack için belgelere bakın.      |
| En fazla dosya paylaşımı sayısı                                    | 1\.            | Yedekleme verilerini depolamak için yalnızca bir dosya paylaşma kullanılabilir.                                                                                        |
| App Services, Function, SQL, MySQL kaynak sağlayıcısı verilerini yedekleme | Kapsamda değil | Microsoft tarafından oluşturulan RPs 'lerin dağıtımı ve yönetimi için yayımlanan kılavuza bakın.                                                  |
| Üçüncü taraf kaynak sağlayıcılarını yedekleme                              | Kapsamda değil | Üçüncü taraf satıcılar tarafından oluşturulan RPs değerlerini dağıtmak ve yönetmek için yayımlanan kılavuza bakın.                                          |

## <a name="next-steps"></a>Sonraki adımlar

 - Infrastructure Backup hizmeti hakkında daha fazla bilgi edinmek için bkz. [Infrastructure Backup hizmeti ile Azure Stack Için Yedekleme ve veri kurtarma](azure-stack-backup-infrastructure-backup.md).
