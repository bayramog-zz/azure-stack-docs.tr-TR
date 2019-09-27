---
title: Kotaları, planları, teklifleri ve abonelikleri silme | Microsoft Docs
description: Azure Stack kotaları, planları, teklifleri ve abonelikleri silmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: bryanla
ms.reviewer: efemmano
ms.lastreviewed: 04/25/2019
ms.openlocfilehash: e89fcb3c218ab8b7228b63211b9f94c7e03d9865
ms.sourcegitcommit: 1bae55e754d7be75e03af7a4db3ec43fd7ff3e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71319123"
---
# <a name="delete-quotas-plans-offers-and-subscriptions"></a>Kotaları, planları, teklifleri ve abonelikleri silme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, artık ihtiyacınız olmayan kotaların, planların, tekliflerin ve aboneliklerin nasıl silineceği açıklanır. Genel bir ilke olarak yalnızca kullanımda olmayan öğeleri silebilirsiniz. Örneğin, bir teklifi silmek yalnızca bu teklifine ait bir abonelik yoksa mümkündür.

Abonelikler bu genel prensibi özel durumdur: kaynakları içeren abonelikleri silebilirsiniz; ve kaynaklar abonelikle birlikte silinecektir.

Bu nedenle, bir kotayı silmek istiyorsanız, bu kotayı kullanan tüm planlar ve teklifler aracılığıyla geri çalışmanız gerekir: tekliflerle başlayarak, abonelikleri olmadığından, her bir teklifi sildiğinizden, sonra kotayı kullanan planları silebilmeniz ve bu şekilde devam etmeniz gerekir.

## <a name="delete-a-subscription"></a>Abonelik silme

Bir aboneliği silmek için, **tüm hizmetler**' i ve ardından **Kullanıcı abonelikleri**' ni seçerek sistemdeki tüm Aboneliklerin listesini görüntüleyin. Bir teklif üzerinde çalışıyorsanız, **abonelikleri** buradan da seçebilirsiniz.

Bu listeden abonelikleri silebilir veya [abonelikler-Sil başvurusunda](/rest/api/azurestack/subscriptions/delete)belgelenen komutları kullanarak sizin için tüm abonelikleri silen bir betik yazmak için PowerShell kullanabilirsiniz.

> [!CAUTION]
> Bir aboneliğin silinmesi, içerdiği tüm verileri ve kaynakları da siler.

## <a name="delete-an-offer"></a>Teklifi silme

Bir teklifi silmek için, yönetici portalında, **tüm hizmetler**' e gidin ve **teklifleri**. Silmek istediğiniz teklifi seçin ve **Sil**' i seçin.

![delsub1](media/azure-stack-delete-offer/delsub1.png)

Bir teklifi yalnızca kullanarak abonelik olmadığında silebilirsiniz. Bu teklif temelinde abonelikler mevcutsa, **silme** seçeneği kullanılamaz. Bu durumda, [abonelik silme](#delete-a-subscription) bölümüne bakın.

## <a name="delete-a-plan"></a>Planı silme

Bir planı silmek için, yönetici portalında **tüm hizmetler**'e ve ardından **planlar**'a gidin. Silmek istediğiniz planı seçin, sonra **Sil**' i seçin.

![delsub2](media/azure-stack-delete-offer/delsub2.png)

Bir planı yalnızca bir teklif veya abonelik olmadığında silebilirsiniz. Planı kullanan teklifler varsa, planı silin, başarısız olmasına izin verin ve bir hata iletisi alırsınız. Planı kullanan tekliflerin listesini göstermek için **üst teklifleri** seçebilirsiniz. Teklifleri silme hakkında daha fazla bilgi için bkz. [bir teklifi silme](#delete-an-offer).

Planlar, teklifin bir parçası olmasalar bile, bir aboneliğe doğrudan eklenti planları olarak eklenmiş olabilir. Bu durumda, plan silinmeden önce bunları kullanan aboneliklerden kaldırılması gerekir.

Ayrıca, bu abonelik için belirli bir kaynağın tek kaynağı olması halinde bir plan bir abonelikten kaldırılamaz. Örneğin, plan A, 1. aboneliğe eklenmiştir ve abonelik için bir ağ kotası sağlayan tek plandır, abonelikten kaldırılamaz. Bu nedenle, silinemez.

## <a name="edit-and-delete-a-quota"></a>Kotayı düzenleme ve silme

Yönetici portalını kullanarak mevcut kotaları görüntüleyebilir ve düzenleyebilirsiniz: **Bölge yönetimi**' ni seçin, ardından ilgili kaynak sağlayıcısını seçin ve **Kotalar**' e tıklayın. Ayrıca, belirli kaynak sağlayıcıları için kotaları da silebilirsiniz.

![delsub3](media/azure-stack-delete-offer/delsub3.png)

Alternatif olarak, şu REST API 'Lerini kullanarak bazı kotalar silebilirsiniz:

- [İşlem](/rest/api/azurestack/quotas%20(compute)/delete)
- [Ağ](/rest/api/azurestack/quotas%20(network)/delete)
- [Depolama](/rest/api/azurestack/storagequotas/delete)

> [!NOTE]
> Bu uygulamayı kullanan geçerli planlar varsa, kotayı silemezsiniz. Öncelikle kotaya başvuran planı silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](../user/azure-stack-create-vm-template.md)