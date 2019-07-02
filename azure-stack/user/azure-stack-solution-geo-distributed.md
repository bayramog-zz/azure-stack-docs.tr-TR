---
title: Doğrudan trafik bir Azure ve Azure Stack ile coğrafi olarak dağıtılmış bir uygulama çözümüyle | Microsoft Docs
description: Azure ve belirli Uç noktalara trafiği yönlendiren Azure Stack ile coğrafi olarak dağıtılmış bir uygulama çözümü oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: a348e4e7eada9537defa292f667cfd3eb1e27438
ms.sourcegitcommit: eccbd0098ef652919f357ef6dba62b68abde1090
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67492471"
---
# <a name="tutorial-create-a-geo-distributed-app-solution-to-direct-traffic-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack ile trafiği yönlendirmek için bir coğrafi olarak dağıtılmış bir uygulama çözümü oluşturun

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Trafiği coğrafi olarak dağıtılmış uygulamaları desenini kullanarak, çeşitli ölçümlere göre belirli Uç noktalara yönlendirmek öğrenin. Traffic Manager'ı oluşturma, coğrafi tabanlı Yönlendirme ve uç nokta yapılandırma profili bilgileri bölgesel gereksinimleri, kurumsal ve uluslararası düzenleme ve verileri gereksinimlerinize göre Uç noktalara yönlendirilir sağlar.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Coğrafi olarak dağıtılmış bir uygulama oluşturun.
> - Traffic Manager, uygulamanızı hedeflemek için kullanın.

## <a name="use-the-geo-distributed-apps-pattern"></a>Coğrafi olarak dağıtılmış uygulamaları desenini kullanma

Coğrafi olarak dağıtılmış desen ile uygulamanızı bölgeleri kapsar. Genel buluta varsayılan, ancak bazı kullanıcılarınız kendi veri bölgelerinde kalmasını gerektirebilir. Kullanıcılar kendi gereksinimlerine göre en uygun bulut yönlendirebilir.

### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

#### <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler

Ölçeklenebilirlik uyum sağlamak için bu öğreticiyle oluşturacaksınız çözüm değildir. Ancak, diğer Azure ve şirket içi çözümler ile birlikte kullanıldığında, ölçeklenebilirlik gereksinimlerini barındırabilir. Yöneticisi aracılığıyla trafiği otomatik olarak ölçeklendirme ile karma bir çözüm oluşturma hakkında daha fazla bilgi için bkz: [Azure ile Bulutlar arası ölçeklendirme çözümleri oluşturun](azure-stack-solution-cloud-burst.md).

#### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Bu çözüm ölçeklenebilirliği hakkında hususlar olduğu gibi doğrudan kullanılabilirlik ele almaz. Ancak, Azure ve şirket içi çözümler dahil tüm bileşenler için yüksek kullanılabilirlik sağlamak için bu çözüm içinde uygulanabilir.

### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar

- Kuruluşunuz özel bölgesel güvenlik ve dağıtım ilkeleri gerektiren uluslararası dalları sahiptir.

- Her biri, kuruluşunuzun ofisleri ilgili yerel düzenlemelere ve saat dilimleri başına etkinlik raporlama gerektiren çalışan, iş ve tesis veri çeker.

- Büyük ölçekli gereksinimler bölgeler aşırı yük gereksinimlerini karşılamak için tek bir bölge içinde ve arasında birden çok uygulama dağıtımları olan uygulamalarda yatay ölçeklendirme tarafından sağlandığından.

### <a name="planning-the-topology"></a>Topoloji planlama

Bir dağıtılmış uygulama Ayak izi kullanıma yapılandırmadan önce şunları bilmek yardımcı olur:

-   **Uygulama için özel etki alanı:** Müşteriler, uygulamaya erişmek için kullanacağı özel etki alanı adı nedir? Örnek uygulama için özel etki alanı adıdır *www.scalableasedemo.com.*

-   **Traffic Manager etki alanı:** Bir etki alanı adı oluşturma sırasında seçilir bir [Azure Traffic Manager profilini](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles). Bu ad ile birlikte *trafficmanager.net* soneki Traffic Manager tarafından yönetilen bir etki alanı girişi kaydetmek için kullanılır. Örnek uygulama için adı seçilen olduğu *ase tanıtım ölçeklenebilir*. Sonuç olarak, Traffic Manager tarafından yönetilen tam etki alanı adıdır *ase demo.trafficmanager.net ölçeklenebilir*.

-   **Uygulama Ayak izi ölçeklendirme stratejisi:** Uygulama Ayak izi birden fazla App Service ortamları tek bir bölge, birden çok bölgeye veya her iki yaklaşım bir karışımını üzerinde dağıtılmış olup olmadığını belirleyin. Burada müşteri trafiğinden kaynaklanan ve ne kadar iyi bir uygulamanın arka uç altyapısı destekleme rest ölçeklendirebilirsiniz beklentilerini kararı bağlı olmalıdır. Örneğin, durum bilgisi olmayan bir % 100 uygulaması sayesinde, bir uygulamayı yüksek düzeyde Azure bölgelerinde dağıtılan App Service ortamları ile çarpılır Azure bölgesi başına birden fazla App Service ortamları oluşan birleşimlerin kullanıldığı ölçeklendirilebilir. 15 + genel olan Azure bölgeleri seçim yapabileceğiniz, müşterilerin gerçek anlamda bir dünya çapında hiper ölçekli uygulama Ayak izi oluşturabilirsiniz. Aşağıda kullanılan örnek uygulama için bir tek bir Azure bölgesinde (Güney Orta ABD) üç App Service ortamları oluşturuldu.

-   **App Service ortamları için adlandırma kuralı:** Her App Service ortamı, benzersiz bir ad gerektirir. Bir veya iki App Service ortamları, her bir App Service ortamı belirlemenize yardımcı olması için bir adlandırma kuralınızın bulunduğundan yardımcı olur. Aşağıda kullanılan örnek uygulama için basit bir adlandırma kuralı kullanıldı. Üç App Service ortamları adlarıdır *fe1ase*, *fe2ase*, ve *fe3ase*.

-   **Uygulamalar için adlandırma kuralı:** Uygulama birden çok örneğini dağıtılacak olduğundan, dağıtılan uygulamanın her örneği için bir ad gereklidir. App Service ortamları ile aynı uygulama adı birden çok ortamda kullanılabilir. Her App Service ortamı benzersiz etki alanı soneki olduğundan, geliştiricilere her ortamda tam olarak aynı uygulama adı yeniden seçebilirsiniz. Örneğin, bir geliştirici uygulamaları gibi adlı sahip olabilirsiniz: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*ve benzeri. Her uygulama örneği, burada kullanılan bir uygulama için benzersiz bir adı vardır. Kullanılan uygulama örneği adları *webfrontend1*, *webfrontend2*, ve *webfrontend3*.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu çevikliğini ve yenilik, şirket içi ortamınıza bulut getirir.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) yazılım kalitesinin yapı taşları tasarlama, dağıtma ve çalıştırma için (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) gözden geçirmeleri karma uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.

## <a name="part-1-create-a-geo-distributed-app"></a>1\. Bölüm: Coğrafi olarak dağıtılmış bir uygulama oluşturma

Bu bölümünde, bir web uygulaması oluşturacaksınız.

> [!div class="checklist"]
> - Web uygulamaları oluşturun ve yayımlayın.
> - Azure depoları için kod ekleyin.
> - Uygulama derleme, birden fazla bulut hedefe gelin.
> - CD işlem yönetip yeniden açın.

### <a name="prerequisites"></a>Önkoşullar

Bir Azure aboneliği ve Azure Stack yükleme gereklidir.

### <a name="geo-distributed-app-steps"></a>Coğrafi olarak dağıtılmış bir uygulama adımları

### <a name="obtain-a-custom-domain-and-configure-dns"></a>Özel bir etki alanı almak ve DNS yapılandırma

Etki alanı için DNS bölge dosyasını güncelleştirin. Azure AD, ardından özel etki alanı adı sahipliğini doğrulayabilirsiniz. Kullanım [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) , azure'da Azure/Office 365/dış DNS kayıtları için veya, DNS girişini eklemesini [farklı bir DNS kayıt şirketi](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1. Özel bir etki alanı, ortak bir kayıt şirketi ile kaydedin.

2. Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. Onaylanmış yönetici DNS güncelleştirmeleri yapmanız gerekebilir.

3. Azure AD tarafından sağlanan DNS girişini ekleyerek etki alanının DNS bölge dosyasını güncelleştirin. DNS girişi, posta yönlendirme veya web barındırma gibi davranışları değiştirmez.

### <a name="create-web-apps-and-publish"></a>Web uygulamaları oluşturma ve yayımlama

Azure, Azure Stack ve her iki bulut için otomatik anında iletme değişiklikleri Web uygulamasını dağıtmak için karma sürekli tümleştirme/sürekli teslimi (CI/CD) ayarlayın.

> [!Note]  
> Azure Stack çalıştırma (Windows Server ve SQL) ve App Service dağıtımı için genel olarak uygun görüntülerle gereklidir. Daha fazla bilgi için [Azure Stack'te App Service ile çalışmaya başlamadan önce](../operator/azure-stack-app-service-before-you-get-started.md) , Azure Stack operatör belgeleri.

#### <a name="add-code-to-azure-repos"></a>Azure depoları için kod ekleyin

1. Oturum açmak için Visual Studio ile bir **proje oluşturma haklarına sahip hesabın** üzerinde Azure depolar.

    CI/CD, hem uygulama kodu ve altyapı kodunu uygulayabilirsiniz. Kullanım [Azure Resource Manager şablonları](https://azure.microsoft.com/resources/templates/) hem özel hem de barındırılan buluta yönelik geliştirme için.

    ![Visual Studio'da bir projeye bağlanın](media/azure-stack-solution-geo-distributed/image1.JPG)

2. **Depoyu kopyalama** oluşturarak ve varsayılan bir web uygulamasını açma.

    ![Visual Studio'da depoyu Kopyala](media/azure-stack-solution-geo-distributed/image2.png)

### <a name="create-web-app-deployment-in-both-clouds"></a>Her iki bulut Web uygulama dağıtımı oluşturma

1.  Düzen **WebApplication.csproj** dosyası: Seçin `Runtimeidentifier` ve ekleme `win10-x64`. (Bkz [Self-contained dağıtım](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) belgelerine.)

    ![Visual Studio'da Web uygulaması proje dosyasını Düzenle](media/azure-stack-solution-geo-distributed/image3.png)

1.  **Azure depoları kodu iade** Takım Gezgini'ni kullanarak.

2.  Onaylayın **uygulama kodu** Azure depolara denetlendi.

### <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1. **Azure işlem hatlarına oturum** yapı tanımları oluşturma olanağı onaylamak için.

2. Ekleme `-r win10-x64` kod. Bu ayrıca, .NET Core ile kendi içinde bir dağıtımı tetiklemek gereklidir.

    ![Derleme tanımı için kod ekleyin](media/azure-stack-solution-geo-distributed/image4.png)

3. **Yapıyı çalıştırmak**. [Müstakil dağıtım derleme](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) işlem, Azure ve Azure Stack üzerinde çalışabilen yapıtları yayımlar.

**Bir Azure barındırılan aracı kullanma**

Azure işlem hatlarında barındırılan aracı kullanarak web uygulamaları oluşturmak ve dağıtmak için kullanışlı bir seçenektir. Bakımı ve yükseltmeler, kesintisiz geliştirme, test ve dağıtım sağlayan Microsoft Azure tarafından otomatik olarak gerçekleştirilir.

### <a name="manage-and-configure-the-cd-process"></a>CD işlem yapılandırma ve yönetme

Azure DevOps ve Azure DevOps sunucusu yüksek oranda yapılandırılabilir ve yönetilebilir bir işlem hattı geliştirme gibi birden çok ortama yayınlar için hazırlama, QA ve üretim ortamlarında; sağlayın onay gerektiren belirli aşamalarda dahil.

## <a name="create-release-definition"></a>Yayın tanımı oluşturma

1.  Seçin **yanı sıra** altında yeni bir yayın eklemek için Ekle düğmesine **yayınlar** sekmesinde **derleme ve yayın** VSO bölümü.

    ![Yayın tanımı oluşturma](media/azure-stack-solution-geo-distributed/image5.png)

2. Azure uygulama hizmeti dağıtımının şablonu uygulayın.

   ![Azure uygulama hizmeti dağıtımının Şablonu Uygula](meDia/azure-stack-solution-geo-distributed/image6.png)

3. Altında **yapıt ekleme**, Azure bulut derleme uygulaması için yapıt ekleyin.

   ![Azure bulut yapı için yapıt ekleme](media/azure-stack-solution-geo-distributed/image7.png)

4. İşlem hattı sekmesi altında seçin **aşama, görev** ortamının bağlamak ve Azure bulut ortamına değerlerini ayarlayın.

   ![Azure bulut ortamına değerlerini ayarlayın](media/azure-stack-solution-geo-distributed/image8.png)

5. Ayarlama **ortam adı** seçip **Azure aboneliği** Azure bulut uç noktası için.

      ![Azure bulut uç noktası için Azure aboneliğini seçin](media/azure-stack-solution-geo-distributed/image9.png)

6. Altında **uygulama hizmeti adı**, gerekli Azure uygulama hizmeti adını ayarlayın.

      ![Set Azure uygulama hizmeti adı](media/azure-stack-solution-geo-distributed/image10.png)

7. Altında "Hosted VS2017" girin **aracı kuyruğu** barındırılan Azure bulut ortamı için.

      ![Azure bulutta barındırılan ortam için aracı kuyruğu ayarlayın](media/azure-stack-solution-geo-distributed/image11.png)

8. Azure App Service'e dağıtma geçerli menüde **paket veya klasör** ortam için. Seçin **Tamam** için **klasör konumu**.
  
      ![Paket veya Azure App Service ortamı için klasör seçin](media/azure-stack-solution-geo-distributed/image12.png)

      ![Paket veya Azure App Service ortamı için klasör seçin](media/azure-stack-solution-geo-distributed/image13.png)

9. Tüm değişiklikleri kaydetmek ve geri dönüp **yayın ardışık düzeni**.

    ![Yayın işlem hattında Değişiklikleri Kaydet](media/azure-stack-solution-geo-distributed/image14.png)

10. Azure Stack uygulaması için derleme seçerek yeni bir yapıt ekleyin.
    
    ![Azure Stack'te uygulama için yeni yapıt ekleme](media/azure-stack-solution-geo-distributed/image15.png)


11. Azure uygulama hizmeti dağıtımının uygulayarak bir daha fazla ortam ekleyin.
    
    ![Azure uygulama hizmeti dağıtımının için ortam Ekle](media/azure-stack-solution-geo-distributed/image16.png)

12. Azure Stack yeni ortam adı.
    
    ![Azure uygulama hizmeti dağıtımının ortamında adı](media/azure-stack-solution-geo-distributed/image17.png)

13. Azure Stack ortamı altında bulmak **görev** sekmesi.
    
    ![Azure Stack ortamı](media/azure-stack-solution-geo-distributed/image18.png)

14. Azure Stack uç noktası için bir abonelik seçin.
    
    ![Azure Stack uç noktası için bir abonelik seçin](media/azure-stack-solution-geo-distributed/image19.png)

15. Azure Stack web uygulaması adı, uygulama hizmeti adı ayarlayın.

    ![Azure Stack web uygulaması adı ayarlayın](media/azure-stack-solution-geo-distributed/image20.png)

16. Azure Stack Aracısı'nı seçin.
    
    ![Azure Stack Aracısı'nı seçin](media/azure-stack-solution-geo-distributed/image21.png)

17. Azure App Service'e dağıtma bölümü altında geçerli seçin **paket veya klasör** ortam için. Seçin **Tamam** klasör konumuna.

    ![Azure uygulama hizmeti dağıtımının klasörü seçin](media/azure-stack-solution-geo-distributed/image22.png)

    ![Azure uygulama hizmeti dağıtımının klasörü seçin](media/azure-stack-solution-geo-distributed/image23.png)

18. Değişken sekmesi altında adlı bir değişken ekleyin `VSTS\_ARM\_REST\_IGNORE\_SSL\_ERRORS`, değer olarak ayarlanmış **true**ve Azure Stack için kapsam.
    
    ![Azure uygulama dağıtımı değişken Ekle](media/azure-stack-solution-geo-distributed/image24.png)

19. Seçin **sürekli** yapıtları hem de etkin dağıtım tetikleyicisi simgesi **devam eder** dağıtım tetikleyicisi.
    
    ![Sürekli dağıtım tetikleyicisi seçin](media/azure-stack-solution-geo-distributed/image25.png)

20. Seçin **dağıtım öncesi** Azure Stack ortamında koşullar simgesi ve tetikleyici kümesine **sürümünden sonra.**
    
    ![Dağıtım öncesi koşulları seçin](media/azure-stack-solution-geo-distributed/image26.png)

21. Tüm değişiklikleri kaydedin.

> [!Note]  
> Görevler için bazı ayarları otomatik olarak tanımlanmış olabilir [ortam değişkenlerini](https://docs.microsoft.com/azure/devops/pipelines/release/variables?view=vsts&tabs=batch#custom-variables) bir şablondan bir yayın tanımı oluşturma sırasında. Bu ayarlar görev ayarlarını değiştirilemez; Bunun yerine, bu ayarları düzenleyebilmeniz için üst ortam öğesi seçilmelidir.

## <a name="part-2-update-web-app-options"></a>2\. Bölüm: Web uygulama seçenekleri güncelleştir

[Azure App Service](https://docs.microsoft.com/azure/app-service/overview), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. 

![Azure App Service](media/azure-stack-solution-geo-distributed/image27.png)

> [!div class="checklist"]
> - Mevcut bir özel DNS adını Azure Web Apps'e eşleyin.
> - Kullanım bir **CNAME kaydı** ve **kayıt** özel DNS adını App Service'e eşlemek için.

### <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Mevcut bir özel DNS adını Azure Web Apps ile eşleme

> [!Note]  
>  Kök etki alanını (örneğin, northwind.com) dışında tüm özel DNS adları için CNAME kullanın.

Canlı siteyi ve onun DNS etki alanı adını App Service'e geçirmek için, bkz. [Etkin DNS adını Azure App Service'e geçirme](https://docs.microsoft.com/azure/app-service/manage-custom-dns-migrate-domain).

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

-   [Bir App Service uygulaması oluşturma](https://docs.microsoft.com/azure/app-service/), veya başka bir öğretici için oluşturduğunuz bir uygulama kullanın.

-   Bir etki alanı adı satın alın ve etki alanı sağlayıcınız için DNS kayıt defterine erişim emin olun.

Etki alanı için DNS bölge dosyasını güncelleştirin. Azure AD'ye özel etki alanı adı sahipliğini doğrular. Kullanım [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) , azure'da Azure/Office 365/dış DNS kayıtları için veya, DNS girişini eklemesini [farklı bir DNS kayıt şirketi](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

-   Özel bir etki alanı, ortak bir kayıt şirketi ile kaydedin.

-   Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. (Onaylanmış yönetici DNS güncelleştirmeleri yapmanız gerekebilir.)

-   Azure AD tarafından sağlanan DNS girişini ekleyerek etki alanının DNS bölge dosyasını güncelleştirin.

Örneğin, northwindcloud.com ve www.northwindcloud.com DNS girdileri eklemek üzere northwindcloud.com kök etki alanı için DNS ayarlarını yapılandırın.

> [!Note]  
>  Kullanarak bir etki alanı adı satın alınabilecek [Azure portalında](https://docs.microsoft.com/azure/app-service/manage-custom-dns-buy-domain). Özel DNS adını web uygulamasına eşlemek için, web uygulamasının [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) ücretli bir katmanda (**Paylaşılan**, **Temel**, **Standart** veya **Premium** olmalıdır).



### <a name="create-and-map-cname-and-a-records"></a>Oluşturun ve CNAME ve A kayıtları eşleyin

#### <a name="access-dns-records-with-domain-provider"></a>Etki alanı sağlayıcısı ile DNS kayıtlarına erişme

> [!Note]  
>  Azure DNS, Azure Web Apps için özel DNS adını yapılandırmak için kullanın. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](https://docs.microsoft.com/azure/dns/dns-custom-domain).

1.  Ana sağlayıcı Web sitesi için oturum açın.

2.  DNS kayıtlarını yönetme sayfasını bulun. Her etki alanı sağlayıcısının kendi DNS kayıtları arabirimi vardır. Sitede **Domain Name**, **DNS** veya **Name Server Management** etiketli alanları bulun.

DNS kayıtları sayfası görüntülenebilir **My etki alanları**. Adlı bağlantı **bölge dosyası**, **DNS kayıtlarını**, veya **Gelişmiş Yapılandırma**.

DNS kayıtları sayfasının bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz:

![Örnek DNS kayıtları sayfası](media/azure-stack-solution-geo-distributed/image28.png)

1. Etki alanı adı kayıt şirketi içinde seçin **Ekle veya oluşturma** bir kayıt oluşturmak için. Bazı sağlayıcıların farklı kayıt türlerini eklemek için farklı bağlantıları vardır. Sağlayıcının belgelerine başvurun.

2. Uygulamanın varsayılan konak adıyla bir alt etki alanı eşlemek için bir CNAME kaydı ekleyin.

   Www.northwindcloud.com etki alanı örneğin adına eşleyen bir CNAME kaydı ekleyin < app\_adı >. azurewebsites.net.

CNAME ekledikten sonra DNS kayıtları sayfası aşağıdaki örnekteki gibi görünür:

![Azure uygulamasına portal gezintisi](media/azure-stack-solution-geo-distributed/image29.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>Azure'da CNAME kaydı eşlemesini etkinleştirme

1. Yeni bir sekmede, Azure portalında oturum açın

2. Uygulama Hizmetleri'ne gidin.

3. Web uygulamasını seçin.

4. Azure Portal'daki uygulama sayfasının sol gezintisinde **Özel etki alanları**'nı seçin.

5. **Konak adı ekle**'nin yanındaki **+** simgesini seçin.

6. Tam etki alanı adı yazın `www.northwindcloud.com`.

7. **Doğrula**'yı seçin.

8. Bu seçenek belirtilmişse, diğer tür ek kayıt ekleme (`A` veya `TXT`) etki alanı adı kaydedicilerin DNS kayıtlarını. Azure, bu kayıt türlerini ve değerlerini sağlayacak:

   a.  Uygulamanın IP adresini eşlemek için bir **A** kaydı.

   b.  A **TXT** uygulamanın varsayılan konak < app_name > adına eşlemek için kaydı. azurewebsites.net. App Service özel etki alanı sahipliğini doğrulamak için yalnızca yapılandırma sırasında bu kaydı kullanır. Doğrulamadan sonra TXT kaydını silin.

9. Kadar düzeltin ve etki alanı kayıt şirketi sekmesinde bu görevi tamamlamak **konak adı Ekle** düğmesi etkinleştirilir.

10. Emin olun **konak adı kayıt türü** ayarlanır **CNAME** (www.example.com veya herhangi bir alt etki alanı).

11. **Konak adı ekle**'yi seçin.

12. Tam etki alanı adı yazın `northwindcloud.com`.

13. **Doğrula**'yı seçin. **Ekle** etkinleştirilir.

14. Emin olun **konak adı kayıt türü** ayarlanır **kayıt** (example.com).

15. **Konak adı Ekle**.

    Yeni konak adları uygulamanın yansıtılması biraz zaman alabilir **özel etki alanları** sayfası. Verileri güncelleştirmek için tarayıcıyı yenilemeyi deneyin.
  
    ![Özel etki alanları](media/azure-stack-solution-geo-distributed/image31.png) 
  
    Bir hata varsa, sayfanın en altında bir doğrulama hata bildirimine görünür. ![Doğrulama hatası](media/azure-stack-solution-geo-distributed/image32.png)

> [!Note]  
>  Yukarıdaki adımları bir joker karakter etki alanını eşlemek için yinelenebilir (\*. northwindcloud.com). Bu, her biri için ayrı bir CNAME kaydı oluşturmak zorunda kalmadan bu app service için ek güncelleştirmemiz eklenmesini sağlar. Bu ayarı yapılandırmak için kayıt yönergeleri izleyin.

#### <a name="test-in-a-browser"></a>Bir tarayıcıda test

Daha önce yapılandırılmış olan DNS adlarına göz atın (örneğin, `northwindcloud.com` veya www.northwindcloud.com).

## <a name="part-3-bind-a-custom-ssl-cert"></a>3\. Bölüm: Özel bir SSL sertifikası bağlama

Bu bölümünde yapacağız:

> [!div class="checklist"]
> - App Service için özel bir SSL sertifikasını bağlar.
> - Uygulama için HTTPS zorlama.
> - Betiklerle SSL sertifikası bağlaması otomatikleştirin.

> [!Note]  
> Gerekirse, bir müşterinin Azure portalında SSL sertifikası elde ve web uygulamasına bağlar. Daha fazla bilgi için [App Service sertifikaları öğreticisini](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site).

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

-   [Bir App Service uygulaması oluşturun.](https://docs.microsoft.com/azure/app-service/)
-   [Özel DNS adını web uygulamanıza eşleme.](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain)
-   Güvenilir sertifika yetkilisinden SSL sertifikası almak ve isteğini imzalamak için anahtar kullanın.

### <a name="requirements-for-your-ssl-certificate"></a>SSL sertifikanıza yönelik gereksinimler

Bir sertifikayı App Service’te kullanabilmek için sertifikanın aşağıdaki tüm gereksinimleri karşılaması gerekir:

-   Bir güvenilen sertifika yetkilisi tarafından imzalanmış.

-   Parola korumalı bir PFX dosyası olarak dışarı aktarılması.

-   Özel anahtarı en az 2048 bit uzun içerir.

-   Sertifika zincirindeki tüm ara sertifikaları içerir.

> [!Note]  
>  **Eliptik Eğri Şifrelemesi (ECC) sertifikaları** App Service ile çalışma, ancak bu kılavuzda dahil edilmez. ECC sertifikaları oluşturma konusunda yardım almak için bir sertifika yetkilisine başvurun. 

#### <a name="prepare-the-web-app"></a>Web uygulaması hazırlama

Web uygulaması için özel bir SSL sertifikası bağlamak için [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) olmalıdır **temel**, **standart**, veya **Premium** katmanı.

#### <a name="sign-in-to-azure"></a>Azure'da oturum açma

1.  Açık [Azure portalında](https://portal.azure.com/) ve web uygulamasına gidin.

2.  Sol menüden **uygulama hizmetleri**ve ardından web uygulaması adını seçin.

![Web uygulaması seçme](media/azure-stack-solution-geo-distributed/image33.png)

#### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

1.  Web uygulaması sayfasının sol gezinti bölmesinde kaydırarak **ayarları** seçin ve bölüm **ölçeği Artır (App Service planı)** .

    ![Ölçeği artır menüsü](media/azure-stack-solution-geo-distributed/image34.png)

1.  Web uygulamasına değil de olun **ücretsiz** veya **paylaşılan** katmanı. Web uygulamanızın geçerli katmanı Koyu mavi bir kutu vurgulanır.

    ![Fiyatlandırma katmanını denetleyin](media/azure-stack-solution-geo-distributed/image35.png)

Özel SSL desteklenmez **ücretsiz** veya **paylaşılan** katmanı. Ölçeklemenizi için sonraki bölümde yer alan adımları izleyin veya **fiyatlandırma katmanınızı seçin** sayfasında ve atlamak [karşıya yükleme ve SSL sertifikanızı bağlama](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl).

#### <a name="scale-up-your-app-service-plan"></a>App Service planınızın ölçeğini artırma

1.  **Temel**, **Standart** veya **Premium** katmanlarından birini seçin.

2.  Seçin **seçin**.

![Fiyatlandırma katmanı seçme](media/azure-stack-solution-geo-distributed/image36.png)

Bildirim görüntülendiğinde ölçeklendirme işlemi tamamlanmıştır.

![Ölçek artırma bildirimi](media/azure-stack-solution-geo-distributed/image37.png)

#### <a name="bind-your-ssl-certificate-and-merge-intermediate-certificates"></a>SSL sertifikanızı bağlama ve Ara sertifikaları birleştirme

Birden çok sertifika zincirindeki birleştirin.

1. **Her sertifika açın** bir metin düzenleyicisinde aldı.

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

Birleştirilmiş SSL sertifikası, sertifika tarafından oluşturulan özel anahtarla dışarı aktarın.

Bir özel anahtar dosyası OpenSSL oluşturulur. Sertifikayı PFX dosyasına aktarmak için aşağıdaki komutu çalıştırın ve yer tutucularını değiştirin  *\<özel anahtar dosyası >* ve  *\<birleştirilmiş-sertifika-dosyası >* ile özel anahtar yolu ve birleştirdiğiniz sertifika dosyasının:

```powershell
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>
```

İstendiğinde, daha sonra App Service için SSL sertifikanızı karşıya yükleme için bir dışarı aktarma parolası tanımlayın.

IIS veya **Certreq.exe** kullanılan sertifika isteği oluşturmak için sertifikayı yerel bir makineye yükleyin ve ardından [sertifikayı PFX'e aktarın](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

#### <a name="upload-the-ssl-certificate"></a>SSL sertifikasını karşıya yükle

1. Seçin **SSL ayarları** web uygulamasının sol gezinti bölmesinde.

2. Seçin **sertifikayı karşıya yükle**.

3. İçinde **PFX sertifika dosyası**seçin PFX dosyası.

4. İçinde **sertifika parolası**, PFX dosyasını dışarı aktarılırken oluşturulan parola yazın.

5. **Karşıya Yükle**’yi seçin.

![Sertifikayı karşıya yükleme](media/azure-stack-solution-geo-distributed/image38.png)

App Service sertifikanızı karşıya yüklemeyi tamamladığında, görünür **SSL ayarları** sayfası.

![SSL ayarları](media/azure-stack-solution-geo-distributed/image39.png)

#### <a name="bind-your-ssl-certificate"></a>SSL sertifikanızı bağlama

1.  İçinde **SSL bağlamaları** bölümünden **bağlama Ekle**.

    > [!Note]  
    >  Sertifika karşıya yüklendi, ancak etki alanı adlarını görünmez **Hostname** açılır listesinde, tarayıcı sayfasını yenilemeyi deneyin.

1.  İçinde **SSL bağlaması Ekle** sayfasında, güvenliğini sağlamak için etki alanı adı seçmek için açılır listeleri ve kullanılacak sertifikayı kullanın.

1.  **SSL Türü** menüsünde [**Sunucu Adı Belirtme (SNI)** ](https://en.wikipedia.org/wiki/Server_Name_Indication) veya IP tabanlı SSL seçeneklerinden hangisini kullanacağınızı belirleyin.

    - **SNI tabanlı SSL**: Birden fazla SNI tabanlı SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden fazla SSL sertifikası ile birden fazla etki alanının güvenliğini sağlamaya olanak tanır. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI’yi destekler (daha kapsamlı tarayıcı desteği bilgilerini [Sunucu Adı Belirtimi](https://wikipedia.org/wiki/Server_Name_Indication) bölümünde bulabilirsiniz).

    - **IP tabanlı SSL**: Yalnızca bir IP tabanlı SSL bağlaması eklenebilir. Bu seçenek yalnızca bir SSL sertifikası ile ayrılmış bir genel IP adresinin güvenliğini sağlamaya olanak tanır. Birden çok etki alanı güvenliğini sağlamak için bunları tüm kaydettiğiniz SSL sertifikasını kullanarak güvenli hale getirin. IP tabanlı SSL SSL bağlaması için geleneksel bir seçenektir.

1. Seçin **bağlaması Ekle**.

    ![SSL bağlaması Ekle](media/azure-stack-solution-geo-distributed/image40.png)

App Service sertifikanızı karşıya yüklemeyi tamamladığında, görünür **SSL bağlamaları** bölümler.

![SSL bağlamaları](media/azure-stack-solution-geo-distributed/image41.png)

#### <a name="remap-the-a-record-for-ip-ssl"></a>IP SSL için A kaydını yeniden eşleme

IP tabanlı SSL web uygulamasında kullanılmayan, atlamak [özel etki alanınız için Test HTTPS](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl).

Varsayılan olarak, web uygulaması, paylaşılan bir genel IP adresini kullanır. Sertifikayı IP tabanlı SSL ile ilişkilidir, App Service web uygulaması için yeni ve ayrılmış bir IP adresi oluşturur.

Web uygulaması için bir A kaydı eşlendiğinde, etki alanı kayıt defteri ayrılmış IP adresi ile güncelleştirilmesi gerekir.

**Özel etki alanı** sayfası, yeni ve ayrılmış IP adresi ile güncelleştirilir. Bu kopyalar [IP adresi](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain), ardından yeniden eşleme [kayıt](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain) bu yeni IP adresi.

#### <a name="test-https"></a>HTTPS’yi test etme

Çeşitli tarayıcılarda https://<your.custom.domain>to göz atın olun web uygulaması olarak sunulur.

![Web uygulamasına göz atma](media/azure-stack-solution-geo-distributed/image42.png)

> [!Note]  
> Sertifika doğrulama hataları oluşursa, neden otomatik olarak imzalanan bir sertifika olabilir veya Ara Sertifika PFX dosyasına dışarı aktarılırken bırakıldı devre dışı.

#### <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkes HTTP kullanarak web uygulamasına erişebilir. tüm HTTP isteklerini HTTPS bağlantı noktasına yönlendirilebilir.

Web uygulaması sayfasında seçin **SL ayarları**. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS zorlama](media/azure-stack-solution-geo-distributed/image43.png)

İşlem tamamlandığında, herhangi bir uygulamaya işaret eden HTTP URL'leri gidin. Örneğin:

-   https://<app_name>.azurewebsites.net
-   https://northwindcloud.com
-   <https://www.northwindcloud.com>

#### <a name="enforce-tls-1112"></a>TLS 1.1/1.2 zorlama

Uygulamanın verir [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) artık sektör standartlarıyla gibi güvenli olarak kabul edilir ve varsayılan olarak 1.0 [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Daha yüksek TLS sürümlerini zorlamak için şu adımları izleyin:

1.  Web uygulama sayfasındaki sol gezinti bölmesinde seçin **SSL ayarları**.

2.  İçinde **TLS sürümü**, en düşük TLS sürümünü seçin.

![TLS 1.1 veya 1.2’yi zorlama](media/azure-stack-solution-geo-distributed/image44.png)

### <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1.  Seçin **kaynak Oluştur** > **ağ** > **Traffic Manager profili** > **Oluştur**.

2.  **Traffic Manager profili oluştur** dikey penceresini aşağıdaki gibi doldurun:

    1.  İçinde **adı**, profil için bir ad sağlayın. Bu ad DNS adını, Traffic Manager profilinize erişmek için kullanılan trafiği manager.net sonuçlanır ve trafik manager.net bölgesi içinde benzersiz olması gerekir.

    2.  İçinde **yönlendirme yöntemi**seçin **Geographicrouting yöntemi**.

    3.  İçinde **abonelik**, altında bu profili oluşturulacağı aboneliği seçin.

    4.  **Kaynak Grubu** alanında bu profilin yerleştirileceği yeni bir kaynak grubu oluşturun.

    5.  **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar kaynak grubunun konumunu ifade eder ve Traffic Manager profilinde herhangi bir etkisi Global olarak dağıtılmış.

    6.  **Oluştur**’u seçin.

    7.  Traffic Manager profilinizin genel dağıtımı tamamlandıktan sonra ilgili kaynak grubunda kaynaklardan biri listelenir.

    ![Kaynak grupları, Traffic Manager profili oluşturma](media/azure-stack-solution-geo-distributed/image45.png)

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

1. Portalları arama çubuğunda arama **Traffic Manager profili** görüntülenen sonuçlarında traffic manager profili seçin ve önceki bölümde oluşturulan adı.

2. İçinde **Traffic Manager profili**, **ayarları** bölümünden **uç noktaları**.

3. **Add (Ekle)** seçeneğini belirleyin.

4. Azure Stack uç noktası ekleniyor.

5. İçin **türü**seçin **dış uç noktası**.

6. Sağlayan bir **adı** için bu endpoint, ideal olarak Azure Stack adını.

7. Tam etki alanı adı (**FQDN**), Azure Stack Web uygulaması için dış URL'yi kullanın.

8. Kaynağın bulunduğu bölge/Kıta coğrafi eşleme altında seçin. Örneğin, **Avrupa.**

9. Ülke/bölge görünen açılan altında Bu uç noktaya uygulayan ülkeyi seçin. Örneğin, **Almanya**.

10. **Devre dışı olarak ekle** seçeneğini işaretsiz bırakın.

11. **Tamam**’ı seçin.

12. Azure uç noktası ekleniyor:

    1.  İçin **türü**seçin **Azure uç noktası**.

    2.  Sağlayan bir **adı** uç noktası için.

    3.  İçin **hedef kaynak türü**seçin **App Service**.

    4.  İçin **hedef kaynak**seçin **uygulama hizmeti seçin** aynı abonelik altında Web uygulamaların listesini göstermek için. İçinde **kaynak**, ilk uç noktası olarak kullanılan App Service'i seçiyorum.

13. Kaynağın bulunduğu bölge/Kıta coğrafi eşleme altında seçin. Örneğin, **Kuzey Amerika/Orta Amerika/Karayipler.**

14. Ülke/bölge görünen açılan altında yukarıdaki bölgesel gruplandırma tümünü seçmek için bu nokta boş bırakın.

15. **Devre dışı olarak ekle** seçeneğini işaretsiz bırakın.

16. **Tamam**’ı seçin.

    > [!Note]  
    >  (Kaynak için varsayılan uç nokta olarak görev yapacak tüm dünya), coğrafi bir kapsamla en az bir uç noktası oluşturun.

1. Her iki uç noktanın eklenmesi tamamlandığında, bunların görüntülenme **Traffic Manager profili** izleme durumuyla birlikte **çevrimiçi**.

    ![Traffic Manager profili ve uç nokta durumu](media/azure-stack-solution-geo-distributed/image46.png)

**Küresel kuruluşlar Azure coğrafi dağıtım yeteneklerini kullanır.**

Azure Traffic Manager ve Coğrafya özel uç noktaları aracılığıyla veri trafiği yönlendiren bölgesel düzenlemelere bağlı kalmayı ve yerel ve uzak iş konumu başarısı için önemli olan verileri uyumlu ve güvenli tutun açıdan sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
