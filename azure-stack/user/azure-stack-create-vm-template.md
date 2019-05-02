---
title: Bu öğreticide, Azure Stack şablon kullanarak VM oluşturma | Microsoft Docs
description: Önceden tanımlanmış bir şablon ve GitHub özel bir şablon kullanarak VM oluşturmak için ASDK kullanmayı açıklar.
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
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: a663a5c45c542ac4bfa19266c73066f8e41ba5d8
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64303348"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Öğretici: topluluk şablonu kullanarak bir VM oluşturma

Bir Azure Stack operatörü veya kullanıcı olarak kullanarak bir sanal makine (VM) oluşturabilirsiniz [özel GitHub hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) Azure Stack marketini şablondan el ile dağıtma yerine.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonlarını kullanma
> * Bir özel GitHub şablon kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin

## <a name="azure-stack-quickstart-templates"></a>Azure Stack hızlı başlangıç şablonları

Azure Stack hızlı başlangıç şablonları GitHub depolanan [genel Azure Stack hızlı başlangıç şablonları depo](https://github.com/Azure/AzureStack-QuickStart-Templates). Bu depo, Microsoft Azure Stack geliştirme Seti'ni (ASDK) ile test edilmiştir Azure Resource Manager dağıtım şablonlarını içerir. Azure Stack'i değerlendirin ve ASDK ortamınızda kolaylaştırmak için bunları kullanabilirsiniz.

Zaman içinde çok sayıda GitHub kullanıcı 400'den fazla dağıtım şablonları koleksiyonu içinde elde edilen depoya katkıda buluna. Azure Stack için çeşitli ortamların nasıl dağıtabilirsiniz daha iyi anlamak için harika bir başlangıç noktası depodur.

>[!IMPORTANT]
> Bu şablonların bazılarını, topluluk üyeleri tarafından ve Microsoft tarafından oluşturulur. Her şablon, Microsoft değil, sahibi tarafından bir lisans sözleşmesi altında lisanslanır. Microsoft, bu şablonları için sorumlu değildir ve güvenlik, uyumluluk veya performansı incelemez. Topluluk Şablonları herhangi bir Microsoft destek programı veya hizmeti altında desteklenmez ve kullanıma sunulan ", hiçbir garanti olmaksızın olduğu gibi".

Azure Resource Manager şablonlarını Github'da katkıda bulunmak isterseniz, katkılarınız için olun [AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates) depo. Bu depo ve kendisine katkıda bulunma hakkında daha fazla bilgi için bkz: [Benioku dosyası](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md).

## <a name="create-a-vm-using-a-custom-github-template"></a>Bir özel GitHub şablon kullanarak VM oluşturma

Bu örnek öğreticide [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure Stack Hızlı Başlangıç şablonu, bir Ubuntu 16.04 sanal makinesini Azure Stack'te çalışan bir Kubernetes kümesini yönetmek için Minikube dağıtmak için kullanılır.

Minikube Kubernetes yerel olarak çalıştırılmasına olanak sağlayan bir araçtır. Minikube Kubernetes deneyin veya onunla geliştirme olanak tanıyarak, bir VM içinde tek düğümlü bir Kubernetes kümesi günlük çalıştırır. Bu, bir Linux VM'de çalışan basit, tek düğümlü Kubernetes kümesi destekler. Minikube çalıştıran tam olarak işlevsel bir Kubernetes kümesi almak için kullanılabilecek en hızlı ve en kolay yoludur. Bu, geliştiricilerin geliştirip test Kubernetes tabanlı uygulama dağıtımlarını yerel kendi makinelerinde olanak tanır. Bu mimari, Minikube VM hem ana hem de aracı düğümü bileşenlerinin yerel olarak çalıştırır:

* Zamanlayıcı, API sunucusu gibi ana düğüm bileşenleri ve [etcd sunucu](https://coreos.com/etcd/) adlı tek bir Linux işlem içinde çalıştırmak **LocalKube**.
* Tam olarak normal bir aracı düğüm üzerinde çalışır gibi aracı düğümü bileşenlerinin docker kapsayıcıları içinde çalışır. Bir uygulama dağıtım açısından fark yoktur arasında bir Minikube veya normal bir Kubernetes kümesinde uygulama dağıtıldığında.

Bu şablon, aşağıdaki bileşenleri yükler:

* Ubuntu 16.04 LTS VM
* [Docker CE](https://download.docker.com/linux/ubuntu)
* [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
* [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
* xFCE4
* xRDP

> [!IMPORTANT]
> Ubuntu VM görüntüsü (Ubuntu Server 16.04 LTS, bu örnekte) zaten Azure Stack marketini için bu adımları gerçekleştirmeden önce eklenmesi gerekir.

1. Seçin **+ kaynak Oluştur**, ardından **özel**, ardından **şablon dağıtımı**.

    ![Şablon oluşturma](media/azure-stack-create-vm-template/1.PNG)

2. Seçin **şablonu Düzen**.

    ![Bir şablon Düzenle](media/azure-stack-create-vm-template/2.PNG)

3. Seçin **Hızlı Başlangıç şablonu**.

    ![Hızlı Başlangıç şablonu](media/azure-stack-create-vm-template/3.PNG)

4. Seçin **101-vm-linux-minikube** kullanarak kullanılabilir şablonlardan **bir şablon seçin** açılan listeler ve ardından **Tamam**.

    ![Bir şablon seçin](media/azure-stack-create-vm-template/4.PNG)

5. JSON şablonu değişiklikler yapmak istiyorsanız bunu yapabilirsiniz. Aksi takdirde veya tamamlandığında seçin **Kaydet** kapatmak için **şablonu Düzen** iletişim.

    ![Şablonu kaydetme](media/azure-stack-create-vm-template/5.PNG)

6. Seçin **parametreleri**doldurun veya kullanılabilir alanları gerektiği gibi değiştirin ve ardından **Tamam**. Abonelik kullanmak, oluşturmak veya mevcut bir kaynak grubu adı seçin ve ardından seçin **Oluştur** şablon dağıtımını başlatmak için.

    ![Parametreler](media/azure-stack-create-vm-template/6.PNG)

7. Abonelik kullanmak, oluşturmak veya mevcut bir kaynak grubu adı seçin ve ardından seçin **Oluştur** şablon dağıtımını başlatmak için.

    ![Abonelik seçin](media/azure-stack-create-vm-template/7.PNG)

8. Kaynak grubu dağıtımı özel şablon tabanlı VM oluşturmak için birkaç dakika sürer. Yükleme durumu ve kaynak grubu özelliklerini bildirimler aracılığıyla izleyebilirsiniz.

    ![Dağıtma](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > Dağıtım tamamlandığında, VM çalıştırırsınız.

## <a name="start-minikube-and-install-an-application"></a>Minikube başlatın ve bir uygulama yükleyin

Linux sanal makinesi başarıyla oluşturuldu, size Minikube başlatmak ve uygulama yüklemek oturum açabilir.

1. Dağıtım tamamlandıktan sonra seçin **Connect** Linux VM'ye bağlanmak için kullanılan genel IP adresini görüntülemek için.

    ![Bağlan](media/azure-stack-create-vm-template/9.PNG)

2. Yükseltilmiş bir komut isteminden çalıştırın **mstsc.exe** Uzak Masaüstü Bağlantısı'nı açın ve önceki adımda bulunan Linux VM ortak IP adresine bağlanın. XRDP için oturum açmanız istendiğinde, sanal makine oluştururken belirttiğiniz kimlik bilgilerini kullanın.

    ![Uzak](media/azure-stack-create-vm-template/10.PNG)

3. Terminal öykünücü açın ve Minikube başlatmak için aşağıdaki komutları girin:

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![Çalıştırılan komutlar](media/azure-stack-create-vm-template/11.PNG)

4. Web tarayıcısı açın ve Kubernetes Panosu adresini ziyaret edin. Tebrikler, artık tam olarak çalışan sahipsiniz Minikube kullanarak Kubernetes yüklemesini!

    ![Pano](media/azure-stack-create-vm-template/12.PNG)

5. Örnek bir uygulama dağıtmak için resmi Kubernetes belgeleri sayfasını ziyaret edin ve önceden önceki adımlardan birinde oluşturduğunuz olarak "Minikube küme oluşturma" bölümüne atlayın. Adresindeki "Node.js uygulamanızı oluşturma" bölümüne atla https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonları hakkında bilgi edinin
> * Bir özel GitHub şablon kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin
