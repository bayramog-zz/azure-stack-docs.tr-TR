---
title: Azure Stack kullanarak bir Linux sunucusu VM'si oluşturma | Microsoft Docs
description: Azure Stack kullanarak bir Linux sunucusu VM'si oluşturma.
services: azure-stack
cloud: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.custom: mvc
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: ce06ffbe48848a30de98025c42711e25ca9a312a
ms.sourcegitcommit: 2ee75ded704e8cfb900d9ac302d269c54a5dd9a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394448"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portalını kullanarak bir Linux sunucusu VM'si oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Ubuntu Server 16.04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bu makalede, oluşturun ve bir sanal makine kullanın. Bu makalede ayrıca gösterilmektedir için:

* Bir uzak istemci ile VM'ye bağlanın.
* Bir NGINX web sunucusunu yükleyin.
* Kaynaklarınızı temizleme.

> [!NOTE]  
> Bu makalede görüntüleri, Azure Stack sürüm 1808 sürümünde yapılan değişikliklerle eşleştirmek için güncelleştirilir. Sürüm 1808 kullanma desteği ekler *yönetilen diskler* yönetilmeyen diskler yanı sıra. Önceki bir sürümü kullanıyorsanız, görüntüler, disk seçimi gibi bazı görevler için kullanıcı Arabiriminde görüntülenen öğesinden farklı.  

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack Marketini içindeki bir Linux görüntüsü

   Azure Stack Marketini varsayılan olarak, bir Linux görüntüsü yok. İhtiyacınız Ubuntu Server 16.04 LTS görüntüsüne sağlayan Azure Stack operatörü vardır. İşleç'ndaki yönergeleri kullanabilirsiniz [Azure Stack için Azure Market indirme öğeleri](../operator/azure-stack-download-azure-marketplace-item.md).

* Bir SSH istemcisi erişim

   Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, bir Secure Shell (SSH) istemcisi erişimi olmayabilir. Bir istemci gerekiyorsa, çeşitli paketler bir SSH istemcisi içerir. Örneğin, bir SSH istemcisi ve SSH anahtar Oluşturucu (puttygen.exe) PuTTY içerir. Kullanılabilir paketler hakkında daha fazla bilgi için bkz: [SSH ortak anahtarı kullanmayı](azure-stack-dev-start-howto-ssh-public-key.md).

* Bu hızlı başlangıçta, PuTTY SSH anahtarları oluşturun ve Linux sunucusu VM'sine bağlanmak için kullanılır. [PuTTY'yi indirin ve yükleyin](https://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu makaledeki tüm adımları tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için:

1. PuTTY yükleme klasörüne gidin (varsayılan konum *C:\Program Files\PuTTY*) ve çalıştırın:

    `puttygen.exe`

1. İçinde **PuTTY anahtar Oluşturucu** penceresinde **oluşturulacak anahtar türü** için **RSA**, ayarlayıp **oluşturulan anahtarı bit sayısı** için**2048**.

   ![PuTTY anahtar Oluşturucu yapılandırması](media/azure-stack-quick-linux-portal/Putty01.PNG)

1. Seçin **oluşturmak**.

1. Bir anahtar oluşturmak için **anahtar** kutusunda, rastgele işaretçiyi.

1. Anahtar oluşturma tamamlandığında, seçin **ortak anahtarı Kaydet**ve ardından **özel anahtarı Kaydet** anahtarlarınızı dosyaları kaydetmek için.

   ![PuTTY anahtar Oluşturucu sonuçları](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* ASDK için Git https://portal.local.azurestack.external.

* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-the-vm"></a>Sanal makine oluşturma

1. Azure Stack portal'ın sol üst köşedeki seçin **kaynak Oluştur**.

1. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
   
   ![Linux sunucusu seçin](media/azure-stack-quick-linux-portal/select.png)

1. **Oluştur**’u seçin.

1. Sanal makine bilgileri yazın. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarı**, SSH ortak anahtarı, kayıtlı ve ardından Yapıştır **Tamam**.

   > [!NOTE]
   > Tüm başında veya sonunda boşluk anahtarının kaldırdığınızdan emin olun.

   ![Temel paneli - VM yapılandırma](media/azure-stack-quick-linux-portal/linux-01.PNG)

1. Seçin **D1** VM için.

   ![Bölmesi boyutu - bir VM boyutu seçme](media/azure-stack-quick-linux-portal/linux-02.PNG)

1. Üzerinde **ayarları** sayfasında, varsayılan ayarlara değişiklikleri yapın.
   
   Yapılandırabileceğiniz 1808 Azure Stack sürümünden başlayarak **depolama** ve kullanmayı *yönetilen diskler*. 1808 daha önceki sürümlerde, yalnızca yönetilmeyen diskler kullanılabilir.

   ![Yönetilen diskler için depolama alanını yapılandırma](media/azure-stack-quick-linux-portal/linux-03.PNG)
    
   Yapılandırmalarınızı hazır olduğunuzda seçin **Tamam** devam etmek için.

1. Üzerinde **özeti** sayfasında **Tamam** sanal makine dağıtımını başlatın.  

   ![Dağıtma](media/azure-stack-quick-linux-portal/deploy.png)

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. Seçin **Connect** VM sayfasında. VM'ye bağlanmak için SSH bağlantı dizesi bulabilirsiniz. 

1. Üzerinde **PuTTY Yapılandırması** sayfasında **kategori** bölmesinde aşağı kaydırın ve genişletin **SSH**ve ardından **Auth**. 

   ![VM'ye bağlanın](media/azure-stack-quick-linux-portal/putty03.PNG)

1. Seçin **Gözat**ve ardından kaydedilen özel anahtar dosyası seçin.

1. İçinde **kategori** bölmesinde kadar kaydırın ve **oturumu**.

1. İçinde **ana bilgisayar adı (veya IP adresi)** kutusuna, Azure Stack portalında gösterilen bağlantı dizesini yapıştırın. Bu örnekte, dizedir *asadmin@192.168.102.34* .

1. Seçin **açın** VM için bir oturum açın.

   ![Linux oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirmek ve sanal makinede en son NGINX paketini yüklemek için aşağıdaki bash komutları girin:

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGINX yükleme işiniz bittiğinde, SSH oturumunu kapatın ve VM'yi açma **genel bakış** Azure Stack portalında sayfası.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Bir ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure Stack portalında bir VM oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Barındıran bir web sunucusu bu VM için bir NSG kuralı 80 numaralı bağlantı noktasını web trafiğine izin verecek şekilde oluşturulması gerekir.

1. VM'de **genel bakış** sayfasını, adını seçin **kaynak grubu**.

1. Seçin **ağ güvenlik grubu** VM için. NSG kullanarak tanımlayabilirsiniz **türü** sütun.

1. Sol bölmede altında **ayarları**seçin **gelen güvenlik kuralları**.

1. **Add (Ekle)** seçeneğini belirleyin.

1. İçinde **adı** kutusuna **http**. 

1. Emin olun **bağlantı noktası aralığı** değerinin 80'e ayarlanır ve **eylem** ayarlanır **izin**.

1. **Tamam**’ı seçin.

## <a name="view-the-welcome-to-nginx-page"></a>Hoş Geldiniz ngınx sayfasında görüntüleyin

NGINX yüklendiğine ve VM'NİZDE açık 80 numaralı bağlantı noktası ile web sunucusu sanal makinenin genel IP adresini kullanarak erişebilirsiniz. (Genel IP adresi, VM'nin üzerinde gösterilen **genel bakış** sayfası.)

Bir web tarayıcısı açın ve gidin *http://\<genel IP adresi >* .

![NGINX web sunucusu Karşılama sayfası](media/azure-stack-quick-linux-portal/linux-05.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. VM ve kaynaklarını silmek için VM sayfasında kaynak grubunu seçin ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir web sunucusu ile temel bir Linux sunucusu VM dağıtılır. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [Azure Stack'te Vm'lerde dikkate alınacak noktalar](azure-stack-vm-considerations.md).
