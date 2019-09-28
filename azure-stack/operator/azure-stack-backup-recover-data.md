---
title: Azure Stack çok önemli veri kaybını kurtar | Microsoft Docs
description: Veri kaybını çok önemli olduktan sonra Azure Stack altyapı verilerinizi kurtarmayı ve geri yüklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: fd7c1ede611f9505ed48c8efc3caa5311c285cd3
ms.sourcegitcommit: c2ea4ffb42563c26faaf2993ba7b484bcb6d5cb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71342857"
---
# <a name="recover-from-catastrophic-data-loss"></a>Geri dönülemez veri kaybından kurtarma

*Uygulama hedefi: Tümleşik sistemler Azure Stack.*

Azure Stack, veri merkezinizde Azure hizmetleri 'ni çalıştırır ve tek bir rafa yüklenmiş dört düğüm kadar küçük bir ortamda çalışabilir. Buna karşılık Azure, birden fazla veri merkezinde 40 ' den fazla bölgede ve her bölgede birden çok bölgede çalışır. Kullanıcı kaynakları birden çok sunucu, raf, veri merkezi ve bölgeye yayılabilir. Azure Stack, şu anda yalnızca tüm bulutunuzu tek bir rafa dağıtma seçeneğiniz vardır. Bu sınırlama, bulutunuzu, veri merkezinizdeki veya önemli ürün hataları nedeniyle oluşan hatalardan kaynaklanan riskli bir şekilde sunar. Olağanüstü bir durum yaşandığında Azure Stack örneği çevrimdışı duruma gelir. Tüm veriler kurtarılamaz olabilir.

Veri kaybının kök nedenine bağlı olarak, tek bir altyapı hizmetini onarmanız veya tüm Azure Stack örneğini geri yüklemeniz gerekebilir. Hatta aynı konumdaki veya farklı bir konumda farklı bir donanıma geri yüklemeniz gerekebilir.

Bu senaryo, bir hata ve özel bulutun yeniden dağıtımı varsa yüklemenizin tamamını kurtarıp ele alınmaktadır.

| Senaryo                                                           | Veri kaybı                            | Dikkat edilmesi gerekenler                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Olağanüstü durum veya ürün hatası nedeniyle çok önemli veri kaybını kurtarın. | Tüm altyapı ve Kullanıcı ve uygulama verileri. | Kullanıcı uygulaması ve verileri altyapı verilerinden ayrı olarak korunur. |

## <a name="workflows"></a>Workflows

Azure başlangıcını korumanın bir yolculuğu, altyapının ve uygulama/kiracı verilerinin ayrı olarak yedeklenmesiyle başlar. Bu belge, altyapının nasıl korunacağını ele alır. 

![Azure Stack veri kurtarma iş akışı — dağıtım](media/azure-stack-backup/azure-stack-backup-workflow1.png)

Tüm verilerin kaybedildiği en kötü durum senaryolarında, Azure Stack kurtarma, bu Azure Stack ve tüm Kullanıcı verilerinin dağıtımına özgü altyapı verilerini geri yükleme işlemidir. 

![Azure Stack veri kurtarma iş akışı — yeniden dağıtım](media/azure-stack-backup/azure-stack-backup-workflow2.png)

## <a name="restore"></a>Geri yükle

Çok önemli veri kaybı varsa, ancak donanım hala kullanılabilir ise Azure Stack yeniden dağıtımı gereklidir. Yeniden dağıtım sırasında, yedeklemelere erişmek için gereken depolama konumunu ve kimlik bilgilerini belirtebilirsiniz. Bu modda, geri yüklenmesi gereken hizmetleri belirtmeniz gerekmez. Infrastructure Backup denetleyicisi, dağıtım iş akışının bir parçası olarak denetim düzlemi durumunu çıkarır.

Donanımı kullanılamaz hale getirecek bir olağanüstü durum varsa, yeniden dağıtım yalnızca yeni donanımda mümkündür. Yeniden dağıtım işlemi birkaç hafta sürebilir, bu da değiştirme donanımı veri merkezinde sıralanmıştır ve ulaşır. Denetim düzlemi verilerinin geri yüklenmesi istediğiniz zaman mümkündür. Ancak, yeniden dağıtılan örneğin sürümü son yedeklemede kullanılan sürümden daha büyük bir sürümsa geri yükleme desteklenmez.

| Dağıtım modu | Başlangıç noktası | Uç nokta                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yüklemeyi temizle   | Ana hat derlemesi | OEM Azure Stack ve güncelleştirmeleri en son desteklenen sürüme dağıtır.                                                                                                                                          |
| Kurtarma modu   | Ana hat derlemesi | OEM, kurtarma modunda Azure Stack dağıtır ve kullanılabilir en son yedeklemeye göre sürüm eşleştirme gereksinimlerini işler. OEM, en son desteklenen sürüme güncelleştirerek dağıtımı tamamlar. |

## <a name="data-in-backups"></a>Yedeklemelerdeki veriler

Azure Stack, bulut kurtarma modu adlı bir dağıtım türünü destekler. Bu mod yalnızca, bir olağanüstü durum veya ürün hatası çözümü kurtarılamaz olduktan sonra Azure Stack kurtarmayı seçtiğinizde kullanılır. Bu dağıtım modu, çözümde depolanan kullanıcı verilerinin hiçbirini kurtarmaz. Bu dağıtım modunun kapsamı, aşağıdaki verileri geri yüklemeyle sınırlıdır:

 - Dağıtım girişleri
 - İç kimlik hizmeti verileri (ADFS dağıtımları).
 - Federasyon tanımlaması yapılandırma (ADFS dağıtımları).
 - İç sertifika yetkilisi tarafından kullanılan kök sertifikalar.
 - Abonelikler, planlar, teklifler, depolama kotaları, ağ kotaları ve işlem kaynakları gibi yapılandırma kullanıcı verilerini Azure Resource Manager.
 - Key Vault gizli dizileri ve kasaları.
 - RBAC ilke atamaları ve rol atamaları.

Hizmet olarak Kullanıcı altyapısının (IaaS) veya hizmet olarak platform (PaaS) kaynaklarının hiçbiri dağıtım sırasında kurtarılır. Bu kayıplar IaaS VM 'Leri, depolama hesapları, Bloblar, tablolar, ağ yapılandırması vb. içerir. Bulut kurtarma amacı, operatörlerinizin ve kullanıcılarınızın dağıtım tamamlandıktan sonra portalda yeniden oturum açabilmesini sağlamaktır. Oturum açan kullanıcılar, kaynaklarından hiçbirini görmez. Kullanıcıların abonelikleri, yönetici tarafından tanımlanan orijinal planlar, teklifler ve ilkelerle birlikte geri yüklenir. Sisteme yeniden oturum açan kullanıcılar, olağanüstü durumdan önce özgün çözüm tarafından uygulanan kısıtlamalar altında çalışır. Bulut kurtarması tamamlandıktan sonra, işleç değer ekleme ve üçüncü taraf RPs 'Leri ve ilişkili verileri el ile geri yükleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Infrastructure Backup hizmetini kullanmaya](azure-stack-backup-best-practices.md)yönelik en iyi yöntemler hakkında bilgi edinin.
