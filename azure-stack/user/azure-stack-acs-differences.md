---
title: Azure Stack depolama farklılıkları ve konuları | Microsoft Docs
description: Azure Stack depolama ile Azure depolama arasındaki farkları Azure Stack dağıtım konuları hakkında anlayın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/2/2019
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.lastreviewed: 01/30/2019
ms.openlocfilehash: e2680a91aa2b9232eb86de4338d1198fb515e6d3
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824717"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack depolama alanı: Farklılıklar ve dikkat edilmesi gerekenler

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack depolama, Microsoft Azure Stack içindeki depolama bulut Hizmetleri kümesidir. Azure Stack depolama, Azure ile tutarlı semantiklerle blob, tablo, kuyruk ve hesap yönetimi işlevleri sağlar.

Bu makalede, Azure depolama hizmetlerinden bilinen Azure Stack depolama farklılıkları özetlenmektedir. Ayrıca, Azure Stack dağıtırken göz önünde bulundurmanız gereken şeyleri de listeler. Küresel Azure ve Azure Stack arasındaki üst düzey farklılıklar hakkında bilgi edinmek için bkz. [temel konular](azure-stack-considerations.md) makalesi.

## <a name="cheat-sheet-storage-differences"></a>Tek sayfa: Depolama farklılıkları

| Özellik | Azure (genel) | Azure Stack |
| --- | --- | --- |
|Dosya depolama|Bulut tabanlı SMB dosya paylaşımları destekleniyor|Henüz desteklenmiyor
|Bekleyen veriler için Azure depolama hizmeti şifrelemesi|256 bit AES şifrelemesi. Key Vault 'de müşteri tarafından yönetilen anahtarları kullanarak şifrelemeyi destekler.|BitLocker 128 bit AES şifrelemesi. Müşteri tarafından yönetilen anahtarları kullanarak şifreleme desteklenmez.
|Depolama hesabı türü|Genel amaçlı v1, v2 ve BLOB depolama hesapları|Yalnızca genel amaçlı v1.
|Çoğaltma seçenekleri|Yerel olarak yedekli depolama, coğrafi olarak yedekli depolama, Okuma Erişimli Coğrafi olarak yedekli depolama ve bölge yedekli depolama|Yerel olarak yedekli depolama.
|Premium depolama|Yüksek performans ve düşük gecikmeli depolama sağlayın. Yalnızca Premium Depolama hesaplarında sayfa bloblarını destekler.|Sağlanabilir, ancak performans sınırı veya garanti yoktur. Premium Depolama hesaplarında blok Blobları, ekleme Blobları, tablolar ve kuyruklar kullanılarak engellenmez.
|Yönetilen diskler|Premium ve standart destekleniyor|Sürüm 1808 veya sonraki bir sürümü kullandığınızda desteklenir.
|Blob adı|1\.024 karakter (2.048 bayt)|880 karakter (1.760 bayt)
|Blok Blobu en büyük boyutu|4,75 TB (100 MB X 50.000 blok)|1802 Update veya daha yeni bir sürüm için 4,75 TB (100 MB x 50.000 blok). önceki sürümler için 50.000 X 4 MB (yaklaşık 195 GB).
|Sayfa Blobu anlık görüntü kopyalama|Desteklenen çalışan bir VM 'ye bağlı Azure yönetilmeyen VM disklerini yedekleme|Henüz desteklenmiyor.
|Sayfa Blobu Artımlı anlık görüntü kopyası|Premium ve standart Azure sayfa Blobları destekleniyor|Henüz desteklenmiyor.
|Sayfa Blobu faturalandırma|Benzersiz sayfalar için, bloblarda veya anlık görüntüde olup olmadıkları için ücretler tahakkuk edilir. Temel blob güncelleştirilene kadar bir blob ile ilişkili anlık görüntüler için ek ücret uygulanmaz.|Temel blob ve asımış anlık görüntüler için ücretler tahakkuk edilir. Her bir anlık görüntü için ek ücret uygulanır.
|BLOB depolama için depolama katmanları|Sık erişimli, seyrek erişimli ve arşiv depolama katmanları.|Henüz desteklenmiyor.
|BLOB depolama için geçici silme|Genel kullanıma sunuldu|Henüz desteklenmiyor.
|Sayfa Blobu en büyük boyutu|8 TB|1 TB
|Sayfa Blobu sayfa boyutu|512 bayt|4 KB
|Tablo bölüm anahtarı ve satır anahtarı boyutu|1\.024 karakter (2.048 bayt)|400 karakter (800 bayt)
|Blob anlık görüntüsü|Bir Blobun en fazla anlık görüntü sayısı sınırlı değildir.|Bir Blobun en fazla anlık görüntü sayısı 1.000 ' dir.
|Depolama için Azure AD kimlik doğrulaması|Önizleme aşamasında|Henüz desteklenmiyor.
|Sabit blob 'Lar|Genel kullanıma sunuldu|Henüz desteklenmiyor.
|Depolama için güvenlik duvarı ve sanal ağ kuralları|Genel kullanıma sunuldu|Henüz desteklenmiyor.|

Depolama ölçümleriyle de farklılıklar vardır:

* Depolama ölçümlerinde işlem verileri, iç veya dış ağ bant genişliğini ayırt etmez.
* Depolama ölçümlerinde işlem verileri, bağlı disklere sanal makine erişimi içermez.

## <a name="api-version"></a>API sürümü

Aşağıdaki sürümler Azure Stack depolama ile desteklenir:

Azure Depolama Hizmetleri API 'Leri:

1811 güncelleştirmesi veya daha yeni sürümler:

- [2017-11-09](https://docs.microsoft.com/rest/api/storageservices/version-2017-11-09)
- [2017-07-29](https://docs.microsoft.com/rest/api/storageservices/version-2017-07-29)
- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Önceki sürümler:

- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Azure Depolama Hizmetleri yönetim API 'Leri:

1811 güncelleştirmesi veya daha yeni sürümler:

- [2017-10-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2017-06-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-12-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-05-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-Önizleme](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Önceki sürümler:

- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-05-01-Önizleme](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Desteklenen Azure Stack depolama istemci kitaplıkları hakkında daha fazla bilgi için bkz.: [Azure Stack Storage geliştirme araçları 'nı kullanmaya](azure-stack-storage-dev.md)başlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack Storage geliştirme araçları 'nı kullanmaya başlama](azure-stack-storage-dev.md)
* [Azure Stack depolama için veri aktarımı araçlarını kullanma](azure-stack-storage-transfer.md)
* [Azure Stack depolama 'ya giriş](azure-stack-storage-overview.md)
