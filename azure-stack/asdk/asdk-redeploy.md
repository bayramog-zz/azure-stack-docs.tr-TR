---
title: ASDK 'yi yeniden dağıtın | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) yeniden dağıtmayı öğrenin.
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
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: 5ff77bbe915a506803a1c06f68579c199439ea73
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025903"
---
# <a name="redeploy-the-asdk"></a>ASDK 'yi yeniden dağıtın
Bu makalede, üretim dışı bir ortamda Azure Stack Geliştirme Seti (ASDK) yeniden dağıtmayı öğreneceksiniz. ASDK 'yi yükseltmek desteklenmediğinden, daha yeni bir sürüme geçmek için tam olarak yeniden dağıtmanız gerekir. Ayrıca, sıfırdan başlamak istediğinizde ASDK 'yi yeniden dağıtabilirsiniz.

> [!IMPORTANT]
> ASDK 'yi yeni bir sürüme yükseltmek desteklenmez. Azure Stack yeni bir sürümünü değerlendirmek istediğiniz her seferinde ASDK ana bilgisayarında ASDK 'yi yeniden dağıtmanız gerekir.

## <a name="remove-azure-registration"></a>Azure kaydını kaldırma 
Azure ile ASDK yüklemenizi daha önce kaydolduysanız, ASDK 'yi yeniden dağıtmaya başlamadan önce kayıt kaynağını kaldırmanız gerekir. ASDK 'yi yeniden dağıtırken Market 'teki öğelerin kullanılabilirliğini etkinleştirmek için ASDK 'yi yeniden kaydedin. Daha önce Azure aboneliğiniz ile ASDK 'ye kaydolmadıysanız, bu bölümü atlayabilirsiniz.

Kayıt kaynağını kaldırmak için **Remove-AzsRegistration** cmdlet 'ini kullanarak Azure Stack kaydını kaldırın. Ardından, Azure aboneliğinizden Azure Stack kaynak grubunu silmek için **Remove-AzureRMResourceGroup** cmdlet 'ini kullanın:

1. Ayrıcalıklı uç noktaya erişimi olan bir bilgisayarda bir PowerShell konsolunu yönetici olarak açın. ASDK için, bu, ASDK ana bilgisayardır.

2. ASDK yüklemenizin kaydını silmek ve **azurestack** kaynak grubunu Azure aboneliğinizden silmek Için aşağıdaki PowerShell komutlarını çalıştırın:

   ```powershell    
   #Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Provide Azure subscription admin credentials
   Add-AzureRmAccount

   # Provide ASDK admin credentials
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

   # Unregister Azure Stack
   Remove-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01

   # Remove the Azure Stack resource group
   Remove-AzureRmResourceGroup -Name azurestack -Force
   ```

3. Betik çalışırken hem Azure aboneliğinizde hem de yerel ASDK yüklemesinde oturum açmanız istenir.
4. Betik tamamlandığında aşağıdaki örneklere benzer iletiler görmeniz gerekir:

    `De-Activating Azure Stack (this may take up to 10 minutes to complete).``Your environment is now unable to syndicate items and is no longer reporting usage data.`
    `Remove registration resource from Azure...`
    `"Deleting the resource..." on target "/subscriptions/<subscription information>"`
    `********** End Log: Remove-AzsRegistration *********`



Azure Stack artık Azure aboneliğinizdeki kaydı başarıyla kaldırılmalıdır. Azurestack kaynak grubu da silinmelidir. Bu kaynak grubu, ASDK 'yi Azure ile ilk kez kaydettiğinizde oluşturulan bir kaynaktır.

## <a name="deploy-the-asdk"></a>ASDK 'yi dağıtma
Azure Stack yeniden dağıtmak için, aşağıda açıklandığı gibi sıfırdan baştan başlamanız gerekir. Bu adımlar, ASDK 'yi yüklemek için Azure Stack Installer (asdk-installer. ps1) betiğinin kullanılıp kullanıldığınıza bağlı olarak farklılık belirtir.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>Yükleyici betiğini kullanarak ASDK 'yi yeniden dağıtın
1. ASDK bilgisayarında, yükseltilmiş bir PowerShell konsolu açın ve sistem dışı bir sürücüde bulunan **AzureStack_Installer** dizininde bulunan asdk-installer. ps1 betiğine gidin. Betiği çalıştırın ve **Yeniden Başlat**' a tıklayın.

   ![Asdk-installer. ps1 betiğini çalıştırma](media/asdk-redeploy/1.png)

2. Temel işletim sistemini ( **Azure Stack**değil) seçin ve **İleri**' ye tıklayın.

   ![Konak işletim sisteminde yeniden başlatın](media/asdk-redeploy/2.png)

3. ASDK ana bilgisayarı temel işletim sisteminde yeniden başlatıldıktan sonra, yerel yönetici olarak oturum açın. Önceki dağıtımın bir parçası olarak kullanılan **C:\cloudbuilder.exe** dosyasını bulun ve silin.

4. İlk olarak [ASDK 'yi dağıtmak](asdk-install.md)için yaptığınız adımların aynısını tekrarlayın.

### <a name="redeploy-the-asdk-without-using-the-installer"></a>Yükleyiciyi kullanmadan ASDK 'yi yeniden dağıtın
ASDK 'yi yüklemek için asdk-installer. ps1 betiğini kullanmıyorsanız, asdk 'yi yeniden dağıtımdan önce, ASDK ana bilgisayarını el ile yeniden yapılandırmanız gerekir.

1. ASDK bilgisayarında **msconfig. exe** ' yi çalıştırarak sistem yapılandırma yardımcı programını başlatın. **Önyükleme** sekmesinde, ana bilgisayar işletim sistemini (Azure Stack değil) seçin, **Varsayılan olarak ayarla**' ya tıklayın ve ardından **Tamam**' a tıklayın. İstendiğinde **Yeniden Başlat** ' a tıklayın.

      ![Önyükleme yapılandırmasını ayarlama](media/asdk-redeploy/4.png)

2. ASDK ana bilgisayarı temel işletim sisteminde yeniden başlatıldıktan sonra, ASDK ana bilgisayarı için yerel yönetici olarak oturum açın. Önceki dağıtımın bir parçası olarak kullanılan **C:\cloudbuilder.exe** dosyasını bulun ve silin.

3. [PowerShell kullanarak ilk olarak ASDK 'yi dağıtmak](asdk-deploy-powershell.md)için yaptığınız adımların aynısını tekrarlayın.


## <a name="next-steps"></a>Sonraki adımlar
[ASDK dağıtım görevleri sonrası](asdk-post-deploy.md)




