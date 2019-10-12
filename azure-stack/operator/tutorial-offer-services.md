---
title: Abonelik için Azure Stack hizmetleri sunun.
description: Teklifler, planlar ve hizmetler kullanarak bir hizmet sunumu oluşturmayı öğrenin.
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: tutorial
ms.date: 10/03/2019
ms.reviewer: shriramnat
ms.lastreviewed: 10/06/2019
ms.openlocfilehash: dc35f5249a9fb71722d51f2a47afc516a8d51379
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72287086"
---
# <a name="tutorial-offer-a-service-to-users"></a>Öğretici: kullanıcılara hizmet sunma

Bu öğreticide, bir teklifin nasıl oluşturulacağı bir operatör gösterilmektedir. Bir teklif, Hizmetleri bir abonelik temelinde kullanıcılara sunar. Bir teklifine abone olduktan sonra, Kullanıcı teklif tarafından belirtilen hizmetlerde kaynak oluşturup dağıtmaya hak kazanır.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Plan oluşturma
> * Plana hizmet ve Kotalar atama
> * Bir teklifin planını atama

## <a name="overview"></a>Genel bakış

Bir teklif bir veya daha fazla plandan oluşur. Bir plan, ilgili kaynak sağlayıcısını ve kotayı belirterek bir veya daha fazla hizmete erişim sahibine. Planlar, temel plan olarak bir teklifine eklenebilir veya teklifi bir eklenti planı olarak genişletebilir. Daha fazla bilgi edinmek için bkz. [hizmet, plan, teklif, aboneliğe genel bakış](service-plan-offer-subscription-overview.md).

![Abonelikler, teklifler ve planlar](media/azure-stack-key-features/image4.png)

### <a name="resource-providers"></a>Kaynak sağlayıcıları

Kaynak sağlayıcısı, kaynaklarını hizmet olarak oluşturmayı, dağıtımını ve yönetimini destekler. Ortak bir örnek, sanal makine (VM) oluşturma ve dağıtma özelliği sunan Microsoft. COMPUTE kaynak sağlayıcısıdır. Azure Kaynak Yönetimi modeline genel bir bakış için bkz. [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) .

Azure Stack, kaynak sağlayıcılarının iki genel kategorisi vardır: kaynakları temel hizmetler olarak dağıtmaları ve eklenti hizmetleri olarak dağıtmaları.

### <a name="foundational-services"></a>Temel hizmetler

>[!NOTE]
> Bu öğreticide temel hizmetler temelinde bir teklif oluşturmayı öğreneceksiniz. 

Temel hizmetler, Azure Stack her yüklemesiyle yerel olarak kullanılabilen aşağıdaki kaynak sağlayıcıları tarafından desteklenir:

| Kaynak sağlayıcısı | Örnek kaynaklar |
| ----------------- | ------------------|
| Microsoft. COMPUTE | Sanal makineler, diskler, sanal makine ölçek kümeleri |
| Microsoft. Keykasası | Anahtar kasaları, gizlilikler |
| Microsoft. Network | Sanal ağlar, genel IP adresleri, yük dengeleyiciler |
| Microsoft. Storage | Depolama hesapları, blob 'lar, kuyruklar, tablolar |

### <a name="add-on-services"></a>Eklenti Hizmetleri

>[!NOTE]
> Bir eklenti hizmeti sunmak için, önce karşılık gelen kaynak sağlayıcısının Azure Stack Market 'te yüklü olması gerekir. Yüklendikten sonra, kaynakları temel hizmetlerle aynı şekilde kullanıcılara sunulur. Eklenti hizmeti tekliflerini destekleyen geçerli kaynak sağlayıcıları kümesi için lütfen TOC 'nin **nasıl yapılır Kılavuzu** bölümüne bakın.

Eklenti Hizmetleri, Azure Stack dağıtıldıktan sonra yüklenen kaynak sağlayıcıları tarafından desteklenir. Örnekler:

| Kaynak sağlayıcısı | Örnek kaynaklar |
| ----------------- | ------------------------- |
| Microsoft. Web | App Service işlevi uygulamaları, Web uygulamaları, API Apps | 
| Microsoft. MySqlAdapter | MySQL barındırma sunucusu, MySQL veritabanı | 
| Microsoft. SqlAdapter | SQL Server barındırma sunucusu, SQL Server veritabanı |

::: moniker range=">=azs-1902"
## <a name="create-an-offer"></a>Teklif oluşturma

Teklif oluşturma işlemi sırasında hem teklif hem de bir plan oluşturursunuz. Plan, teklifin temel planı olarak kullanılır. Plan oluşturma sırasında, planda kullanıma sunulan hizmetleri ve ilgili kotaları belirtirsiniz.

1. Yönetici portalında bir bulut yönetici hesabıyla oturum açın.

   - Tümleşik bir sistem için, URL, işlecin bölgesine ve dış etki alanı adına göre değişir https://adminportal.&lt; biçimini kullanarak *bölge*&gt;. &lt;*FQDN*&gt;.
   - Azure Stack Geliştirme Seti kullanıyorsanız, URL https://adminportal.local.azurestack.external ' dır.

   Ardından **+ @no__t kaynak oluştur** ' u seçin-3 teklif **@no__t-** 3 **teklif**.

   ![Yeni teklif](media/tutorial-offer-services/1-create-resource-offer.png)

1. **Temel bilgiler** sekmesinde **Yeni bir teklif oluştur** ' da, bir **görünen ad**, **kaynak adı**girin ve var olan bir kaynak grubu seçin veya yeni bir **kaynak grubu**oluşturun. Görünen ad, teklifin kolay adıdır. Yalnızca bulut operatörü, yöneticilerin teklifle birlikte Azure Resource Manager kaynağı olarak çalışmak için kullandığı ad olan kaynak adını görebilir.

   ![Görünen ad](media/tutorial-offer-services/2-create-new-offer.png)

1. **Temel planlar** sekmesini seçin, yeni bir plan oluşturmak için **Yeni plan oluştur** ' u seçin. Plan, bir temel plan olarak teklifine da eklenecektir.

   ![Plan ekleme](media/tutorial-offer-services/3-create-new-offer-base-plans.png)

1. **Temel bilgiler** sekmesinin altındaki **Yeni plana** bir **görünen ad** ve **kaynak adı**girin. Görünen ad, kullanıcıların göreceği planın kolay adıdır. Yalnızca bulut operatörü, bulut işleçlerinin Azure Resource Manager kaynak olarak planla birlikte çalışmak için kullandığı ad olan kaynak adını görebilir. **Kaynak grubu** , teklif için belirtilen birine ayarlanır.

   ![Görünen ad planı](media/tutorial-offer-services/4-create-new-plan-basics.png)

1. **Hizmetler** sekmesini seçin ve yüklü kaynak sağlayıcılarından kullanılabilen hizmetlerin bir listesini görürsünüz. **Microsoft. COMPUTE**, **Microsoft. Network**ve **Microsoft. Storage**' ı seçin. 

   ![Plan hizmetleri](media/tutorial-offer-services/5-create-new-plan-services.png)

1. **Kotalar** sekmesini seçin ve bu plan için etkinleştirdiğiniz hizmetlerin listesini görürsünüz. **Microsoft. COMPUTE**için özel bir kota belirtmek Için **Yeni oluştur** ' a tıklayın. Kota **adı** gereklidir; Her kota değerini kabul edebilir veya değiştirebilirsiniz. Bittiğinde **Tamam** ' ı seçin, ardından kalan hizmetler için bu adımları tekrarlayın.

   ![İşlem kotası oluştur](media/tutorial-offer-services/6-create-new-plan-quotas.png)

1. **Gözden geçir + oluştur** sekmesini seçin. En üstte yeşil bir "doğrulama geçildi" başlığı görmeniz gerekir ve bu, yeni temel planın oluşturulması için hazırlandığını gösterir. **Oluştur**' u seçin. Ayrıca planın oluşturulduğunu belirten bir bildirim görmeniz gerekir.

   ![Yeni plan oluştur](media/tutorial-offer-services/7-create-new-plan-review-create.png)

1. **Yeni teklif oluştur** sayfasının **temel planlar** sekmesine döndükten sonra planın oluşturulduğunu fark edersiniz. Temel plan olarak teklifte eklenmek üzere yeni planın seçildiğinden emin olun ve ardından **gözden geçir + oluştur**' u seçin.

   ![Temel plan Ekle](media/tutorial-offer-services/8-create-new-offer-base-plans-done.png)

1. **Gözden geçir + oluştur** sekmelerinde, üstteki yeşil "doğrulama geçildi" başlığını görmeniz gerekir. "Temel" ve "temel planlar" bilgilerini gözden geçirin ve hazırsanız **Oluştur** ' u seçin. 

   ![Yeni teklif oluştur](media/tutorial-offer-services/9-create-new-offer-review-create.png)

1. "Dağıtımınız devam eder" sayfasında, teklif dağıtıldıktan sonra "dağıtımınız tamamlanmıştır" şeklinde görüntülenir. **Kaynak** sütununun altında, teklifin adına tıklayın.

   ![Teklif dağıtımı Tamam](media/tutorial-offer-services/10-offer-deployment-complete.png)


1. Ana başlığa dikkat edin, bu da teklifinizin, kullanıcıların abone olmasını önleyen, hala özel olduğunu gösterir. **Durumu Değiştir**' i seçip **genel**' i seçerek ortak olarak değiştirin.

    ![Genel durum](media/tutorial-offer-services/11-offer-change-state.png)
::: moniker-end

::: moniker range="<=azs-1901"
## <a name="create-an-offer-1901-and-earlier"></a>Teklif oluşturma (1901 ve önceki sürümler)

Teklif oluşturma işlemi sırasında hem teklif hem de bir plan oluşturursunuz. Plan, teklifin temel planı olarak kullanılır. Plan oluşturma sırasında, planda kullanıma sunulan hizmetleri ve ilgili kotaları belirtirsiniz.

1. Yönetici portalında bir bulut yönetici hesabıyla oturum açın.

   - Tümleşik bir sistem için, URL, işlecin bölgesine ve dış etki alanı adına göre değişir https://adminportal.&lt; biçimini kullanarak *bölge*&gt;. &lt;*FQDN*&gt;.
   - Azure Stack Geliştirme Seti kullanıyorsanız, URL https://adminportal.local.azurestack.external ' dır.
   
   Ardından **+ @no__t kaynak oluştur** ' u seçin-3 teklif **@no__t-** 3 **teklif**.

   ![Yeni teklif](media/tutorial-offer-services/image01.png)

1. **Yeni teklifte**, bir **görünen ad** ve **kaynak adı**girin ve ardından yeni veya var olan bir **kaynak grubu**seçin. Görünen ad, teklifin kolay adıdır. Yalnızca bulut operatörü, yöneticilerin teklifle birlikte Azure Resource Manager kaynağı olarak çalışmak için kullandığı ad olan kaynak adını görebilir.

   ![Görünen ad](media/tutorial-offer-services/image02.png)

1. **Temel planlar**' ı seçin ve **plan** bölümünde **Ekle** ' yi seçerek teklifine yeni bir plan ekleyin.

   ![Plan ekleme](media/tutorial-offer-services/image03.png)

1. **Yeni plan** bölümünde **görünen ad** ve **kaynak adı**' nı girin. Görünen ad, kullanıcıların göreceği planın kolay adıdır. Yalnızca bulut operatörü, bulut işleçlerinin Azure Resource Manager kaynak olarak planla birlikte çalışmak için kullandığı ad olan kaynak adını görebilir.

   ![Görünen ad planı](media/tutorial-offer-services/image04.png)

1. **Hizmetler**' i seçin. Hizmetler listesinden **Microsoft. COMPUTE**, **Microsoft. Network**ve **Microsoft. Storage**' ı seçin. Bu hizmetleri plana eklemek için **Seç ' i** seçin.

   ![Plan hizmetleri](media/tutorial-offer-services/image05.png)

1. **Kotalar**' ı seçin ve ardından kota oluşturmak istediğiniz ilk hizmeti seçin. IaaS kotası için, Işlem, ağ ve depolama hizmetleri için kotaları yapılandırmaya yönelik bir kılavuz olarak aşağıdaki örneği kullanın.

   - İlk olarak, Işlem hizmeti için bir kota oluşturun. Ad alanı listesinde **Microsoft. COMPUTE** ' i seçin ve ardından **yeni kota oluştur**' u seçin.

     ![Yeni kota oluştur](media/tutorial-offer-services/image06.png)

   - **Kota oluştur**bölümünde kota için bir ad girin. Gösterilen kota değerlerinden herhangi birini değiştirebilir veya kabul edebilirsiniz. Bu örnekte, varsayılan ayarları kabul ediyoruz ve **Tamam**' ı seçmelisiniz.

     ![Kota adı](media/tutorial-offer-services/image07.png)

   - Ad alanı listesinde **Microsoft. COMPUTE** ' i seçin ve ardından oluşturduğunuz kotayı seçin. Bu adım, kotayı Işlem hizmetine bağlar.

     ![Kota seçin](media/tutorial-offer-services/image08.png)

      Ağ ve depolama hizmetleri için bu adımları yineleyin. İşiniz bittiğinde, tüm kotaları kaydetmek için **Kotalar** bölümünde **Tamam** ' ı seçin.

1. **Yeni planda** **Tamam**' ı seçin.

1. **Plan**altında yeni planı seçin ve ardından öğesini **seçin**.

1. **Yeni teklifte** **Oluştur**' u seçin. Teklif oluşturulduğunda bir bildirim görürsünüz.

1. Pano menüsünde, **teklifler** ' i seçin ve ardından oluşturduğunuz teklifi seçin.

1. **Durumu Değiştir**' i ve ardından **genel**' i seçin.

    ![Genel durum](media/tutorial-offer-services/image09.png)
::: moniker-end
 
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Plan oluşturma
> * Plana hizmet ve Kotalar atama
> * Bir teklifin planını atama

Nasıl yapılacağını öğrenmek için bir sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Bu öğreticide sunulan hizmetleri test etme](tutorial-test-offer.md)
