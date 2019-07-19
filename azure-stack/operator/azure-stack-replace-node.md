---
title: Azure Stack tümleşik bir sistemdeki ölçek birimi düğümünü değiştirme | Microsoft Docs
description: Azure Stack tümleşik bir sistemdeki fiziksel ölçek birimi düğümünü değiştirmeyi öğrenin.
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
ms.date: 07/18/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: 64e85e3b83962e0e5b2e0cac2072392b4cf28c88
ms.sourcegitcommit: cb2376ed76c784e475b99352a024eaa7a148f42f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328752"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Azure Stack tümleşik bir sistemdeki ölçek birimi düğümünü değiştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, Azure Stack tümleşik bir sistemde fiziksel bir bilgisayarın (ölçek birimi düğümü olarak da bilinir) yerini alacak genel işlem açıklanır. Gerçek ölçek birimi düğüm değiştirme adımları, özgün ekipman üreticisi (OEM) donanım satıcınıza göre değişir. Sisteminize özgü ayrıntılı adımlar için satıcınızın alan değiştirilebilir birimi (FRU) belgelerine bakın.

> [!CAUTION]  
> Üretici yazılımı seviyelendirme, bu makalede açıklanan işlemin başarısı için önemlidir. Bu adımın eksik olması, sistem kararsızlığına, performans düşüşüyle, güvenlik iş parçacıklarından veya Azure Stack otomasyonunun işletim sistemini dağıtmasına engel olabilir. , Uygulanan üretici yazılımının [Azure Stack yönetici portalında](azure-stack-updates.md)görünen OEM sürümüyle eşleşmesini sağlamak için donanımı değiştirirken her zaman donanım iş ortağınızın belgelerine başvurun.

| Donanım Iş ortağı | Bölge | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | Tümü | [https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb)<br><br>[https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) |
| Dell EMC | Tümü | [https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | JAPONYA | [https://eservice.fujitsu.com/supportdesk-web/](https://eservice.fujitsu.com/supportdesk-web/) |
|  | EMEA | [https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) |
|  |  | [https://support.ts.fujitsu.com/IndexMySupport.asp](https://support.ts.fujitsu.com/IndexMySupport.asp) |
| HPE | Tümü | [http://www.hpe.com/info/MASupdates](http://www.hpe.com/info/MASupdates) |
| Lenovo |  | [https://datacentersupport.lenovo.com/us/en/solutions/ht505122](https://datacentersupport.lenovo.com/us/en/solutions/ht505122) |

Aşağıdaki akış diyagramı, tüm ölçek birimi düğümünün yerini alacak genel FRU işlemini gösterir.

![Düğüm değiştirme işlemi için akış grafiği](media/azure-stack-replace-node/replacenodeflow.png)

\* Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

> [!Note]  
> Bu işlem başarısız olursa, boşaltma işlemini ve sonrasında durdur işlemini kullanmanız önerilir. Daha fazla ayrıntı için bkz. kullanılabilir düğüm işlemleri  

## <a name="review-alert-information"></a>Uyarı bilgilerini gözden geçirin

Ölçek birimi düğümü kapalıysa, aşağıdaki kritik uyarıları alırsınız:

- Düğüm ağ denetleyicisine bağlanmadı
- Sanal makine yerleşimi için düğüm erişilebilir değil
- Ölçek birimi düğümü çevrimdışı

![Ölçek birimi için uyarı listesi aşağı](media/azure-stack-replace-node/nodedownalerts.png)

**Ölçek birimi düğümünü** açarsanız, uyarı açıklaması, erişilemeyen ölçek birimi düğümünü içerir. Ayrıca, donanım yaşam döngüsü ana bilgisayarında çalışan OEM 'e özgü izleme çözümünde ek uyarılar da alabilirsiniz.

![Düğüm çevrimdışı uyarısı ayrıntıları](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Birim düğümü değiştirme işlemini Ölçeklendir

Aşağıdaki adımlar, ölçek birimi düğümü değiştirme işlemine yüksek düzey bir genel bakış olarak sunulmaktadır. Sisteminize özgü ayrıntılı adımlar için OEM Donanım satıcınızın FRU belgelerine bakın. OEM tarafından sağlanmış belgelerinize başvurulmadan bu adımları takip edin.

1. Ölçek birimi düğümünü düzgün bir şekilde kapatmak için **kapatılıyor** eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir. 

2. Kapalı olma ihtimaline karşı koruma eylemi başarısız olursa, ölçek birimi düğümünü bakım moduna almak için [boşalt](azure-stack-node-actions.md#drain) eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

   > [!NOTE]  
   > Herhangi bir durumda, S2D (Depolama Alanları Doğrudan) bozmadan aynı anda yalnızca bir düğüm devre dışı bırakılabilir ve kapatılabilir.

3. Ölçek birimi düğümü bakım modundayken, [Durdur](azure-stack-node-actions.md#stop) eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

   > [!NOTE]  
   > Kapatma eyleminin çalışmamasının olası olmaması durumunda bunun yerine temel kart yönetim denetleyicisi (BMC) Web arabirimini kullanın.

4. Fiziksel bilgisayarı değiştirin. Genellikle bu, OEM donanım satıcınız tarafından yapılır.
5. Yeni fiziksel bilgisayarı ölçek birimine eklemek için [onarma](azure-stack-node-actions.md#repair) eylemini kullanın.
6. [Sanal disk onarımı durumunu denetlemek](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair-using-the-privileged-endpoint)için ayrıcalıklı uç noktasını kullanın. Yeni veri sürücüleriyle, tam bir depolama onarma işi sistem yüküne ve tüketilen alana bağlı olarak birden çok saat sürebilir.
7. Onarım eylemi tamamlandıktan sonra, tüm etkin uyarıların otomatik olarak kapatıldığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sistem açıkken fiziksel disk değiştirme hakkında daha fazla bilgi için bkz. [disk değiştirme](azure-stack-replace-disk.md). 
- Sistemin kapatılmasını gerektiren bir donanım bileşenini değiştirme hakkında daha fazla bilgi için bkz. [bir donanım bileşenini değiştirme](azure-stack-replace-component.md).
