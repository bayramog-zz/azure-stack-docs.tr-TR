---
title: Azure Stack geliştirme Seti'ni (ASDK) ana bilgisayar hazırlama | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) ana bilgisayar ASDK yüklemesine hazırlanmak açıklar.
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
ms.date: 01/21/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: 3352df80a64bb259320a49729e42c02cb19fca58
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66267601"
---
# <a name="prepare-the-asdk-host-computer"></a>ASDK ana bilgisayarını hazırlayın
Ana bilgisayarda ASDK yükleyebilmek için önce ASDK konak yükleme için hazırlıklı olmalıdır. Geliştirme Seti ana bilgisayar hazırlandığınızda ASDK dağıtımına başlamak için CloudBuilder.vhdx sanal makine sabit sürücüden önyükleme yapmaz.

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarını hazırlayın
Ana bilgisayarda ASDK yükleyebilmek için önce ASDK ana bilgisayar ortamı hazırlanmalıdır.
1. Geliştirme Seti ana bilgisayarınızda yerel yönetici olarak oturum açın.
2. CloudBuilder.vhdx dosyayı C:\ sürücüsüne (C:\CloudBuilder.vhdx) kök dizinine taşındığından emin olun.
3. Gelen geliştirme seti yükleyicisini (installer.ps1 asdk) indirmek için aşağıdaki betiği çalıştırın [Azure Stack GitHub araçları depo](https://github.com/Azure/AzureStack-Tools) için **C:\AzureStack_Installer** klasöründe, Geliştirme Seti ana bilgisayarı:

   > [!IMPORTANT]
   > ASDK yüklediğiniz her zaman asdk installer.ps1 dosyayı indirmek emin olun. Bu betik için sık sık değişiklik ve her ASDK dağıtımı için en güncel sürümü kullanılmalıdır. Betik daha eski sürümleri, en son sürüm ile çalışmayabilir.

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

4. Yükseltilmiş bir PowerShell konsolundan başlatın **C:\AzureStack_Installer\asdk-installer.ps1** betik ve ardından **hazırlama ortamı**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Üzerinde **seçin Cloudbuilder vhdx** yükleyici, bulun ve seçin sayfasında **cloudbuilder.vhdx** yüklediğiniz ve açtığınız içinde dosya [önceki adımları](asdk-download.md). Bu sayfada, ayrıca, isteğe bağlı olarak, etkinleştirebilirsiniz **sürücüleri ekleme** Geliştirme Seti ana bilgisayara ek sürücüler eklemeniz gerekiyorsa onay kutusu. **İleri**’ye tıklayın.  

    ![](media/asdk-prepare-host/2.PNG)

6. Üzerinde **isteğe bağlı ayarlar** sayfasında, yerel yöneticinizden hesap Geliştirme Seti konak bilgisayar için bilgi ve ardından **sonraki**.<br><br>Bu adımda yerel yönetici kimlik bilgileri sağlamazsanız, Geliştirme Seti ayarlama bir parçası olarak bilgisayar yeniden başlatıldıktan sonra doğrudan ya da konağa KVM erişim gerekir.

   ![](media/asdk-prepare-host/3.PNG)

    Aşağıdaki isteğe bağlı ayarları için değerleri de sağlayabilirsiniz:
    - **ComputerName**: Bu seçenek, Geliştirme Seti konak adını ayarlar. Adı, FQDN gereksinimlere uygun olmalıdır ve 15 karakter veya daha az olmalıdır. Varsayılan Windows tarafından oluşturulan bir rastgele bir bilgisayar adıdır.
    - **Statik IP yapılandırması**: Dağıtımınızı statik bir IP adresi kullanacak şekilde ayarlar. Aksi takdirde, yükleyici cloudbuilder.vhdx yeniden başlatıldığında, ağ arabirimleri DHCP ile yapılandırılır. Bir statik IP yapılandırması kullanmayı seçerseniz, burada da gerekir ek seçenekler görüntülenir:
      - Bir ağ bağdaştırıcısını seçin. Tıklamadan önce bağdaştırıcısına bağlanabilir olduğundan emin olun **sonraki**.
      - Emin olun görüntülenen **IP adresi**, **ağ geçidi**, ve **DNS** değerlerin doğru olduğundan ve ardından **sonraki**.
13. Tıklayın **sonraki** hazırlama işlemini başlatmak için.
14. Hazırlık zaman gösterdiği **tamamlandı**, tıklayın **sonraki**.

    ![](media/asdk-prepare-host/4.PNG)

15. Tıklayın **şimdi yeniden Başlat** Geliştirme Seti ana bilgisayar cloudbuilder.vhdx önyükleme ve [dağıtım işlemine devam etmek](asdk-install.md).

    ![](media/asdk-prepare-host/5.PNG)


## <a name="next-steps"></a>Sonraki adımlar
[ASDK yükleyin](asdk-install.md)
