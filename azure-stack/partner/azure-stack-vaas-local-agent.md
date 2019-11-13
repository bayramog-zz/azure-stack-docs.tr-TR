---
title: Yerel aracıyı dağıtma | Microsoft Docs
description: Azure Stack doğrulaması için yerel aracıyı hizmet olarak dağıtın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 9cd13f16df804c674c2901ae0e167e47294f176a
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955891"
---
# <a name="deploy-the-local-agent"></a>Yerel aracıyı dağıtma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama testlerini çalıştırmak için hizmet olarak doğrulama (VaaS) yerel aracısını kullanmayı öğrenin. Doğrulama testlerini çalıştırmadan önce yerel aracısının dağıtılması gerekir.

> [!Note]  
> Yerel aracısının çalıştırıldığı makinenin internet 'e giden erişimi kaybetmediğinden emin olun. Bu makineye yalnızca kiracınız adına VaaS kullanma yetkisine sahip olan kullanıcılar erişilebilir olmalıdır.

Yerel aracıyı dağıtmak için:

1. Yerel aracıyı indirip yükleyin.
2. Testleri başlatmadan önce sağlamlık denetimleri yapın.
3. Yerel aracıyı çalıştırın.

## <a name="download-and-start-the-local-agent"></a>Yerel aracıyı indir ve Başlat

Aracıyı, veri merkezinizdeki önkoşulları karşılayan ve tüm Azure Stack uç noktalarına erişebilen bir makineye indirin. Bu makine Azure Stack sisteminin bir parçası olmamalıdır veya Azure Stack bulutta barındırılmalıdır.

### <a name="machine-prerequisites"></a>Makine önkoşulları

Makinenizin aşağıdaki ölçütleri karşıladığından emin olun:

- Tüm Azure Stack uç noktalarına erişim
- .NET 4,6 ve PowerShell 5,0 yüklendi
- En az 8 GB RAM
- En az 8 çekirdek işlemci
- Minimum 200 GB disk alanı
- İnternet 'e yönelik kararlı ağ bağlantısı

### <a name="download-and-install-the-local-agent"></a>Yerel aracıyı indirme ve yükleme

1. Testleri çalıştırmak için kullanacağınız makinede, yükseltilmiş bir istemde Windows PowerShell 'i açın.
2. Yerel Aracı bağımlılıklarını indirip yüklemek ve ortak görüntü deposu (PIR) görüntülerini (OS VHD) Azure Stack ortamınıza kopyalamak için aşağıdaki komutu çalıştırın.

    ```powershell
    # Review and update the following five parameters
    $RootFolder = "c:\VaaS"
    $CloudAdmindUserName = "<Cloud admin user name>"
    $CloudAdminPassword = "<Cloud admin password>"
    $AadServiceAdminUserName = "<AAD service admin user name>"
    $AadServiceAdminPassword = "<AAD service admin password>"

    if (-not(Test-Path($RootFolder))) {
        mkdir $RootFolder
    }
    Set-Location $RootFolder
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "$rootFolder\OnPremAgent.zip"
    Expand-Archive -Path "$rootFolder\OnPremAgent.zip" -DestinationPath "$rootFolder\VaaSOnPremAgent" -Force
    Set-Location "$rootFolder\VaaSOnPremAgent\lib\net46"

    $cloudAdminCredential = New-Object System.Management.Automation.PSCredential($cloudAdmindUserName, (ConvertTo-SecureString $cloudAdminPassword -AsPlainText -Force))
    $getStampInfoUri = "https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation" 
    $stampInfo = Invoke-RestMethod -Method Get -Uri $getStampInfoUri -Credential $cloudAdminCredential -ErrorAction Stop
    $serviceAdminCreds = New-Object System.Management.Automation.PSCredential $aadServiceAdminUserName, (ConvertTo-SecureString $aadServiceAdminPassword -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $stampInfo.AADTenantID `
                            -ServiceAdminCreds $serviceAdminCreds `
                            -ArmEndpoint $stampInfo.AdminExternalEndpoints.AdminResourceManager `
                            -Region $stampInfo.RegionName
    ```

> [!Note]  
> Install-Vaaspyeniden yönlendirme cmdlet 'i büyük VM görüntü dosyalarını indirir. Yavaş ağ hızına karşılaşıyorsanız, dosyaları yerel dosya sunucunuza indirebilir ve test environemnt 'nize el ile sanal makine görüntüleri ekleyebilirsiniz. Daha fazla bilgi için bkz. [yavaş ağ bağlantısını işleme](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) .

**Parametreler**

| Parametre | Açıklama |
| --- | --- |
| AadServiceAdminUser | Azure AD kiracınız için genel yönetici kullanıcı. Örneğin, vaasadmin@contoso.onmicrosoft.comolabilir. |
| AadServiceAdminPassword | Genel yönetici kullanıcısının parolası. |
| CloudAdminUserName | Ayrıcalıklı uç nokta dahilinde izin verilen komutlara erişebilen ve çalıştıran bulut Yöneticisi Kullanıcı. Örneğin, AzusreStack\CloudAdmin. olabilir Daha fazla bilgi için [buraya](azure-stack-vaas-parameters.md) bakın. |
| CloudAdminPassword | Bulut Yöneticisi hesabı için parola.|

![Önkoşulları indir](media/installing-prereqs.png)

## <a name="perform-sanity-checks-before-starting-the-tests"></a>Testleri başlatmadan önce sağlamlık denetimleri gerçekleştirme

Testler uzak işlemleri çalıştırır. Testleri çalıştıran makinenin Azure Stack uç noktalarına erişimi olması gerekir, aksi takdirde testler çalışmaz. VaaS yerel aracısını kullanıyorsanız, aracının çalıştırılacağı makineyi kullanın. Aşağıdaki denetimleri çalıştırarak makinenizin Azure Stack uç noktalara erişimi olup olmadığını kontrol edebilirsiniz:

1. Taban URI 'sine ulaşılabildiğini denetleyin. Bir komut istemi veya bash kabuğu açın ve aşağıdaki komutu çalıştırarak `<EXTERNALFQDN>`, ortamınızın dış FQDN 'siyle değiştirin:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Bir Web tarayıcısı açın ve ma portalına ulaşılmadığını denetlemek için `https://adminportal.<EXTERNALFQDN>` gidin.

3. Test geçişi oluştururken belirtilen Azure AD hizmet yöneticisi adı ve parola değerlerini kullanarak oturum açın.

4. [Azure Stack için doğrulama testi çalıştırma](../operator/azure-stack-diagnostic-test.md)bölümünde açıklandığı gibi **Test-azurestack** PowerShell cmdlet 'ini çalıştırarak sistemin sistem durumunu kontrol edin. Herhangi bir testi başlatmadan önce tüm uyarıları ve hataları düzeltin.

## <a name="run-the-local-agent"></a>Yerel aracıyı Çalıştır

1. Windows PowerShell 'i yükseltilmiş bir istemde açın.

2. Şu komutu çalıştırın:

    ```powershell
   # Review and update the following five parameters
    $RootFolder = "c:\VAAS"
    $CloudAdmindUserName = "<Cloud admin user name>"
    $CloudAdminPassword = "<Cloud admin password>"
    $VaaSUserId = "<VaaS user ID>"
    $VaaSTenantId = "<VaaS tenant ID>"

    Set-Location "$rootFolder\VaaSOnPremAgent\lib\net46"
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u $VaaSUserId -t $VaaSTenantId -x $CloudAdmindUserName -y $CloudAdminPassword
    ```

      **Parametreler**  

    | Parametre | Açıklama |
    | --- | --- |
    | CloudAdminUserName | Ayrıcalıklı uç nokta dahilinde izin verilen komutlara erişebilen ve çalıştıran bulut Yöneticisi Kullanıcı. Örneğin, AzusreStack\CloudAdmin. olabilir Daha fazla bilgi için [buraya](azure-stack-vaas-parameters.md) bakın. |
    | CloudAdminPassword | Bulut Yöneticisi hesabı için parola.|
    | Vaasuserıd | VaaS portalında oturum açmak için kullanılan Kullanıcı KIMLIĞI (örneğin, Kullanıcı adı\@Contoso.com) |
    | VaaSTenantId | Hizmet olarak doğrulamaya kayıtlı Azure hesabı için Azure AD kiracı KIMLIĞI. |

    > [!Note]  
    > Aracıyı çalıştırdığınızda geçerli çalışma dizini, **Microsoft. Vaasonpred. TaskEngineHost. exe** görev altyapısı ana bilgisayar yürütülebilir dosyasının konumu olmalıdır.

Bildirilen bir hata görmüyorsanız, yerel aracı başarılı olmuştur. Aşağıdaki örnek metin konsol penceresinde görünür.

`Heartbeat was sent successfully.`

![Aracı başlatıldı](media/started-agent.png)

Bir aracı adıyla benzersiz olarak tanımlanır. Varsayılan olarak, başlatıldığı yerden makinenin tam etki alanı adı (FQDN) adını kullanır. Odağı değiştirmek diğer tüm işlemleri durakladıkça pencerede yanlışlıkla seçim yapmadan kaçınmak için pencereyi en aza indirmiş olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md)
- [Hızlı başlangıç: ilk testinizi zamanlamak için bir hizmet portalı olarak doğrulamayı kullanın](azure-stack-vaas-schedule-test-pass.md)