---
title: Azure Stack 'de SQL barındırma sunucuları | Microsoft Docs
description: SQL bağdaştırıcısı kaynak sağlayıcısı aracılığıyla sağlama için SQL örnekleri ekleme.
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: ce89ffdee0e0de2db8109102418f4513ce1cb99a
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418156"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı için barındırma sunucuları ekleme

SQL kaynak sağlayıcısı örneğe bağlanabilmiş olduğu sürece [Azure Stack](azure-stack-overview.md)veya Azure Stack ortamınızın dışındaki bir sanal makinede bulunan bir sanal MAKINEDE (vm) SQL Server veritabanı barındırma sunucuları oluşturabilirsiniz.

> [!NOTE]
> SQL barındırma sunucularının faturalandırılabilir, Kullanıcı aboneliğinde oluşturulması sırasında SQL kaynak sağlayıcısı varsayılan sağlayıcı aboneliğinde oluşturulmalıdır. Kaynak sağlayıcısı sunucusu, Kullanıcı veritabanlarını barındırmak için kullanılmamalıdır.

## <a name="overview"></a>Genel Bakış

Bir SQL barındırma sunucusu eklemeden önce, aşağıdaki zorunlu ve genel gereksinimleri gözden geçirin.

### <a name="mandatory-requirements"></a>Zorunlu gereksinimler

* SQL Server örneğinde SQL kimlik doğrulamasını etkinleştirin. SQL kaynak sağlayıcısı VM 'si etki alanına katılmış olmadığından, SQL kimlik doğrulaması kullanılarak yalnızca bir barındırma sunucusuna bağlanabilir.
* Azure Stack yüklendiğinde, SQL örneklerinin IP adreslerini ortak olarak yapılandırın. Kaynak sağlayıcısı ve Web Apps gibi kullanıcılar Kullanıcı ağı üzerinden iletişim kurarak, bu ağdaki SQL örneğine yönelik bağlantı gereklidir.

### <a name="general-requirements"></a>Genel gereksinimler

* SQL örneğini kaynak sağlayıcısı ve Kullanıcı iş yükleri tarafından kullanılmak üzere atayın. Başka bir tüketici tarafından kullanılan bir SQL örneğini kullanamazsınız. Bu kısıtlama uygulama hizmetleri için de geçerlidir.
* Kaynak sağlayıcısı için uygun ayrıcalık düzeylerine sahip bir hesap yapılandırın (aşağıda açıklanmıştır).
* SQL örneklerinin ve konaklarının yönetilmesi sizin sorumluluğunuzdadır.  Örneğin, kaynak sağlayıcısı güncelleştirme uygulamaz, yedeklemeleri işlemez veya kimlik bilgisi döndürmesini işlemez.

### <a name="sql-server-virtual-machine-images"></a>Sanal makine görüntülerini SQL Server

SQL IaaS sanal makine görüntüleri Market yönetim özelliği aracılığıyla kullanılabilir. Bu görüntüler, Azure 'da kullanılabilen SQL VM 'leriyle aynıdır.

Market öğesi kullanarak bir SQL VM 'yi dağıtmadan önce **SQL IaaS uzantısının** en son sürümünü her zaman indirdiğinizden emin olun. IaaS uzantısı ve ilgili Portal geliştirmeleri, otomatik düzeltme eki uygulama ve yedekleme gibi ek özellikler sağlar. Bu uzantı hakkında daha fazla bilgi için bkz. [SQL Server Agent uzantılı Azure sanal makinelerinde yönetim görevlerini otomatikleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

> [!NOTE]
> Market 'teki tüm Windows yansımaları için SQL IaaS uzantısı _gereklidir_ ; uzantıyı indirmediyseniz VM dağıtımı başarısız olur. Linux tabanlı SQL sanal makine görüntülerinde kullanılmaz.

SQL VM 'Leri dağıtmak için [Azure Stack hızlı başlangıç galerisindeki](https://github.com/Azure/AzureStack-QuickStart-Templates)şablonlar da dahil olmak üzere başka seçenekler vardır.

> [!NOTE]
> Çok düğümlü Azure Stack yüklü olan barındırma sunucularının, varsayılan sağlayıcı aboneliği değil, bir Kullanıcı aboneliğinden oluşturulması gerekir. Bunların Kullanıcı portalından veya uygun bir oturum açma ile bir PowerShell oturumundan oluşturulması gerekir. Tüm barındırma sunucuları faturalandırılabilir sanal makineler ve uygun SQL lisanslarına sahip olmalıdır. Hizmet _Yöneticisi,_ bu aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

SQL sysadmin 'ten daha düşük ayrıcalıklara sahip bir yönetici kullanıcı oluşturabilirsiniz. Kullanıcı yalnızca aşağıdaki işlemler için izinlere ihtiyaç duyuyor:

* Veritabanı: Kapsama Ile oluşturma, değiştirme (yalnızca açık için), bırakma, yedekleme
* Kullanılabilirlik grubu: Veritabanını Değiştir, Birleştir, Ekle/Kaldır
* Oturum aç: Oluşturma, seçme, değiştirme, bırakma, Iptal etme
* İşlemler: \[ana\]öğesini seçin\[ . sys\].\[ availability_group_listeners\] (AlwaysOn), sys. availability_replicas (AlwaysOn), sys. databases, \[Master\].\[ sys\].\[ dm_os_sys_memory\], ServerProperty, \[Master\].\[ sys\].\[ availability_groups\] (AlwaysOn), sys. master_files

### <a name="additional-security-information"></a>Ek güvenlik bilgileri

Aşağıdaki bilgiler ek güvenlik kılavuzu sağlar:

* Tüm Azure Stack depolama BitLocker kullanılarak şifrelenir, bu nedenle Azure Stack üzerindeki tüm SQL örnekleri şifreli BLOB depolama alanı kullanır.
* SQL kaynak sağlayıcısı, TLS 1,2 ' i tam olarak destekler. SQL RP aracılığıyla yönetilen tüm SQL Server _yalnızca_ TLS 1,2 için yapılandırıldığından emin olun ve RP varsayılan değer olarak ayarlanır. Tüm desteklenen SQL Server TLS 1,2 sürümleri, bkz. [Microsoft SQL Server Için tls 1,2 desteği](https://support.microsoft.com/en-us/help/3135244/tls-1-2-support-for-microsoft-sql-server).
* SQL Server 'a yönelik tüm iletişimlerin her zaman şifrelendiğinden emin olmak için **ForceEncryption** seçeneğini ayarlamak için SQL Server Yapılandırma Yöneticisi kullanın. [Sunucuyu şifreli bağlantıları zorlayacak şekilde yapılandırmak için](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine?view=sql-server-2017#to-configure-the-server-to-force-encrypted-connections)bkz.
* Aynı zamanda şifrelenmiş bir bağlantı üzerinden iletişim kurduğu tüm istemci uygulamalarının da bulunduğundan emin olun.
* RP, SQL Server örnekleri tarafından kullanılan sertifikalara güvenecek şekilde yapılandırılmıştır.

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Tek başına bir SQL Server barındırma sunucusuna bağlanarak kapasite sağlama

SQL Server 2014 veya SQL Server 2016 ' nin herhangi bir sürümünü kullanarak tek başına (HA olmayan) SQL Server 'lar kullanabilirsiniz. Sysadmin ayrıcalıklarına sahip bir hesabın kimlik bilgilerine sahip olduğunuzdan emin olun.

Zaten ayarlanmış bir tek başına barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Azure Stack operatör portalında hizmet yöneticisi olarak oturum açın.

2. **Tüm hizmetler** &gt; **Yönetim kaynakları** &gt; **SQL barındırma sunucuları**' nı seçin.

   ![SQL barındırma sunucuları](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   **SQL barındırma sunucuları**altında, SQL kaynak sağlayıcısını kaynak sağlayıcının arka ucu olarak kullanılacak SQL Server örneklerine bağlayabilirsiniz.

   ![SQL bağdaştırıcı panosu](./media/azure-stack-sql-rp-deploy/sqlrp-hostingserver.png)

3. **Ekle** ' ye tıklayın ve ardından **SQL barındırma sunucusu ekle** dikey penceresinde SQL Server Örneğiniz için bağlantı ayrıntılarını sağlayın.

   ![SQL barındırma sunucusu ekleme](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    İsteğe bağlı olarak, bir örnek adı sağlayın ve örnek varsayılan 1433 bağlantı noktasına atanmamışsa bir bağlantı noktası numarası belirtin.

   > [!NOTE]
   > SQL örneğine Kullanıcı ve yönetici Azure Resource Manager tarafından erişilebileceği sürece, kaynak sağlayıcının denetimi altına yerleştirilebilir. SQL örneği,  kaynak sağlayıcısına özel olarak ayrılmalıdır.

4. Sunucu eklerken, bunları mevcut bir SKU 'ya atamanız veya yeni bir SKU oluşturmanız gerekir. **Barındırma sunucusu Ekle**altında **SKU 'lar**' ı seçin.

   * Mevcut bir SKU 'yu kullanmak için kullanılabilir bir SKU seçin ve ardından **Oluştur**' u seçin.
   * Bir SKU oluşturmak için **+ yenı SKU oluştur**' u seçin. **SKU oluştur**bölümünde gerekli bilgileri girip **Tamam**' ı seçin.

     ![SKU oluşturma](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>SQL Always on kullanılabilirlik gruplarını kullanarak yüksek kullanılabilirlik sağlama

SQL Always on örneklerinin yapılandırılması ek adımlar gerektirir ve üç VM (veya fiziksel makine) gerektirir. Bu makalede, her zaman açık kullanılabilirlik grupları hakkında daha fazla bilginiz olduğunu varsaymış olursunuz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure sanal makinelerinde SQL Server Always on kullanılabilirlik grupları ile tanışın](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always on kullanılabilirlik grupları (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> SQL bağdaştırıcı _kaynak sağlayıcısı,_ her zaman açık kullanılabilirlik GRUPLARı için SQL 2016 SP1 Enterprise veya sonraki örneklerini destekler. Bu bağdaştırıcı yapılandırması, otomatik dağıtım gibi yeni SQL özellikleri gerektirir.

### <a name="automatic-seeding"></a>Otomatik dengeli dağıtım

Her bir SQL Server örneği için her bir kullanılabilirlik grubunda [Otomatik dengeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) dağıtımı etkinleştirmeniz gerekir.

Tüm örneklerde otomatik dengeli dağıtımı etkinleştirmek için, her ikincil örnek için birincil çoğaltmada aşağıdaki SQL komutunu düzenleyin ve çalıştırın:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON '<secondary_node>'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Kullanılabilirlik grubu köşeli ayraç içine alınmalıdır.

İkincil düğümlerde aşağıdaki SQL komutunu çalıştırın:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="configure-contained-database-authentication"></a>Kapsanan veritabanı kimlik doğrulamasını yapılandırma

Kapsanan bir veritabanını bir kullanılabilirlik grubuna eklemeden önce, kapsanan veritabanı kimlik doğrulama sunucusu seçeneğinin kullanılabilirlik grubu için bir kullanılabilirlik çoğaltması barındıran her sunucu örneğinde 1 olarak ayarlandığından emin olun. Daha fazla bilgi için bkz. [Kapsanan Veritabanı kimlik doğrulama sunucusu yapılandırma seçeneği](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Her örnek için kapsanan veritabanı kimlik doğrulama sunucusu seçeneğini ayarlamak için şu komutları kullanın:

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>SQL Always on barındırma sunucuları eklemek için

1. Azure Stack yönetim portalında hizmet yöneticisi olarak oturum açın.

2. &gt; **Yönetim kaynaklarına**  &gt;   gözatıp SQL barındırma sunucuları + Ekle ' yi seçin. &gt;

   **SQL barındırma sunucuları**altında, SQL Server kaynak sağlayıcısını kaynak sağlayıcının arka ucu olarak görev yapan gerçek SQL Server örneklerine bağlayabilirsiniz.

3. SQL Server Örneğiniz için bağlantı ayrıntılarıyla formu doldurun. Her zaman açık dinleyicinin FQDN adresini (ve isteğe bağlı bağlantı noktası numarası ve örnek adı) kullandığınızdan emin olun. Sysadmin ayrıcalıklarıyla yapılandırdığınız hesabın bilgilerini sağlayın.

4. SQL Always on kullanılabilirlik grubu örnekleri desteğini etkinleştirmek için Always on kullanılabilirlik grubu kutusunu işaretleyin.

   ![Her zaman açık etkinleştir](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. SQL Always on örneğini bir SKU 'ya ekleyin.

   > [!IMPORTANT]
   > Tek başına sunucuları aynı SKU 'daki her zaman açık örneklerle karıştıramazsınız. İlk barındırma sunucusu eklendikten sonra türleri karıştırmaya çalışmak bir hataya neden olur.

## <a name="sku-notes"></a>SKU notları
SKU 'da kapasite ve performans gibi sunucuların yeteneklerini açıklayan bir SKU adı kullanın. Ad, kullanıcıların veritabanlarını uygun SKU 'ya dağıtmalarına yardımcı olmak için bir yardım görevi görür. Örneğin, hizmet tekliflerini aşağıdaki özelliklerle ayırt etmek için SKU adlarını kullanabilirsiniz:
  
* yüksek kapasite
* yüksek performanslı
* Yüksek kullanılabilirlik

En iyi uygulama olarak, bir SKU 'daki tüm barındırma sunucuları aynı kaynak ve performans özelliklerine sahip olmalıdır.

SKU 'Lar belirli kullanıcılara veya gruplara atanamaz.

SKU 'Ların portalda görünür olması bir saate kadar sürebilir. Kullanıcılar, SKU tam olarak oluşturuluncaya kadar bir veritabanı oluşturamaz.

Bir SKU 'yu düzenlemek için **tüm hizmetler** > **SQL bağdaştırıcısı** > **SKU 'larına**gidin. Değiştirilecek SKU 'yu seçin, tüm gerekli değişiklikleri yapın ve değişiklikleri kaydetmek için **Kaydet** ' e tıklayın. 

Artık gerekli olmayan bir SKU 'yu silmek için **tüm hizmetler** > **SQL bağdaştırıcısı** > **SKU 'larına**gidin. SKU adına sağ tıklayın ve Sil ' i **seçerek silin** .

> [!IMPORTANT]
> Yeni SKU 'Ların Kullanıcı portalında kullanılabilmesi bir saate kadar sürebilir.

## <a name="make-sql-databases-available-to-users"></a>SQL veritabanlarını kullanıcılara açık hale getirme

SQL veritabanlarını kullanıcılar için kullanılabilir hale getirmek için planlar ve teklifler oluşturun. Plana **Microsoft. SqlAdapter** hizmetini ekleyin ve yeni bir kota oluşturun.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Veritabanı Ekle](azure-stack-sql-resource-provider-databases.md)
