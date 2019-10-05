---
title: Azure Stack ' de sanal makine ölçek kümelerini kullanılabilir hale getirin | Microsoft Docs
description: Bir bulut işlecinin Azure Stack Market 'e nasıl sanal makine ölçek kümeleri ekleyebileceğinizi öğrenin.
services: azure-stack
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 10/04/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: ed73441f1a8d3c1c722ce35d5deda9ab7387283b
ms.sourcegitcommit: f91979c1613ea1aa0e223c818fc208d902b81299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974087"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Azure Stack ' de sanal makine ölçek kümelerini kullanılabilir hale getirin

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0
  
Sanal Makine Ölçek Kümeleri Azure Stack işlem kaynağıdır. Bunları, bir özdeş sanal makine (VM) kümesini dağıtmak ve yönetmek için kullanabilirsiniz. Aynı şekilde yapılandırılmış tüm VM 'Ler ile, ölçek kümeleri VM 'lerin ön sağlamasını gerektirmez. Büyük işlem, büyük veri ve Kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmak daha kolaydır.

Bu makale, Azure Stack marketi 'nde ölçek kümeleri kullanılabilir hale getirme sürecinde size rehberlik eder. Bu yordamı tamamladıktan sonra, kullanıcılarınız aboneliklerine sanal makine ölçek kümeleri ekleyebilir.

Azure Stack üzerindeki sanal makine ölçek kümeleri, Azure 'daki sanal makine ölçek kümelerine benzerdir. Daha fazla bilgi için aşağıdaki videoları inceleyin:

* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman ile sanal makine ölçek kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure Stack, sanal makine ölçek kümeleri otomatik ölçeklendirmeyi desteklemez. Kaynak Yöneticisi şablonları, CLı veya PowerShell kullanarak bir ölçek kümesine daha fazla örnek ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* **Azure Stack marketi:** Azure Stack Market 'teki öğelerin kullanılabilirliğini etkinleştirmek için genel Azure ile Azure Stack kaydedin. [Azure Ile kayıt Azure Stack](azure-stack-registration.md)içindeki yönergeleri izleyin.
* **İşletim sistemi görüntüsü:** Bir sanal makine ölçek kümesi oluşturulabilmesi için, [Azure Stack marketi](azure-stack-download-azure-marketplace-item.md)'ndeki ölçek kümesinde kullanmak üzere VM görüntülerini indirmeniz gerekir. Bir kullanıcının yeni bir ölçek kümesi oluşturabilmek için önce görüntülerin zaten mevcut olması gerekir.

## <a name="use-the-azure-stack-portal"></a>Azure Stack portalını kullanma

>[!IMPORTANT]  
> Bu bölümdeki bilgiler 1808 veya sonraki bir sürümü Azure Stack kullandığınızda geçerlidir. Sürümünüz 1807 veya daha önceki bir sürümdeyse, bkz. [sanal makine ölçek kümesini ekleme (1808 ' den önce)](#add-the-virtual-machine-scale-set-prior-to-version-1808).

1. Azure Stack portalında oturum açın. Ardından, **tüm hizmetler**'e ve ardından **sanal makine ölçek kümelerine**gidin ve **işlem**altında **Sanal Makine Ölçek Kümeleri**' ni seçin.
   ![Sanal makine ölçek kümelerini seçin @ no__t-1

2. ***Sanal Makine Ölçek Kümeleri oluştur***' u seçin.
   ![Sanal makine ölçek kümesi oluşturma](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. Boş alanları doldur, **işletim sistemi disk görüntüsü**, **abonelik**ve **örnek boyutu**için açılan kutudan seçim yapın. **Yönetilen diskleri kullanmak**için **Evet** ' i seçin. Ardından, **Oluştur**'u tıklatın.
    ![Yapılandırma ve sanal makine ölçek kümelerini oluşturma @ no__t-1

4. Yeni sanal makine ölçek kümesini görmek için **tüm kaynaklar**' a gidin, sanal makine ölçek kümesi adını arayın ve ardından aramada adını seçin.
   ![Sanal makine ölçek kümesini görüntüle @ no__t-1

## <a name="add-the-virtual-machine-scale-set-prior-to-version-1808"></a>Sanal makine ölçek kümesini ekleyin (sürüm 1808 ' den önce)

>[!IMPORTANT]  
> Bu bölümdeki bilgiler 1808 ' den önceki bir Azure Stack sürümünü kullandığınızda geçerlidir. Sürüm 1808 veya sonraki bir sürümü kullanıyorsanız, bkz. [Azure Stack portalını kullanma](#use-the-azure-stack-portal).

1. Azure Stack marketi 'ni açın ve Azure 'a bağlanın. **Market yönetimi**' ni seçin ve ardından **Azure 'dan + Ekle**' ye tıklayın.

    ![Market yönetimi Azure Stack](media/azure-stack-compute-add-scalesets/image01.png)

2. Sanal makine ölçek kümesi Market öğesini ekleyin ve indirin.

    ![Sanal makine ölçek kümesi Market öğesi](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesindeki görüntüleri güncelleştirme

Bir sanal makine ölçek kümesi oluşturduktan sonra, kullanıcılar ölçek kümesindeki görüntüleri, ölçek kümesinin yeniden oluşturulması gerekmeden güncelleştirebilir. Bir görüntüyü güncelleştirme işlemi aşağıdaki senaryolara bağlıdır:

1. Sanal makine ölçek kümesi dağıtım şablonu **Sürüm**için **en son** şunları belirtir:  

   @No__t-0, bir ölçek kümesi için şablonun `imageReference` bölümünde **en son** olarak ayarlandığında, ölçek kümesindeki ölçeği genişletme işlemleri, ölçek kümesi örnekleri için görüntünün en yeni kullanılabilir sürümünü kullanır. Ölçeği tamamladıktan sonra, eski sanal makine ölçek kümeleri örneklerini silebilirsiniz. @No__t-0, `offer` ve `sku` değerleri değişmeden kalır.

   Aşağıdaki JSON örneği @no__t belirtir-0:  

    ```json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Ölçek kullanılmadan önce yeni bir görüntü kullanabilmeniz için, bu yeni görüntüyü indirmeniz gerekir:  

   * Azure Stack Market 'teki görüntü, ölçek kümesindeki görüntüden daha yeni bir sürümse, eski görüntünün yerini alan yeni görüntüyü indirin. Görüntü değiştirildikten sonra, bir Kullanıcı ölçeği büyütme işlemine devam edebilir.

   * Azure Stack Market 'teki görüntü sürümü ölçek kümesindeki görüntüyle aynı olduğunda, ölçek kümesinde kullanımda olan görüntüyü silin ve ardından yeni görüntüyü indirin. Orijinal görüntünün kaldırılması ve yeni görüntünün indirilmesi arasındaki süre boyunca ölçeklendiremez.

   Bu işlem, sürüm 1803 ile tanıtılan seyrek dosya biçimini kullanan görüntülerin yeniden eşitlenmesi için gereklidir.

2. Sanal makine ölçek kümesi dağıtım şablonu **Sürüm** için **en son** ' i belirtmez ve bunun yerine bir sürüm numarası belirtir:  

    Daha yeni bir sürüme sahip (kullanılabilir sürümü değiştiren) bir görüntü indirirseniz, ölçek kümesi ölçeklenmez. Bu tasarım, ölçek kümesi şablonunda belirtilen görüntü sürümü kullanılabilir olmalıdır.  

Daha fazla bilgi için bkz. [işletim sistemi diskleri ve görüntüleri](../user/azure-stack-compute-overview.md#operating-system-disks-and-images).  

## <a name="scale-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini ölçeklendirme

Bir sanal makine ölçek kümesinin boyutunu daha büyük veya daha küçük hale getirmek için ölçeklendirebilirsiniz.

1. Portalda ölçek kümesini seçin ve ardından **ölçekleme**' ı seçin.

2. Bu sanal makine ölçek kümesi için yeni ölçek düzeyini ayarlamak üzere slayt çubuğunu kullanın ve ardından **Kaydet**' e tıklayın.

     ![Sanal makine kümesini ölçeklendirin](media/azure-stack-compute-add-scalesets/scale.png)

## <a name="remove-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesini kaldır

Bir sanal makine ölçek kümesi Galerisi öğesini kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell  
Remove-AzsGalleryItem
```

> [!NOTE]
> Galeri öğesi hemen kaldırılmayabilir. Öğe Azure Stack Market 'ten kaldırıldıkça, portalı birkaç kez yenilemeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
