---
title: Azure Stack içinde yüksek oranda kullanılabilir SQL veritabanları sunun
description: Azure Stack ile SQL Server kaynak sağlayıcısı ana bilgisayarı ve yüksek oranda kullanılabilir SQL AlwaysOn veritabanları oluşturmayı öğrenin.
services: azure-stack
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 10/07/2019
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: e5866a80367a826dd58aa39109ebbbbd9f2edce6
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283330"
---
# <a name="offer-highly-available-sql-databases"></a>Yüksek oranda kullanılabilir SQL veritabanları sunun

Azure Stack operatörü olarak sunucu VM 'lerini SQL Server veritabanlarını barındıracak şekilde yapılandırabilirsiniz. SQL barındırma sunucusu başarıyla oluşturulduktan ve Azure Stack tarafından yönetiliyorsa, SQL Services 'e abone olan kullanıcılar kolayca SQL veritabanı oluşturabilir.

Bu makalede, bir [SQL Server AlwaysOn kullanılabilirlik grubu](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)oluşturmak için bir Azure Stack hızlı başlangıç şablonu kullanma, bu dosyayı Azure Stack SQL barındırma sunucusu olarak ekleme ve yüksek oranda KULLANILABILIR bir SQL veritabanı oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.

Öğrenirsiniz:

> [!div class="checklist"]
> * Şablondan SQL Server AlwaysOn kullanılabilirlik grubu oluşturma
> * Azure Stack SQL barındırma sunucusu oluşturma
> * Yüksek oranda kullanılabilir SQL veritabanı oluşturma

İki VM SQL Server AlwaysOn kullanılabilirlik grubu, kullanılabilir Azure Stack Market öğeleri kullanılarak oluşturulur ve yapılandırılır. 

Başlamadan önce, [SQL Server kaynak sağlayıcısının](azure-stack-sql-resource-provider-deploy.md) başarıyla yüklendiğinden ve Azure Stack Market 'te aşağıdaki öğelerin bulunduğundan emin olun:

> [!IMPORTANT]
> Azure Stack hızlı başlangıç şablonu kullanılması için aşağıdakiler gereklidir.

- [Windows Server 2016 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/MicrosoftWindowsServer.WindowsServer) Market görüntüsü.
- Windows Server 2016 Server görüntüsünde 2016 SP1 veya SP2 (Enterprise, Standard veya Developer) SQL Server. Bu makalede, [Windows Server 2016 Market görüntüsünde SQL Server 2016 SP2 Enterprise](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016) kullanılmaktadır.
- [IaaS uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) sürüm 1.2.30 veya üstünü SQL Server. SQL IaaS uzantısı, tüm Windows sürümleri için Market SQL Server öğeleri için gereken gerekli bileşenleri yüklüyor. SQL sanal makinelerinde SQL 'e özgü ayarların yapılandırılmasını sağlar. Uzantı yerel market 'te yüklü değilse SQL sağlama başarısız olur.
- Windows sürüm 1.9.1 veya üzeri [Için özel Betik uzantısı](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.CustomScriptExtension) . Özel Betik uzantısı, dağıtım sonrası VM Özelleştirme görevlerini otomatik olarak başlatmak için kullanılabilen bir araçtır.
- [PowerShell Istenen durum yapılandırması (DSC)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.DSC-arm) sürüm 2.76.0.0 veya üzeri. DSC, Windows PowerShell 'de, yazılım hizmetleri için yapılandırma verilerinin dağıtılmasına ve yönetilmesine ve bu hizmetlerin çalıştırıldığı ortamı yönetmeye izin veren bir yönetim platformudur.

Azure Stack Market 'e öğe ekleme hakkında daha fazla bilgi için bkz. [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md).

## <a name="create-a-sql-server-alwayson-availability-group"></a>SQL Server AlwaysOn kullanılabilirlik grubu oluşturma
[SQL-2016-AlwaysOn Azure Stack hızlı başlangıç şablonunu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-alwayson)kullanarak SQL Server AlwaysOn kullanılabilirlik grubunu dağıtmak için bu bölümdeki adımları kullanın. Bu şablon, her zaman açık kullanılabilirlik grubunda iki SQL Server Enterprise, standart veya geliştirici örneği dağıtır. Aşağıdaki kaynakları oluşturur:

- Bir ağ güvenlik grubu
- Bir sanal ağ
- Dört depolama hesabı (bir adet Active Directory (AD), biri SQL için, biri dosya paylaşma tanığı ve diğeri VM tanılamaları için)
- Dört genel IP adresi (biri AD, her SQL sanal makinesi için ikisi ve SQL AlwaysOn dinleyicisine bir genel yük dengeleyici için)
- SQL VM 'Leri için SQL AlwaysOn dinleyicisine bağlantılı genel IP içeren bir dış yük dengeleyici
- Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan bir VM (Windows Server 2016)
- SQL Server 2016 SP1 veya SP2 Enterprise, Standard veya Developer Edition ve kümelenmiş ile yapılandırılmış iki VM (Windows Server 2016). Bunlar Market görüntüleri olmalıdır.
- Küme için dosya paylaşma tanığı olarak yapılandırılmış bir VM (Windows Server 2016)
- SQL ve dosya paylaşımında tanık VM 'Leri içeren bir kullanılabilirlik kümesi  

1. 
   [!INCLUDE [azs-admin-portal](../includes/azs-admin-portal.md)]

2. **@No__t-1** ' i seçin  > **özel** **oluştur**' u ve ardından **şablon dağıtımı**.

   ![Özel şablon dağıtımı](media/azure-stack-tutorial-sqlrp/1.png)


3. **Özel dağıtım** dikey penceresinde, **Şablonu Düzenle** > **hızlı başlangıç şablonu** ' nu seçin ve ardından kullanılabilir özel şablonların açılan listesini kullanarak **SQL-2016-AlwaysOn** şablonunu seçin, **Tamam**' a tıklayın ve ardından  **Kaydedin**.

   [![](media/azure-stack-tutorial-sqlrp/2-sm.PNG "Hızlı başlangıç şablonunu seçin")](media/azure-stack-tutorial-sqlrp/2-lg.PNG#lightbox)

4. **Özel dağıtım** dikey penceresinde, **parametreleri Düzenle** ' yi seçin ve varsayılan değerleri gözden geçirin. Gerekli tüm parametre bilgilerini sağlamak için değerleri gereken şekilde değiştirin ve ardından **Tamam**' a tıklayın.<br><br> En azından:

    - ADMINPASSWORD, SQLSERVERSERVICEACCOUNTPASSWORD ve SQLAUTHPASSWORD parametreleri için karmaşık parolalar sağlayın.
    - DNSSUFFIX parametresi (ASDK yüklemeleri için**azurestack. external** ) için tüm küçük harflerden ters arama Için DNS sonekini girin.
    
   [![](media/azure-stack-tutorial-sqlrp/3-sm.PNG "Özel dağıtım parametrelerini Düzenle")](media/azure-stack-tutorial-sqlrp/3-lg.PNG#lightbox)

5. **Özel dağıtım** dikey penceresinde, kullanılacak aboneliği seçin ve yeni bir kaynak grubu oluşturun veya özel dağıtım için var olan bir kaynak grubunu seçin.<br><br> Sonra, kaynak grubu konumunu (ASDK yüklemeleri için**Yerel** ) seçin ve ardından **Oluştur**' a tıklayın. Özel dağıtım ayarları doğrulanacak ve ardından dağıtım başlatılır.

    [![](media/azure-stack-tutorial-sqlrp/4-sm.PNG "Özel dağıtım oluştur")](media/azure-stack-tutorial-sqlrp/4-lg.PNG#lightbox)


6. Yönetim portalında, **kaynak grupları** ' nı ve ardından özel dağıtım için oluşturduğunuz kaynak grubunun adını (Bu örnek için**kaynak grubu** ) seçin. Tüm dağıtımların başarıyla tamamlandığından emin olmak için dağıtımın durumunu görüntüleyin.<br><br>Sonra, kaynak grubu öğelerini gözden geçirin ve **Sqlpipsql @ no__t-1resource Group Name @ no__t-2** genel IP adresi öğesini seçin. Genel IP adresini ve yük dengeleyici genel IP 'sinin tam FQDN 'sini kaydedin. Bunu bir Azure Stack Işlecine sağlamanız gerekir, böylelikle bu SQL AlwaysOn kullanılabilirlik grubundan yararlanan bir SQL barındırma sunucusu oluşturamazlar.

   > [!NOTE]
   > Şablon dağıtımının tamamlanması birkaç saat sürer.

   ![Özel dağıtım Tamam](./media/azure-stack-tutorial-sqlrp/5.png)

### <a name="enable-automatic-seeding"></a>Otomatik dengeli dağıtımı etkinleştir
Şablon başarıyla dağıtıldıktan ve SQL AlwaysON kullanılabilirlik grubunu yapılandırdıktan sonra, kullanılabilirlik grubundaki her bir SQL Server örneğinde [Otomatik dengeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) dağıtımı etkinleştirmeniz gerekir. 

Otomatik dengeli dağıtım ile bir kullanılabilirlik grubu oluşturduğunuzda SQL Server, AlwaysOn veritabanlarının yüksek kullanılabilirliğini sağlamak için herhangi bir el ile müdahale gerektirmeden gruptaki her veritabanı için ikincil çoğaltmaları otomatik olarak oluşturur.

AlwaysOn kullanılabilirlik grubu için otomatik dengeli dağıtımı yapılandırmak üzere bu SQL komutlarını kullanın. @No__t-0ınstancename @ no__t-1 ' i birincil örnek SQL Server adı ve < availability_group_name >, gereken şekilde AlwaysOn kullanılabilirlik grubu adıyla değiştirin. 

Birincil SQL örneğinde:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON '<InstanceName>'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

>  ![Birincil SQL örneği betiği](./media/azure-stack-tutorial-sqlrp/sql1.png)

İkincil SQL örneklerinde:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```
>  ![İkincil SQL örneği betiği](./media/azure-stack-tutorial-sqlrp/sql2.png)

### <a name="configure-contained-database-authentication"></a>Kapsanan veritabanı kimlik doğrulamasını yapılandırma
Kapsanan bir veritabanını bir kullanılabilirlik grubuna eklemeden önce, kapsanan veritabanı kimlik doğrulama sunucusu seçeneğinin kullanılabilirlik grubu için bir kullanılabilirlik çoğaltması barındıran her sunucu örneğinde 1 olarak ayarlandığından emin olun. Daha fazla bilgi için bkz. [Kapsanan Veritabanı kimlik doğrulaması](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Kullanılabilirlik grubundaki her bir SQL Server örneği için kapsanan veritabanı kimlik doğrulama sunucusu seçeneğini ayarlamak için şu komutları kullanın:

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```
>  ![Kapsanan veritabanı kimlik doğrulamasını ayarla](./media/azure-stack-tutorial-sqlrp/sql3.png)

## <a name="create-an-azure-stack-sql-hosting-server"></a>Azure Stack SQL barındırma sunucusu oluşturma
SQL Server AlwayOn kullanılabilirlik grubu oluşturulduktan ve düzgün şekilde yapılandırıldıktan sonra Azure Stack, kullanıcıların veritabanı oluşturmalarına yönelik ek kapasiteyi kullanılabilir hale getirmek için bir Azure Stack SQL barındırma sunucusu oluşturmanız gerekir. 

SQL AlwaysOn kullanılabilirlik grubunun kaynak grubu oluşturulduğunda daha önce kaydedilen SQL yük dengeleyicinin genel IP 'si için genel IP veya tam FQDN 'yi kullandığınızdan emin olun (**Sqlpipsql @ no__t-1kaynak grubu adı @ no__t-2**). Ayrıca, AlwaysOn kullanılabilirlik grubundaki SQL örneklerine erişmek için kullanılan SQL Server kimlik doğrulama kimlik bilgilerini bilmeniz gerekir.

> [!NOTE]
> Bu adımın Azure Stack yönetim portalından bir Azure Stack Işleci tarafından çalıştırılması gerekir.

SQL AlwaysOn kullanılabilirlik grubunun yük dengeleyici dinleyicisi genel IP ve SQL kimlik doğrulaması oturum açma bilgileri ile bir Azure Stack operatörü artık [SQL AlwaysOn kullanılabilirlik grubunu kullanarak BIR SQL barındırma sunucusu oluşturabilir](azure-stack-sql-resource-provider-hosting-servers.md#provide-high-availability-using-sql-always-on-availability-groups). 

Ayrıca, SQL AlwaysOn veritabanı oluşturmayı kullanıcılar için kullanılabilir hale getirmek için planlar ve teklifler oluşturduğunuzdan emin olun. Operatör, **Microsoft. SqlAdapter** hizmetini bir plana eklemesi ve yüksek oranda kullanılabilir veritabanları için özel olarak yeni bir kota oluşturmanız gerekecektir. Plan oluşturma hakkında daha fazla bilgi için bkz. [hizmet, plan, teklif, aboneliğe genel bakış](service-plan-offer-subscription-overview.md).

> [!TIP]
> **Microsoft. SqlAdapter** hizmeti, [SQL Server kaynak sağlayıcısı dağıtılana](azure-stack-sql-resource-provider-deploy.md)kadar planlara eklemek için kullanılamaz.

## <a name="create-a-highly-available-sql-database"></a>Yüksek oranda kullanılabilir SQL veritabanı oluşturma
SQL AlwaysOn kullanılabilirlik grubu oluşturulduktan, yapılandırıldıktan ve bir Azure Stack Işleci tarafından Azure Stack bir SQL barındırma sunucusu olarak eklendikten sonra, SQL Server veritabanı özellikleri dahil olmak üzere aboneliği olan bir Kiracı Kullanıcı, destekleyen SQL veritabanları oluşturabilir Bu bölümdeki adımları izleyerek AlwaysOn işlevselliği. 

> [!NOTE]
> Azure Stack Kullanıcı portalından, SQL Server özellikleri (Microsoft. SQLAdapter hizmeti) sağlayan abonelikle bir kiracı kullanıcısı olarak bu adımları çalıştırın.

1. 
   [!INCLUDE [azs-user-portal](../includes/azs-user-portal.md)]

2. **@No__t-1** ' i seçin  > **veri \+ depolama alanı**ve sonra **SQL veritabanı** **oluşturun**.<br><br>Dağıtım için kullanılacak ad, harmanlama, maksimum boyut ve abonelik, kaynak grubu ve konum dahil olmak üzere gerekli veritabanı özellik bilgilerini sağlayın. 

   ![SQL veritabanı oluştur](./media/azure-stack-tutorial-sqlrp/createdb1.png)

3. **SKU** ' yı seçin ve ardından kullanılacak uygun SQL BARıNDıRMA sunucusu SKU 'sunu seçin. Bu örnekte Azure Stack Işleci, SQL AlwaysOn kullanılabilirlik grupları için yüksek kullanılabilirliği destekleyecek şekilde **Enterprise-ha** SKU 'su oluşturmuştur.

   ![SKU seçin](./media/azure-stack-tutorial-sqlrp/createdb2.png)

4. @No__t **oturum açma**seçin-1**Yeni bir oturum açma oluşturun** ve sonra yenı veritabanı için kullanılacak SQL kimlik doğrulama kimlik bilgilerini sağlayın. İşiniz bittiğinde, **Tamam** ' a tıklayın ve ardından **Oluştur** ' a tıklayarak veritabanı dağıtım sürecini başlatın.

   ![Oturum açma oluştur](./media/azure-stack-tutorial-sqlrp/createdb3.png)

5. SQL veritabanı dağıtımı başarıyla tamamlandığında, yeni yüksek oranda kullanılabilir veritabanına bağlanırken kullanılacak bağlantı dizesini saptamak için veritabanı özelliklerini gözden geçirin. 

   ![Bağlantı dizesini görüntüle](./media/azure-stack-tutorial-sqlrp/createdb4.png)




## <a name="next-steps"></a>Sonraki adımlar

[SQL kaynak sağlayıcısını güncelleştirme](azure-stack-sql-resource-provider-update.md)
