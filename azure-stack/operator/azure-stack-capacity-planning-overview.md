---
title: Kapasite planlama Azure Stack için genel bakış | Microsoft Docs
description: Kapasite planlaması için Azure Stack dağıtımları hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: prchint
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 0d2610d2082973f4ab255027cc299b5858660c58
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66460997"
---
# <a name="overview-of-azure-stack-capacity-planning"></a>Azure Stack kapasite planlaması genel bakış

Azure Stack çözümünü değerlendirirken, genel ve Azure Stack bulut kapasite üzerinde doğrudan etkisi donanım yapılandırma seçeneğiniz vardır. Bunlar, CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm ölçek veya sunucu sayısını Klasik seçimlerdir. Geleneksel bir sanallaştırma çözümü, kullanılabilir kapasitesini belirlemek için bu bileşenlerin basit aritmetik geçerli değildir. Azure Stack altyapısını veya yönetim bileşenleri çözüm içinde barındırmak için geliştirilmiştir ilk neden olmasıdır. Çözümün kapasite bazıları ayrılır, dayanıklılık, hizmet kesintisi durumunda, Kiracı iş yüklerini en aza indirmek için bir şekilde çözümün yazılım güncelleştirme desteklemek üzere ikinci nedenidir. 

> [!IMPORTANT]
> Bu kapasite planlama bilgileri ve [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) Azure Stack planlama ve yapılandırma kararları için bir başlangıç noktasıdır. Araştırma ve analiz için bir alternatif olarak hizmet vermek için tasarlanmamıştır. Microsoft, hiçbir taahhütte veya garantide, açık veya zımni burada verilen bilgilere göre yapar.
 
Azure Stack çözümünü hiper yakınsanmış küme hesaplama ve depolama olarak oluşturulmuştur. Convergence donanım kapasitesi olarak adlandırılan küme içinde paylaşmak için sağlayan bir *ölçek birimi*. Azure Stack'te bir ölçek birimi, kullanılabilirlik ve ölçeklenebilirlik kaynakları sağlar. Bir ölçek birimi olarak adlandırılır, Azure Stack sunucular kümesi oluşur *konakları*. Altyapı yazılım, VM'ler bir dizi içinde barındırıldığı ve Kiracı Vm'leri olarak aynı fiziksel sunucuların paylaşır. Tüm Azure Stack Vm'leri, sonra ölçek biriminin Windows Server küme teknolojileri ve bağımsız Hyper-V örnekleri tarafından yönetilir. Ölçek birimi, alma ve Azure Stack Yönetimi basitleştirir. Ölçek birimi, Azure Stack arasında taşımayı ve ölçeklenebilirlik (Kiracı ve altyapı) tüm hizmetleri için de sağlar. 

Aşağıdaki konular, her bileşen hakkında daha fazla ayrıntı sağlar:

- [Azure Stack işlem](azure-stack-capacity-planning-compute.md)
- [Azure Stack depolama](azure-stack-capacity-planning-storage.md)
- [Azure Stack kapasite Planlayıcısı](azure-stack-capacity-planner.md)
