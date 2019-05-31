---
title: Azure Stack'te Azure CLI kullanarak bir Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure Stack'te Azure CLI kullanarak Linux sanal makinesi oluşturun.
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
ms.date: 05/16/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: d47e5908e674a8b57b9e6d686e4596e1002b67c9
ms.sourcegitcommit: 2ee75ded704e8cfb900d9ac302d269c54a5dd9a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394408"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-the-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack'te Azure CLI kullanarak bir Linux sunucusu VM'si oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure CLI kullanarak bir Ubuntu Server 16.04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bu makalede, oluşturun ve bir sanal makine kullanın. Bu makalede ayrıca gösterilmektedir için:

* Sanal Makine Uzak istemcisi ile bağlanın.
* Bir NGINX web sunucusu yüklemek ve varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack Marketini içindeki bir Linux görüntüsü

   Azure Stack Marketini varsayılan olarak bir Linux görüntüsü içermiyor. İhtiyacınız Ubuntu Server 16.04 LTS görüntüsüne sağlayan Azure Stack operatörü vardır. İşleç'ndaki yönergeleri kullanabilirsiniz [Azure Stack için Azure Market indirme öğeleri](../operator/azure-stack-download-azure-marketplace-item.md).

* Azure Stack alt kaynakları oluşturup yönetmek için Azure CLI'yı belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış Azure CLI yoksa, oturum [Azure Stack geliştirme Seti'ni](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) (veya size Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)) ve yönergeleri izleyin için [yükleme ve Azure CLI'yı yapılandırma](azure-stack-version-profiles-azurecli2.md).

* Adlı bir genel güvenli Kabuk (SSH) anahtarının *id_rsa.pub* kaydedilmiş *.ssh* Windows kullanıcı profilinizin dizin. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [bir SSH ortak anahtarı kullanmak](azure-stack-dev-start-howto-ssh-public-key.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu, dağıtma ve Azure Stack kaynaklarını yönetme mantıksal bir kapsayıcıdır. Geliştirme Seti ya da Azure Stack tümleşik sistemi çalıştırın, [az grubu oluşturma](/cli/azure/group#az-group-create) bir kaynak grubu oluşturmak için komutu.

> [!NOTE]
> Biz aşağıdaki kod örnekleri, tüm değişkenlerin değerlerini atadınız. Ancak, kendi değerlerinizi atayabilirsiniz.

Aşağıdaki örnek, yerel konuma myResourceGroup adlı bir kaynak grubu oluşturur: 

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir sanal makine oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) komutu. Aşağıdaki örnek, myVM adlı bir VM oluşturur. Örnekte *Demouser* yönetici kullanıcı adı olarak ve *Demouser@123* yönetici parolası. Bu değerleri ortamınız için uygun bir şey değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

Genel IP adresini döndürülür **Publicıpaddress** parametresi. Sanal makine ile daha sonra kullanmak için adresini not edin.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

IIS web sunucusu çalıştırmak için bu sanal makine olacağından internet trafiği için 80 numaralı bağlantı noktasını açmanız gerekir. Bağlantı noktasını açmaya [az vm open-port](/cli/azure/vm) komutu: 

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Sanal makineye bağlanmak için SSH kullanın

SSH yüklü olan bir istemci bilgisayardan sanal makineye bağlanın. Bir Windows istemcisi üzerinde çalışıyoruz kullanırsanız [PuTTY](https://www.putty.org/) bağlantı oluşturmak için. Sanal makineye bağlanmak için aşağıdaki komutu kullanın:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki betiği çalıştırın:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX web sunucusunun yüklenmesiyle ve 80 numaralı bağlantı noktası, sanal makinenizde ile sanal makinenin genel IP adresini kullanarak web sunucusuna erişebilirsiniz. Bunu yapmak için bir tarayıcı açın ve gidin ```http://<public IP address>```.

![NGINX web sunucusu Karşılama sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. Kullanabileceğiniz [az grubu Sil](/cli/azure/group#az-group-delete) bunları kaldırmak için komutu. Şu komutu çalıştırın:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir web sunucusu ile temel bir Linux sunucusu sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için bkz: [sanal makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
