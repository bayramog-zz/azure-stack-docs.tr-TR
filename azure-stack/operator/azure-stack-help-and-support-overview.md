---
title: Microsoft Azure Stack yardım ve desteğe genel bakış | Microsoft Docs
description: Microsoft Azure Stack için destek alın.
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
ms.openlocfilehash: ff7911f64cf7c1413e623cd84cebe3d26585a3e5
ms.sourcegitcommit: f6ea6daddb92cbf458f9824cd2f8e7e1bda9688e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68493851"
---
# <a name="microsoft-azure-stack-help-and-support"></a>Yardım ve destek Microsoft Azure Stack

Azure Stack portalındaki **Yardım + Destek** , operatörlerin Azure Stack hakkında daha fazla bilgi alması, destek seçeneklerini kontrol etmek ve uzman yardımı almak için kaynaklar içerir. İşleçler 1907 sürümünden itibaren, sorun giderme için tanılama günlüklerini toplamak üzere yardım + destek ' i de kullanabilir.  

## <a name="help-resources"></a>Yardım kaynakları 

İşleçler Ayrıca, Azure Stack hakkında daha fazla bilgi edinmek, destek seçeneklerini denetlemek ve uzman yardımı almak için **Yardım + Destek** 'i de kullanabilir. 

### <a name="things-to-try-first"></a>İlk denenecek şeyler

**Yardım ve destek** ' in en üstünde, yeni bir kavram hakkında bilgi edinmek, faturalandırmayı anlamak veya hangi destek seçeneklerinin kullanılabildiğini görmek gibi, ilk olarak deneyebileceğiniz öğelerin bağlantıları bulunur. 

![Self servis desteği](media/azure-stack-help-and-support/get-support-tiles.png)

- **Belgeler**. [Azure Stack operatör belgeleri](index.yml) , sanal MAKINELER, SQL veritabanları, Web uygulamaları ve daha fazlası gibi Azure Stack hizmetlerin nasıl sunulmasını gösteren kavramlar, nasıl yapılır konuları ve öğreticiler içerir. 

- **Faturalandırma hakkında bilgi edinin**. [Kullanım ve faturalandırma](azure-stack-billing-and-chargeback.md)hakkında ipuçları alın.

- **Destek seçenekleri**. Azure Stack işleçler, herhangi bir kuruluşun ihtiyaçlarına uygun bir dizi [Azure destek seçeneğinden](https://aka.ms/azstacksupport) seçim yapabilir. 

### <a name="get-expert-help"></a>Uzman yardımı alın 

Tümleşik bir sistem için, Microsoft ve özgün ekipman üreticisi (OEM) donanım iş ortakları arasında eşgüdümlü bir ilerletme ve çözüm süreci vardır.

Bir bulut hizmetleri sorunu varsa, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla destek sunulur. Yönetici portalının sağ üst köşesindeki **Yardım** ' a (soru işareti) tıklayabilir ve ardından Yardım + Destek ' **i açmak için** Yardım **+ destek** ' e tıklayabilir ve yeni bir destek isteği gönderebilirsiniz. Destek isteği oluşturmak, Azure Stack hizmeti 'ni önceden seçer. Müşterilerin ortak Azure portal kullanmak yerine bilet göndermek için bu deneyimi kullanmalarını önemle öneririz. 

Dağıtım, düzeltme eki ve güncelleştirme, donanım (alan değiştirilebilir birimler dahil) ve donanım yaşam döngüsü ana bilgisayarında çalışan yazılımlar gibi donanım markalı yazılımlar hakkında bir sorun varsa, önce OEM donanım satıcınıza başvurun. Başka bir şey için Microsoft CSS ile iletişim kurun.

![Tümleşik sistemler için uzman yardımı alın](media/azure-stack-help-and-support/get-support-integrated.png)

ASDK için, [Azure Stack MSDN forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)destekle ilgili sorularınızı sorabilirsiniz. 

Yönetici portalının sağ üst köşesindeki **Yardım** ' a (soru işareti) tıklayabilir ve ardından Yardım + Destek ' **e tıklayarak yardım** + destek ' i **açarak**foruma yönelik bir bağlantı bulunur. MSDN forumları düzenli olarak izlenir.  
Geliştirme seti bir değerlendirme ortamı olduğundan, Microsoft CSS aracılığıyla sunulan resmi bir destek yoktur.

![ASDK için uzman yardımı alın](media/azure-stack-help-and-support/get-support-asdk.png)

Bir sorunu tartışmak veya çevrimiçi eğitim almak ve kendi becerilerinizi geliştirmek için MSDN forumlarına da ulaşabilirsiniz. 

![Uzman yardımı alın](media/azure-stack-help-and-support/get-support-cards.png)

### <a name="get-up-to-speed-with-azure-stack"></a>Azure Stack hız kazanın

Bu öğretici kümesi, ortamınızda hızlı bir şekilde hızlanabilmeniz için, ASDK veya tümleşik sistemleri çalıştırıp çalıştırdığınıza bağlı olarak özelleştirilir. 

![Destek öğreticilerini alın](media/azure-stack-help-and-support/get-support-tutorials.png)

## <a name="diagnostic-log-collection"></a>Tanılama günlüğü koleksiyonu

1907 sürümünden başlayarak, **Yardım ve destek**'te günlükleri toplamanın iki yeni yolu vardır:

- **Otomatik koleksiyon**: Etkinleştirilirse, günlük toplama belirli sistem durumu uyarıları tarafından tetiklenir 
- **Günlükleri Şimdi topla**: Son yedi gün içinde bir 1-4 saat kayan pencere seçebilirsiniz

![Tanılama günlüğü toplama seçeneklerinin ekran görüntüsü](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

Tümleşik sistemler, Microsoft müşteri destek hizmeti (CSS) ile tanılama günlüklerini paylaşabilir. Azure Stack Geliştirme Seti (ASDK) bir değerlendirme ortamı olduğundan, CSS tarafından desteklenmez. Daha fazla bilgi için bkz. [tanılama günlüğü toplamaya genel bakış Azure Stack](azure-stack-diagnostic-log-collection-overview.md).



## <a name="help-and-support-for-earlier-releases-azure-stack-pre-1905"></a>Azure Stack önceki sürümler için yardım ve destek (1905 öncesi)

Önceki Azure Stack sürümlerde Ayrıca, [Azure Stack Işleci belgelerine](https://aka.ms/adminportaldocs)yönlendiren **Yardım + Destek** bağlantısı da vardır.

![Destek öğreticilerini alın](media/azure-stack-help-and-support/get-support-previous.png)

Bir bulut hizmetleri sorunu varsa, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla destek sunulur. Yönetici portalının sağ üst köşesindeki **Yardım** ' a (soru işareti) tıklayabilir, **Yardım ve destek**' e ve ardından **Yeni destek isteği** ' ne tıklayarak CSS ile yeni bir destek isteği gönderebilirsiniz.

Tümleşik bir sistem için, Microsoft ile OEM iş ortakları arasında eşgüdümlü bir ilerletme ve çözüm süreci vardır. Bir bulut hizmetleri sorunu varsa, destek Microsoft CSS aracılığıyla sunulur. 

Dağıtım, düzeltme eki ve güncelleştirme, donanım (alan değiştirilebilir birimler dahil) ve donanım yaşam döngüsü ana bilgisayarında çalışan yazılımlar gibi donanım markalı yazılımlar hakkında bir sorun varsa, önce OEM donanım satıcınıza başvurun. Başka bir şey için Microsoft CSS ile iletişim kurun.

Geliştirme seti için [Azure Stack MSDN forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)destekle ilgili sorularınızı sorabilirsiniz. Yönetici portalının sağ üst köşesindeki **Yardım** ' a (soru işareti) tıklayabilir ve ardından Azure Stack topluluğundaki uzmanlardan yardım almak için **Yeni destek isteği** ' ne tıklayabilirsiniz.
Geliştirme seti bir değerlendirme ortamı olduğundan, Microsoft CSS aracılığıyla sunulan resmi bir destek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun giderme Azure Stack](azure-stack-troubleshooting.md) hakkında bilgi edinin