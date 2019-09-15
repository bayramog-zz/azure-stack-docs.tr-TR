---
title: App Service Azure Stack güncelleştirme 5 sürüm notları | Microsoft Docs
description: Azure Stack App Service için güncelleştirme 5 ' teki geliştirmeler, düzeltmeler ve bilinen sorunlar hakkında bilgi edinin.
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
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 4adad49b27b1ab1d255ccc566c95b003cfd09b3b
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974910"
---
# <a name="app-service-on-azure-stack-update-5-release-notes"></a>App Service Azure Stack güncelleştirme 5 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure Stack güncelleştirme 5 ' teki Azure App Service geliştirmeleri, düzeltmeleri ve bilinen sorunları anlatmaktadır. Bilinen sorunlar üç bölüme ayrılmıştır: doğrudan dağıtım, güncelleştirme işlemiyle ilgili sorunlar ve yapıyla ilgili sorunlar (yükleme sonrası).

> [!IMPORTANT]
> 1901 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,5 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 5 derleme numarasında App Service **80.0.2.15**.

### <a name="prerequisites"></a>Önkoşullar

Dağıtıma başlamadan önce [Azure Stack App Service dağıtmaya yönelik önkoşulları](azure-stack-app-service-before-you-get-started.md) inceleyin.

Azure App Service Azure Stack 1,5 sürümüne yükseltmeye başlamadan önce:

- Tüm rollerin Azure Stack yönetici portalındaki Azure App Service yönetimi için kullanılabilir olduğundan emin olun.

- App Service ve ana veritabanlarını yedekleyin:
  - AppService_Hosting;
  - AppService_Metering;
  - Ana Şablon

- Kiracı uygulaması içerik dosyası payını yedekleyin.

- **Özel Betik uzantısı** sürüm **1.9.1** Azure Marketi 'nden genel olarak yayımlar.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 5 ' te Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.12205**'a güncelleştirir.

- , **Bağlı** olmayan Azure Stack kullanıcılara yönelik stil oluşturma ve işlevlerle ilgili sorunları gidermek Için **kudu araçlarının** güncelleştirmeleri. 

- Genel sorunların daha kolay tanılanmasını sağlayan güvenilirlik ve hata iletilerini artırmak için çekirdek hizmette güncelleştirmeler.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
  - ASP.NET Core 2.1.6 ve 2.2.0 eklendi
  - NodeJS 10.14.1 eklendi
  - NPM 6.4.1 eklendi
  - Kudu 79.20129.3767 'e güncelleştirildi
  
- **Tüm rollerin temeldeki işletim sisteminde güncelleştirmeler**:
  - [2019-02 x64 tabanlı sistemler için Windows Server 2016 toplu güncelleştirmesi (KB4487006)](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006)

### <a name="post-deployment-steps"></a>Dağıtım sonrası adımlar

> [!IMPORTANT]  
> App Service kaynak sağlayıcısını bir SQL Always on örneğiyle birlikte sağladıysanız, [appservice_hosting ve appservice_metering veritabanlarını bir kullanılabilirlik grubuna eklemeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) ve veritabanlarını kullanarak hizmet kaybını önleyebilirsin bir veritabanı yük devretmesinin olayı.

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

Azure Stack dağıtımlarında mevcut Azure App Service için kapsanan bir veritabanına geçiş isteyen müşteriler için Azure Stack 1,5 güncelleştirme tamamlandıktan Azure App Service sonra şu adımları yürütün:

> [!IMPORTANT]
> Geçiş yordamı yaklaşık 5-10 dakika sürer. Yordam, mevcut veritabanı oturum açma oturumlarını sonlandırmasını içerir. Azure Stack geçiş sonrası Azure App Service geçirmek ve doğrulamak için kapalı kalma süresini planlayın. Azure Stack 1,3 Azure App Service güncelleştirme sonrasında bu adımları tamamladıysanız, bu adımlar gerekli değildir.

1. [Appservice veritabanlarını (appservice_hosting ve appservice_metering) bir kullanılabilirlik grubuna](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)ekleyin.

1. Kapsanan veritabanını etkinleştirin.
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. Bir veritabanını kısmen dahil olarak dönüştürmek, tüm etkin oturumların sonlandıryüklenmesi gerektiği için dönüştürme kapalı kalma süresine neden olur.

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

[Azure Stack 1809 sürüm notlarındaki](azure-stack-update-1903.md)belgelere başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için bkz. [Azure Stack üzerinde App Service dağıtmak Için Önkoşullar](azure-stack-app-service-before-you-get-started.md).
