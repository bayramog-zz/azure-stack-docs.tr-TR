---
title: Azure Stack Visual Studio Code ile Dağıt | Microsoft Docs
description: Kullanıcı olarak, Visual Studio Code Azure Resource Manager şablon oluşturmak istiyorum ve Azure Stack sürümmda uyumlu bir şablon hazırlamak için dağıtım şemasını kullanın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/30/2019
ms.openlocfilehash: 914e3e8db57009d58a14aa87d24ff86a8291e52b
ms.sourcegitcommit: e8aa26b078a9bab09c8fafd888a96785cc7abb4d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71711089"
---
# <a name="deploy-with-visual-studio-code-to-azure-stack"></a>Azure Stack Visual Studio Code ile dağıtma

Azure Stack sürümünüzle çalışacak Azure Resource Manager şablonları oluşturmak ve düzenlemek için Visual Studio Code ve Azure Resource Manager araçları uzantısını kullanabilirsiniz. Uzantı olmadan Visual Studio Code'da Resource Manager şablonları oluşturabilirsiniz, ancak uzantı, şablon geliştirmeyi kolaylaştıran otomatik tamamlama seçenekleri sağlar. Ayrıca, Azure Stack kullanılabilir kaynakları anlamanıza yardımcı olacak bir dağıtım şeması da belirtebilirsiniz.

Bu makalede, bir Windows sanal makinesini dağıtacaksınız.

## <a name="concepts-for-azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi Kavramları

### <a name="azure-stack-resource-manager"></a>Azure Stack Kaynak Yöneticisi

Azure Çözümlerinizi Azure Stack ' de dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [Azure Stack Azure Resource Manager şablonlarını kullanma](azure-stack-arm-templates.md).

### <a name="api-profiles"></a>API profilleri
Kaynak sağlayıcılarını koordine etme ile ilişkili kavramları anlamak için Azure Stack bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md).

### <a name="the-deployment-schema"></a>Dağıtım şeması

Azure Stack dağıtım şeması, karma profillerini Visual Studio Code Azure Resource Manager şablonları aracılığıyla destekler. Şemaya başvurmak için JSON şablonundaki bir satırı değiştirebilir ve ardından Azure uyumlu kaynağı gözden geçirmek için IntelliSense 'i kullanabilirsiniz. Şema ile Azure Stack sürümünüzde desteklenen kaynak sağlayıcılarını, türleri ve API sürümlerini gözden geçirin. Şema, Azure Stack sürümünüzde desteklenen kaynak sağlayıcılarındaki API uç noktalarının belirli sürümlerini almak için API profiline bağımlıdır. Type ve apiVersion için Word tamamlanmasını kullanabilirsiniz ve ardından API profili için kullanılabilir olan apiVersion ve kaynak türleriyle sınırlı olursunuz.

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio Code](https://code.visualstudio.com/)
- Azure Stack erişim
- Yönetim uç noktalarına ulaşan bir makineye [Azure Stack PowerShell yüklendi](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fbreadcrumb%2Ftoc.json)

## <a name="install-resource-manager-tools-extension"></a>Kaynak Yöneticisi Araçları uzantısını yükler

Kaynak Yöneticisi Araçları uzantısını yüklemek için aşağıdaki adımları kullanın:

1. Visual Studio Code'u açın.
2. Uzantılar bölmesini açmak için CTRL + SHIFT + X tuşlarına basın
3. @No__t-0 ' ı arayın ve ardından **Install**' ı seçin.
4. Uzantı yüklemesini tamamlamak için **Yeniden Yükle**’yi seçin.

## <a name="get-a-template"></a>Şablon al

Sıfırdan şablon oluşturmak yerine, AzureStack-QuickStart-Templates (https://github.com/Azure/AzureStack-QuickStart-Templates) ' dan bir şablon açarsınız. AzureStack-hızlı başlangıç-şablonlar, Azure Stack kaynakları dağıtan Kaynak Yöneticisi şablonlar için bir depodur. 

Bu makaledeki şablon `101-vm-windows-create` olarak adlandırılmıştır. Şablon, Azure Stack için bir Windows sanal makinesinin temel dağıtımını tanımlar.  Bu şablon Ayrıca bir sanal ağ (DNS ile), ağ güvenlik grubu ve bir ağ arabirimi dağıtır.

1. Visual Studio Code açın ve makinenizde çalışan bir klasöre gidin.
2. Git Bash terminalini Visual Studio Code ' de açın.
3. Azure Stack hızlı başlangıç deposunu almak için aşağıdaki komutu çalıştırın.
    ```bash  
    Git clone https://github.com/Azure/AzureStack-QuickStart-Templates.git
    ```
4. Depoyu içeren dizini açın.
    ```bash  
    CD AzureStack-QuickStart-Templates
    ```
5. Dosyayı depoda `/101-vm-windows-create/azuredeploy.json` ' de açmak için **Aç** ' ı seçin.
6. Dosyayı kendi çalışma alanınıza kaydedin veya deponun bir dalını oluşturduysanız yerinde çalışabilirsiniz.
7. Dosya hala açıkken `$Schema` alanını `https://schema.management.azure.com/schemas/2019-03-01-hybrid/deploymentTemplate.json#` olarak değiştirin.
8. ApiProfile alanının değerini temizleyerek dağıtım şemasının çalışıp çalışmadığını kontrol edebilirsiniz.
    ```JSON  
    "apiProfile": ""
    ```
9. İmlecinizi boş tırnak işaretleriyle yerleştirin ve CTRL + Ara çubuğu tuşlarına basın. Azure Stack için dağıtım şemasında geçerli API profilleri arasından seçim yapabilirsiniz. Bu işlemi, şablondaki kaynak sağlayıcılarının her biriyle gerçekleştirebilirsiniz.

    ![Azure Stack Kaynak Yöneticisi dağıtım şeması](./media/azure-stack-resource-manager-deploy-template-vscode/azure-stack-resource-manager-vscode-schema.png)

10. Hazırsanız, PowerShell kullanarak şablonunuzu dağıtabilirsiniz. [PowerShell Ile dağıtma](azure-stack-deploy-template-powershell.md)bölümündeki yönergeleri izleyin. Komut dosyasındaki şablonun konumunu belirtin.
11. Windows VM 'nizi dağıttıktan sonra Azure Stack portalına gidin ve kaynak grubunu bulun. Bu alıştırmanın sonucunu Azure Stack temizlemek istiyorsanız, kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Kaynak Yöneticisi şablonları](azure-stack-arm-templates.md)hakkında daha fazla bilgi edinin.  
- [Azure Stack ' de API profilleri](azure-stack-version-profiles.md)hakkında daha fazla bilgi edinin.