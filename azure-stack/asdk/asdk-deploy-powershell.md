---
title: PowerShell kullanarak komut satırından ASDK dağıtma | Microsoft Docs
description: PowerShell kullanarak, ASDK 'yi komut satırından dağıtmayı öğrenin.
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
ms.custom: ''
ms.date: 05/06/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 5b517eec23950380bf5f0fc8febe717683960b65
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579109"
---
# <a name="deploy-asdk-from-the-command-line-using-powershell"></a>PowerShell kullanarak komut satırından ASDK dağıtma

Azure Stack Geliştirme Seti (ASDK), Azure Stack özellikleri ve hizmetleri değerlendirmek ve göstermek için dağıtabileceğiniz bir test ve geliştirme ortamıdır. Çalışır duruma getirmek ve çalıştırmak için ortam donanımını hazırlamanız ve bazı betikleri çalıştırmanız gerekir. Betikler çalıştırmak birkaç saat sürer. Bundan sonra, Azure Stack kullanmaya başlamak için yönetici ve Kullanıcı portalında oturum açabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

ASDK ana bilgisayarını hazırlayın. Donanımınızı, yazılımınızı ve ağınızı planlayın. ASDK 'yi barındıran bilgisayarın donanım, yazılım ve ağ gereksinimlerini karşılaması gerekir. Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanma arasında seçim yapın. Yükleme işleminin sorunsuz çalışması için dağıtımınıza başlamadan önce bu önkoşulları izlediğinizden emin olun.

ASDK 'yi dağıtmadan önce, planlı ASDK ana bilgisayar donanımınızın, işletim sisteminin, hesabın ve ağ yapılandırmalarının ASDK 'yi yüklemek için gereken en düşük gereksinimleri karşıladığından emin olun.

**[ASDK dağıtım gereksinimlerini ve önemli noktaları gözden geçirin](asdk-deploy-considerations.md)** .

> [!TIP]
> Donanımınızın tüm gereksinimleri karşıladığından emin olmak için işletim sistemini yükledikten sonra [Azure Stack dağıtım gereksinimleri denetim aracını](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) kullanabilirsiniz.

## <a name="download-and-extract-the-deployment-package"></a>Dağıtım paketini indir ve Ayıkla
ASDK ana bilgisayarınızın, ASDK 'yi yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra, bir sonraki adım ASDK dağıtım paketini indirip ayıklamanıza yardımcı olur. Dağıtım paketi, önyüklenebilir bir işletim sistemini ve Azure Stack yükleme dosyalarını içeren bir sanal sabit sürücü olan Cloudbuilder. vhdx dosyasını içerir.

Dağıtım paketini ASDK konağına veya başka bir bilgisayara indirebilirsiniz. Ayıklanan dağıtım dosyaları 60 GB boş disk alanı kaplar, bu nedenle başka bir bilgisayar kullanmak, ASDK konağının donanım gereksinimlerini azaltmaya yardımcı olabilir.

**[Azure Stack Geliştirme Seti indir ve Ayıkla (ASDK)](asdk-download.md)**

## <a name="prepare-the-asdk-host-computer"></a>ASDK ana bilgisayarını hazırlama
Ana bilgisayara ASDK yüklemeden önce, ortam hazırlanmalıdır ve sistem VHD 'den önyüklenecek şekilde yapılandırılır. Bu adımdan sonra, ASDK ana bilgisayarı Cloudbuilder. vhdx ' i (önyüklenebilir bir işletim sistemi ve Azure Stack yükleme dosyalarını içeren bir sanal sabit sürücü) önyükleyecek.

PowerShell kullanarak, ASDK ana bilgisayarını CloudBuilder. vhdx ' den önyüklenecek şekilde yapılandırın. Bu komutlar, ASDK ana bilgisayarınızı indirilen ve ayıklanan Azure Stack sanal sabit diskten (CloudBuilder. vhdx) önyüklenecek şekilde yapılandırır. Bu adımları tamamladıktan sonra, ASDK ana bilgisayarını yeniden başlatın.

ASDK konak bilgisayarını CloudBuilder. vhdx 'ten önyüklenecek şekilde yapılandırmak için:

  1. Yönetici olarak bir komut istemi başlatın.
  2. `bcdedit /copy {current} /d "Azure Stack"` öğesini çalıştırın.
  3. Kopyala (CTRL + C) gerekli süslü ayraç (`{}`) dahil olmak üzere döndürülen CLSID değeri. Bu değer olarak `{CLSID}` adlandırılır ve geri kalan adımlarda (Ctrl + V veya sağ tıklama) içine yapıştırılabilmesi gerekir.
  4. `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` öğesini çalıştırın.
  5. `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` öğesini çalıştırın.
  6. `bcdedit /set {CLSID} detecthal on` öğesini çalıştırın.
  7. `bcdedit /default {CLSID}` öğesini çalıştırın.
  8. Önyükleme ayarlarını doğrulamak için öğesini çalıştırın `bcdedit`.
  9. CloudBuilder. vhdx dosyasının C:\ ' un köküne taşındığından emin olun. Drive (`C:\CloudBuilder.vhdx`) ve asdk ana bilgisayarını yeniden başlatın. ASDK ana bilgisayar yeniden başlatıldığında, ASDK dağıtımına başlamak için CloudBuilder. VHDX sanal makinesi (VM) sabit sürücüsünden önyükleme yapılmalıdır.

> [!IMPORTANT]
> Yeniden başlatmadan önce, ASDK ana bilgisayar bilgisayarına doğrudan fiziksel veya KVM erişiminiz olduğundan emin olun. VM ilk başlatıldığında, Windows Server kurulumu 'Nu tamamlamanızı ister. ASDK ana bilgisayarında oturum açmak için kullandığınız yönetici kimlik bilgilerini sağlayın.

### <a name="prepare-the-asdk-host-using-powershell"></a>PowerShell kullanarak ASDK konağını hazırlama 
ASDK konak bilgisayarı CloudBuilder. vhdx görüntüsüne başarıyla önyüklendiğinde, ASDK ana bilgisayarında oturum açmak için kullandığınız aynı yerel yönetici kimlik bilgileriyle oturum açın. Bunlar aynı zamanda, ana bilgisayar VHD 'den önyüklendiğinde Windows Server kurulumu 'nu tamamlamanın bir parçası olarak girdiğiniz kimlik bilgileridir.

> [!NOTE]
> İsteğe bağlı olarak, ASDK 'yi yüklemeden *önce* [Azure Stack telemetri ayarlarını](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) da yapılandırabilirsiniz.

Yükseltilmiş bir PowerShell konsolu açın ve bu bölümdeki komutları çalıştırarak asdk 'yi ASDK konağına dağıtın.

> [!IMPORTANT]
> ASDK yüklemesi ağ için tam olarak bir ağ arabirim kartını (NIC) destekler. Birden çok NIC varsa dağıtım betiğini çalıştırmadan önce yalnızca bir tane etkinleştirildiğinden (ve diğerlerinin tümünün devre dışı bırakıldığından) emin olun.

Azure Stack, kimlik sağlayıcısı olarak Azure AD veya Windows Server AD FS dağıtabilirsiniz. Azure Stack, kaynak sağlayıcıları ve diğer uygulamalar her ikisiyle de aynı şekilde çalışır.

> [!TIP]
> Herhangi bir kurulum parametresi belirtmezseniz (bkz. ınstallazurestackpoc. ps1 isteğe bağlı parametreleri ve örnekleri aşağıda verilmiştir), sizden gerekli parametreler istenir.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure AD kullanarak Azure Stack dağıtma 
**Kimlik sağlayıcısı olarak Azure AD 'yi kullanarak**Azure Stack dağıtmak için doğrudan veya saydam bir ara sunucu üzerinden İnternet bağlantınız olması gerekir. 

Azure AD 'yi kullanarak ASDK 'yi dağıtmak için aşağıdaki PowerShell komutlarını çalıştırın:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

Azure AD kimlik bilgileri için birkaç dakika ASDK yüklemesine sorulur. Azure AD kiracınız için genel yönetici kimlik bilgilerini sağlayın.

Dağıtımdan sonra, Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler genel yönetici kimlik bilgileri gerektirebilir. Bu tür işlemlere örnek olarak bir kaynak sağlayıcısı yükleyici betiği veya izin verilmesi gereken yeni bir özellik dahildir. Hesabın genel yönetici izinlerini geçici olarak yeniden başlatabilir ya da *varsayılan sağlayıcı aboneliğinin*sahibi olan ayrı bir genel yönetici hesabı kullanabilirsiniz.

### <a name="deploy-azure-stack-using-ad-fs"></a>AD FS kullanarak Azure Stack dağıtma 
**Kimlik sağlayıcısı olarak AD FS kullanarak**asdk 'yi dağıtmak Için aşağıdaki PowerShell komutlarını çalıştırın (yalnızca-useadfs parametresini eklemeniz gerekir):

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

AD FS dağıtımlarda, varsayılan damga dizin hizmeti kimlik sağlayıcısı olarak kullanılır. İle azurestackadmin@azurestack.localoturum açmak için varsayılan hesap, ve parola PowerShell kurulum komutlarının bir parçası olarak sağladıklarınız olarak ayarlanır.

Dağıtım işlemi birkaç saat sürebilir ve bu süre, sistemin otomatik olarak bir kez yeniden başlatılır. Dağıtım başarılı olduğunda, PowerShell konsolu şunları görüntüler: **TAMAM ' Dağıtım '** eylemi. Dağıtım başarısız olursa,-yeniden çalıştır parametresini kullanarak betiği yeniden çalıştırmayı deneyin. Ya da, [ASDK](asdk-redeploy.md) 'yi sıfırdan yeniden dağıtabilirsiniz.

> [!IMPORTANT]
> ASDK ana bilgisayarı yeniden başlatıldıktan sonra dağıtım ilerlemesini izlemek isterseniz, AzureStack\AzureStackAdmin. olarak oturum açmanız gerekir Ana bilgisayar yeniden başlatıldıktan sonra yerel yönetici olarak oturum açarsanız (ve azurestack. local etki alanına katıldıktan), dağıtım ilerleme durumunu görmezsiniz. Dağıtımı yeniden çalıştırmayın, kurulumun çalıştığını doğrulamak için yerel yönetici ile aynı parolayla AzureStack\AzureStackAdmin olarak oturum açın.


#### <a name="azure-ad-deployment-script-examples"></a>Azure AD Dağıtım betiği örnekleri
Tüm Azure AD dağıtımı için komut dosyası oluşturabilirsiniz. Aşağıda, bazı isteğe bağlı parametreleri içeren bazı yorum örnekleri verilmiştir.

Azure AD Kimliğiniz yalnızca **bir** Azure AD diziniyle ilişkiliyse:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Azure AD kimliğiniz, birden **fazla** Azure AD diziniyle ilişkiliyse:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Ortamınızda DHCP etkin değilse, yukarıdaki seçeneklerden birine aşağıdaki ek parametreleri eklemeniz gerekir (örnek kullanım verilmiştir): 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK ınstallazurestackpoc. ps1 isteğe bağlı parametreler

|Parametre|Gerekli/Isteğe bağlı|Açıklama|
|-----|-----|-----|
|AdminPassword|Gerekli|Yerel yönetici hesabını ve tüm diğer Kullanıcı hesaplarını, ASDK dağıtımının bir parçası olarak oluşturulan tüm VM 'lerde ayarlar. Bu parolanın konaktaki geçerli yerel yönetici parolasıyla eşleşmesi gerekir.|
|InfraAzureDirectoryTenantName|Gerekli|Kiracı dizinini ayarlar. AAD hesabının birden çok dizini yönetme izinleri olduğu belirli bir dizini belirtmek için bu parametreyi kullanın. . Onmicrosoft.com biçiminde bir AAD kiracının tam adı veya bir Azure AD tarafından doğrulanan özel etki alanı adı.|
|TimeServer|Gerekli|Belirli bir saat sunucusunu belirtmek için bu parametreyi kullanın. Bu parametrenin geçerli bir saat sunucusu IP adresi olarak sağlanması gerekir. Sunucu adları desteklenmez.|
|InfraAzureDirectoryTenantAdminCredential|İsteğe Bağlı|Azure Active Directory Kullanıcı adı ve parolayı ayarlar. Bu Azure kimlik bilgileri bir kuruluş KIMLIĞI olmalıdır.|
|InfraAzureEnvironment|İsteğe Bağlı|Bu Azure Stack dağıtımını kaydetmek istediğiniz Azure ortamını seçin. Seçenekler genel Azure, Azure-Çin, Azure-US kamu ' u içerir.|
|DNSForwarder|İsteğe Bağlı|Azure Stack dağıtımının bir parçası olarak bir DNS sunucusu oluşturulur. Çözümdeki bilgisayarların damga dışında adları çözümlemesine izin vermek için, mevcut altyapı DNS sunucunuzu sağlayın. Damga içi DNS sunucusu bilinmeyen ad çözümleme isteklerini bu sunucuya iletir.|
|Yeniden Çalıştır|İsteğe Bağlı|Dağıtımı yeniden çalıştırmak için bu bayrağı kullanın. Önceki tüm girişler kullanılır. Birden çok benzersiz değer oluşturulması ve dağıtım için kullanılması gerektiği için önceden sağlanmış verilerin yeniden girilmesi desteklenmez.|


## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırma gerçekleştirme
ASDK 'yi yükledikten sonra, birkaç önerilen yükleme sonrası denetim ve yapılması gereken yapılandırma değişiklikleri vardır. Test-AzureStack cmdlet 'ini kullanarak yüklemenizin başarıyla yüklendiğini doğrulayın, ardından Azure Stack PowerShell ve GitHub araçları 'nı yükleme.

Deneme süreniz sona ermeden önce, ASDK konağının parolasının süresinin dolmamasını sağlamak için parola süre sonu ilkesini sıfırlamanıza önerilir.

> [!NOTE]
> İsteğe bağlı olarak, ASDK 'yi yükledikten *sonra* [Azure Stack telemetri ayarlarını](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) da yapılandırabilirsiniz.

**[ASDK dağıtım görevleri sonrası](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure 'a kaydolun
[Azure Market öğelerini](../operator/azure-stack-create-and-publish-marketplace-item.md) Azure Stack indirebilmeniz Için Azure Stack Azure ile kaydetmeniz gerekir.

**[Azure ile Azure Stack kaydetme](asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Bu adımları tamamladıktan sonra [yönetici](https://adminportal.local.azurestack.external) ve [Kullanıcı](https://portal.local.azurestack.external) PORTALLARıNA sahip bir asdk ortamınız olacaktır. 

[ASDK yükleme yapılandırma görevlerinin sonrası](asdk-post-deploy.md)

