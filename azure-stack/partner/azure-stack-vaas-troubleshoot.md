---
title: Hizmet olarak Azure Stack doğrulama sorunlarını giderme | Microsoft Docs
description: Azure Stack hizmet olarak doğrulama sorunlarını giderin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 9c8807d6fb28a99c9de8464a0eaff7114bd6a162
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418261"
---
# <a name="troubleshoot-validation-as-a-service"></a>Hizmet olarak doğrulama sorunlarını giderme

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Aşağıda, yazılım yayınları ve çözümlerinin ilgisi olmayan yaygın sorunlar verilmiştir.

## <a name="local-agent"></a>Yerel Aracı

### <a name="the-portal-shows-local-agent-in-debug-mode"></a>Portal, yerel aracıyı hata ayıklama modunda gösterir

Bunun nedeni, aracının kararsız bir ağ bağlantısı nedeniyle hizmete sinyal gönderemediği için olasıdır. Her beş dakikada bir sinyal gönderilir. Hizmet, 15 dakika boyunca bir sinyal almadığında, hizmet aracıyı devre dışı olarak kabul eder ve testlerin bundan sonra planlanmayacak. Aracının başlatıldığı dizinde bulunan,. *log* dosyasındaki hata iletisini kontrol edin.

> [!Note]
> Aracıda çalışmakta olan tüm testler çalışmaya devam eder, ancak test bitmeden önce sinyal geri yüklenemezse, aracı test durumunu güncelleştiremez veya günlükleri karşıya yükleyebilir. Test her zaman **çalışıyor** olarak görünür ve iptal edilmesi gerekir.

### <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>Test yürütülürken makinedeki Aracı işlemi kapatıldı. Beklensin mi?

Aracı işlemi düzgün şekilde kapalıysa, makine yeniden başlatılır, işlem sonlandırıldı (aracı penceresinde CTRL + C) düzgün kapanma olarak kabul edilir), üzerinde çalışan test **çalışıyor**olarak gösterilmeye devam eder. Aracı yeniden başlatılırsa, aracı testin durumunu **iptal edildi**olarak güncelleştirir. Aracı yeniden başlatılmazsa, test **çalışıyor** olarak görünür ve testi el ile iptal etmeniz gerekir.

> [!Note]
> Bir iş akışı içindeki testler sırayla çalışacak şekilde zamanlanır. **Bekleyen** testler, **çalışma** durumundaki testler aynı iş akışında tamamlanana kadar yürütülmeyecektir.

## <a name="vm-images"></a>VM görüntüleri

### <a name="handle-slow-network-connectivity"></a>Yavaş ağ bağlantısını işleme

PIR görüntüsünü yerel veri merkezinizdeki bir paylaşıma indirebilirsiniz. Ardından görüntüyü kontrol edebilirsiniz.

<!-- This is from the appendix to the Deploy local agent topic. -->

#### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>Ağ trafiği yavaş olduğunda PIR görüntüsünü yerel paylaşıma indirin

1. AzCopy şuradan indirin: [vaasexternaldependencies (AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. AzCopy. zip dosyasını ayıklayın ve AzCopy. exe ' yi içeren dizine geçin

3. Yükseltilmiş bir komut isteminden Windows PowerShell 'i açın. Aşağıdaki komutları çalıştırın:

```powershell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare, paylaşma yolu veya yerel yoldur.

#### <a name="verifying-pir-image-file-hash-value"></a>PIR görüntü dosyası karma değeri doğrulanıyor

Görüntülerin bütünlüğünü denetlemek için, indirilen ortak görüntü deposu görüntü dosyalarının karma değerini almak üzere **Get-HashFile** cmdlet 'ini kullanabilirsiniz.

| Dosya Adı | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL. vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL. vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS. vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1. vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

### <a name="failure-occurs-when-uploading-vm-image-in-the-vaasprereq-script"></a>`VaaSPreReq` Betikte VM görüntüsü karşıya yüklenirken hata oluştu

Öncelikle ortamın sağlıklı olup olmadığını denetleyin:

1. DVı/atbox kutusundan yönetici kimlik bilgilerini kullanarak yönetici portalında başarıyla oturum açabilistediğinizi kontrol edin.
1. Uyarı veya uyarı olmadığından emin olun.

Ortam sağlıklı ise, VaaS test çalıştırmaları için gereken 5 VM görüntüsünü el ile karşıya yükleyin:

1. Yönetim Portalı 'nda hizmet yöneticisi olarak oturum açın. Yönetim Portalı URL 'sini, ECE deposundan veya damga bilgi dosyanızda bulabilirsiniz. Yönergeler için bkz. [Ortam parametreleri](azure-stack-vaas-parameters.md#environment-parameters).
1. **Diğer hizmetler** > **kaynak sağlayıcıları** > işlemVM > **görüntülerini**seçin.
1. **VM görüntüleri** dikey penceresinin en üstündeki **+ Ekle** düğmesini seçin.
1. İlk VM görüntüsü için aşağıdaki alanların değerlerini değiştirin veya denetleyin:
    > [!IMPORTANT]
    > Mevcut Market öğesi için tüm varsayılanlar doğru değil.

    | Alan  | Value  |
    |---------|---------|
    | Yayımcı | MicrosoftWindowsServer |
    | Sunduğu | WindowsServer |
    | İşletim sistemi türü | Windows |
    | SKU | 2012-R2-Datacenter |
    | Version | 1.0.0 |
    | İşletim sistemi diski blob URI 'SI | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |

1. **Oluştur** düğmesini seçin.
1. Kalan VM görüntüleri için tekrarlayın.

Tüm 5 VM görüntülerinin özellikleri şunlardır:

| Yayımcı  | Sunduğu  | İşletim sistemi türü | SKU | Version | İşletim sistemi diski blob URI 'SI |
|---------|---------|---------|---------|---------|---------|
| MicrosoftWindowsServer| WindowsServer | Windows | 2012-R2-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-veri merkezi | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterFullBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter-Server-Core | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterCoreBYOL.vhd |
| Canonical | UbuntuServer | Linux | 14.04.3-LTS | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1404LTS.vhd |
| Canonical | UbuntuServer | Linux | 16.04-LTS | 16.04.20170811 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1604-20170619.1.vhd |

## <a name="next-steps"></a>Sonraki adımlar

- En son sürümlerde değişiklikler için [hizmet olarak doğrulama Için sürüm notlarını](azure-stack-vaas-release-notes.md) gözden geçirin.