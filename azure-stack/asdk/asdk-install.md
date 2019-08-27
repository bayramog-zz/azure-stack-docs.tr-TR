---
title: ASDK 'yi yükler | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) yüklemeyi öğrenin.
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
ms.date: 05/06/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: c616736cf9ea8cc350c2c53bb7d6994346dac990
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025964"
---
# <a name="install-the-asdk"></a>ASDK 'yi yükler
[Asdk ana bilgisayar bilgisayarı hazırlandıktan](asdk-prepare-host.md)sonra, bu makaledeki aşağıdaki adımları kullanarak cloudbuilder. vhdx resmine Azure Stack geliştirme seti (asdk) dağıtılabilir.

## <a name="install-the-asdk"></a>ASDK 'yi yükler
Bu makaledeki adımlarda, **asdk-installer. ps1** PowerShell betiğini indirip çalıştırarak, bir grafik kullanıcı ARABIRIMI (GUI) kullanarak asdk dağıtma işlemi gösterilmektedir.

> [!NOTE]
> ASDK için yükleyici Kullanıcı arabirimi, WCF ve PowerShell tabanlı açık kaynaklı bir betiktir.


1. Ana bilgisayar CloudBuilder. vhdx görüntüsüne başarıyla önyüklendiğinde, asdk [ana bilgisayarını](asdk-prepare-host.md) asdk yüklemesi için hazırladığınız sırada belirtilen yönetici kimlik bilgilerini kullanarak oturum açın. Bu, ASDK ana bilgisayar yerel yönetici kimlik bilgileriyle aynı olmalıdır.
2. Yükseltilmiş bir PowerShell konsolu açın ve  **&lt;sürücü harfini > \azurestack_ınstaller\asdk-Installer.exe** adlı PowerShell betiği çalıştırın. Betiğinizin artık C:\ ' dan farklı bir sürücüde olabileceğini unutmayın. CloudBuilder. vhdx görüntüsünde. **Yükle**'ye tıklatın.

    ![ASDK 'yi yükler](media/asdk-install/1.PNG) 

3. Kimlik sağlayıcısı **türü** açılan kutusunda **Azure Çin bulutu**, **azure ABD kamu bulutu**, **AD FS**veya **Azure bulutu**' nı seçin. **Yerel yönetici parolası** altında, **parola** kutusunda yerel yönetici parolasını (yapılandırılmış geçerli yerel yönetici parolasıyla eşleşmesi gerekir) yazın ve ardından **İleri**' ye tıklayın.

    ![ASDK 'de kimlik sağlayıcısı türü açılan eklentisi](media/asdk-install/2.PNG) 
  
    Bir Azure aboneliği kimlik sağlayıcısı seçerseniz, bir internet bağlantısı, bir Azure AD dizin kiracının *DomainName*. onmicrosoft.com biçiminde tam adı veya BIR Azure AD tarafından doğrulanan özel etki alanı adı gerekir. Ayrıca, belirtilen dizin için genel yönetici kimlik bilgilerine de ihtiyacınız vardır.

    Dağıtımdan sonra, Azure Active Directory (AAD) genel yönetici izni gerekli değildir. Ancak, bazı işlemler genel yönetici kimlik bilgileri gerektirebilir. Örneğin, kaynak sağlayıcısı yükleyici betiği veya izin verilmesi gereken yeni bir özellik. Hesabın genel yönetici izinlerini geçici olarak yeniden başlatabilir ya da *varsayılan sağlayıcı aboneliğinin*sahibi olan ayrı bir genel yönetici hesabı kullanabilirsiniz.

    Kimlik sağlayıcısı olarak AD FS kullanırken, varsayılan damga dizin hizmeti kullanılır. İle azurestackadmin@azurestack.localoturum açmak için kullanılan varsayılan hesap, kurulum 'un bir parçası olarak belirttiğiniz paroladır.

   > [!NOTE]
   > En iyi sonuçlar için, kimlik sağlayıcısı olarak AD FS kullanarak bağlantısı kesilen Azure Stack ortamı kullanmak istiyor olsanız bile, internet 'e bağlıyken ASDK 'yi yüklemek en iyisidir. Bu şekilde, ASDK yüklemesinde bulunan Windows Server 2016 değerlendirme sürümü, dağıtım sırasında etkinleştirilebilir.

4. ASDK için kullanılacak bir ağ bağdaştırıcısı seçin ve ardından **İleri**' ye tıklayın.

    ![ASDK için ağ bağdaştırıcısı seçin](media/asdk-install/3.PNG)

5. **Ağ yapılandırması** sayfasında, geçerli bir **saat sunucusu IP** adresi sağlayın. Bu gerekli alan, ASDK tarafından kullanılacak saat sunucusunu ayarlar. Bu parametrenin geçerli bir saat sunucusu IP adresi olarak sağlanması gerekir. Sunucu adları desteklenmez.

      > [!TIP]
      > Bir saat sunucusu IP adresi bulmak için [ntppool.org](https://www.ntppool.org/) veya PING Time.Windows.com adresini ziyaret edin. 

    **Isteğe bağlı**olarak, **DNS ileticisi** IP adresi sağlayabilirsiniz. Azure Stack dağıtımının bir parçası olarak bir DNS sunucusu oluşturulur. Çözümdeki bilgisayarların damga dışında adları çözümlemesine izin vermek için, mevcut altyapı DNS sunucunuzu sağlayın. Damga içi DNS sunucusu bilinmeyen ad çözümleme isteklerini bu sunucuya iletir.

    ![ASDK 'de DNS ileticisi ve ağ yapılandırması](media/asdk-install/4.PNG)

6. **Ağ arabirim kartı özelliklerini doğrulama** sayfasında, bir ilerleme çubuğu görürsünüz. Doğrulama tamamlandığında **İleri**' ye tıklayın.

    ![ASDK 'de ağ arabirim kartı özellikleri doğrulanıyor](media/asdk-install/5.PNG)

7. **Özet** sayfasında, asdk ana bilgisayarına yüklemeyi başlatmak için **Dağıt** ' a tıklayın.

    ![ASDK içinde dağıtılmadan önce betik Özeti](media/asdk-install/6.PNG)

    > [!TIP]
    > Burada, ASDK 'yi yüklemek için kullanılacak PowerShell kurulum komutlarını da kopyalayabilirsiniz. Bu, [PowerShell 'i kullanarak ana bilgisayarda ASDK 'yi yeniden dağıtmanız](asdk-deploy-powershell.md)gerekiyorsa yararlıdır.

8. Bir Azure AD dağıtımı gerçekleştiriyorsanız, Kurulum başladıktan sonra Azure AD Genel yönetici hesabı kimlik bilgilerinizi birkaç dakika sonra girmeniz istenir.

9. Dağıtım işlemi birkaç saat sürer. bu zaman, ana bilgisayar bir kez otomatik olarak yeniden başlatılır. Dağıtım ilerlemesini izlemek isterseniz, ASDK ana bilgisayarı yeniden başlatıldıktan sonra azurestack\AzureStackAdmin olarak oturum açın. Dağıtım başarılı olduğunda, PowerShell konsolu şunları görüntüler: **TAMAM ' Dağıtım '** eylemi. 
    > [!IMPORTANT]
    > Makine azurestack etki alanına katıldıktan sonra yerel yönetici olarak oturum açarsanız, dağıtım ilerleme durumunu görmezsiniz. Dağıtımı yeniden çalıştırmayın, bunun çalıştığını doğrulamak için azurestack\AzureStackAdmin olarak oturum açın.

    ![ASDK dağıtım başarısı](media/asdk-install/7.PNG)

Tebrikler, ASDK 'yi başarıyla yüklediniz!

Dağıtım bazı nedenlerle başarısız olursa, son başarılı adımdan dağıtımı [](asdk-redeploy.md) yeniden başlatmak için sıfırdan yeniden dağıtım yapabilir veya aşağıdaki PowerShell komutlarını kullanabilirsiniz. Komutlar aynı yükseltilmiş PowerShell penceresinden kullanılabilir:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtım sonrası yapılandırma](asdk-post-deploy.md)
