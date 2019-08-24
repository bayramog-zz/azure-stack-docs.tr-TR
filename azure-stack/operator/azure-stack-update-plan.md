---
title: Azure Stack güncelleştirme planı | Microsoft Docs
description: Azure Stack güncelleştirme planlanacağını öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2019
ms.author: mabrigg
ms.lastreviewed: 08/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: b4f98f61068d6c428dc17c635651ad9aac54f6a8
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70010203"
---
# <a name="plan-for-an-azure-stack-update"></a>Azure Stack güncelleştirme planı

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Kullanıcılarınız üzerinde en az etkisi olması için Azure Stack güncelleştirme işleminin olabildiğince sorunsuz bir şekilde gitmesini sağlayabilirsiniz. Bu makalede, kullanıcılarınıza olası hizmet kesintilerini bildirmeye yönelik adımları ve bir güncelleştirmeye hazırlanmak için almak istediğiniz adımları gözden geçirin.

## <a name="notify-your-users-of-maintenance-operations"></a>Kullanıcılarınıza bakım işlemlerini bildirme

Kullanıcılara herhangi bir bakım işlemini bilgilendirmelisiniz ve mümkünse normal bakım pencerelerini iş dışı saatlerde zamanlamanız gerekir. Bakım işlemleri, hem kiracı iş yüklerini hem de Portal işlemlerini etkileyebilir.

## <a name="prepare-for-an-azure-stack-update"></a>Azure Stack güncelleştirmesine hazırlanma

Tüm düzeltmeleri, güvenlik düzeltme eklerini ve OEM güncelleştirmelerini uyguladığınızdan, Azure Stack örneğinizin sistem durumunun doğrulanmasını, kullanılabilir kapasiteyi denetlediğinizden ve güncelleştirme paketini gözden geçirdiğinizden emin olarak güncelleştirmeye hazırlanabilirsiniz.

1. Bilinen sorunları gözden geçirin. Yönergeler için bkz. [Azure Stack bilinen sorunlar](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-known-issues-1907).

2. Güvenlik güncelleştirmelerini gözden geçirin. Güncelleştirmelerin listesi için bkz. [Azure Stack güvenlik güncelleştirmeleri](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-security-updates-1907).

3. Bu güncelleştirmeyi yüklemeye başlamadan önce, Azure Stack durumunu doğrulamak ve tüm uyarılar ve arızalar dahil olmak üzere bulunan işlem sorunlarını gidermek için [Test-AzureStack](https://docs.microsoft.com/azure-stack/operator/azure-stack-diagnostic-test) komutunu çalıştırın. Ayrıca etkin uyarıları gözden geçirin ve eylem gerektirenleri çözün.

    1. Azure Stack ERCS VM 'lerine ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturumu açın. PEP kullanma hakkında yönergeler için, bkz. [Azure Stack ayrıcalıklı uç noktası kullanma](https://docs.microsoft.com/azure-stack/operator/azure-stack-privileged-endpoint).

    2. Test-AzureStack-Group Updaterekurban

    3. Çıktıyı gözden geçirin ve sistem durumu hatalarını çözün. Daha fazla bilgi için bkz. [Azure Stack doğrulama testi çalıştırma](https://docs.microsoft.com/azure-stack/operator/azure-stack-diagnostic-test).

4. Sorunları çözün. Bayrak eklenmiş sorun hakkında daha fazla bilgi edinmek için sistem durumu ve uyarıları Izleme konusuna başvurabilirsiniz. Yönergeler için bkz. [Azure Stack durumu ve Uyarıları izleme](https://docs.microsoft.com/azure-stack/operator/azure-stack-monitor-health).

5. Son düzeltmeleri uygulayın. En son düzeltmelerin listesi için sürüm notları düzeltme bölümüne bakın. En son düzeltmeyi uyguladıktan sonra 3 ve 4. adımları yineleyin.

6. OEM paketinizin, güncelleştirdiğiniz Azure Stack sürümle uyumlu olduğundan emin olun. OEM paketiniz, güncelleştirdiğiniz Azure Stack sürümle uyumlu değilse, bir Azure Stack güncelleştirmesini çalıştırmadan önce bir OEM paketi güncelleştirmesi gerçekleştirmeniz gerekecektir. Yönergeler için, bkz. özgün donanım üreticisi (OEM) güncelleştirmelerini Azure Stack uygulama. " OEM paketi güncelleştirmesini uyguladıktan sonra 3 ve 4. adımları yineleyin.

7. Kapasite planlayıcısı aracını çalıştırın. Aracı kullanma hakkında genel bakış ve yönergeler için bkz. [Azure Stack kapasite planlamasına genel bakış](https://docs.microsoft.com/azure-stack/operator/azure-stack-capacity-planning-overview).

8. Güncelleştirme paketini gözden geçirin. Bakım pencereniz için planlama yaparken, Microsoft 'tan yayınlanan ve sürüm notlarında çağrılan şekilde yayımlanan belirli güncelleştirme paketi türünü gözden geçirmeniz önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güncelleştirmeyi uygulayın](azure-stack-apply-updates.md).
