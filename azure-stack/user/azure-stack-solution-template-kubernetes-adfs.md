---
title: Azure Stack Active Directory Federasyon Hizmetleri 'ni kullanarak Kubernetes dağıtma (AD FS) | Microsoft Docs
description: Active Directory federe Hizmetleri (AD FS) kullanarak Kubernetes 'i Azure Stack nasıl dağıtacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: nav
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: d1ac66074f88ed131623888d8f1aa6ba044686b3
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277689"
---
# <a name="deploy-kubernetes-to-azure-stack-using-active-directory-federated-services"></a>Azure Stack Active Directory Federasyon Hizmetleri kullanarak Kubernetes dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack Kubernetes önizleme aşamasındadır. Azure Stack bağlantısı kesik bir senaryo şu anda önizleme tarafından desteklenmiyor. Yalnızca geliştirme ve test senaryoları için Market öğesini kullanın.

Kubernetes kaynaklarını dağıtmak ve ayarlamak için bu makaledeki adımları izleyebilirsiniz. Active Directory Federasyon Hizmetleri (AD FS) kimlik yönetimi hizmetiniz olduğunda bu adımları kullanın.

## <a name="prerequisites"></a>Prerequisites 

Başlamak için doğru izinlere sahip olduğunuzdan ve Azure Stack uygun olduğundan emin olun.

1. Azure Stack Linux VM 'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtar gerekir.

    Anahtar oluşturma yönergeleri için bkz. [SSH anahtar üretimi](azure-stack-dev-start-howto-ssh-public-key.md).

1. Azure Stack kiracı portalınızda geçerli bir aboneliğiniz olduğunu ve yeni uygulamalar eklemek için yeterli genel IP adresiniz olduğunu denetleyin.

    Küme, Azure Stack **yönetici** aboneliğine dağıtılamaz. **Kullanıcı** aboneliği kullanmanız gerekir. 

1. Market 'te Kubernetes kümeniz yoksa Azure Stack yöneticinizle görüşün.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kimlik çözümünüz olarak AD FS kullanırken hizmet sorumlunuzu ayarlamak için Azure Stack yöneticinizle birlikte çalışmanız gerekir. Hizmet sorumlusu, uygulamanızın Azure Stack kaynaklara erişmesini sağlar.

1. Azure Stack yöneticiniz, hizmet sorumlusu için bilgiler sağlar. Hizmet sorumlusu bilgileri şöyle görünmelidir:

     ```Text  
       ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
       ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
       ClientSecret          : <your client secret>
       Thumbprint            : <often this value is empty>
       ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
       PSComputerName        : 192.168.200.224
       RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
     ```

2. Yeni hizmet sorumlusuna, aboneliğinize katkıda bulunan bir rol atayın. Yönergeler için bkz. [rol atama](../operator/azure-stack-add-users-adfs.md).

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. [Azure Stack portalını](https://portal.local.azurestack.external)açın.

1. **+ @No__t kaynak oluştur**' u seçin-3**Kubernetes kümesi** **@no__t.** **Oluştur**' u seçin.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. temel bilgiler

1. Kubernetes kümesi oluşturma bölümünde **temel bilgiler** ' i seçin.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. **Abonelik** kimliğinizi seçin.

1. Yeni bir kaynak grubunun adını girin veya var olan bir kaynak grubunu seçin. Kaynak adının alfasayısal ve küçük harf olması gerekir.

1. Kaynak grubunun **konumunu** seçin. Bu, Azure Stack yüklemeniz için seçtiğiniz bölgedir.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes kümesi ayarları

1. Kubernetes kümesi oluşturma ' da **Kubernetes kümesi ayarları** ' nı seçin.

    ![Çözüm şablonu dağıtma](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-adfs.png)

1. **LINUX VM yönetici kullanıcı adını**girin. Kubernetes kümesinin ve DVD 'nin parçası olan Linux Sanal Makineleri Kullanıcı adı.

1. Kubernetes kümesi ve DVD 'nin bir parçası olarak oluşturulan tüm Linux makinelere yetkilendirme için kullanılan **SSH ortak anahtarını** girin.

1. Bölgeye özgü olan **ana PROFIL DNS ön ekini** girin. Bu, `k8s-12345` gibi bir bölgeye özgü ad olmalıdır. Kaynak grubu adı ile aynı uygulamayı en iyi yöntemle seçmeyi deneyin.

    > [!Note]  
    > Her küme için yeni ve benzersiz bir ana profil DNS öneki kullanın.

1. **Kubernetes ana havuz profili sayısını**seçin. Sayı, ana havuzdaki düğümlerin sayısını içerir. 1 ile 7 arasında olabilir. Bu değer tek bir sayı olmalıdır.

1. **Kubernetes ana VM 'Lerinin VMSize boyutunu**seçin.

1. **Kubernetes düğüm havuzu profil sayısını**seçin. Sayı kümedeki aracıların sayısını içerir. 

1. **Kubernetes düğüm VM 'Lerinin VMSize**öğesini seçin. Bu, Kubernetes düğüm VM 'lerinin VM boyutunu belirtir. 

1. Azure Stack yüklemenizin **Azure Stack kimlik sistemi** için **ADFS** 'yi seçin.

1. Bu, Kubernetes Azure bulut sağlayıcısı tarafından kullanılan **hizmet sorumlusu ClientID** değerini girin. Azure Stack yöneticiniz hizmet sorumlusunu oluştururken uygulama KIMLIĞI olarak tanımlanan Istemci KIMLIĞI.

1. **Hizmet sorumlusu istemci gizli**anahtarını girin. Bu, Azure Stack yöneticinizden AD FS hizmet prensibi için size sağlanmış olan istemci gizliliğine yöneliktir.

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