---
title: ASDK 'ye Bağlan | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) ' e nasıl bağlanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/06/2019
ms.author: justinha
ms.reviewer: knithinc
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 3f1dd0d7efa2aa9549f65ef97ebb7a589949154b
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579145"
---
# <a name="connect-to-the-asdk"></a>ASDK 'ye bağlanma

Kaynakları yönetmek için önce Azure Stack Geliştirme Seti (ASDK) ' e bağlanmanız gerekir. Bu makalede, aşağıdaki bağlantı seçeneklerini kullanarak ASDK 'e bağlanmak için yaptığınız adımları açıklıyoruz:

* [Uzak Masaüstü bağlantısı (RDP)](#connect-with-rdp): Uzak Masaüstü Bağlantısı kullanarak bağlandığınızda, tek bir kullanıcı hızlı bir şekilde ASDK 'ye bağlanabilir.
* [Sanal özel ağ (VPN)](#connect-with-vpn): VPN kullanarak bağlandığınızda, birden fazla Kullanıcı Azure Stack altyapısının dışındaki istemcilerden Azure Stack portallara eşzamanlı olarak bağlanabilir. Bir VPN bağlantısı, bazı kurulum gerektirir.

<a name="connect-with-rdp"></a>
## <a name="connect-to-azure-stack-using-rdp"></a>RDP kullanarak Azure Stack bağlanma

Tek bir eşzamanlı kullanıcı Azure Stack yönetici portalındaki veya Kullanıcı portalındaki kaynakları doğrudan ASDK ana bilgisayarından Uzak Masaüstü Bağlantısı aracılığıyla yönetebilir.

> [!TIP]
> Bu seçenek ayrıca, asdk ana bilgisayarında oluşturulan sanal makinelerde (VM) oturum açmak için, ASDK ana bilgisayarında oturum açarken RDP 'yi yeniden kullanmanızı da sağlar.

1. Uzak Masaüstü Bağlantısı (mstc. exe) açın ve ASDK ana bilgisayar bilgisayarı IP adresine bağlanın. ASDK ana bilgisayarında uzaktan oturum açmak için yetkilendirilmiş bir hesap kullandığınızdan emin olun. Varsayılan olarak, **AzureStack\AzureStackAdmin** , asdk ana bilgisayarında uzaktan oturum açmaya yönelik izinlere sahiptir.  

2. ASDK ana bilgisayarında Sunucu Yöneticisi açın (ServerManager. exe). **Yerel sunucu**' yı seçin, **IE artırılmış güvenlik yapılandırmasını**kapatın ve Sunucu Yöneticisi kapatın.

3. **AzureStack\CloudAdmin** olarak yönetici portalında oturum açın veya diğer Azure Stack operatör kimlik bilgilerini kullanın. ASDK Yönetici portalı adresi [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).

4. Kullanıcı portalında **AzureStack\CloudAdmin** olarak oturum açın veya diğer Azure Stack Kullanıcı kimlik bilgilerini kullanın. ASDK Kullanıcı Portalı adresi [https://portal.local.azurestack.external](https://portal.local.azurestack.external).

> [!NOTE]
> Hangi hesabın kullanılacağı hakkında daha fazla bilgi için bkz. [asdk yönetici temelleri](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-with-vpn"></a>
## <a name="connect-to-azure-stack-using-vpn"></a>VPN kullanarak Azure Stack bağlanma

Azure Stack portallarına ve Visual Studio ve PowerShell gibi yerel olarak yüklenmiş araçlara erişmek için bir ASDK ana bilgisayarına bölünmüş bir tünel VPN bağlantısı kurabilirsiniz. VPN bağlantıları kullanarak, çok sayıda kullanıcı, ASDK tarafından barındırılan kaynakları Azure Stack için aynı anda bağlanabilir.

VPN bağlantısı, hem Azure AD hem de Active Directory Federasyon Hizmetleri (AD FS) (AD FS) dağıtımları için desteklenir.

> [!NOTE]
> VPN bağlantısı *,* Azure Stack VM 'lere bağlantı sağlamaz. VPN ile bağlıyken Azure Stack sanal makinelere RDP yükleyemezsiniz.

### <a name="prerequisites"></a>Önkoşullar
ASDK 'ye bir VPN bağlantısı ayarlamadan önce, aşağıdaki önkoşulları karşıladığınızı doğrulayın:

- [Azure Stack uyumlu Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) yerel bilgisayarınıza yükler.  
- İndirme [Azure Stack ile çalışması için gereken araçları](asdk-post-deploy.md#download-the-azure-stack-tools).

### <a name="set-up-vpn-connectivity"></a>VPN bağlantısını ayarlama

ASDK 'ye bir VPN bağlantısı oluşturmak için PowerShell 'i yerel Windows tabanlı bilgisayarınızda yönetici olarak açın. Ardından, aşağıdaki betiği çalıştırın (ortamınız için IP adresi ve parola değerlerini güncelleştirin):

```powershell
# Change directories to the default Azure Stack tools directory
cd C:\AzureStack-Tools-master

# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the ASDK host computer's IP address as the ASDK certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Kurulum başarılı olursa VPN bağlantıları listenizde **Azure Stack** görüntülenir:

![Ağ bağlantıları](media/asdk-connect/vpn.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

  Aşağıdaki yöntemlerden birini kullanarak Azure Stack örneğine bağlanın:  

  * `Connect-AzsVpn` Şu komutu kullanın:
      
    ```powershell
    Connect-AzsVpn `
      -Password $Password
    ```

  * Yerel bilgisayarınızda, **ağ ayarları** > **VPN** > **Azure Stack** > **Bağlan**' ı seçin. Oturum açma isteminde Kullanıcı adını (**AzureStack\AzureStackAdmin**) ve parolanızı girin.

İlk kez bağlandığınızda, yerel bilgisayarınızın sertifika deposuna **Azurestackcertificateauthority** konumundan Azure Stack kök sertifikayı yüklemeniz istenecektir. Bu adım, ASDK sertifika yetkilisini (CA) güvenilen konaklar listesine ekler. Sertifikayı yüklemek için **Evet** ' e tıklayın.

![Kök sertifika](media/asdk-connect/cert.png)  
  
  > [!IMPORTANT]
  > İstem, PowerShell penceresi veya diğer uygulamalar tarafından gizlenmiş olabilir.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını sına

Portal bağlantısını test etmek için bir tarayıcı açın ve ardından Kullanıcı portalına (https://portal.local.azurestack.external/) veya yönetim portalı 'na (https://adminportal.local.azurestack.external/) ) gidin. 

Kaynakları oluşturmak ve yönetmek için uygun abonelik kimlik bilgileriyle oturum açın.  

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme](asdk-troubleshooting.md)
