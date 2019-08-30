---
title: ASDK ana bilgisayarını hazırlama | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) ana bilgisayarını ASDK yüklemesine hazırlama hakkında bilgi edinin.
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
ms.date: 08/28/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 08/28/2019
ms.openlocfilehash: cf15aebac3ad4d099892270bb2e334d32f82f580
ms.sourcegitcommit: 5efa09034a56eb2f3dc0c9da238fe60cff0c67ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70144023"
---
# <a name="prepare-the-asdk-host-computer"></a>ASDK ana bilgisayarını hazırlama
Ana bilgisayara Azure Stack Geliştirme Seti (ASDK) yüklemeden önce, ASDK konağının yüklenmek üzere hazırlanmalıdır. Konak hazırlandıktan sonra, ASDK dağıtımına başlamak için CloudBuilder. VHDX sanal makinesi (VM) sabit sürücüsünden önyükleme yapılır.

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti konak bilgisayarını hazırlama
Ana bilgisayara ASDK yüklemeden önce, ASDK ana bilgisayar ortamı hazırlanmalıdır. Bu ortamı hazırlamak için şu adımları izleyin:

1. ASDK ana bilgisayarınızda yerel yönetici olarak oturum açın.
2. CloudBuilder. vhdx dosyasının C:\ ' un köküne taşındığından emin olun. Drive (`C:\CloudBuilder.vhdx`).
3. [Azure Stack GitHub araçları deposundaki](https://github.com/Azure/AzureStack-Tools) asdk yükleyici dosyasını (asdk-installer. ps1) asdk ana bilgisayarınızın **C:\azurestack_ınstaller** klasörüne indirmek için aşağıdaki betiği çalıştırın:

   > [!IMPORTANT]
   > Asdk-installer. ps1 dosyasını, ASDK 'yi her yüklediğinizde indirdiğinizden emin olun. Bu betikte sık yapılan değişiklikler yapıldı ve en güncel sürüm her bir ASDK dağıtımı için kullanılmalıdır. Betiğin eski sürümleri en güncel sürümle çalışmayabilir.

   ```powershell
   # Variables
   $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
   $LocalPath = 'C:\AzureStack_Installer'
   # Create folder
   New-Item $LocalPath -Type directory
   # Enforce usage of TLSv1.2 to download from GitHub
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   # Download file
   Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
   ```

4. Yükseltilmiş bir PowerShell konsolundan **C:\azurestack_ınstaller\asdk-Installer.exe** ' ı başlatın ve ardından **ortamı hazırla**' ya tıklayın.

    ![Ortamı ASDK 'de hazırlama](media/asdk-prepare-host/1.PNG) 

5. Yükleyicinin **cloudbuilder VHDX seçin** sayfasında, [önceki adımlarda](asdk-download.md)indirdiğiniz ve ayıkladığınız **cloudbuilder. vhdx** dosyasına gidin ve bu dosyayı seçin. Bu sayfada, ASDK Kit ana bilgisayarına ek sürücüler eklemeniz gerekiyorsa **Sürücü Ekle** onay kutusunu da etkinleştirebilirsiniz. **İleri**'ye tıklayın.  

    ![Cloudbuilder. vhdx ' i seçin ve ASDK 'ye sürücü ekleyin](media/asdk-prepare-host/2.PNG)

6. **Isteğe bağlı ayarlar** sayfasında, asdk ana bilgisayar için yerel yönetici hesabı bilgilerini belirtin ve ardından **İleri**' ye tıklayın.

    Bu adımda Yerel yönetici kimlik bilgilerini sağlamazsanız, ASDK ayarlamanın bir parçası olarak bilgisayar yeniden başlatıldıktan sonra doğrudan veya KVM 'ye erişmeniz gerekir.

   ![ASDK 'de isteğe bağlı ayarlar — yerel yönetici hesabı bilgilerini sağlar](media/asdk-prepare-host/3.PNG)

    Aşağıdaki isteğe bağlı ayarlar için de değerler sağlayabilirsiniz:
    - **ComputerName**: Bu seçenek, ASDK konağının adını ayarlar. Ad, FQDN gereksinimleriyle uyumlu olmalıdır ve 15 karakter uzunluğunda veya daha az olmalıdır. Varsayılan değer, Windows tarafından oluşturulan rastgele bir bilgisayar adıdır.

        - Bir ağ bağdaştırıcısı seçin. **İleri**' ye tıklamadan önce bağdaştırıcıya bağlanabildiğinizden emin olun.

            ![Ağ bağdaştırıcısı ayarlarının ekran görüntüsü](media/asdk-prepare-host/step-four-network-adapter.png)

        - Görüntülenen **IP adresi**, **ağ geçidi**ve **DNS** değerlerinin doğru olduğundan emin olun, geçerli bir **saat sunucusu IP** adresi sağlayın ve ardından **İleri**' ye tıklayın.

            >[!TIP]
            >Bir saat sunucusu IP adresi bulmak için [ntppool.org](https://www.ntppool.org/) veya PING Time.Windows.com adresini ziyaret edin. 

            ![IP yapılandırma ayarlarının ekran görüntüsü](media/asdk-prepare-host/step-five-host-ip-config.png)

7. Hazırlama işlemini başlatmak için **İleri** ' ye tıklayın.
8. Hazırlık **tamamlandığında**, **İleri**' ye tıklayın.

    ![ASDK 'de ortam hazırlama](media/asdk-prepare-host/4.PNG)

9. ASDK ana bilgisayarını cloudbuilder. vhdx ' e önyüklemek ve [dağıtım işlemine devam](asdk-install.md)etmek Için **Şimdi yeniden Başlat** ' a tıklayın.

    ![ASDK 'yi yeniden başlatın](media/asdk-prepare-host/5.PNG)


## <a name="next-steps"></a>Sonraki adımlar
[ASDK 'yi yükler](asdk-install.md)
