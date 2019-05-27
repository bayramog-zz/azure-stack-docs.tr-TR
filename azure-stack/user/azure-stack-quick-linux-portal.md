---
title: Azure Stack ile bir Linux sunucusu VM'si oluşturma | Microsoft Docs
description: Azure Stack ile bir Linux sunucusu VM oluşturun.
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
ms.openlocfilehash: 379c473080fdbddec811d7982a571cd00e6e1e62
ms.sourcegitcommit: be5382f715a9c1c18c660b630d8fcd823f13aae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66197416"
---
# <a name="quickstart-create-a-linux-server-vm-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portal ile bir Linux sunucusu VM'si oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Ubuntu Server 16.04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bir VM oluşturup bu makaledeki adımları izleyin. Bu makalede ayrıca adımları sunar:

* Bir uzak istemci ile VM'ye bağlanın.
* Bir NGINX web sunucusunu yükleyin.
* Kaynaklarınızı temizleme.

> [!NOTE]  
> Bu makalede ekran görüntüleri, Azure Stack 1808 sürümünde sunulan değişikliklerle eşleştirmek için güncelleştirilir. 1808 kullanma desteği ekler *yönetilen diskler* yönetilmeyen diskler yanı sıra. Önceki bir sürümünü kullanıyorsanız, disk seçimi gibi görevler için bazı ekran görüntüleri, kullanıcı Arabiriminde bkz değerinden farklı görünür.  


## <a name="prerequisites"></a>Önkoşullar

* **Azure Stack marketini içindeki bir Linux görüntüsü**

   Azure Stack marketini varsayılan olarak bir Linux görüntüsü yok. Sağlamak için Azure Stack operatörü alma **Ubuntu Server 16.04 LTS** , Market'ten görüntüde. İşleç açıklanan bu adımları kullanabilirsiniz [indirme Market öğesi Azure'dan Azure Stack'e](../operator/azure-stack-download-azure-marketplace-item.md) makalesi.

* **Bir SSH istemcisi erişim**

   Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, SSH istemcisi erişimi olmayabilir. Bir istemci gerekiyorsa, bir SSH istemcisi dahil çeşitli paketler vardır. Örneğin, bir SSH istemcisi ve SSH anahtarı Oluşturucu (puttygen.exe) PuTTY içerir. Kullanılabilir paketler hakkında daha fazla bilgi için okuma [SSH ortak anahtarı kullanmayı](azure-stack-dev-start-howto-ssh-public-key.md) makalesi.

   Bu hızlı başlangıçta, PuTTY SSH anahtarları oluşturun ve Linux sunucusu VM'sine bağlanmak için kullanılır. Putty'yi indirin ve yükleyin gidin [ https://www.putty.org/ ](https://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu makaledeki tüm adımları tamamlamak için bir SSH anahtar çifti gerekir. Mevcut bir SSH anahtar çiftiniz varsa, bu adımı atlayabilirsiniz.

1. PuTTY yükleme klasörüne gidin (varsayılan konum `C:\Program Files\PuTTY`) çalıştırıp `puttygen.exe`.
2. PuTTY anahtar Oluşturucu penceresinde **oluşturulacak anahtar türü** için **RSA**ve **oluşturulan anahtarı bit sayısı** için **2048**. Hazır olduğunuzda, tıklayın **Oluştur**.

   ![PuTTY anahtar Oluşturucu yapılandırması](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Bir anahtar oluşturmak için fare imlecinizi rastgele PuTTY anahtar Oluşturucu pencerede hareket ettirin.
4. Anahtar oluşturma tamamlandığında, **ortak anahtarı Kaydet** ve ardından **özel anahtarı Kaydet** anahtarlarınızı dosyaları kaydetmek için.

   ![PuTTY anahtar Oluşturucu sonuçları](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* Azure Stack geliştirme Seti'ni (ASDK) için gidin: https://portal.local.azurestack.external.
* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-the-vm"></a>Sanal makine oluşturma

1. Tıklayın **kaynak Oluştur** Azure Stack portal'ın sol üst köşedeki.

2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
   
   ![Linux sunucusu seçin](media/azure-stack-quick-linux-portal/select.png)
1. **Oluştur**’a tıklayın.

4. Sanal makine bilgileri yazın. **Kimlik doğrulama türü** için **SSH ortak anahtarı**’nı seçin. Yapıştırma seçeneğiyle, kaydedilen ve ardından SSH ortak anahtarını **Tamam**.

   > [!NOTE]
   > Tüm başında veya sonunda boşluk anahtarının kaldırdığınızdan emin olun.

   ![Temel paneli - VM yapılandırma](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Seçin **D1** VM için.

   ![Panel boyutunu - bir VM boyutu seçme](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Üzerinde **ayarları** sayfasında, varsayılan olarak istediğiniz değişiklikleri yapın.
   
   - Yapılandırabileceğiniz 1808 Azure Stack sürümünden başlayarak **depolama** ve kullanmayı *yönetilen diskler*. 1808 önceki sürümlerinde, yalnızca yönetilmeyen diskler kullanılabilir.
     ![Yönetilen diskler için depolama alanını yapılandırma](media/azure-stack-quick-linux-portal/linux-03.PNG)
    
     Yapılandırmalarınızı hazır olduğunuzda seçin **Tamam** devam etmek için.

7. Üzerinde **özeti** sayfasında **Tamam** sanal makine dağıtımını başlatın.  
   ![Dağıt](media/azure-stack-quick-linux-portal/deploy.png)

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. Tıklayın **Connect** VM sayfasında. VM'ye bağlanmak için SSH bağlantı dizesi bulabilirsiniz. 

2. PuTTY’yi açın.

3. PuTTY yapılandırma ekranına kullanacağınız **kategori** penceresinde yukarı veya aşağı kaydırın. Ekranı aşağı kaydırarak **SSH**, genişletme **SSH**ve ardından **Auth**. Tıklayın **Gözat** ve kaydedilen özel anahtar dosyası seçin.
   ![VM'ye bağlanın](media/azure-stack-quick-linux-portal/putty03.PNG)

4. Yukarı kaydırın **kategori** penceresi ve ardından **oturumu**.
5. İçinde **ana bilgisayar adı (veya IP adresi)** kutusuna, Azure Stack portalında gösterilen bağlantı dizesini yapıştırın. Bu örnekte, dizedir `asadmin@192.168.102.34`.

   ![PuTTY yapılandırması bağlantı dizesi](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Tıklayın **açın** VM için bir oturum açın.

   ![Linux oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGINX web sunucusunu yükleme

Paket kaynaklarını güncelleştirmek ve sanal makinede en son NGINX paketini yüklemek için aşağıdaki bash komutları kullanın.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGINX yükleme işiniz bittiğinde, SSH oturumunu kapatın ve Azure Stack portalında VM'e genel bakış sayfasını açın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Bir ağ güvenlik grubu (NSG), gelen ve giden trafiğin güvenliğini sağlar. Azure Stack portalında bir VM oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Barındıran bir web sunucusu bu VM için bir NSG kuralı 80 numaralı bağlantı noktasını web trafiğine izin verecek şekilde oluşturulması gerekir.

1. VM'de **genel bakış** sayfasında, adına **kaynak grubu**.
2. Seçin **ağ güvenlik grubu** VM için. NSG, **Tür** sütunu kullanılarak tanımlanabilir.
3. Sol taraftaki menüsünden altında **ayarları**, tıklayın **gelen güvenlik kuralları**.
4. **Ekle**'yi tıklatın.
5. **Ad** alanına **http** yazın. **Bağlantı noktası aralığı** değerinin 80, **Eylem** ayarının **İzin Ver** olarak belirlendiğinden emin olun.
6. **Tamam**'ı tıklatın.

## <a name="view-the-nginx-welcome-page"></a>NGINX karşılama sayfasını görüntüleme

NGINX ve 80 numaralı bağlantı noktası sanal makinenizde ile sanal makinenin genel IP adresini kullanarak web sunucusuna erişebilirsiniz. (Genel IP adresini sanal makinenin genel bakış sayfasında gösterilen.)

Bir tarayıcı açın ve gidin `http://<public IP address>`.

![NGINX web-server-Hoş Geldiniz sayfası](media/azure-stack-quick-linux-portal/linux-05.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmayan kaynakları temizleyin. VM ve kaynaklarını silmek için VM sayfasında kaynak grubunu seçin ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir web sunucusu ile temel bir Linux sunucusu VM dağıtılır. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [Azure Stack'te Vm'lerde dikkate alınacak noktalar](azure-stack-vm-considerations.md).
