---
title: Azure Stack hizmet teklifini test edin.
description: Bir abonelik oluşturup kaynak dağıtarak bir hizmet teklifini test etme hakkında bilgi edinin.
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: tutorial
ms.date: 10/13/2019
ms.reviewer: shriramnat
ms.lastreviewed: 10/06/2019
ms.openlocfilehash: 63c8e37c19b46f5cabe197dd55875e9bcbd5cb12
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286998"
---
# <a name="tutorial-test-a-service-offering"></a>Öğretici: hizmet sunumunu test etme

Önceki öğreticide, kullanıcılar için bir teklif oluşturdunuz. Bu öğreticide, bir abonelik oluşturmak için kullanarak bu teklifin nasıl test yapılacağı gösterilir. Daha sonra, aboneliğin sahip olduğu temel hizmetlerde kaynak oluşturup dağıtabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Abonelik oluşturma
> * Kaynak oluşturma ve dağıtma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce, aşağıdaki önkoşulları gerçekleştirmeniz gerekir:

- [Kullanıcılara hizmet sunma](tutorial-offer-services.md) öğreticisini doldurun. Bu öğreticide, bu öğretici tarafından kullanılan teklifin nasıl oluşturulacağını öğreneceksiniz.

- Bu öğreticide abone olduğunuz teklif, bir sanal makine (VM) kaynağının dağıtımını sağlar. VM dağıtımını test etmek isterseniz, önce Azure Marketi 'nden indirerek Azure Stack marketi 'nde bir VM görüntüsünü kullanılabilir yapmanız gerekir. Yönergeler için bkz. [Azure 'dan Market öğelerini indirme Azure Stack](azure-stack-download-azure-marketplace-item.md) . 

## <a name="subscribe-to-the-offer"></a>Teklife abone olma

1. Kullanıcı hesabıyla Kullanıcı Portalı 'nda oturum açın 

   - Tümleşik bir sistem için, URL, işlecin bölgesine ve dış etki alanı adına göre, https://portal.&ltbiçimini kullanarak farklılık gösterir. *bölge*&gt;.&lt;*FQDN*&gt;.
   - Azure Stack Geliştirme Seti kullanıyorsanız, Portal adresi https://portal.local.azurestack.external.

1. **Abonelik al** kutucuğunu seçin.

   ![Abonelik al](media/tutorial-test-offer/1-get-subscription.png)

1. **Abonelik al**bölümünde, **görünen ad** alanına yeni aboneliğiniz için bir ad girin. **Teklif**' i seçin ve ardından **bir teklif seçin** listesinden önceki öğreticide oluşturduğunuz teklifi seçin. **Oluştur**’u seçin.

   ![Teklif oluşturma](media/tutorial-test-offer/2-create-subscription.png)

1. Aboneliği görüntülemek için **tüm hizmetler**' i seçin ve ardından **genel** kategori altında **abonelikler**' i seçin. İlişkilendirildiği teklifi ve özelliklerini görüntülemek için yeni aboneliğinizi seçin.

   >[!NOTE]
   >Teklife abone olduktan sonra, hangi hizmetlerin yeni aboneliğin parçası olduğunu görmek için portalı yenilemeniz gerekebilir.

::: moniker range=">=azs-1902"
## <a name="deploy-a-storage-account-resource"></a>Depolama hesabı kaynağı dağıtma

Kullanıcı portalından, önceki bölümde oluşturduğunuz aboneliği kullanarak bir depolama hesabı temin edersiniz.

1. Kullanıcı Portalı ' nda kullanıcı hesabıyla oturum açın.

1. **+ Bir kaynak oluştur** > **veri + depolama** > **depolama hesabı-blob, dosya, tablo, kuyruk '** ı seçin.

1. **Depolama hesabı oluştur**' da, aşağıdaki bilgileri sağlayın:
  
   - Bir **ad** girin
   - Yeni **aboneliğinizi** seçin
   - Bir **kaynak grubu** seçin (veya bir tane oluşturun.) 
   - Depolama hesabını oluşturmak için **Oluştur**’u seçin.

1. Dağıtım başladıktan sonra panoya geri dönersiniz. Yeni depolama hesabını görmek için **tüm kaynaklar**' ı seçin. Depolama hesabını arayın ve arama sonuçlarından adını seçin. Buradan, depolama hesabını ve içeriğini yönetebilirsiniz.

## <a name="deploy-a-virtual-machine-resource"></a>Sanal makine kaynağı dağıtma

Kullanıcı portalından, önceki bölümde oluşturduğunuz aboneliği kullanarak bir sanal makine hazırlarsınız.

1. Kullanıcı Portalı ' nda kullanıcı hesabıyla oturum açın.

1. "Görüntü-adı", önkoşullardan indirdiğiniz sanal makinenin adı olan **\<görüntü adı\>** > > **işlem** **Oluştur** ' u seçin.
1. **Sanal makine oluşturma** / **temelleri**bölümünde aşağıdaki bilgileri sağlayın:
  
   - VM için bir **ad** girin.
   - Yönetici hesabı için bir **Kullanıcı adı** girin.
   - Linux sanal makineleri için, **kimlik doğrulama türü**Için "parola" yı seçin.
   - Yönetici hesabı için bir **parola** ve parolayı **Onayla**için aynısını girin.
   - Yeni **aboneliğinizi**seçin.
   - Bir **kaynak grubu** seçin (veya bir tane oluşturun). 
   - Bu bilgileri doğrulamak ve devam etmek için **Tamam** ' ı seçin.

1. **Boyut seçin**' de, gerekirse listeyi filtreleyin, BIR VM SKU 'su seçin ve **Seç**' i seçin.  
1. **Ayarlar**' da, **Genel gelen bağlantı noktalarını seçin**altında açılacak bağlantı noktalarını belirtin ve **Tamam**' ı seçin.
   > [!NOTE]
   > Örneğin, "RDP (3389)" seçilmesi, çalışırken VM 'ye uzaktan bağlanmanızı sağlar.
1. **Özet**bölümünde, seçimlerinizi gözden geçirin ve ardından sanal makineyi oluşturmak için **Tamam** ' ı seçin.  
1. Dağıtım başladıktan sonra panoya geri dönersiniz. Yeni sanal makineyi görmek için **tüm kaynaklar**' ı seçin. Sanal makineyi arayın ve arama sonuçlarından adını seçin. Buradan sanal makineye erişip yönetebilirsiniz.
   > [!NOTE]
   > Tam dağıtım ve VM 'nin başlatılması birkaç dakika sürebilir. VM kullanıma hazırlandıktan sonra [durum](/azure/virtual-machines/windows/states-lifecycle) "çalışıyor" olarak değişir.

::: moniker-end

::: moniker range="<=azs-1901"
## <a name="deploy-a-virtual-machine-resource-1901-and-earlier"></a>Bir sanal makine kaynağı dağıtma (1901 ve önceki sürümler)

Kullanıcı portalından, yeni aboneliği kullanarak bir sanal makine hazırlarsınız.

1. Kullanıcı Portalı ' nda kullanıcı hesabıyla oturum açın.

1. Panoda + **Windows Server 2016 Datacenter Eval**> **Işlem** > **kaynak oluştur** ' u seçin ve ardından **Oluştur**' u seçin.

1. **Temel bilgiler**bölümünde aşağıdaki bilgileri sağlayın:
  
   - Bir **ad** girin
   - Bir **Kullanıcı adı** girin
   - **Parola** girin
   - Yeni **aboneliğinizi** seçin
   - Bir **kaynak grubu** oluşturun (veya var olan bir grubu seçin.) 
   - Bu bilgileri kaydetmek için **Tamam ' ı** seçin.

1. **Boyut seçin**bölümünde **a1 standart**' ı seçin ve ardından öğesini **seçin**.  
1. **Ayarlar**' da, **sanal ağ**' ı seçin.

1. **Sanal ağ seçin**' de, **Yeni oluştur**' u seçin.

1. **Sanal ağ oluştur**' da, tüm varsayılanları kabul edin ve **Tamam**' ı seçin.

1. Ağ yapılandırmasını kaydetmek için **Ayarlar** ' da **Tamam ' ı** seçin.

1. **Özet**bölümünde, sanal makineyi oluşturmak için **Tamam** ' ı seçin.  

1. Yeni sanal makineyi görmek için **tüm kaynaklar**' ı seçin. Sanal makineyi arayın ve arama sonuçlarından adını seçin.
::: moniker-end

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Abonelik oluşturma
> * Kaynak oluşturma ve dağıtma 

Ardından, eklenti hizmetleri için kaynak sağlayıcılarını dağıtma hakkında bilgi edinin. Planlarınızda kullanıcılara daha fazla hizmet sunmanızı sağlar:

- [Azure Stack üzerinde SQL sunun](azure-stack-sql-resource-provider.md)
- [Azure Stack için MySQL sunun](azure-stack-mysql-resource-provider.md)
- [Azure Stack App Service teklif](azure-stack-app-service-overview.md)
