---
title: Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak Stack dağıtma | Microsoft Docs
description: Kubernetes için Azure Active Directory Federasyon Hizmetleri'nde (AD FS) kullanarak Stack dağıtmayı öğrenin.
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
ms.date: 06/18/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 738a8ab4c43eac9cc68deb63f44577df0f512f0b
ms.sourcegitcommit: 759a01b566597a71b18fca25902cacc983a5a63b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67298078"
---
# <a name="deploy-kubernetes-to-azure-stack-using-active-directory-federated-services"></a>Kubernetes kullanarak Active Directory Federasyon Hizmetleri Azure Stack'e dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack'i bağlantısız senaryo preview tarafından şu anda desteklenmiyor. Yalnızca geliştirme için Market öğesi kullanın ve test senaryoları. [AKS altyapısı](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md) üretim için kullanılabilir.

Dağıtma ve Kubernetes için kaynakları ayarlamak için bu makaledeki adımları izleyebilirsiniz. Active Directory Federasyon Hizmetleri'nde (AD FS) kimlik yönetimi hizmetiniz olduğunda, aşağıdaki adımları kullanın.

## <a name="prerequisites"></a>Önkoşullar 

Başlamak için doğru izinlere sahip ve Azure Stack hazır olduğundan emin olun.

1. Azure Stack Linux VM'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtar gerekir.

    Bir anahtarı oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](azure-stack-dev-start-howto-ssh-public-key.md).

1. Azure Stack Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Kullanmanız gereken bir **kullanıcı** abonelik. 

1. Kubernetes kümesi, Market'te yoksa, Azure Stack yöneticinizle görüşün.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

AD FS kimlik çözümünüz olarak kullanırken hizmet sorumlunuzu ayarlamak için Azure Stack yöneticinizle birlikte çalışmanız gerekiyor. Hizmet sorumlusu, uygulamanın Azure Stack kaynaklarına erişmenizi sağlar.

1. Azure Stack yöneticiniz size hizmet sorumlusu bilgilerini sağlar. Hizmet sorumlusu bilgileri gibi görünmelidir:

     ```Text  
       ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
       ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
       ClientSecret          : <your client secret>
       Thumbprint            : <often this value is empty>
       ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
       PSComputerName        : 192.168.200.224
       RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
     ```

2. Yeni hizmet sorumlunuzu Aboneliğinize bir katkıda bulunan olarak bir rol atayın. Yönergeler için [rol atama](../operator/azure-stack-add-users-adfs.md).

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).

1. Seçin **+ kaynak Oluştur** > **işlem** > **Kubernetes kümesi**. **Oluştur**’u seçin.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Temel Bilgiler

1. Seçin **Temelleri** Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Seçin, **abonelik** kimliği

1. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

1. Seçin **konumu** kaynak grubu. Bu, Azure Stack yüklemeniz için seçtiğiniz bölgedir.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes küme ayarları

1. Seçin **Kubernetes küme ayarlarını** Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-adfs.png)

1. Girin **Linux VM yönetici kullanıcı adı**. Kubernetes kümesinin parçası olan bir Linux sanal makineleri ve DVM için kullanıcı adı.

1. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

1. Girin **Yöneticisi profili, DNS ön eki** bölgeye benzersiz. Bu gibi bölgesi benzersiz bir adı olmalıdır `k8s-12345`. Deneme için seçtiğiniz aynı kaynak grubu adı en iyi yöntem.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

1. Seçin **Kubernetes ana sunucu havuzu profili sayısı**. Sayıyı ana havuzdaki düğüm sayısını içerir. 7 1'den olabilir. Bu değer, tek bir sayı olmalıdır.

1. Seçin **Kubernetes ana Vm'lerden oluşan bir VMSize**.

1. Seçin **Kubernetes düğüm havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 

1. Seçin **Kubernetes düğümünün Vm'leri VMSize**. Bu VM'ler, VM boyutu Kubernetes düğüm belirtir. 

1. Seçin **ADFS** için **Azure Stack kimlik sistemi** Azure Stack yüklemenizin.

1. Girin **hizmet sorumlusu ClientID** bu Kubernetes Azure bulut sağlayıcısı tarafından kullanılır. Azure Stack yöneticinize hizmet sorumlusu oluştururken uygulama kimliği olarak tanımlanan istemci kimliği.

1. Girin **hizmet sorumlusu istemci parolasını**. Bu, AD FS hizmet ilkesi için Azure Stack yöneticinizden size sağlanan istemci gizli dizinidir.

1. Girin **Kubernetes sürümü**. Kubernetes Azure sağlayıcısı sürümüdür. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

### <a name="3-summary"></a>3. Özet

1. Özet'i seçin. Dikey bir Kubernetes kümesi yapılandırma ayarlarınızı doğrulama iletisi görüntüler.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Ayarlarınızı gözden geçirin.

3. Seçin **Tamam** kümenize dağıtmak için.

> [!TIP]  
>  Dağıtımınız hakkında sorularınız varsa sorunuzu gönderin veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="next-steps"></a>Sonraki adımlar

[Kümenize bağlanın](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Kubernetes panosunu etkinleştir](azure-stack-solution-template-kubernetes-dashboard.md)