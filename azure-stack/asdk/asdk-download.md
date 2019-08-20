---
title: ASDK 'yi indir ve Ayıkla | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) indirme ve ayıklama hakkında bilgi edinin.
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
ms.date: 05/06/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 08/10/2018
ms.openlocfilehash: a4d0258d2a7a5168c94159d9eae13605b1269b86
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579132"
---
# <a name="download-and-extract-the-asdk"></a>ASDK 'yi indir ve Ayıkla
Geliştirme seti ana bilgisayarınızın Azure Stack Geliştirme Seti (ASDK) yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra, bir sonraki adım Cloudbuilder. vhdx 'i almak için ASDK dağıtım paketini indirip ayıklamaya yöneliktir.

## <a name="download-the-asdk"></a>ASDK 'yi indirin
1. İndirmeyi başlamadan önce, bilgisayarınızın aşağıdaki önkoşulları karşıladığından emin olun:

   - Bilgisayarınızda, işletim sistemi diskine ek olarak dört ayrı ve özdeş mantıksal sabit sürücüde en az 60 GB boş disk alanı bulunmalıdır.
   - [.NET Framework 4,6 (veya sonraki bir sürüm)](https://dotnet.microsoft.com/download/dotnet-framework-runtime/net46) yüklü olmalıdır.

2. ASDK 'yi indirebileceğiniz [Başlarken sayfasına gidin](https://azure.microsoft.com/overview/azure-stack/try/?v=try) , ayrıntılarınızı sağlayın ve ardından **Gönder**' e tıklayın.
3. ASDK önkoşul denetleyicisi betiği [Için dağıtım denetleyicisi](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) ' ni indirip çalıştırın. Bu tek başına betiği, ASDK kurulumu tarafından gerçekleştirilen önkoşul denetimlerinden geçer. Bu, ASDK için daha büyük paketi indirmeden önce donanım ve yazılım gereksinimlerini karşıladığınızı doğrulamak için bir yol sağlar.
4. **Yazılımı İndir**altında **Azure Stack geliştirme seti**' ye tıklayın.

   > [!NOTE]
   > ASDK indirmesi (AzureStackDevelopmentKit. exe) yaklaşık 10 GB 'dir.

## <a name="extract-the-asdk"></a>ASDK 'yi Ayıkla
1. İndirme tamamlandıktan sonra, ASDK Self-Extractor (AzureStackDevelopmentKit. exe) başlatmak için **Çalıştır** ' a tıklayın.
2. Self-Extractor sihirbazının **Lisans Sözleşmesi** sayfasında, görüntülenmiş lisans sözleşmesini gözden geçirin ve kabul edin ve ardından **İleri**' ye tıklayın.
3. Self-Extractor sihirbazının **önemli duyuru** sayfasında görünen gizlilik bildirimi bilgilerini gözden geçirin ve ardından **İleri**' ye tıklayın.
4. Self-Extractor sihirbazının **hedef konumunu seçin** sayfasında Ayıklanacak Azure Stack kurulum dosyalarının konumunu seçin ve ardından **İleri**' ye tıklayın. Varsayılan konum *geçerli klasör*\Azure Stack geliştirme seti ' dir. 
5. Self-Extractor sihirbazının **ayıklanmaya hazırlanma** sayfasında hedef konum özetini gözden geçirin ve ardından cloudbuilder. vhdx (yaklaşık 28gb) ve üçüncü taraf lisansları. rtf dosyalarını ayıklamak için **Ayıkla** ' ya tıklayın. Bu işlemin tamamlanabilmesi biraz zaman alır.
6. CloudBuilder. vhdx dosyasını kopyalayın veya C:\ ' un köküne taşıyın. sürücüsünü (`C:\CloudBuilder.vhdx`), asdk ana bilgisayarında.

> [!NOTE]
> Dosyaları ayıkladıktan sonra, öğesini silebilirsiniz. EXE ve. Sabit disk alanını kurtarmak için BIN dosyaları. Ya da, ASDK 'yi yeniden dağıtmanız gerekiyorsa dosyaları indirmeniz gerekmiyorsa bu dosyaları yedekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
[ASDK ana bilgisayarını hazırlama](asdk-prepare-host.md)