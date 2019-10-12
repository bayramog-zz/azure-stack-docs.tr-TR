---
title: Uygulamaları Azure 'a dağıtma ve Azure Stack
description: Azure 'a uygulama dağıtmayı ve karma CI/CD işlem hattı ile Azure Stack öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/23/2019
ms.topic: conceptual
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/07/2018
ms.openlocfilehash: eb9ed23437d5fd708d3f98d5a5b601f3ed1a02a0
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283731"
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Uygulamaları Azure 'a dağıtma ve Azure Stack

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu çözüm, Azure 'a uygulama dağıtmayı ve Azure Pipelines karma sürekli tümleştirme ve sürekli teslim (CI/CD) kullanarak Azure Stack gösterir.

Azure Stack, Azure DevOps ve Web uygulamalarını [önkoşullara](#prerequisites)göre ayarladıktan sonra şunları yapabilirsiniz:

> [!div class="checklist"]
> - Web uygulamanızı kaydedin ve Azure Active Directory (Azure AD) Azure Pipelines erişimi ayarlayın. 
> - Azure AD ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) için Azure Pipelines uç noktaları oluşturun. 
> - Azure Pipelines yapı aracısını Azure Stack derleme sunucusuna yükler. 
> - Bağımsız uygulama dağıtımını Azure 'da yapılandırın ve Azure Stack.
> - Projenize kod yürütmelerine göre otomatik derlemeler çalıştıran bir yapı işlem hattı oluşturun.
> - Derlemelerinizi otomatik olarak Azure AD ve Azure Stack dağıtan bir yayın işlem hattı oluşturun. 
> - Yayınları el ile oluşturun ve dağıtın ve yayın özetlerini ve günlüklerini görüntüleyin. 

CI/CD hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Sürekli tümleştirme nedir?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [Sürekli teslim nedir?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="azure-stack-and-hybrid-cicd"></a>Azure Stack ve karma CI/CD

Microsoft Azure Stack, şirket içi ortamınıza bulut bilgi işlemin çevikliğini ve yeniliklerini getiren bir Azure uzantısıdır. Bu, şirket içinde ve genel bulut ortamlarında karma uygulamalar oluşturmanıza ve dağıtmanıza olanak tanıyan tek karma bulutunuz. Azure Stack bir uygulama oluşturabilir ve ardından bunu Azure Stack, Azure veya Azure hibrit buluta dağıtabilirsiniz. 

Uygulama dağıtımı sürekliliği, güvenlik ve güvenilirlik, kuruluşunuz ve geliştirme ekibiniz için önemli öğelerdir. Karma CI/CD teslim modeli Azure Pipelines, şirket içi ortamınızda ve genel bulutta yapı işlem hatlarınızı birleştirip uygulamanızı değiştirmeden dağıtım konumlarını değiştirmenize olanak sağlar. Karma yaklaşımı kullanmanın diğer avantajları şunlardır:

- Şirket içi Azure Stack ortamınızda ve Azure genel bulutunda tutarlı bir geliştirme araçları kümesi tutabilirsiniz.  Ortak bir araç kümesi, CI/CD desenleri ve uygulamaları uygulamayı kolaylaştırır.
- Azure 'da veya Azure Stack dağıtılan uygulamalar ve hizmetler birbirinin yerini alabilir ve aynı kod her iki konumda da çalıştırılabilir. Şirket içi ve genel bulut özelliklerinden ve özelliklerinden faydalanabilirsiniz.

> [!TIP]
> @no__t -0hybrid-pil,. png @ no__t-1  
> [Azure Stack Için hibrit bulut tasarım desenleri](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesi ile ilgili olarak gözden geçirir. Kalite ölçütleri yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik içerir. Bu tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur

## <a name="prerequisites"></a>Prerequisites

- Azure ve Azure Stack temel bilgileri. Bu çözümü dağıtmaya başlamadan daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:
  
  - [Azure 'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
  - [Azure Stack genel bakış](../operator/azure-stack-overview.md)
  
- Bir Azure aboneliği. Bir tane yoksa, başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
  
- Azure 'da oluşturulan bir Web uygulaması. Bir [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/) kullanarak hem şirket içinde hem de genel buluta dağıtabileceğiniz bir Web uygulaması oluşturun. Daha sonra kullanmak için uygulama URI 'sini bir yere unutmayın. 
  
- Visual Studio 2019 [yüklendi](/visualstudio/install/install-visual-studio).
  
- İşlem hatları ve bir DevOps [projesi](/azure/devops/organizations/projects/create-project) veya [çalışma alanı](/azure/devops/repos/tfvc/create-work-workspaces)oluşturabileceğiniz bir [Azure DevOps](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) kuruluşuna yönetici erişimi. 
  
- Özel derleme aracısı sanal makinesi (VM) için .NET 3,5 içeren bir Windows Server 2016 görüntüsü, Azure Stack derlemeleri Azure Pipelines.
  
- Aşağıdaki yönergelere göre dağıtılan ve yapılandırılan Azure Stack tümleşik bir sistem veya Azure Stack Geliştirme Seti (ASDK). 
  
  
  Azure Stack Geliştirme Seti (ASDK), ücretsiz olarak indirebileceğiniz ve kullanabileceğiniz Azure Stack tek düğümlü bir dağıtımdır. ASDK, Azure Stack değerlendirmenize ve üretim dışı bir ortamda Azure API 'Leri ve araçları kullanmanıza olanak sağlar.
  
  Azure AD veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) yönetici kimlik bilgilerine sahip herhangi bir Kullanıcı, ASDK 'yi dağıtabilir. Bir Azure OEM/donanım ortağı, bir üretim Azure Stack dağıtabilir. Aşağıdaki Azure Stack yapılandırma görevlerini yapmak için bir Azure Stack işleci olmanız gerekir: 
  
  - Azure App Service dağıt
  - Planlar ve teklifler oluşturma
  - Kiracı aboneliği oluşturma
  - Windows Server 2016 görüntüsü uygulama
  
  > [!Note]
  > ASDK yüklemesi yaklaşık yedi saat sürer, bu yüzden buna göre planlayın.
    
  ASDK 'yi dağıtmak ve yapılandırmak için:
  
  1. [Yükleyiciyi kullanarak ASDK dağıtma](../asdk/asdk-install.md)bölümündeki ayrıntılı dağıtım talimatlarını izleyin.
     
  1. ASDK dağıtım sonrası adımlarını otomatik hale getirmek için [Configasdk. ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) PowerShell betiğini kullanın.
     
  1. [Azure App Service](../operator/azure-stack-app-service-deploy.md) PaaS hizmetlerini Azure Stack olarak dağıtın.
     
  1. Azure Stack bir [plan ve teklif](../operator/service-plan-offer-subscription-overview.md) oluşturun.
     
  1. Azure Stack ' de teklifine bir [Kiracı aboneliği](../operator/azure-stack-subscribe-plan-provision-vm.md) oluşturun. 
     
  1. Kiracı aboneliğinde bir Web uygulaması oluşturun. Yeni Web uygulaması URL 'sini daha sonra kullanmak üzere unutmayın.
     
  1. Azure Pipelines çalıştıran yapı sunucusu olması için kiracı aboneliğinde .NET 3,5 ile bir Windows Server 2016 VM dağıtın.
  
  > [!Note]
  > Azure Stack ortamınız Windows Server ve SQL Server çalıştırmak için doğru görüntülere ihtiyaç duyuyor. Ayrıca, App Service dağıtılması gerekir.

## <a name="register-your-web-app-and-give-it-access-to-resources"></a>Web uygulamanızı kaydedin ve kaynaklara erişim izni verin 

Azure Active Directory (Azure AD) ' de, *hizmet sorumlusu*kullanarak Azure Resource Manager karşı Azure Pipelines doğrular. Azure Pipelines kaynakları sağlamak için, hizmet sorumlusunun Azure aboneliğinde **katkıda** bulunan rolü olması gerekir. 

Uygulamanızın kimlik doğrulamasını yapılandırmak için Azure portal kullanabilirsiniz. 

1. Hizmet sorumlusu oluşturmak için uygulamanızı kaydedin.
1. **Katkıda** bulunan rolüne hizmet asıl adı (SPN) vermek için *rol tabanlı ERIŞIM denetimi (RBAC)* kullanın.
1. Azure Pipelines için uç noktalar oluşturmanız gereken uygulama KIMLIĞI ve kiracı KIMLIĞI değerlerini kopyalayın ve kaydedin. 
1. Bir uygulama gizli anahtarı değeri oluşturun ve kaydedin.

Bir hizmet sorumlusu ve uç noktaları oluşturmak için [bir PowerShell betiği de kullanabilirsiniz](https://github.com/Microsoft/vsts-rm-extensions/blob/master/TaskModules/powershell/Azure/SPNCreation.ps1#L5) . [Mevcut hizmet sorumlusu ile Azure Resource Manager hizmet bağlantısı oluşturma](/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal) makalesi bu işlemi açıklar.

 > [!Note]  
 > PowerShell betiğini Azure Stack Azure Resource Manager uç noktası oluşturmak için kullanırsanız **-azurestackmanagementurl** parametresini ve **-environmentname** parametresini geçirmeniz gerekir. Örneğin:  
 > `-azureStackManagementURL https://management.local.azurestack.external -environmentName AzureStack`

### <a name="register-your-app-in-azure-ad"></a>Uygulamanızı Azure AD 'ye kaydetme 

1. [Azure portal](https://portal.azure.com) **Azure Active Directory**' i seçin ve ardından sol gezinti bölmesinde **uygulama kayıtları** ' yı seçin.
   
1. **Yeni kayıt**seçeneğini belirleyin.
   
1. **Uygulama kaydetme** sayfasında
   1. Web uygulamanızın adını girin.
   1. Desteklenen bir hesap türü seçin. 
   1. **Yeniden yönlendirme URI 'si**altında, oluşturmak istediğiniz uygulama türü için **Web** ' i seçin ve Web uygulamanızın URI 'sini girin. 
   1. **Kaydol**' u seçin.
      
      ![Uygulamanızı kaydedin](./media/azure-stack-solution-pipeline/create-app.png) 

### <a name="assign-the-app-to-a-role"></a>Uygulamayı bir role atama

Aboneliğinizdeki kaynaklara erişebilmesi için uygulamanızı bir role atamanız gerekir. Azure RBAC, kullanıcıların işlerini yapması için ihtiyaç duyduğu erişim düzeyini denetlemenizi sağlar. RBAC hakkında daha fazla bilgi için bkz. [Azure abonelik kaynaklarına erişimi yönetme](/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json). Kullanılabilir roller hakkında daha fazla bilgi edinmek için bkz. [RBAC: yerleşik roller](/azure/role-based-access-control/built-in-roles).

Azure Pipelines, bir Azure Stack aboneliğinde kaynak sağlayabilmek için **katkıda** bulunan rolüne sahip olmalıdır. 

Rol kapsamını abonelik, kaynak grubu veya kaynak düzeyinde ayarlayabilirsiniz. İzinler, daha düşük kapsam düzeylerine devralınır. Örneğin, bir kaynak grubu için **okuyucu** rolüne bir uygulama eklemek, uygulamanın kaynak grubunu ve kaynaklarından herhangi birini okuyabileceği anlamına gelir.

Uygulamanızı **katkıda bulunan** rolüne atamak için:

1. Azure portal istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için **tüm hizmetler** ve **abonelikler**' i seçin.
   
   ![Abonelik kapsamında rol atama](./media/azure-stack-solution-pipeline/select-subscription.png)
   
1. Uygulamanın atanacağı aboneliği seçin.
   
1. Sol gezinti bölmesinde **erişim denetimi (IAM)** seçeneğini belirleyin.
   
1. **Rol ataması Ekle**' yi seçin.
   
1. **Rol ataması Ekle** iletişim kutusunda, **katkıda bulunan** rolünü seçin. Varsayılan olarak, Azure AD uygulamaları kullanılabilir seçeneklerde gösterilmez. Uygulamanızı bulmak için adı arayın ve seçin.
   
   ![Rolü ve uygulamayı seçin](./media/azure-stack-solution-pipeline/select-role.png)
   
1. Rolü atamaya son vermek için **Kaydet** ' i seçin. Uygulamanızı bu kapsam için bir role atanan kullanıcılar listesinde görebilirsiniz.

Hizmet sorumlusu ayarlanır. Sonraki bölümde, Azure Pipelines programlı olarak oturum açması için gereken değerlerin nasıl alınacağı gösterilmektedir.

### <a name="get-values-for-signing-in"></a>Oturum açmak için değerleri Al

Azure Pipelines için uç noktalar oluştururken, kiracı KIMLIĞINI ve uygulama KIMLIĞINI girmeniz gerekir. Bu değerleri almak için:

1. Azure portal **Azure Active Directory**' ni seçin.
   
1. Sol gezinti bölmesinde **uygulama kayıtları**' yi seçin ve ardından uygulamanızı seçin.
   
1. Uç nokta oluşturmak için kullanılacak **Dizin (kiracı) kimliğini** ve **uygulama (istemci) kimliğini** kopyalayın ve kaydedin.
   
   ![Dizini (kiracı KIMLIĞI) ve uygulama (istemci) KIMLIĞINI Kopyala](./media/azure-stack-solution-pipeline/copy-app-id.png)

### <a name="create-a-new-application-secret"></a>Yeni bir uygulama parolası oluştur

Azure Pipelines için uç noktalar oluştururken uygulamanın kimlik doğrulaması gerekir. [Bir sertifika](/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) veya bir uygulama gizli anahtarı kullanabilir. Kimlik sağlayıcısı olarak AD FS Azure Stack dağıttıysanız, bir sertifika kullanmanız gerekir.

Yeni bir sertifika oluşturmak ve yüklemek için [Sertifikalar ve gizli](/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) dizileri bölümündeki adımları izleyin. 

Ya da yeni bir uygulama parolası oluşturmak için:

1. Azure portal **Azure Active Directory**' ni seçin.
   
1. Sol gezinti bölmesinde **uygulama kayıtları**' yi seçin ve ardından uygulamanızı seçin.
   
1. Sol gezinti bölmesinde **sertifikalar & gizlilikler**' ı seçin.
   
1. **İstemci gizli**dizileri altında **yeni istemci parolası**' nı seçin.
   
1. **İstemci parolası Ekle**' de bir açıklama yazın, süre sonu ' nu seçin ve **Ekle**' yi seçin.
   
1. Yeni gizliliğin **değerini** kopyalayın. Uygulama olarak oturum açmak için değeri sağlamanız gerekir. Bu sayfadan ayrıldıktan sonra yeniden görüntülenmeyeceği için, bu değeri şimdi kaydetmeniz önemlidir.
   
   ![Daha sonra alamadığı için gizli değeri kopyalayın](./media/azure-stack-solution-pipeline/copy-secret.png)

## <a name="create-endpoints"></a>Uç noktalar oluşturma

Uç noktalar oluşturarak Azure Pipelines bir derleme, Azure Stack için Azure AD uygulamalarını dağıtabilir. Azure Pipelines, Azure Stack bağlanan yapı aracısına bağlanır.

Uç nokta oluşturma izinlerini ayarladıktan sonra Azure AD veya AD FS için uç noktalar oluşturabilirsiniz. 

- Kimlik sağlayıcısı olarak Azure AD ile Azure Stack dağıttıysanız, Azure dağıtımları için Azure Resource Manager hizmet bağlantısı oluşturmak üzere bir sertifika veya uygulama gizli anahtarı kullanabilirsiniz. 
  
- Azure Stack kimlik sağlayıcısı olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullandıysanız, kimlik doğrulaması için bir istemci parolası yerine bir sertifika kullanarak hizmet bağlantısı oluşturmanız gerekir. 

### <a name="set-endpoint-creation-permissions"></a>Uç nokta oluşturma izinlerini ayarlama

1. Azure DevOps kuruluşunuzda ve projenizde **proje ayarları**' nı seçin. 
   
1. **Güvenlik**' i seçin ve **Azure DevOps grupları**altında **Endpoint Administrators**' ı seçin.
   
1. **Üyeler** sekmesinde **Ekle**' yi seçin.
   
1. Kullanıcı **ve Grup Ekle**' de, listeden Kullanıcı adları ' nı seçin ve ardından **Değişiklikleri Kaydet**' i seçin.
   
   ![Üye ekleme](./media/azure-stack-solution-pipeline/endpoint-permissions.png)
   
1. **Azure DevOps grupları** listesinde **Endpoint Creators**' ı seçin ve **uç nokta oluşturucuları** grubuna kullanıcı eklemek için önceki adımları tekrarlayın. 

### <a name="create-an-endpoint-for-azure-ad-or-ad-fs-deployments"></a>Azure AD veya AD FS dağıtımları için bir uç nokta oluşturma

Hizmet bağlantı uç noktası oluşturmak için [var olan bir hizmet sorumlusu ile Azure Resource Manager hizmet bağlantısı oluşturma](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal) bölümündeki yönergeleri izleyin.  

Formu doldururken aşağıdaki değerleri kullanın: 

- **Bağlantı adı**: Bu hizmet bağlantısına başvururken kullanılacak kullanıcı dostu bir ad girin.
  
- **Ortam**: **azurecyüksek** veya **azurestack**gibi ortam adını seçin. Açılan listede AzureStack ' i görmüyorsanız bkz. [Connect Azure Stack](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops#connect-to-azure-stack).
  
- **Ortam URL 'si**: **azurecyüksek**SEÇENEĞINI seçmediyseniz, ortam URL 'nizi (örneğin, *https: \//Management. Local. azurestack. external*) girin.
  
- **Kapsam düzeyi**: **abonelik**gibi, ihtiyacınız olan kapsam düzeyini seçin. 
  
- **ABONELIK kimliği**: abonelik kimliğinizi girin.
  
- **Abonelik adı**: Azure Stack Kullanıcı adınızı girin.
  
- **Hizmet sorumlusu ISTEMCI kimliği**: daha önce kaydettiğiniz **uygulama (istemci) kimliğini** girin. 
  
- **Hizmet sorumlusu anahtarı** veya **sertifikası**: bir veya diğer seçeneği belirleyin. 
  
  > [!NOTE]
  > AD FS uç noktası oluşturmak için kimlik doğrulaması için bir sertifika kullanmanız gerekir. 
  
  - **Hizmet sorumlusu anahtarı**için, daha önce kaydettiğiniz istemci gizli anahtarını girin.
  - **Sertifika**' yı seçerseniz, *. ped* sertifika dosyasının hem sertifika hem de özel anahtar bölümlerinin içeriğini girin. 
    
    > [!NOTE]
    > Bir *. pfx* dosyasını *. pek* sertifika dosyasına dönüştürmek için `openssl pkcs12 -in file.pfx -out file.pem -nodes -password pass:<password_here>` ' yi çalıştırın.
  
- **KIRACı kimliği**: daha önce kaydettiğiniz **Dizin (kiracı) kimliğini** girin.
  
- **Bağlantı: doğrulanmadı**: hizmet sorumlusu için bağlantı ayarlarınızı doğrulamak üzere **bağlantıyı doğrula** ' yı seçin.
  
  > [!NOTE]
  > Azure Resource Manager uç noktanız Internet 'e açık değilse, bağlantı doğrulaması başarısız olur. Bu beklenen bir işlemdir ve basit bir görevle bir yayın işlem hattı oluşturarak bağlantınızı doğrulayabilirsiniz.

## <a name="install-and-configure-the-build-agent"></a>Yapı aracısını yükleyip yapılandırma 

Azure Pipelines içinde barındırılan bir yapı aracısının kullanılması, Web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Azure otomatik olarak aracı bakımı ve yükseltmeleri gerçekleştirerek sürekli ve kesintisiz bir geliştirme döngüsünü etkinleştirir.

Azure DevOps 'da Azure Stack için kullanmak üzere bir kişisel erişim belirteci (PAT) oluşturun. Ardından, Azure Stack yapı sunucusu VM 'sinde yapı aracısını dağıtmak ve yapılandırmak için PAT kullanın. 
   
### <a name="create-a-personal-access-token"></a>Kişisel erişim belirteci oluşturma

1. Azure DevOps ' da oturum açın ve sağ üst köşedeki **profilimi** seçin. 
   
1. Profil sayfanızda Azure Stack kuruluş adınızın yanındaki açılan listeyi genişletin ve **Güvenliği Yönet**' i seçin. 
   
   ![Güvenliği Yönet](media/azure-stack-solution-pipeline/managesecurity.png)
   
1. **Kişisel erişim belirteçleri** sayfasında **Yeni belirteç**' yi seçin.
   
   ![Kişisel erişim belirteçleri](media/azure-stack-solution-pipeline/pats.png)
   
1. **Yeni kişisel erişim belirteci oluştur** sayfasında, belirteç bilgilerini doldurun ve **Oluştur**' u seçin. 
   
   ![PAT oluşturma](media/azure-stack-solution-pipeline/createpat.png)
   
1. Belirteci kopyalayın ve kaydedin. Web sayfasından ayrıldıktan sonra bir daha gösterilmez.
   
   ![PAT uyarısı](media/azure-stack-solution-pipeline/patwarning.png)
   
### <a name="install-and-configure-the-agent-on-the-build-server"></a>Aracıyı derleme sunucusuna yükleyip yapılandırma

1. Azure Stack konağında dağıttığınız yapı sunucusu sanal makinesine bağlanın.
   
1. Yapı Aracısı görüntüsünü indirin. 
   
1. Yönetici komut isteminde, PAT 'nizi kullanarak aracıyı hizmet olarak dağıtın ve çalıştırın.
   
1. Ayıklanan derleme aracısının klasörüne gidin ve *config. cmd* dosyasını çalıştırın. *Config. cmd* , derleme Aracısı klasörünü ek dosyalarla güncelleştirir.
   
   ![Yapı aracısını yükleyip yapılandırma](media/azure-stack-solution-pipeline/buildagent.png)

Uç noktasını oluşturup Azure Pipelines yapı aracısını derleme sunucusuna yükleolduğunuza göre, Azure Stack bağlantı Azure Pipelines kullanıma hazırdır. Azure Stack içindeki yapı Aracısı Azure Pipelines yönergeleri alır ve aracı Azure Stack ile iletişim için uç nokta bilgilerini alır.

Her aracıyı ayrı ayrı yönetmek yerine aracıları *Aracı havuzlarıyla*düzenleyebilirsiniz. Bir aracı havuzu, bu havuzdaki tüm aracıların paylaşım sınırını tanımlar. Aracı havuzları, Azure DevOps kuruluşunun kapsamına alınır ve bu, projeler genelinde bir aracı havuzunu paylaşabileceğiniz anlamına gelir. Aracı havuzları hakkında daha fazla bilgi için bkz. [Aracı havuzları ve kuyruklar oluşturma](/azure/devops/pipelines/agents/pools-queues).

## <a name="create-build-and-release-pipelines"></a>Derleme ve yayın işlem hatları oluşturma 

Azure Pipelines, yayınlar için geliştirme, hazırlama, kalite güvencesi (QA) ve üretim gibi birden çok ortama yönelik yüksek düzeyde yapılandırılabilir ve yönetilebilir bir işlem hattı sağlar. Serbest bırakma işlemi, uygulama yaşam döngüsünün belirli aşamalarındaki onayları gerektirmeyi içerebilir.

Çözümün bu bölümünde şunları yapabilirsiniz:

- Visual Studio 'da Azure DevOps projenize kopyalama, bağlama ve kod ekleme.
- Kendi içinde bir Web uygulaması dağıtımı oluşturun.
- CI/CD derleme ve yayın işlem hatlarını yapılandırın.

Karma CI/CD, hem uygulama kodu hem de altyapı kodu için uygulanabilir. Azure Web App kodunuzu şirket içi ve genel bulutlara dağıtmak için [karma şablonlar Azure Resource Manager](https://azure.microsoft.com/resources/templates/) kullanabilirsiniz.

### <a name="clone-your-project"></a>Projenizi kopyalayın

1. Visual Studio **Takım Gezgini**' de **Bağlan** simgesini seçin ve Azure DevOps kuruluşunuzda oturum açın. 
   
1. @No__t **Bağlantıları Yönet**' i seçin-1**bir projeye bağlanın**. 
   
   ![Takım Gezgini bir projeye bağlanma](media/azure-stack-solution-pipeline/connecttoprojectteamexp.png)

1. **Bir projeye Bağlan** iletişim kutusunda Web uygulaması projenizi seçin, bir yerel yol ayarlayın ve ardından depoyu yerel olarak kopyalamak için **Kopyala** ' yı seçin.
   
   ![Bir projeye Bağlan öğesinde depoyu Kopyala](media/azure-stack-solution-pipeline/cloneproject.png)

### <a name="create-a-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Her iki bulutta da uygulama hizmetleri için kendi kendine içerilen bir Web uygulaması dağıtımı oluşturun

1. Visual Studio **Çözüm Gezgini**' de, *WebApplication. csproj* dosyanızı açın ve @no__t ekleyin. Bu adımla ilgili daha fazla bilgi için, bkz. [kendi içinde dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd).
   
   ![Runtimeıdentifier 'ı yapılandırma](media/azure-stack-solution-pipeline/runtimeidentifier.png)
   
1. Çalışmanızı kaydedin ve projenizdeki kodu denetlemek için **Takım Gezgini** kullanın.

### <a name="create-a-build-pipeline-and-run-a-build"></a>Derleme işlem hattı oluşturma ve derleme çalıştırma

1. Web tarayıcınızda, Azure DevOps kuruluşunuzu ve projenizi açın.
   
1. Sol gezinti bölmesinde işlem **hatları** > **derlemeleri** seçin ve ardından yeni işlem **hattı**' nı seçin. 
   
1. **Şablon seç**' in altında **ASP.NET Core** şablonunu seçin ve ardından **Uygula**' yı seçin. 
   
1. Yapılandırma sayfasında, sol bölmede **Yayımla** ' yı seçin.
   
1. Sağ bölmedeki **bağımsız değişkenler**altında, yapılandırmaya `-r win10-x64` ekleyin. 
   
   ![Derleme ardışık düzen bağımsız değişkeni Ekle](media/azure-stack-solution-pipeline/buildargument.png)
   
1. Sayfanın üst kısmında **& kuyruğu kaydet** ' i seçin.
   
1. İşlem hattını **Çalıştır** Iletişim kutusunda **Kaydet ve Çalıştır**' ı seçin. 
   
[Kendi içinde çalışan dağıtım derlemesi](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) hem Azure 'da hem de Azure Stack çalışabilen yapıtları yayımlar.

### <a name="create-a-release-pipeline"></a>Yayın işlem hattı oluşturma

Yayın işlem hattı oluşturmak, karma CI/CD yapılandırma işleminizin son adımıdır. Yayın işlem hattını kullanarak bir yayın oluşturur ve yapınızı dağıtabilirsiniz.

1. Azure DevOps projenizde, sol gezinti bölmesinde işlem **hatları** > **yayınlar** ' ı seçin ve ardından yeni işlem **hattı**' nı seçin. 
   
1. **Şablon seçin** sayfasında, **Azure App Service dağıtım**' ı seçin ve ardından **Uygula**' yı seçin.
   
   ![Yayın şablonunu seçin](media/azure-stack-solution-pipeline/releasetemplate.png)
   
1. **Ardışık düzen** sekmesinde, sol bölmedeki **yapıt Ekle** ' yi seçin. Sağ bölmede, **kaynak (derleme işlem hattı)** açılan menüsünden Yeni oluşturduğunuz Web uygulaması derlemesini seçin ve **Ekle**' yi seçin.
   
   ![Derleme yapıtı ekleme](media/azure-stack-solution-pipeline/addartifact.png)
   
1. İşlem **hattı** sekmesinde, **aşamalar**' ın altında, **aşama görevlerini görüntülemek**için **1. aşama** ' daki köprüyü seçin.
   
   ![Aşama görevlerini görüntüle](media/azure-stack-solution-pipeline/viewstagetasks.png)
   
1. **Görevler** sekmesinde, **aşama adı**olarak *Azure* girin. 
   
1. **Parametreler**altında, **Azure aboneliği** açılır listesinden aboneliğinizi seçin ve **App Service adınızı**girin.
   
   ![Abonelik ' ı seçin ve App Service adı girin](media/azure-stack-solution-pipeline/stage1.png)
   
1. Sol bölmede, **aracıda Çalıştır**' ı seçin. Sağ bölmede, zaten seçili değilse, **Aracı havuzu** açılır LISTESINDEN **barındırılan VS2017** öğesini seçin.
   
   ![Barındırılan aracıyı seçin](media/azure-stack-solution-pipeline/agentjob.png)
   
1. Sol bölmede **Azure App Service dağıt**' ı seçin ve sağ bölmedeki Azure Web uygulaması derlemeniz için **pakete veya klasöre** gidin.
   
   ![Paket veya klasör seçin](media/azure-stack-solution-pipeline/packageorfolder.png)
   
1. **Dosya veya klasör seçin** Iletişim kutusunda **Tamam**' ı seçin.
   
1. **Yeni yayın ardışık düzeni** sayfasında sağ üst köşedeki **Kaydet** ' i seçin.
   
   ![Değişiklikleri Kaydet](media/azure-stack-solution-pipeline/save-devops-icon.png)
   
1. İşlem **hattı** sekmesinde **yapıt Ekle**' yi seçin. Projenizi seçin ve **kaynak (derleme işlem hattı)** açılır menüsünden Azure Stack yapınızı seçin. **Ekle**' yi seçin. 
   
1. İşlem **hattı** sekmesinde, **aşamalar**altında **Ekle**' yi seçin.
   
1. Yeni aşamada, **aşama görevlerini görüntülemek**için köprüyü seçin. Aşama adı olarak *Azure Stack* girin. 
   
   ![Yeni aşamayı görüntüle](media/azure-stack-solution-pipeline/newstage.png)
   
1. **Parametreler**altında Azure Stack uç noktanızı seçin ve **App service adı**olarak Azure Stack Web uygulamanızın adını girin.
   
1. **Aracıda Çalıştır**altında, **Aracı havuzu** açılır listesinden Azure Stack yapı sunucu aracınızı seçin.
   
1. **Dağıtım Azure App Service**için, Azure Stack derlemesi **için paketi veya klasörü** seçin ve ardından **bir dosya veya klasör seçin** iletişim kutusunda **Tamam** ' ı seçin.
   
1. **Değişkenler** sekmesinde, **VSTS_ARM_REST_IGNORE_SSL_ERRORS**adlı değişkeni bulun. Değişken değerini **true**olarak ayarlayın ve kapsamını **Azure Stack**olarak ayarlayın.
   
   ![Değişkeni Yapılandır](media/azure-stack-solution-pipeline/setvariable.png)
   
1. İşlem **hattı** sekmesinde her yapıt için **sürekli dağıtım tetikleme** simgesini seçin ve **etkin**olarak ayarlayın.  
   
   ![Sürekli dağıtım tetikleyicisi ayarla](media/azure-stack-solution-pipeline/contindeploymentenabled.png)
   
1. Azure Stack aşamada **dağıtım öncesi koşullar** simgesini seçin ve tetikleyiciyi **yayından sonra**olarak ayarlayın.
   
   ![Dağıtım öncesi koşullar tetikleyicisi ayarla](media/azure-stack-solution-pipeline/predeployafterrelease.png)
   
1. Yayın işlem hattını kaydetmek için **Yeni yayın işlem hattı** sayfasında sağ üst köşedeki **Kaydet** ' i seçin.

> [!Note]
> Yayın görevlerinin bazı ayarları, şablondan yayın işlem hattını oluştururken otomatik olarak [özel değişkenler](/azure/devops/pipelines/release/variables?view=vsts#custom-variables) olarak tanımlanabilir. Bu ayarlar görev ayarlarından değiştirilemez, ancak bunları üst aşama öğelerinde düzenleyebilirsiniz.

## <a name="release-the-app"></a>Uygulamayı serbest bırakma

Artık bir yayın işlem hattına sahip olduğunuza göre, bir yayın oluşturmak ve uygulamanızı dağıtmak için kullanabilirsiniz. 

Yayın işlem hattınızda sürekli dağıtım tetikleyicisi ayarlandığından, kaynak kodu değiştirmek yeni bir derleme başlatır ve otomatik olarak yeni bir yayın oluşturur. Bununla birlikte, bu yeni yayını oluşturup el ile çalıştıracaksınız.

Bir sürüm oluşturup dağıtmak için:

1. Yeni sürüm ardışık düzeni sayfanızda sağ üst köşedeki **yayın oluştur** ' u seçin. 
   
   ![Yayın oluştur ](media/azure-stack-solution-pipeline/createreleaseicon.png)
   
1. **Yeni yayın oluştur** sayfasında:
   1. İşlem **hattı**altında, tetikleyicisinin otomatik olarak el ile olarak değiştirilmesini sağlamak için **Azure** aşamasını seçin. 
   1. **Yapıtlar**' ın altında, doğru yapıtların seçildiğinden emin olun.
   1. Bir **sürüm açıklaması**girin ve ardından **Oluştur**' u seçin. 
   
   Bir başlık, yeni sürümün oluşturulduğunu gösterir. Dağıtım durumu gibi, sürüm hakkındaki ayrıntıları gösteren bir sürüm Özeti sayfasını görmek için sürüm adı bağlantısını seçebilirsiniz.
   
1. El ile yayını dağıtmak için **Azure** aşamasını seçin, **Dağıt**' ı seçin ve ardından Aşama iletişim kutusunda **Dağıt** ' ı seçin. 
   
1. Dağıtım başarıyla tamamlandığında, dağıtılan uygulamayı tarayıcınızda açın. Örneğin, Azure App Services Web sitesi için URL 'YI `https://<your-app-name>.azurewebsites.net` ' ı açın.

### <a name="monitor-and-track-releases"></a>İzleme ve izleme yayınları

Dağıtım hakkında daha fazla bilgi için bir yayın aşamasında köprü durumunu seçebilirsiniz. 

![Yayın Özeti sayfası](media/azure-stack-solution-pipeline/releasesummary.png)

Yöneticinin, yayınların ilerlemesini izlemek ve hangi sürümlerin onay beklediğini görmek çok kolay.

![Bekleyen onayı gösteren yayın Özeti sayfası](media/azure-stack-solution-pipeline/releasepending.png)

Tüm dağıtımlarınızdan yayın günlüklerini görebilirsiniz: 

1. Azure DevOps projenizde, sol taraftaki işlem **hatları** **@no__t 1 '** i seçin ve ardından bir sürüm seçin. 
   
1. Yayın Özeti sayfasında, üzerine gelin veya bir aşama seçin ve ardından **Günlükler**' i seçin. 
   
   Yayın günlüğünde, sol bölmede her bir aşamanın her bir işlemin durumu gösterilir. Dağıtım sırasında sağ bölmede aracıdan canlı günlük görüntülenir. Dağıtım bittikten sonra, tüm günlük dosyası sağ bölmede görüntülenir. 
   
1. **Dağıtım öncesi onaylar**gibi, bu adım için günlük dosyasını görmek üzere sol bölmedeki herhangi bir adımı seçin. 
   
1. Onayları görüntülemek için, sürümü onaylayan veya reddeden kişileri ve diğer ayrıntıları görmek için sağ bölmedeki **onayı görüntüle** ' yi seçin. 
   
Bireysel adımlarla ilgili günlükleri görmek, genel dağıtımın parçalarını izlemeyi ve hata ayıklamayı kolaylaştırır. Ayrıca, **tüm günlükleri** bir *. zip* dosyası olarak indirebilirsiniz.
   
![Yayın günlüğü](media/azure-stack-solution-pipeline/releaselog.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](/azure/architecture/patterns).
