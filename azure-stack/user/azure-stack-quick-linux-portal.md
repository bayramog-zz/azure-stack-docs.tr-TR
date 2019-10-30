---
title: Azure Stack kullanarak bir Linux VM oluşturma | Microsoft Docs
description: Azure Stack kullanarak bir Linux sunucu VM 'si oluşturun.
services: azure-stack
cloud: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.custom: mvc
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 5c3b8d85f5dca0eeb439ca475d4396848d316366
ms.sourcegitcommit: 0d27456332031ab98ba2277117395ae5ffcbb79f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73047260"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-the-azure-stack-portal"></a>Hızlı başlangıç: Azure Stack portalını kullanarak bir Linux Server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak Ubuntu Server 16,04 LTS sanal makinesi (VM) oluşturabilirsiniz. Bu makalede, bir sanal makine oluşturup kullanacaksınız. Bu makalede ayrıca nasıl yapılacağı gösterilmektedir:

* Uzak bir istemciyle VM 'ye bağlanın.
* Bir NGıNX Web sunucusu yükler.
* Kaynaklarınızı temizleyin.

> [!NOTE]  
> Bu makaledeki görüntüler Azure Stack sürüm 1808 ' de tanıtılan değişikliklerle eşleşecek şekilde güncelleştirilir. Sürüm 1808, yönetilmeyen disklere ek olarak *yönetilen disklerin* kullanılmasına yönelik destek ekler. Daha önceki bir sürümünü kullanıyorsanız, disk seçimi gibi bazı görevler için görüntüler, Kullanıcı arabiriminizdeki görüntülendiklerden farklıdır.  

## <a name="prerequisites"></a>Önkoşullar

* Azure Stack Market 'teki bir Linux görüntüsü

   Azure Stack marketi 'nin varsayılan olarak bir Linux görüntüsü yoktur. Azure Stack işlecine ihtiyacınız olan Ubuntu Server 16,04 LTS görüntüsünü sağlayın. İşleci, [Azure 'Dan Market öğelerini indirme bölümündeki yönergeleri Azure Stack için](../operator/azure-stack-download-azure-marketplace-item.md)kullanabilir.

* SSH istemcisine erişim

   Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, bir Secure Shell (SSH) istemcisine erişiminiz olmayabilir. Bir istemciye ihtiyacınız varsa, birkaç paket SSH istemcisi içerir. Örneğin, PuTTY bir SSH istemcisi ve SSH anahtar Oluşturucusu (PuTTYgen. exe) içerir. Kullanılabilir paketler hakkında daha fazla bilgi için bkz. [SSH ortak anahtarı kullanma](azure-stack-dev-start-howto-ssh-public-key.md).

* Bu hızlı başlangıç, SSH anahtarlarını oluşturmak ve Linux sunucu VM 'sine bağlanmak için PuTTY kullanır. [PuTTY'yi indirin ve yükleyin](https://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu makaledeki tüm adımları tamamlaması için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

Bir SSH anahtar çifti oluşturmak için:

1. PuTTY yükleme klasörüne gidin (varsayılan konum *C:\Program Files\PuTTY*) ve şunu çalıştırın:

    `puttygen.exe`

1. **Putty anahtar Oluşturucu** penceresinde, oluşturulacak **anahtar türünü** **RSA**olarak ayarlayın ve **oluşturulan bir anahtardaki bit sayısını** **2048**olarak ayarlayın.

   ![PuTTY anahtar Oluşturucu yapılandırması](media/azure-stack-quick-linux-portal/Putty01.PNG)

1. **Oluştur**' u seçin.

1. Anahtar oluşturmak için, **anahtar** kutusunda işaretçiyi rastgele taşıyın.

1. Anahtar üretimi tamamlandığında, **ortak anahtarı kaydet**' i seçin ve ardından anahtarlarınızı dosyalarınıza kaydetmek için **özel anahtarı kaydet** ' i seçin.

   ![PuTTY anahtar Oluşturucu sonuçları](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalının adresi, bağlandığınız Azure Stack ürüne bağlıdır:

* ASDK için https://portal.local.azurestack.external gidin.

* Azure Stack tümleşik bir sistem için Azure Stack işletmenizin verdiğiniz URL 'ye gidin.

## <a name="create-the-vm"></a>Sanal makine oluşturma

1. Azure Stack portalının sol üst köşesinde **kaynak oluştur**' u seçin.

1. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
   
   ![Linux sunucusunu seçin](media/azure-stack-quick-linux-portal/select.png)

1. **Oluştur**'u seçin.

1. VM bilgilerini yazın. **Kimlik doğrulama türü**için **SSH ortak anahtarı**' nı seçin, kaydettiğiniz SSH ortak anahtarını yapıştırın ve **Tamam**' ı seçin.

   > [!NOTE]
   > Anahtar için baştaki veya sondaki boşlukları kaldırdığınızdan emin olun.

   ![Temel bilgiler paneli-VM 'yi yapılandırma](media/azure-stack-quick-linux-portal/linux-01.PNG)

1. VM için **D1** öğesini seçin.

   ![Boyut bölmesi-VM boyutunu seçin](media/azure-stack-quick-linux-portal/linux-02.PNG)

1. **Ayarlar** sayfasında, varsayılanlar üzerinde değişiklikler yapın.
   
   Azure Stack sürüm 1808 ' den başlayarak, **depolamayı** yapılandırabilir ve *yönetilen diskleri*kullanmayı seçebilirsiniz. 1808 ' den önceki sürümlerde yalnızca yönetilmeyen diskler kullanılabilir.

   ![Yönetilen diskler için depolamayı yapılandırma](media/azure-stack-quick-linux-portal/linux-03.PNG)
    
   Yapılandırmalarınız hazırlandığınızda, devam etmek için **Tamam** ' ı seçin.

1. **Özet** SAYFASıNDA, VM dağıtımını başlatmak için **Tamam** ' ı seçin.  

   ![Kurulum](media/azure-stack-quick-linux-portal/deploy.png)

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. VM sayfasında **Bağlan** ' ı seçin. SANAL makineye bağlanmak için gereken SSH bağlantı dizesini bulabilirsiniz. 

1. **Putty yapılandırma** sayfasında, **Kategori** bölmesinde, öğesine gidip **SSH**' ı genişletin ve ardından **kimlik doğrulaması**' nı seçin. 

   ![VM 'yi bağlama](media/azure-stack-quick-linux-portal/putty03.PNG)

1. **Araştır**' ı seçin ve ardından kaydettiğiniz özel anahtar dosyasını seçin.

1. **Kategori** bölmesinde, yukarı kaydırarak **oturum**' ı seçin.

1. **Ana bilgisayar adı (veya IP adresi)** kutusunda, Azure Stack portalında gösterilen bağlantı dizesini yapıştırın. Bu örnekte, dize *asadmin@192.168.102.34* .

1. VM için bir oturum açmak üzere **Aç** ' ı seçin.

   ![Linux oturumu](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGıNX Web sunucusunu yükler

Paket kaynaklarını güncelleştirmek ve VM 'ye en son NGıNX paketini yüklemek için aşağıdaki Bash komutlarını girin:

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGıNX yüklemesini bitirdiğinizde SSH oturumunu kapatın ve Azure Stack portalında VM **genel bakış** sayfasını açın.

## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın

Ağ güvenlik grubu (NSG) gelen ve giden trafiğin güvenliğini sağlar. Azure Stack portalında bir VM oluşturulduğunda, SSH bağlantıları için 22 numaralı bağlantı noktasında bir gelen kuralı oluşturulur. Bu VM bir Web sunucusu barındırdığından, 80 numaralı bağlantı noktasında Web trafiğine izin vermek için bir NSG kuralının oluşturulması gerekir.

1. VM **'ye genel bakış** sayfasında, **kaynak grubunun**adını seçin.

1. VM için **ağ güvenlik grubunu** seçin. **Tür** sütununu kullanarak NSG 'yi tanımlayabilirsiniz.

1. Sol bölmede, **Ayarlar**altında **gelen güvenlik kuralları**' nı seçin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. **Ad** kutusuna **http**yazın. 

1. **Bağlantı noktası aralığının** 80 olarak ayarlandığından ve **eylemin** **izin ver**olarak ayarlandığından emin olun.

1. **Tamam**’ı seçin.

## <a name="view-the-welcome-to-nginx-page"></a>NGINX sayfasına hoş geldiniz sayfasını görüntüleyin

NGıNX yüklü ve bağlantı noktası 80, VM 'niz üzerinde açık olduğunda, VM 'nin genel IP adresini kullanarak Web sunucusuna erişebilirsiniz. (Genel IP adresi, VM 'nin **genel bakış** sayfasında gösterilir.)

Bir Web tarayıcısı açın ve *http://\<genel IP adresi >* sayfasına gidin.

![NGıNX Web sunucusu karşılama sayfası](media/azure-stack-quick-linux-portal/linux-05.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha uzun bir süre gerekmeyen kaynakları temizleyin. VM 'yi ve kaynaklarını silmek için VM sayfasında kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Web sunucusuyla temel bir Linux sunucu VM 'si dağıttınız. Azure Stack VM 'Ler hakkında daha fazla bilgi edinmek için [Azure Stack VM 'lerle Ilgili noktalara](azure-stack-vm-considerations.md)devam edin.
