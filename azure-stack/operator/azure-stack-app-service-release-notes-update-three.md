---
title: App Service Azure Stack güncelleştirme 3 sürüm notları | Microsoft Docs
description: Azure Stack App Service için güncelleştirme 3 ' teki geliştirmeler, düzeltmeler ve bilinen sorunlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 08/20/2018
ms.openlocfilehash: d226be9bad3bd6ddf775d8415329ea1fa8099eb0
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159552"
---
# <a name="app-service-on-azure-stack-update-3-release-notes"></a>App Service Azure Stack güncelleştirme 3 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure Stack güncelleştirme 3 ' teki Azure App Service geliştirmeleri, düzeltmeleri ve bilinen sorunları anlatmaktadır. Bilinen sorunlar üç bölüme ayrılmıştır: doğrudan dağıtım, güncelleştirme işlemiyle ilgili sorunlar ve yapıyla ilgili sorunlar (yükleme sonrası).

> [!IMPORTANT]
> 1807 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,3 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 3 derleme numarasında App Service **74.0.13698.31**.

### <a name="prerequisites"></a>Önkoşullar

Dağıtıma başlamadan önce [Azure Stack App Service dağıtmaya yönelik önkoşulları](azure-stack-app-service-before-you-get-started.md) inceleyin.

Azure App Service Azure Stack 1,3 sürümüne yükseltmeye başlamadan önce, Azure Stack yönetici portalındaki Azure App Service yönetimine tüm rollerin hazırlandığından emin olun.

![App Service rol durumu](media/azure-stack-app-service-release-notes-update-three/image01.png)

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 3 ' Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- Azure App Service kaynak sağlayıcısı veritabanları için her zaman açık SQL Server kullanımı için destek.

- Farklı Azure AD bölgelerini hedeflemeye yardımcı olmak için Create-AADIdentityApp Helper betiğine yeni ortam parametresi eklendi.

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.11820**'a güncelleştirir.

- Genel sorunların daha kolay tanılanmasını sağlayan güvenilirlik ve hata iletilerini artırmak için çekirdek hizmette güncelleştirmeler.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
  - ASP.NET Core 2.1.2 'yi eklendi
  - NodeJS 10.0.0 eklendi
  - Zulu OpenJDK 8.30.0.1 eklendi
  - Tomcat 8.5.31 ve 9.0.8 eklendi
  - PHP sürümleri eklendi:
    - 5.6.36
    - 7.0.30
    - 7.1.17
    - 7.2.5
  - WinCache 2.0.0.8 eklendi
  - Windows için git, v 2.17.1.2 'ye güncelleştirildi
  - Kudu 74.10611.3437 'e güncelleştirildi
  
- **Tüm rollerin temeldeki işletim sisteminde güncelleştirmeler**:
  - [X64 tabanlı sistemler için Windows Server 2016 için hizmet yığını güncelleştirmesi (KB4132216)](https://support.microsoft.com/help/4132216/servicing-stack-update-for-windows-10-1607-may-17-2018)
  - [2018-07 x64 tabanlı sistemler için Windows Server 2016 toplu güncelleştirmesi (KB4338822)](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)

### <a name="post-update-steps-optional"></a>Güncelleştirme sonrası adımlar (isteğe bağlı)

Azure Stack dağıtımlarında mevcut Azure App Service için kapsanan bir veritabanına geçiş isteyen müşteriler için Azure Stack 1,3 güncelleştirme tamamlandıktan Azure App Service sonra şu adımları yürütün:

> [!IMPORTANT]
> Bu yordam yaklaşık 5-10 dakika sürer. Bu yordam, mevcut veritabanı oturum açma oturumlarını sonlandırmasını içerir. Azure Stack geçiş sonrası Azure App Service geçirmek ve doğrulamak için kapalı kalma süresini planlayın
>
>

1. [Appservice veritabanlarını (appservice_hosting ve appservice_metering) bir kullanılabilirlik grubuna](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)ekleyin.

1. Kapsanan veritabanını etkinleştirin.
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. Bir veritabanını kısmen içerir olarak dönüştürme. Bu adım, tüm etkin oturumların sonlandıryüklenmesi gerektiği için kapalı kalma süresine neden olur.

    ```sql
        /******** [appservice_metering] Migration Start********/
            USE [master];

            -- kill all active sessions
            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_metering')

            EXEC(@kill);

            USE [master]  
            GO  
            ALTER DATABASE [appservice_metering] SET CONTAINMENT = PARTIAL  
            GO  

        /********[appservice_metering] Migration End********/

        /********[appservice_hosting] Migration Start********/

            -- kill all active sessions
            USE [master];

            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_hosting')

            EXEC(@kill);

            -- Convert database to contained
            USE [master]  
            GO  
            ALTER DATABASE [appservice_hosting] SET CONTAINMENT = PARTIAL  
            GO  

            /********[appservice_hosting] Migration End********/
    '''

1. Migrate logins to contained database users.

    ```sql
        IF EXISTS(SELECT * FROM sys.databases WHERE Name=DB_NAME() AND containment = 1)
        BEGIN
        DECLARE @username sysname ;  
        DECLARE user_cursor CURSOR  
        FOR
            SELECT dp.name
            FROM sys.database_principals AS dp  
            JOIN sys.server_principals AS sp
                ON dp.sid = sp.sid  
                WHERE dp.authentication_type = 1 AND dp.name NOT IN ('dbo','sys','guest','INFORMATION_SCHEMA');
            OPEN user_cursor  
            FETCH NEXT FROM user_cursor INTO @username  
                WHILE @@FETCH_STATUS = 0  
                BEGIN  
                    EXECUTE sp_migrate_user_to_contained
                    @username = @username,  
                    @rename = N'copy_login_name',  
                    @disablelogin = N'do_not_disable_login';  
                FETCH NEXT FROM user_cursor INTO @username  
            END  
            CLOSE user_cursor ;  
            DEALLOCATE user_cursor ;
            END
        GO
    ```

**Doğrulamalısınız**

1. SQL Server kapsama özelliğinin etkin olup olmadığını denetleyin.

    ```sql
        sp_configure  @configname='contained database authentication'
    ```

1. İçerilen mevcut davranışı denetleyin.
    ```sql
        SELECT containment FROM sys.databases WHERE NAME LIKE (SELECT DB_NAME())
    ```

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Mevcut bir sanal ağda App Service dağıtıldığında ve dosya sunucusu yalnızca özel ağda kullanılabilir olduğunda çalışanlar dosya sunucusuna ulaşamamakta olur. Bu sorun Azure Stack dağıtım belgelerindeki Azure App Service çağırılır.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği sağlayan bir giden güvenlik kuralı eklemeniz gerekir. Yönetici portalında WorkersNsg adresine gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

 * Kaynak: Any
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP adresleri
 * Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: Allow
 * Öncelik: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Cloud Admins işletim Azure App Service ilgili bilinen sorunlar

Azure Stack 1807 sürüm notlarındaki belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için bkz. [Azure Stack üzerinde App Service dağıtmak Için Önkoşullar](azure-stack-app-service-before-you-get-started.md).
