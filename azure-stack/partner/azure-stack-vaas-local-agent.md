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
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b1a658b428d13cdd12c16b767430f87a80e89fdc
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418375"
---
# <a name="deploy-the-local-agent"></a>Yerel aracıyı dağıtma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Doğrulama testlerini çalıştırmak için hizmet olarak doğrulama (VaaS) yerel aracısını kullanmayı öğrenin. Doğrulama testlerini çalıştırmadan önce yerel aracısının dağıtılması gerekir.

> [!Note]  
> Yerel aracısının çalıştırıldığı makinenin internet 'e giden erişimi kaybetmediğinden emin olun. Bu makineye yalnızca kiracınız adına VaaS kullanma yetkisine sahip olan kullanıcılar erişilebilir olmalıdır.

Yerel aracıyı dağıtmak için:

1. Yerel aracıyı yükler.
2. Sağlamlık denetimleri yapın.
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

### <a name="download-and-install-the-agent"></a>Aracısını indirme ve yükleme

1. Testleri çalıştırmak için kullanacağınız makinede, yükseltilmiş bir istemde Windows PowerShell 'i açın.
2. Yerel aracıyı indirmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "OnPremAgent.zip"
    Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent -Force
    Set-Location VaaSOnPremAgent\lib\net46
    ```

3. Yerel Aracı bağımlılıklarını yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

    **Parametreler**

    | Parametre | Açıklama |
    | --- | --- |
    | aadServiceAdminUser | Azure AD kiracınız için genel yönetici kullanıcı. Örneğin, vaasadmin@contoso.onmicrosoft.comolabilir. |
    | aadServiceAdminPassword | Genel yönetici kullanıcısının parolası. |
    | AadTenantId | Hizmet olarak doğrulamaya kayıtlı Azure hesabı için Azure AD kiracı KIMLIĞI. |
    | ExternalFqdn | Yapılandırma dosyasından tam etki alanı adını alabilirsiniz. Yönergeler için bkz. [hizmet olarak Azure Stack doğrulamasında Iş akışı ortak parametreleri](azure-stack-vaas-parameters.md). |
    | Bölge | Azure AD kiracınızın bölgesi. |

Komutu ortak görüntü deposu (PIR) görüntüsünü indirir ve bir Azure Blob depolama alanından Azure Stack depolama alanına kopyalar.

![Önkoşulları indir](media/installingprereqs.png)

> [!Note]
> Bu görüntüleri indirirken yavaş ağ hızına karşılaşıyorsanız, bunları yerel bir paylaşıma ayrı olarak indirin ve **-LocalPackagePath** *fileshareorlocalpath*parametresini belirtin. [Bir hizmet olarak doğrulama sorunlarını gidermek](azure-stack-vaas-troubleshoot.md)için [yavaş ağ bağlantısını işleme](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) bölümünde PIR indirilebilirliğinizi daha fazla rehberlik bulabilirsiniz.

## <a name="checks-before-starting-the-tests"></a>Testleri başlatmadan önce denetimler

Testler uzak işlemleri çalıştırır. Testleri çalıştıran makinenin Azure Stack uç noktalarına erişimi olması gerekir, aksi takdirde testler çalışmaz. VaaS yerel aracısını kullanıyorsanız, aracının çalıştırılacağı makineyi kullanın. Aşağıdaki denetimleri çalıştırarak makinenizin Azure Stack uç noktalara erişimi olup olmadığını kontrol edebilirsiniz:

1. Taban URI 'sine ulaşılabildiğini denetleyin. Bir komut istemi veya bash kabuğu açın ve aşağıdaki komutu çalıştırarak ortamınızın dış FQDN 'si `<EXTERNALFQDN>` ile değiştirin:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Bir Web tarayıcısı açın ve bu portala `https://adminportal.<EXTERNALFQDN>` gidip gelmediğini denetlemek için adresine gidin.

3. Test geçişi oluştururken belirtilen Azure AD hizmet yöneticisi adı ve parola değerlerini kullanarak oturum açın.

4. [Azure Stack için doğrulama testi çalıştırma](../operator/azure-stack-diagnostic-test.md)bölümünde açıklandığı gibi **Test-azurestack** PowerShell cmdlet 'ini çalıştırarak sistemin sistem durumunu kontrol edin. Herhangi bir testi başlatmadan önce tüm uyarıları ve hataları düzeltin.

## <a name="run-the-agent"></a>Aracıyı çalıştırma

1. Windows PowerShell 'i yükseltilmiş bir istemde açın.

2. Şu komutu çalıştırın:

    ```powershell
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ```

      **Parametreler**  

    | Parametre | Açıklama |
    | --- | --- |
    | Vaasuserıd | Vaas portalında oturum açmak için kullanılan Kullanıcı kimliği (örneğin, Kullanıcı adı\@contoso.com) |
    | VaaSTenantId | Hizmet olarak doğrulamaya kayıtlı Azure hesabı için Azure AD kiracı KIMLIĞI. |

    > [!Note]  
    > Aracıyı çalıştırdığınızda geçerli çalışma dizini, **Microsoft. Vaasonpred. TaskEngineHost. exe** görev altyapısı ana bilgisayar yürütülebilir dosyasının konumu olmalıdır.

Bildirilen bir hata görmüyorsanız, yerel aracı başarılı olmuştur. Aşağıdaki örnek metin konsol penceresinde görünür.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Aracı başlatıldı](media/startedagent.png)

Bir aracı adıyla benzersiz olarak tanımlanır. Varsayılan olarak, başlatıldığı yerden makinenin tam etki alanı adı (FQDN) adını kullanır. Odağı değiştirmek diğer tüm işlemleri durakladıkça pencerede yanlışlıkla seçim yapmadan kaçınmak için pencereyi en aza indirmiş olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md)
- [Hızlı Başlangıç: İlk testinizi zamanlamak için bir hizmet portalı olarak doğrulamayı kullanın](azure-stack-vaas-schedule-test-pass.md)