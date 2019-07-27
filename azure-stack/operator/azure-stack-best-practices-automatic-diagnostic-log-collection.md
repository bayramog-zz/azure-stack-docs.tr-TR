---
title: Otomatik Azure Stack günlük toplama için en iyi uygulamalar | Microsoft Docs
description: Azure Stack yardım + destek içindeki otomatik günlük toplama için en iyi yöntemler
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
ms.date: 07/25/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: 03fb0550bfaa41a3336ff17dd1c44e03bcea1402
ms.sourcegitcommit: b752f4e6733d9ebe56dbd171a14528dcb9a693fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68522039"
---
# <a name="best-practices-for-automatic-azure-stack-log-collection"></a>Otomatik Azure Stack günlük toplama için en iyi yöntemler 

*Uygulama hedefi: Azure Stack tümleşik sistemler*


Bu konu, Azure Stack için otomatik tanılama günlüğü toplamayı yönetmeye yönelik en iyi yöntemleri içerir. 

## <a name="collecting-logs-from-multiple-azure-stack-systems"></a>Birden çok Azure Stack sisteminden Günlükler toplanıyor

Günlükleri toplamak istediğiniz her Azure Stack ölçek birimi için bir blob kapsayıcısı ayarlayın. Blob kapsayıcısının nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [otomatik Azure Stack tanılama günlüğü toplamayı yapılandırma](azure-stack-configure-automatic-diagnostic-log-collection.md). En iyi uygulama olarak, yalnızca tek bir blob kapsayıcısı içindeki aynı Azure Stack ölçek biriminden tanılama günlüklerini kaydedin. 

## <a name="retention-policy"></a>Saklama ilkesi

Günlük tutma ilkesini yönetmek için bir Azure Blob depolama [yaşam döngüsü yönetim kuralı](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts) oluşturun. Tanılama günlüklerini 30 gün boyunca saklamayı öneririz. Azure depolama 'da bir yaşam döngüsü yönetimi kuralı oluşturmak için Azure portal oturum açın, **depolama hesapları**' na tıklayın, blob kapsayıcısına tıklayın ve **BLOB hizmeti**altında **yaşam döngüsü yönetimi**' ne tıklayın.

![Azure portal yaşam döngüsü yönetimini gösteren ekran görüntüsü](media/azure-stack-automatic-log-collection/blob-storage-lifecycle-management.png)


## <a name="sas-token-expiration"></a>SAS belirteci süre sonu

SAS URL 'SI bitiş tarihi 'ni iki yıl olarak ayarlayın. Depolama hesabı anahtarlarınızı yenilemezseniz SAS URL 'sini yeniden oluşturmayı unutmayın. SAS belirtecini en iyi yöntemlere göre yönetmeniz gerekir. Daha fazla bilgi için bkz. [SAS kullanırken en iyi uygulamalar](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#best-practices-when-using-sas).


## <a name="bandwidth-consumption"></a>Bant genişliği tüketimi

Tanılama günlük toplamanın ortalama boyutu, günlük toplamanın isteğe bağlı veya otomatik olup olmamasına göre farklılık gösterir. 

İsteğe bağlı günlük toplama için, günlüklerin toplanması, kaç saat toplandığına bağlıdır. Son yedi gün içinde herhangi bir 1-4 saatlik kayan pencere seçebilirsiniz. 

Otomatik tanılama günlüğü koleksiyonu etkinleştirildiğinde, hizmet kritik uyarıları izler. Kritik bir uyarı gerçekleştirildikten sonra 30 dakika boyunca devam ediyorsa, hizmet uygun günlükleri toplayıp karşıya yükler. Bu günlük toplama boyutu, ortalama 2 GB 'tır. Düzeltme Eki ve güncelleştirme hatası durumunda otomatik günlük toplama yalnızca kritik bir uyarı ortaya çıktığında ve 30 dakika boyunca devam ediyorsa başlar. [Düzeltme ekini ve güncelleştirmeyi izlemeye yönelik yönergeleri](azure-stack-updates.md)izlemeniz önerilir.
Uyarı izleme, günlük toplama ve karşıya yükleme kullanıcıya saydamdır. 



Sağlıklı bir sistemde Günlükler hiç toplanmayacaktır. Sağlıksız bir sistemde, günlük toplama bir günde iki veya üç kez çalışabilir, ancak genellikle yalnızca bir kez çalıştırılabilir. En çok, en kötü durum senaryosunda günde on kez çalışabilir.  

Aşağıdaki tabloda, Azure için sınırlı veya tarifeli bağlantıları olan ortamlara, otomatik günlük toplamayı etkinleştirme etkisini göz önünde bulundurun.

| Ağ bağlantısı | Etkisi |
|--------------------|--------|
| Düşük-bant genişliği/yüksek gecikmeli bağlantı | Günlük yüklemesinin tamamlanması uzun zaman alır | 
| Paylaşılan bağlantı | Karşıya yükleme, ağ bağlantısını paylaşan diğer uygulamaları/kullanıcıları da etkileyebilir |
| Tarifeli bağlantı | Ek ağ kullanımı için ISS 'nizden ek ücret ödemeniz gerekebilir |


## <a name="managing-costs"></a>Maliyetleri yönetme

Azure [BLOB depolama ücretleri](https://azure.microsoft.com/pricing/details/storage/blobs/) , her ay ne kadar veri kaydedildiğini ve veri artıklığı gibi diğer faktörleri temel alır. Mevcut bir depolama hesabınız yoksa, Azure portal oturum açabilir, **depolama hesapları**' na tıklayabilir ve [Azure Blob kapsayıcı SAS URL 'si oluşturmak](azure-stack-configure-automatic-diagnostic-log-collection.md)için adımları izleyebilirsiniz.

En iyi uygulama olarak, devam eden depolama maliyetlerini en aza indirmek için bir Azure Blob depolama [yaşam döngüsü yönetim ilkesi](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts) oluşturun. Depolama hesabının nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [otomatik Azure Stack tanılama günlüğü toplamayı yapılandırma](azure-stack-configure-automatic-diagnostic-log-collection.md)

## <a name="see-also"></a>Ayrıca bkz.

[Otomatik Azure Stack günlük toplamayı yapılandırma](azure-stack-best-practices-automatic-diagnostic-log-collection.md)

