---
title: Azure Stack Geliştirme Seti (ASDK) konak bilgisayarı hazırlama | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) ana bilgisayarının, ASDK yüklemesi için nasıl hazırlanacağını açıklar.
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
ms.date: 08/20/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 08/20/2019
ms.openlocfilehash: 291042e0a7af78ed2431c901901e7e44b1f05de1
ms.sourcegitcommit: fc7da38321736e952b2cc6d5d07f276d095dc8d1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887130"
---
# <a name="prepare-the-asdk-host-computer"></a>ASDK ana bilgisayarını hazırlama
Ana bilgisayara ASDK yüklemeden önce, ASDK konağının yüklenmek üzere hazırlanmalıdır. Geliştirme seti ana bilgisayar bilgisayarı hazırlandığında, ASDK dağıtımına başlamak için CloudBuilder. VHDX sanal makinesi sabit sürücüsünden önyükleme yapılır.

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti konak bilgisayarını hazırlama
Ana bilgisayara ASDK yüklemeden önce, ASDK ana bilgisayar ortamı hazırlanmalıdır.
1. Geliştirme seti ana bilgisayarınızda yerel yönetici olarak oturum açın.
2. CloudBuilder. vhdx dosyasının C:\ ' un köküne taşındığından emin olun. Sürücü (C:\cloudbuilder.asp. vhdx).
3. [Azure Stack GitHub araçları deposundan](https://github.com/Azure/AzureStack-Tools) geliştirme seti yükleyici dosyasını (asdk-installer. ps1) geliştirme seti ana bilgisayarınızın **C:\azurestack_ınstaller** klasörüne indirmek için aşağıdaki betiği çalıştırın:

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

    ![Ortam hazırlama ekran görüntüsü](media/asdk-prepare-host/1.PNG) 

5. Yükleyicinin **cloudbuilder VHDX seçin** sayfasında, [önceki adımlarda](asdk-download.md)indirdiğiniz ve ayıkladığınız **cloudbuilder. vhdx** dosyasına gidin ve bu dosyayı seçin. Bu sayfada Ayrıca, geliştirme seti ana bilgisayarına ek sürücüler eklemeniz gerekiyorsa, isteğe bağlı olarak **Sürücü Ekle** onay kutusunu da etkinleştirebilirsiniz. **İleri**'ye tıklayın.  

    ![Cloudbuilder. vhdx seçme ekran görüntüsü](media/asdk-prepare-host/2.PNG)

6. **Isteğe bağlı ayarlar** sayfasında, geliştirme seti ana bilgisayarı için yerel yönetici hesabı bilgilerini belirtin ve ardından **İleri**' ye tıklayın.<br><br>Bu adımda Yerel yönetici kimlik bilgilerini sağlamazsanız, geliştirme setini ayarlamanın bir parçası olarak bilgisayar yeniden başlatıldıktan sonra doğrudan veya KVM 'ye erişmeniz gerekir.

   ![Isteğe bağlı ayarların ekran görüntüsü](media/asdk-prepare-host/3.PNG)

    Aşağıdaki isteğe bağlı ayarlar için de değerler sağlayabilirsiniz:
    - **ComputerName**: Bu seçenek, geliştirme seti konağının adını ayarlar. Ad, FQDN gereksinimleriyle uyumlu olmalıdır ve 15 karakter uzunluğunda veya daha az olmalıdır. Varsayılan değer, Windows tarafından oluşturulan rastgele bir bilgisayar adıdır.
    - **STATIK IP yapılandırması**: Dağıtımınızı statik bir IP adresi kullanacak şekilde ayarlar. Aksi takdirde, yükleyici cloudbuilder. vhdx ' de yeniden başlatıldığında, ağ arabirimleri DHCP ile yapılandırılır. Statik IP yapılandırması kullanmayı seçerseniz, ek seçenekler de şunları yapmanız gereken yerde görüntülenir:
      - Bir ağ bağdaştırıcısı seçin. **İleri**' ye tıklamadan önce bağdaştırıcıya bağlanabildiğinizden emin olun.

        ![Ağ bağdaştırıcısı ayarlarının ekran görüntüsü](media/asdk-prepare-host/step-four-network-adapter.png)

      - Görüntülenen **IP adresi**, **ağ geçidi**ve **DNS** değerlerinin doğru olduğundan emin olun, geçerli bir **saat sunucusu IP** adresi sağlayın ve ardından **İleri**' ye tıklayın.

        >[!TIP]
        >Bir saat sunucusu IP adresi bulmak için [ntppool.org](https://www.ntppool.org/) veya PING Time.Windows.com adresini ziyaret edin. 

        ![IP yapılandırma ayarlarının ekran görüntüsü](media/asdk-prepare-host/step-five-host-ip-config.png)

7. Hazırlama işlemini başlatmak için **İleri** ' ye tıklayın.
8. Hazırlık **tamamlandığında**, **İleri**' ye tıklayın.

    ![Tamamlanan ekranın ekran görüntüsü](media/asdk-prepare-host/4.PNG)

9. Geliştirme seti ana bilgisayarını cloudbuilder. vhdx ' e önyüklemek ve [dağıtım işlemine devam](asdk-install.md)etmek Için **Şimdi yeniden Başlat** ' a tıklayın.

    ![Şimdi yeniden başlatmanın ekran görüntüsü](media/asdk-prepare-host/5.PNG)


## <a name="next-steps"></a>Sonraki adımlar
[ASDK 'yi yükler](asdk-install.md)
