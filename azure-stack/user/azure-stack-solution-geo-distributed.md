---
title: Azure ve Azure Stack coğrafi olarak dağıtılmış bir uygulama çözümü ile trafiği doğrudan yönlendirme | Microsoft Docs
description: Azure ve Azure Stack coğrafi olarak dağıtılmış bir uygulama çözümünü, trafiği belirli uç noktalara yönlendiren bir şekilde nasıl oluşturacağınızı öğrenin.
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
ms.openlocfilehash: bac13382c1fe579a04b96274d8f310d5a1737be2
ms.sourcegitcommit: a0dcb61890ad0f7b8e1f738f7186198681adcc2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820785"
---
# <a name="create-a-geo-distributed-app-solution-to-direct-traffic-with-azure-and-azure-stack"></a>Trafiği Azure ile yönlendirmek için coğrafi olarak dağıtılmış bir uygulama çözümü oluşturma ve Azure Stack

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Coğrafi olarak dağıtılmış uygulamalar modelini kullanarak çeşitli ölçümleri temel alarak trafiğin belirli uç noktalara nasıl yönlendirileceğini öğrenin. Coğrafi tabanlı Yönlendirme ve uç nokta yapılandırmasıyla Traffic Manager profili oluşturma, bilgilerin bölgesel gereksinimlere, şirkete ve uluslararası yönetmelere ve veri gereksinimlerinize göre uç noktalara yönlendirilmesini sağlar.

Bu çözümde, aşağıdakileri yapmak için bir örnek ortam oluşturacaksınız:

> [!div class="checklist"]
> - Coğrafi olarak dağıtılmış bir uygulama oluşturun.
> - Uygulamanızı hedeflemek için Traffic Manager kullanın.

## <a name="use-the-geo-distributed-apps-pattern"></a>Coğrafi olarak dağıtılmış uygulamalar modelini kullanma

Coğrafi olarak dağıtılmış düzende, uygulamanız bölgeleri kapsar. Genel bulutu varsayılan olarak kullanabilirsiniz, ancak bazı kullanıcılarınız, verilerinin bölgesinde kalmasını gerektirebilir. Kullanıcıları gereksinimlerine göre en uygun buluta yönlendirebilirsiniz.

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

#### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Bu makalede oluşturacağınız çözüm, ölçeklenebilirlik sağlamak için değildir. Ancak, diğer Azure ve şirket içi çözümlerle birlikte kullanılırsa ölçeklenebilirlik gereksinimlerine uyum sağlayabilirsiniz. Traffic Manager aracılığıyla otomatik ölçeklendirmeyle karma çözüm oluşturma hakkında bilgi için bkz. [Azure ile platformlar arası ölçeklendirme çözümleri oluşturma](azure-stack-solution-cloud-burst.md).

#### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Ölçeklenebilirlik konusunda olduğu gibi bu çözüm, kullanılabilirliği doğrudan ele almaz. Ancak, Azure ve şirket içi çözümler bu çözüm içinde, dahil edilen tüm bileşenler için yüksek kullanılabilirlik sağlamak üzere uygulanabilir.

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

- Kuruluşunuzun özel bölgesel güvenlik ve dağıtım ilkeleri gerektiren Uluslararası dalları vardır.

- Kuruluşunuzun ofislerinin her biri, her yerel yönetmeliğe ve saat dilimlerine göre raporlama etkinliği gerektiren çalışan, iş ve tesis verilerini çeker.

- Yüksek ölçekli gereksinimler, tek bir bölgede birden çok uygulama dağıtımına sahip uygulamaları yatay olarak ölçeklendirerek ve çok fazla yük gereksinimini işlemek için bölgeler arasında karşılanır.

### <a name="planning-the-topology"></a>Topolojiyi planlama

Dağıtılmış bir uygulama ayak izi oluşturmadan önce, aşağıdaki şeyleri öğrenmenize yardımcı olur:

-   **Uygulama için özel etki alanı:** Müşterilerin uygulamaya erişmek için kullanacağı özel etki alanı adı nedir? Örnek uygulama için, özel etki alanı adı *www\.scalableasedemo.com* ' dir.

-   **Traffic Manager etki alanı:** Bir [Azure Traffic Manager profili](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)oluştururken bir etki alanı adı seçilir. Bu ad, Traffic Manager tarafından yönetilen bir etki alanı girdisini kaydetmek için *trafficmanager.net* sonekiyle birleştirilir. Örnek uygulama için, seçilen ad *ölçeklenebilir-Ao-demo*' dir. Sonuç olarak, Traffic Manager tarafından yönetilen tam etki alanı adı *Scalable-ASE-demo.trafficmanager.net*' dir.

-   **Uygulama parmak izini ölçeklendirmeye yönelik strateji:** Uygulama parmak izin tek bir bölgede, birden çok bölgede veya her iki yaklaşımın bir karışımında birden çok App Service ortamına dağıtılıp dağıtılmayacağına karar verin. Karar, müşteri trafiğinin nereden kaynaklanacaktır ve bir uygulamanın destekleme arka uç altyapısının ne kadar iyi ölçeklendirilebileceğine ilişkin beklentileri temel almalıdır. Örneğin,% 100 durum bilgisi içermeyen bir uygulamayla, Azure bölgesi başına birden çok App Service ortamının bir birleşimi kullanılarak, birden fazla Azure bölgesinde dağıtılan App Service ortamları ile çarpılarak bir uygulama daha büyük bir şekilde ölçeklendirilebilir. Üzerinde seçim yapabileceğiniz 15 + küresel Azure bölgesi sayesinde müşteriler gerçek anlamda dünya genelinde bir hiper ölçekli uygulama ayak izi oluşturabilir. Burada kullanılan örnek uygulama için, tek bir Azure bölgesinde üç App Service ortamı oluşturulmuştur (Orta Güney ABD).

-   **App Service ortamları için adlandırma kuralı:** Her App Service Ortamı benzersiz bir ad gerektirir. Bir veya iki App Service ortamının ötesinde, her bir App Service Ortamı tanımlanmasına yardımcı olmak için bir adlandırma kuralı olması yararlı olacaktır. Burada kullanılan örnek uygulama için basit bir adlandırma kuralı kullanılmıştır. Üç App Service ortamının adları *fe1ase*, *fe2ase*ve *fe3ase*.

-   **Uygulamalar için adlandırma kuralı:** Uygulamanın birden çok örneği dağıtılırsa, dağıtılan uygulamanın her örneği için bir ad gereklidir. App Service ortamlarıyla aynı uygulama adı birden çok ortamda kullanılabilir. Her App Service Ortamı benzersiz bir etki alanı sonekine sahip olduğundan, geliştiriciler her ortamda tam olarak aynı uygulama adını kullanmayı seçebilir. Örneğin, bir geliştirici şu şekilde adlandırılan uygulamalara sahip olabilir: *MyApp.foo1.p.azurewebsites.net*, *MyApp.Foo2.p.azurewebsites.net*, *MyApp.Foo3.p.azurewebsites.net*, vb. Burada kullanılan uygulama için, her bir uygulama örneğinin benzersiz bir adı vardır. Kullanılan uygulama örneği adları *webfrontend1*, *webfrontend2*ve *webfrontend3*.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

## <a name="part-1-create-a-geo-distributed-app"></a>1\. Bölüm: Coğrafi olarak dağıtılmış bir uygulama oluşturma

Bu bölümde, bir Web uygulaması oluşturacaksınız.

> [!div class="checklist"]
> - Web uygulamaları oluşturun ve yayımlayın.
> - Azure Repos kod ekleyin.
> - Uygulamanın yapısını birden çok bulut hedeflerine işaret edin.
> - CD işlemini yönetin ve yapılandırın.

### <a name="prerequisites"></a>Önkoşullar

Bir Azure aboneliği ve Azure Stack yüklemesi gereklidir.

### <a name="geo-distributed-app-steps"></a>Coğrafi olarak dağıtılmış uygulama adımları

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Özel etki alanı edinme ve DNS 'yi yapılandırma

Etki alanı için DNS bölge dosyasını güncelleştirin. Daha sonra Azure AD, özel etki alanı adının sahipliğini doğrulayabilirler. Azure 'da Azure/Office 365/dış DNS kayıtları için [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) kullanın veya DNS girişini [farklı bir DNS kaydedicisinde](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)ekleyin.

1. Özel bir etki alanını ortak bir kayıt defteri ile kaydedin.

2. Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. DNS güncelleştirmelerini yapmak için onaylanan yönetici gerekli olabilir.

3. Azure AD tarafından belirtilen DNS girişini ekleyerek etki alanı için DNS bölge dosyasını güncelleştirin. DNS girişi, posta yönlendirme veya Web barındırma gibi davranışları değiştirmez.

### <a name="create-web-apps-and-publish"></a>Web uygulamaları oluşturma ve yayımlama

Web uygulamasını Azure 'a dağıtmak ve Azure Stack için karma sürekli tümleştirme/sürekli teslim (CI/CD) ayarlayın ve değişiklikleri her iki bulutta da otomatik olarak gönderin.

> [!Note]  
> Çalışmak için dağıtılmış uygun görüntülerle Azure Stack (Windows Server ve SQL) ve App Service dağıtımı gereklidir. Daha fazla bilgi için, Azure Stack operatör belgelerindeki [Azure Stack App Service kullanmaya başlamadan önce](../operator/azure-stack-app-service-before-you-get-started.md) bölümüne bakın.

#### <a name="add-code-to-azure-repos"></a>Azure Repos kod ekleme

1. Azure Repos üzerinde **Proje oluşturma hakları** olan bir hesapla Visual Studio 'da oturum açın.

    CI/CD, hem uygulama kodu hem de altyapı kodu için uygulanabilir. Hem özel hem de barındırılan bulut geliştirmesi için [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) kullanın.

    ![Visual Studio 'da bir projeye bağlanma](media/azure-stack-solution-geo-distributed/image1.JPG)

2. Varsayılan Web uygulamasını oluşturarak ve açarak **depoyu kopyalayın** .

    ![Visual Studio 'da depoyu Kopyala](media/azure-stack-solution-geo-distributed/image2.png)

### <a name="create-web-app-deployment-in-both-clouds"></a>Her iki bulutta Web uygulaması dağıtımı oluşturma

1.  **WebApplication. csproj** dosyasını düzenleyin: Seçin `Runtimeidentifier` ve ekleyin `win10-x64`. (Bkz. [kendi Içinde dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgeleri.)

    ![Visual Studio 'da Web uygulaması proje dosyasını düzenleme](media/azure-stack-solution-geo-distributed/image3.png)

1.  Takım Gezgini kullanarak **Azure Repos kodu Iade edin** .

2.  **Uygulama kodunun** Azure Repos işaretli olduğunu doğrulayın.

### <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1. Derleme tanımları oluşturma yeteneğini onaylamak için **Azure pipelines ' de oturum açın** .

2. Kod `-r win10-x64` ekleyin. Bu ek, .NET Core ile bağımsız bir dağıtımı tetiklemek için gereklidir.

    ![Derleme tanımına kod ekleme](media/azure-stack-solution-geo-distributed/image4.png)

3. **Derlemeyi çalıştırın**. [Kendi içinde çalışan dağıtım oluşturma](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) Işlemi, Azure 'da çalışabilecek yapıtları yayımlayacak ve Azure Stack.

**Azure barındırılan Aracısı kullanma**

Azure Pipelines barındırılan bir aracının kullanılması, Web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Bakım ve yükseltmeler, kesintisiz geliştirme, test ve dağıtım sağlayan Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="manage-and-configure-the-cd-process"></a>CD işlemini yönetme ve yapılandırma

Azure DevOps ve Azure DevOps Server, yayınlar için geliştirme, hazırlık, QA ve üretim ortamları gibi birden çok ortama yönelik yüksek düzeyde yapılandırılabilir ve yönetilebilir bir işlem hattı sağlar; belirli aşamalarda onay gerektirme de dahil.

## <a name="create-release-definition"></a>Yayın tanımı oluştur

1.  VSO 'ın **derleme ve yayınlama** bölümündeki **yayınlar** sekmesinin altına yeni bir yayın eklemek için **artı** düğmesini seçin.

    ![Yayın tanımı oluşturma](media/azure-stack-solution-geo-distributed/image5.png)

2. Azure App Service Dağıtım şablonunu uygulayın.

   ![Azure App Service dağıtım şablonu Uygula](meDia/azure-stack-solution-geo-distributed/image6.png)

3. **Yapıt Ekle**' nin altında, Azure Cloud Build uygulaması için yapıt ekleyin.

   ![Azure Cloud Build 'e yapıt ekleme](media/azure-stack-solution-geo-distributed/image7.png)

4. İşlem hattı sekmesi altında, ortamın **aşamasını, görev** bağlantısını seçin ve Azure bulut ortamı değerlerini ayarlayın.

   ![Azure bulut ortamı değerlerini ayarlama](media/azure-stack-solution-geo-distributed/image8.png)

5. **Ortam adını** ayarlayın ve Azure bulut uç noktası için **Azure aboneliğini** seçin.

      ![Azure bulut uç noktası için Azure aboneliğini seçin](media/azure-stack-solution-geo-distributed/image9.png)

6. **App Service adı**altında, gerekli Azure App Service adını ayarlayın.

      ![Azure App Service adını ayarla](media/azure-stack-solution-geo-distributed/image10.png)

7. Azure bulut barındırılan ortamı için **Aracı kuyruğu** altında "barındırılan VS2017" yazın.

      ![Azure bulut barındırılan ortamı için aracı kuyruğunu ayarla](media/azure-stack-solution-geo-distributed/image11.png)

8. Azure App Service dağıt menüsünde, ortam için geçerli **paketi veya klasörü** seçin. **Klasör konumuna** **Tamam ' ı** seçin.
  
      ![Azure App Service ortamı için paket veya klasör seçin](media/azure-stack-solution-geo-distributed/image12.png)

      ![Azure App Service ortamı için paket veya klasör seçin](media/azure-stack-solution-geo-distributed/image13.png)

9. Tüm değişiklikleri kaydedin ve **yayın ardışık düzenine**geri dönün.

    ![Değişiklikleri yayın ardışık düzeninde Kaydet](media/azure-stack-solution-geo-distributed/image14.png)

10. Azure Stack uygulaması için yapıyı seçerek yeni bir yapıt ekleyin.
    
    ![Azure Stack uygulaması için yeni yapıt ekleme](media/azure-stack-solution-geo-distributed/image15.png)


11. Azure App Service dağıtımını uygulayarak bir ortam daha ekleyin.
    
    ![Azure App Service dağıtımına ortam ekleme](media/azure-stack-solution-geo-distributed/image16.png)

12. Yeni ortam Azure Stack adlandırın.
    
    ![Azure App Service dağıtımında ad ortamı](media/azure-stack-solution-geo-distributed/image17.png)

13. **Görev** sekmesinde Azure Stack ortamını bulun.
    
    ![Azure Stack ortamı](media/azure-stack-solution-geo-distributed/image18.png)

14. Azure Stack uç noktası için aboneliği seçin.
    
    ![Azure Stack uç noktası için aboneliği seçin](media/azure-stack-solution-geo-distributed/image19.png)

15. Azure Stack Web uygulaması adını App Service adı olarak ayarlayın.

    ![Azure Stack Web uygulaması adını ayarla](media/azure-stack-solution-geo-distributed/image20.png)

16. Azure Stack aracısını seçin.
    
    ![Azure Stack aracısını seçin](media/azure-stack-solution-geo-distributed/image21.png)

17. Dağıtım Azure App Service bölümünde, ortam için geçerli **paketi veya klasörü** seçin. Klasör konumuna **Tamam ' ı** seçin.

    ![Azure App Service dağıtımı için klasör seçin](media/azure-stack-solution-geo-distributed/image22.png)

    ![Azure App Service dağıtımı için klasör seçin](media/azure-stack-solution-geo-distributed/image23.png)

18. Değişken sekmesi altında adlı `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`bir değişken ekleyin, değerini **doğru**olarak ayarlayın ve Azure Stack.
    
    ![Azure Uygulama dağıtımına değişken ekleme](media/azure-stack-solution-geo-distributed/image24.png)

19. Her iki yapıt içinde **sürekli** dağıtım tetikleme simgesini seçin ve **devam** eden dağıtım tetikleyicisini etkinleştirin.
    
    ![Sürekli dağıtım tetikleyicisi seçin](media/azure-stack-solution-geo-distributed/image25.png)

20. Azure Stack ortamında **dağıtım öncesi** koşullar simgesini seçin ve tetikleyiciyi **yayından sonra** olarak ayarlayın.
    
    ![Dağıtım öncesi koşulları seçin](media/azure-stack-solution-geo-distributed/image26.png)

21. Tüm değişiklikleri kaydedin.

> [!Note]  
> Görevler için bazı ayarlar, bir şablondan bir yayın tanımı oluşturulurken otomatik olarak [ortam değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) olarak tanımlanabilir. Bu ayarlar görev ayarlarında değiştirilemez; Bunun yerine, bu ayarları düzenlemek için üst ortam öğesinin seçilmesi gerekir.

## <a name="part-2-update-web-app-options"></a>2\. Bölüm: Web uygulaması seçeneklerini Güncelleştir

[Azure App Service](https://docs.microsoft.com/azure/app-service/overview), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. 

![Azure uygulama hizmeti](media/azure-stack-solution-geo-distributed/image27.png)

> [!div class="checklist"]
> - Mevcut bir özel DNS adını Azure Web Apps eşleştirin.
> - Özel bir DNS adını App Service eşlemek için bir **CNAME kaydı** ve **a kaydı** kullanın.

### <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Mevcut bir özel DNS adını Azure Web Apps'e eşleme

> [!Note]  
>  Kök etki alanı dışındaki tüm özel DNS adları için CNAME kullanın (örneğin, northwind.com).

Canlı siteyi ve onun DNS etki alanı adını App Service'e geçirmek için, bkz. [Etkin DNS adını Azure App Service'e geçirme](https://docs.microsoft.com/azure/app-service/manage-custom-dns-migrate-domain).

### <a name="prerequisites"></a>Önkoşullar

Bu çözümü gerçekleştirmek için:

-   [App Service bir uygulama oluşturun](https://docs.microsoft.com/azure/app-service/)veya başka bir çözüm için oluşturulmuş bir uygulama kullanın.

-   Etki alanı adı satın alıp etki alanı sağlayıcısı için DNS kayıt defterine erişim sağlayın.

Etki alanı için DNS bölge dosyasını güncelleştirin. Azure AD, özel etki alanı adının sahipliğini doğrulayacaktır. Azure 'da Azure/Office 365/dış DNS kayıtları için [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) kullanın veya DNS girişini [farklı bir DNS kaydedicisinde](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)ekleyin.

-   Özel bir etki alanını ortak bir kayıt defteri ile kaydedin.

-   Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. (DNS güncelleştirmeleri yapmak için onaylanan yönetici gerekli olabilir.)

-   Azure AD tarafından belirtilen DNS girişini ekleyerek etki alanı için DNS bölge dosyasını güncelleştirin.

Örneğin, northwindcloud.com ve www\.northwindcloud.com için DNS girişleri eklemek için, northwindcloud.com kök etki alanı için DNS ayarlarını yapılandırın.

> [!Note]  
>  [Azure Portal](https://docs.microsoft.com/azure/app-service/manage-custom-dns-buy-domain)kullanarak bir etki alanı adı satın alınabilir. Özel DNS adını web uygulamasına eşlemek için, web uygulamasının [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) ücretli bir katmanda (**Paylaşılan**, **Temel**, **Standart** veya **Premium** olmalıdır).



### <a name="create-and-map-cname-and-a-records"></a>CNAME ve A kayıtlarını oluşturma ve eşleme

#### <a name="access-dns-records-with-domain-provider"></a>Etki alanı sağlayıcısı ile DNS kayıtlarına erişme

> [!Note]  
>  Azure Web Apps için özel bir DNS adı yapılandırmak üzere Azure DNS kullanın. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](https://docs.microsoft.com/azure/dns/dns-custom-domain).

1.  Ana sağlayıcının web sitesinde oturum açın.

2.  DNS kayıtlarını yönetme sayfasını bulun. Her etki alanı sağlayıcısının kendi DNS kayıtları arabirimi vardır. Sitede **Domain Name**, **DNS** veya **Name Server Management** etiketli alanları bulun.

DNS kayıtları sayfası, **etki Alanlarmda**görüntülenebilir. **Bölge dosyası**, **DNS kayıtları**veya **Gelişmiş yapılandırma**adlı bağlantıyı bulun.

DNS kayıtları sayfasının bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz:

![Örnek DNS kayıtları sayfası](media/azure-stack-solution-geo-distributed/image28.png)

1. Kayıt oluşturmak için etki alanı adı kaydedicisi ' nde **Ekle veya oluştur** ' u seçin. Bazı sağlayıcıların farklı kayıt türlerini eklemek için farklı bağlantıları vardır. Sağlayıcının belgelerine başvurun.

2. Bir alt etki alanını uygulamanın varsayılan ana bilgisayar adına eşlemek için bir CNAME kaydı ekleyin.

   Www\.northwindcloud.com etki alanı örneği için adı < App\_name >. azurewebsites. net olarak eşleyen bir CNAME kaydı ekleyin.

CNAME eklendikten sonra DNS kayıtları sayfası aşağıdaki örneğe benzer şekilde görünür:

![Azure uygulamasına portal gezintisi](media/azure-stack-solution-geo-distributed/image29.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>Azure'da CNAME kaydı eşlemesini etkinleştirme

1. Yeni bir sekmede Azure portal oturum açın,

2. Uygulama Hizmetleri ' ne gidin.

3. Web uygulaması ' nı seçin.

4. Azure Portal'daki uygulama sayfasının sol gezintisinde **Özel etki alanları**'nı seçin.

5. **Konak adı ekle**'nin yanındaki **+** simgesini seçin.

6. Tam etki alanı adını `www.northwindcloud.com`(gibi) yazın.

7. **Doğrula**'yı seçin.

8. Belirtilmişse, etki alanı adı kayıt şirketlerinde DNS kayıtlarına diğer türlerin`A` ( `TXT`veya) ek kayıtlarını ekleyin. Azure, bu kayıtların değerlerini ve türlerini sağlar:

   a.  Uygulamanın IP adresini eşlemek için bir **A** kaydı.

   b.  Uygulamanın varsayılan ana bilgisayar adına eşlenecek bir **txt** kaydı < app_name >. azurewebsites. net. App Service, özel etki alanı sahipliğini doğrulamak için bu kaydı yalnızca yapılandırma zamanında kullanır. Doğrulamadan sonra TXT kaydını silin.

9. Bu görevi etki alanı kaydedici sekmesinde doldurun ve **ana bilgisayar adı Ekle** düğmesi etkinleştirilinceye kadar yeniden doğrulayın.

10. **Ana bilgisayar adı kayıt türünün** **CNAME** (www.example.com veya herhangi bir alt etki alanı) olarak ayarlandığından emin olun.

11. **Konak adı ekle**'yi seçin.

12. Tam etki alanı adını `northwindcloud.com`(gibi) yazın.

13. **Doğrula**'yı seçin. **Ekleme** etkinleştirilir.

14. **Ana bilgisayar adı kayıt türünün** **bir kayıt** (example.com) olarak ayarlandığından emin olun.

15. **Konak adı Ekle**.

    Yeni ana bilgisayar adlarının uygulamanın **özel etki alanları** sayfasında yansıtılması biraz zaman alabilir. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.
  
    ![Özel etki alanları](media/azure-stack-solution-geo-distributed/image31.png) 
  
    Bir hata oluşursa, sayfanın alt kısmında bir doğrulama hatası bildirimi görüntülenir. ![Doğrulama hatası](media/azure-stack-solution-geo-distributed/image32.png)

> [!Note]  
>  Yukarıdaki adımlar, joker bir etki alanını (\*. northwindcloud.com) eşlemek için yinelenebilir. Bu, her biri için ayrı bir CNAME kaydı oluşturmak zorunda kalmadan bu App Service 'e ek alt etki alanları eklenmesine izin verir. Bu ayarı yapılandırmak için kaydedici yönergelerini izleyin.

#### <a name="test-in-a-browser"></a>Bir tarayıcıda test etme

Daha önce yapılandırılan DNS adına (örneğin, `northwindcloud.com` veya www.northwindcloud.com) gidin.

## <a name="part-3-bind-a-custom-ssl-cert"></a>Bölüm 3: Özel bir SSL sertifikası bağlama

Bu bölümde şunları göndereceğiz:

> [!div class="checklist"]
> - Özel SSL sertifikasını App Service bağlayın.
> - Uygulama için HTTPS 'yi zorunlu tutun.
> - SSL sertifikası bağlamasını betiklerle otomatikleştirin.

> [!Note]  
> Gerekirse, Azure portal bir müşteri SSL sertifikası alın ve Web uygulamasına bağlayın. Daha fazla bilgi için [App Service sertifikaları öğreticisine](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site)bakın.

### <a name="prerequisites"></a>Önkoşullar

Bu çözümü gerçekleştirmek için:

-   [App Service uygulaması oluşturun.](https://docs.microsoft.com/azure/app-service/)
-   [Özel bir DNS adını Web uygulamanıza eşleyin.](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)
-   Güvenilir bir sertifika yetkilisinden SSL sertifikası alın ve bu anahtarı kullanarak isteği imzalayın.

### <a name="requirements-for-your-ssl-certificate"></a>SSL sertifikanıza yönelik gereksinimler

Bir sertifikayı App Service’te kullanabilmek için sertifikanın aşağıdaki tüm gereksinimleri karşılaması gerekir:

-   Güvenilen bir sertifika yetkilisi tarafından imzalandı.

-   Parola korumalı bir PFX dosyası olarak verildi.

-   En az 2048 bit uzunluğunda özel anahtar içerir.

-   Sertifika zincirindeki tüm ara sertifikaları içerir.

> [!Note]  
>  **Eliptik Eğri Şifreleme (ECC) sertifikaları** App Service ile çalışır, ancak bu kılavuza dahil değildir. ECC sertifikaları oluşturma konusunda yardım almak için bir sertifika yetkilisine başvurun. 

#### <a name="prepare-the-web-app"></a>Web uygulamasını hazırlama

Özel bir SSL sertifikasını Web uygulamasına bağlamak için [App Service planının](https://azure.microsoft.com/pricing/details/app-service/) **temel**, **Standart**veya **Premium** katmanda olması gerekir.

#### <a name="sign-in-to-azure"></a>Azure'da oturum açma

1.  [Azure Portal](https://portal.azure.com/) açın ve Web uygulamasına gidin.

2.  Sol menüden **uygulama hizmetleri**' ni ve ardından Web uygulaması adını seçin.

![Web uygulaması seçme](media/azure-stack-solution-geo-distributed/image33.png)

#### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

1.  Web uygulaması sayfasının sol tarafında, **Ayarlar** bölümüne gidin ve **ölçeği büyütme (App Service planı)** seçeneğini belirleyin.

    ![Ölçeği artır menüsü](media/azure-stack-solution-geo-distributed/image34.png)

1.  Web uygulamasının **ücretsiz** veya **paylaşılan** katmanda olmadığından emin olun. Web uygulamasının geçerli katmanı, koyu mavi bir kutu içinde vurgulanır.

    ![Fiyatlandırma katmanını denetleyin](media/azure-stack-solution-geo-distributed/image35.png)

Özel SSL, **ücretsiz** veya **paylaşılan** katmanda desteklenmez. Üst ölçekte, sonraki bölümde bulunan adımları izleyin veya **fiyatlandırma katmanınızı seçin** SAYFASıNDA, [SSL sertifikanızı karşıya yüklemek ve bağlamak](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)için atlayın.

#### <a name="scale-up-your-app-service-plan"></a>App Service planınızın ölçeğini artırma

1.  **Temel**, **Standart** veya **Premium** katmanlarından birini seçin.

2.  Seçin **seçin**.

![Fiyatlandırma katmanı seçme](media/azure-stack-solution-geo-distributed/image36.png)

Bildirim görüntülenirken, ölçek işlemi tamamlanır.

![Ölçek artırma bildirimi](media/azure-stack-solution-geo-distributed/image37.png)

#### <a name="bind-your-ssl-certificate-and-merge-intermediate-certificates"></a>SSL sertifikanızı bağlama ve ara sertifikaları birleştirme

Zincirdeki birden çok sertifikayı birleştirin.

1. Bir metin düzenleyicisinde aldığınız **her sertifikayı açın** .

2. Birleştirilmiş sertifika için *mergedcertificate.crt* adlı bir dosya oluşturun. Bir metin düzenleyicisinde her bir sertifikanın içeriğini bu dosyaya kopyalayın. Sertifikalarınızın sırası, sertifikanızla başlayıp kök sertifika ile sona ererek sertifika zincirindeki sırayla aynı olmalıdır. Aşağıdaki örneğe benzer şekilde görünür:

    ```Text

    -----BEGIN CERTIFICATE-----

    <your entire Base64 encoded SSL certificate>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded intermediate certificate 1>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded intermediate certificate 2>

    -----END CERTIFICATE-----

    -----BEGIN CERTIFICATE-----

    <The entire Base64 encoded root certificate>

    -----END CERTIFICATE-----
    ```

#### <a name="export-certificate-to-pfx"></a>Sertifikayı PFX dosyasına aktarma

Birleştirilmiş SSL sertifikasını sertifika tarafından oluşturulan özel anahtarla dışarı aktarın.

OpenSSL aracılığıyla bir özel anahtar dosyası oluşturulur. Sertifikayı PFX 'e aktarmak için aşağıdaki komutu çalıştırın ve  *\<özel anahtar dosyası >* ve  *\<birleştirilmiş sertifika dosyası >* yer tutucuları özel anahtar yolu ve birleştirilmiş sertifika dosyası ile değiştirin:

```powershell
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>
```

İstendiğinde, daha sonra App Service SSL sertifikanızı karşıya yüklemek için bir dışarı aktarma parolası tanımlayın.

Sertifika isteği oluşturmak için IIS veya **CertReq. exe** kullanıldığında, sertifikayı yerel makineye yükler ve ardından [sertifikayı PFX 'e dışarı aktarın](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

#### <a name="upload-the-ssl-certificate"></a>SSL sertifikasını karşıya yükleme

1. Web uygulamasının sol gezinti bölmesinde **SSL ayarları** ' nı seçin.

2. **Sertifikayı karşıya yükle**' yi seçin.

3. **PFX Sertifika dosyasında**pfx dosyası ' nı seçin.

4. **Sertifika parolası**' nda, pfx dosyası dışarı aktarılırken oluşturulan parolayı yazın.

5. **Karşıya Yükle**’yi seçin.

![Karşıya sertifika yükleme](media/azure-stack-solution-geo-distributed/image38.png)

App Service sertifikayı karşıya yüklemeyi bitirdiğinde, bu, **SSL ayarları** sayfasında görüntülenir.

![SSL ayarları](media/azure-stack-solution-geo-distributed/image39.png)

#### <a name="bind-your-ssl-certificate"></a>SSL sertifikanızı bağlama

1.  **SSL bağlamaları** bölümünde **bağlama Ekle**' yi seçin.

    > [!Note]  
    >  Sertifika karşıya yüklenmişse, ancak **ana bilgisayar** adı açılır listesinde etki alanı adında görünmüyorsa, tarayıcı sayfasını yenilemeyi deneyin.

1.  **SSL bağlaması Ekle** sayfasında, güvenli hale getirmek için etki alanı adını ve kullanılacak sertifikayı seçmek için açılan listeleri kullanın.

1.  **SSL Türü** menüsünde [**Sunucu Adı Belirtme (SNI)** ](https://en.wikipedia.org/wiki/Server_Name_Indication) veya IP tabanlı SSL seçeneklerinden hangisini kullanacağınızı belirleyin.

    - **SNI tabanlı SSL**: Birden çok SNı tabanlı SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden fazla SSL sertifikası ile birden fazla etki alanının güvenliğini sağlamaya olanak tanır. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI’yi destekler (daha kapsamlı tarayıcı desteği bilgilerini [Sunucu Adı Belirtimi](https://wikipedia.org/wiki/Server_Name_Indication) bölümünde bulabilirsiniz).

    - **IP tabanlı SSL**: Yalnızca tek bir IP tabanlı SSL bağlaması eklenebilir. Bu seçenek yalnızca bir SSL sertifikası ile ayrılmış bir genel IP adresinin güvenliğini sağlamaya olanak tanır. Birden çok etki alanının güvenliğini sağlamak için, bunların tümünü aynı SSL sertifikası ile güvenli hale getirin. IP tabanlı SSL, SSL bağlama için geleneksel bir seçenektir.

1. **Bağlama Ekle**' yi seçin.

    ![SSL bağlaması Ekle](media/azure-stack-solution-geo-distributed/image40.png)

App Service sertifikayı karşıya yüklemeyi bitirdiğinde, **SSL bağlamaları** bölümlerinde görüntülenir.

![SSL bağlamaları](media/azure-stack-solution-geo-distributed/image41.png)

#### <a name="remap-the-a-record-for-ip-ssl"></a>IP SSL için bir kaydı yeniden eşleyin

Web uygulamasında IP tabanlı SSL kullanılmıyorsa, [özel etki alanınız Için test https](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)'ye atlayın.

Varsayılan olarak, Web uygulaması paylaşılan bir genel IP adresi kullanır. Sertifika, IP tabanlı SSL ile bağlandığında, App Service Web uygulaması için yeni ve ayrılmış bir IP adresi oluşturur.

Bir kayıt Web uygulamasına eşlendiğinde, etki alanı kayıt defteri ayrılmış IP adresi ile birlikte güncelleştirilmeleri gerekir.

**Özel etki alanı** sayfası, yeni ve ayrılmış IP adresi ile güncelleştirilir. Bu [IP adresini](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)kopyalayın ve ardından [bir kaydı](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain) bu yeni IP adresiyle yeniden eşleyin.

#### <a name="test-https"></a>HTTPS’yi test etme

Çeşitli tarayıcılarda, Web uygulamasının sunulmasını sağlamak için. Custom. Domain > https://< gidin.

![Web uygulamasına gidin](media/azure-stack-solution-geo-distributed/image42.png)

> [!Note]  
> Sertifika doğrulama hataları oluşursa, kendinden imzalı bir sertifika neden olabilir ya da PFX dosyasına aktarılırken ara sertifikalar bırakılmış olabilir.

#### <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkes HTTP kullanarak Web uygulamasına erişebilir. HTTPS bağlantı noktasına yapılan tüm HTTP istekleri yeniden yönlendirilebilir.

Web uygulaması sayfasında **SL ayarları**' nı seçin. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS zorlama](media/azure-stack-solution-geo-distributed/image43.png)

İşlem tamamlandığında, uygulamayı işaret eden HTTP URL 'Lerinden birine gidin. Örneğin:

-   https://< app_name >. azurewebsites. net
-   https://northwindcloud.com
-   <https://www.northwindcloud.com>

#### <a name="enforce-tls-1112"></a>TLS 1.1/1.2 zorlama

Uygulama, artık [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)gibi sektör standartları tarafından güvenli bir şekilde ele alınmayacak olan [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1,0 ' i varsayılan olarak sağlar. Daha yüksek TLS sürümlerini zorlamak için şu adımları izleyin:

1.  Web uygulaması sayfasında, sol gezinti bölmesinde **SSL ayarları**' nı seçin.

2.  **TLS sürümü**' nde, en düşük TLS sürümünü seçin.

![TLS 1.1 veya 1.2’yi zorlama](media/azure-stack-solution-geo-distributed/image44.png)

### <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1.  Profil >  > oluştur Traffic Manager kaynak ağı oluştur ' u seçin. > 

2.  **Traffic Manager profili oluştur** dikey penceresini aşağıdaki gibi doldurun:

    1.  **Ad**alanına profil için bir ad girin. Bu adın Traffic manager.net bölgesi içinde benzersiz olması gerekir ve Traffic Manager profiline erişmek için kullanılan, trafik manager.net olan DNS adıyla sonuçlanır.

    2.  **Yönlendirme yöntemi**' nde **geographicrouting yöntemini**seçin.

    3.  **Abonelikte**, bu profilin oluşturulacağı aboneliği seçin.

    4.  **Kaynak Grubu** alanında bu profilin yerleştirileceği yeni bir kaynak grubu oluşturun.

    5.  **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılan Traffic Manager profilini etkilemez.

    6.  **Oluştur**’u seçin.

    7.  Traffic Manager profilinin genel dağıtımı tamamlandığında, ilgili kaynak grubunda kaynaklardan biri olarak listelenir.

    ![Traffic Manager profili oluşturma içindeki kaynak grupları](media/azure-stack-solution-geo-distributed/image45.png)

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

1. Portal arama çubuğunda, yukarıdaki bölümde oluşturulan **Traffic Manager profili** adını arayın ve görünen sonuçlarda Traffic Manager profilini seçin.

2. **Traffic Manager profili**' nde, **Ayarlar** bölümünde **uç noktalar**' ı seçin.

3. **Add (Ekle)** seçeneğini belirleyin.

4. Azure Stack uç noktası ekleniyor.

5. **Tür**için **dış uç nokta**' ı seçin.

6. Bu uç nokta için, ideal olarak Azure Stack adını belirtin.

7. Tam etki alanı adı (**FQDN**) için, Azure Stack Web UYGULAMASıNıN dış URL 'sini kullanın.

8. Coğrafi eşleme altında kaynağın bulunduğu bölgeyi/kıta seçin. Örneğin, **Avrupa.**

9. Görüntülenen ülke/bölge açılır kısmında, bu uç nokta için geçerli olan ülkeyi seçin. Örneğin, **Almanya**.

10. **Devre dışı olarak ekle** seçeneğini işaretsiz bırakın.

11. **Tamam**’ı seçin.

12. Azure Uç Noktası Ekleme:

    1.  **Tür**için **Azure uç noktası**' nı seçin.

    2.  Uç nokta için bir **ad** girin.

    3.  **Hedef kaynak türü**için **App Service**seçin.

    4.  **Hedef kaynak**için, aynı abonelikte Web Apps listesini göstermek üzere **bir App Service seçin** öğesini seçin. **Kaynak**bölümünde ilk uç nokta olarak kullanılan App Service 'i seçin.

13. Coğrafi eşleme altında kaynağın bulunduğu bölgeyi/kıta seçin. Örneğin, **Kuzey Amerika/Orta Amerika/Karayipler.**

14. Görüntülenen ülke/bölge açılır listesi altında, yukarıdaki bölgesel gruplamayı seçmek için bu noktayı boş bırakın.

15. **Devre dışı olarak ekle** seçeneğini işaretsiz bırakın.

16. **Tamam**’ı seçin.

    > [!Note]  
    >  Kaynak için varsayılan uç nokta olarak kullanılacak bir coğrafi kapsamı (Dünya) olan en az bir uç nokta oluşturun.

1. Her iki uç noktanın eklenmesi tamamlandığında, bunlar **Traffic Manager profilinde** görüntülenir ve bunların Izleme durumu **çevrimiçi**olarak gösterilir.

    ![Traffic Manager profili uç noktası durumu](media/azure-stack-solution-geo-distributed/image46.png)

**Küresel kurumsal, Azure coğrafi dağıtım özelliklerini kullanır**

Azure Traffic Manager ve coğrafi konuma özgü uç noktalar aracılığıyla veri trafiğini yönlendirmek, küresel kuruluşların bölgesel yönetmeliklere erişmesini sağlar ve yerel ve uzak iş konumlarının başarısı açısından önemli olan verileri uyumlu ve güvenli tutar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
