---
title: Azure Stack 'de çevrimdışı bir ortamda App Service dağıtın | Microsoft Docs
description: AD FS tarafından güvenliği sağlanmış çevrimdışı bir Azure Stack ortamında App Service dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: BryanLa
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
ms.openlocfilehash: 25cb846b8e3a2200636dff0f2acfc25244a5e3d4
ms.sourcegitcommit: e8aa26b078a9bab09c8fafd888a96785cc7abb4d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709015"
---
# <a name="deploy-app-service-in-an-offline-environment-in-azure-stack"></a>Azure Stack 'de çevrimdışı bir ortamda App Service dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> 1907 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,7 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

Bu makaledeki yönergeleri izleyerek [App Service kaynak sağlayıcısını](azure-stack-app-service-overview.md) aşağıdaki bir Azure Stack ortamına dağıtabilirsiniz:

- İnternet 'e bağlı değil.
- Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli hale getirilir (AD FS).

> [!IMPORTANT]
> Kaynak sağlayıcısı yükleyicisini çalıştırmadan önce [Azure Stack App Service dağıtmaya yönelik önkoşullar](azure-stack-app-service-before-you-get-started.md)bölümündeki adımları tamamladığınızdan emin olun. Ayrıca, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için 1,7 sürümüne eşlik eden [sürüm notlarını](azure-stack-app-service-release-notes-update-seven.md) da okumalısınız.

App Service kaynak sağlayıcısını çevrimdışı Azure Stack dağıtımınıza eklemek için, bu en üst düzey görevleri gerçekleştirmeniz gerekir:

1. [Önkoşul adımlarını](azure-stack-app-service-before-you-get-started.md) (alınması gereken birkaç gün sürebilen sertifika satın alma gibi) doldurun.
2. [Yükleme ve yardımcı dosyalarını](azure-stack-app-service-before-you-get-started.md) Internet 'e bağlı bir makineye indirip ayıklayın.
3. Çevrimdışı yükleme paketi oluşturun.
4. Appservice. exe yükleyici dosyasını çalıştırın.

## <a name="create-an-offline-installation-package"></a>Çevrimdışı yükleme paketi oluşturma

App Service çevrimdışı bir ortamda dağıtmak için, önce internet 'e bağlı bir makinede çevrimdışı yükleme paketi oluşturun.

1. AppService. exe yükleyicisini Internet 'e bağlı bir makinede çalıştırın.

2. **Gelişmiş**@no__t seçin-1**çevrimdışı yükleme paketi oluşturun**.

    ![App Service yükleyicisinde çevrimdışı paket oluşturma][1]

3. App Service yükleyicisi bir çevrimdışı yükleme paketi oluşturur ve bu paketin yolunu görüntüler. Dosya Gezgini 'nde klasörü açmak için **klasörü aç** ' ı seçebilirsiniz.

    ![App Service yükleyicisinde çevrimdışı yükleme paketi başarıyla oluşturuldu](media/azure-stack-app-service-deploy-offline/image02.png)

4. Yükleyiciyi (AppService. exe) ve çevrimdışı yükleme paketini Azure Stack konak makinenize kopyalayın.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>App Service çevrimdışı yüklemesini Azure Stack üzerinde doldurun

1. Appservice. exe ' yi Azure Stack Yöneticisi Azure Kaynak Yönetimi uç noktasına ulaşabilme bir bilgisayardan yönetici olarak çalıştırın.

1. **Gelişmiş** >  ' i seçin,**çevrimdışı yükleme tamamlanmıştır**.

    ![App Service yükleyici 'de çevrimdışı yükleme 'yi doldurun][2]

1. Daha önce oluşturduğunuz çevrimdışı yükleme paketinin konumuna gidin ve ardından **İleri**' yi seçin.

    ![Çevrimdışı yükleme paketi yolu anlık ileti App Service yükleyiciyi belirtin](media/azure-stack-app-service-deploy-offline/image04.png)

1. Microsoft yazılımı lisans koşulları 'nı gözden geçirip kabul edin ve ardından **İleri**' yi seçin.

1. Üçüncü taraf lisans koşullarını gözden geçirip kabul edin ve ardından **İleri**' yi seçin.

1. App Service bulut yapılandırması bilgilerinin doğru olduğundan emin olun. ASDK dağıtımı sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri burada kabul edebilirsiniz. Ancak, Azure Stack dağıttığınız veya tümleşik bir sisteme dağıtırken seçenekleri özelleştirdiyseniz, Bu penceredeki değerleri bu değişiklikleri yansıtacak şekilde düzenlemeniz gerekir. Örneğin, mycloud.com etki alanı sonekini kullanırsanız, Azure Stack kiracı Azure Resource Manager uç noktasının olarak `management.<region>.mycloud.com`değiştirilmesi gerekir. Bilgilerinizi doğruladıktan sonra **İleri**' yi seçin.

    ![App Service yükleyicisinde Azure App Service bulutu yapılandırma][3]

1. Sonraki sayfada:

   a. **Azure Stack abonelikler** kutusunun yanındaki **Bağlan** düğmesini seçin. 

   b. Yönetici hesabınızı girin. Örneğin: cloudadmin@azurestack.local. Parolanızı girip **oturum aç '** ı seçin.

   c. **Azure Stack abonelikler** kutusunda **varsayılan sağlayıcı aboneliği**' ni seçin.
   > [!NOTE]
   > App Service, yalnızca **varsayılan sağlayıcı aboneliğine**dağıtılabilir.

   d. **Azure Stack konumları** kutusunda, dağıtmakta olduğunuz bölgeye karşılık gelen konumu seçin. Örneğin, ASDK 'ye dağıtıyorsanız **Yerel**' i seçin.

   e. **İleri**’yi seçin.

      ![App Service yükleyicisindeki abonelikleri ve konumları Azure Stack][4]

1. App Service yükleyicisinin bir sanal ağ ve ilişkili alt ağlar oluşturmasına izin verebilirsiniz. Ya da, [Bu adımlar](azure-stack-app-service-before-you-get-started.md#virtual-network)aracılığıyla yapılandırıldığı gibi var olan bir sanal ağa dağıtabilirsiniz.
   - App Service Installer metodunu kullanmak için **varsayılan ayarlarla VNET oluştur**' u seçin, Varsayılanları kabul edin ve ardından **İleri**' yi seçin.
   - Mevcut bir ağa dağıtmak için, **mevcut VNET ve alt ağları kullan**' ı seçin ve ardından:
       1. Sanal ağınızı içeren **kaynak grubu** seçeneğini belirleyin.
       2. Dağıtmak istediğiniz **sanal ağ** adını seçin.
       3. Gerekli rol alt ağlarının her biri için doğru **alt ağ** değerlerini seçin.
       4. **İleri**’yi seçin.

      ![App Service yükleyicisinde sanal ağ ve alt ağ bilgileri][5]

1. Dosya paylaşımınızın bilgilerini girin ve ardından **İleri**' yi seçin. Dosya paylaşımının adresi, dosya sunucunuzun tam etki alanı adını (FQDN) veya IP adresini kullanmalıdır. Örneğin: \\ \ appservicedosyasunucusu. Local. cloudapp. azurestack. external\websiteleri veya \\ \ 10.0.0.1 \ Web siteleri.  Etki alanına katılmış bir dosya sunucusu kullanıyorsanız, etki alanı dahil olmak üzere tam Kullanıcı adını sağlamanız gerekir. Örneğin: `<myfileserverdomain>\<FileShareOwner>`.

    > [!NOTE]
    > Yükleyici, devam etmeden önce dosya paylaşımıyla bağlantıyı test etmeye çalışır. Ancak, var olan bir sanal ağa dağıtmayı seçerseniz, yükleyici dosya paylaşımıyla bağlantı kuramıyor ve devam etmek isteyip istemediğinizi soran bir uyarı görüntülüyor olabilir. Dosya paylaşma bilgilerini doğrulayın ve doğruysa devam edin.

   ![App Service yükleyicisinde dosya paylaşma bilgileri][8]

1. Sonraki sayfada:
    1. **Kimlik uygulama kimliği** kutusunda kimlik için KULLANDıĞıNıZ uygulamanın GUID 'sini girin (Azure AD 'den).
    1. **Kimlik uygulama sertifikası dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).
    1. **Kimlik uygulama sertifikası parolası** kutusuna sertifika için parolayı girin. Bu parola, sertifikaları oluşturmak için betiği kullandığınız zaman ' i unutmayın.
    1. **Azure Resource Manager kök sertifika dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).
    1. **İleri**’yi seçin.

    ![App Service yükleyicisinde uygulama KIMLIĞI ve sertifika bilgilerini girin][10]

1. Üç sertifika dosyası kutusunun her biri için, **Araştır**' ı seçin ve ardından uygun sertifika dosyasına gidin. Her sertifika için parola sağlamalısınız. Bu sertifikalar, [gerekli sertifikaları oluştur adımında](azure-stack-app-service-before-you-get-started.md#get-certificates)oluşturduğunuz olanlardır. Tüm bilgileri girdikten sonra **İleri ' yi** seçin.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **App Service varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluştururken farklı bir etki alanı soneki kullandıysanız, sertifika Dosya adlarınız *Yerel kullanılmaz. AzureStack. external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![App Service yükleyicisinde SSL sertifikası bilgilerini girin][11]

1. App Service kaynak sağlayıcısı veritabanlarını barındırmak için kullanılan sunucu örneğinin SQL Server ayrıntılarını girin ve ardından **İleri**' yi seçin. Yükleyici SQL bağlantı özelliklerini doğrular. SQL Server adı için iç IP veya FQDN girmeniz **gerekir** .

    > [!NOTE]
    > Yükleyici devam etmeden önce SQL Server çalıştıran bilgisayarla bağlantıyı test etmeye çalışır. Ancak, var olan bir sanal ağa dağıtmayı seçerseniz, yükleyici SQL Server çalıştıran bilgisayara bağlanamamalı ve devam etmek isteyip istemediğinizi soran bir uyarı görüntülüyor olabilir. SQL Server bilgilerini doğrulayın ve doğruysa devam edin.
    >
    > Azure Stack 1,3 ile ilgili Azure App Service, yükleyici, SQL Server çalıştıran bilgisayarın SQL Server düzeyinde veritabanı içerme özelliği etkinleştirilmiş olduğunu denetler. Aksi takdirde, aşağıdaki özel durum istenir:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;
    >    GO
    >    RECONFIGURE;
    >    GO
    >    ***********************************************************
    > ```
    > Daha fazla ayrıntı için, [Azure Stack 1,3 Azure App Service sürüm notlarına](azure-stack-app-service-release-notes-update-three.md)bakın.

    ![App Service yükleyicide SQL Server bilgileri girin][12]

1. Rol örneği ve SKU seçeneklerini gözden geçirin. Varsayılanlar, bir ASDK dağıtımındaki her rol için minimum örnek sayısı ve minimum SKU ile doldurulur. Dağıtımınızı planlamaya yardımcı olmak için vCPU ve bellek gereksinimlerinin bir özeti verilmiştir. Seçimlerinizi yaptıktan sonra **İleri**' yi seçin.

     > [!NOTE]
     > Üretim dağıtımları için, [Azure Stack Azure App Service sunucu rolleri Için kapasite planlaması](azure-stack-app-service-capacity-planning.md)bölümündeki yönergeleri izleyin.
     >
     >

    | Role | Minimum örnekler | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service bulutu durumunu yönetir ve bakımını yapar. |
    | Yönetim | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service Azure Resource Manager ve API uç noktalarını, Portal uzantılarını (yönetici, kiracı, Işlevler portalı) ve veri hizmetini yönetir. Yük devretmeyi desteklemek için önerilen örnekleri 2 ' ye yükseltin. |
    | Yayımcı | 1\. | Standard_A1-(1 vCPU, 1792 MB) | FTP ve Web dağıtımı aracılığıyla içerik yayımlar. |
    | Ön Uç | 1\. | Standard_A1-(1 vCPU, 1792 MB) | İstekleri App Service uygulamalara yönlendirir. |
    | Paylaşılan çalışan | 1\. | Standard_A1-(1 vCPU, 1792 MB) | Web veya API uygulamalarını ve Azure Işlevleri uygulamalarını barındırır. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak, teklifinizi tanımlayabilir ve herhangi bir SKU katmanını seçebilirsiniz. Katmanların en az bir vCPU olması gerekir. |

    ![App Service yükleyicisinde rol katmanlarını ve SKU seçeneklerini ayarlama][14]

    > [!NOTE]
    > Windows Server 2016 Core, Azure Stack üzerinde Azure App Service kullanımı için desteklenen bir platform görüntüsü *değil* .  Üretim dağıtımları için değerlendirme görüntülerini kullanmayın. Azure Stack Azure App Service, dağıtım için kullanılan görüntüde Microsoft .NET 3.5.1 SP1 'in etkinleştirilmesini gerektirir. Market-dağıtılmış Windows Server 2016 görüntülerinde bu özellik etkin değildir. Bu nedenle, bu özellikle önceden etkinleştirilmiş bir Windows Server 2016 görüntüsü oluşturmanız ve kullanmanız gerekir.

1. **Platform görüntüsünü seçin** kutusunda, App Service bulutu için işlem kaynak sağlayıcısında bulunan görüntülerden dağıtım Windows Server 2016 sanal MAKINESI (VM) görüntünüzü seçin. **İleri**’yi seçin.

1. Sonraki sayfada:
     1. Çalışan rolü VM Yöneticisi Kullanıcı adı ve parolasını girin.
     2. Diğer roller VM Yönetici Kullanıcı adı ve parolasını girin.
     3. **İleri**’yi seçin.

    ![App Service yükleyici 'de rol VM yöneticileri girin][16]

1. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için **önceki düğmeleri kullanarak önceki sayfaları** ziyaret edin.
    2. Yapılandırma doğruysa onay kutusunu seçin.
    3. Dağıtımı başlatmak için **İleri**' yi seçin.

    ![App Service yükleyicisinde yapılan seçimlerin Özeti][17]

1. Sonraki sayfada:
    1. Yükleme ilerlemesini izleyin. Azure Stack App Service, varsayılan seçimlere göre dağıtımı 60 dakika sürer.
    2. Yükleyici çalışmayı tamamladıktan sonra **Çıkış**' ı seçin.

    ![App Service yükleyicisinde yükleme işlemini izleme][18]

## <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]
> App Service RP 'yi bir SQL Always on örneğiyle sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna eklemeniz](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) *gerekir* . Veritabanı yük devretmesi durumunda herhangi bir hizmet kaybını engellemek için de veritabanlarını eşitlemeniz gerekir.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiğini etkinleştirerek bir giden güvenlik kuralı eklemeniz gerekir. Yönetici portalında, WorkersNsg Network Security grubuna gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

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

1. Azure Stack yönetici portalında **Yönetim-App Service**' e gidin.

1. Genel bakışta durum ' ın altında, **durumun** **tüm rolleri**görüntüleyip görüntülemediğini kontrol edin.

    ![App Service yönetimine genel bakış](media/azure-stack-app-service-deploy/image12.png)

## <a name="test-drive-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service test sürücüsü

App Service kaynak sağlayıcısını dağıttıktan ve kaydettikten sonra, kullanıcıların Web ve API Apps dağıtmaları için test edin.

> [!NOTE]
> Plan içinde Microsoft. Web ad alanı olan bir teklif oluşturmanız gerekir. Ardından, bu teklifine abone olan bir kiracı aboneliğine sahip olmanız gerekir. Daha fazla bilgi için bkz. [teklif oluşturma](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
> Azure Stack App Service kullanan uygulamalar oluşturmak için bir kiracı aboneliğiniz olması *gerekir* . Bir hizmet yöneticisinin Yönetici portalı içinde tamamlayabilecek tek yetenekler App Service kaynak sağlayıcısı yönetimiyle ilgilidir. Bu yetenekler arasında kapasite ekleme, dağıtım kaynaklarını yapılandırma ve çalışan katmanları ve SKU 'Lar ekleme sayılabilir.
>
> Üçüncü Teknik Önizleme sürümünden itibaren Web, API ve Azure Işlevleri uygulamaları oluşturmak için kiracı portalını kullanmanız ve bir kiracı aboneliğine sahip olmanız gerekir.

1. Azure Stack kiracı portalında **+ kaynak oluştur** > **Web ve mobil** > **Web uygulaması**' nı seçin.

1. **Web uygulaması** dikey penceresinde, **Web uygulaması** kutusuna bir ad yazın.

1. **Kaynak grubu**altında **Yeni**' yi seçin. **Kaynak grubu** kutusuna bir ad yazın.

1. **App Service planı/konumu** > **Yeni oluştur**' u seçin.

1. **App Service planı** dikey penceresinde, **App Service planı** kutusuna bir ad yazın.

1. **Fiyatlandırma katmanını** > **serbest paylaşılan** veya **paylaşılan-paylaşılan** > **Seç** > TamamOluştur > ' u seçin.

1. Bir dakikadan kısa bir sürede, panoda yeni Web uygulaması için bir kutucuk görünür. Kutucuğu seçin.

1. **Web uygulaması** dikey penceresinde, bu uygulamanın varsayılan Web sitesini görüntülemek Için, **Araştır** ' ı seçin.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress, DNN veya Docgo Web sitesi dağıtma (isteğe bağlı)

1. Azure Stack kiracı portalında **+** ' i seçin, Azure Marketi ' ne gidin, bir Docgo Web sitesi dağıtın ve başarılı bir şekilde tamamlanmasını bekleyin. Docgo web platformu, dosya sistemi tabanlı bir veritabanı kullanır. SQL veya MySQL gibi ek kaynak sağlayıcıları gerektirmez.

1. Ayrıca bir MySQL kaynak sağlayıcısı dağıttıysanız, Azure Marketi 'nden bir WordPress web sitesi dağıtabilirsiniz. Veritabanı parametreleri istendiğinde, tercih ettiğiniz Kullanıcı adı ve sunucu adı ile Kullanıcı *adını\@Kullanıcı1 Sunucu1*olarak girin.

1. Ayrıca bir SQL Server kaynak sağlayıcısı dağıttıysanız, Azure Marketi 'nden bir DNN Web sitesi dağıtabilirsiniz. Veritabanı parametreleri sorulduğunda, kaynak sağlayıcınıza bağlı SQL Server çalıştıran bilgisayardaki bir veritabanını seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack App Service için ek yönetici işlemlerine hazırlanın:

- [Kapasite planlaması](azure-stack-app-service-capacity-planning.md)
- [Dağıtım kaynaklarını yapılandırma](azure-stack-app-service-configure-deployment-sources.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
