---
title: Topluluk şablonu kullanarak VM oluşturma | Microsoft Docs
description: Önceden tanımlanmış bir şablon ve GitHub özel şablonu kullanarak bir VM oluşturmak için Azure Stack Geliştirme Seti (ASDK) kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/03/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: 02300ae5c3a6ef7fd104dfb59e179b557e0c8778
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961716"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Öğretici: Topluluk şablonu kullanarak VM oluşturma

Azure Stack işleci veya Kullanıcı olarak, bir şablonu Azure Stack marketi 'nden el ile dağıtmak yerine [özel GitHub hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) kullanarak bir sanal makıne (VM) oluşturabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonları kullanma
> * Özel bir GitHub şablonu kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükler

## <a name="azure-stack-quickstart-templates"></a>Azure Stack hızlı başlangıç şablonları

Azure Stack hızlı başlangıç şablonları GitHub [genel Azure Stack hızlı başlangıç şablonları deposunda](https://github.com/Azure/AzureStack-QuickStart-Templates)depolanır. Bu depo, Microsoft Azure Stack geliştirme seti (ASDK) ile test edilmiş Azure Resource Manager dağıtım şablonları içerir. Bunları, Azure Stack değerlendirmeniz ve ASDK ortamını kullanmanız daha kolay hale getirmek için kullanabilirsiniz.

Zaman içinde, birçok GitHub kullanıcısı depoya katkıda bulunan ve 400 'den fazla dağıtım şablonu elde eden bir koleksiyon sağlar. Bu depo, Azure Stack için çeşitli türlerde ortamları nasıl dağıtabileceğinizi anlamak için iyi bir başlangıç noktasıdır.

>[!IMPORTANT]
> Bu şablonlardan bazıları, Microsoft tarafından değil, topluluk üyeleri tarafından oluşturulmuştur. Her şablon, Microsoft tarafından değil, sahibi tarafından bir lisans sözleşmesi kapsamında lisanslanır. Microsoft bu şablonlardan sorumlu değildir ve güvenlik, uyumluluk veya performans için ekran yapmaz. Topluluk şablonları herhangi bir Microsoft destek programı veya hizmeti altında desteklenmez ve herhangi bir garanti olmadan "olduğu gıbı" kullanılabilir hale getirilir.

GitHub 'a Azure Resource Manager şablonları eklemek istiyorsanız, [Azurestack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates) depolarına katılımınızı yapın. Bu depo ve bu depoyu hakkında daha fazla bilgi edinmek için [Benioku dosyasına](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md)bakın.

## <a name="create-a-vm-using-a-custom-github-template"></a>Özel bir GitHub şablonu kullanarak VM oluşturma

Bu örnek öğreticide, bir Kubernetes kümesini yönetmek için Minikube çalıştıran Azure Stack bir Ubuntu 16,04 VM dağıtmak üzere [101-VM-Linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure Stack hızlı başlangıç şablonu kullanılır.

Minikube, Kubernetes 'i yerel olarak çalıştırmanızı kolaylaştıran bir araçtır. Minikube, bir VM içinde tek düğümlü bir Kubernetes kümesi çalıştırır ve Kubernetes 'i denemenizi veya BT gününe günde bir geliştirme olanağı sağlar. Linux VM üzerinde çalışan basit, tek düğümlü bir Kubernetes kümesini destekler. Minikube, çalışan tam işlevli bir Kubernetes kümesi almanın en hızlı ve en kolay yoludur. Geliştiricilerin kendi yerel makinelerinde Kubernetes tabanlı uygulama dağıtımlarını geliştirmesini ve test etmesini sağlar. Mimari türsel olarak, Minikube VM hem ana hem de aracı düğüm bileşenlerini yerel olarak çalıştırır:

* API Server, Scheduler ve [etcd sunucusu](https://coreos.com/etcd/) gibi ana düğüm bileşenleri, **localkuas**adlı tek bir Linux işleminde çalıştırılır.
* Aracı düğümü bileşenleri, Docker kapsayıcılarının içinde normal bir aracı düğümünde çalıştıkları gibi çalışır. Uygulama dağıtımı açısından, uygulamanın bir Minikube üzerinde veya normal bir Kubernetes kümesinde dağıtılması arasında fark yoktur.

Bu şablon aşağıdaki bileşenleri yüklenir:

* Ubuntu 16,04 LTS VM
* [Docker-CE](https://download.docker.com/linux/ubuntu)
* [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
* [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
* xFCE4
* xRDP

> [!IMPORTANT]
> Bu adımları gerçekleştirmeden önce Ubuntu VM görüntüsünün (Bu örnekteki Ubuntu Server 16,04 LTS) Azure Stack Market 'e zaten eklenmiş olması gerekir.

1. **+ Kaynak oluştur**' u ve ardından **özel**' i seçin ve **şablon dağıtımı**.

    ![Şablon oluşturma](media/azure-stack-create-vm-template/1.PNG)

2. **Şablonu Düzenle**' yi seçin.

    ![Bir şablonu düzenleme](media/azure-stack-create-vm-template/2.PNG)

3. **Hızlı başlangıç şablonu**' nu seçin.

    ![Hızlı başlangıç şablonu](media/azure-stack-create-vm-template/3.PNG)

4. **Şablon seç** açılan listesini kullanarak kullanılabilir şablonlardan **101-VM-Linux-minikube** öğesini seçin ve ardından **Tamam**' a tıklayın.

    ![Bir şablon seçin](media/azure-stack-create-vm-template/4.PNG)

5. JSON şablonunda değişiklik yapmak istiyorsanız bunu yapabilirsiniz. Aksi takdirde veya tamamlandığında, **Şablonu Düzenle** iletişim kutusunu kapatmak için **Kaydet** ' i seçin.

    ![Şablonu kaydet](media/azure-stack-create-vm-template/5.PNG)

6. **Parametreler**' i seçin, gerekli alanları doldurup değiştirin ve ardından **Tamam**' a tıklayın.

    ![Parametreler](media/azure-stack-create-vm-template/6.PNG)

7. Kullanılacak aboneliği seçin ve ardından var olan bir kaynak grubu adı oluşturun veya seçin. Ardından, şablon dağıtımını başlatmak için **Oluştur** ' u seçin.

    ![Abonelik seçin](media/azure-stack-create-vm-template/7.PNG)

8. Kaynak grubu dağıtımı, özel şablon tabanlı VM oluşturmak için birkaç dakika sürer. Yükleme durumunu bildirimler ve kaynak grubu özelliklerinden izleyebilirsiniz.

    ![Dağıtma](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > Dağıtım tamamlandığında VM çalışır.

## <a name="start-minikube-and-install-an-application"></a>Minikube başlatın ve bir uygulama yükler

Linux VM başarıyla oluşturuldığına göre, Minikube başlatmak ve bir uygulamayı yüklemek için oturum açabilirsiniz.

1. Dağıtım tamamlandıktan sonra, Linux VM 'sine bağlanmak için kullanılacak genel IP adresini görüntülemek için **Bağlan** ' ı seçin.

    ![Bağlan](media/azure-stack-create-vm-template/9.PNG)

2. Yükseltilmiş bir komut isteminden, Uzak Masaüstü Bağlantısı açmak ve önceki adımda bulunan Linux VM genel IP adresine bağlanmak için **mstsc. exe** ' yi çalıştırın. XRDP 'de oturum açmanız istendiğinde, VM oluştururken belirttiğiniz kimlik bilgilerini kullanın.

    ![Remote](media/azure-stack-create-vm-template/10.PNG)

3. Terminal öykünücüsünü açın ve Minikube başlatmak için aşağıdaki komutları girin:

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![Çalıştırılan komutlar](media/azure-stack-create-vm-template/11.PNG)

4. Bir tarayıcı açın ve Kubernetes Pano adresine gidin. Tebrikler, artık Minikube kullanarak tamamen çalışan bir Kubernetes yüklemesine sahipsiniz!

    ![Pano](media/azure-stack-create-vm-template/12.PNG)

5. Örnek bir uygulama dağıtmak için resmi Kubernetes belgeleri sayfasını ziyaret edin ve zaten bir tane oluşturduğunuz için "Minikube kümesi oluştur" bölümünü atlayın. @No__t-0 ' da "Node. js uygulamanızı oluşturma" bölümüne atlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonları hakkında bilgi edinin
> * Özel bir GitHub şablonu kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükler
