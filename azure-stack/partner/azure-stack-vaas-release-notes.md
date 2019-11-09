---
title: Hizmet sürüm notları olarak Azure Stack doğrulaması | Microsoft Docs
description: Hizmet sürüm notları olarak doğrulama Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: aa85310314a09db47f10424e84fe40e355bacb25
ms.sourcegitcommit: ed44d477b9fd11573d1e0d1ed3a3c0ef4512df53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73846231"
---
# <a name="release-notes-for-validation-as-a-service"></a>Hizmet olarak doğrulama için sürüm notları

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bu makalede hizmet olarak Azure Stack doğrulaması için sürüm notları bulunur.

## <a name="version-4353"></a>Sürüm 4.3.5.3

2019 Kasım 7

- Test içeriği güncelleştirmeleri
  - Aylık Azure Stack güncelleştirme doğrulaması (sürüm 5.1.46.0-> 5.1.49.0)
  - OEM Uzantı paketi doğrulaması (sürüm 5.1.46.0-> 5.1.49.0)
  - 5\.1.46.0 için sonuçlar korundu. 5\.1.46.0 üzerinde başarılı bir çalışmalarınız varsa, sonuçları gönderirken vaashelp@microsoft.com bildirin.

- Hata düzeltmeleri
  - Update. zip özel karakterler içeriyorsa aylık Azure Stack güncelleştirme doğrulamasının çalıştırılmediği bir sorun düzeltildi.

- Bilinen sorunlar
  - MSTest. exe bulunamazsa, VaaS testleri başarısız olur. Sorunu
    1. PowerShell penceresinde aracı CTRL + C.
    1. MSTest. exe ' nin tanınan bir program olduğunu doğrulamak için MSTest. exe yazın.
    1. MSTest. exe tanınmazsa, geçerli PowerShell penceresini kapatın.
    1. Başlat ' a tıklayın (görev çubuğunuzda PowerShell değil), PowerShell bulun ve yönetici olarak açın.
    1. MSTest. exe yazın ve bir komut olarak kullanılabilir olduğunu doğrulayın.
    1. Aracıyı yeniden başlatın ve testi yeniden çalıştırın.
  - Bulut benzetimi altyapısı bazen \*VM testleriyle ilgili sorunları raporlar. Yeniden çalıştırmayı denemeden önce vaashelp@microsoft.com başvurun. 


2019 Ekim 29

- Aylık Azure Stack güncelleştirme iş akışı ve OEM paketi doğrulama iş akışı için çevrimiçi belgeler güncelleştirilmiştir.

    Lütfen OEM paketlerini doğrulamak ve Microsoft 'tan yazılım güncelleştirmelerini doğrulamak için buradaki güncelleştirilmiş belgeleri inceleyin
- VaaS Iş akışı güncelleştirmesi: aylık Azure Stack Güncelleştirmesi (sürüm 5.1.30.0-> 5.1.46.0) – aylık Azure Stack güncelleştirme doğrulama test iş akışı güncelleştirildi.

    İş akışı artık el ile müdahale gerektirmez ve sorunsuz çalışacak şekilde zamanlanabilir.
- VaaS Iş akışı güncelleştirmesi: OEM paket doğrulaması (sürüm 5.1.30.0-> 5.1.46.0) – OEM paketi doğrulama iş akışı güncelleştirilmiştir.

    İş akışı artık el ile müdahale gerektirmez ve sorunsuz çalışacak şekilde zamanlanabilir.
- OEM paketi doğrulama iş akışındaki (sürüm 5.1.30.0-> 5.1.46.0) bulut benzetimi altyapısı, doğrulama süresini hızlandırmak için güncelleştirilmiştir: çalışma süresi 1 saate düşürüldü.
- OEM paketi doğrulama iş akışındaki bulut benzetimi altyapısı ve Azure Stack güncelleştirme iş akışı (sürüm 5.1.30.0-> 5.1.46.0), tüm güncelleştirmelerin, alt klasörlerde diğer güncelleştirmeler olmadan 2 farklı üst klasörde olmasını gerektirir.
- OEM paketi doğrulama iş akışındaki bulut benzetimi altyapısı ve Azure Stack güncelleştirme iş akışı (sürüm 5.1.30.0-> 5.1.46.0), testlerin şu sırada zamanlanmasını gerektirir – aylık Azure Stack güncelleştirme doğrulama testi, OEM Uzantı paketi Doğrulama testi ve son olarak bulut benzetim altyapısı.
- VaaS aracı güncelleştirmesi: güncelleştirilmiş VaaS Aracısı artık iş akışlarını otomatik olarak doldurmak için damga bilgilerini almak üzere damgası sorgulamak için Azure Stack Cloud Manager kimlik bilgilerini kullanır. 

    Bu güncelleştirme, tüm aracıların güncelleştirilmesini ve yeniden başlatılmasını gerektirir. Lütfen VaaS aracısını güncelleştirme hakkında şu yönergelere bakın: https://docs.microsoft.com/en-us/azure-stack/partner/azure-stack-vaas-local-agent
- VaaS portalı Kullanıcı arabirimi güncelleştirmesi: aracı seçim tablosu, testi kolaylaştırmak için test zamanlama bölmesinin üzerine taşınmıştır.

    Bir işi zamanlarken, VaaS aracıları doğru şekilde güncelleştirilirse damga bilgilerini girmeye gerek yoktur.


## <a name="version-405"></a>Sürüm 4.0.5

7 Haziran 2019

- Paket doğrulama iş akışındaki bulut benzetimi altyapısı, doğrulama süresini hızlandırmak için güncelleştirilmiştir:  
    Çalışma Zamanı: 6 saate düşürüldü  
    Sürüm: 5.1.13.0-> 5.1.22.0  


17 Ocak 2019

- Disk kimlik testi, depolama havuzu tutarsızlıklarını karşılamak üzere güncelleştirildi. Sürüm: 5.1.14.0-> 5.1.15.0
- Azure Stack onaylanan yazılım ve içerik doğrulama tutarsızlıklarına yönelik aylık güncelleştirme doğrulaması güncelleştirildi. Sürüm: 5.1.14.0-> 5.1.17.0
- OEM Uzantı paketi doğrulaması Azure Stack güncelleştirme adımından önce gerekli denetimleri gerçekleştirecek şekilde güncelleştirildi. Sürüm: 5.1.14.0-> 5.1.16.0
- İç hata düzeltmeleri

## <a name="version-402"></a>Sürüm 4.0.2

7 Ocak 2019

Azure Stack aylık güncelleştirme doğrulama iş akışını çalıştırıyorsanız ve OEM güncelleştirme paketinizin sürümü 1810 veya üzeri değilse, OEM güncelleştirme adımına geldiğinizde bir hata alırsınız. Bu bir hatadır. Bir çözüm geliştirilmiştir. Risk azaltma adımları aşağıdaki gibidir:

1. OEM güncelleştirmesini normal olarak çalıştırın.
2. Paketin başarılı bir uygulamadan sonra test-AzureStack komutunu yürütün ve çıktıyı kaydedin.
3. Testi iptal edin.
4. Çalıştırma sonuçlarını almak için kaydedilen çıktıyı VaaSHelp@microsoft.com ' a gönderin.

## <a name="version-402"></a>Sürüm 4.0.2

30 Kasım 2018

- İç hata düzeltmeleri

## <a name="version-401"></a>Sürüm 4.0.1

8 Ekim 2018

- VaaS önkoşulları

    `Install-VaaSPrerequisites` artık bulut Yöneticisi kimlik bilgileri gerektirmez. Bu cmdlet 'in en son sürümünü çalıştırıyorsanız, önkoşulları yüklemeye yönelik düzeltilen komutlar için bkz. [Yerel aracıyı indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent) . Komutlar şunlardır:

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>Sürüm 4.0.0

29 Ağustos 2018

- VaaS önkoşulları ve VHD güncelleştirmeleri

    `Install-VaaSPrerequisites` artık paket doğrulaması sırasında bir sorunu gidermek için bulut Yöneticisi kimlik bilgileri gerektirir. [Yerel aracıyı indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent) belgelerindeki belgeler, aşağıdakiler ile güncelleştirilmiştir:

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > Betik için gereken `$CloudAdminCreds`, Azure Stack örneğinin doğrulanması içindir. VaaS kiracısı tarafından kullanılan Azure Active Directory kimlik bilgileri değildir.

- Yerel Aracı güncelleştirmesi

    Yerel aracının önceki sürümü hizmetin geçerli 4.0.0 sürümüyle uyumlu değil. Tüm kullanıcılar kendi yerel aracılarını güncelleştirmelidir. En Yeni aracıyı yükleme hakkında yönergeler için bkz. [Yerel aracıyı indirme ve yükleme](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent) .

- PowerShell Otomasyonu güncelleştirmesi

    Komut dosyası paketlerinin en son sürümünü gerektiren `LaunchVaaSTests` PowerShell betiklerine yapılan değişiklikler yapılmıştır. Betik paketinin en son sürümünü yükleme yönergeleri için bkz. [test geçiş iş akışını başlatma](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow) .

- Hizmet portalı olarak doğrulama

  - Paket imzalama bildirimleri

    Bir OEM Özelleştirme paketi, paket doğrulama iş akışının bir parçası olarak gönderildiğinde, yayımlanan belirtimi takip etmek için paket biçimi doğrulanacaktır. Paket uyumlu değilse, çalıştırma başarısız olur. E-posta bildirimleri, kiracının kayıtlı Azure Active Directory kişisinin e-posta adresine gönderilir.

  - Etkileşimli test kategorisi

    **Etkileşimli** test kategorisi eklendi. Bu sınamalar etkileşimli, otomatik olmayan Azure Stack senaryolar ister.

  - Etkileşimli Özellik doğrulaması

    Belirli özellikler için odaklanmış geri bildirim sağlama özelliği artık test geçiş iş akışında kullanılabilir. `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` test, belirli güncelleştirmelerin doğru şekilde uygulanmış olup olmadığını denetler ve ardından geri bildirim toplar.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
