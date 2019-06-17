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
ms.openlocfilehash: 335e92fcdb2eadd8bebfbfd3fb2e3b31edd00734
ms.sourcegitcommit: b79a6ec12641d258b9f199da0a35365898ae55ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131335"
---
# <a name="overview-of-azure-stack-capacity-planning"></a>Azure Stack kapasite planlaması genel bakış

Azure Stack çözümünü değerlendirirken, genel ve Azure Stack bulut kapasite üzerinde doğrudan etkisi donanım yapılandırma seçeneklerini göz önünde bulundurun. 

Örneğin, CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm ölçek veya sunucu sayısı ile ilgili seçimler gerekir. Geleneksel bir sanallaştırma çözümü, kullanılabilir kapasitesini belirlemek için bu bileşenlerin basit aritmetik geçerli değildir. Azure Stack altyapısını veya yönetim bileşenleri çözüm içinde barındırmak için yerleşik olarak bulunur. Ayrıca, çözümün kapasite bazıları ayrılır dayanıklılık, hizmet kesintisi durumunda, Kiracı iş yüklerini en aza indirmek için bir şekilde çözümün yazılım güncelleştirme desteklemek için. 

> [!IMPORTANT]
> Bu kapasite planlama bilgileri ve [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) Azure Stack planlama ve yapılandırma kararları için bir başlangıç noktasıdır. Bu bilgileri, araştırma ve analiz için bir alternatif olarak hizmet vermek için tasarlanmamıştır. Microsoft, hiçbir taahhütte veya garantide, açık veya zımni burada verilen bilgilere göre yapar.
 
Azure Stack çözüm hesaplama ve depolama hiper yakınsanmış bir küme oluşturulmuştur. Convergence donanım kapasitesi olarak adlandırılan küme içinde paylaşmak için sağlayan bir *ölçek birimi*. Azure Stack'te bir ölçek birimi, kullanılabilirlik ve ölçeklenebilirlik kaynakları sağlar. Bir ölçek birimi olarak adlandırılır, Azure Stack sunucular kümesi oluşur *konakları*. Altyapı yazılımı, sanal makineleri (VM'ler) bir dizi içinde barındırılan ve Kiracı Vm'leri olarak aynı fiziksel sunucuların paylaşır. Tüm Azure Stack Vm'leri, sonra ölçek biriminin Windows Server küme teknolojileri ve bağımsız Hyper-V örnekleri tarafından yönetilir. 

Ölçek birimi, alma ve Azure Stack Yönetimi basitleştirir. Ölçek birimi, Azure Stack arasında taşımayı ve ölçeklenebilirlik (Kiracı ve altyapı) tüm hizmetleri için de sağlar. 

Aşağıdaki konular, her bileşen hakkında daha fazla ayrıntı sağlar:

- [Azure Stack işlem](azure-stack-capacity-planning-compute.md)
- [Azure Stack depolama](azure-stack-capacity-planning-storage.md)
- [Azure Stack kapasite Planlayıcısı](azure-stack-capacity-planner.md)
