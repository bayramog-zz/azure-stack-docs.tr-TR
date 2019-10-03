---
title: Azure Stack SQL kaynak sağlayıcısı 1.1.30.0 sürüm notları | Microsoft Docs
description: Bilinen sorunlar ve nereye indirileceği dahil olmak üzere en son Azure Stack SQL kaynak sağlayıcısı güncelleştirmesinde neler olduğunu öğrenin.
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
ms.openlocfilehash: 69ea42a9efbf57cfdeb589cc221eae8a9f21913c
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829313"
---
# <a name="sql-resource-provider-11330-release-notes"></a>SQL kaynak sağlayıcısı 1.1.33.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları SQL kaynak sağlayıcısı sürüm 1.1.33.0 geliştirmeleri ve bilinen sorunları anlatmaktadır.

## <a name="build-reference"></a>Yapı Başvurusu
SQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için kendi kendine ayıklayıcısı 'nı çalıştırın. Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır. SQL kaynak sağlayıcısı 'nın bu sürümünü yüklemek için gereken en düşük Azure Stack yayın sürümü aşağıda listelenmiştir:

> |En düşük Azure Stack sürümü|SQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[SQL RP sürüm 1.1.33.0](https://aka.ms/azurestacksqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> Desteklenen en düşük Azure Stack güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya SQL kaynak sağlayıcısı 'nın en son sürümünü dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack SQL kaynak sağlayıcısı 'nın bu sürümü aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

### <a name="fixes"></a>Düzeltmeleri
- **SQL kaynak sağlayıcısı portalı uzantısı yanlış aboneliği seçebilir**. SQL kaynak sağlayıcısı, kullanılacak ilk hizmet yönetici aboneliğini belirleyen Azure Resource Manager çağrılarını kullanır ve bu, *varsayılan sağlayıcı aboneliği*olmayabilir. Bu durumda, SQL kaynak sağlayıcısı normal şekilde çalışmaz. 

- **SQL barındırma sunucusu barındırılan veritabanlarını listelemez.** Kullanıcı tarafından oluşturulan veritabanları, SQL barındırma sunucularının kiracı kaynakları görüntülenirken listelenmeyebilir.

- **TLS 1,2 etkinleştirilmemişse ÖNCEKI SQL kaynak sağlayıcısı (1.1.30.0) dağıtımı başarısız olabilir**. Kaynak sağlayıcısını dağıtmakta, kaynak sağlayıcısını güncelleştirirken veya gizli dizileri döndürürken TLS 1,2 ' i etkinleştirmek için SQL kaynak sağlayıcısı 1.1.33.0 güncelleştirildi. 

- **SQL kaynak sağlayıcısı gizli anahtarı döndürme başarısız oluyor**. Sorun düzeltildi: `New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bilinen sorunlar 

- **SQL SKU 'larının portalda görünür olması bir saate kadar sürebilir**. Yeni oluşturulan SKU 'Ların yeni SQL veritabanları oluşturulurken kullanılmak üzere görünür olması bir saate kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **SQL oturum açmaları yeniden kullanılır**. Aynı abonelikte var olan bir oturum açma işlemiyle aynı kullanıcı adıyla yeni bir SQL oturumu oluşturmaya çalışmak, aynı oturumun ve mevcut parolanın yeniden kullanılmaya neden olur. 

    **Geçici çözüm**: Aynı abonelik altında yeni oturumlar oluştururken farklı Kullanıcı adları kullanın veya farklı Abonelikler altında aynı kullanıcı adıyla oturum açmaları oluşturun.

- **PAYLAŞıLAN SQL oturum açmaları veri tutarsızlığına neden olur**. Aynı abonelik kapsamında birden çok SQL veritabanı için bir SQL oturumu paylaşılmışsa, oturum açma parolasının değiştirilmesi veri tutarsızlığına neden olur.

    **Geçici çözüm**: Aynı abonelikte farklı veritabanları için her zaman farklı oturumlar kullanın.

- **SQL kaynak sağlayıcısı SQL Server her zaman dinleyici ekleme işlemi yapamıyor**. SQL Server her zaman dinleyicinin dinleyici IP adresi kullanılırken SQL kaynak sağlayıcısı VM, dinleyicinin ana bilgisayar adını çözümleyemiyor.

    **Geçici çözüm**: DNS 'in dinleyici IP 'si ile dinleyici ana bilgisayar adına çözümlenmesi için doğru çalıştığından emin olun.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Cloud Admins işletim Azure Stack için bilinen sorunlar
[Azure Stack sürüm notlarındaki](azure-stack-servicing-policy.md)belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar
[SQL kaynak sağlayıcısı hakkında daha fazla bilgi edinin](azure-stack-sql-resource-provider.md).

[SQL kaynak sağlayıcısı 'nı dağıtmaya hazırlanın](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[SQL kaynak sağlayıcısını önceki bir sürümden yükseltin](azure-stack-sql-resource-provider-update.md). 