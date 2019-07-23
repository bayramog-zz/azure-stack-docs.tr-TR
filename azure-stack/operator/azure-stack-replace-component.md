---
title: Azure Stack ölçek birimi düğümündeki bir donanım bileşenini değiştirme | Microsoft Docs
description: Azure Stack tümleştirilmiş bir sistemdeki bir donanım bileşenini değiştirmeyi öğrenin.
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
ms.author: thoroet
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: b9b1e862df288d599e9bd17cba09430372be0ce2
ms.sourcegitcommit: 159da88a52701679571bbedde1c36b72bbfe32dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68380428"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Azure Stack ölçek birimi düğümündeki bir donanım bileşenini değiştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, sık erişimli olmayan donanım bileşenlerinin yerini alan genel işlem açıklanmaktadır. Gerçek değiştirme adımları, özgün ekipman üreticisi (OEM) donanım satıcınıza göre farklılık gösterir. Azure Stack tümleşik sisteminize özgü ayrıntılı adımlar için satıcınızın alan değiştirilebilir birimi (FRU) belgelerine bakın.

> [!CAUTION]  
> Üretici yazılımı seviyelendirme, bu makalede açıklanan işlemin başarısı için önemlidir. Bu adımın eksik olması, sistem kararsızlığına, performans düşüşüyle, güvenlik iş parçacıklarından veya Azure Stack otomasyonunun işletim sistemini dağıtmasına engel olabilir. , Uygulanan üretici yazılımının [Azure Stack yönetici portalında](azure-stack-updates.md)görünen OEM sürümüyle eşleşmesini sağlamak için donanımı değiştirirken her zaman donanım iş ortağınızın belgelerine başvurun.

| Donanım Iş ortağı | Bölge | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | Tümü | [Microsoft Azure Stack Işlemler Kılavuzu için Cisco Tümleşik Sistem](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb)<br><br>[Microsoft Azure Stack için Cisco Tümleşik sistemi sürüm notları](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) |
| Dell EMC | Tümü | [Microsoft Azure Stack 14G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Microsoft Azure Stack 13G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | JAPONYA | [Fujitsu yönetilen hizmet destek masası (hesap ve oturum açma gereklidir)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | EMEA | [Fujitsu BT ürünlerini ve sistemlerini destekler](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) |
|  | AB | [Fujitsu MySupport (hesap ve oturum açma gereklidir)](https://support.ts.fujitsu.com/IndexMySupport.asp) |
| HPE | Tümü | [Microsoft Azure Stack için HPE ProLiant](http://www.hpe.com/info/MASupdates) |
| Lenovo | Tümü | [Ölçülü Kagile SXD En Iyi Tarifler](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)

Etkin olmayan değiştirilebilir bileşenler şunlardır:

- 'SUNA
- Bellek
- Anakart/temel kart yönetim denetleyicisi (BMC)/ekran kartı
- Disk denetleyicisi/ana bilgisayar veri yolu bağdaştırıcısı (HBA)/geri düzlemi
- Ağ bağdaştırıcısı (NIC)
- İşletim sistemi diski *
- Veri sürücüleri (dinamik değiştirmeyi desteklemeyen sürücüler, örneğin PCI-e eklenti kartları) *

\* Bu bileşenler, etkin değiştirmeyi destekleyebilir, ancak satıcı uygulamasına göre farklılık gösterebilir. Ayrıntılı adımlar için OEM satıcınızın FRU belgelerine bakın.

Aşağıdaki akış diyagramı, etkin olmayan bir donanım bileşenini değiştirmek için genel FRU işlemini gösterir.

![Bileşen değiştirme akışını gösteren akış diyagramı](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

\* * OEM Donanım satıcınızın bileşen değişimini gerçekleştirip gerçekleştirmediği ve bellenim, destek sözleşmeniz temelinde farklılık gösterebilir.

## <a name="review-alert-information"></a>Uyarı bilgilerini gözden geçirin

Azure Stack sistem durumu ve izleme sistemi, Depolama Alanları Doğrudan tarafından denetlenen ağ bağdaştırıcılarının ve veri sürücülerinin sistem durumunu izler. Diğer donanım bileşenlerini izlemez. Diğer tüm donanım bileşenleri için, uyarılar, donanım yaşam döngüsü ana bilgisayarında çalışan satıcıya özgü donanım izleme çözümünde oluşturulur.  

## <a name="component-replacement-process"></a>Bileşen değiştirme işlemi

Aşağıdaki adımlarda bileşen değiştirme işlemine yüksek düzey bir genel bakış sağlanmaktadır. OEM tarafından sunulan FRU belgelerinize başvurmadan bu adımları takip edin.

1. Ölçek birimi düğümünü düzgün bir şekilde kapatmak için kapatma eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

2. Beklenmeyen bir durumda, kapatılma eylemi başarısız olursa, ölçek birimi düğümünü bakım moduna almak için [boşalt](azure-stack-node-actions.md#drain) eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

   > [!NOTE]  
   > Herhangi bir durumda, S2D (Depolama Alanları Doğrudan) bozmadan aynı anda yalnızca bir düğüm devre dışı bırakılabilir ve kapatılabilir.

3. Ölçek birimi düğümü bakım modundan olduktan sonra, [kapatma](azure-stack-node-actions.md#scale-unit-node-actions) eylemini kullanın. Bu eylem, donanımın fiziksel koşuluna bağlı olarak gerekli olmayabilir.

   > [!NOTE]  
   > Kapatma eyleminin çalışmamasının olası olmaması durumunda bunun yerine temel kart yönetim denetleyicisi (BMC) Web arabirimini kullanın.

4. Hasarlı donanım bileşenini değiştirin. OEM Donanım satıcınızın bileşen değişimini gerçekleştirip gerçekleştirmediği, destek sözleşmeniz temelinde farklılık gösterebilir.  
5. Üretici yazılımını güncelleştirin. Değişen donanım bileşeninin onaylanan bellenim düzeyinin uygulanmış olduğundan emin olmak için, donanım yaşam döngüsü konağını kullanarak satıcıya özgü bellenim güncelleştirme işleminizi izleyin. OEM Donanım satıcınızın bu adımı gerçekleştirip gerçekleştirmediğini, destek sözleşmeniz temelinde farklılık gösterebilir.  
6. Ölçek birimi düğümünü ölçek birimine geri getirmek için [onarma](azure-stack-node-actions.md#scale-unit-node-actions) eylemini kullanın.
7. [Sanal disk onarımı durumunu denetlemek](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair-using-the-privileged-endpoint)için ayrıcalıklı uç noktasını kullanın. Yeni veri sürücüleriyle, tam bir depolama onarma işi sistem yüküne ve tüketilen alana bağlı olarak birden çok saat sürebilir.
8. Onarım eylemi tamamlandıktan sonra, tüm etkin uyarıların otomatik olarak kapatıldığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Etkin değiştirilebilen bir fiziksel diski değiştirme hakkında daha fazla bilgi için bkz. [disk değiştirme](azure-stack-replace-disk.md).
- Fiziksel bir düğümü değiştirme hakkında daha fazla bilgi için bkz. [ölçek birimi düğümünü değiştirme](azure-stack-replace-node.md).
