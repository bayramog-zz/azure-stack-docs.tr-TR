---
title: Azure Stack App Service dağıtma | Microsoft Docs
description: Azure Stack App Service dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 7088a001fab8e3e618da4be8107f5a5f5a22373f
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271769"
---
# <a name="deploy-app-service-in-azure-stack"></a>Azure Stack App Service dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede Azure Stack App Service nasıl dağıtılacağı açıklanır.

> [!IMPORTANT]
> 1907 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,7 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

Kullanıcılarınıza Web ve API uygulamaları oluşturma özelliği verebilirsiniz. Kullanıcıların bu uygulamaları oluşturmasına izin vermek için şunları yapmanız gerekir:

- Bu makalede açıklanan adımları kullanarak Azure Stack dağıtımınıza [App Service kaynak sağlayıcısı](azure-stack-app-service-overview.md) ekleyin.
- App Service kaynak sağlayıcısı 'nı yükledikten sonra, bu sürümü tekliflerinizi ve planlarınıza dahil edebilirsiniz. Kullanıcılar daha sonra hizmeti almak ve uygulama oluşturmaya başlamak için abone olabilir.

> [!IMPORTANT]
> Kaynak sağlayıcısı yükleyicisini çalıştırmadan önce, [başlamadan önce](azure-stack-app-service-before-you-get-started.md) ' de kılavuzluk ve 1,7 sürümüyle birlikte gelen [sürüm notlarını](azure-stack-app-service-release-notes-update-seven.md) okuduğunuzdan emin olun. Bu içeriği okumak, yeni işlevleri, düzeltmeleri ve dağıtımınızı etkileyebilecek bilinen sorunları öğrenmenize yardımcı olur.

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyicisini çalıştırma

App Service kaynak sağlayıcısını yüklemek en az bir saat sürer. Gereken sürenin uzunluğu, kaç tane rol örneğine dağıtım yapmanız gerektiğini gösterir. Dağıtım sırasında, yükleyici aşağıdaki görevleri çalıştırır:

- Belirtilen Azure Stack depolama hesabında bir blob kapsayıcısı oluşturun.
- App Service için bir DNS bölgesi ve girişleri oluşturun.
- App Service kaynak sağlayıcısını kaydedin.
- App Service galeri öğelerini kaydedin.

App Service kaynak sağlayıcısını dağıtmak için aşağıdaki adımları izleyin:

1. Appservice. exe ' yi Azure Stack Yöneticisi Azure Kaynak Yönetimi uç noktasına erişebilen bir bilgisayardan yönetici olarak çalıştırın.

2. App Service Dağıt ' ı seçin **veya en son sürüme yükseltin**.

    ![App Service yükleyicisi][1]

3. Microsoft yazılımı lisans koşulları 'nı gözden geçirip kabul edin ve ardından **İleri**' yi seçin.

4. Üçüncü taraf lisans koşullarını gözden geçirip kabul edin ve ardından **İleri**' yi seçin.

5. App Service bulut yapılandırması bilgilerinin doğru olduğundan emin olun. ASDK dağıtımı sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri kabul edebilirsiniz. Ancak, ASDK 'yi dağıtırken veya Azure Stack tümleşik bir sisteme dağıtırken seçenekleri özelleştirdiyseniz, farkları yansıtmak için bu penceredeki değerleri düzenlemeniz gerekir.

   Örneğin, mycloud.com etki alanı sonekini kullanırsanız, Azure Stack kiracı Azure Resource Manager uç noktasının yönetimi olarak değiştirilmesi gerekir. &lt;Region&gt;. MyCloud.com. Bu ayarları gözden geçirin ve sonra ayarları kaydetmek için **İleri** ' yi seçin.

   ![App Service yükleyicisi][2]

6. Sonraki App Service yükleyicisi sayfasında, aşağıdaki adımları izleyin:

    a. **Azure Stack aboneliklerinin**yanındaki **Bağlan** ' ı seçin.

   - Azure Active Directory (Azure AD) kullanıyorsanız, Azure Stack dağıtırken verdiğiniz Azure AD yönetici hesabını ve parolasını girin. **Oturum aç '** ı seçin.
   - Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanıyorsanız, yönetici hesabınızı girin. Örneğin: cloudadmin@azurestack.local. Parolanızı girin ve **oturum aç**' ı seçin.

   b. **Azure Stack abonelikler**' de, **varsayılan sağlayıcı aboneliğini**seçin.

     > [!IMPORTANT]
     > App Service **varsayılan sağlayıcı aboneliğine**dağıtılmalıdır.

   c. **Azure Stack konumlarda**, dağıtmakta olduğunuz bölgeye karşılık gelen konumu seçin. Örneğin, ASDK 'ye dağıtım yapıyorsanız **Yerel** ' i seçin.

    ![App Service yükleyicisi][3]

7. Artık [Bu adımları kullanarak](azure-stack-app-service-before-you-get-started.md#virtual-network)yapılandırdığınız mevcut bir sanal ağa dağıtabilir veya App Service yükleyicinin yeni bir sanal ağ ve alt ağlar oluşturmasına izin verebilirsiniz. VNet oluşturmak için aşağıdaki adımları izleyin:

   a. **Varsayılan ayarlarla VNET oluştur**' u seçin, Varsayılanları kabul edin ve ardından **İleri**' yi seçin.

   b. Alternatif olarak, **mevcut VNET ve alt ağları kullan**' ı seçin. Aşağıdaki eylemleri doldurun:

     - Sanal ağınızı içeren **kaynak grubunu** seçin.
     - Dağıtmak istediğiniz **sanal ağ** adını seçin.
     - Gerekli rol alt ağlarının her biri için doğru **alt ağ** değerlerini seçin.
     - **İleri**’yi seçin.

   ![App Service yükleyicisi][4]

8. Dosya paylaşımınızın bilgilerini girin ve ardından **İleri**' yi seçin. Dosya paylaşımının adresinin tam etki alanı adını (FQDN) veya dosya sunucunuzun IP adresini kullanması gerekir. Örneğin, \\\appservicefileserver.Local.cloudapp.azurestack.external\websites veya \\\10.0.0.1\websites.  Etki alanına katılmış bir dosya sunucusu kullanıyorsanız, etki alanı dahil olmak üzere tam Kullanıcı adını sağlamanız gerekir. Örneğin, Myfileserverdomain\fileshareowner.

   >[!NOTE]
   >Yükleyici, devam etmeden önce dosya paylaşımıyla bağlantıyı test etmeye çalışır. Ancak, var olan bir sanal ağa dağıtım yapıyorsanız bu bağlantı testi başarısız olabilir. Bir uyarı ve devam etmek için bir istem vermiş olursunuz. Dosya paylaşma bilgileri doğruysa dağıtıma devam edin.

   ![App Service yükleyicisi][7]

9. Sonraki App Service yükleyicisi sayfasında, aşağıdaki adımları izleyin:

   a. **Kimlik uygulama kimliği** kutusunda kimlik için KULLANDıĞıNıZ uygulamanın GUID 'sini girin (Azure AD 'den).

   b. **Kimlik uygulama sertifikası dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).

   c. **Kimlik uygulama sertifikası parolası** kutusuna sertifika için parolayı girin. Bu parola, sertifikaları oluşturmak için betiği kullandığınız zaman ' i unutmayın.

   d. **Azure Resource Manager kök sertifika dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).

   e. **İleri**’yi seçin.

   ![App Service yükleyicisi][9]

10. Üç sertifika dosyası kutusunun her biri için, **Araştır** ' ı seçin ve uygun sertifika dosyasına gidin. Her sertifika için parola sağlamalısınız. Bu sertifikalar, [gerekli sertifikaları oluştur adımında](azure-stack-app-service-before-you-get-started.md#get-certificates)oluşturduğunuz olanlardır. Tüm bilgileri girdikten sonra **İleri ' yi** seçin.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **App Service varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluştururken farklı bir etki alanı soneki kullandıysanız, sertifika Dosya adlarınız *Yerel kullanılmaz. AzureStack. external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![App Service yükleyicisi][10]

11. App Service kaynak sağlayıcısı veritabanını barındırmak için kullanılan sunucu örneğinin SQL Server ayrıntılarını girin ve ardından **İleri**' yi seçin. Yükleyici SQL bağlantı özelliklerini doğrular.<br><br>App Service yükleyicisi devam etmeden önce SQL Server bağlantıyı test etmeye çalışır. Var olan bir sanal ağa dağıtım yapıyorsanız bu bağlantı testi başarısız olabilir. Bir uyarı ve devam etmek için bir istem vermiş olursunuz. SQL Server bilgileri doğruysa dağıtıma devam edin.

    ![App Service yükleyicisi][11]

12. Rol örneği ve SKU seçeneklerini gözden geçirin. Varsayılanlar, bir ASDK dağıtımındaki her rol için minimum örnek sayısı ve minimum SKU ile doldurulur. Dağıtımınızı planlamaya yardımcı olmak için vCPU ve bellek gereksinimlerinin bir özeti verilmiştir. Seçimlerinizi yaptıktan sonra **İleri**' yi seçin.

    >[!NOTE]
    >Üretim dağıtımları için, [Azure Stack Azure App Service sunucu rolleri Için kapasite planlama](azure-stack-app-service-capacity-planning.md)yönergbir yönergeden sonra.

    | Role | Minimum örnekler | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service bulutu durumunu yönetir ve bakımını yapar. |
    | Yönetim | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service Azure Resource Manager ve API uç noktalarını, Portal uzantılarını (yönetici, kiracı, Işlevler portalı) ve veri hizmetini yönetir. Yük devretmeyi desteklemek için önerilen örnekleri 2 ' ye kadar arttırın. |
    | Yayımcı | 1\. | Standard_A1-(1 vCPU, 1792 MB) | FTP ve Web dağıtımı aracılığıyla içerik yayımlar. |
    | FrontEnd | 1\. | Standard_A1-(1 vCPU, 1792 MB) | İstekleri App Service uygulamalara yönlendirir. |
    | Paylaşılan çalışan | 1\. | Standard_A1-(1 vCPU, 1792 MB) | Web veya API uygulamalarını ve Azure Işlevleri uygulamalarını barındırır. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak, teklifinizi tanımlayabilir ve herhangi bir SKU katmanını seçebilirsiniz. Katmanların en az bir vCPU olması gerekir. |

    ![App Service yükleyicisi][13]

    >[!NOTE]
    >**Windows Server 2016 Core, Azure Stack üzerinde Azure App Service kullanımı için desteklenen bir platform görüntüsü değil.  Üretim dağıtımları için değerlendirme görüntülerini kullanmayın.**

13. **Platform görüntüsünü seçin** kutusunda, App Service bulutu için işlem kaynak sağlayıcısında bulunan görüntülerden dağıtım Windows Server 2016 sanal MAKINESI (VM) görüntünüzü seçin. **İleri**’yi seçin.

14. Sonraki App Service yükleyicisi sayfasında, aşağıdaki adımları izleyin:

     a. Çalışan rolü VM Yöneticisi Kullanıcı adı ve parolasını girin.

     b. Diğer roller VM Yönetici Kullanıcı adı ve parolasını girin.

     c. **İleri**’yi seçin.

    ![App Service yükleyicisi][15]

15. App Service Installer Özeti sayfasında, aşağıdaki adımları izleyin:

    a. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için **önceki düğmeleri kullanarak önceki sayfaları** ziyaret edin.

    b. Yapılandırma doğruysa onay kutusunu seçin.

    c. Dağıtımı başlatmak için **İleri**' yi seçin.

    ![App Service yükleyicisi][16]

16. Sonraki App Service yükleyicisi sayfasında, aşağıdaki adımları izleyin:

    a. Yükleme ilerlemesini izleyin. Azure Stack App Service, varsayılan seçimlere göre dağıtımı 60 dakika sürer.

    b. Yükleyici başarıyla tamamlandıktan sonra **Çıkış**' ı seçin.

    ![App Service yükleyicisi][17]

## <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]
> App Service RP 'Yi bir SQL Always on örneğiyle birlikte sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna eklemeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve bir hata durumunda hizmet kaybını engellemek için veritabanlarını eşitlemeniz **gerekir** veritabanı yük devretmesi.

Var olan bir sanal ağa dağıtım yapıyorsanız ve dosya sunucunuza bağlanmak için bir iç IP adresi kullanıyorsanız, bir giden güvenlik kuralı eklemeniz gerekir. Bu kural, çalışan alt ağı ve dosya sunucusu arasında SMB trafiğine izin vermez. Yönetim portalında, WorkersNsg Network Security grubuna gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

- Kaynak: Any
- Kaynak bağlantı noktası aralığı: *
- Hedef: IP adresleri
- Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
- Hedef bağlantı noktası aralığı: 445
- Protokol: TCP
- Eylem: Allow
- Öncelik: 700
- Ad: Outbound_Allow_SMB445

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Azure Stack yüklemesinde App Service doğrulama

1. Azure Stack yönetim portalında **Yönetim-App Service**' e gidin.

2. Genel bakışta durum ' ın altında, **durumun** **tüm rolleri**görüntüleyip görüntülemediğini kontrol edin.

    ![App Service yönetimi](media/azure-stack-app-service-deploy/image12.png)

## <a name="test-drive-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service test sürücüsü

App Service kaynak sağlayıcısını dağıttıktan ve kaydettikten sonra, kullanıcıların Web ve API Apps dağıtmaları için test edin.

>[!NOTE]
>Planda Microsoft. Web ad alanı olan bir teklif oluşturmanız gerekir. Ayrıca, teklifine abone olan bir kiracı aboneliğine de ihtiyacınız vardır. Daha fazla bilgi için bkz. [teklif oluşturma](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
>Azure Stack App Service kullanan uygulamalar oluşturmak için bir kiracı aboneliğiniz olması *gerekir* . Yönetim portalında bir hizmet yöneticisinin tamamlayabilmesi için yalnızca görevler App Service kaynak sağlayıcısı yönetimiyle ilgilidir. Bu, kapasite eklemeyi, dağıtım kaynaklarını yapılandırmayı ve çalışan katmanları ve SKU 'Ları eklemeyi içerir.
>
>Web, API ve Azure Işlevleri uygulamaları oluşturmak için kiracı portalını kullanmanız ve bir kiracı aboneliğine sahip olmanız gerekir.
>

Bir test Web uygulaması oluşturmak için şu adımları izleyin:

1. Azure Stack Kullanıcı portalında **+ kaynak** > oluştur**Web ve mobil** > **Web uygulaması**' nı seçin.

2. **Web uygulaması**altında **Web**uygulamasında bir ad girin.

3. **Kaynak grubu**altında **Yeni**' yi seçin. **Kaynak grubu**için bir ad girin.

4. **App Service planı/konumu** > **Yeni oluştur**' u seçin.

5. **App Service planı**altında **App Service planı**için bir ad girin.

6. **Fiyatlandırma katmanını** > **serbest paylaşılan** veya **paylaşılan-paylaşılan** > **Seç** > TamamOluştur > ' u seçin.

7. Panoda yeni Web uygulaması için bir kutucuk görüntülenir. Kutucuğu seçin.

8. **Web uygulamasında**, bu uygulamanın varsayılan Web sitesini görüntülemek Için, **Araştır** ' ı seçin.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress, DNN veya Docgo Web sitesi dağıtma (isteğe bağlı)

1. Azure Stack kiracı portalında **+** , Azure Marketi ' ne gidin, bir docgo Web sitesi dağıtın ve dağıtımın bitmesini bekleyin. Docgo web platformu, dosya sistemi tabanlı bir veritabanı kullanır. SQL veya MySQL gibi ek kaynak sağlayıcıları gerektirmez.

2. Ayrıca bir MySQL kaynak sağlayıcısı dağıttıysanız Market 'ten bir WordPress web sitesi dağıtabilirsiniz. Veritabanı parametreleri istendiğinde, tercih ettiğiniz Kullanıcı adı ve sunucu adı ile Kullanıcı *adını\@Kullanıcı1 Sunucu1*olarak girin.

3. Ayrıca bir SQL Server kaynak sağlayıcısı dağıttıysanız Market 'ten bir DNN Web sitesi dağıtabilirsiniz. Veritabanı parametreleri istendiğinde, kaynak sağlayıcınıza bağlı SQL Server çalıştıran bilgisayardaki bir veritabanını seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack App Service için ek yönetici işlemlerine hazırlanın:

- [Kapasite planlama](azure-stack-app-service-capacity-planning.md)
- [Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-installer.png
[2]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-arm-endpoints.png
[3]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-subscription-information.png
[4]: ./media/azure-stack-app-service-deploy/app-service-default-VNET-config.png
[5]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config-with-values.png
[7]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration.png
[8]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration-error.png
[9]: ./media/azure-stack-app-service-deploy/app-service-identity-app.png
[10]: ./media/azure-stack-app-service-deploy/app-service-certificates.png
[11]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration-error.png
[13]: ./media/azure-stack-app-service-deploy/app-service-cloud-quantities.png
[14]: ./media/azure-stack-app-service-deploy/app-service-windows-image-selection.png
[15]: ./media/azure-stack-app-service-deploy/app-service-role-credentials.png
[16]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-deployment-summary.png
[17]: ./media/azure-stack-app-service-deploy/app-service-deployment-progress.png
