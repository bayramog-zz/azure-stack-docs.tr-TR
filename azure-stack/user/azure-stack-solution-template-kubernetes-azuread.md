---
title: Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack'e dağıtma | Microsoft Docs
description: Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack dağıtmayı öğrenin.
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
ms.date: 05/17/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 05/17/2019
ms.openlocfilehash: a4fe557175aaa4e2faa6c120645c409c0b3449f5
ms.sourcegitcommit: ad2f2cb4dc8d5cf0c2c37517d5125921cff44cdd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67138929"
---
# <a name="deploy-kubernetes-to-azure-stack-using-azure-active-directory"></a>Kubernetes Azure Active Directory'yi kullanarak Azure Stack'e dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo, preview tarafından şu anda desteklenmemektedir.

Kimlik Yönetimi hizmeti olarak dağıtma ve Azure Active Directory (Azure AD) kullanırken, Kubernetes için kaynakları ayarlamak için bu makaledeki adımları izleyin. 

## <a name="prerequisites"></a>Önkoşullar

Başlamak için doğru izinlere sahip ve Azure Stack hazır olduğundan emin olun.

1. Azure Active Directory (Azure AD) kiracınız uygulamalar oluşturabilirsiniz doğrulayın. Kubernetes dağıtımı için bu izinlere ihtiyacınız olur.

    İzinlerinizi denetlerken ile ilgili yönergeler için bkz: [denetleyin Azure Active Directory izinlerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

1. Linux sanal makinesi (VM) oturum açmak için SSH ortak ve özel anahtar çifti Azure Stack'te oluşturur). Kümeyi oluştururken, ortak anahtar gerekir.

    Bir anahtarı oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Azure Stack Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Kullanmanız gereken bir **kullanıcı** abonelik. 

1. Kubernetes kümesi, Market'te yoksa, Azure Stack yöneticinizle konuşun.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Hizmet sorumlusu, uygulamanın Azure Stack kaynaklarına erişmenizi sağlar. 

1. Oturum açmak için genel [Azure portalında](https://portal.azure.com).

1. Azure Stack örneği ile ilişkili Azure AD kiracısı'ı kullanarak oturum açmış denetleyin. Azure araç çubuğundaki filtre simgesine tıklayarak oturum açma işleminiz geçiş yapabilirsiniz.

    ![AD kiracısı seçin](media/azure-stack-solution-template-kubernetes-deploy/tenantselector.png)

1. Azure AD uygulaması oluşturun.

    a. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).  
    b. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.  
    c. Uygulama için bir ad ve URL belirtin.  
    d. Seçin **desteklenen hesap türleri**.  
    e.  Ekleme `http://localhost` uygulama için URI. Seçin **Web** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **kaydetme**.

1. **Uygulama Kimliği**’ni not alın. Kümeyi oluştururken, kimliği gerekir. Kimliği olarak başvurulan **hizmet sorumlusu istemci kimliği**.

1. Hizmet İlkesi dikey penceresinde, seçin **yeni gizli** > **ayarları** > **anahtarları** ve ardından bir kimlik doğrulama anahtarı oluşturun Hizmet İlkesi.

    a. Girin **açıklama**.

    b. Seçin **her zaman geçerli olsun** için **Expires**.

    c. **Add (Ekle)** seçeneğini belirleyin. Anahtar dize not edin. Anahtar dize, kümeyi oluştururken gerekir. Anahtar olarak başvurulan **hizmet sorumlusu istemci parolası**.

## <a name="give-the-service-principal-access"></a>Hizmet sorumlusu erişimi verin

Asıl kaynakları oluşturabilmesi aboneliğinizde hizmet sorumlusu erişimi verin.

1.  Oturum [Azure Stack portalı](https://portal.local.azurestack.external/).

1. Seçin **tüm hizmetleri** > **abonelikleri**.

1. Kubernetes kümesini kullanmak için operatör tarafından oluşturulan aboneliği seçin.

1. Seçin **erişim denetimi (IAM)** > seçin **rol ataması Ekle**.

1. Seçin **katkıda bulunan** rol.

1. Hizmetiniz için asıl oluşturulan uygulama adını seçin. Arama kutusuna adını yazmanız gerekebilir.

1. **Kaydet**’e tıklayın.

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).

1. Seçin **+ kaynak Oluştur** > **işlem** > **Kubernetes kümesi**. Sonra **Oluştur**’a tıklayın.

    ![Azure Stack'te Kubernetes kümesi oluşturma](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Temel Bilgiler

1. Seçin **Temelleri** Kubernetes kümesi oluşturun.

    ![Temel ayarları - Kubernetes kümesi yapılandırma](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Seçin, **abonelik** kimliği

1. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

1. Seçin **konumu** kaynak grubu. Azure Stack yüklemeniz için seçtiğiniz bölgeye konumdur.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes küme ayarları

1. Seçin **Kubernetes küme ayarlarını** Kubernetes kümesi oluşturun.

    ![Kubernetes küme ayarları](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-aad.png)

1. Girin **Linux VM yönetici kullanıcı adı**. Bu ad, Kubernetes kümesinin parçası olan Linux sanal makineleri ve DVM kullanıcı adıdır.

1. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

1. Girin **ana profili DNS ön eki**. Bu ad bir bölge-benzersiz gibi olmalıdır `k8s-12345`. Kaynak grubu adı en iyi uygulama olarak eşleşecek şekilde deneyin.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

1. Seçin **Kubernetes ana havuzu profili sayısı**. Sayıyı ana havuzdaki düğüm sayısını içerir. 7 1'den olabilir. Bu değer, tek bir sayı olmalıdır.

1. Seçin **Kubernetes ana Vm'lerden oluşan bir VMSize**.

1. Seçin **Kubernetes düğüm havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 

1. Seçin **depolama profili**. Seçebileceğiniz **Blob Disk** veya **yönetilen Disk**. Bu profili, VM boyutu Kubernetes düğümünü sanal makinelere belirtir. 

1. Seçin **Azure AD'ye** için **Azure Stack kimlik sistemi** Azure Stack yüklemenizin. 

1. Girin **hizmet sorumlusu ClientID**. Bu tanımlayıcı, Kubernetes Azure bulut sağlayıcısı tarafından kullanılır. Hizmet sorumlusu oluştururken istemci kimliği, uygulama kimliği olarak adlandırılır.

1. Girin **hizmet sorumlusu istemci parolası** , hizmet sorumlusu oluştururken oluşturulur.

1. Girin **Kubernetes Azure bulut sağlayıcısı sürümü**. Bu Kubernetes Azure sağlayıcısı sürüm numarasıdır. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

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
