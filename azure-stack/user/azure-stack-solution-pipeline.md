---
title: Azure ve Azure uygulama dağıtma yığın | Microsoft Docs
description: Azure ve Azure Stack için hibrit CI/CD işlem hattı ile uygulamaları dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: solution
ms.date: 03/11/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/07/2018
ms.openlocfilehash: 9fbadb923452fc2420d1f8626a69d377c4d72e12
ms.sourcegitcommit: 2a4cb9a21a6e0583aa8ade330dd849304df6ccb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286958"
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Azure ve Azure Stack’e uygulama dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure ve Azure Stack, hibrit sürekli tümleştirme/sürekli teslim (CI/CD) işlem hattı kullanarak uygulama dağıtma konusunda bilgi edinin.

Bu çözümde bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> * Azure DevOps Hizmetleri deponuza kod tamamlama dayalı yeni bir derleme başlatır.
> * Otomatik olarak kullanıcı kabul testi için genel Azure uygulamanıza dağıtın.
> * Kodunuzu test geçtiğinde, aynı zamanda Azure Stack için otomatik olarak uygulamayı dağıtın.

## <a name="benefits-of-the-hybrid-delivery-build-pipeline"></a>Hybrid dağıtım avantajları işlem hattı oluşturma

Süreklilik, güvenlik ve güvenilirlik uygulama dağıtımı için anahtar öğeleridir. Bu, kuruluşunuz için önemli ve Geliştirme ekibiniz için kritik öğeleridir. Bir karma CI/CD işlem hattı şirket içi ortamınız ve genel bulutta, derleme kanallar birleştirmenize olanak tanır. Karma teslim modeli Ayrıca uygulamanızı değiştirmeden dağıtım konumlarını değiştirmek de sağlar.

Karma yaklaşım kullanmanın diğer avantajları şunlardır:

* Tutarlı bir dizi geliştirme aracı, şirket içi Azure Stack ortamınıza ve Azure genel bulutunda koruyabilirsiniz.  Ortak bir araç kümesi, CI/CD desenler ve uygulamalar uygulamak kolaylaştırır.
* Uygulamaları ve Hizmetleri Azure veya Azure Stack dağıtılmış birbirinin yerine kullanılabilir ve aynı kod iki konumdan birinde çalıştırabilirsiniz. Şirket içinde ve genel bulut özellikleri ve yetenekleri yararlanabilirsiniz.

CI ve CD hakkında daha fazla bilgi edinmek için:

* [Sürekli Tümleştirme nedir?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [Sürekli teslim nedir?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu çevikliğini ve yenilik, şirket içi ortamınıza bulut getirir.  
> 
> Makaleyi [karma uygulamaları için tasarım konuları](azure-stack-edge-pattern-overview.md) (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve karma çalıştırma için gözden geçirmeleri uygulamalar. Tasarım konuları, karma uygulama tasarımı, üretim ortamlarında sorunlarını en aza en iyi duruma getirme yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bileşenleri karma CI/CD işlem hattı oluşturma yerinde olması gerekir. Aşağıdaki bileşenler hazırlamak için uzun sürer:

* Bir Azure OEM donanım iş ortağı, Azure Stack üretim dağıtabilirsiniz. Tüm kullanıcılar, Azure Stack geliştirme Seti'ni (ASDK) dağıtabilirsiniz.
* Bir Azure Stack operatörü aşağıdakileri tamamlamanız gerekir: App Service'e dağıtım, planlar ve Teklifler oluşturma, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntüsü ekleyin.

>[!NOTE]
>Dağıtılan Bu bileşenlerden bazıları zaten varsa, bu çözüm başlatmadan önce tüm gereksinimleri karşıladığından emin olun.

Bu çözüm, Azure ve Azure Stack bazı temel bilgiye sahip olduğunuzu varsayar. Çözüm başlatmadan önce daha fazla bilgi için bu makaleleri okuyun:

* [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
* [Azure Stack temel kavramları](../operator/azure-stack-overview.md)

### <a name="azure-requirements"></a>Azure gereksinimleri

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Oluşturma bir [Web uygulaması](https://docs.microsoft.com/azure/app-service/overview) azure'da. Web uygulama URL'sini Not olun, çözümde kullanmanız gerekir.

### <a name="azure-stack-requirements"></a>Azure Stack gereksinimleri

* Bir Azure Stack tümleşik sistemi kullanın veya Azure Stack geliştirme Seti'ni (ASDK) dağıtın. ASDK dağıtmak için:
  * [ Çözümü: Yükleyiciyi kullanarak ASDK dağıtma](../asdk/asdk-install.md) makalede ayrıntılı dağıtım yönergeleri sağlanır.
  * Kullanım [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) ASDK dağıtım sonrası adımları otomatikleştirmek için PowerShell Betiği.

    > [!Note]
    > ASDK yüklemeyi tamamlamak için bu nedenle buna göre planlayın yaklaşık yedi saat sürer.

  * Dağıtma [App Service](../operator/azure-stack-app-service-deploy.md) PaaS Hizmetleri Azure stack'e.
  * Oluşturma [planı/teklifler](../operator/azure-stack-plan-offer-quota-overview.md) Azure Stack'te.
  * Oluşturma bir [Kiracı aboneliği](../operator/azure-stack-subscribe-plan-provision-vm.md) Azure Stack'te.
  * Bir Web uygulaması, Kiracı aboneliği oluşturabilir. Daha sonra kullanmak için yeni Web App URL'si not edin.
  * Kiracı aboneliği bir Windows Server 2012 sanal makinesi dağıtın. Yapı sunucunuz ve Azure DevOps hizmetlerini çalıştırmak için bu sunucuyu kullanırsınız.
* Bir Windows Server 2016 görüntüsü ile .NET 3.5 bir sanal makine (VM) sağlayın. Bu sanal Makineyi bir özel yapı aracısı Azure Stack oluşturulur.

### <a name="developer-tool-requirements"></a>Geliştirici aracı gereksinimleri

* Oluşturma bir [Azure DevOps Hizmetleri çalışma](https://docs.microsoft.com/azure/devops/repos/tfvc/create-work-workspaces). Kayıt işlemini adlı bir proje oluşturur **MyFirstProject**.
* [Visual Studio 2019 yükleme](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ve [oturum açma Azure DevOps hizmetlerine](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Projenize bağlayın ve [yerel ortamda kopyalayın](https://www.visualstudio.com/docs/git/gitquickstart).

  > [!Note]
  > Windows Server ve SQL Server çalıştırmak için Dağıtılmış doğru görüntüleri, Azure Stack ortamınıza gerekir. Ayrıca dağıtılan App Service olması gerekir.

## <a name="prepare-the-private-azure-pipelines-agent-for-azure-devops-services-integration"></a>Azure DevOps Hizmetleri Tümleştirme için özel Azure işlem hatları Aracısı'nı hazırlama

### <a name="prerequisites"></a>Önkoşullar

Azure DevOps hizmetleriyle Azure Resource Manager'da bir hizmet sorumlusunu kullanarak kimliğini doğrular. Azure DevOps hizmetleriyle olmalıdır **katkıda bulunan** Azure Stack aboneliği'ndeki sağlamak için rol.

Aşağıdaki adımlar, hangi kimlik doğrulamasını yapılandırmak için gereken açıklar:

1. Hizmet sorumlusu oluşturma veya mevcut bir hizmet sorumlusunu kullanın.
2. Kimlik doğrulaması anahtarları, hizmet sorumlusu oluşturun.
3. Azure Stack aboneliğine hizmet asıl adı (Katkıda bulunanın rolünün bir parçası olarak SPN) izin vermek için rol tabanlı erişim denetimi aracılığıyla doğrulayın.
4. Azure DevOps hizmetlerinde Azure Stack uç noktaları ve SPN bilgileri kullanarak yeni bir hizmet tanımı oluşturun.

### <a name="create-a-service-principal"></a>Hizmet Sorumlusu oluşturma

Başvurmak [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) bir hizmet sorumlusu oluşturmak için yönergeler. Seçin **Web uygulaması/API'si** uygulama türü için veya [PowerShell betiğini kullanın](https://github.com/Microsoft/vsts-rm-extensions/blob/master/TaskModules/powershell/Azure/SPNCreation.ps1#L5) makalesinde açıklandığı gibi [bir hizmetiniz ile bir Azure Resource Manager hizmet bağlantısı oluştur Asıl ](https://docs.microsoft.com/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal).

 > [!Note]  
 > Betik bir Azure Stack Azure Resource Manager uç nokta oluşturmak için kullandığınız geçirmek gerekirse **- azureStackManagementURL** parametresi ve **- environmentName** parametresi. Örneğin:  
> `-azureStackManagementURL https://management.local.azurestack.external -environmentName AzureStack`

### <a name="create-an-access-key"></a>Bir erişim anahtarı oluştur

Bir hizmet sorumlusu kimlik doğrulaması için bir anahtar gerektirir. Bir anahtar oluşturmak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtları** Azure Active Directory'de, uygulamanızı seçin.

    ![-Azure Active Directory uygulaması seçin](media/azure-stack-solution-hybrid-pipeline/000_01.png)

2. Değerini not edin **uygulama kimliği**. Azure DevOps Hizmetleri'nde hizmet uç noktasını yapılandırırken bu değeri kullanacaksınız.

    ![Uygulama Kimliği - Azure Active Directory](media/azure-stack-solution-hybrid-pipeline/000_02.png)

3. Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

    ![Uygulama ayarları - Azure Active Directory Düzenle](media/azure-stack-solution-hybrid-pipeline/000_03.png)

4. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

    ![Anahtar ayarlarını - Azure Active Directory'yi yapılandırma](media/azure-stack-solution-hybrid-pipeline/000_04.png)

5. Anahtar için bir açıklama sağlayın ve anahtar süresini ayarlayın. İşiniz bittiğinde **Kaydet**’i seçin.

    ![Anahtar açıklaması ve süresi - Azure Active Directory](media/azure-stack-solution-hybrid-pipeline/000_05.png)

    Anahtar, anahtar kaydettikten sonra **değer** görüntülenir. Bu değer daha sonra alınamıyor çünkü bu değeri kopyalayın. Sağladığınız **anahtar değerini** ile **uygulama kimliği** uygulaması olarak oturum açmak için. Uygulamanızı, alabildiği anahtar değeri Store.

    ![Anahtar değeri - Azure Active Directory](media/azure-stack-solution-hybrid-pipeline/000_06.png)

### <a name="get-the-tenant-id"></a>Kiracı Kimliğinizi alma

Hizmet uç noktası yapılandırmasının bir parçası olarak, Azure DevOps hizmetleri gerektirir **Kiracı kimliği** karşılık gelen AAD dizinine, Azure Stack damga dağıtılır. Kiracı kimliğini almak için aşağıdaki adımları kullanın.

1. **Azure Active Directory**'yi seçin.

    ![Kiracı için Azure Active Directory](media/azure-stack-solution-hybrid-pipeline/000_07.png)

2. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

    ![Azure Active Directory Kiracı özelliklerini görüntülemek](media/azure-stack-solution-hybrid-pipeline/000_08.png)

3. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

    ![Dizin kimliği - Azure Active Directory](media/azure-stack-solution-hybrid-pipeline/000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Azure Stack aboneliğine kaynakları dağıtmak için hizmet sorumlusu haklar

Aboneliğinizdeki kaynaklara erişmek için uygulamayı rol atamanız gerekir. En iyi uygulama izinlerini rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: Yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, okuyucu rolüne bir kaynak grubu için bir uygulama eklemek kaynak grubunu ve tüm kaynaklarının okuyabilirsiniz anlamına gelir.

1. Uygulamayı atamak istediğiniz kapsam düzeyine gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

    ![Azure Stack aboneliklerini - seçin](media/azure-stack-solution-hybrid-pipeline/000_10.png)

2. İçinde **abonelik**, Visual Studio Enterprise'ı seçin.

    ![Visual Studio Enterprise - Azure Stack](media/azure-stack-solution-hybrid-pipeline/000_11.png)

3. Visual Studio Enterprise'ı seçin **erişim denetimi (IAM)** .

4. Seçin **rol ataması Ekle**.

    ![Rol ataması - Azure Stack Ekle](media/azure-stack-solution-hybrid-pipeline/000_13.png)

5. İçinde **izinleri eklemek**, rolü seçin, uygulamaya atamak istediğiniz. Bu örnekte, bu **sahibi** rol.

    ![Sahip rolü izinleri - Azure Stack](media/azure-stack-solution-hybrid-pipeline/000_14.png)

6. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için adını sağlamalısınız **seçin** arama alanı. Uygulamayı seçin.

    ![Uygulama arama sonucu - Azure Stack](media/azure-stack-solution-hybrid-pipeline/000_16.png)

7. Seçin **Kaydet** rol atama tamamlanması. Uygulamanızda bu kapsam için bir role atanan kullanıcıların listesini görebilirsiniz.

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Azure rol tabanlı erişim denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapmak için gereken erişim düzeyini denetleyebilirsiniz. Rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz. [Azure abonelik kaynaklarına erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="azure-devops-services-agent-pools"></a>Azure DevOps aracı havuzları Hizmetleri

Her bir aracıyı ayrı ayrı yönetmek yerine, aracı havuzlarına aracıları düzenleyebilirsiniz. Bir aracı havuzu paylaşım sınırı, havuzdaki tüm aracılar için tanımlar. Azure DevOps Hizmetleri'nde aracı havuzları projeler arasında bir aracı havuzu paylaşabildiği anlamına gelir Azure DevOps Hizmetleri kuruluşun kapsamına eklenir. Aracı havuzları hakkında daha fazla bilgi için bkz: [aracı havuzları oluşturma ve kuyruklar](https://docs.microsoft.com/azure/devops/pipelines/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Azure Stack için kişisel erişim belirteci (PAT) Ekle

Azure DevOps hizmetlerine erişmek için kişisel erişim belirteci oluşturun.

1. Azure DevOps Hizmetleri kuruluşunuz oturum açın ve kuruluşunuzun profil adı seçin.

2. Seçin **Güvenliği Yönet** için erişim belirteci oluşturma sayfası. 

    ![Azure Stack güvenliği - yönetme](media/azure-stack-solution-hybrid-pipeline/000_18.png)

3. Tıklayın **Ekle** yeni bir kişisel erişim belirteci oluşturmak için.

    ![Kişisel erişim belirteci - Azure Stack Ekle](media/azure-stack-solution-hybrid-pipeline/000_18a.png)

    ![Belirteci - Azure yığını oluşturma](media/azure-stack-solution-hybrid-pipeline/000_18b.png)

4. Belirteci kopyalayın.

    > [!Note]
    > Belirteç bilgileri kaydedin. Bu bilgi saklanmaz ve web sayfası çıktığınızda tekrar gösterilmeyecektir.

    ![Kişisel erişim belirteci - Azure Stack](media/azure-stack-solution-hybrid-pipeline/000_19.png)

### <a name="install-the-azure-devops-services-build-agent-on-the-azure-stack-hosted-build-server"></a>Azure DevOps Hizmetleri derleme aracısı Azure Stack barındırılan yapı sunucusuna yükleyin.

1. Azure Stack konak üzerinde dağıtılan yapı sunucunuza bağlanın.
2. Kişisel erişim belirteci (PAT) kullanan bir hizmet olarak yapı aracısını dağıtma indirin ve VM yönetici olarak çalıştır

    ![Derleme Aracısı'nı indirme](media/azure-stack-solution-hybrid-pipeline/010_downloadagent.png)

3. Ayıklanan derleme aracısı klasöre gidin. Çalıştırma **config.cmd** dosyasını yükseltilmiş bir komut isteminden.

    ![Ayıklanan derleme aracısı](media/azure-stack-solution-hybrid-pipeline/000_20.png)

    ![Derleme aracısı kaydedilmeye](media/azure-stack-solution-hybrid-pipeline/000_21.png)

4. Config.cmd sona erdiğinde, derleme aracısı klasörü ek dosyalarla güncelleştirilir. Ayıklanan içeriğiyle klasörü aşağıdaki örnekteki gibi görünmelidir:

    ![Yapı Aracısı klasör güncelleştirme](media/azure-stack-solution-hybrid-pipeline/009_token_file.png)

    Azure DevOps Hizmetleri klasörü aracıyı görebilirsiniz.

## <a name="endpoint-creation-permissions"></a>Uç nokta oluşturma izinleri

Visual Studio Online (VSTO) derleme, uç noktaları oluşturarak, Azure Stack için Azure hizmet uygulamaları dağıtabilirsiniz. Azure DevOps Hizmetleri Azure Stack'e bağlanır yapı aracısı bağlanır.

![VSTO NorthwindCloud örnek uygulaması](media/azure-stack-solution-hybrid-pipeline/012_securityendpoints.png)

1. VSTO için oturum açın ve uygulama ayarları sayfasına gidin.
2. İçinde **ayarları**seçin **güvenlik**.
3. İçinde **Azure DevOps Hizmetleri gruplarında**seçin **uç noktasını oluşturanlar**.

    ![NorthwindCloud uç noktasını oluşturanlar](media/azure-stack-solution-hybrid-pipeline/013_endpoint_creators.png)

4. Üzerinde **üyeleri** sekmesinde **Ekle**.

    ![Üye ekleme](media/azure-stack-solution-hybrid-pipeline/014_members_tab.png)

5. Üzerinde **kullanıcılar ve gruplar ekleme** sayfasında bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.
6. Seçin **değişiklikleri kaydetmek**.
7. İçinde **Azure DevOps Hizmetleri gruplarında** listesinden **uç nokta yöneticileri**.

    ![NorthwindCloud uç nokta yöneticileri](media/azure-stack-solution-hybrid-pipeline/015_save_endpoint.png)

8. Üzerinde **üyeleri** sekmesinde **Ekle**.
9. Üzerinde **kullanıcılar ve gruplar ekleme** sayfasında bir kullanıcı adı girin ve kullanıcının kullanıcılar listesinden seçin.
10. Seçin **değişiklikleri kaydetmek**.

Mevcut uç nokta bilgileri, Azure Stack bağlantı Azure DevOps Hizmetleri'nin kullanıma hazırdır. Yapı aracısının Azure Stack'te Azure DevOps hizmetlerinden yönergeler alır ve ardından aracıyı Azure Stack ile iletişim için uç nokta bilgileri iletmez.

## <a name="create-an-azure-stack-endpoint"></a>Bir Azure Stack uç noktası oluşturma

### <a name="create-an-endpoint-for-azure-ad-deployments"></a>Azure AD dağıtımları için bir uç nokta oluşturma

' Ndaki yönergeleri takip edebilirsiniz [bir Azure Resource Manager hizmet bağlantısı ile mevcut bir hizmet sorumlusu oluşturma ](https://docs.microsoft.com/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal) makale bir hizmet bağlantısı ile mevcut bir hizmet sorumlusu oluşturma ve aşağıdaki eşlemeyi kullanın:

| Ad | Örnek | Açıklama |
| --- | --- | --- |
| Bağlantı adı | Azure Stack Azure AD | Bağlantının adı. |
| Ortam | AzureStack | Ortamınızın adını. |
| Ortam URL'si | `https://management.local.azurestack.external` | Yönetim uç noktanıza. |
| Kapsam düzeyi | Subscription | Bağlantının kapsamı. |
| Abonelik Kimliği | 65710926-XXXX-4F2A-8FB2-64C63CD2FAE9 | Azure Stack kullanıcı abonelik kimliği |
| Abonelik adı | name@contoso.com | Azure Stack kullanıcı abonelik adı. |
| Hizmet sorumlusu istemci kimliği | FF74AACF-XXXX-4776-93FC-C63E6E021D59 | Asıl Kimliğinden [bu](azure-stack-solution-pipeline.md#create-a-service-principal) bu makaledeki bir bölüm. |
| Hizmet sorumlusu anahtarı | THESCRETGOESHERE = | Aynı makalede (veya betiği kullandıysanız parolayı) anahtarı. |
| Kiracı Kimliği | D073C21E-XXXX-4AD0-B77E-8364FCA78A94 | Aşağıdaki yönerge almak Kiracı kimliği [Kiracı Kimliğinizi alma](azure-stack-solution-pipeline.md#get-the-tenant-id).  |
| Bağlantı: | Doğrulanmadı | Hizmet sorumlusu için bağlantı ayarlarınızı doğrulayın. |

Uç nokta oluşturulduktan sonra Azure Stack bağlantı DevOps kullanıma hazırdır. Yapı aracısının Azure Stack'te DevOps yönergeleri alır ve ardından aracıyı Azure Stack ile iletişim için uç nokta bilgileri iletmez.

![Derleme aracısı Azure AD](media/azure-stack-solution-hybrid-pipeline/016_save_changes.png)

### <a name="create-an-endpoint-for-ad-fs"></a>AD FS için bir uç nokta oluşturma

Azure DevOps en son güncelleştirmesi, bir hizmet sorumlusu kimlik doğrulaması için bir sertifika kullanarak bir hizmet bağlantı oluşturmanızı sağlar. Bu bağlantı, kimlik sağlayıcısı olarak AD FS ile Azure Stack dağıtıldığında gereklidir. 

![Derleme aracısı AD FS](media/azure-stack-solution-hybrid-pipeline/image06.png)

Aşağıdaki eşlemeyi kullanarak bir hizmet bağlantı oluşturabilirsiniz:

| Ad | Örnek | Açıklama |
| --- | --- | --- |
| Bağlantı adı | Azure Stack ADFS | Bağlantının adı. |
| Ortam | AzureStack | Ortamınızın adını. |
| Ortam URL'si | `https://management.local.azurestack.external` | Yönetim uç noktanıza. |
| Kapsam düzeyi | Subscription | Bağlantının kapsamı. |
| Abonelik Kimliği | 65710926-XXXX-4F2A-8FB2-64C63CD2FAE9 | Azure Stack kullanıcı abonelik kimliği |
| Abonelik adı | name@contoso.com | Azure Stack kullanıcı abonelik adı. |
| Hizmet sorumlusu istemci kimliği | FF74AACF-XXXX-4776-93FC-C63E6E021D59 | AD FS için oluşturduğunuz gelen hizmet sorumlusu istemci kimliği. |
| Sertifika | `<certificate>` |  Sertifikayı PFX'ten PEM'ye dönüştürün. Sertifika PEM dosyasının içeriğini bu alana yapıştırın. <br> PFX PEM'ye dönüştürme:<br>`openssl pkcs12 -in file.pfx -out file.pem -nodes -password pass:<password_here>` |
| Kiracı Kimliği | D073C21E-XXXX-4AD0-B77E-8364FCA78A94 | Aşağıdaki yönerge almak Kiracı kimliği [Kiracı Kimliğinizi alma](azure-stack-solution-pipeline.md#get-the-tenant-id). |
| Bağlantı: | Doğrulanmadı | Hizmet sorumlusu için bağlantı ayarlarınızı doğrulayın. |

Uç nokta oluşturulduktan sonra Azure Stack bağlantı için Azure DevOps kullanıma hazırdır. Yapı aracısının Azure Stack'te Azure DevOps yönergeleri alır ve ardından aracıyı Azure Stack ile iletişim için uç nokta bilgileri iletmez.

> [!Note]
> Azure Resource Manager uç noktanızın Internet'e açık değilse, bağlantı doğrulama başarısız olur. Bu beklenen bir durumdur ve basit bir görevle yayın işlem hattı oluşturarak bağlantınızı doğrulayabilirsiniz. 

## <a name="develop-your-application-build"></a>Uygulama derleme geliştirin

Bu çözümün parçası, gerekir:

* Bir Azure DevOps Services projesi için kod ekleyin.
* Kendi içinde bir web uygulaması dağıtımı oluşturun.
* Sürekli dağıtım işlemi yapılandırma

> [!Note]
 > Windows Server ve SQL Server çalıştırmak için Dağıtılmış doğru görüntüleri, Azure Stack ortamınıza gerekir. Ayrıca dağıtılan App Service olması gerekir. Azure Stack operatörü gereksinimleri için "Önkoşullar" bölümüne App Service belgelerini inceleyin.

Karma CI/CD, hem uygulama kodunda hem de altyapı kodunu uygulayabilirsiniz. Kullanım [Azure Resource Manager şablonları gibi web](https://azure.microsoft.com/resources/templates/) hem bulutlara dağıtmak için Azure DevOps hizmetlerden uygulama kodu.

### <a name="add-code-to-an-azure-devops-services-project"></a>Bir Azure DevOps Services projesi için kod ekleyin

1. Azure Stack'te proje oluşturma haklarına sahip bir kuruluş ile Azure DevOps Hizmetleri için oturum açın. Sonraki ekran görüntüsü yakalamayı HybridCICD projesine bağlanma işlemi gösterilmektedir.

    ![Bir projeye - Azure DevOps Hizmetleri bağlayın](media/azure-stack-solution-hybrid-pipeline/017_connect_to_project.png)

2. **Depoyu kopyalama** oluşturarak ve varsayılan bir web uygulamasını açma.

    ![Depo - Azure DevOps Hizmetleri kopyalama](media/azure-stack-solution-hybrid-pipeline/018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Uygulama hizmetleri için kendi içinde bir web uygulaması dağıtımı her iki bulut oluşturma

1. Düzen **WebApplication.csproj** dosyası: Seçin `Runtimeidentifier` ve ardından eklemek `win10-x64.` daha fazla bilgi için [müstakil dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri.

    ![Runtimeidentifier yapılandırın](media/azure-stack-solution-hybrid-pipeline/019_runtimeidentifer.png)

2. Azure DevOps hizmetlerine kodunu denetlemek için Takım Gezgini'ni kullanın.

3. Uygulama kodu Azure DevOps hizmetlerine denetlendi onaylayın.

### <a name="create-the-build-pipeline"></a>Derleme işlem hattı oluşturma

1. Azure DevOps Hizmetleri için bir derleme işlem hattı oluşturmak bir kuruluş ile oturum açın.

2. Gidin **Web uygulaması derleme** proje sayfası.

3. İçinde **bağımsız değişkenleri**, ekleme **- r win10-x64** kod. Bu adım, .NET Core ile kendi içinde bir dağıtımı tetiklemek için gereklidir.

    ![Bağımsız değişken derleme işlem hattı ekleyin](media/azure-stack-solution-hybrid-pipeline/020_publish_additions.png)

4. Yapı çalıştırın. [Müstakil dağıtım derleme](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlem, Azure ve Azure Stack üzerinde çalışabilen yapıtları yayımlar.

### <a name="use-an-azure-hosted-build-agent"></a>Azure'da barındırılan derleme Aracısı'nı kullanın

Barındırılan derleme aracısı, Azure DevOps Hizmetleri'ni kullanarak, oluşturmak ve web uygulamalarını dağıtmak için kullanışlı bir seçenektir. Aracı Bakımı ve yükseltmeler, sürekli ve kesintisiz geliştirme döngüsü sağlayan Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="configure-the-continuous-deployment-cd-process"></a>Sürekli dağıtım (CD) işlem yapılandırma

Azure DevOps Hizmetleri ve Team Foundation Server (TFS) geliştirme, hazırlama, kalite güvencesi kapsayan (QA) ve üretim gibi birden çok ortama yayınlar için son derece yapılandırılabilir ve yönetilebilir bir işlem hattı sağlar. Bu işlem, belirli bir uygulama yaşam döngüsü aşamalarında onay gerektiren içerebilir.

### <a name="create-release-pipeline"></a>Yayın işlem hattı oluşturma

Yayın işlem hattı oluşturmak, uygulamanızı son adımı yapı işlemi olur. Bu yayın ardışık düzeni, bir yayın oluşturun ve bir yapı dağıtmak için kullanılır.

1. Azure DevOps Hizmetleri için oturum açın ve gidin **Azure işlem hatları** projeniz için.
2. Üzerinde **yayınlar** sekmesinde  **\[ +]** ve ardından çekme **Oluştur yayın tanımı**.

   ![Yayın işlem hattı - Azure DevOps hizmetleri oluşturma](media/azure-stack-solution-hybrid-pipeline/021a_releasedef.png)

3. Üzerinde **bir şablon seçin** sayfasında **Azure uygulama hizmeti dağıtımının**ve ardından **Uygula**.

    ![Şablon - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/102.png)

4. Üzerinde **yapıt ekleme** sayfasında gelen **kaynak (derleme tanımı)** aşağı açılır menüsünde, Azure bulut yapı uygulamayı seçin.

    ![Yapıt - Azure DevOps Hizmetleri ekleme](media/azure-stack-solution-hybrid-pipeline/103.png)

5. Üzerinde **işlem hattı** sekmesinde **1. Aşama**, **1 görev** bağlantı **ortam görevlerini görüntüle**.

    ![İşlem hattı görünümü görevleri - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/104.png)

6. Üzerinde **görevleri** sekmesinde, Azure olarak girin **ortam adı** gelen AzureCloud Traders Web EP seçip **Azure aboneliği** aşağı açılan listesi.

    ![Azure DevOps Hizmetleri - ortam değişkenlerini ayarlama](media/azure-stack-solution-hybrid-pipeline/105.png)

7. Girin **Azure uygulama hizmeti adı**, sonraki ekran görüntüsünde "northwindtraders" olduğu.

    ![Uygulama hizmeti adı - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/106.png)

8. Aracı aşaması için seçin **Hosted VS2017** gelen **aracı kuyruğu** aşağı açılan listesi.

    ![Barındırılan aracı - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/107.png)

9. İçinde **Azure App Service'e dağıtma**, geçerli seçin **paket veya klasör** ortam için.

    ![Paket ya da klasörü - Azure DevOps hizmetleri seçin](media/azure-stack-solution-hybrid-pipeline/108.png)

10. Üzerinde **seçin dosya veya klasör** sayfasında **Tamam** klasör konumu için.

    ![Dosya veya klasör - Azure DevOps hizmetleri seçin](media/azure-stack-solution-hybrid-pipeline/109.png)

11. Tüm değişiklikleri kaydetmek ve geri dönüp **işlem hattı**.

    ![Azure DevOps Hizmetleri - Değişiklikleri Kaydet](media/azure-stack-solution-hybrid-pipeline/110.png)

12. Üzerinde **işlem hattı** sekmesinde **yapıt ekleme**ve **NorthwindCloud Traders-tekne** gelen **kaynak (derleme tanımı)** aşağı açılan listesi.

    ![Yeni yapıt - Azure DevOps Hizmetleri ekleme](media/azure-stack-solution-hybrid-pipeline/111.png)

13. Üzerinde **bir şablon seçin** sayfasında, başka bir ortama ekleyin. Çekme **Azure uygulama hizmeti dağıtımının** seçip **Uygula**.

    ![Şablon - Azure DevOps hizmetleri seçin](media/azure-stack-solution-hybrid-pipeline/112.png)

14. "Azure Stack" olarak girin **ortam adı**.

    ![Ortam adı - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/113.png)

15. Üzerinde **görevleri** sekmesinde, bulmak ve Azure Stack seçin.

    ![Azure Stack ortam - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/114.png)

16. Gelen **Azure aboneliği** aşağı açılan listesinde, Azure Stack uç noktası için "AzureStack Traders tekne EP"'i seçin.

    ![Azure aboneliği açılan - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/115.png)

17. Azure Stack web uygulaması adı olarak girin **uygulama hizmeti adı**.

    ![Uygulama hizmeti adı - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/116.png)

18. Altında **Aracısı Seçimi**, "Alanının AzureStack - bDouglas" arasından **aracı kuyruğu** aşağı açılan listesi.

    ![Azure DevOps Hizmetleri Aracısı - çekme](media/azure-stack-solution-hybrid-pipeline/117.png)

19. İçin **Azure App Service'e dağıtma**, geçerli seçin **paket veya klasör** ortam için. Üzerinde **dosya veya klasörü seçin**seçin **Tamam** klasör **konumu**.

    ![Paket ya da klasör - Azure DevOps hizmetleri seçin](media/azure-stack-solution-hybrid-pipeline/118.png)

    ![Konum - Azure DevOps Hizmetleri Onayla](media/azure-stack-solution-hybrid-pipeline/119.png)

20. Üzerinde **değişkenleri** sekmesinde, bulmak adlı değişken **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. Değişken değeri ayarlamak **true**ve kapsamı ayarlayın **Azure Stack**.

    ![Değişken - Azure DevOps Hizmetleri Yapılandırma](media/azure-stack-solution-hybrid-pipeline/120.png)

21. Üzerinde **işlem hattı** sekmesinde **sürekli dağıtım tetikleyicisi** kümesi ve NorthwindCloud Traders-Web yapıt simgesi **sürekli dağıtım tetikleyicisi** için **Etkin**.  "NorthwindCloud Traders tekne" yapıtı için aynı işlevi görür.

    ![Azure DevOps Hizmetleri - sürekli dağıtım tetikleyicisi ayarlayın](media/azure-stack-solution-hybrid-pipeline/121.png)

22. Azure Stack ortamı için seçin **dağıtım öncesi koşulları** simge tetikleyiciyi ayarlayın **sürümünden sonra**.

    ![Azure DevOps Hizmetleri dağıtım öncesi koşulları tetikleyici - ayarlayın](media/azure-stack-solution-hybrid-pipeline/122.png)

23. Yaptığınız tüm değişiklikleri kaydedin.

> [!Note]
> Yayın görevleri için bazı ayarları otomatik olarak tanımlanmış olabilir [ortam değişkenlerini](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts#custom-variables) bir şablondan bir yayın ardışık düzeni oluşturduğunuzda. Bu ayarlar görev ayarlarını değiştirilemez. Ancak, bu ayarları üst ortam öğelerini düzenleyebilirsiniz.

## <a name="create-a-release"></a>Bir yayın oluşturun

Yayın ardışık düzeni için yapılan değişiklikleri tamamladınız, bu dağıtımı başlatmak için saattir. Dağıtımına başlamak için sürüm yayın işlem hattı oluşturun. Bir yayın otomatik olarak oluşturulabilir; Örneğin, sürekli dağıtım tetikleyicisi yayın işlem hattında ayarlandığında. Kaynak kodun değiştirilmesiyle tp'nin yeni derleme ve ardından yeni bir yayın başlar Bu tetikleyici anlamına gelir ayarlanıyor. Ancak, bu bölümde yeni bir yayın el ile oluşturmanız.

1. Üzerinde **işlem hattı** sekmesini **yayın** aşağı açılan listesinden **yayın oluştur**.

    ![Bir yayın - Azure DevOps hizmetleri oluşturma](media/azure-stack-solution-hybrid-pipeline/200.png)

2. Yayını için bir açıklama girin, doğru yapıtlar seçili olduğunu görmek için kontrol edin ve ardından **Oluştur**. Birkaç dakika sonra yeni yayın oluşturuldu ve yayın adı bir bağlantı gösterilir belirten bir başlık görüntülenir. Sürüm özeti sayfasında görmek için bağlantıyı seçin.

    ![Sürüm oluşturma başlığı - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/201.png)

3. Sürüm özeti sayfasında sürüm hakkındaki ayrıntıları gösterir. Aşağıdaki ekran görüntüsünde "Release-2" için **ortamları** bölümünde gösterildiği **dağıtım durumu** Azure "Sürüyor" olarak ve Azure Stack için durumu başarılı"için". Azure ortamı için dağıtım durumu "Başarılı" için değiştiğinde, yayın onay için hazır olduğunu belirten bir başlık görüntülenir. Ne zaman bir dağıtımı Beklemede veya başarısız oldu, mavi bir **(i)** bilgi simgesi gösterilir. Gecikme veya hatanın nedenini içeren bir açılır pencere için simgesinin üzerine gelin.

    ![Sürüm özeti sayfasında - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/202.png)

Listesi gibi diğer görünümleri sürümleri, ayrıca onay bekliyor belirten bir simge görüntülenir. Ortam adı ve dağıtımıyla ilgili daha fazla ayrıntı için bu simge açılır gösterir. Sürümler ve sürümler onay bekliyor bkz: Genel ilerlemesi yönetici görmek için kolay bir işlemdir.

### <a name="monitor-and-track-deployments"></a>İzleme ve izleme dağıtımları

Bu bölüm, nasıl izleyebilir ve tüm dağıtımları izleyin gösterir. İki Azure uygulama Hizmetleri Web sitelerini dağıtmak için sürüm iyi bir örnek sağlar.

1. "Release-2" Özet sayfasında, seçin **günlükleri**. Bir dağıtım sırasında bu sayfa, Aracıdan dinamik günlüğü gösterir. Sol bölmede, her ortam için dağıtım her işlemin durumunu gösterir.

    Kişi simgesini **eylem** sütun dağıtım onaylayan (veya reddedildi) bakın ve sağladıkları ileti bir dağıtım öncesi veya dağıtım sonrası onayı.

2. Dağıtım tamamlandıktan sonra tüm günlük dosyasına sağ bölmede görüntülenir. Seçin **adım** sol bölmesinde "İşi başlatılamıyor" gibi tek bir adım için günlük dosyasına bakın. Tek tek günlükleri görme olanağı, izlemek ve genel dağıtım bölümlerinde hata ayıklamak kolaylaştırır. Ayrıca **Kaydet** bir adım için günlük dosyasına veya **tüm günlükleri zip olarak indir**.

    ![Yayın günlükleri - Azure DevOps Hizmetleri](media/azure-stack-solution-hybrid-pipeline/203.png)

3. Açık **özeti** sürüm hakkındaki genel bilgileri görmek için sekmesinde. Bu görünüm, yapı, için dağıtılan ortamları, dağıtım durumu ve sürüm hakkındaki diğer bilgileri hakkındaki ayrıntıları gösterir.

4. Bir ortam bağlantıyı seçin (**Azure** veya **Azure Stack**) belirli bir ortama dağıtımları bekleyen ve varolan hakkında bilgi için. Bu görünümler, aynı derlemenin iki ortamlara dağıtılan doğrulamak için hızlı bir yolu olarak kullanabilirsiniz.

5. Açık **üretim uygulamasını dağıtmışsınızdır** tarayıcınızda. Örneğin, Azure App Services Web sitesi için URL'yi açmaya `https://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Sonraki adımlar

* Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
