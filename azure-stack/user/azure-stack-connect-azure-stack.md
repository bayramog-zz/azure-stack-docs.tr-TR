---
title: Azure Stack Bağlan | Microsoft Docs
description: Azure Stack nasıl bağlanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 90759186b008c41bd9b3e35bf368d4f819534823
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975059"
---
# <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Kaynakları yönetmek için Azure Stack Geliştirme Seti bağlanmanız gerekir. Bu makalede, geliştirme paketine bağlanmak için gereken adımlar ayrıntılı olarak açıklanır. Aşağıdaki bağlantı seçeneklerinden birini kullanabilirsiniz:

* Uzak Masaüstü: tek bir eşzamanlı kullanıcının geliştirme setinden hızlıca bağlanmasına izin verir.
* Sanal özel ağ (VPN): birden çok eş zamanlı kullanıcının Azure Stack altyapısının dışındaki istemcilerden bağlanmasına izin verir (yapılandırma gerektirir).

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Uzak Masaüstü ile Azure Stack bağlanma
Uzak Masaüstü Bağlantısı, tek bir eşzamanlı kullanıcı, kaynakları yönetmek için portalla birlikte çalışabilir.

1. Bir Uzak Masaüstü Bağlantısı açın ve geliştirme paketine bağlanın. Kullanıcı adı olarak **AzureStack\AzureStackAdmin** girin ve Azure Stack kurulumu sırasında verdiğiniz yönetici parolasını girin.  

2. Geliştirme Seti bilgisayarından Sunucu Yöneticisi açın, **yerel sunucu**' ya tıklayın, Internet Explorer Artırılmış Güvenlik ' i kapatın ve sonra Sunucu Yöneticisi kapatın.

3. Portalı açmak için bölümüne gidin (https://portal.local.azurestack.external/) ve Kullanıcı kimlik bilgilerini kullanarak oturum açın.


## <a name="connect-to-azure-stack-with-vpn"></a>VPN ile Azure Stack bağlanma

Bir Azure Stack Geliştirme Seti bölünmüş tünel VPN bağlantısı kurabilirsiniz. VPN bağlantısı aracılığıyla, Azure Stack kaynaklarını yönetmek için yönetici portalına, Kullanıcı portalına ve Visual Studio ve PowerShell gibi yerel olarak yüklenmiş araçlara erişebilirsiniz. VPN bağlantısı hem Azure Active Directory (AAD) hem de Active Directory Federasyon Hizmetleri (AD FS) (AD FS) tabanlı dağıtımlarda desteklenir. VPN bağlantıları birden çok istemcinin aynı anda Azure Stack bağlanmasına izin verir. 

> [!NOTE] 
> Bu VPN bağlantısı, Azure Stack altyapı VM 'lerine bağlantı sağlamaz. 

### <a name="prerequisites"></a>Önkoşullar

* [Azure Stack uyumlu Azure PowerShell](../operator/azure-stack-powershell-install.md) yerel bilgisayarınıza yükler.  
* İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md). 

### <a name="configure-vpn-connectivity"></a>VPN bağlantısı yapılandırma

Geliştirme Seti 'ne bir VPN bağlantısı oluşturmak için, yerel Windows tabanlı bilgisayarınızdan yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki betiği çalıştırın (ortamınız için IP adresi ve parola değerlerini güncelleştirdiğinizden emin olun):

```powershell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer's host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the IP address and password values for your environment. 

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<Administrator password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Kurulum başarılı olursa, VPN bağlantıları listenizde görürsünüz `azurestack` .

![Ağ bağlantıları](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Aşağıdaki iki yöntemden birini kullanarak Azure Stack örneğine bağlanın:  

* Şu `Connect-AzsVpn` komutu kullanarak: 
    
  ```powershell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure Stack konağına güvenin ve sertifikayı **Azurestackcertificateauthority** konumundan yerel bilgisayarınızın sertifika deposuna yüklersiniz. İstem, PowerShell oturum penceresinin arkasında görünebilir. 

* Yerel bilgisayarınızda **ağ ayarları** > **VPN** ' ye gidin > **Bağlan**' ı `azurestack`seçin.  >  Oturum açma isteminde Kullanıcı adını (AzureStack\AzureStackAdmin) ve parolayı girin.

### <a name="test-the-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantısını test etmek için bir tarayıcı açın ve Kullanıcı portalına gidin (https://portal.local.azurestack.external/), oturum açın ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar



