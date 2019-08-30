---
title: Azure Stack 'de çevrimdışı bir ortamda App Service dağıtın | Microsoft Docs
description: AD FS tarafından güvenliği sağlanmış, bağlantısı kesilen Azure Stack ortamında App Service dağıtma hakkında ayrıntılı yönergeler.
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
ms.openlocfilehash: a33dc4ee1d9889b1dfa817b18b2c5c97e78c8432
ms.sourcegitcommit: 701685f0b59e5a3d1a8d39fe477b8df701a51cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70159621"
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>AD FS tarafından güvenliği sağlanmış bağlantısı kesilen Azure Stack ortamına App Service kaynak sağlayıcısı ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> 1907 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya en son Azure Stack geliştirme setini Azure App Service 1,7 ' i dağıtmadan önce dağıtın.

Bu makaledeki yönergeleri izleyerek [App Service kaynak sağlayıcısını](azure-stack-app-service-overview.md) aşağıdaki bir Azure Stack ortamına yükleyebilirsiniz:

- Internet 'e bağlı değil
- Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli hale getirilir (AD FS).

> [!IMPORTANT]
> Kaynak sağlayıcısı yükleyicisini çalıştırmadan önce, başlamadan [önce](azure-stack-app-service-before-you-get-started.md) ' daki Kılavuzu izlediğinizden ve 1,7 sürümüne eşlik eden [sürüm notlarını](azure-stack-app-service-release-notes-update-seven.md) okuduğunuzdan emin olun; yeni işlevsellik, düzeltmeler ve bilinen sorunlar hakkında bilgi edinin dağıtımınızı etkileyebilir.

App Service kaynak sağlayıcısını çevrimdışı Azure Stack dağıtımınıza eklemek için, bu en üst düzey görevleri gerçekleştirmeniz gerekir:

1. [Önkoşul adımlarını](azure-stack-app-service-before-you-get-started.md) (alınması gereken birkaç gün sürebilen sertifika satın alma gibi) doldurun.
2. [Yükleme ve yardımcı dosyalarını](azure-stack-app-service-before-you-get-started.md) Internet 'e bağlı bir makineye indirip ayıklayın.
3. Çevrimdışı yükleme paketi oluşturun.
4. Appservice. exe yükleyici dosyasını çalıştırın.

## <a name="create-an-offline-installation-package"></a>Çevrimdışı yükleme paketi oluşturma

Bağlantısı kesilmiş bir ortamda App Service dağıtmak için, önce Internet 'e bağlı bir makinede bir çevrimdışı yükleme paketi oluşturmanız gerekir.

1. AppService. exe yükleyicisini Internet 'e bağlı bir makinede çalıştırın.

2. **Gelişmiş** > **çevrimdışı yükleme paketi oluştur**' a tıklayın.

    ![App Service yükleyicisi][1]

3. App Service yükleyicisi bir çevrimdışı yükleme paketi oluşturur ve bu paketin yolunu görüntüler. Dosya Gezgini 'nde klasörü açmak için **klasörü aç** ' a tıklayabilirsiniz.

    ![App Service yükleyicisi](media/azure-stack-app-service-deploy-offline/image02.png)

4. Yükleyiciyi (AppService. exe) ve çevrimdışı yükleme paketini Azure Stack konak makinenize kopyalayın.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>App Service çevrimdışı yüklemesini Azure Stack üzerinde doldurun

1. Appservice. exe ' yi Azure Stack Yöneticisi Azure Kaynak Yönetimi uç noktasına ulaşabilbir bilgisayardan yönetici olarak çalıştırın.

2. **Gelişmiş** > **çevrimdışı yükleme**' ye tıklayın.

    ![App Service yükleyicisi][2]

3. Daha önce oluşturduğunuz çevrimdışı yükleme paketinin konumuna gidin ve ardından **İleri**' ye tıklayın.

    ![App Service yükleyicisi](media/azure-stack-app-service-deploy-offline/image04.png)

4. Microsoft yazılımı lisans koşulları 'nı gözden geçirip kabul edin ve ardından **İleri**' ye tıklayın.

5. Üçüncü taraf lisans koşullarını gözden geçirip kabul edin ve ardından **İleri**' ye tıklayın.

6. App Service bulut yapılandırması bilgilerinin doğru olduğundan emin olun. Azure Stack Geliştirme Seti dağıtımı sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri burada kabul edebilirsiniz. Ancak, Azure Stack dağıttığınız veya tümleşik bir sisteme dağıtırken seçenekleri özelleştirdiyseniz, Bu penceredeki değerleri, bunu yansıtacak şekilde düzenlemeniz gerekir. Örneğin, mycloud.com etki alanı sonekini kullanırsanız, Azure Stack kiracı Azure Resource Manager uç noktasının olarak `management.<region>.mycloud.com`değiştirilmesi gerekir. Bilgilerinizi doğruladıktan sonra **İleri**' ye tıklayın.

    ![App Service yükleyicisi][3]

7. Sonraki sayfada:
   1. **Azure Stack abonelikler** kutusunun yanındaki **Bağlan** düğmesine tıklayın.
      - Yönetici hesabınızı girin. Örneğin: cloudadmin@azurestack.local. Parolanızı girin ve **oturum aç**' a tıklayın.
   2. **Azure Stack abonelikler** kutusunda **varsayılan sağlayıcı aboneliğini**seçin.

      > [!NOTE]
      > App Service, yalnızca **varsayılan sağlayıcı aboneliğine**dağıtılabilir.
      >

   3. **Azure Stack konumları** kutusunda, dağıtmakta olduğunuz bölgeye karşılık gelen konumu seçin. Örneğin, Azure Stack Geliştirme Seti dağıtdıysanız **Yerel** ' i seçin.
   4. **İleri**'ye tıklayın.

      ![App Service yükleyicisi][4]

8. Şimdi [buradaki](azure-stack-app-service-before-you-get-started.md#virtual-network)adımlar aracılığıyla yapılandırılmış var olan bir sanal ağa dağıtım veya App Service yükleyicinin bir sanal ağ ve Ilişkili alt ağlar oluşturmasına izin verme seçeneğiniz vardır.
   1. **Varsayılan ayarlarla VNET oluştur**' u seçin, Varsayılanları kabul edin ve ardından **İleri**' ye tıklayın veya;
   2. **Mevcut VNET ve alt ağları kullan ' ı**seçin.
       1. Sanal ağınızı içeren **kaynak grubunu** seçin;
       2. Dağıtmak istediğiniz doğru **sanal ağ** adını seçin;
       3. Gerekli rol alt ağlarının her biri için doğru **alt ağ** değerlerini seçin;
       4. **İleri**'ye tıklayın.

      ![App Service yükleyicisi][5]

9. Dosya paylaşımınızla ilgili bilgileri girin ve ardından **İleri**' ye tıklayın. Dosya paylaşımının adresi, tam etki alanı adını veya dosya sunucunuzun IP adresini kullanmalıdır. Örneğin, \\\appservicefileserver.Local.cloudapp.azurestack.external\websites veya \\\10.0.0.1\websites.  Etki alanına katılmış bir dosya sunucusu kullanıyorsanız, etki alanı dahil olmak üzere tam Kullanıcı adını sağlamanız gerekir, örneğin, Myfileserverdomain\fileshareowner.

    > [!NOTE]
    > Yükleyici devam etmeden önce FileShare 'e bağlantıyı test girişiminde bulunur.  Ancak, var olan bir sanal ağda dağıtmayı seçerseniz, yükleyici dosya paylaşımına bağlanamaz ve devam etmek isteyip istemediğinizi soran bir uyarı görüntüler.  FileShare bilgilerini doğrulayın ve doğruysa devam edin.
    >
    >

   ![App Service yükleyicisi][8]

10. Sonraki sayfada:
    1. **Kimlik uygulama kimliği** kutusunda kimlik için KULLANDıĞıNıZ uygulamanın GUID 'sini girin (Azure AD 'den).
    2. **Kimlik uygulama sertifikası dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).
    3. **Kimlik uygulama sertifikası parolası** kutusuna sertifika için parolayı girin. Bu parola, sertifikaları oluşturmak için betiği kullandığınız zaman ' i unutmayın.
    4. **Azure Resource Manager kök sertifika dosyası** kutusunda, sertifika dosyasının konumunu girin (veya konumuna gidin).
    5. **İleri**'ye tıklayın.

    ![App Service yükleyicisi][10]

11. Üç sertifika dosyası kutusunun her biri için, **Araştır** ' a tıklayın ve ardından uygun sertifika dosyasına gidin. Her sertifika için parola sağlamalısınız. Bu sertifikalar, [gerekli sertifikaları oluştur adımında](azure-stack-app-service-before-you-get-started.md#get-certificates)oluşturduğunuz olanlardır. Tüm bilgileri girdikten sonra **İleri** ' ye tıklayın.

    | Box | Sertifika dosyası adı örneği |
    | --- | --- |
    | **App Service varsayılan SSL sertifika dosyası** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API SSL sertifika dosyası** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL sertifika dosyası** | ftp.appservice.local.AzureStack.external.pfx |

    Sertifikaları oluştururken farklı bir etki alanı soneki kullandıysanız, sertifika Dosya adlarınız *Yerel kullanılmaz. AzureStack. external*. Bunun yerine, özel etki alanı bilgilerinizi kullanın.

    ![App Service yükleyicisi][11]

12. App Service kaynak sağlayıcısı veritabanlarını barındırmak için kullanılan sunucu örneğinin SQL Server ayrıntılarını girin ve ardından **İleri**' ye tıklayın. Yükleyici SQL bağlantı özelliklerini doğrular. SQL Server adı için iç IP veya tam etki alanı adını girmeniz **gerekir** .

    > [!NOTE]
    > Yükleyici devam etmeden önce SQl Server 'a bağlantıyı test girişiminde bulunur.  Ancak, var olan bir sanal ağda dağıtmayı seçerseniz, yükleyici SQL Server bağlanamaz ve devam etmek isteyip istemediğiniz bir uyarı görüntüler.  SQL Server bilgilerini doğrulayın ve doğruysa devam edin.
    >
    > Azure Stack 1,3 ve sonraki sürümlerde Azure App Service, yükleyici SQL Server SQL Server düzeyinde veritabanı içerme özelliği etkinleştirilmiş olduğunu kontrol eder.  Aksi takdirde, aşağıdaki özel durum istenir:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;
    >    GO
    >    RECONFIGURE;
    >    GO
    >    ***********************************************************
    > ```
    > Daha fazla bilgi için [Azure Stack 1,3 Azure App Service için sürüm notlarına](azure-stack-app-service-release-notes-update-three.md) bakın.

    ![App Service yükleyicisi][12]

13. Rol örneği ve SKU seçeneklerini gözden geçirin. Varsayılanlar, en az örnek sayısı ve bir ASDK dağıtımındaki her bir rol için minimum SKU ile doldurulur. Dağıtımınızı planlamaya yardımcı olmak için vCPU ve bellek gereksinimlerinin bir özeti verilmiştir. Seçimlerinizi yaptıktan sonra **İleri**' ye tıklayın.

     > [!NOTE]
     > Üretim dağıtımları için, [Azure Stack Azure App Service sunucu rolleri Için kapasite planlaması](azure-stack-app-service-capacity-planning.md)bölümündeki yönergeleri izleyin.
     >
     >

    | Role | Minimum örnekler | Minimum SKU | Notlar |
    | --- | --- | --- | --- |
    | Denetleyici | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service bulutu durumunu yönetir ve bakımını yapar. |
    | Yönetim | 1\. | Standard_A2-(2 vCPU, 3584 MB) | App Service Azure Resource Manager ve API uç noktalarını, Portal uzantılarını (yönetici, kiracı, Işlevler portalı) ve veri hizmetini yönetir. Yük devretmeyi desteklemek için önerilen örnekleri 2 ' ye kadar arttırın. |
    | Yayımcı | 1\. | Standard_A1-(1 vCPU, 1792 MB) | FTP ve Web dağıtımı aracılığıyla içerik yayımlar. |
    | FrontEnd | 1\. | Standard_A1-(1 vCPU, 1792 MB) | İstekleri App Service uygulamalara yönlendirir. |
    | Paylaşılan çalışan | 1\. | Standard_A1-(1 vCPU, 1792 MB) | Web veya API uygulamalarını ve Azure Işlevleri uygulamalarını barındırır. Daha fazla örnek eklemek isteyebilirsiniz. Bir operatör olarak, teklifinizi tanımlayabilir ve herhangi bir SKU katmanını seçebilirsiniz. Katmanların en az bir vCPU olması gerekir. |

    ![App Service yükleyicisi][14]

    > [!NOTE]
    > **Windows Server 2016 Core, Azure Stack üzerinde Azure App Service kullanımı için desteklenen bir platform görüntüsü değil.  Üretim dağıtımları için değerlendirme görüntülerini kullanmayın.  Azure Stack Azure App Service, dağıtım için kullanılan görüntüde Microsoft.NET 3.5.1 SP1 'in etkinleştirilmesini gerektirir.   Market dağıtılmış Windows Server 2016 görüntülerinde bu özellik etkin değildir, bu nedenle bu önceden etkinleştirilmiş bir Windows Server 2016 görüntüsü oluşturmanız ve kullanmanız gerekir.**

14. **Platform görüntüsünü seçin** kutusunda, App Service bulutu için işlem kaynak sağlayıcısı 'nda bulunan görüntülerden dağıtım Windows Server 2016 sanal makine görüntünüzü seçin. **İleri**’yi seçin.

15. Sonraki sayfada:
     1. Çalışan rolü sanal makine yöneticisi Kullanıcı adı ve parolasını girin.
     2. Diğer roller sanal makine yöneticisi Kullanıcı adı ve parolasını girin.
     3. **İleri**'ye tıklayın.

    ![App Service yükleyicisi][16]

16. Özet sayfasında:
    1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için önceki düğmeleri kullanarak önceki sayfaları ziyaret edin.
    2. Yapılandırma doğruysa onay kutusunu seçin.
    3. Dağıtımı başlatmak için **İleri**' ye tıklayın.

    ![App Service yükleyicisi][17]

17. Sonraki sayfada:
    1. Yükleme ilerlemesini izleyin. Azure Stack App Service, varsayılan seçimlere göre dağıtımı 60 dakika sürer.
    2. Yükleyici başarıyla tamamlandıktan sonra **Çıkış**' a tıklayın.

    ![App Service yükleyicisi][18]

## <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]
> App Service RP 'Yi bir SQL Always on örneğiyle sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna eklemeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve veritabanlarını bir veritabanı durumunda herhangi bir hizmet kaybını engelleyecek şekılde eşitlemeniz gerekir yükünü.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiğini etkinleştirerek bir giden güvenlik kuralı eklemeniz gerekir.  Yönetim Portalı ' nda WorkersNsg, Network Security grubuna gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

- Kaynak: Any
- Kaynak bağlantı noktası aralığı: *
- Hedef: IP Adresleri
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

> [!NOTE]
> Plan içinde Microsoft. Web ad alanı olan bir teklif oluşturmanız gerekir. Ardından, bu teklifine abone olan bir kiracı aboneliğine sahip olmanız gerekir. Daha fazla bilgi için bkz. [teklif oluşturma](azure-stack-create-offer.md) ve [plan oluşturma](azure-stack-create-plan.md).
>
> Azure Stack App Service kullanan uygulamalar oluşturmak için bir kiracı aboneliğiniz olması *gerekir* . Bir hizmet yöneticisinin yönetim portalı içinde tamamlayabilecek tek yetenekler App Service kaynak sağlayıcısı yönetimiyle ilgilidir. Bu yetenekler arasında kapasite ekleme, dağıtım kaynaklarını yapılandırma ve çalışan katmanları ve SKU 'Lar ekleme sayılabilir.
>
> Üçüncü Teknik Önizleme sürümünden itibaren Web, API ve Azure Işlevleri uygulamaları oluşturmak için kiracı portalını kullanmanız ve bir kiracı aboneliğine sahip olmanız gerekir.

1. Azure Stack kiracı portalında **+ kaynak** > oluştur**Web ve mobil** > **Web uygulaması**' na tıklayın.

2. **Web uygulaması** dikey penceresinde, **Web uygulaması** kutusuna bir ad yazın.

3. **Kaynak grubu**altında **Yeni**' ye tıklayın. **Kaynak grubu** kutusuna bir ad yazın.

4. **App Service planı/Konum** > **Yeni Oluştur**'a tıklayın.

5. **App Service planı** dikey penceresinde, **App Service planı** kutusuna bir ad yazın.

6. **Fiyatlandırma katmanı** > **serbest paylaşılan** veya **paylaşılan-paylaşılan** > **Seç** > TamamOluştur > ' a tıklayın.

7. Bir dakika içinde, panoda yeni Web uygulaması için bir kutucuk görünür. Kutucuğa tıklayın.

8. **Web uygulaması** dikey penceresinde, bu uygulamanın varsayılan Web sitesini görüntülemek Için, **Araştır** ' a tıklayın.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress, DNN veya Docgo Web sitesi dağıtma (isteğe bağlı)

1. Azure Stack kiracı portalında, Azure Marketi ' **+** ne gidin, bir docgo Web sitesi dağıtın ve başarılı bir şekilde tamamlanmasını bekleyin. Docgo web platformu, dosya sistemi tabanlı bir veritabanı kullanır. SQL veya MySQL gibi ek kaynak sağlayıcıları gerektirmez.

2. Ayrıca bir MySQL kaynak sağlayıcısı dağıttıysanız Market 'ten bir WordPress web sitesi dağıtabilirsiniz. Veritabanı parametreleri istendiğinde, tercih ettiğiniz Kullanıcı adı ve sunucu adı ile Kullanıcı *adını\@Kullanıcı1 Sunucu1*olarak girin.

3. Ayrıca bir SQL Server kaynak sağlayıcısı dağıttıysanız Market 'ten bir DNN Web sitesi dağıtabilirsiniz. Veritabanı parametreleri istendiğinde, kaynak sağlayıcınıza bağlı SQL Server çalıştıran bilgisayardaki bir veritabanını seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack App Service için ek yönetici işlemlerine hazırlanma

- [Kapasite planlama](azure-stack-app-service-capacity-planning.md)
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
