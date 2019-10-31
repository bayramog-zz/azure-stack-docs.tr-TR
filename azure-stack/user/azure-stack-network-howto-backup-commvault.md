---
title: SANAL cihazınızı, Commkasasıyla Azure Stack yedekleme | Microsoft Docs
description: SANAL cihazınızı, Commkasasıyla Azure Stack nasıl yedekleyeceğinizi öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/19/2019
ms.openlocfilehash: 553b6af0e61067b4223baee100bd1a9b3079d1f1
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73168119"
---
# <a name="back-up-your-vm-on-azure-stack-with-commvault"></a>Sanal makineyi Commkasasıyla Azure Stack yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="overview-of-backing-up-a-vm-with-commvault"></a>Bir VM 'yi Commkasala yedeklemeye genel bakış

Bu makalede, ayrı bir Azure Stack ölçek biriminde bulunan bir kurtarma VM 'sini güncelleştirmek üzere Commkasali canlı eşitleme 'nin yapılandırılması gösterilmektedir. Bu makalede, Azure Stack üzerinde dağıtılan sanal makinelerin verilerini ve sistem durumunu korumak ve kurtarmak için ortak bir iş ortağı çözümünün nasıl yapılandırılacağı açıklanır.

Aşağıdaki diyagramda, sanal makinelerinizi yedeklemek için Commkasasını kullanırken genel çözüm gösterilmektedir.

![](./media/azure-stack-network-howto-backup-commvault/bcdr-commvault-overall-arc.png)

Bu makalede şunları yapmanız gerekir:

1. Kaynak Azure Stack örneğiniz üzerinde Commkasa yazılımını çalıştıran bir VM oluşturun.

2. İkincil bir konumda depolama hesabı oluşturun. Makale, bir Azure Stack örneğindeki (hedef) bir depolama hesabında bir blob kapsayıcısı oluşturacağınız ve hedef Azure Stack kaynak Azure Stack ulaşılabilir olduğunu varsayar.

3. Kaynak Azure Stack örneğiniz üzerinde Commkasasını yapılandırın ve VM grubuna kaynak Azure Stack VM 'Ler ekleyin.

4. Commkasasının Lifessync 'i yapılandırın.

Ayrıca, Azure Stack VM 'lerinizi bir Azure bulutuna veya başka bir Azure Stack korumak için uyumlu iş ortağı VM görüntülerini indirebilir ve sunabilirsiniz. Bu makalede, Commkasali canlı eşitleme ile VM koruması gösterilmektedir.

Bu yaklaşımın topolojisi aşağıdaki diyagram gibi görünür:

![](./media/azure-stack-network-howto-backup-commvault/backup-vm-commvault-diagram.png)

## <a name="create-the-commvault-vm-form-the-commvault-marketplace-item"></a>Commkasasyon sanal makinesi oluşturma Market öğesi

1. Azure Stack Kullanıcı portalını açın.

2. **İşlem > işlem** > **kaynak oluştur** 'u seçin.

    > [!Note]  
    > Commkasası sizin için kullanılabilir değilse, bulut işletmenizin ile iletişime geçin.

    ![](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-01.png)

3. Sanal makine oluşturma bölümünde temel ayarları yapılandırın **, 1 temel bilgiler**:

    a. Bir **ad**girin.

    b. **Standart HHD**'yi seçin.
    
    c. Bir **Kullanıcı adı**girin.
    
    d. Bir **parola**girin.
    
    e. Parolanızı onaylayın.
    
    f. Yedekleme için bir **abonelik** seçin.
    
    g. Bir **kaynak grubu**seçin.
    
    h. Azure Stack **konumunu** seçin. Bir ASDK kullanıyorsanız, **Yerel**' i seçin.
    
    i. **Tamam**’ı seçin.

    ![](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-02.png)

4. Commkasa VM 'sinin boyutunu seçin. Yedekleme için VM boyutu, en az 10 GB RAM ve 100 GB depolama olmalıdır.

    ![](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-03.png).

5. Commkasa sanal makinesi için ayarları seçin.

    a. Kullanılabilirliği **hiçbiri**olarak ayarlayın.
    
    b. Yönetilen diskleri kullanmak için **Evet** ' i seçin.
    
    c. **Sanal ağ**Için varsayılan VNET ' i seçin.
    
    d. Varsayılan **alt ağı**seçin.
    
    e. Varsayılan **genel IP adresini**seçin.
    
    f. VM 'yi **temel** ağ güvenlik grubunda bırakın.
    
    g. HTTP (80), HTTPS (443), SSH (22) ve RDP (3389) bağlantı noktalarını açın.
    
    h. **Uzantı yok**' u seçin.
    
    i. **Önyükleme tanılaması**için **etkin** ' i seçin.
    
    j. **Konuk işletim sistemi tanılamayı** **devre dışı**olarak ayarlayın.
    
    k. Varsayılan **Tanılama depolama hesabı**' nı bırakın.
    
    girişindeki. **Tamam**’ı seçin.

6. Doğrulama başarılı olduktan sonra, Commkasaızın sanal makinenizin özetini gözden geçirin. **Tamam**’ı seçin.

## <a name="get-your-service-principal"></a>Hizmet sorumlunuzu alın

Kimlik yöneticinizin Azure AD veya AD DFS olduğunu bilmeniz gerekir. Aşağıdaki tabloda, Azure Stack Commkasasını ayarlamanız için gereken bilgiler yer almaktadır.

| Öğe | Açıklama | Kaynak |
|--------------------------|--------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Resource Manager URL 'SI | Azure Stack Kaynak Yöneticisi uç noktası. | https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-ruby?view=azs-1908#the-azure-stack-resource-manager-endpoint |
| Uygulama adı |  |  |
| Uygulama Kimliği | Hizmet sorumlusu bu makalenin önceki bölümünde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI. | https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals?view=azs-1908 |
| Abonelik Kimliği | Azure Stack tekliflere erişmek için abonelik KIMLIĞINI kullanırsınız. | https://docs.microsoft.com/azure-stack/operator/service-plan-offer-subscription-overview?view=azs-1908#subscriptions |
| Kiracı KIMLIĞI (dizin KIMLIĞI) | Azure Stack kiracı KIMLIĞINIZ. | https://docs.microsoft.com/azure-stack/operator/azure-stack-identity-overview?view=azs-1908 |
| Uygulama parolası | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı. | https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals?view=azs-1908 |

## <a name="configure-backup-using-the-commvault-console"></a>Commkasa konsolunu kullanarak yedeklemeyi yapılandırma

1. RDP istemcinizi açın ve Azure Stack Commavult sanal makinesine bağlanın. Kimlik bilgilerinizi girin.

2. Azure Stack PowerShell ve Azure Stack araçları 'nı Commkasa VM 'ye yükler.

    a. Azure Stack PowerShell 'i yükleme yönergeleri için bkz. [PowerShell 'i Azure Stack Için yükleme](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fbreadcrumb%2Ftoc.json).  
    b. Azure Stack araçlarını yüklemeyle ilgili yönergeler için bkz. [GitHub 'dan Azure Stack araçları indirme](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-download?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fuser%2FTOC.json%3Fview%3Dazs-1908&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure-stack%2Fbreadcrumb%2Ftoc.json%3Fview%3Dazs-1908&view=azs-1908).

3. Commkasasını, Commkasasındaki sanal makinenize yükledikten sonra, Commcell konsolunu açın. Başlangıçtan itibaren **, commkasa > ** Commkasası **commcell konsolunu**seçin.

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-console.png)

4. Yedekleme depolarınızı, Commkasacommcell konsolundaki Azure Stack harici depolamayı kullanacak şekilde yapılandırın. CommCell tarayıcısında, depolama havuzları > depolama kaynakları ' nı seçin. Sağ tıklayın ve **depolama havuzu Ekle** ' yi seçin. **Bulut**’u seçin.

5. Depolama havuzunun adını ekleyin. **İleri**’yi seçin.

6.  > **bulut depolaması** **Oluştur** ' u seçin.

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-storage-add-storage-device.png)

7. Bulut hizmeti sağlayıcınızı seçin. Bu yordamda, farklı bir konumda ikinci bir Azure Stack kullanacağız. Microsoft Azure Depolama seçin.

8. MediaAgent sanal ağınız olarak Commkasasını VM 'nizi seçin.

9. Depolama hesabınız için erişim bilgilerinizi girin. Burada bir Azure depolama hesabı ayarlama yönergeleri bulabilirsiniz. Erişim bilgileri:

    -  **Hizmet ana bilgisayarı**: kaynağınızın blob KAPSAYıCı özelliklerinden URL 'nin adını alın. Örneğin, URL 'im https://backuptest.blob.westus.stackpoc.com/mybackups ve kullanıldım, hizmet ana bilgisayarında blob.westus.stackpoc.com.
    
    -   **Hesap adı**: depolama hesabı adını kullanın. Bunu, depolama kaynağındaki erişim tuşları dikey penceresinde bulabilirsiniz.
    
    -   **Erişim anahtarı**: depolama kaynağındaki erişim tuşları dikey penceresinden erişim anahtarını alın.
    
    -   **Kapsayıcı**: kapsayıcının adı. Bu durumda, MyBackups.
    
    -   **Depolama sınıfı**: Kullanıcı kapsayıcısının varsayılan depolama sınıfı olarak bırakın.

10. [Microsoft Azure Stack Istemci oluşturma](https://documentation.commvault.com/commvault/v11_sp13/article?p=86495.htm) yönergelerini Izleyerek Microsoft Azure Stack istemci oluşturma

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-ceate-client.png)

11. Korumak ve bir yedekleme ilkesi eklemek için VM 'Leri veya kaynak gruplarını seçin.

12. Yedekleme zamanlamanızı, kurtarma için RPO gereksinimlerinize uyacak şekilde yapılandırın.

13. İlk tam yedeklemeyi gerçekleştirin.

## <a name="configure-commvault-live-sync"></a>Commkasasını canlı eşitlemeyi yapılandırma 

İki seçenek mevcuttur. Değişiklikleri birincil kopyadan çoğaltmayı veya ikincil bir kopyadan kurtarma VM 'sine çoğaltmayı seçebilirsiniz. Bir yedekleme kümesinden çoğaltmak, kaynak makinedeki Okuma GÇ etkisini ortadan kaldırır.

1. Canlı eşitleme yapılandırması sırasında, kaynak Azure Stack (sanal sunucu Aracısı) ve hedef Azure Stack ayrıntılarını sağlamanız gerekir.

2. Commkasak canlı eşitlemesini yapılandırma adımları için bkz. [Microsoft Azure Stack Için canlı eşitleme çoğaltması](https://documentation.commvault.com/commvault/v11_sp13/article?p=94386.htm).

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-1.png)
 
3. Canlı eşitleme yapılandırması sırasında, hedef Azure Stack ve sanal sunucu Aracısı ayrıntılarını sağlamanız gerekir.

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-2.png)

4. Yapılandırmaya devam edin ve çoğaltma disklerinin barındırılacak hedef depolama hesabını, çoğaltma sanal makinelerinin yerleştirileceği kaynak grupları ve çoğaltma VM 'lerine eklemek istediğiniz adı ekleyin.

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-3.png)

5. Ayrıca, VM boyutunu değiştirebilir ve her bir VM 'nin yanındaki **Yapılandır** öğesini seçerek ağ ayarlarını yapılandırabilirsiniz.

6. Hedefe çoğaltma sıklığını ayarlayın Azure Stack

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-5.png)

7. Yapılandırmayı kaydetmek için ayarlarınızı gözden geçirin. Daha sonra kurtarma ortamı oluşturulur ve çoğaltma, seçilen aralıkta başlayacaktır.


## <a name="set-up-failover-behavior-using-live-sync"></a>Canlı eşitleme kullanarak yük devretme davranışı ayarlama

Commkasali canlı eşitleme, makinelerin bir Azure Stack diğerine yük devretmesini ve özgün Azure Stack işlem işlemlerine yeniden çalışma olanağı sağlar. İş akışı otomatikleştirilebilir ve günlüğe kaydedilir.

![](./media/azure-stack-network-howto-backup-commvault/back-up-live-sync-panel.png)

Kurtarma Azure Stack yük devretmek istediğiniz VM 'Leri seçin ve planlı veya planlanmamış bir yük devretme seçin. Planlı Yük devretme, kurtarma sitesindeki işlemlere devam etmeden önce üretim ortamının sorunsuz bir şekilde kapatılması sırasında uygundur. Planlı Yük devretme üretim sanal makinelerini kapatır, kurtarma sitesinde son değişiklikleri çoğaltır ve kurtarma VM 'lerini en son verilerle çevrimiçi hale getirir ve canlı eşitleme yapılandırması sırasında belirtilen VM boyutunu ve ağ yapılandırmasını uygular. Planlanmamış bir yük devretme üretim VM 'lerini kapatmaya çalışır, ancak üretim ortamı kullanılamıyorsa devam eder ve VM 'ye ve boyuta ve ağa uygulanan son alınan çoğaltma verileri kümesiyle birlikte kurtarma VM 'lerini çevrimiçi duruma getirir. yapılandırma daha önce seçildi. Aşağıdaki görüntüler, kurtarma VM 'lerinin Commkasalive Sync tarafından çevrimiçi hale getirildiği planlanmamış bir yük devretme göstermektedir.

![](./media/azure-stack-network-howto-backup-commvault/unplanned-failover.png)

![](./media/azure-stack-network-howto-backup-commvault/fail-over-2.png)

![](./media/azure-stack-network-howto-backup-commvault/fail-over-3.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  