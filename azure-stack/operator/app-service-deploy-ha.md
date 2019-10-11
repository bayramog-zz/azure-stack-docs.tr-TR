---
title: Yüksek oranda kullanılabilir bir yapılandırmada Azure Stack App Service dağıtma | Microsoft Docs
description: Yüksek oranda kullanılabilir bir yapılandırma kullanarak Azure Stack App Service dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 0bf89b0f80557f99c83fb5ad6afd0c4a5dcd3849
ms.sourcegitcommit: dfaf0126bc9975ca1643d55f06c71df9e32ea976
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165018"
---
# <a name="deploy-app-service-in-a-highly-available-configuration"></a>Yüksek oranda kullanılabilir bir yapılandırmada App Service dağıtma

Bu makalede, yüksek oranda kullanılabilir bir yapılandırmada Azure Stack için App Service dağıtmak üzere Azure Stack Market öğelerinin nasıl kullanılacağı gösterilmektedir. Bu çözüm, kullanılabilir Market öğelerine ek olarak [appservice-FileShare-SqlServer-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack hızlı başlangıç şablonunu da kullanır. Bu şablon App Service kaynak sağlayıcısını barındırmak için yüksek oranda kullanılabilir bir altyapının oluşturulmasını otomatikleştirir. App Service daha sonra bu yüksek oranda kullanılabilir VM altyapısına yüklenir. 

## <a name="deploy-the-highly-available-app-service-infrastructure-vms"></a>Yüksek oranda kullanılabilir App Service altyapısı VM 'lerini dağıtma
[Appservice-FileShare-SqlServer-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack hızlı başlangıç şablonu, yüksek oranda kullanılabilir bir yapılandırmada App Service dağıtımını basitleştirir. Varsayılan sağlayıcı aboneliğine dağıtılmalıdır. 

Azure Stack içinde özel bir kaynak oluşturmak için kullanıldığında, şablon şunları oluşturur:
- Bir sanal ağ ve gerekli alt ağlar.
- Dosya sunucusu, SQL Server ve Active Directory Domain Services (AD DS) alt ağları için ağ güvenlik grupları.
- VM diskleri ve küme bulutu tanığı için depolama hesapları.
- SQL VM 'Leri için SQL AlwaysOn dinleyicisine göre özel IP 'si olan bir iç yük dengeleyici.
- AD DS, dosya sunucusu kümesi ve SQL kümesi için üç kullanılabilirlik kümesi.
- İki düğümlü SQL kümesi.
- İki düğümlü dosya sunucusu kümesi.
- İki etki alanı denetleyicisi.

### <a name="required-azure-stack-marketplace-items"></a>Market öğelerini gerekli Azure Stack
Bu şablonu kullanmadan önce, aşağıdaki [Azure Stack Market öğelerinin](azure-stack-marketplace-azure-items.md) Azure Stack örneğiniz içinde kullanılabilir olduğundan emin olun:

- Windows Server 2016 Datacenter çekirdek görüntüsü (AD DS ve dosya sunucusu VM 'Leri için)
- Windows Server 2016 üzerinde SQL Server 2016 SP2 (Enterprise)
- En son SQL IaaS uzantısı 
- En son PowerShell Istenen durum yapılandırma uzantısı 

> [!TIP]
> Şablon gereksinimleri ve varsayılan değerler hakkında daha fazla bilgi için GitHub 'daki [şablon Benioku dosyasını](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) gözden geçirin. 

### <a name="deploy-the-app-service-infrastructure"></a>App Service altyapısını dağıtma
**Appservice-FileShare-SqlServer-ha** Azure Stack hızlı başlangıç şablonunu kullanarak özel bir dağıtım oluşturmak için bu bölümdeki adımları kullanın.

1. [!INCLUDE [azs-admin-portal](../includes/azs-admin-portal.md)]

2. **@No__t-1** ' i seçin  > **özel** **oluştur**' u ve ardından **şablon dağıtımı**.

   ![Özel şablon dağıtımı](media/app-service-deploy-ha/1.png)


3. **Özel dağıtım** dikey penceresinde, **Şablonu Düzenle** > **hızlı başlangıç şablonu** ' nu seçin ve ardından, kullanılabilir özel şablonların açılan listesini kullanarak **appservice-FileShare-SqlServer-ha** şablonunu seçin. **Tamam**' a ve ardından **Kaydet**' e tıklayın.

   ![Appservice-FileShare-SqlServer-ha hızlı başlangıç şablonunu seçin](media/app-service-deploy-ha/2.png)

4. **Özel dağıtım** dikey penceresinde **parametreleri Düzenle** ' yi seçin ve varsayılan şablon değerlerini gözden geçirmek için aşağı kaydırın. Tüm gerekli parametre bilgilerini sağlamak için bu değerleri gereken şekilde değiştirin ve ardından **Tamam**' a tıklayın.<br><br> En azından, `ADMINPASSWORD`, `FILESHAREOWNERPASSWORD`, `FILESHAREUSERPASSWORD`, `SQLSERVERSERVICEACCOUNTPASSWORD` ve `SQLLOGINPASSWORD` parametreleri için karmaşık parolalar sağlayın.
    
   ![Özel dağıtım parametrelerini Düzenle](media/app-service-deploy-ha/3.png)

5. **Özel dağıtım** dikey penceresinde, kullanılacak abonelik olarak **varsayılan sağlayıcı aboneliğinin** seçildiğinden emin olun ve ardından yeni bir kaynak grubu oluşturun veya özel dağıtım için var olan bir kaynak grubunu seçin.<br><br> Sonra, kaynak grubu konumunu (ASDK yüklemeleri için**Yerel** ) seçin ve ardından **Oluştur**' a tıklayın. Özel dağıtım ayarları, şablon dağıtımı başlamadan önce onaylanır.

    ![Özel dağıtım oluştur](media/app-service-deploy-ha/4.png)

6. Yönetici portalında **kaynak grupları** ' nı ve ardından özel dağıtım için oluşturduğunuz kaynak grubunun adını (Bu örnekteki**App-Service-ha** ) seçin. Tüm dağıtımların başarıyla tamamlandığından emin olmak için dağıtımın durumunu görüntüleyin.

   > [!NOTE]
   > Şablon dağıtımının tamamlanmasını yaklaşık bir saat sürer.

   [![](media/app-service-deploy-ha/5-sm.png "Şablon dağıtım durumunu gözden geçirme")](media/app-service-deploy-ha/5-lg.png#lightbox)


### <a name="record-template-outputs"></a>Kayıt şablonu çıkışları
Şablon dağıtımı başarıyla tamamlandıktan sonra, şablon dağıtım çıkışlarını kaydedin. App Service yükleyicisini çalıştırırken bu bilgiye ihtiyacınız vardır.

Bu çıkış değerlerinin her birini kaydetmeyi sağlayın:
- Dosya SharePath
- FileShareOwner
- FileShareUser
- SQLserver
- SQLuser

Şablon çıkış değerlerini öğrenmek için şu adımları izleyin:

1. [!INCLUDE [azs-admin-portal](../includes/azs-admin-portal.md)]

2. Yönetici portalında **kaynak grupları** ' nı ve ardından özel dağıtım için oluşturduğunuz kaynak grubunun adını (Bu örnekteki**App-Service-ha** ) seçin. 

3. **Dağıtımlar** ' a tıklayın ve **Microsoft. Template**' i seçin.

    ![Microsoft. Şablon dağıtımı](media/app-service-deploy-ha/6.png)

4. **Microsoft. Template** dağıtımını seçtikten sonra, **çıktılar** ' i seçin ve şablon parametresi çıkışını kaydedin. App Service dağıtıldığında bu bilgiler gereklidir.

    ![Parametre çıkışı](media/app-service-deploy-ha/7.png)


## <a name="deploy-app-service-in-a-highly-available-configuration"></a>Yüksek oranda kullanılabilir bir yapılandırmada App Service dağıtma
[Appservice-FileShare-SqlServer-ha](https://github.com/Azure/azurestack-quickstart-templates/tree/master/appservice-fileserver-sqlserver-ha) Azure Stack hızlı başlangıç şablonunu temel alan yüksek kullanılabilirliğe sahip bir yapılandırmada Azure Stack için App Service dağıtmak üzere bu bölümdeki adımları izleyin. 

App Service kaynak sağlayıcısı 'nı yükledikten sonra, bu sürümü tekliflerinizi ve planlarınıza dahil edebilirsiniz. Kullanıcılar daha sonra hizmeti almak ve uygulama oluşturmaya başlamak için abone olabilir.

> [!IMPORTANT]
> Kaynak sağlayıcısı yükleyicisini çalıştırmadan önce, yeni işlevsellik, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için her bir App Service sürümüyle birlikte gelen sürüm notlarını okuduğunuzdan emin olun.

### <a name="prerequisites"></a>Önkoşullar
App Service yükleyicisini çalıştırmadan önce, [Azure Stack makalesinde App Service kullanmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)bölümünde açıklandığı gibi birkaç adım gerekir:

> [!TIP]
> Şablon dağıtımı, altyapı VM 'lerini sizin için yapılandırdığından, [App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md) ' de açıklanan adımların tümü gereklidir.

- [App Service yükleyicisini ve yardımcı betikleri indirin](azure-stack-app-service-before-you-get-started.md#download-the-installer-and-helper-scripts).
- [Azure Stack marketi 'nden öğeleri indirin](azure-stack-app-service-before-you-get-started.md#download-items-from-the-azure-marketplace).
- [Gerekli sertifikaları oluştur](azure-stack-app-service-before-you-get-started.md#get-certificates).
- Azure Stack için seçtiğiniz kimlik sağlayıcısını temel alarak kimlik uygulamasını oluşturun. KIMLIK uygulaması, [Azure AD](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app) veya [Active Directory Federasyon Hizmetleri (AD FS)](azure-stack-app-service-before-you-get-started.md#create-an-active-directory-federation-services-app) IÇIN yapılabilir ve uygulama kimliğini kaydedebilir.
- Windows Server 2016 Datacenter görüntüsünü Azure Stack Market 'e eklemiş olduğunuzdan emin olun. Bu görüntü App Service yüklemesi için gereklidir.

### <a name="steps-for-app-service-deployment"></a>App Service dağıtım adımları
App Service kaynak sağlayıcısını yüklemek en az bir saat sürer. Gereken sürenin uzunluğu, kaç tane rol örneğine dağıtım yapmanız gerektiğini gösterir. Dağıtım sırasında, yükleyici aşağıdaki görevleri çalıştırır:

- Belirtilen Azure Stack depolama hesabında bir blob kapsayıcısı oluşturun.
- App Service için bir DNS bölgesi ve girişleri oluşturun.
- App Service kaynak sağlayıcısını kaydedin.
- App Service galeri öğelerini kaydedin.

App Service kaynak sağlayıcısını dağıtmak için şu adımları izleyin:

1. Daha önce indirilen App Service yükleyicisini (**appservice. exe**), Azure Stack Yöneticisi Azure Kaynak Yönetimi uç noktasına erişebilen bir bilgisayardan yönetici olarak çalıştırın.

2. App Service Dağıt ' ı seçin **veya en son sürüme yükseltin**.

    ![App Service dağıtma veya yükseltme](media/app-service-deploy-ha/01.png)

3. Microsoft lisans koşulları 'nı kabul edin ve **İleri**' ye tıklayın.

    ![App Service Microsoft Lisanslama Koşulları](media/app-service-deploy-ha/02.png)

4. Microsoft dışı lisans koşullarını kabul edin ve **İleri**' ye tıklayın.

    ![App Service Microsoft dışı lisans koşulları](media/app-service-deploy-ha/03.png)

5. Azure Stack ortamınız için App Service bulut uç noktası yapılandırmasını sağlayın.

    ![App Service üzerinde App Service bulut uç noktası yapılandırması](media/app-service-deploy-ha/04.png)

6. Yükleme için kullanılacak Azure Stack aboneliğine **bağlanın** ve konumu seçin. 

    ![App Service Azure Stack aboneliğine bağlanma](media/app-service-deploy-ha/05.png)

7. Yüksek oranda kullanılabilir şablonu dağıtmak için kullanılan kaynak grubu için **mevcut VNET ve alt ağları** ve **kaynak grubu adını** kullan ' ı seçin.<br><br>Ardından, şablon dağıtımının bir parçası olarak oluşturulan sanal ağı seçin ve ardından aşağı açılan liste seçeneklerinden uygun rol alt ağlarını seçin. 

    ![App Service sanal ağ seçimi](media/app-service-deploy-ha/06.png)

8. Dosya paylaşma yolu ve dosya paylaşımının sahibi parametreleri için önceden kaydedilen şablon çıkışları bilgilerini sağlayın. İşiniz bittiğinde **İleri**' ye tıklayın.

    ![App Service dosya paylaşma çıkış bilgileri](media/app-service-deploy-ha/07.png)

9. App Service yüklemek için kullanılan makine, App Service dosya paylaşımının barındırıldığında kullanılan dosya sunucusuyla aynı VNet üzerinde bulunmadığından, adı çözeyükleyemezsiniz. **Bu hata beklenen davranıştır**.<br><br>Dosya paylaşımının UNC yolu ve hesap bilgileri için girilen bilgisinin doğru olduğundan emin olun. Sonra App Service yüklemeye devam etmek için uyarı iletişim kutusunda **Evet** ' e basın.

    ![App Service için beklenen hata iletişim kutusu](media/app-service-deploy-ha/08.png)

    Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, giden bir güvenlik kuralı eklemeniz gerekir. Bu kural, çalışan alt ağı ve dosya sunucusu arasında SMB trafiğine izin vermez. Yönetici portalında WorkersNsg adresine gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:
    - Kaynak: Any
    - Kaynak bağlantı noktası aralığı: *
    - Hedef: IP Adresleri
    - Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
    - Hedef bağlantı noktası aralığı: 445
    - Protokol: TCP
    - Eylem: Allow
    - Öncelik: 700
    - Ad: Outbound_Allow_SMB445

10. Kimlik sertifikalarına kimlik uygulama KIMLIĞINI ve yolu ve parolaları sağlayın ve **İleri**' ye tıklayın:
    - Kimlik uygulama sertifikası ( **SSO. appservice. Local. azurestack. external. pfx**biçiminde)
    - Azure Resource Manager kök sertifika (**Azurestackcertificationauthority. cer**)

    ![App Service KIMLIK uygulama sertifikası ve kök sertifikası](media/app-service-deploy-ha/008.png)

11. Ardından, aşağıdaki sertifikalar için kalan gerekli bilgileri sağlayın ve **İleri**' ye tıklayın:
    - Varsayılan Azure Stack SSL sertifikası ( **_. appservice. Local. azurestack. external. pfx**biçiminde)
    - API SSL sertifikası ( **API. appservice. Local. azurestack. external. pfx**biçiminde)
    - Yayımcı sertifikası ( **FTP. appservice. Local. azurestack. external. pfx**biçiminde) 

    ![App Service ek yapılandırma sertifikaları](media/app-service-deploy-ha/09.png)

12. Yüksek kullanılabilirlik şablonu dağıtım çıktılarından SQL Server bağlantı bilgilerini kullanarak SQL Server bağlantı bilgilerini girin:

    ![App Service bağlantı bilgilerini SQL Server](media/app-service-deploy-ha/10.png)

13. App Service yüklemek için kullanılan makine, App Service veritabanlarını barındırmak için kullanılan SQL Server ile aynı VNet üzerinde bulunmadığından, adı gidereyükleyemezsiniz.  **Bu beklenen davranıştır**.<br><br>SQL Server adı ve hesap bilgileri için girilen bilgilerinizin doğru olduğundan emin olun ve App Service yüklemeye devam etmek için **Evet** ' e basın. **İleri**’ye tıklayın.

    ![App Service bağlantı bilgilerini SQL Server](media/app-service-deploy-ha/11.png)

14. Varsayılan rol yapılandırma değerlerini kabul edin veya önerilen değerlere değiştirin ve **İleri**' ye tıklayın.<br><br>App Service altyapısı rol örnekleri için varsayılan değerlerin, yüksek kullanılabilirliğe sahip yapılandırmalarda aşağıdaki şekilde değiştirilmesini öneririz:

    |Role|Varsayılan|Yüksek oranda kullanılabilir öneri|
    |-----|-----|-----|
    |Denetleyici rolü|2|2|
    |Yönetim rolü|1\.|3|
    |Yayımcı rolü|1\.|3|
    |Ön uç rolü|1\.|3|
    |Paylaşılan çalışan rolü|1\.|10|
    |     |     |     |

    ![App Service altyapı rolü örnek değerleri](media/app-service-deploy-ha/12.png)

    > [!NOTE]
    > Varsayılan değerlerden bu tutoral için önerilen olarak değiştirilmesi, App Service yüklemek için donanım gereksinimlerini arttırır. 15 VM için varsayılan 18 çekirdek ve 32.256 MB RAM yerine önerilen 21 VM 'yi desteklemek için toplam 26 çekirdek ve 46.592 MB RAM gereklidir.

15. App Service altyapısı VM 'lerini yüklemek için kullanılacak platform görüntüsünü seçin ve **İleri**' ye tıklayın:

    ![App Service platform görüntüsü seçimi](media/app-service-deploy-ha/13.png)

16. Kullanılacak App Service altyapı rolü kimlik bilgileri sağlayın ve **İleri**' ye tıklayın:

    ![App Service 'de altyapı rolü kimlik bilgileri](media/app-service-deploy-ha/14.png)

17. App Service dağıtmak için kullanılacak bilgileri gözden geçirin ve dağıtıma başlamak için **İleri** ' ye tıklayın.

    ![App Service yükleme özetini gözden geçirin](media/app-service-deploy-ha/15.png)

18. App Service dağıtımı ilerlemesini gözden geçirin. Bu dağıtım, belirli dağıtım yapılandırmanıza ve donanımınıza bağlı olarak bir saatten fazla sürebilir. Yükleyici başarıyla tamamlandıktan sonra **Çıkış**' ı seçin.

    ![App Service için Kurulum tamamlanmıştır](media/app-service-deploy-ha/16.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir SQL Always on örneğiyle App Service kaynak sağlayıcısı sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna ekleyin](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) . Veritabanı yük devretmesi durumunda herhangi bir hizmet kaybını engellemek için veritabanlarını eşitler. Ayrıca, özgün birincil sunucudan bir yük devretme sunucusuna AppServices oturumlarını içeri aktarmak için bir [komut dosyası](https://blog.sqlauthority.com/2017/11/30/sql-server-alwayson-availability-groups-script-sync-logins-replicas/) çalıştırabilirsiniz.

[Ölçeği genişletme App Service](azure-stack-app-service-add-worker-roles.md). Ortamınızdaki beklenen uygulama taleplerini karşılamak için ek App Service altyapı rolü çalışanları eklemeniz gerekebilir. Varsayılan olarak, Azure Stack App Service ücretsiz ve paylaşılan çalışan katmanlarını destekler. Diğer çalışan katmanlarını eklemek için daha fazla çalışan rolü eklemeniz gerekir.

[Dağıtım kaynaklarını yapılandırın](azure-stack-app-service-configure-deployment-sources.md). GitHub, BitBucket, OneDrive ve DropBox gibi birden çok kaynak denetimi sağlayıcısından isteğe bağlı dağıtımı desteklemek için ek yapılandırma gerekir.

[Yedekleme App Service](app-service-back-up.md). App Service başarıyla dağıttıktan ve yapılandırıldıktan sonra, olağanüstü durum kurtarma için gerekli tüm bileşenlerin yedeklendiğinden emin olmanız gerekir. Temel bileşenlerinizi yedeklemek, kurtarma işlemleri sırasında veri kaybını ve gereksiz hizmet kesintilerini önlemeye yardımcı olur.
