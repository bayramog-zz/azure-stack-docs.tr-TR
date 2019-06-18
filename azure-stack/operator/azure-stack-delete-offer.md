---
title: Kotalar, planlar, teklifler ve abonelikler silme | Microsoft Docs
description: Azure Stack kotalar, planlar, teklifler ve abonelikleri silmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 04/25/2019
ms.openlocfilehash: 2e0e4ef7abd1885d843832ed7cc9e845003d0ed7
ms.sourcegitcommit: 593d40bccf1b2957a763017a8a2d7043f8d8315c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152542"
---
# <a name="delete-quotas-plans-offers-and-subscriptions"></a>Kotalar, planlar, teklifler ve abonelikler Sil

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, kotalar, planlar, teklifler ve artık ihtiyacınız olmayan abonelikleri silme işlemini açıklar. Yalnızca ne kullanımda olmadığından, genel bir ilke olarak silebilirsiniz. Örneğin, bu teklife ait abonelik olduğunda yalnızca bir teklif siliniyor olur.

Bu genel ilke için bir özel durum aboneliklerinin: kaynakları; içeren abonelikleri silebilirsiniz ve kaynakları abonelik birlikte silinecek.

Kota silmek istiyorsanız, bu nedenle, tüm planlar ve o kota kullanın teklifler geri çalışmalıdır: teklifler sayesinde, başlatma, aboneliğiniz yok, her bir teklifin silin ve kota ve benzeri planlarını silmek olun.

## <a name="delete-a-subscription"></a>Aboneliği silme

Bir aboneliği silmek için işaretleyin **tüm hizmetleri**, ardından **kullanıcı aboneliklerini**sistemde tüm Aboneliklerin listesini görüntülemek için. Bir teklif üzerinde çalışıyorsanız ve o da seçebilirsiniz **abonelikleri** buradan.

Bu listeden abonelikleri silebilir veya PowerShell belirtilmiştir komutlar kullanılarak sizin için tüm abonelikleri siler bir komut dosyası yazmak için kullanabileceğiniz [abonelikler - Delete başvuru](/rest/api/azurestack/subscriptions/delete).

> [!CAUTION]
> Abonelik siliniyor herhangi bir veri ve içerdiği kaynakları da siler.

## <a name="delete-an-offer"></a>Bir teklifi Sil

Yönetici portalı'nda bir teklif silmek için Git **tüm hizmetleri**, ardından **sunar**. Silin ve ardından istediğiniz teklif seçin **Sil**.

![delsub1](media/azure-stack-delete-offer/delsub1.png)

Bunu kullanan hiçbir abonelikler varken bir teklif yalnızca silebilirsiniz. Abonelik teklifi göre mevcutsa **Sil** seçeneği gri. Bu durumda bkz [aboneliği silme](#delete-a-subscription) bölümü.

## <a name="delete-a-plan"></a>Planı silme

Yönetici portalı Git bir planı silmek için **tüm hizmetleri**, ardından **planları**. Silin ve ardından istediğiniz planı seçin **Sil**.

![delsub2](media/azure-stack-delete-offer/delsub2.png)

Teklifler ya da onu kullanan abonelikleri olduğunda, yalnızca bir planı silebilirsiniz. Varsa planı kullanın, planı silmek, başarısız olmasına izin teklifler vardır ve bir hata iletisi alırsınız. Seçebileceğiniz **üst teklifler** planını kullanın tekliflerinin bir listesini görüntülemek için. Teklifler silme hakkında daha fazla bilgi için bkz. [teklif Sil](#delete-an-offer).

Teklifin bir parçası olmasa bile planları doğrudan bir abonelikte eklenti planı için eklenmiş. Bu durumda, planın silinebilmesi için önce bunları kullanan aboneliklerden kaldırılmalıdır.

Ayrıca, bu abonelik için belirli bir kaynağın yalnızca kaynak ise bir abonelikten bir plan kaldırılamaz. Örneğin, bir Plan 1. aboneliğe eklenmiş olan ve bu abonelik için bir ağ kotası sağlayarak yalnızca plan, aboneliğin kaldırılamaz. Bu nedenle, silinemez.

## <a name="edit-and-delete-a-quota"></a>Düzenleme ve kota silme

Görüntüleyebilir ve Yönetici portalını kullanarak mevcut kotaları Düzenle: seçin **bölge Yönetimi**, ilgili kaynak Sağlayıcısı'nı seçin ve tıklayın **kotalar**. Belirli kaynak sağlayıcıları için kotaları da silebilirsiniz.

![delsub3](media/azure-stack-delete-offer/delsub3.png)

Alternatif olarak, bu REST API'lerini kullanarak bazı kotalar silebilirsiniz:

- [İşlem](/rest/api/azurestack/quotas%20(compute)/delete)
- [Ağ](/rest/api/azurestack/quotas%20(network)/delete)
- [Depolama](/rest/api/azurestack/storagequotas/delete)

> [!NOTE]
> Kullanan herhangi bir geçerli plan, kota silemezsiniz. Kota başvuran planı silmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](../user/azure-stack-create-vm-template.md)