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
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 7679589090022f6f07c5e1fddb9c768716628865
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829435"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>MySQL kaynak sağlayıcısı 1.1.30.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notlarında, MySQL kaynak sağlayıcısı sürüm 1.1.30.0 ' deki iyileştirmeler ve bilinen sorunlar açıklanır.

## <a name="build-reference"></a>Yapı Başvurusu
MySQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için Self-Extractor ' i çalıştırın. Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır. MySQL kaynak sağlayıcısı 'nın bu sürümünü yüklemek için gereken en düşük Azure Stack yayın sürümü aşağıda listelenmiştir:

> |En düşük Azure Stack sürümü|MySQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Azure Stack 1808 Güncelleştirmesi (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Desteklenen en düşük Azure Stack güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya MySQL kaynak sağlayıcısı 'nın en son sürümünü dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack MySQL kaynak sağlayıcısının bu sürümü aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **MySQL kaynak sağlayıcısı dağıtımları Için telemetri etkinleştirildi**. Telemetri toplama, MySQL kaynak sağlayıcısı dağıtımları için etkinleştirildi. Toplanan telemetri, kaynak sağlayıcısı dağıtımı, başlatma ve durdurma zamanları, çıkış durumu, çıkış iletileri ve hata ayrıntılardır (varsa).

- **TLS 1,2 şifreleme güncelleştirmesi**. Etkin TLS 1,2-yalnızca dahili Azure Stack bileşenleriyle kaynak sağlayıcısı iletişimi için destek. 

### <a name="fixes"></a>Düzeltmeleri

- **MySQL kaynak sağlayıcısı PowerShell uyumluluğunu Azure Stack**. MySQL kaynak sağlayıcısı, Azure Stack 2018-03-01-hibrit PowerShell profiliyle çalışacak ve Azurerd 1.3.0 ve üzeri sürümlerle uyumluluk sağlayacak şekilde güncelleştirilmiştir.

- **MySQL oturum açma parolasını değiştir dikey**penceresi. Parola Değiştir dikey penceresinde parolanın değiştirilebileceği bir sorun düzeltildi. Parola değişikliği bildirimlerinin bağlantıları kaldırıldı.

## <a name="known-issues"></a>Bilinen sorunlar 

- **MySQL SKU 'larının portalda görünür olması bir saate kadar sürebilir**. Yeni oluşturulan SKU 'Ların yeni MySQL veritabanları oluştururken kullanılması için bir saate kadar zaman alabilir. 

    **Geçici çözüm**: Yok.

- **MySQL oturum açmaları yeniden kullanılır**. Aynı abonelikte var olan bir oturum açma işlemiyle aynı kullanıcı adıyla yeni bir MySQL oturumu oluşturmaya çalışmak, aynı oturumun ve mevcut parolanın yeniden kullanılmaya neden olur. 

    **Geçici çözüm**: Aynı abonelik altında yeni oturumlar oluştururken farklı Kullanıcı adları kullanın veya farklı Abonelikler altında aynı kullanıcı adıyla oturum açmaları oluşturun.

- **TLS 1,2 destek gereksinimi**. MySQL kaynak sağlayıcısını TLS 1,2 'nin etkinleştirilmediği bir bilgisayardan dağıtmayı veya güncelleştirmeyi denerseniz, işlem başarısız olabilir. TLS 1,2 ' nin desteklendiği şekilde döndürüldüğünü doğrulamak için, kaynak sağlayıcısını dağıtmak veya güncelleştirmek üzere kullanılan bilgisayarda aşağıdaki PowerShell komutunu çalıştırın:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  **Tls12** , komutun çıkışına dahil edilmediğinde bilgisayarda TLS 1,2 etkin değildir.

    **Geçici çözüm**: TLS 1,2 ' i etkinleştirmek için aşağıdaki PowerShell komutunu çalıştırın ve ardından aynı PowerShell oturumundan kaynak sağlayıcısı dağıtımını veya güncelleştirme betiğini başlatın:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
 
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Cloud Admins işletim Azure Stack için bilinen sorunlar
[Azure Stack sürüm notlarındaki](azure-stack-servicing-policy.md)belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı hakkında daha fazla bilgi edinin](azure-stack-mysql-resource-provider.md).

[MySQL kaynak sağlayıcısını dağıtmaya hazırlanın](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[MySQL kaynak sağlayıcısını önceki bir sürümden yükseltin](azure-stack-mysql-resource-provider-update.md). 