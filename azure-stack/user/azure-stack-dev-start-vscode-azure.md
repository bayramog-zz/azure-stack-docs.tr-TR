---
title: Visual Studio Code'da Azure hesabı uzantısını kullanarak Azure stack'e bağlanma | Microsoft Docs
description: Bir geliştirici olarak, Azure Stack Visual Studio Code'da Azure hesabı uzantısını kullanarak bağlanma
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: Howto
ms.date: 05/31/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 8e64a570ab45e57e3cf58639bc2ec23d9b9bd81b
ms.sourcegitcommit: 07cc716d97bf484c7260eb165ae205ae25e09589
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453499"
---
# <a name="connect-to-azure-stack-using-azure-account-extension-in-visual-studio-code"></a>Visual Studio Code'da Azure hesabı uzantısını kullanarak Azure stack'e bağlanma

Bu makalede, biz size Azure hesabı uzantısını kullanarak Azure Stack'e bağlanma yol gösterir. Visual Studio Code (VS Code) ayarlarınızı güncelleştirmeniz gerekir.

VS Code, web ve bulut uygulamaları oluşturma ve hata ayıklama için basit bir düzenleyicidir. ASP.NET Core, Python, NodeJS, Git ve diğer geliştiriciler tarafından kullanılır. Azure hesabı uzantısı ile tek bir Azure kullanabileceğiniz filtreleme için ek Azure uzantıları aboneliği ile oturum açın. Uzantı, Azure Cloud shell'e VS Code tümleşik terminale kullanılabilir hale getirir. Eklentiyi kullanarak, hem Azure kullanarak, Azure Stack aboneliğine bağlanabilirsiniz AD (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS), Identity manager için. Bu, Azure Stack için oturum açın, aboneliğinizi seçin ve yeni bir komut satırı cloud shell'de açmak sağlar. 

> [!Note]  
> Bir Active Directory Federasyon Hizmetleri'nde (AD FS) ortamı için bu makaledeki adımları kullanabilirsiniz. AD FS kimlik bilgilerini ve uç noktaları kullanın.

## <a name="pre-requisites-for-the-azure-account-extension"></a>Azure hesabı uzantısı için ön koşullar

1. Azure Stack ortamı 1904 derleme veya üzeri
2. [Visual Studio Code](https://code.visualstudio.com/)
3. [Azure hesabı uzantısı](https://github.com/Microsoft/vscode-azure-account)
4. [Azure Stack aboneliğine](https://azure.microsoft.com/overview/azure-stack/)

## <a name="steps-to-connect-to-azure-stack"></a>Azure Stack'e bağlanma adımları

1. Açık VS kodu.

2. Seçin **uzantıları** sol taraftaki köşedeki.

3. Arama kutusuna `Azure Account` yazın.

4. Seçin **Azure hesabı** seçip **yükleme**.

      ![Azure Stack Visual Studio kodu](media/azure-stack-dev-start-vscode-azure/image1.png)

5. Uzantıyı yüklemek için VS Code'u yeniden başlatın.

6. Azure Stack, Azure Resource Manager'a bağlanmak için meta veriler alır. 
    
    Microsoft Azure Resource Manager dağıtma, yönetme ve Azure kaynaklarınızı izlemenize olanak tanıyan bir yönetim çerçevesidir.
    - Azure Stack geliştirme Seti'ni (ASDK) için kaynak yöneticisi URL'si şöyledir: `https://management.local.azurestack.external/` 
    - Tümleşik bir Sistem Kaynağı Yöneticisi URL'si şudur: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`
    - URL'nizi meta verilerine erişmek için aşağıdaki metni ekleyin: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

    Örneğin, Azure Resource Manager uç noktanız için meta verilerini almak için URL şöyle görünebilir: `https://management.local.azurestack.external/metadata/endpoints?api-version=1.0`

    Dönüş JSON'u not edin. Değerleri için ihtiyacınız olacak `loginEndpoint` ve `loginEndgraphEndpointpoint` özelliği.

7. Tuşuna **Ctrl + Shift + P** seçip **tercihleri: Kullanıcı ayarları (JSON)** .

8. Kod düzenleyicisinde, aşağıdaki JSON kod parçacığında, ortamınız için değerlerle güncelleştirin ve sonra blok kod parçacığını yapıştırın.

    - Değerler:

        | Parametre | Açıklama |
        | --- | --- |
        | `tenant-ID` | Azure Stack değerini [Kiracı kimliği](../operator/azure-stack-identity-overview.md). |
        | `activeDirectoryEndpointUrl` | Bu loginEndpoint özelliğinden URL'dir. |
        | `activeDirectoryResourceId` | Bu loginEndgraphEndpointpoint özelliğinden URL'dir.
        | `resourceManagerEndpointUrl` | Azure Stack için Azure Resource Manager için kök URL'yi budur. | 

    - JSON parçacığı:

      ```JSON  
      "azure.tenant": "tenant-ID",
      "azure.ppe": {
          "activeDirectoryEndpointUrl": "Login endpoint",
          "activeDirectoryResourceId": "graph audience",
          "resourceManagerEndpointUrl": "Management Endpoint",
      },
      "azure.cloud": "AzurePPE"
      ```

8. Kullanıcı ayarları kaydedin ve kullanın **Ctrl + Shift + P** yine. Seçin **Azure: Azure Bulutuna oturum**. Yeni seçenek **AzurePPE**, hedefleri listesinde görünür.

9. Seçin **AzurePPE**. Kimlik doğrulaması sayfası tarayıcınızda yükler. Uç noktanız için oturum açın.

11. Azure Stack aboneliğinize başarıyla kapattınız test etmek için **Ctrl + Shift + P** seçip **Azure: Aboneliği seçin** ve sahip olduğunuz abonelik olup olmadığına bakın.

## <a name="commands"></a>Komutlar

| Azure: Oturum Aç | Azure aboneliğinizde oturum açın |
| --- | --- |
| Azure: Cihaz kodu ile oturum açın | Bir cihaz kodu ile Azure aboneliğinizde oturum açın. Bu ayarlar burada oturum komutu çalışmıyor kullanın. |
| Azure: Azure Bulutuna oturum açın | Bağımsız bulutlarda biriyle Azure aboneliğinizde oturum açın. |
| Azure: Oturumu Kapatma | Dışında Azure aboneliğinizde oturum açın. |
| Azure: Abonelikleri seçin | Birlikte çalışmak istediğiniz abonelikleri kümesini seçin. Uzantı yalnızca filtrelenen abonelik içindeki kaynaklara gösterir. |
| Azure: Bir hesap oluşturun | Bir Azure hesabınız yoksa, şunları yapabilirsiniz [kaydolun](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-azure-account&mktingSource=vscode-azure-account) için bir bugün alıp \$200 ücretsiz krediler. |
| Azure: Açık Cloud Shell'deki Bash hizmetinde | Cloud Shell'de Bash çalıştıran yeni bir terminal açın. |
| Azure: Cloud shell'de PowerShell'i açın | Cloud Shell'de PowerShell çalıştıran yeni bir terminal açın. |
| Azure: Cloud Shell için karşıya yükleme | Bir dosyayı Cloud Shell depolama hesabınıza yükleyin. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack geliştirme ortamında ayarlama ](azure-stack-dev-start.md)