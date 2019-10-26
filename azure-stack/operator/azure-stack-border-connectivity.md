---
title: Azure Stack tümleşik sistemler için sınır bağlantısı ve Ağ tümleştirmesi | Microsoft Docs
description: Azure Stack tümleşik sistemlerde veri merkezi sınır ağ bağlantısını nasıl planlayacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: d6e4aa25b558d12cfb437155c453d3d093ea7e00
ms.sourcegitcommit: ef7efcde76d1d7875ca1c882afebfd6a27f1c686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888056"
---
# <a name="border-connectivity"></a>Sınır bağlantısı 
Ağ tümleştirme planlaması, başarılı Azure Stack tümleşik sistemler dağıtımı, işlemi ve yönetimi için önemli bir önkoşuldur. Sınır bağlantı planlaması, Sınır Ağ Geçidi Protokolü (BGP) ile dinamik yönlendirmeyi kullanmak istiyorsanız öğesini seçerek başlar. Bu, bir 16 bit BGP otonom sistem numarası (genel veya özel) atanmasını veya statik bir varsayılan yolun kenarlık cihazlarına atandığı statik yönlendirmeyi kullanmayı gerektirir.

> [!IMPORTANT]
> Raf üstü (TOR) anahtarları, fiziksel arabirimlerde yapılandırılmış noktadan noktaya IP 'Ler (/30 ağ) ile katman 3 yukarı bağlantılar gerektirir. Azure Stack işlemlerini destekleyen TOR anahtarlarıyla katman 2 yukarı bağlantılar desteklenmez.

## <a name="bgp-routing"></a>BGP yönlendirme
BGP gibi dinamik bir yönlendirme protokolü kullanmak sisteminizin ağ değişikliklerinin her zaman farkında olmasını sağlar ve yönetimini kolaylaştırır. Gelişmiş güvenlik için, TOR ve kenarlık arasındaki BGP eşlemesi üzerinde bir parola ayarlanabilir.

Aşağıdaki diyagramda gösterildiği gibi, TOR anahtarındaki özel IP alanının tanıtılması bir ön ek listesi kullanılarak engellenir. Ön ek listesi, özel ağın tanıtımını reddeder ve bu, TOR ile kenarlık arasındaki bağlantıda bir yol haritası olarak uygulanır.

Azure Stack çözümünün içinde çalışan yazılım Load Balancer (SLB), sanal cihaz adreslerini dinamik bir şekilde duyurabilmesi için TOR cihazlarına eşler.

Kullanıcı trafiğinin hatadan hemen ve şeffaf bir şekilde kurtarılmasından emin olmak için, TOR cihazları arasında yapılandırılan VPC veya MLAG 'ler, IP ağları için ağ artıklığı sağlayan ana bilgisayarlar ve HSRP veya VRRP 'ye çok kasalı bağlantı toplama kullanılmasına izin verir.

![BGP yönlendirme](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>Statik yönlendirme
Statik yönlendirme, sınır cihazlara ek yapılandırma gerektirir. Bu, herhangi bir değişiklikten önce daha el ile müdahale ve yönetimin yanı sıra kapsamlı analizler de gerektirir. Yapılandırma hatasından kaynaklanan sorunların, yapılan değişikliklere bağlı olarak geri alınması daha uzun sürebilir. Bu yönlendirme yöntemi önerilmez, ancak desteklenir.

Statik yönlendirmeyi kullanarak ağ ortamınıza Azure Stack bütünleştirmek için, sınır ve TOR cihazı arasındaki dört fiziksel bağlantı bağlanmalıdır. Statik yönlendirmenin nasıl çalıştığı ile yüksek kullanılabilirlik garanti edilemez.

Sınır cihazı, ' ın Azure Stack içindeki herhangi bir ağa giden trafik için Ise ve sınır kenarlığı arasındaki her birine işaret eden statik yollarla yapılandırılmalıdır, ancak işlem için yalnızca *dış* veya genel VIP ağı gereklidir. İlk dağıtım için *BMC* 'ye ve *dış* ağlara yönelik statik yollar gereklidir. İşleçler, *BMC* ve *altyapı* ağı üzerinde bulunan yönetim kaynaklarına erişmek için, kenarlıkta statik yollardan ayrılmamaya izin verebilir. *Anahtar altyapısına* ve *anahtar yönetim* ağlarına statik yollar eklemek isteğe bağlıdır.

TOR cihazları, tüm trafiği kenarlık cihazlarına gönderen statik bir varsayılan yol ile yapılandırılır. Varsayılan kural için tek trafik özel durumu özel alan içindir ve bu, TOR üzerinde bağlantı için uygulanan bir Access Control listesi kullanılarak engellenir.

Statik yönlendirme yalnızca TOR ve Border anahtarları arasındaki yukarı bağlantılar için geçerlidir. BGP dinamik yönlendirmesi, SLB ve diğer bileşenler için gerekli bir araç olduğundan ve devre dışı bırakılacağından veya kaldırılacağından raf içinde kullanılır.

![Statik yönlendirme](media/azure-stack-border-connectivity/static-routing.png)

<sup>\*</sup> BMC ağı dağıtımdan sonra isteğe bağlıdır.

<sup>\*\*</sup> Anahtar altyapısı ağı, tüm ağ, anahtar yönetimi ağına dahil edilebilir.

<sup>\*\*\*</sup> Anahtar yönetimi ağı gereklidir ve anahtar altyapısı ağından ayrı olarak eklenebilir.

## <a name="transparent-proxy"></a>Saydam proxy
Veri merkeziniz tüm trafiğin bir ara sunucu kullanmasını gerektiriyorsa, bir *saydam proxy* 'yi, tüm trafiği ilkeye göre işleyecek şekilde işleyecek şekilde yapılandırmanız ve ağınızdaki bölgeler arasında trafiği ayırmaktır.

> [!IMPORTANT]
> Azure Stack çözümü normal web proxy 'lerini desteklemez.  

Saydam bir ara sunucu (kesme, satır içi veya zorlamalı ara sunucu olarak da bilinir), herhangi bir özel istemci yapılandırması gerekmeden Ağ katmanında normal iletişimi keser. İstemcilerin proxy varlığını farkında olması gerekmez.

![Saydam proxy](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Sonraki adımlar
[DNS tümleştirmesi](azure-stack-integrate-dns.md)
