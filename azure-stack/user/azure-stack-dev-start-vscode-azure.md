---
title: Visual Studio Code Azure Hesap uzantısı 'nı kullanarak Azure Stack bağlanın | Microsoft Docs
description: Bir geliştirici olarak, Visual Studio Code Azure hesap uzantısını kullanarak Azure Stack bağlanın
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 06/25/2019
ms.openlocfilehash: 9f45e94f26e577f1a47f60b7df24758d7bc88767
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603047"
---
# <a name="connect-to-azure-stack-using-azure-account-extension-in-visual-studio-code"></a>Visual Studio Code Azure hesap uzantısını kullanarak Azure Stack bağlanma

Bu makalede, Azure hesap uzantısını kullanarak Azure Stack nasıl bağlanacağımız konusunda size kılavuzluk edeceğiz. Visual Studio Code (VS Code) ayarlarınızı güncelleştirmeniz gerekecektir.

VS Code Web ve bulut uygulamaları oluşturmaya ve hata ayıklamaya yönelik hafif bir düzenleyicidir. ASP.NET Core, Python, NodeJS, Go ve diğer geliştiriciler tarafından kullanılır. Azure hesap Uzantısı ile, ek Azure uzantıları için abonelik filtrelemesine sahip tek bir Azure oturum açma özelliği kullanabilirsiniz. Uzantı, Azure Cloud Shell VS Code tümleşik terminalde kullanılabilir hale getirir. Uzantıyı kullanarak, kimlik yöneticiniz için hem Azure AD (Azure AD) hem de Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure Stack aboneliğinize bağlanabilirsiniz. Bu, Azure Stack oturum açmanızı, aboneliğinizi seçmenizi ve bir Cloud Shell 'de yeni bir komut satırı açmanızı sağlar. 

> [!Note]  
> Active Directory Federasyon Hizmetleri (AD FS) ortamı için bu makaledeki adımları kullanabilirsiniz. AD FS kimlik bilgilerinizi ve uç noktalarını kullanın.

## <a name="pre-requisites-for-the-azure-account-extension"></a>Azure Hesap uzantısı için önkoşulların önkoşulları

1. Azure Stack Environment 1904 derlemesi veya üzeri
2. [Visual Studio Code](https://code.visualstudio.com/)
3. [Azure hesabı uzantısı](https://github.com/Microsoft/vscode-azure-account)
4. [Azure Stack aboneliği](https://azure.microsoft.com/overview/azure-stack/)

## <a name="steps-to-connect-to-azure-stack"></a>Azure Stack bağlanma adımları

1. GitHub 'daki Azure Stack araçlarından **kimlik** betiğini çalıştırın.

    - Betiği çalıştırmadan önce, PowerShell 'in ortamınız için yüklü ve yapılandırılmış olması gerekir. Yönergeler için bkz. [Azure Stack Için PowerShell 'ı Install](../operator/azure-stack-powershell-install.md).

    - **Kimlik** betiği yönergeleri ve betiği için bkz. [azurestack-Tools/Identity](https://github.com/Azure/AzureStack-Tools/tree/master/Identity).

2. VS Code açın.

3. Sol taraftaki köşedeki **Uzantılar** ' ı seçin.

3. Arama kutusuna `Azure Account` yazın.

4. **Azure hesabı** ' nı seçin ve ardından **yüklemeyi**seçin.

      ![Azure Stack Visual Studio Code](media/azure-stack-dev-start-vscode-azure/image1.png)

5. Uzantıyı yüklemek için VS Code yeniden başlatın.

6. Azure Stack Azure Resource Manager bağlanmak için meta verileri alın. 
    
    Microsoft Azure Kaynak Yöneticisi, Azure kaynaklarını dağıtmanıza, yönetmenize ve izlemenize olanak tanıyan bir yönetim çerçevesidir.
    - Azure Stack Geliştirme Seti (ASDK) Kaynak Yöneticisi URL 'SI:`https://management.local.azurestack.external/` 
    - Tümleşik bir sistemin Kaynak Yöneticisi URL 'SI:`https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`
    - Meta verilere erişmek için URL 'nize aşağıdaki metni ekleyin:`<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

    Örneğin, Azure Resource Manager uç noktanız için meta verileri almaya yönelik URL şöyle görünebilir:`https://management.local.azurestack.external/metadata/endpoints?api-version=1.0`

    Dönüş JSON 'sini bir yere iade edin. `loginEndpoint` Ve`audiences` özelliğinin değerlerine ihtiyacınız olacaktır.

7. **CTRL + SHIFT + P** tuşlarına basın ve Tercihler **' i seçin: Kullanıcı ayarları 'nı (JSON)** açın.

8. Kod Düzenleyicisi 'nde, aşağıdaki JSON kod parçacığını ortamınızın değerleriyle güncelleştirin ve ardından bir kod parçacığını ayarlar bloğuna yapıştırın.

    - Deðerler

        | Parametre | Açıklama |
        | --- | --- |
        | `tenant-ID` | Azure Stack [KIRACı kimliğinizin](../operator/azure-stack-identity-overview.md)değeri. |
        | `activeDirectoryEndpointUrl` | Bu, loginEndpoint özelliğinden alınan URL 'dir. |
        | `activeDirectoryResourceId` | Bu, izleyiciler özelliğinden URL 'dir.
        | `resourceManagerEndpointUrl` | Bu, Azure Stack için Azure Resource Manager kök URL 'sidir. | 

    - JSON kod parçacığı:

      ```JSON  
      "azure.tenant": "tenant-ID",
      "azure.ppe": {
          "activeDirectoryEndpointUrl": "Login endpoint",
          "activeDirectoryResourceId": "This is the URL from the audiences property.",
          "resourceManagerEndpointUrl": "Aure Resource Management Endpoint",
      },
      "azure.cloud": "AzurePPE"
      ```

9. Kullanıcı ayarlarını kaydedin ve **CTRL + SHIFT + P** 'yi bir kez daha kullanın. Azure **seçin: Azure bulutta**oturum açın. Yeni seçenek olan **Azureppe**, hedef listesinde görünür.

10. **Azureppe**öğesini seçin. Kimlik doğrulama sayfası tarayıcınızda yüklenir. Uç noktanıza oturum açın.

11. Azure Stack aboneliğinizde başarıyla oturum açtığınızı test etmek için **CTRL + SHIFT + P** tuşlarını kullanın ve Azure ' u seçin **: Abonelik** ' ı seçin ve sahip olduğunuz aboneliğin kullanılabilir olup olmadığını görün.

## <a name="commands"></a>Komutlar

| Azure: Oturum Aç | Azure aboneliğinizde oturum açın |
| --- | --- |
| Azure: Cihaz koduyla oturum açın | Bir cihaz koduyla Azure aboneliğinizde oturum açın. Bu bunu, oturum açma komutunun çalışmadaki kurulumların içinde kullanın. |
| Azure: Azure bulutta oturum açın | Bağımsız bulutlarından birinde Azure aboneliğinizde oturum açın. |
| Azure: Oturumu Kapat | Azure aboneliğinizde oturumunuzu kapatın. |
| Azure: Abonelikleri Seç | Birlikte çalışmak istediğiniz abonelik kümesini seçin. Uzantı yalnızca filtrelenmiş abonelikler içindeki kaynakları gösterir. |
| Azure: Hesap oluşturun | Azure hesabınız yoksa bugün [kaydolun](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-azure-account&mktingSource=vscode-azure-account) ve ücretsiz kredilerle 200 alabilirsiniz \$. |
| Azure: Bash 'i Cloud Shell açın | Cloud Shell yeni bir Terminal çalıştıran yeni bir Terminal açın. |
| Azure: PowerShell 'i Cloud Shell açın | Cloud Shell yeni bir Terminal çalıştıran PowerShell açın. |
| Azure: Cloud Shell karşıya yükle | Cloud Shell depolama hesabınıza bir dosya yükleyin. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack bir geliştirme ortamı ayarlama](azure-stack-dev-start.md)
