---
title: Infrastructure Backup hizmetini kullanarak Azure Stack çok zararlı veri kaybından kurtarma | Microsoft Docs
description: Çok zararlı bir hata Azure Stack başarısız olmasına neden olduğunda, Azure Stack dağıtımınızı yeniden oluştururken altyapı verilerinizi geri yükleyebilirsiniz.
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
ms.openlocfilehash: b6b6796f5d47189499e01c94b9c988dbf03091bb
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68493991"
---
# <a name="recover-from-catastrophic-data-loss"></a>Geri dönülemez veri kaybından kurtarma

*Uygulama hedefi: Tümleşik sistemler Azure Stack.*

Azure Stack, veri merkezinizde Azure hizmetleri 'ni çalıştırır ve tek bir rafa yüklenmiş dört düğüm kadar küçük bir ortamda çalışabilir. Buna karşılık Azure, birden fazla veri merkezinde 40 ' den fazla bölgede ve her bölgede birden çok bölgede çalışır. Kullanıcı kaynakları birden çok sunucu, raf, veri merkezi ve bölgeye yayılabilir. Azure Stack, şu anda yalnızca tüm bulutunuzu tek bir rafa dağıtma seçeneğiniz vardır. Bu, bulutunuzu, veri merkezinizdeki veya önemli ürün hataları nedeniyle oluşan hatalardan kaynaklanan riskli bir şekilde sunar. Olağanüstü bir durum yaşandığında Azure Stack örneği çevrimdışı duruma gelir. Tüm veriler kurtarılamaz olabilir.

Veri kaybının kök nedenine bağlı olarak, tek bir altyapı hizmetini onarmanız veya tüm Azure Stack örneğini geri yüklemeniz gerekebilir. Hatta aynı konumdaki veya farklı bir konumda farklı bir donanıma geri yüklemeniz gerekebilir.

Bu senaryo donanımda hata olması durumunda ve özel bulutun yeniden dağıtımında yüklemenizin tamamını kurtarmayı kapsar.

| Senaryo                                                           | Veri kaybı                            | Dikkat edilmesi gerekenler                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Olağanüstü durum veya ürün hatası nedeniyle çok önemli veri kaybından kurtarma | Tüm altyapı ve Kullanıcı ve uygulama verileri | Kullanıcı uygulaması ve verileri altyapı verilerinden ayrı olarak korunur |

## <a name="workflows"></a>Workflows

Azure başlangıcını korumanın bir yolculuğu, altyapının ve uygulama/kiracı verilerinin ayrı olarak yedeklenmesiyle başlar. Bu belge, altyapının nasıl korunacağını ele alır. 

![Azure Stack ilk dağıtımı](media/azure-stack-backup/azure-stack-backup-workflow1.png)

Tüm verilerin kaybedildiği en kötü durum senaryolarında, Azure Stack kurtarma, bu Azure Stack ve tüm Kullanıcı verilerinin dağıtımına özgü altyapı verilerini geri yükleme işlemidir. 

![Yeniden dağıtma Azure Stack](media/azure-stack-backup/azure-stack-backup-workflow2.png)

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
 - İç kimlik hizmeti verileri (ADFS dağıtımları)
 - Federasyon tanımlaması yapılandırma (ADFS dağıtımları)
 - İç sertifika yetkilisi tarafından kullanılan kök sertifikalar
 - Depolama, ağ ve işlem kaynakları için abonelikler, planlar, teklifler ve kotalar gibi yapılandırma Kullanıcı verileri Azure Resource Manager
 - Anahtar Kasası gizli dizileri ve kasaları
 - RBAC ilke atamaları ve rol atamaları 

Hizmet olarak Kullanıcı altyapısının (IaaS) veya hizmet olarak platform (PaaS) kaynaklarının hiçbiri dağıtım sırasında kurtarılır. Bu, IaaS VM 'Leri, depolama hesapları, Bloblar, tablolar, ağ yapılandırması vb. için kaybolur. Bulut kurtarma amacı, operatörlerinizin ve kullanıcılarınızın dağıtım tamamlandıktan sonra portala yeniden oturum açabilmesini sağlamaktır. Yeniden oturum açan kullanıcılar, kaynaklarından hiçbirini görmez. Kullanıcılar aboneliklerinin geri yüklendiği ve orijinal planlarla birlikte yönetici tarafından tanımlanan ilkelerle birlikte. Sisteme geri oturum açan kullanıcılar, olağanüstü durumdan önce özgün çözüm tarafından uygulanan kısıtlamalar altında çalışır. Bulut kurtarması tamamlandıktan sonra, işleç değer ekleme ve üçüncü taraf RPs 'Leri ve ilişkili verileri el ile geri yükleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Infrastructure Backup hizmetini kullanmaya](azure-stack-backup-best-practices.md)yönelik en iyi yöntemler hakkında bilgi edinin.
