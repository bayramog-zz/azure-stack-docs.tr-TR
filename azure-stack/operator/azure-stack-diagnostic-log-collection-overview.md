---
title: Azure Stack tanılama günlüğü koleksiyonuna genel bakış | Microsoft Docs
description: İsteğe bağlı ve otomatik günlük toplama dahil Azure Stack yardım + destek ' te tanılama günlüğü toplamayı açıklar.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/24/2019
ms.openlocfilehash: 5409cf49f298d18680a0fb9428c71552dad54ff9
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497095"
---
# <a name="overview-of-azure-stack-diagnostic-log-collection"></a>Azure Stack tanılama günlüğü koleksiyonuna genel bakış 

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack, birlikte çalışan ve birbirleriyle etkileşim kuran büyük bir bileşen koleksiyonudur. Tüm bu bileşenler kendi benzersiz günlüklerini oluşturur. Bu durum, özellikle birden çok, etkileşim Azure Stack bileşenlerinden gelen hatalar için, özellikle de zorlayıcı bir görev için sorunları tanılamayı kolaylaştırabilir. Bu zorluğu ele almak için bir tanılama günlüğü toplama deneyimi tasarlıyoruz. 

1907 ' den önce, tanılama deneyimi, sistem durumunu doğrulamak için [Test-AzureStack](azure-stack-diagnostic-test.md) kullanılarak ve sorun giderme amacıyla günlükleri toplamak için [ayrıcalıklı uç nokta (Pep)](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep) kullanılarak dahil edilmiştir. 

1907 sürümünden itibaren, **Yardım ve destek** sayfası **tanılama günlüğü toplamayı**kullanarak daha basit bir deneyim ekler. 
**Tanılama günlüğü koleksiyonu** , sorun giderme işlemiyle Azure Stack işlecinin deneyimini geliştirmek için devam eden bir yatırımın bir parçasıdır. Bu geliştirmelerle, operatörler Microsoft Müşteri Destek Hizmetleri (CSS) ile tanılama günlüklerini hızlıca toplayıp paylaşabilir. Günlükler, erişimin gerektiği şekilde özelleştirilebileceği Azure 'da bir blob kapsayıcısında depolanabilir.    
   
**Tanılama günlüğü toplama** , tanılama günlüklerini iki farklı şekilde toplayabilir:

- **Otomatik koleksiyon**: Etkinleştirilirse (önerilir), günlük koleksiyonu belirli sistem durumu uyarıları tarafından otomatik olarak tetiklenir ve Azure depolama hesabınızda depolanır
- **Günlükleri Şimdi topla**: Bu, son yedi gündeki 1-4 saatlik bir kayan pencereden günlükleri toplamayı seçebileceğiniz isteğe bağlı bir seçenektir

![Tanılama günlüğü toplama seçeneklerinin ekran görüntüsü](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

**Tanılama günlüğü koleksiyonu** kolay bir kullanıcı arabirimine sahiptir ve PowerShell gerektirmez. Altyapı Hizmetleri çalışmıyor olsa bile Günlükler güvenilir bir şekilde toplanır.
İlkeniz tanılama günlüklerinin CSS ile paylaşılmasına izin veriyorsa, **tanılama günlüğü koleksiyonu** 1907 sürümünden itibaren önerilen koleksiyon yöntemidir. Yardım ve destek ' de **tanılama günlüğü koleksiyonu** kullanılamıyorsa günlükleri toplamak için yalnızca [Pep](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep) 'yi kullanmanız gerekir.

## <a name="automatic-diagnostic-log-collection"></a>Otomatik tanılama günlüğü koleksiyonu 

Otomatik tanılama günlüğü koleksiyonu, bazı kritik uyarılar ortaya çıktığında, tanılama günlüklerini CSS ile paylaşmak için gereken süreyi önemli ölçüde azaltarak Azure 'daki bir depolama blobuna Azure Stack tanılama günlüklerini etkin bir şekilde yükler.

Otomatik günlük toplama hakkında daha fazla bilgi için bkz. [otomatik Azure Stack tanılama günlüğü toplamayı yapılandırma](azure-stack-configure-automatic-diagnostic-log-collection.md).

## <a name="on-demand-diagnostic-log-collection"></a>İsteğe bağlı tanılama günlüğü koleksiyonu

İsteğe bağlı koleksiyon ile, Azure Stack operatörü koleksiyonu el ile tetiklediğinde, tanılama günlükleri Azure 'daki bir depolama blobuna Azure Stack yüklenir.
CSS, CSS 'ye ait bir depolama blobuna paylaşılan erişim imzası (SAS) URL 'SI sağlar. Azure Stack işleci **günlükleri Şimdi topla** ' ya tıklayıp SAS URL 'sini girebilirler. Tanılama günlükleri, bir ara paylaşıma gerek olmadan doğrudan CSS blobuna karşıya yüklenir. 

Günlükleri isteğe bağlı toplama hakkında daha fazla bilgi için bkz. [Azure Stack tanılama günlüklerini Şimdi topla](azure-stack-configure-on-demand-diagnostic-log-collection.md).

## <a name="bandwidth-considerations"></a>Bant genişliği konuları

Tanılama günlük toplamanın ortalama boyutu, isteğe bağlı veya otomatik çalışmasına göre farklılık gösterir. Otomatik günlük toplama için Ortalama Boyut 2 GB 'tır, ancak isteğe bağlı günlük toplama boyutu, toplanan saat sayısına bağlıdır. 

Aşağıdaki tabloda, Azure ile sınırlı veya tarifeli bağlantıları olan ortamlara yönelik konular listelenmektedir.

| Ağ bağlantısı | Etkisi |
|--------------------|--------|
| Düşük-bant genişliği/yüksek gecikmeli bağlantı | Günlük yüklemesinin tamamlanması uzun zaman alır | 
| Paylaşılan bağlantı | Karşıya yükleme, ağ bağlantısını paylaşan diğer uygulamaları/kullanıcıları da etkileyebilir |
| Tarifeli bağlantı | Ek ağ kullanımı için ISS 'nizden ek ücret ödemeniz gerekebilir |

Daha fazla bilgi için bkz. [otomatik Azure Stack günlük toplama Için en iyi uygulamalar](azure-stack-best-practices-automatic-diagnostic-log-collection.md).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Stack günlüğü ve müşteri verileri işleme](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

[Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[Otomatik Azure Stack günlük toplama Için en iyi uygulamalar](azure-stack-best-practices-automatic-diagnostic-log-collection.md).
