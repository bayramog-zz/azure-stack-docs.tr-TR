---
title: Azure ile karma bulut dağıtma ve Azure Stack | Microsoft Docs
description: Azure ve Azure Stack ile karma bulut dağıtmayı öğrenin.
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
ms.date: 01/25/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 201af19756ef3958e943549482610ee32e208f01
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603075"
---
# <a name="deploy-a-hybrid-cloud-solution-with-azure-and-azure-stack"></a>Azure ve Azure Stack bir karma bulut çözümü dağıtın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu senaryoda, Azure genel bulutu ve Azure Stack özel bulutu kullanan bir hibrit bulut çözümünün nasıl dağıtılacağı gösterilmektedir.

Karma bulut çözümünü kullanarak, özel bir bulutun uyumluluk avantajlarını genel bulutun ölçeklenebilirliği ile birleştirebilirsiniz. Ayrıca, geliştiricileriniz Microsoft Developer ekosisteminden faydalanabilir ve yeteneklerini buluta ve şirket içi ortamlara uygulayabilir.

## <a name="overview-and-assumptions"></a>Genel bakış ve varsayımlar

Geliştiricilerin aynı Web uygulamasını ortak bir buluta ve özel buluta dağıtmasına imkan tanıyan bir iş akışı ayarlamak için bu öğreticiyi izleyin. Bu uygulama, özel bulutta barındırılan internet 'e ait olmayan yönlendirilebilir ağa erişebilir. Bu Web uygulamaları izlenir ve trafikte ani bir artış olduğunda, bir program trafiği genel buluta yönlendirmek için DNS kayıtlarını değiştirir. Trafik, ani bir düzeye düştüğünde trafik, özel buluta geri yönlendirilir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> - Karma bağlantılı SQL Server veritabanı sunucusu dağıtın.
> - Küresel Azure 'da bir Web uygulamasını karma ağa bağlayın.
> - DNS 'i platformlar arası ölçekleme için yapılandırın.
> - Platformlar arası ölçeklendirme için SSL sertifikaları yapılandırın.
> - Web uygulamasını yapılandırın ve dağıtın.
> - Traffic Manager profili oluşturun ve bulut tabanlı ölçeklendirme için yapılandırın.
> - Artan trafik için Application Insights izlemeyi ve uyarı ayarlamayı ayarlayın.
> - Genel Azure ve Azure Stack arasında otomatik trafik geçişini yapılandırın.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack, bulut bilgi işlemin çevikliğini ve yeniliklerini şirket içi ortamınıza getirerek, karma uygulamaları her yerde oluşturup dağıtmanıza imkan tanıyan tek karma bulutu etkinleştirir.  
> 
> [Karma uygulamalar Için tasarım konuları](azure-stack-edge-pattern-overview.md) , karma uygulamalar tasarlamak, dağıtmak ve çalıştırmak için yazılım kalitesine (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) göre önemli noktalar inceler. Tasarım konuları karma uygulama tasarımını iyileştirirken, üretim ortamlarındaki zorlukları en aza indirmeyle ilgili olarak size yardımcı olur.

### <a name="assumptions"></a>Çoğu

Bu öğreticide, küresel Azure ve Azure Stack temel bir bilginiz olduğunu varsaymaktadır. Öğreticiyi başlatmadan önce daha fazla bilgi edinmek istiyorsanız, şu makaleleri gözden geçirin:

 - [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack temel kavramları](../operator/azure-stack-overview.md)

Bu öğreticide Ayrıca bir Azure aboneliğiniz olduğunu varsaymaktadır. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Bu çözüme başlamadan önce, aşağıdaki gereksinimleri karşıladığınızdan emin olun:

- Bir Azure Stack Geliştirme Seti (ASDK) veya Azure Stack tümleşik bir sistemde bir abonelik. Azure Stack Geliştirme Seti dağıtmak için, [yükleyiciyi kullanarak ASDK dağıtma](../asdk/asdk-install.md)bölümündeki yönergeleri izleyin.
- Azure Stack yüklemenizde aşağıdakiler yüklü olmalıdır:
  - Azure App Service. Ortamınızdaki Azure App Service dağıtmak ve yapılandırmak için Azure Stack Işleçle çalışın. Bu öğreticide, App Service için en az bir (1) ayrılmış çalışan rolü olması gerekir.
  - Bir Windows Server 2016 görüntüsü.
  - Microsoft SQL Server bir görüntüyle Windows Server 2016.
  - Uygun planlar ve teklifler.
  - Web uygulamanız için bir etki alanı adı. Bir etki alanı adınız yoksa, GoDaddy, BlueHost ve Inmotion gibi bir etki alanı sağlayıcısından bir tane satın alabilirsiniz.
- Letiniz gibi güvenilir bir sertifika yetkilisinden etki alanınız için bir SSL sertifikası.
- SQL Server veritabanıyla iletişim kuran ve Application Insights destekleyen bir Web uygulaması. [Dotnetcore-SQLDB-öğreticisi](https://github.com/Azure-Samples/dotnetcore-sqldb-tutorial) örnek uygulamasını GitHub 'dan indirebilirsiniz.
- Azure sanal ağı ile Azure Stack sanal ağı arasındaki karma ağ. Ayrıntılı yönergeler için bkz. [Azure ile karma bulut bağlantısını yapılandırma ve Azure Stack](azure-stack-solution-hybrid-connectivity.md).

- Azure Stack üzerinde özel yapı aracısına sahip karma bir sürekli tümleştirme/sürekli dağıtım (CI/CD) işlem hattı. Ayrıntılı yönergeler için bkz. [Azure ve Azure Stack uygulamalarla karma bulut kimliğini yapılandırma](azure-stack-solution-hybrid-identity.md).

## <a name="deploy-a-hybrid-connected-sql-server-database-server"></a>Karma bağlantılı SQL Server veritabanı sunucusu dağıtma

1. Azure Stack Kullanıcı portalında oturum açın.

2. **Panoda** **Market**' i seçin.

    ![Azure Stack Market](media/azure-stack-solution-hybrid-cloud/image1.png)

3. **Market**' te **işlem**' i seçin ve **daha sonra diğer**' i seçin. **Daha fazla bilgi**için **ücretsiz SQL Server lisansını seçin: Windows Server** görüntüsünde 2017 SQL Server geliştirici.

    ![Bir sanal makine görüntüsü seçin](media/azure-stack-solution-hybrid-cloud/image2.png)

4. **Ücretsiz SQL Server Lisansı: Windows Server**'da 2017 SQL Server Geliştirici, **Oluştur**' u seçin.

5. **Temel ayarları yapılandırma > temel bilgiler**, sanal makıne (VM) Için bir **ad** , SQL Server SA için BIR **Kullanıcı adı** ve SA için bir **parola** sağlayın.  **Abonelik** açılan listesinden, dağıtmakta olduğunuz aboneliği seçin. **Kaynak grubu**için **var olanı seç** ' i kullanın ve sanal makineyi Azure Stack Web uygulamanızla aynı kaynak grubuna yerleştirin.

    ![VM için temel ayarları yapılandırma](media/azure-stack-solution-hybrid-cloud/image3.png)

6. **Boyut**bölümünde VM 'niz için bir boyut seçin. Bu öğretici için A2_Standard veya DS2_V2_Standard önerilir.

7. **Ayarlar > isteğe bağlı özellikleri yapılandırmak**için aşağıdaki ayarları yapılandırın:

   - **Depolama hesabı**: Gerekiyorsa yeni bir hesap oluşturun.
   - **Sanal ağ**:

     > [!Important]  
     > SQL Server VM VPN ağ geçitleri ile aynı sanal ağda dağıtıldığından emin olun.

   - **Genel IP adresi**: Varsayılan ayarları kullanın.
   - **Ağ güvenlik grubu**: (NSG). Yeni bir NSG oluşturun.
   - **Uzantılar ve izleme**: Varsayılan ayarları koruyun.
   - **Tanılama depolama hesabı**: Gerekiyorsa yeni bir hesap oluşturun.
   - Yapılandırmanızı kaydetmek için **Tamam ' ı** seçin.

     ![İsteğe bağlı özellikleri yapılandır](media/azure-stack-solution-hybrid-cloud/image4.png)

8. **SQL Server ayarları**altında aşağıdaki ayarları yapılandırın:
   - **SQL bağlantısı**için **Genel (Internet)** seçeneğini belirleyin.
   - **Bağlantı noktası**için, varsayılan **1433**' ı koruyun.
   - **SQL kimlik doğrulaması**için **Etkinleştir**' i seçin.

     > [!Note]  
     > SQL kimlik doğrulamasını etkinleştirdiğinizde, **temel**bilgiler ' de yapılandırdığınız "SQLAdmin" bilgileri ile otomatik olarak doldurulur.

   - Ayarların geri kalanı için varsayılan değerleri tutun. **Tamam**’ı seçin.

     ![SQL Server ayarlarını yapılandırma](media/azure-stack-solution-hybrid-cloud/image5.png)

9. **Özet**sayfasında, sanal makine yapılandırmasını gözden geçirin ve ardından dağıtımı başlatmak için **Tamam** ' ı seçin.

    ![Yapılandırma Özeti](media/azure-stack-solution-hybrid-cloud/image6.png)

10. Yeni VM 'yi oluşturmak biraz zaman alır. **Sanal makinelerde**VM 'nizin durumunu görüntüleyebilirsiniz.

    ![Sanal makineler](media/azure-stack-solution-hybrid-cloud/image7.png)

## <a name="create-web-apps-in-azure-and-azure-stack"></a>Azure 'da ve Azure Stack Web uygulamaları oluşturun

Azure App Service, bir Web uygulamasını çalıştırmayı ve yönetmeyi basitleştirir. Azure Stack Azure ile tutarlı olduğundan, App Service her iki ortamda da çalıştırılabilir. Uygulamanızı barındırmak için App Service kullanacaksınız.

### <a name="create-web-apps"></a>Web uygulamaları oluşturma

1. Azure 'da [App Service planını yönetme](https://docs.microsoft.com/azure/app-service/app-service-plan-manage#create-an-app-service-plan)bölümündeki yönergeleri izleyerek Azure 'da bir Web uygulaması oluşturun. Web uygulamasını karma ağınızla aynı aboneliğe ve kaynak grubuna yerleştirdiğinizden emin olun.

2. Azure Stack önceki adımını (1) yineleyin.

### <a name="add-route-for-azure-stack"></a>Azure Stack için rota Ekle

Azure Stack üzerindeki App Service, kullanıcıların uygulamanıza erişmesine izin vermek için genel İnternet 'ten yönlendirilebilir olmalıdır. Azure Stack internet 'ten erişilebiliyorsa, Azure Stack Web uygulaması için genel kullanıma yönelik IP adresini veya URL 'YI bir yere göz önüne alın.

Bir ASDK kullanıyorsanız, sanal ortam dışında App Service göstermek için [statik BIR NAT eşlemesi yapılandırabilirsiniz](../operator/azure-stack-create-vpn-connection-one-node.md#configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal) .

### <a name="connect-a-web-app-in-azure-to-a-hybrid-network"></a>Azure 'da bir Web uygulamasını karma ağa bağlama

Azure 'daki Web ön ucu ve Azure Stack SQL Server veritabanı arasında bağlantı sağlamak için, Web uygulamasının Azure ile Azure Stack arasında karma ağa bağlanması gerekir. Bağlantıyı etkinleştirmek için şunları yapmanız gerekir:

- Noktadan siteye bağlantıyı yapılandırın.
- Web uygulamasını yapılandırın.
- Azure Stack yerel ağ geçidini değiştirin.

### <a name="configure-the-azure-virtual-network-for-point-to-site-connectivity"></a>Noktadan siteye bağlantı için Azure sanal ağını yapılandırma

Karma ağın Azure tarafındaki sanal ağ geçidi, Azure App Service ile tümleştirilecek Noktadan siteye bağlantılara izin vermelidir.

1. Azure 'da sanal ağ geçidi sayfasına gidin. **Ayarlar**' ın altında, **Noktadan siteye yapılandırma**' yı seçin.

    ![Noktadan siteye seçeneği](media/azure-stack-solution-hybrid-cloud/image8.png)

2. Noktadan siteye yapılandırmak için **Şimdi Yapılandır** ' ı seçin.

    ![Noktadan siteye yapılandırmayı Başlat](media/azure-stack-solution-hybrid-cloud/image9.png)

3. **Noktadan siteye** yapılandırma sayfasında, **adres havuzunda**kullanmak istediğiniz özel IP adresi aralığını girin.

   > [!Note]  
   > Belirttiğiniz aralığın, karma ağın genel Azure veya Azure Stack bileşenlerinde bulunan alt ağlar tarafından zaten kullanılmakta olan adres aralıklarıyla çakışmadığından emin olun.

   **Tünel türü**altında, **IKEv2 VPN**seçeneğinin işaretini kaldırın. Noktadan siteye yapılandırmayı sona **kazanmak Için kaydet** ' i seçin.

   ![Noktadan siteye ayarları](media/azure-stack-solution-hybrid-cloud/image10.png)

### <a name="integrate-the-azure-app-service-app-with-the-hybrid-network"></a>Azure App Service uygulamasını karma ağla tümleştirme

1. Uygulamayı Azure VNet 'e bağlamak için [Ağ Geçidi gerekli VNET tümleştirmesi](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet#gateway-required-vnet-integration)bölümündeki yönergeleri izleyin.

2. Web uygulamasını barındıran App Service planına yönelik **Ayarlar** ' a gidin. **Ayarlar**' da **ağ**' ı seçin.

    ![Ağı yapılandırma](media/azure-stack-solution-hybrid-cloud/image11.png)

3. **Sanal ağ tümleştirmesi**, **yönetmek Için buraya tıklayın ' ı**seçin.

    ![VNET tümleştirmesini yönetme](media/azure-stack-solution-hybrid-cloud/image12.png)

4. Yapılandırmak istediğiniz VNET 'i seçin. **VNET 'e yönlendirilen IP adresleri**altında, Azure vnet, Azure Stack VNET ve Noktadan siteye adres ALANLARı için IP adresi aralığını girin. Bu ayarları doğrulamak ve kaydetmek için **Kaydet** ' i seçin.

    ![Rota için IP adresi aralıkları](media/azure-stack-solution-hybrid-cloud/image13.png)

App Service Azure sanal ağları ile tümleştirme hakkında daha fazla bilgi edinmek için bkz. [uygulamanızı bir Azure sanal ağı Ile tümleştirme](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet).

### <a name="configure-the-azure-stack-virtual-network"></a>Azure Stack sanal ağını yapılandırma

Azure Stack sanal ağındaki yerel ağ geçidinin, trafiği App Service Noktadan siteye adres aralığından yönlendirmek üzere yapılandırılması gerekir.

1. Azure Stack, **yerel ağ geçidi**' ne gidin. **Ayarlar** altında **Yapılandırma**'yı seçin.

    ![Ağ Geçidi yapılandırma seçeneği](media/azure-stack-solution-hybrid-cloud/image14.png)

2. **Adres alanı**' nda, Azure 'daki sanal ağ geçidi için Noktadan siteye adres aralığını girin.

    ![Noktadan siteye adres alanı](media/azure-stack-solution-hybrid-cloud/image15.png)

3. Yapılandırmayı doğrulamak ve kaydetmek için **Kaydet** ' i seçin.

## <a name="configure-dns-for-cross-cloud-scaling"></a>DNS 'i platformlar arası ölçeklendirme için yapılandırma

Kullanıcılar, platformlar arası uygulamalar için düzgün şekilde yapılandırılarak, Web uygulamanızın Genel Azure ve Azure Stack örneklerine erişebilirler. Bu öğreticinin DNS yapılandırması, Yük arttıkça veya azaldıkça Azure Traffic Manager trafiği yönlendirmeye de olanak tanır.

Bu öğretici, DNS 'yi yönetmek için Azure DNS kullanır, çünkü App Service etki alanları çalışmıyor.

### <a name="create-subdomains"></a>Alt etki alanları oluşturma

Traffic Manager DNS CNAMEs ' i kullandığından, trafiği uç noktalara doğru bir şekilde yönlendirmek için bir alt etki alanı gerekir. DNS kayıtları ve etki alanı eşlemesi hakkında daha fazla bilgi için bkz. [Traffic Manager etki alanlarını eşleme](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager-custom-domain-name).

Azure uç noktası için kullanıcıların Web uygulamanıza erişmek için kullanabileceği bir alt etki alanı oluşturacaksınız. Bu öğretici için **app.Northwind.com**kullanabilir, ancak bu değeri kendi etki alanınızı temel alarak özelleştirmeniz gerekir.

Ayrıca, Azure Stack uç noktası için bir kayıt içeren bir alt etki alanı oluşturmanız gerekecektir. **Azurestack.Northwind.com**kullanabilirsiniz.

### <a name="configure-a-custom-domain-in-azure"></a>Azure 'da özel bir etki alanı yapılandırma

1. [CNAME 'i Azure App Service ile eşleyerek](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record) **app.Northwind.com** ana bilgisayar adını Azure Web uygulamasına ekleyin.

### <a name="configure-custom-domains-in-azure-stack"></a>Azure Stack özel etki alanlarını yapılandırma

1. [Bir kaydı Azure App Service ile eşleyerek](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-an-a-record)Azure Stack **Web ana bilgisayar** adını ekleyin. App Service uygulaması için Internet yönlendirilebilir IP adresini kullanın.

2. [CNAME 'i Azure App Service eşleyerek](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record)Azure Stack **Web ana bilgisayar** adını Web uygulamasına ekleyin. Önceki adımda yapılandırdığınız ana bilgisayar adını (1) CNAME için hedef olarak kullanın.

## <a name="configure-ssl-certificates-for-cross-cloud-scaling"></a>Platformlar arası ölçeklendirme için SSL sertifikalarını yapılandırma

Web uygulamanız tarafından toplanan gizli verilerin, SQL veritabanında depolanan ve ' de yapılan aktarım sırasında güvenli olduğundan emin olmanız önemlidir.

Azure ve Azure Stack Web uygulamalarınızı tüm gelen trafik için SSL sertifikaları kullanacak şekilde yapılandıracaksınız.

### <a name="add-ssl-to-azure-and-azure-stack"></a>Azure ve Azure Stack SSL ekleme

Azure 'a SSL eklemek için:

1. Aldığınız SSL sertifikasının oluşturduğunuz alt etki alanı için geçerli olduğundan emin olun. (Joker karakter sertifikaları kullanmak normaldir.)

2. Azure 'da, **Web uygulamanızı hazırlama** **ve** [var olan özel bir SSL sertifikasını Azure 'a](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) bağlama bölümündeki yönergeleri izleyin Web Apps makalesine gidin. **SSL türü**olarak **SNı tabanlı SSL** ' yi seçin.

3. Tüm trafiği HTTPS bağlantı noktasına yönlendir. [Var olan bir özel SSL sertifikası 'nı Web Apps Azure 'A bağlama](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-ssl) makalesindeki **https 'yi zorla** bölümünde bulunan yönergeleri izleyin.

Azure Stack SSL eklemek için:

- Azure için kullandığınız 1-3 adımlarını yineleyin.

## <a name="configure-and-deploy-the-web-app"></a>Web uygulamasını yapılandırma ve dağıtma

Uygulama kodunu Telemetriyi doğru Application Insights örneğine rapor verecek şekilde yapılandıracaksınız ve Web uygulamalarını doğru bağlantı dizelerine yapılandıracaksınız. Application Insights hakkında daha fazla bilgi edinmek için bkz. [Application Insights nedir?](https://docs.microsoft.com/azure/application-insights/app-insights-overview)

### <a name="add-application-insights"></a>Application Insights Ekleme

1. Web uygulamanızı Microsoft Visual Studio açın.

2. Web trafiği arttıkça veya azaldıkça, Application Insights tarafından uyarı oluşturmak için kullanılan Telemetriyi iletmek üzere projenize [Application Insights ekleyin](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core#enable-client-side-telemetry-for-web-applications) .

### <a name="configure-dynamic-connection-strings"></a>Dinamik bağlantı dizelerini yapılandırma

Web uygulamasının her örneği SQL veritabanına bağlanmak için farklı bir yöntem kullanacaktır. Azure 'daki uygulama SQL Server sanal makinenin (VM) özel IP adresini kullanır ve Azure Stack içindeki uygulama SQL Server VM genel IP adresini kullanır.

> [!Note]  
> Azure Stack tümleşik bir sistemde, genel IP adresi internet yönlendirilebilir olmamalıdır. Bir Azure Stack Geliştirme Seti (ASDK) üzerinde genel IP adresi, ASDK dışında yönlendirilebilir değildir.

App Service ortam değişkenlerini, uygulamanın her örneğine farklı bir bağlantı dizesi geçirmek için kullanabilirsiniz.

1. Uygulamayı Visual Studio 'da açın.

2. Startup.cs açın ve aşağıdaki kod bloğunu bulun:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
    ```

3. Önceki kod bloğunu, *appSettings. JSON* dosyasında tanımlı bir bağlantı dizesi kullanan aşağıdaki kodla değiştirin:

    ```C#
    services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
     // Automatically perform database migration
     services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
    ```

### <a name="configure-app-service-app-settings"></a>App Service uygulama ayarlarını yapılandırma

1. Azure ve Azure Stack için bağlantı dizeleri oluşturun. Dizeler, kullanılan IP adresleri dışında, aynı olmalıdır.

2. Azure ve Azure Stack ' de, ad içinde ön ek olarak kullanarak `SQLCONNSTR\_` , Web uygulamasında uygun bağlantı dizesini [uygulama ayarı olarak](https://docs.microsoft.com/azure/app-service/web-sites-configure) ekleyin.

3. Web uygulaması ayarlarını **kaydedin** ve uygulamayı yeniden başlatın.

## <a name="enable-automatic-scaling-in-global-azure"></a>Küresel Azure 'da otomatik ölçeklendirmeyi etkinleştirme

Web uygulamanızı bir App Service ortamında oluşturduğunuzda, tek bir örnekle başlar. Uygulamanız için daha fazla işlem kaynağı sağlamak üzere örnekleri eklemek için otomatik olarak ölçeği değiştirebilirsiniz. Benzer şekilde, uygulamanızda otomatik olarak ölçeklendirme yapabilir ve uygulamanızın ihtiyaç duyacağı örnek sayısını azaltabilirsiniz.

> [!Note]  
> Ölçek Genişletme ve ölçekleme yapılandırmak için bir App Service planına sahip olmanız gerekir. Bir planınız yoksa, sonraki adımları başlatmadan önce bir tane oluşturun.

### <a name="enable-automatic-scale-out"></a>Otomatik ölçeklendirmeyi etkinleştir

1. Azure 'da, genişletmek istediğiniz siteler için App Service planı bulun ve ardından **genişleme (App Service planı)** öğesini seçin.

    ![Ölçeği genişletme](media/azure-stack-solution-hybrid-cloud/image16.png)

2. **Otomatik ölçeklendirmeyi etkinleştir**' i seçin.

    ![Otomatik ölçeklendirmeyi etkinleştir](media/azure-stack-solution-hybrid-cloud/image17.png)

3. **Otomatik ölçeklendirme ayarı adı**için bir ad girin. **Varsayılan** otomatik ölçeklendirme kuralı için, **ölçüm temelinde ölçek**' i seçin. **Örnek sınırlarını** en az olarak **ayarlayın: 1** ,**en fazla: 10** ve**varsayılan: 1**.

    ![Otomatik ölçeklendirmeyi Yapılandır](media/azure-stack-solution-hybrid-cloud/image18.png)

4. **+ Kural Ekle**' yi seçin.

5. **Ölçüm kaynağı**' nda **geçerli kaynak**' ı seçin. Kural için aşağıdaki ölçütleri ve eylemleri kullanın.

**Ölçütler**

1. Zaman toplama altında **Ortalama** **' ı** seçin.

2. **Ölçüm adı**altında **CPU yüzdesi**' ni seçin.

3. **İşleç**altında, **büyüktür**' i seçin.

   - **Eşiği** **50**olarak ayarlayın.
   - **Süreyi** **10**olarak ayarlayın.

**Eylem**

1. **İşlem**altında, **sayıyı göre artır**' ı seçin.

2. **Örnek sayısını** **2**olarak ayarlayın.

3. **Cool tuşuna** **5**olarak ayarlayın.

4. **Add (Ekle)** seçeneğini belirleyin.

5. **+ Kural Ekle**' yi seçin.

6. **Ölçüm kaynağı**' nda **geçerli kaynak** ' ı seçin.

   > [!Note]  
   > Geçerli kaynak App Service planınızın adını/GUID 'sini içerecektir ve **kaynak türü** ve **kaynak** açılan listeleri kullanılamaz.

### <a name="enable-automatic-scale-in"></a>Otomatik ölçeklendirmeyi etkinleştir

Trafik azaldıkça Azure Web uygulaması, maliyetleri azaltmak için etkin örnek sayısını otomatik olarak azaltabilir. Bu eylem, ölçeklendirmeden daha az agresif ve uygulama kullanıcıları üzerindeki etkiyi en aza indirir.

1. **Varsayılan** genişleme durumuna gidin, **+ Kural Ekle**' yi seçin. Kural için aşağıdaki ölçütleri ve eylemleri kullanın.

**Ölçütler**

1. Zaman toplama altında **Ortalama** **' ı** seçin.

2. **Ölçüm adı**altında **CPU yüzdesi**' ni seçin.

3. **İşleç**altında küçüktür ' **i seçin.**

   - **Eşiği** **30**olarak ayarlayın.
   - **Süreyi** **10**olarak ayarlayın.

**Eylem**

1. **İşlem**altında **sayıyı göre azalt**' ı seçin.

   - **Örnek sayısını** **1**olarak ayarlayın.
   - **Cool tuşuna** **5**olarak ayarlayın.

2. **Add (Ekle)** seçeneğini belirleyin.

## <a name="create-a-traffic-manager-profile-and-configure-cross-cloud-scaling"></a>Traffic Manager profili oluşturma ve platformlar arası ölçeklendirmeyi yapılandırma

Azure 'da bir Traffic Manager profili oluşturun ve sonra da platformlar arası ölçeklendirmeyi etkinleştirmek için uç noktaları yapılandırın.

### <a name="create-traffic-manager-profile"></a>Traffic Manager profili oluştur

1. Seçin **kaynak Oluştur**.
2. **Ağ**' ı seçin.
3. **Traffic Manager profil** ' i seçin ve aşağıdaki ayarları yapılandırın:

   - **Ad**alanına profiliniz için bir ad girin. Bu ad trafficmanager.net bölgesinde benzersiz **olmalıdır** ve yenı bir DNS adı oluşturmak için kullanılır (örneğin, northwindstore.trafficmanager.net).
   - **Yönlendirme yöntemi**için **ağırlıklı**' ı seçin.
   - **Abonelik**için, bu profili oluşturmak istediğiniz aboneliği seçin.
   - **Kaynak grubu**' nda, bu profil için yeni bir kaynak grubu oluşturun.
   - **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılan Traffic Manager profilini etkilemez.

4. **Oluştur**’u seçin.

    ![Traffic Manager profili oluştur](media/azure-stack-solution-hybrid-cloud/image19.png)

   Traffic Manager profilinizin genel dağıtımı tamamlandığında, altında oluşturduğunuz kaynak grubu için kaynak listesinde gösterilir.

### <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

1. Oluşturduğunuz Traffic Manager profilini arayın. Profil için kaynak grubuna gezindiyseniz, profili seçin.

2. **Traffic Manager profilinde**, **Ayarlar**altında **uç noktalar**' ı seçin.

3. **Add (Ekle)** seçeneğini belirleyin.

4. **Uç nokta Ekle**' de Azure Stack için aşağıdaki ayarları kullanın:

   - **Tür**için **dış uç nokta**' ı seçin.
   - Uç nokta için bir **ad** girin.
   - **Tam etki alanı adı (FQDN) veya IP**için, Azure Stack Web uygulamanızın dış URL 'sini girin.
   - **Ağırlık**için varsayılan değer olan **1**' i tutun. Bu ağırlık, sağlıklı ise bu uç noktaya giden tüm trafik ile sonuçlanır.
   - **Eklemeyi devre dışı** bırak işareti kaldırılmış olarak bırakın.

5. Azure Stack uç noktasını kaydetmek için **Tamam ' ı** seçin.

Sonraki Azure uç noktasını yapılandıracaksınız.

1. **Traffic Manager profilinde** **uç noktalar**' ı seçin.
2. **+ Ekle**' yi seçin.
3. **Uç nokta Ekle**sayfasında Azure için aşağıdaki ayarları kullanın:

   - **Tür**için **Azure uç noktası**' nı seçin.
   - Uç nokta için bir **ad** girin.
   - **Hedef kaynak türü**için **App Service**seçin.
   - **Hedef kaynak**için, aynı abonelikte Web Apps listesini görmek üzere **bir App Service seçin** öğesini seçin.
   - **Kaynak** bölümünde ilk uç nokta olarak kullanmak istediğiniz uygulama hizmetini seçin.
   - **Ağırlık**için **2**' yi seçin. Bu ayar, birincil uç noktanın sağlıksız olması durumunda ya da tetiklendiğinde trafiği yeniden yönlendiren bir kuralınız/uyarıınız varsa, bu uç noktaya giden tüm trafiğe neden olur.
   - **Eklemeyi devre dışı** bırak işareti kaldırılmış olarak bırakın.

4. Azure uç noktasını kaydetmek için **Tamam ' ı** seçin.

Her iki uç nokta yapılandırıldıktan sonra, **uç noktalar**' ı seçerken **Traffic Manager profilde** listelenir. Aşağıdaki ekran yakalamadaki örnek, her biri için durum ve yapılandırma bilgileriyle iki uç nokta gösterir.

![Uç Noktalar](media/azure-stack-solution-hybrid-cloud/image20.png)

## <a name="set-up-application-insights-monitoring-and-alerting"></a>Application Insights izlemeyi ve uyarı ayarlamayı ayarlama

Azure Application Insights, uygulamanızı izlemenizi ve yapılandırdığınız koşullara göre uyarı göndermenizi sağlar. Bazı örnekler şunlardır: uygulama kullanılamıyor, hatalarla karşılaşıyor veya performans sorunlarını gösteriyor.

Uyarı oluşturmak için Application Insights ölçümleri kullanacaksınız. Bu uyarılar tetiklenmesi durumunda, Web uygulamanızın örneği ölçeği genişletmek için Azure Stack otomatik olarak Azure 'a geçer ve sonra ölçeklendirmek için Azure Stack geri dönecek.

### <a name="create-an-alert-from-metrics"></a>Ölçülerden uyarı oluşturma

Bu öğretici için kaynak grubuna gidin ve **Application Insights**açmak için Application Insights örneğini seçin.

![Application Insights](media/azure-stack-solution-hybrid-cloud/image21.png)

Bu görünümü, bir ölçek genişletme uyarısı ve uyarı ölçeğini oluşturmak için kullanacaksınız.

### <a name="create-the-scale-out-alert"></a>Ölçek Genişletme uyarısını oluşturma

1. **Yapılandır**altında **Uyarılar (klasik)** öğesini seçin.
2. **Ölçüm uyarısı Ekle (klasik)** seçeneğini belirleyin.
3. **Kural Ekle**' de, aşağıdaki ayarları yapılandırın:

   - **Ad**Için **Azure buluta veri bloğu**girin.
   - **Açıklama** isteğe bağlıdır.
   - **Kaynak** > **uyarısı**' nın altında, **ölçümler**' i seçin.
   - **Ölçüt**altında, aboneliğinizi, Traffic Manager profilinizin kaynak grubunu ve kaynak için Traffic Manager profilinin adını seçin.

4. **Ölçüm**Için **istek hızı**' nı seçin.
5. **Koşul**Için **şundan büyüktür**' i seçin.
6. **Eşik**için **2**girin.
7. **Süre**için **son 5 dakikayı**seçin.
8. **Bildirim**altında:
   - **E-posta sahipleri, katkıda bulunanlar ve okuyucular**için onay kutusunu işaretleyin.
   - **Ek yönetici e-postaları**için e-posta adresinizi girin.

9. Menü çubuğunda **Kaydet**' i seçin.

### <a name="create-the-scale-in-alert"></a>Uyarı içinde ölçek oluşturma

1. **Yapılandır**altında **Uyarılar (klasik)** öğesini seçin.
2. **Ölçüm uyarısı Ekle (klasik)** seçeneğini belirleyin.
3. **Kural Ekle**' de, aşağıdaki ayarları yapılandırın:

   - **Ad**Için **Azure Stack ölçeği geri ' ye**girin.
   - **Açıklama** isteğe bağlıdır.
   - **Kaynak** > **uyarısı**' nın altında, **ölçümler**' i seçin.
   - **Ölçüt**altında, aboneliğinizi, Traffic Manager profilinizin kaynak grubunu ve kaynak için Traffic Manager profilinin adını seçin.

4. **Ölçüm**Için **istek hızı**' nı seçin.
5. **Koşul**için küçüktür ' **i seçin.**
6. **Eşik**için **2**girin.
7. **Süre**için **son 5 dakikayı**seçin.
8. **Bildirim**altında:
   - **E-posta sahipleri, katkıda bulunanlar ve okuyucular**için onay kutusunu işaretleyin.
   - **Ek yönetici e-postaları**için e-posta adresinizi girin.

9. Menü çubuğunda **Kaydet**' i seçin.

Aşağıdaki ekran yakalama, ölçeği genişletme ve ölçekleme için uyarıları gösterir.

   ![Uyarılar (klasik)](media/azure-stack-solution-hybrid-cloud/image22.png)

## <a name="redirect-traffic-between-azure-and-azure-stack"></a>Azure ile Azure Stack arasında trafiği yeniden yönlendirme

Azure ile Azure Stack arasında Web uygulaması trafiği geçişi için el ile veya otomatik geçişi yapılandırabilirsiniz.

### <a name="configure-manual-switching-between-azure-and-azure-stack"></a>Azure ile Azure Stack arasında el ile geçiş yapılandırma

Web siteniz yapılandırdığınız eşiklere ulaştığında bir uyarı alırsınız. Trafiği Azure 'a el ile yönlendirmek için aşağıdaki adımları kullanın.

1. Azure portal Traffic Manager profilinizi seçin.

    ![Traffic Manager uç noktaları](media/azure-stack-solution-hybrid-cloud/image20.png)

2. **Uç noktaları**seçin.
3. **Azure uç noktasını**seçin.
4. **Durum**altında, **etkin**' i seçin ve ardından **Kaydet**' i seçin.

    ![Azure uç noktasını etkinleştir](media/azure-stack-solution-hybrid-cloud/image23.png)

5. Traffic Manager profilinin **uç noktalarında** **dış uç nokta**' ı seçin.
6. **Durum**altında **devre dışı**' yı seçin ve ardından **Kaydet**' i seçin.

    ![Azure Stack uç noktasını devre dışı bırak](media/azure-stack-solution-hybrid-cloud/image24.png)

Uç noktalar yapılandırıldıktan sonra, uygulama trafiği Azure Stack Web uygulaması yerine Azure genişleme Web uygulamanıza gider.

 ![Uç noktalar değişti](media/azure-stack-solution-hybrid-cloud/image25.png)

Akışı Azure Stack geri çevirmek için önceki adımları kullanın:

- Azure Stack uç noktasını etkinleştirin.
- Azure uç noktasını devre dışı bırakın.

### <a name="configure-automatic-switching-between-azure-and-azure-stack"></a>Azure ile Azure Stack arasında otomatik geçiş yapılandırma

Uygulamanız Azure Işlevleri tarafından sunulan [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir ortamda çalıştırılıyorsa Application Insights izlemeyi da kullanabilirsiniz.

Bu senaryoda, bir işlev uygulaması çağıran bir Web kancasını kullanmak için Application Insights yapılandırabilirsiniz. Bu uygulama, bir uyarıya yanıt olarak bir uç noktayı otomatik olarak sağlar veya devre dışı bırakır.

Otomatik trafik geçişini yapılandırmak için aşağıdaki adımları kılavuz olarak kullanın.

1. Bir Azure Işlev uygulaması oluşturun.
2. HTTP ile tetiklenen bir işlev oluşturun.
3. Kaynak Yöneticisi, Web Apps ve Traffic Manager için Azure SDK 'larını içeri aktarın.
4. Kodu şu şekilde geliştirin:

   - Azure aboneliğinizde kimlik doğrulaması yapın.
   - Traffic Manager uç noktalarına trafiği Azure 'a veya Azure Stack yönlendirmek için geçiş yapan bir parametre kullanın.

5. Kodunuzu kaydedin ve işlev uygulamasının URL 'sini, Application Insights uyarı kuralı ayarlarının **Web kancası** bölümüne uygun parametrelerle ekleyin.
6. Application Insights bir uyarı tetiklendiğinde trafik otomatik olarak yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
