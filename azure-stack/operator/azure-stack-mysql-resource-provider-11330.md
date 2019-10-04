---
title: Azure Stack MySQL kaynak sağlayıcısı 1.1.30.0 sürüm notları | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere en son Azure Stack MySQL kaynak sağlayıcısı güncelleştirmesinde neler olduğunu ve nereden indirileceği hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: jiahan
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: fdac6c099980a1c2cedb3271123908539d18169d
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909171"
---
# <a name="mysql-resource-provider-11330--release-notes"></a>MySQL kaynak sağlayıcısı 1.1.33.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notlarında, MySQL kaynak sağlayıcısı sürüm 1.1.33.0 ' deki iyileştirmeler ve bilinen sorunlar açıklanır.

## <a name="build-reference"></a>Yapı Başvurusu
MySQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için Self-Extractor ' i çalıştırın. Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır. MySQL kaynak sağlayıcısı 'nın bu sürümünü yüklemek için gereken en düşük Azure Stack yayın sürümü aşağıda listelenmiştir:

> |En düşük Azure Stack sürümü|MySQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[MySQL RP sürüm 1.1.33.0](https://aka.ms/azurestackmysqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> Desteklenen en düşük Azure Stack güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya MySQL kaynak sağlayıcısı 'nın en son sürümünü dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack MySQL kaynak sağlayıcısının bu sürümü aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

### <a name="fixes"></a>Düzeltmeleri
- **MySQL kaynak sağlayıcısı portalı uzantısı yanlış aboneliği seçebilir**. MySQL kaynak sağlayıcısı, kullanılacak ilk hizmet yönetici aboneliğini belirleyen Azure Resource Manager çağrılarını kullanır ve bu, *varsayılan sağlayıcı aboneliği*olmayabilir. Bu durumda, MySQL kaynak sağlayıcısı normal şekilde çalışmaz. 

- **MySQL barındırma sunucusu barındırılan veritabanlarını listelemez.** Kullanıcı tarafından oluşturulan veritabanları, MySQL barındırma sunucuları için kiracı kaynakları görüntülenirken listelenmeyebilir.

- **TLS 1,2 etkinleştirilmemişse önceki MySQL kaynak sağlayıcısı (1.1.30.0) dağıtımı başarısız olabilir**. Kaynak sağlayıcısını dağıtmakta, kaynak sağlayıcıyı güncelleştirirken veya gizli dizileri döndürürken TLS 1,2 ' i etkinleştirmek için MySQL kaynak sağlayıcısı 1.1.33.0 ' yi güncelledi. 

- **MySQL kaynak sağlayıcısı gizli anahtarı döndürme başarısız oluyor**. Sorun düzeltildi: `New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bilinen sorunlar 

- **MySQL SKU 'larının portalda görünür olması bir saate kadar sürebilir**. Yeni oluşturulan SKU 'Ların yeni MySQL veritabanları oluştururken kullanılması için bir saate kadar zaman alabilir. 

    **Geçici çözüm**: Yok.

- **MySQL oturum açmaları yeniden kullanılır**. Aynı abonelikte var olan bir oturum açma işlemiyle aynı kullanıcı adıyla yeni bir MySQL oturumu oluşturmaya çalışmak, aynı oturumun ve mevcut parolanın yeniden kullanılmaya neden olur. 

    **Geçici çözüm**: Aynı abonelik altında yeni oturumlar oluştururken farklı Kullanıcı adları kullanın veya farklı Abonelikler altında aynı kullanıcı adıyla oturum açmaları oluşturun.

- **Paylaşılan MySQL oturum açmaları veri tutarsızlığına neden olur**. Aynı abonelik kapsamında birden çok MySQL veritabanı için bir MySQL oturumu paylaşılmışsa, oturum açma parolasının değiştirilmesi veri tutarsızlığına neden olur.

    **Geçici çözüm**: Aynı abonelikte farklı veritabanları için her zaman farklı oturumlar kullanın.


### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Cloud Admins işletim Azure Stack için bilinen sorunlar
[Azure Stack sürüm notlarındaki](azure-stack-servicing-policy.md)belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı hakkında daha fazla bilgi edinin](azure-stack-mysql-resource-provider.md).

[MySQL kaynak sağlayıcısını dağıtmaya hazırlanın](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[MySQL kaynak sağlayıcısını önceki bir sürümden yükseltin](azure-stack-mysql-resource-provider-update.md). 