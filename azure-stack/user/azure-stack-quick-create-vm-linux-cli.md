---
title: Azure Stack 'de Azure CLı kullanarak Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure Stack ' de Azure CLı kullanarak bir Linux sanal makinesi oluşturun.
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
ms.openlocfilehash: 84689e45bff8150616f37205eaa4a9bd9b25ff04
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824243"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-the-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack ' de Azure CLı kullanarak bir Linux Server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure CLı kullanarak bir Ubuntu Server 16,04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bu makalede, bir sanal makine oluşturup kullanacaksınız. Bu makalede ayrıca nasıl yapılacağı gösterilmektedir:

* Uzak bir istemciyle sanal makineye bağlanın.
* Bir NGıNX Web sunucusu yükleyip varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack Market 'teki bir Linux görüntüsü

   Azure Stack Marketi, varsayılan olarak bir Linux görüntüsü içermez. Azure Stack işlecine ihtiyacınız olan Ubuntu Server 16,04 LTS görüntüsünü sağlayın. İşleci, [Azure 'Dan Market öğelerini indirme bölümündeki yönergeleri Azure Stack için](../operator/azure-stack-download-azure-marketplace-item.md)kullanabilir.

* Azure Stack, kaynaklarını oluşturmak ve yönetmek için Azure CLı 'nin belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış Azure CLı yoksa, [Azure Stack geliştirme seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) (veya [VPN üzerinden bağlandıysanız](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)Windows tabanlı bir dış istemcide) oturum açın ve [Azure 'u yükleme ve yapılandırma yönergelerini izleyin. CLı](azure-stack-version-profiles-azurecli2.md).

* Windows Kullanıcı profilinizin *. SSH* dizinine kaydedilen *id_rsa. pub* adlı BIR genel Secure Shell (SSH) anahtarı. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz. [SSH ortak anahtarı kullanma](azure-stack-dev-start-howto-ssh-public-key.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu Azure Stack kaynaklarını dağıtabileceğiniz ve yönetebileceğiniz bir mantıksal kapsayıcıdır. Geliştirme setinizden veya tümleşik Azure Stack sisteminizde, bir kaynak grubu oluşturmak için [az Group Create](/cli/azure/group#az-group-create) komutunu çalıştırın.

> [!NOTE]
> Aşağıdaki kod örneklerinde tüm değişkenler için değerler atandık. Ancak, kendi değerlerinizi atayabilirsiniz.

Aşağıdaki örnek yerel konumda myResourceGroup adlı bir kaynak grubu oluşturur: 

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create) komutunu kullanarak bir sanal makine oluşturun. Aşağıdaki örnek, myVM adlı bir VM oluşturur. Örnek, Yönetici Kullanıcı adı olarak *Demokullanıcı* ve yönetici parolası olarak *Demouser@123* kullanır. Bu değerleri, ortamınız için uygun olan bir şekilde değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

Genel IP adresi **Publicıpaddress** parametresinde döndürülür. Daha sonra sanal makineyle birlikte kullanılacak adresi aklınızda yapın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Bu sanal makine IIS Web sunucusunu çalıştıracağından, 80 numaralı bağlantı noktasını Internet trafiğine açmanız gerekir. Bağlantı noktasını açmak için [az VM Open-Port](/cli/azure/vm) komutunu kullanın: 

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Sanal makineye bağlanmak için SSH kullanma

SSH yüklü bir istemci bilgisayarından sanal makineye bağlanın. Bir Windows istemcisinde çalışıyorsanız, bağlantıyı oluşturmak için [Putty](https://www.putty.org/) ' ı kullanın. Sanal makineye bağlanmak için aşağıdaki komutu kullanın:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>NGıNX Web sunucusunu yükler

Paket kaynaklarını güncelleştirmek ve en son NGıNX paketini yüklemek için aşağıdaki betiği çalıştırın:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGıNX Web sunucusu yüklüyken ve bağlantı noktası 80 sanal makinenizde açık olduğunda, sanal makinenin genel IP adresini kullanarak Web sunucusuna erişebilirsiniz. Bunu yapmak için bir tarayıcı açın ve ```http://<public IP address>``` ' a gidin.

![NGıNX Web sunucusu karşılama sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha uzun bir süre gerekmeyen kaynakları temizleyin. Bunları kaldırmak için [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanabilirsiniz. Şu komutu çalıştırın:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Web sunucusuyla temel bir Linux sunucusu sanal makinesi dağıttınız. Azure Stack sanal makineler hakkında daha fazla bilgi edinmek için bkz. [Azure Stack sanal makinelere yönelik hususlar](azure-stack-vm-considerations.md).
