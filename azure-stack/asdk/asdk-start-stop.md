---
title: ASDK 'yi başlatma ve durdurma | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) başlatma ve durdurma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: 5232b1087414d6e7149157063a253b18a6b6e13a
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025803"
---
# <a name="start-and-stop-the-asdk"></a>ASDK 'yi başlatma ve durdurma
Yalnızca ASDK ana bilgisayar bilgisayarı yeniden başlatmanız önerilmez. Bunun yerine, bu makaledeki yordamları izleyerek, ASDK hizmetlerini doğru şekilde kapatıp yeniden başlatın.

## <a name="stop-azure-stack"></a>Azure Stack durdur 
Azure Stack Hizmetleri ve ASDK ana bilgisayar bilgisayarını düzgün bir şekilde kapatmak için aşağıdaki PowerShell komutlarını kullanın:

1. ASDK ana bilgisayarında AzureStack\AzureStackAdmin olarak oturum açın.
2. PowerShell 'i yönetici olarak açın (PowerShell ıSE).
3. Ayrıcalıklı uç nokta (PEP) oturumu oluşturmak için aşağıdaki komutları çalıştırın: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Sonra, PEP oturumunda, Azure Stack hizmetlerini durdurmak ve ASDK ana bilgisayarını kapatmak için **stop-AzureStack** cmdlet 'ini kullanın:

   ```powershell
   Stop-AzureStack
   ```
5. Tüm Azure Stack hizmetlerinin, ASDK konak bilgisayarı kapatılmadan önce başarıyla kapatılmasını sağlamak için PowerShell çıkışını gözden geçirin. Kapalı işlemi birkaç dakika sürer.

## <a name="start-azure-stack"></a>Azure Stack Başlat 
Ana bilgisayar başlatıldığında ASDK Hizmetleri otomatik olarak başlamalıdır. Ancak, ASDK Altyapı Hizmetleri başlangıç süresi, ASDK ana bilgisayar donanım yapılandırmasının performansına göre farklılık gösterir. Bazı durumlarda tüm hizmetlerin başarıyla yeniden başlatılması birkaç saat sürebilir.

ASDK 'nin nasıl kapatıldığına bakılmaksızın, tüm Azure Stack hizmetlerinin başlatıldığını ve ana bilgisayar oturum açtıktan sonra tam olarak çalıştığını doğrulamak için aşağıdaki adımları kullanmanız gerekir: 

1. ASDK ana bilgisayarında güç. 
2. ASDK ana bilgisayarında AzureStack\AzureStackAdmin olarak oturum açın.
3. PowerShell 'i yönetici olarak açın (PowerShell ıSE).
4. Ayrıcalıklı uç nokta (PEP) oturumu oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Sonra, PEP oturumunda Azure Stack hizmetlerinin başlatma durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Azure Stack hizmetlerinin başarıyla yeniden başlatıldığından emin olmak için çıktıyı gözden geçirin.

Azure Stack hizmetlerini düzgün bir şekilde kapatmak ve yeniden başlatmak için Önerilen yordamlar hakkında daha fazla bilgi edinmek için bkz. [Azure Stack başlatma ve durdurma](../operator/azure-stack-start-and-stop.md).

## <a name="troubleshoot-startup-and-shutdown"></a>Başlatma ve kapatmadan ilgili sorunları giderme 
Azure Stack Hizmetleri, ASDK ana bilgisayar bilgisayarınızı etkinleştirdikten sonra iki saat içinde başarıyla başlamazsa, bu adımları gerçekleştirin:

1. ASDK ana bilgisayarında AzureStack\AzureStackAdmin olarak oturum açın.
2. PowerShell 'i yönetici olarak açın (PowerShell ıSE).
3. Ayrıcalıklı uç nokta (PEP) oturumu oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Sonra, PEP oturumunda Azure Stack hizmetlerinin başlatma durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Test-AzureStack
   ```
5. Çıktıyı gözden geçirin ve hataları çözün. Daha fazla bilgi için bkz. [Azure Stack doğrulama testi çalıştırma](../operator/azure-stack-diagnostic-test.md).
6. **Start-AzureStack** cmdlet 'ini çalıştırarak Pep oturumunun içinden Azure Stack hizmetlerini yeniden başlatın:

   ```powershell
   Start-AzureStack
   ```

**Start-AzureStack** çalıştırmak bir hata ile sonuçlanırsa, asdk sorun giderme desteğini almak için [Azure Stack destek forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) ziyaret edin. 

## <a name="next-steps"></a>Sonraki adımlar 
Azure Stack Tanılama aracı ve sorun günlüğü hakkında daha fazla bilgi edinin, bkz. [Azure Stack tanılama araçları](../operator/azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep).
