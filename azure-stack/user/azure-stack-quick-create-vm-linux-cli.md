---
title: Azure Stack'te Azure CLI kullanarak bir Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure stack'teki CLI ile Linux sanal makinesi oluşturun.
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
ms.date: 01/14/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: aa744414b554d5da72a63e8af6371bc94b2d6b39
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64477384"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Hızlı Başlangıç: Azure Stack'te Azure CLI kullanarak bir Linux server sanal makinesi oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure CLI kullanarak bir Ubuntu Server 16.04 LTS sanal makine oluşturabilirsiniz. Bir sanal makine oluşturup, bu makaledeki adımları izleyin. Bu makalede ayrıca adımları sunar:

* Sanal Makine Uzak istemcisi ile bağlanın.
* NGINX web sunucusu ve varsayılan giriş sayfasını görüntüleyin.
* Kullanılmayan kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Stack marketini içindeki bir Linux görüntüsü**

   Azure Stack marketini varsayılan olarak bir Linux görüntüsü içermiyor. Azure Stack operatörü sağlamak için alma **Ubuntu Server 16.04 LTS** ihtiyacınız görüntü. İşleç açıklanan bu adımları kullanabilirsiniz [indirme Market öğesi Azure'dan Azure Stack'e](../operator/azure-stack-download-azure-marketplace-item.md) makalesi.

* Azure Stack kaynakları oluşturup yönetmek için Azure CLI'yı belirli bir sürümünü gerektirir. Azure Stack için yapılandırılmış Azure CLI yoksa, oturum [Geliştirme Seti](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp), ya da Eğer bir Windows tabanlı dış istemci [VPN üzerinden bağlı](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) için adımları [ Azure CLI'yi yükleme ve yapılandırma](azure-stack-version-profiles-azurecli2.md).

* Bir ortak SSH anahtarının Windows kullanıcı profilinizin .ssh dizininde kaydedilen adı id_rsa.pub ile. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [SSH ortak anahtarı kullanmayı](azure-stack-dev-start-howto-ssh-public-key.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu, dağıtma ve Azure Stack kaynaklarını yönetme mantıksal bir kapsayıcıdır. Geliştirme Seti ya da Azure Stack tümleşik sistemi çalıştırın, [az grubu oluşturma](/cli/azure/group#az-group-create) bir kaynak grubu oluşturmak için komutu.

> [!NOTE]
>  Kod örnekleri tüm değişkenler için değerler atanır. Ancak, isterseniz yeni değerler atayabilirsiniz.

Aşağıdaki örnek, yerel konuma myResourceGroup adlı bir kaynak grubu oluşturur.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir sanal makine oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) komutu. Aşağıdaki örnek, myVM adlı bir VM oluşturur. Bu örnekte yönetici kullanıcı adı için Demouser ve Demouser@123 kullanıcı parolası. Bu değerleri ortamınız için uygun bir şey değiştirin.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

Genel IP adresini döndürülür **Publicıpaddress** parametresi. Bu adres sanal makineye erişmek için buna ihtiyacınız olduğundan yazma.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

IIS web sunucusu çalıştırmak için bu sanal makine olacağından Internet trafiği için 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm) komutunu kullanın.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Sanal makineye bağlanmak için SSH kullanın

SSH yüklü olan bir istemci bilgisayardan sanal makineye bağlanın. Bir Windows istemcisi üzerinde çalışıyoruz kullanırsanız [Putty](https://www.putty.org/) bağlantı oluşturmak için. Sanal makineye bağlanmak için aşağıdaki komutu kullanın:

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

NGINX ve 80 numaralı bağlantı noktası, sanal makinede, sanal makinenin genel IP adresini kullanarak web sunucusuna erişebilirsiniz. Bir web tarayıcısı açın ve gidin ```http://<public IP address>```.

![NGINX web-server-Hoş Geldiniz sayfası](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. Kullanabileceğiniz [az grubu Sil](/cli/azure/group#az-group-delete) bu kaynakları kaldırmak için komutu. Kaynak grubunu ve tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir web sunucusu ile temel bir Linux sunucusu sanal makine dağıtıldı. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [sanal Makineler'de Azure Stack için Değerlendirmeler](azure-stack-vm-considerations.md).
