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
ms.openlocfilehash: f17c2ba41097d5b9bda903ae5d95c62e0ac9f53a
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829350"
---
# <a name="sql-resource-provider-11300-release-notes"></a>SQL kaynak sağlayıcısı 1.1.30.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları SQL kaynak sağlayıcısı sürüm 1.1.30.0 geliştirmeleri ve bilinen sorunları anlatmaktadır.

## <a name="build-reference"></a>Yapı Başvurusu
SQL kaynak sağlayıcısı ikilisini indirin ve ardından içeriği geçici bir dizine çıkarmak için kendi kendine ayıklayıcısı 'nı çalıştırın. Kaynak sağlayıcının en düşük karşılık gelen Azure Stack derlemesi vardır. SQL kaynak sağlayıcısı 'nın bu sürümünü yüklemek için gereken en düşük Azure Stack yayın sürümü aşağıda listelenmiştir:

> |En düşük Azure Stack sürümü|SQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Desteklenen en düşük Azure Stack güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya SQL kaynak sağlayıcısı 'nın en son sürümünü dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack SQL kaynak sağlayıcısı 'nın bu sürümü aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **SQL kaynak sağlayıcısı dağıtımları Için telemetri etkinleştirildi**. Telemetri koleksiyonu SQL kaynak sağlayıcısı dağıtımları için etkinleştirildi. Toplanan telemetri, kaynak sağlayıcısı dağıtımı, başlatma ve durdurma zamanları, çıkış durumu, çıkış iletileri ve hata ayrıntılardır (varsa).

- **TLS 1,2 şifreleme güncelleştirmesi**. Etkin TLS 1,2-yalnızca dahili Azure Stack bileşenleriyle kaynak sağlayıcısı iletişimi için destek. 

### <a name="fixes"></a>Düzeltmeleri

- **SQL kaynak sağlayıcısı PowerShell uyumluluğunu Azure Stack**. SQL kaynak sağlayıcısı, Azure Stack 2018-03-01-hibrit PowerShell profiliyle çalışacak ve Azurerd 1.3.0 ve üzeri sürümlerle uyumluluk sağlayacak şekilde güncelleştirilmiştir.

- **SQL oturum açma parolasını değiştir dikey**penceresi. Parola Değiştir dikey penceresinde parolanın değiştirilebileceği bir sorun düzeltildi. Parola değişikliği bildirimlerinin bağlantıları kaldırıldı.

- **SQL barındırma sunucusu ayarları dikey penceresi güncelleştirmesi**. Ayarlar dikey penceresinin yanlış "parola" olarak gösterildiği bir sorun düzeltildi.

## <a name="known-issues"></a>Bilinen sorunlar 

- **SQL SKU 'larının portalda görünür olması bir saate kadar sürebilir**. Yeni oluşturulan SKU 'Ların yeni SQL veritabanları oluşturulurken kullanılmak üzere görünür olması bir saate kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **SQL oturum açmaları yeniden kullanılır**. Aynı abonelikte var olan bir oturum açma işlemiyle aynı kullanıcı adıyla yeni bir SQL oturumu oluşturmaya çalışmak, aynı oturumun ve mevcut parolanın yeniden kullanılmaya neden olur. 

    **Geçici çözüm**: Aynı abonelik altında yeni oturumlar oluştururken farklı Kullanıcı adları kullanın veya farklı Abonelikler altında aynı kullanıcı adıyla oturum açmaları oluşturun.

- **PAYLAŞıLAN SQL oturum açmaları veri tutarsızlığına neden olur**. Aynı abonelik kapsamında birden çok SQL veritabanı için bir SQL oturumu paylaşılmışsa, oturum açma parolasının değiştirilmesi veri tutarsızlığına neden olur.

    **Geçici çözüm**: Aynı abonelikte farklı veritabanları için her zaman farklı oturumlar kullanın.

- **TLS 1,2 destek gereksinimi**. SQL kaynak sağlayıcısı 'nı TLS 1,2 'nin etkinleştirilmediği bir bilgisayardan dağıtmayı veya güncelleştirmeyi denerseniz, işlem başarısız olabilir. TLS 1,2 ' nin desteklendiği şekilde döndürüldüğünü doğrulamak için, kaynak sağlayıcısını dağıtmak veya güncelleştirmek üzere kullanılan bilgisayarda aşağıdaki PowerShell komutunu çalıştırın:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  **Tls12** , komutun çıkışına dahil edilmediğinde bilgisayarda TLS 1,2 etkin değildir.

    **Geçici çözüm**: TLS 1,2 ' i etkinleştirmek için aşağıdaki PowerShell komutunu çalıştırın ve ardından aynı PowerShell oturumundan kaynak sağlayıcısı dağıtımını veya güncelleştirme betiğini başlatın:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
- **SQL kaynak sağlayıcısı SQL Server her zaman dinleyici ekleme işlemi yapamıyor**. SQL Server her zaman dinleyicinin dinleyici IP adresi kullanılırken SQL kaynak sağlayıcısı VM, dinleyicinin ana bilgisayar adını çözümleyemiyor.

    **Geçici çözüm**: DNS 'in dinleyici IP 'si ile dinleyici ana bilgisayar adına çözümlenmesi için doğru çalıştığından emin olun.
    
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Cloud Admins işletim Azure Stack için bilinen sorunlar
[Azure Stack sürüm notlarındaki](azure-stack-servicing-policy.md)belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar
[SQL kaynak sağlayıcısı hakkında daha fazla bilgi edinin](azure-stack-sql-resource-provider.md).

[SQL kaynak sağlayıcısı 'nı dağıtmaya hazırlanın](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[SQL kaynak sağlayıcısını önceki bir sürümden yükseltin](azure-stack-sql-resource-provider-update.md). 