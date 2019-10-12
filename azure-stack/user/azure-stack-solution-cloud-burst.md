---
title: Azure ve Azure Stack platformlar arası ölçeklendirme uygulaması çözümleri oluşturun | Microsoft Docs
description: Azure ile platformlar arası ölçeklendirme çözümleri oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: ca313e2e936f7bb006fdc1c15c49d271b1059a16
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283303"
---
# <a name="create-cross-cloud-scaling-app-solutions-with-azure-and-azure-stack"></a>Azure ve Azure Stack ile platformlar arası ölçeklendirme uygulaması çözümleri oluşturun

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack barındırılan bir Web uygulamasından, Traffic Manager aracılığıyla otomatik ölçeklendirmeyle Azure 'da barındırılan bir Web uygulamasına geçiş için el ile tetiklenen bir işlem sağlamak üzere bir çoklu bulut çözümü oluşturmayı öğrenin. Bu işlem, yük altında esnek ve ölçeklenebilir bulut yardımcı programı sağlar.

Bu Düzenle, kiracınız uygulamanızı genel bulutta çalıştırmaya hazırlamayabilir. Bununla birlikte, iş için şirket içi ortamlarında gereken kapasiteyi korumak, uygulama için talepte ani artışları karşılamak amacıyla ekonomik bir şekilde uygulanabilir olmayabilir. Kiracınız, şirket içi çözümüyle ortak bulutun esneklik düzeyini kullanabilir.

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Çok düğümlü bir Web uygulaması oluşturun.
> - Sürekli dağıtım (CD) işlemini yapılandırın ve yönetin.
> - Web uygulamasını Azure Stack yayımlayın.
> - Bir yayın oluşturun.
> - Dağıtımlarınızı izlemeyi ve izlemeyi öğrenin.

> [!Tip]  
> @no__t -0hybrid-pil,. png @ no__t-1  
> Microsoft Azure Stack bir Azure uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

## <a name="prerequisites"></a>Prerequisites

-   Azure aboneliği. Gerekirse, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Azure Stack tümleşik bir sistem veya Azure Stack Geliştirme Seti dağıtımı.
    - Azure Stack yükleme yönergeleri için bkz. [Azure Stack Geliştirme Seti yükleme](../asdk/asdk-install.md).
    - Bir ASDK dağıtım sonrası Otomasyon betiği için şuraya gidin: [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) 
    - Bu yüklemenin tamamlanabilmesi için birkaç saat gerekebilir.

-   [App Service](../operator/azure-stack-app-service-deploy.md) PaaS hizmetlerini Azure Stack olarak dağıtın.

-   Azure Stack ortamı içinde [plan/teklifler oluşturun](../operator/service-plan-offer-subscription-overview.md) .

-   Azure Stack ortamı içinde [Kiracı aboneliği oluşturun](../operator/azure-stack-subscribe-plan-provision-vm.md) .

-   Kiracı aboneliği içinde bir Web uygulaması oluşturun. Yeni Web uygulaması URL 'sini daha sonra kullanmak üzere unutmayın.

-   Kiracı aboneliği içinde Azure Pipelines sanal makineyi (VM) dağıtın.

-   .NET 3,5 ile Windows Server 2016 VM gereklidir. Bu VM, özel derleme aracısı olarak Azure Stack kiracı aboneliğinde oluşturulur.

-   [SQL 2017 Için Windows Server 2016 sanal makine görüntüsü](../operator/azure-stack-add-vm-image.md#add-a-vm-image-as-an-azure-stack-operator-using-the-portal) Azure Stack marketi 'nde kullanılabilir. Bu görüntü kullanılamıyorsa, ortama eklendiğinden emin olmak için Azure Stack Işleçle çalışın.

## <a name="issues-and-considerations"></a>Sorunlar ve konular

### <a name="scalability"></a>Ölçeklenebilirlik

Platformlar arası ölçeklendirmenin anahtar bileşeni, tutarlı ve güvenilir hizmet sağlayan ortak ve şirket içi bulut altyapısı arasında anında ve isteğe bağlı ölçeklendirme sunma olanağıdır.

### <a name="availability"></a>Kullanılabilirlik

Yerel olarak dağıtılan uygulamaların şirket içi donanım yapılandırması ve yazılım dağıtımı aracılığıyla yüksek kullanılabilirlik için yapılandırıldığından emin olun.

### <a name="manageability"></a>Yönetilebilirlik

Platformlar arası çözüm, ortamlar arasında sorunsuz yönetim ve tanıdık arabirim sağlar. Platformlar arası yönetim için PowerShell önerilir.

## <a name="cross-cloud-scaling"></a>Platformlar arası ölçeklendirme

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Özel etki alanı edinme ve DNS 'yi yapılandırma

Etki alanı için DNS bölge dosyasını güncelleştirin. Azure AD, özel etki alanı adının sahipliğini doğrulayacaktır. Azure 'da Azure/Office 365/dış DNS kayıtları için [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) kullanın veya DNS girişini [farklı bir DNS kaydedicisinde](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)ekleyin.

1.  Özel bir etki alanını ortak bir kayıt defteri ile kaydedin.

2.  Etki alanı için etki alanı adı kaydedicisi ' nde oturum açın. DNS güncelleştirmeleri yapmak için onaylanan yönetici gerekli olabilir.

3.  Azure AD tarafından belirtilen DNS girişini ekleyerek etki alanı için DNS bölge dosyasını güncelleştirin. (DNS girişi, posta yönlendirme veya Web barındırma davranışlarını etkilemez.)

### <a name="create-a-default-multi-node-web-app-in-azure-stack"></a>Azure Stack 'de varsayılan bir çok düğümlü Web uygulaması oluşturma

Web uygulamalarını Azure Stack Azure 'a dağıtmak için karma sürekli tümleştirme ve sürekli dağıtım (CI/CD) ayarlayın ve değişiklikleri her iki bulutta da otomatik olarak gönderin.

> [!Note]  
> Çalışmak için dağıtılmış uygun görüntülerle Azure Stack (Windows Server ve SQL) ve App Service dağıtımı gereklidir. Daha fazla bilgi için, [Azure Stack App Service kullanmaya başlamadan önce](../operator/azure-stack-app-service-before-you-get-started.md)App Service belgelerini gözden geçirin.

### <a name="add-code-to-azure-repos"></a>Azure Repos kod ekleme

Azure Repos

1. Azure Repos üzerinde proje oluşturma haklarına sahip bir hesapla Azure Repos için oturum açın.

    Karma CI/CD, hem uygulama kodu hem de altyapı kodu için uygulanabilir. Hem özel hem de barındırılan bulut geliştirmesi için [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) kullanın.

    ![Azure Repos bir projeye bağlanma](media/azure-stack-solution-cloud-burst/image1.JPG)

2. Varsayılan Web uygulamasını oluşturarak ve açarak **depoyu kopyalayın** .

    ![Azure Web App 'te Depo kopyalama](media/azure-stack-solution-cloud-burst/image2.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Her iki bulutta da uygulama hizmetleri için kendi kendine içerilen Web uygulaması dağıtımı oluşturma

1.  **WebApplication. csproj** dosyasını düzenleyin. @No__t-0 ' ı seçin ve @no__t ekleyin-1. (Bkz. [kendi Içinde dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri.) 

    ![Web uygulaması proje dosyasını Düzenle](media/azure-stack-solution-cloud-burst/image3.png)

2.  Takım Gezgini kullanarak Azure Repos kodu iade edin.

3.  Uygulama kodunun Azure Repos işaretli olduğunu doğrulayın.

## <a name="create-the-build-definition"></a>Derleme tanımı oluşturma

1. Derleme tanımları oluşturma özelliğini onaylamak için Azure Pipelines ' de oturum açın.

2. Add **-r win10-x64** kodu. Bu ek, .NET Core ile bağımsız bir dağıtımı tetiklemek için gereklidir.

    ![Web uygulamasına kod ekleme](media/azure-stack-solution-cloud-burst/image4.png)

3. Derlemeyi çalıştırın. [Kendi içinde çalışan dağıtım oluşturma](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) Işlemi, Azure 'da ve Azure Stack çalışan yapıtları yayımlar.

## <a name="use-an-azure-hosted-agent"></a>Azure barındırılan Aracısı kullanma

Azure Pipelines içinde barındırılan bir yapı aracısının kullanılması, Web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Bakım ve yükseltmeler Microsoft Azure tarafından otomatik olarak gerçekleştirilir ve sürekli ve kesintisiz bir geliştirme döngüsünü etkinleştirir.

### <a name="manage-and-configure-the-cd-process"></a>CD işlemini yönetme ve yapılandırma

Azure Pipelines ve Azure DevOps Server, yayınlar için geliştirme, hazırlık, QA ve üretim ortamları gibi birden çok ortama yüksek düzeyde yapılandırılabilir ve yönetilebilir bir işlem hattı sağlar; belirli aşamalarda onay gerektirme de dahil.

## <a name="create-release-definition"></a>Yayın tanımı oluştur

1.  VSO 'ın **derleme ve yayınlama** bölümündeki **yayınlar** sekmesinin altına yeni bir yayın eklemek için **artı** düğmesini seçin.

    ![Yayın tanımı oluştur](media/azure-stack-solution-cloud-burst/image5.png)

2. Azure App Service Dağıtım şablonunu uygulayın.

   ![Azure App Service dağıtım şablonu Uygula](meDia/azure-stack-solution-cloud-burst/image6.png)

3. **Yapıt Ekle**' nin altında, Azure Cloud Build uygulaması için yapıt ekleyin.

   ![Azure Cloud Build 'e yapıt ekleme](media/azure-stack-solution-cloud-burst/image7.png)

4. İşlem hattı sekmesi altında, ortamın **aşamasını, görev** bağlantısını seçin ve Azure bulut ortamı değerlerini ayarlayın.

   ![Azure bulut ortamı değerlerini ayarlama](media/azure-stack-solution-cloud-burst/image8.png)

5. **Ortam adını** ayarlayın ve Azure bulut uç noktası için **Azure aboneliğini** seçin.

      ![Azure bulut uç noktası için Azure aboneliğini seçin](media/azure-stack-solution-cloud-burst/image9.png)

6. **App Service adı**altında, gerekli Azure App Service adını ayarlayın.

      ![Azure App Service adını ayarla](media/azure-stack-solution-cloud-burst/image10.png)

7. Azure bulut barındırılan ortamı için **Aracı kuyruğu** altında "barındırılan VS2017" yazın.

      ![Azure bulut barındırılan ortamı için aracı kuyruğunu ayarla](media/azure-stack-solution-cloud-burst/image11.png)

8. Azure App Service dağıt menüsünde, ortam için geçerli **paketi veya klasörü** seçin. **Klasör konumuna** **Tamam ' ı** seçin.
  
      ![Azure App Service ortamı için paket veya klasör seçin](media/azure-stack-solution-cloud-burst/image12.png)

      ![Azure App Service ortamı için paket veya klasör seçin](media/azure-stack-solution-cloud-burst/image13.png)

9. Tüm değişiklikleri kaydedin ve **yayın ardışık düzenine**geri dönün.

    ![Değişiklikleri yayın ardışık düzeninde Kaydet](media/azure-stack-solution-cloud-burst/image14.png)

10. Azure Stack uygulaması için yapıyı seçerek yeni bir yapıt ekleyin.
    
    ![Azure Stack uygulaması için yeni yapıt ekleme](media/azure-stack-solution-cloud-burst/image15.png)


11. Azure App Service dağıtımını uygulayarak bir ortam daha ekleyin.
    
    ![Azure App Service dağıtımına ortam ekleme](media/azure-stack-solution-cloud-burst/image16.png)

12. Yeni ortam Azure Stack adlandırın.
    
    ![Azure App Service dağıtımında ad ortamı](media/azure-stack-solution-cloud-burst/image17.png)

13. **Görev** sekmesinde Azure Stack ortamını bulun.
    
    ![Azure Stack ortamı](media/azure-stack-solution-cloud-burst/image18.png)

14. Azure Stack uç noktası için aboneliği seçin.
    
    ![Azure Stack uç noktası için aboneliği seçin](media/azure-stack-solution-cloud-burst/image19.png)

15. Azure Stack Web uygulaması adını App Service adı olarak ayarlayın.

    ![Azure Stack Web uygulaması adını ayarla](media/azure-stack-solution-cloud-burst/image20.png)

16. Azure Stack aracısını seçin.
    
    ![Azure Stack aracısını seçin](media/azure-stack-solution-cloud-burst/image21.png)

17. Dağıtım Azure App Service bölümünde, ortam için geçerli **paketi veya klasörü** seçin. Klasör konumuna **Tamam ' ı** seçin.

    ![Azure App Service dağıtımı için klasör seçin](media/azure-stack-solution-cloud-burst/image22.png)

    ![Azure App Service dağıtımı için klasör seçin](media/azure-stack-solution-cloud-burst/image23.png)

18. Değişken sekmesi altında `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS` adlı bir değişken ekleyin, değerini **true**olarak ayarlayın ve kapsamını Azure Stack.
    
    ![Azure Uygulama dağıtımına değişken ekleme](media/azure-stack-solution-cloud-burst/image24.png)

19. Her iki yapıt içinde **sürekli** dağıtım tetikleme simgesini seçin ve **devam** eden dağıtım tetikleyicisini etkinleştirin.
    
    ![Sürekli dağıtım tetikleyicisi seçin](media/azure-stack-solution-cloud-burst/image25.png)

20. Azure Stack ortamında **dağıtım öncesi** koşullar simgesini seçin ve tetikleyiciyi **yayından sonra** olarak ayarlayın.
    
    ![Dağıtım öncesi koşulları seçin](media/azure-stack-solution-cloud-burst/image26.png)

21. Tüm değişiklikleri kaydedin.

> [!Note]  
> Görevler için bazı ayarlar, bir şablondan bir yayın tanımı oluşturulurken otomatik olarak [ortam değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) olarak tanımlanabilir. Bu ayarlar görev ayarlarında değiştirilemez; Bunun yerine, bu ayarları düzenlemek için üst ortam öğesinin seçilmesi gerekir.

## <a name="publish-to-azure-stack-via-visual-studio"></a>Visual Studio aracılığıyla Azure Stack yayımlayın

Bir Visual Studio Online (VSTO) derlemesi, uç noktalar oluşturarak Azure Stack için Azure hizmet uygulamalarını dağıtabilir. Azure Pipelines, Azure Stack bağlanan yapı aracısına bağlanır.

1.  VSTO 'da oturum açın ve uygulama ayarları sayfasına gidin.

2.  **Ayarlar**' da **güvenlik**' i seçin.

3.  **VSTS grupları**' nda **Endpoint Creators**' ı seçin.

4.  **Üyeler** sekmesinde **Ekle**' yi seçin.

5.  Kullanıcı **ve Grup Ekle**' de, bir Kullanıcı adı girin ve bu kullanıcıyı Kullanıcı listesinden seçin.

6.  **Değişiklikleri Kaydet**' i seçin.

7.  **VSTS grupları** listesinde **Endpoint Administrators**' ı seçin.

8.  **Üyeler** sekmesinde **Ekle**' yi seçin.

9.  Kullanıcı **ve Grup Ekle**' de, bir Kullanıcı adı girin ve bu kullanıcıyı Kullanıcı listesinden seçin.

10. **Değişiklikleri Kaydet**' i seçin.

Artık uç nokta bilgileri mevcut olduğuna göre, Azure Stack bağlantı Azure Pipelines kullanıma hazırdır. Azure Stack içindeki yapı Aracısı Azure Pipelines yönergeleri alır ve aracı Azure Stack ile iletişim için uç nokta bilgilerini alır.

## <a name="develop-the-application-build"></a>Uygulama derlemesini geliştirme

> [!Note]  
> Çalışmak için dağıtılmış uygun görüntülerle Azure Stack (Windows Server ve SQL) ve App Service dağıtımı gereklidir. Daha fazla bilgi için, [Azure Stack App Service kullanmaya başlamadan önce](../operator/azure-stack-app-service-before-you-get-started.md)App Service belgelerini gözden geçirin.

Her iki buluta dağıtmak için Azure Repos Web uygulaması kodu gibi [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) kullanın.

### <a name="add-code-to-an-azure-repos-project"></a>Azure Repos projesine kod ekleme

1.  Azure Stack üzerinde proje oluşturma haklarına sahip bir hesapla Azure Repos için oturum açın. Sonraki ekran yakalama, HybridCICD projesine bağlanmayı gösterir.

2.  Varsayılan Web uygulamasını oluşturarak ve açarak **depoyu kopyalayın** .

#### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Her iki bulutta da uygulama hizmetleri için kendi kendine içerilen Web uygulaması dağıtımı oluşturma

1.  **WebApplication. csproj** dosyasını düzenleyin: `Runtimeidentifier` ' i seçin ve sonra @no__t ekleyin. Daha fazla bilgi için, bkz. [kendi kendine kapsanan dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri.

2.  Kodu Azure Repos olarak denetlemek için Takım Gezgini kullanın.

3.  Uygulama kodunun Azure Repos işaretli olduğunu doğrulayın.

### <a name="create-the-build-definition"></a>Derleme tanımı oluşturma

1.  Derleme tanımı oluşturabileceğiniz bir hesapla Azure Pipelines için oturum açın.

2.  Projenin **Build Web Application** sayfasına gidin.

3.  **Bağımsız değişkenler**içinde **-r win10-x64** kodu ekleyin. Bu ek, .NET Core ile bağımsız bir dağıtımı tetiklemek için gereklidir.

4.  Derlemeyi çalıştırın. [Kendi içinde çalışan dağıtım oluşturma](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) Işlemi, Azure 'da çalışabilecek yapıtları yayımlayacak ve Azure Stack.

#### <a name="use-an-azure-hosted-build-agent"></a>Azure 'da barındırılan derleme Aracısı kullanma

Azure Pipelines içinde barındırılan bir yapı aracısının kullanılması, Web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Bakım ve yükseltmeler Microsoft Azure tarafından otomatik olarak gerçekleştirilir ve sürekli ve kesintisiz bir geliştirme döngüsünü etkinleştirir.

### <a name="configure-the-continuous-deployment-cd-process"></a>Sürekli dağıtım (CD) işlemini yapılandırma

Azure Pipelines ve Azure DevOps Server, yayınlar için geliştirme, hazırlama, kalite güvencesi (QA) ve üretim gibi birden çok ortama yönelik yüksek düzeyde yapılandırılabilir ve yönetilebilir bir işlem hattı sağlar. Bu işlem, uygulama yaşam döngüsünün belirli aşamalarındaki onayları gerektirmeyi içerebilir.

#### <a name="create-release-definition"></a>Yayın tanımı oluştur

Yayın tanımı oluşturmak, uygulama oluşturma işlemindeki son adımdır. Bu yayın tanımı bir yayın oluşturmak ve bir derlemeyi dağıtmak için kullanılır.

1.  Azure Pipelines oturum açın ve proje için **derleme ve yayın '** a gidin.

2.  **Yayınlar** sekmesinde **[+]** öğesini seçin ve ardından **yayın tanımı oluştur**' u seçin.

3.  **Şablon seçin**sayfasında, **Azure App Service dağıtım**' ı seçin ve ardından **Uygula**' yı seçin.

4.  **Yapıt Ekle**sayfasında, **kaynaktan (derleme tanımı)** Azure Cloud Build uygulamasını seçin.

5.  İşlem **hattı** sekmesinde, **ortam görevlerini görüntülemek**için **1 aşama**, **1 görev** bağlantısını seçin.

6.  **Görevler** sekmesinde, **ortam adı** olarak Azure yazın ve **Azure aboneliği** listesinden AZURECYÜKSEK Traders-Web EP ' yi seçin.

7.  Sonraki ekran yakalamadaki `northwindtraders` olan **Azure App Service adını**girin.

8.  Aracı aşaması için, **Aracı sırası** LISTESINDEN **barındırılan VS2017** öğesini seçin.

9.  **Azure App Service dağıt**' da, ortam Için geçerli **paketi veya klasörü** seçin.

10. **Dosya veya klasör seç**' de konuma **Tamam** 'ı seçin.

11. Tüm değişiklikleri kaydedin ve **ardışık düzene**geri dönün.

12. İşlem **hattı** sekmesinde **yapıt Ekle**' yi seçin ve **kaynak (derleme tanımı)** listesinden **northwindcloud Traders-vessel** ' i seçin.

13. **Şablon seçin**sayfasında başka bir ortam ekleyin. **Azure App Service dağıtımını** seçin ve ardından **Uygula**' yı seçin.

14. **Ortam adı**olarak `Azure Stack` girin.

15. **Görevler** sekmesinde Azure Stack bulun ve seçin.

16. **Azure aboneliği** listesinden Azure Stack uç noktası Için **Azurestack Traders-vessel EP** ' yi seçin.

17. **App Service adı**olarak Azure Stack Web uygulaması adını girin.

18. **Aracı seçimi**altında, **Aracı sırası** listesinden **Azurestack-b Douglas FI** ' yi seçin.

19. **Dağıtım Azure App Service**için, ortam Için geçerli **paketi veya klasörü** seçin. **Dosya veya klasör seçin**sayfasında, klasör **konumu**için **Tamam** ' ı seçin.

20. **Değişken** sekmesinde `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS` adlı değişkeni bulun. Değişken değerini **true**olarak ayarlayın ve kapsamını **Azure Stack**olarak ayarlayın.

21. İşlem **hattı** sekmesinde, Northwindcloud Traders-Web yapıtı için **sürekli dağıtım tetikleme** simgesini seçin ve **sürekli dağıtım tetikleyicisini** **etkin**olarak ayarlayın. **Northwindcloud Traders-vessel** yapıtı için aynı şeyi yapın.

22. Azure Stack ortamı için **dağıtım öncesi koşullar** simgesini, **yayından sonra**olarak ayarla ' yı seçin.

23. Tüm değişiklikleri kaydedin.

> [!Note]  
> Yayın görevlerine yönelik bazı ayarlar, bir şablondan bir yayın tanımı oluşturulurken otomatik olarak [ortam değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) olarak tanımlanır. Bu ayarlar görev ayarlarından değiştirilemez, ancak üst ortam öğelerinde değiştirilebilir.

## <a name="create-a-release"></a>Yayın oluştur

1.  İşlem **hattı** sekmesinde, **yayın** listesini açın ve **yayın oluştur**' u seçin.

2.  Yayın için bir açıklama girin, doğru yapıtların seçili olup olmadığını denetleyin ve ardından **Oluştur**' u seçin. Birkaç dakika sonra, yeni yayının oluşturulduğunu ve yayın adının bir bağlantı olarak görüntülendiğini gösteren bir başlık görünür. Yayın Özeti sayfasını görmek için bağlantıyı seçin.

3.  Yayın Özeti sayfası, sürüm hakkındaki ayrıntıları gösterir. "Release-2" için aşağıdaki ekran yakalamada **ortamlar** bölümü, Azure 'un **DAĞıTıM durumunu** "sürüyor" olarak gösterir ve Azure Stack durum "başarılı" olur. Azure ortamının dağıtım durumu "başarılı" olarak değiştiğinde, yayının onaya hazırlandığını gösteren bir başlık görüntülenir. Bir dağıtım beklendiğinde veya başarısız olduysa, mavi **(i)** bilgi simgesi gösterilir. Gecikme veya başarısızlık nedenini içeren bir açılır pencere görmek için simgenin üzerine gelin.

4.  Yayın listesi gibi diğer görünümler de onay beklendiğini belirten bir simge görüntüler. Bu simgenin açılır penceresi, ortam adı ve dağıtımla ilgili daha fazla ayrıntı gösterir. Yöneticiler, yayınların genel ilerleme durumunu görmek ve hangi sürümlerin onay beklediğini görmek için çok kolay.

## <a name="monitor-and-track-deployments"></a>Dağıtımları izleme ve izleme

1.  2\. **Sürüm** Özeti sayfasında **Günlükler**' i seçin. Dağıtım sırasında bu sayfada, aracıdan canlı günlük görüntülenir. Sol bölmede her bir ortam için dağıtımdaki her bir işlemin durumu gösterilir.

2.  Dağıtım öncesi veya dağıtım sonrası onayı için **eylem** sütununda kişi simgesini seçerek dağıtımı kimin onayladığını (veya reddettiğini) ve sağladıkları iletiyi görün.

3.  Dağıtım bittikten sonra, tüm günlük dosyası sağ bölmede görüntülenir. **Başlat işi**gibi tek bir adım için günlük dosyasını görmek üzere sol bölmedeki herhangi bir **adımı** seçin. Tek tek günlükleri görme özelliği, genel dağıtımın parçalarını izlemeyi ve hata ayıklamayı kolaylaştırır. Bir adım için günlük dosyasını **kaydedin** veya **tüm günlükleri zip olarak indirin**.

4.  Sürümle ilgili genel bilgileri görmek için **Özet** sekmesini açın. Bu görünüm, derleme hakkındaki ayrıntıları, dağıtıldığı ortamları, dağıtım durumunu ve sürümle ilgili diğer bilgileri gösterir.

5.  Belirli bir ortama yönelik mevcut ve bekleyen dağıtımlar hakkındaki bilgileri görmek için bir ortam bağlantısı (**Azure** veya **Azure Stack**) seçin. Aynı yapılandırmanın her iki ortama de dağıtıldığını doğrulamak için bu görünümleri hızlı bir şekilde kullanın.

6.  **Dağıtılan üretim uygulamasını** bir tarayıcıda açın. Örneğin, Azure App Services Web sitesi için URL 'YI `https://[your-app-name\].azurewebsites.net` ' ı açın.

**Azure ve Azure Stack tümleştirmesi, ölçeklenebilir bir çapraz bulut çözümü sağlar**

Esnek ve sağlam bir çoklu bulut hizmeti, veri güvenliği, yedekleme ve artıklık, tutarlı ve hızlı kullanılabilirlik, ölçeklenebilir depolama ve dağıtım ve coğrafi olarak uyumlu yönlendirme sağlar. El ile tetiklenen bu işlem, barındırılan Web uygulamaları ve önemli verilerin hemen kullanılabilirliği arasında güvenilir ve verimli yük geçiş sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).
