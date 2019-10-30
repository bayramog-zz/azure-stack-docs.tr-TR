---
title: Azure Stack Azure Active Directory kullanarak Kubernetes dağıtma (Azure AD) | Microsoft Docs
description: Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack nasıl dağıtacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 902645ffcb6fda4afad76a1a258b55f0ace2b189
ms.sourcegitcommit: 0d27456332031ab98ba2277117395ae5ffcbb79f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73047253"
---
# <a name="deploy-kubernetes-to-azure-stack-using-azure-active-directory"></a>Azure Stack Azure Active Directory kullanarak Kubernetes dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!Note]  
> Kümeleri kavram kanıtı olarak dağıtmak için Kubernetes Azure Stack Market öğesini kullanın. Azure Stack üzerinde desteklenen Kubernetes kümeleri için [AKS altyapısını](azure-stack-kubernetes-aks-engine-overview.md)kullanın.

Bu makaledeki adımları izleyerek, kimlik yönetimi hizmetiniz olarak Azure Active Directory (Azure AD) kullandığınızda, tek ve eşgüdümlü bir işlemde Kubernetes için kaynakları dağıtmak ve ayarlamak üzere bu makaledeki adımları uygulayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için doğru izinlere sahip olduğunuzdan ve Azure Stack uygun olduğundan emin olun.

1. Azure Active Directory (Azure AD) kiracınızda uygulamalar oluşturabileceğiniz doğrulayın. Kubernetes dağıtımı için bu izinlere ihtiyacınız vardır.

    İzinlerinizi denetleme yönergeleri için bkz. [denetim Azure Active Directory izinleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

1. Azure Stack Linux VM 'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtara ihtiyaç duyarsınız.

    Anahtar oluşturma yönergeleri için bkz. [SSH anahtar üretimi](azure-stack-dev-start-howto-ssh-public-key.md).

1. Azure Stack kiracı portalınızda geçerli bir aboneliğiniz olduğunu ve yeni uygulamalar eklemek için yeterli genel IP adresiniz olduğunu denetleyin.

    Küme, Azure Stack **yönetici** aboneliğine dağıtılamaz. **Kullanıcı** aboneliği kullanmanız gerekir. 

1. Market 'te Kubernetes kümeniz yoksa Azure Stack yöneticinizle görüşün.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Azure 'da bir hizmet sorumlusu ayarlayın. Hizmet sorumlusu, uygulamanızın Azure Stack kaynaklara erişmesini sağlar.

1. Genel [Azure Portal](https://portal.azure.com)oturum açın.

1. Azure Stack örneğiyle ilişkili Azure AD kiracısı kullanarak oturum açtığınızdan emin olun. Azure araç çubuğundaki filtre simgesine tıklayarak oturum açma alanınızı değiştirebilirsiniz.

    ![AD kiracınızı seçin](media/azure-stack-solution-template-kubernetes-deploy/tenantselector.png)

1. Bir Azure AD uygulaması oluşturun.

    a. [Azure Portal](https://portal.azure.com)aracılığıyla Azure hesabınızda oturum açın.  
    b. **Azure Active Directory** >  ' i seçin**uygulama kayıtları** > **Yeni kayıt**.  
    c. Uygulama için bir ad ve URL sağlayın.  
    d. **Desteklenen hesap türlerini**seçin.  
    e.  Uygulamanın URI 'SI için `http://localhost` ekleyin. Oluşturmak istediğiniz uygulama türü için **Web** ' i seçin. Değerleri ayarladıktan sonra **Kaydet**' i seçin.

1. **Uygulama Kimliği**’ni not alın. Kümeyi oluştururken KIMLIĞE ihtiyacınız olacak. KIMLIĞE **hizmet sorumlusu ISTEMCI kimliği**olarak başvurulur.

1. Hizmet ilkesinin dikey penceresinde **yeni istemci parolası**' nı seçin. **Ayarlar** > **anahtarlar**. Hizmet ilkesi için bir kimlik doğrulama anahtarı oluşturmanız gerekir.

    a. **Açıklamayı**girin.

    b. **Süre sonu** **için süresiz** seçeneğini belirleyin.

    c. **Add (Ekle)** seçeneğini belirleyin. Anahtar dizesini aklınızda yapın. Kümeyi oluştururken anahtar dizesine ihtiyaç duyarsınız. Anahtara **hizmet sorumlusu Istemci parolası**olarak başvurulur.

## <a name="give-the-service-principal-access"></a>Hizmet sorumlusu erişimi verme

Sorumlunun kaynak oluşturabilmesi için aboneliğinize hizmet sorumlusu erişimi verin.

1.  [Azure Stack portalında](https://portal.local.azurestack.external/)oturum açın.

1. **Tüm hizmetler** > **abonelikleri**' ni seçin.

1. Kubernetes kümesini kullanmak için operatörünüz tarafından oluşturulan aboneliği seçin.

1. **Erişim denetimi (IAM)** seçeneğini belirleyin > **rol ataması Ekle**' yi seçin.

1. **Katkıda bulunan** rolünü seçin.

1. Hizmet sorumlusu için oluşturulan uygulama adını seçin. Arama kutusuna adı yazmanız gerekebilir.

1. **Kaydet** düğmesine tıklayın.

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. [Azure Stack portalını](https://portal.local.azurestack.external)açın.

1. **Kubernetes kümesi** > **Işlem** > **kaynak oluştur** ' u seçin. **Oluştur**’a tıklayın.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. temel bilgiler

1. Kubernetes kümesi oluşturma bölümünde **temel bilgiler** ' i seçin.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. **Abonelik** kimliğinizi seçin.

1. Yeni bir kaynak grubunun adını girin veya var olan bir kaynak grubunu seçin. Kaynak adının alfasayısal ve küçük harf olması gerekir.

1. Kaynak grubunun **konumunu** seçin. Bu, Azure Stack yüklemeniz için seçtiğiniz bölgedir.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes kümesi ayarları

1. Kubernetes kümesi oluşturma ' da **Kubernetes kümesi ayarları** ' nı seçin.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-aad.png)

1. **LINUX VM yönetici kullanıcı adını**girin. Kubernetes kümesinin ve DVD 'nin parçası olan Linux Sanal Makineleri Kullanıcı adı.

1. Kubernetes kümesi ve DVD 'nin bir parçası olarak oluşturulan tüm Linux makinelere yetkilendirme için kullanılan **SSH ortak anahtarını** girin.

1. Bölgeye özgü olan **ana PROFIL DNS ön ekini** girin. Bu, `k8s-12345` gibi bir bölgeye özgü ad olmalıdır. Kaynak grubu adı ile aynı uygulamayı en iyi yöntemle seçmeyi deneyin.

    > [!Note]  
    > Her küme için yeni ve benzersiz bir ana profil DNS öneki kullanın.

1. **Kubernetes ana havuz profili sayısını**seçin. Sayı, ana havuzdaki düğümlerin sayısını içerir. 1 ile 7 arasında olabilir. Bu değer tek bir sayı olmalıdır.

1. **Kubernetes ana VM 'Lerinin VMSize boyutunu**seçin. Bu, Kubernetes ana VM 'lerinin VM boyutunu belirtir. 

1. **Kubernetes düğüm havuzu profil sayısını**seçin. Sayı kümedeki aracıların sayısını içerir. 

1. **Kubernetes düğüm VM 'Lerinin VMSize**öğesini seçin. Bu, Kubernetes düğüm VM 'lerinin VM boyutunu belirtir. 

1. Azure Stack yüklemenizin **Azure Stack kimlik sistemi** IÇIN **Azure AD** ' yi seçin.

1. Bu, Kubernetes Azure bulut sağlayıcısı tarafından kullanılan **hizmet sorumlusu ClientID** değerini girin. Azure Stack yöneticiniz hizmet sorumlusunu oluştururken uygulama KIMLIĞI olarak tanımlanan Istemci KIMLIĞI.

1. **Hizmet sorumlusu istemci gizli**anahtarını girin. Bu, hizmetinizi oluştururken ayarladığınız istemci sırrı.

1. **Kubernetes sürümünü**girin. Bu, Kubernetes Azure sağlayıcısı 'nın sürümüdür. Azure Stack her Azure Stack sürümü için özel bir Kubernetes derlemesini yayınlar.

### <a name="3-summary"></a>3. Özet

1. Özet ' i seçin. Dikey pencere, Kubernetes küme yapılandırmalarının ayarları için bir doğrulama iletisi görüntüler.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Ayarlarınızı gözden geçirin.

3. Kümenizi dağıtmak için **Tamam ' ı** seçin.

> [!TIP]  
>  Dağıtımınız hakkında sorularınız varsa sorunuzu gönderebilir veya birisinin [Azure Stack forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)soruyu zaten cevaplamış olup olmadığını görebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

[Kümenize bağlanma](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Kubernetes panosunu etkinleştirme](azure-stack-solution-template-kubernetes-dashboard.md)
