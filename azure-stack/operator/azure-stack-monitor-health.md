---
title: Azure Stack 'de sistem durumunu ve Uyarıları izleme | Microsoft Docs
description: Azure Stack sistem durumunu ve uyarıları izlemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/2/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 5d7074997a42da9ca19006e3b597d2ba02613b28
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534130"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Azure Stack durumu ve Uyarıları izleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, bir Azure Stack bölgesinin sistem durumunu ve uyarılarını görüntülemenize yardımcı olan altyapı izleme özelliklerini içerir. **Bölge yönetimi** kutucuğu Azure Stack dağıtılan tüm bölgeleri listeler. Varsayılan sağlayıcı aboneliği için yönetici portalında varsayılan olarak sabitlenmiştir. Kutucuk, her bölge için etkin kritik ve uyarı uyarılarının sayısını gösterir. Kutucuk, giriş noktanğunuz Azure Stack sistem durumu ve uyarı işlevselliğine sahiptir.

![Azure Stack Yönetici portalı 'nda bölge yönetimi kutucuğu](media/azure-stack-monitor-health/image1.png)

## <a name="understand-health-in-azure-stack"></a>Azure Stack durumunu anlama

Sistem durumu kaynak sağlayıcısı, sistem durumunu ve uyarıları yönetir. Azure Stack altyapı bileşenleri, Azure Stack dağıtım ve yapılandırma sırasında sistem durumu kaynak sağlayıcısına kayıt kaydeder. Bu kayıt, her bileşen için sistem durumu ve uyarıların görüntülenmesini mümkün bir şekilde sunar. Azure Stack sistem durumu basit bir kavramdır. Bir bileşenin kayıtlı bir örneğinin uyarıları varsa, bu bileşenin sistem durumu en kötü etkin uyarı önem derecesini yansıtır: uyarı veya kritik.

## <a name="alert-severity-definition"></a>Uyarı önem derecesi tanımı

Azure Stack, uyarıları yalnızca iki önem derecesi ile başlatır: **Uyarı** ve **kritik**.

- **Warning**  
  Bir işleç, uyarı uyarısını zamanlanan bir şekilde ele alabilir. Uyarı genellikle kullanıcı iş yüklerini etkilemez.

- **Başlatma**  
  Bir operatör önemli uyarıyı acille adreslemelidir. Bu uyarılar Şu anda Azure Stack kullanıcıları etkileyen veya yakında etkileyecek sorunları gösterir.


## <a name="view-and-manage-component-health-state"></a>Bileşen sistem durumunu görüntüleyin ve yönetin

Bileşenlerin sistem durumunu yönetici portalında ve REST API ve PowerShell aracılığıyla görüntüleyebilirsiniz.

Portalda sistem durumunu görüntülemek için **Bölge yönetimi** kutucuğunda görüntülemek istediğiniz bölgeye tıklayın. Altyapı rollerinin ve kaynak sağlayıcılarının sistem durumunu görüntüleyebilirsiniz.

![Altyapı rollerinin listesi](media/azure-stack-monitor-health/image2.png)

Daha ayrıntılı bilgi görüntülemek için bir kaynak sağlayıcısına veya altyapı rolüne tıklayabilirsiniz.

> [!WARNING]  
> Bir altyapı rolüne tıklayıp rol örneğine tıklarsanız, **başlatma**, **yeniden başlatma**veya **kapanmaya**yönelik seçenekler vardır. Tümleşik bir sisteme güncelleştirmeler uyguladığınızda bu eylemleri kullanmayın. Ayrıca, bu seçenekleri bir Azure Stack Geliştirme Seti (ASDK) **ortamında kullanmayın.** Bu seçenekler yalnızca, altyapı rolü başına birden fazla rol örneği olduğu tümleşik sistemler ortamı için tasarlanmıştır. ASDK içindeki bir rol örneğini yeniden başlatmak sistem kararsızlığına neden olur. Sorun giderme yardımı için sorununuzu [Azure Stack forumuna](https://aka.ms/azurestackforum)gönderin.
>

## <a name="view-alerts"></a>Uyarıları görüntüleme

Her bir Azure Stack bölgesi için etkin uyarıların listesi doğrudan **Bölge yönetimi** dikey penceresinden kullanılabilir. Varsayılan yapılandırmadaki ilk kutucuk, bölge için kritik ve uyarı uyarılarının özetini görüntüleyen **Uyarılar** kutucuğudur. Uyarı kutucuğunu, bu dikey penceredeki diğer tüm kutucuklar gibi hızlı erişim panosuna sabitleyebilirsiniz.

![Azure Stack yönetici portalında bir uyarı gösteren uyarılar kutucuğu](media/azure-stack-monitor-health/image3.png)

 Bölge için tüm etkin uyarıların bir listesini görüntülemek için, **Uyarılar** kutucuğunun en üstteki kısmını seçin. Filtrelenmiş bir uyarı listesini (kritik veya uyarı) görüntülemek için, kutucuğun içindeki **kritik** veya **Uyarı** satırı öğesini seçin.

**Uyarılar** dikey penceresi hem durum durumunda (etkin veya kapalı) hem de önem derecesine (kritik veya uyarı) göre filtreleme özelliğini destekler. Varsayılan görünüm tüm etkin uyarıları görüntüler. Tüm kapatılan uyarılar, yedi gün sonra sistemden kaldırılır.

>[!Note]
>Bir uyarı etkin kalırsa ancak bir gün içinde güncelleştirilmemiş ise, [Test-AzureStack](azure-stack-diagnostic-test.md) ' i çalıştırabilir ve herhangi bir sorun bildirilmemişse uyarıyı kapatabilirsiniz.

![Azure Stack yönetici portalında kritik veya uyarı durumuna göre filtrelemek için filtre bölmesi](media/azure-stack-monitor-health/alert-view.png)

**API görüntüle** eylemi, liste görünümünü oluşturmak için kullanılan REST API görüntüler. Bu eylem, uyarıları sorgulamak için kullanabileceğiniz REST API söz dizimini tanımak için hızlı bir yol sağlar. Bu API 'YI Otomasyon veya mevcut veri merkezi izleme, raporlama ve bilet oluşturma çözümleriyle tümleştirme için kullanabilirsiniz.

Uyarı ayrıntılarını görüntülemek için belirli bir uyarıya tıklayabilirsiniz. Uyarı ayrıntıları, uyarıyla ilişkili tüm alanları gösterir ve etkilenen bileşene ve uyarının kaynağına hızlı gezinmeyi etkinleştirir. Örneğin, altyapı rolü örneklerinden biri çevrimdışı veya erişilebilir değilse aşağıdaki uyarı oluşur.  

![Azure Stack yönetici portalındaki Uyarı ayrıntıları dikey penceresi](media/azure-stack-monitor-health/alert-detail.png)

## <a name="repair-alerts"></a>Uyarıları onarma

Bazı uyarılarda **Onar** ' ı seçebilirsiniz.

Seçildiğinde, **onarım** eylemi sorunu çözmeye çalışmak için uyarıya özgü adımları gerçekleştirir. Seçildiğinde, **onarım** eyleminin durumu bir portal bildirimi olarak kullanılabilir.

![Onarma uyarısı eylemi devam ediyor](media/azure-stack-monitor-health/repair-in-progress.png)

**Onarım** eylemi, aynı portal bildirim dikey penceresinde eylemi tamamlamak için başarılı tamamlamayı veya başarısız olduğunu bildirir.  Bir uyarı için onarım eylemi başarısız olursa, uyarı ayrıntısından **onarma** eylemini yeniden çalıştırabilirsiniz. Onarma eylemi başarıyla tamamlanırsa, **onarım** eylemini **yeniden çalıştırmayın** .

![Onarma eylemi başarıyla tamamlandı](media/azure-stack-monitor-health/repair-completed.png)

Altyapı rolü örneği yeniden çevrimiçi olduktan sonra, bu uyarı otomatik olarak kapanır. Her uyarı için pek çok, temel alınan sorun çözüldüğünde otomatik olarak kapat. Azure Stack sorunu giderirse, bir onarım eylemi düğmesi sağlayan uyarılar otomatik olarak kapanır. Diğer tüm uyarılar için düzeltme adımlarını gerçekleştirdikten sonra **uyarıyı kapat** ' ı seçin. Sorun devam ederse, Azure Stack yeni bir uyarı oluşturur. Sorunu çözemezseniz, uyarı kapalı kalır ve daha fazla adım gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güncelleştirmelerini yönetme](azure-stack-updates.md)

[Azure Stack 'de bölge yönetimi](azure-stack-region-management.md)
