---
title: Azure Stack depolamaya giriş | Microsoft Docs
description: Azure Stack depolama hizmetleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: 092aba28-04bc-44c0-90e1-e79d82f4ff42
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mabrigg
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: c8828076c74e5f12c742206e3c38c8ee44f90b4c
ms.sourcegitcommit: 72d45bb935db0db172d4d7c37d8e48e79e25af64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68376867"
---
# <a name="introduction-to-azure-stack-storage"></a>Azure Stack depolama 'ya giriş

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="overview"></a>Genel Bakış

Azure Stack depolama, Azure depolama tarafından sunulan hizmetlerle tutarlı bir bulut depolama hizmetleri kümesidir. Bu hizmetler Bloblar, tablolar ve kuyruklar içerir.

## <a name="azure-stack-storage-services"></a>Azure Stack depolama hizmetleri

Azure Stack depolama aşağıdaki üç hizmeti sağlar:

- **Blob depolama**

    Blob Storage yapılandırılmamış nesne verilerini depolar. Blob, bir belge, medya dosyası veya uygulama yükleyicisi gibi herhangi bir tür metin veya ikili veri olabilir.

- **Tablo depolama**

    Tablo depolama, yapılandırılmış veri kümelerini depolar. Tablo depolama, çok büyük miktarlarda veriye hızlı geliştirme ve hızlı erişim sağlayan bir NoSQL anahtar özniteliği veri deposudur.

- **Kuyruk depolama**

    Kuyruk depolama, iş akışı işleme ve bulut Hizmetleri bileşenleri arasında iletişim için güvenilir mesajlaşma sağlar.

Azure Stack depolama hesabı, Azure Stack depolama alanındaki hizmetlere erişmenizi sağlayan güvenli bir hesaptır. Depolama hesabınız depolama kaynaklarınız için benzersiz ad alanı sağlar. Aşağıdaki diyagramda bir depolama hesabındaki Azure Stack depolama kaynakları arasındaki ilişkiler gösterilmektedir:

![Azure Stack depolamaya genel bakış](media/azure-stack-storage-overview/AzureStackStorageOverview.png)

### <a name="blob-storage"></a>Blob depolama

Bulutta depolanacak büyük miktarda yapılandırılmamış nesne verisi olan kullanıcılar için, blob Storage etkili ve ölçeklenebilir bir çözüm sunar. Blob depolamayı kullanarak içeriği depolamak için:

- Belgeler
- Fotoğraf, video, müzik ve blog gibi sosyal veriler
- Dosyaların, bilgisayarların, veritabanlarının ve cihazların yedekleri
- Web Apps için görüntüler ve metinler
- Bulut uygulamaları için yapılandırma verileri
- Günlükler ve diğer büyük veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabı herhangi bir sayıda kapsayıcı içerebilir ve bir kapsayıcı, herhangi bir sayıda blob (depolama hesabı sınırına kadar) içerebilir.

BLOB depolama, üç tür blob sunar:

- **Blok Blobları**

    Blok Blobları, bulut nesnelerini akışa alma ve depolama için iyileştirilmiştir. Belgeler, medya dosyaları, yedeklemeler ve diğer benzer dosyaları depolamak için iyi bir seçimdir.

- **Blob ekleme**

    Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir.

- **Sayfa Blobları**

    Sayfa Blobları IaaS disklerini temsil etmek için optimize edilmiştir ve rastgele yazma işlemlerini 1 TB 'a kadar destekler. IaaS diskine bağlı Azure Stack sanal makine, Sayfa Blobu olarak depolanan bir VHD 'dir.

### <a name="table-storage"></a>Tablo depolama alanı

Modern uygulamalar genellikle daha fazla ölçeklenebilirlik ve esneklik sağlayan veri depolarını daha önceki nesil yazılıma göre talep altına kaydeder. Tablo depolaması, uygulamanızın kullanıcı taleplerini karşılamak üzere otomatik olarak ölçeklenebilmesi için yüksek oranda kullanılabilir ve yüksek düzeyde ölçeklenebilir depolama alanı sunar. Tablo depolama, Microsoft 'un NoSQL anahtar/öznitelik deposudur ve geleneksel ilişkisel veritabanlarından farklı hale getirerek şemaya daha az bir tasarıma sahiptir. Daha az bir veri deposuyla, uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo depolamanın kullanımı kolaydır, böylece geliştiriciler uygulamaları hızla oluşturabilir.

Tablo depolama bir anahtar öznitelik deposudur; bu, bir tablodaki her değerin türü belirtilmiş bir özellik adıyla depolandığı anlamına gelir. Özellik adı, filtre uygulamak ve seçim ölçütlerini belirtmek için kullanılır. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Tablo Depolaması şemayana olduğundan, aynı tablodaki iki varlık farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türlerde olabilir.

Web uygulamaları için Kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetinizin gerektirdiği diğer meta veri türleri gibi esnek veri kümelerini depolamak için tablo depolama alanını kullanabilirsiniz. Bugünün internet tabanlı uygulamaları için tablo depolaması gibi NoSQL veritabanları geleneksel ilişkisel veritabanlarına popüler bir alternatif sağlar.

Bir depolama hesabı herhangi bir sayıda tablo içerebilir ve bir tablo, depolama hesabının kapasite sınırına kadar herhangi bir sayıda varlık içerebilir.

### <a name="queue-storage"></a>Kuyruk depolama

Uygulamaları ölçeklendirmek için tasarlarken, uygulama bileşenleri genellikle birbirinden bağımsız olarak ölçeklendirilebilecek şekilde ayrılır. Kuyruk depolama, bulutta, masaüstünde, şirket içi sunucuda veya mobil cihazlarda çalışıp çalışmadığını uygulama bileşenleri arasında zaman uyumsuz iletişim için güvenilir bir mesajlaşma çözümü sağlar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Bir depolama hesabı herhangi bir sayıda sıra içerebilir ve bir sıra, herhangi bir sayıda ileti içerebilir (depolama hesabının kapasite sınırına kadar). Tek bir ileti boyut olarak en fazla 64 KB olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ile tutarlı depolama: farklar ve konular](azure-stack-acs-differences.md)

- Azure depolama hakkında daha fazla bilgi edinmek için bkz. [tanıtım Microsoft Azure depolama](/azure/storage/common/storage-introduction)
