---
title: Azure CLı kullanarak Azure Stack Windows sanal makinesi oluşturma | Microsoft Docs
description: Azure CLı kullanarak Azure Stack Windows sanal makinesi oluşturma
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/02/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: b88d65806abfe83dfff59307d3bdcd4e99adf96d
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824258"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-using-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack 'de Azure CLı kullanarak Windows Server sanal makinesi oluşturma

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure CLı kullanarak bir Windows Server 2016 sanal makinesi oluşturabilirsiniz. Bir sanal makine oluşturmak ve kullanmak için bu makaledeki adımları izleyin. Bu makale aşağıdaki adımları da sağlar:

* Uzak bir istemciyle sanal makineye bağlanın.
* IIS Web sunucusunu yükleyip varsayılan giriş sayfasını görüntüleyin.
* Kaynaklarınızı temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack operatörünüzün **Windows Server 2016** görüntüsünü Azure Stack Market 'e eklediğinizden emin olun.

* Azure Stack, kaynakları oluşturmak ve yönetmek için Azure CLı 'nin belirli bir sürümünü gerektirir. Azure Stack için Azure CLı yapılandırılmamışsa, [Azure CLI 'yi yüklemek ve yapılandırmak](azure-stack-version-profiles-azurecli2.md)için adımları izleyin.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu Azure Stack kaynaklarını dağıtabileceğiniz ve yönetebileceğiniz bir mantıksal kapsayıcıdır. Azure Stack ortamınızda, bir kaynak grubu oluşturmak için [az Group Create](/cli/azure/group#az-group-create) komutunu çalıştırın.

> [!NOTE]
>  Kod örneklerindeki tüm değişkenler için değerler atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

Aşağıdaki örnek yerel konumda myResourceGroup adlı bir kaynak grubu oluşturur:

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create) komutunu kullanarak bir sanal makıne (VM) oluşturun. Aşağıdaki örnek, myVM adlı bir VM oluşturur. Bu örnek, Yönetici Kullanıcı adı için Demokullanıcı ve yönetici parolası olarak Demouser@123 kullanır. Bu değerleri ortamınıza uygun bir şekilde değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

VM oluşturulduğunda, çıkışdaki **Publicıpaddress** parametresi, sanal makine IÇIN genel IP adresini içerir. Sanal makineyi kullanmak için ihtiyaç duyduğunuz için bu adresi yazın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Bu VM IIS Web sunucusunu çalıştıracağından, 80 numaralı bağlantı noktasını Internet trafiğine açmanız gerekir.

80 numaralı bağlantı noktasını açmak için [az VM Open-Port](/cli/azure/vm) komutunu kullanın:

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makinenize Uzak Masaüstü bağlantısı oluşturmak için bir sonraki komutu kullanın. "Genel IP adresi" ni sanal makinenizin IP adresiyle değiştirin. Sorulduğunda, sanal makine için kullandığınız kullanıcı adını ve parolayı girin.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>PowerShell kullanarak IIS yükleme

Artık sanal makinede oturum açdığınıza göre, PowerShell 'i kullanarak IIS yükleyebilirsiniz. Sanal makinede PowerShell 'i başlatın ve aşağıdaki komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Varsayılan IIS karşılama sayfasını görüntülemek için istediğiniz bir tarayıcıyı kullanabilirsiniz. Önceki bölümde listelenen genel IP adresini kullanarak varsayılan sayfayı ziyaret edin:

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha uzun bir süre gerekmeyen kaynakları temizleyin. Kaynak grubunu, sanal makineyi ve tüm ilgili kaynakları kaldırmak için [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanın.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel bir Windows Server sanal makinesi dağıttınız. Azure Stack sanal makineler hakkında daha fazla bilgi edinmek için [Azure Stack sanal makinelerle Ilgili noktalara](azure-stack-vm-considerations.md)devam edin.
