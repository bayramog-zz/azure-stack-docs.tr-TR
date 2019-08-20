---
title: Azure Stack kurtarma App Service | Microsoft Docs
description: Azure Stack App Service için olağanüstü durum kurtarma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: 82498781e83aedf13a3ba33da24f484bc7e80d4b
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579019"
---
# <a name="app-service-recovery-on-azure-stack"></a>Azure Stack kurtarma App Service

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Bu konuda App Service olağanüstü durum kurtarma için hangi eylemlerin uygulanması için yönergeler sağlanmaktadır.

Azure Stack yedekten App Service kurtarmak için aşağıdaki eylemler gerçekleştirilmelidir:
1. App Service veritabanlarını geri yükleyin.
2. Dosya sunucusu paylaşma içeriğini geri yükleyin.
3. App Service rollerini ve hizmetleri geri yükleyin.

Işlev uygulamaları depolaması için Azure Stack depolama kullanılmışsa, Işlev uygulamalarını geri yüklemek için de gerekli adımları uygulamanız gerekir.

## <a name="restore-the-app-service-databases"></a>App Service veritabanlarını geri yükleme
App Service SQL Server veritabanları, üretime Ready bir SQL Server örneğine geri yüklenmelidir. 

App Service veritabanlarını barındırmak için [SQL Server örneğini hazırladıktan](azure-stack-app-service-before-you-get-started.md#prepare-the-sql-server-instance) sonra, veritabanlarını yedekten geri yüklemek için aşağıdaki adımları kullanın:

1. Kurtarılan App Service veritabanlarını barındıracak SQL Server oturum açmak için yönetici izinlerine sahip olmanız gerekir.
2. App Service veritabanlarını yönetici izinleriyle çalışan bir komut isteminden geri yüklemek için aşağıdaki komutları kullanın:
    ```dos
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_hosting FROM DISK='<full path to backup>' WITH REPLACE"
    sqlcmd -U <SQL admin login> -P <SQL admin password> -Q "RESTORE DATABASE appservice_metering FROM DISK='<full path to backup>' WITH REPLACE"
    ```
3. App Service veritabanlarının her ikisi de başarıyla geri yüklendi ve SQL Server Management Studio çıkış olduğunu doğrulayın.

> [!NOTE]
> Yük devretme kümesi örneği hatasından kurtarmak için bkz. [Yük devretme kümesi örneğinden Kurtarma hatası](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/recover-from-failover-cluster-instance-failure?view=sql-server-2017). 

## <a name="restore-the-app-service-file-share-content"></a>App Service dosya paylaşma içeriğini geri yükleme
[Dosya sunucusunu](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server) App Service dosya paylaşımından barındıracak şekilde hazırladıktan sonra, kiracı dosya paylaşma içeriğini yedekten geri yüklemeniz gerekir. Dosyaları yeni oluşturulan App Service dosya paylaşımının konumuna kopyalamak için kullanabileceğiniz herhangi bir yöntemi kullanabilirsiniz. Bu örneği dosya sunucusunda çalıştırmak, uzak bir paylaşıma bağlanmak ve dosyaları paylaşıma kopyalamak için PowerShell ve Robocopy kullanır:

```powershell
$source = "<remote backup storage share location>"
$destination = "<local file share location>"
net use $source /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy /E $source $destination
net use $source /delete
```

Dosya paylaşımının içeriğini kopyalamaya ek olarak, dosya paylaşımının üzerindeki izinleri de sıfırlamanız gerekir. İzinleri sıfırlamak için, dosya sunucusu bilgisayarında bir yönetici komut istemi açın ve **Reacl. cmd** dosyasını çalıştırın. **Reacl. cmd** dosyası, **bcdr** dizinindeki App Service yükleme dosyalarında bulunur.

## <a name="restore-app-service-roles-and-services"></a>App Service rollerini ve hizmetleri geri yükleme
App Service veritabanları ve dosya paylaşma içeriği geri yüklendikten sonra, App Service rollerini ve hizmetlerini geri yüklemek için bir sonraki PowerShell kullanmanız gerekir. Bu adımlar App Service gizli dizileri ve hizmet yapılandırmasını geri yükler.  

1. App Service yükleme sırasında verdiğiniz parolayı kullanarak **RoleAdmin** olarak App Service Controller **CN0-VM** VM 'de oturum açın. 
    > [!TIP]
    > RDP bağlantılarına izin vermek için VM 'nin ağ güvenlik grubunu değiştirmeniz gerekir. 
2. **Systemgizlilikler. JSON** dosyasını denetleyici VM 'sine yerel olarak kopyalayın. Sonraki adımda `$pathToExportedSecretFile` parametresi olarak bu dosyanın yolunu sağlamanız gerekir.
3. App Service rollerini ve hizmetleri geri yüklemek için yükseltilmiş bir PowerShell konsol penceresinde aşağıdaki komutları kullanın:

    ```powershell
    # Stop App Service services on the primary controller VM
    net stop WebFarmService
    net stop ResourceMetering
    net stop HostingVssService # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # Restore App Service secrets. Provide the path to the App Service secrets file copied from backup. For example, C:\temp\SystemSecrets.json.
    # Press ENTER when prompted to reconfigure App Service from backup 

    # If necessary, use -OverrideDatabaseServer <restored server> with Restore-AppServiceStamp when the restored database server has a different address than backed-up deployment.
    # If necessary, use -OverrideContentShare <restored file share path> with Restore-AppServiceStamp when the restored file share has a different path from backed-up deployment.
    Restore-AppServiceStamp -FilePath $pathToExportedSecretFile 

    # Restore App Service roles
    Restore-AppServiceRoles

    # Restart App Service services
    net start WebFarmService
    net start ResourceMetering
    net start HostingVssService  # This service was deprecated in the App Service 1.5 release and is not required after the App Service 1.4 release.

    # After App Service has successfully restarted, and at least one management server is in ready state, synchronize App Service objects to complete the restore
    # Enter Y when prompted to get all sites and again for all ServerFarm entities.
    Get-AppServiceSite | Sync-AppServiceObject
    Get-AppServiceServerFarm | Sync-AppServiceObject
    ```

> [!TIP]
> Komut tamamlandığında bu PowerShell oturumunun kapatılması kesinlikle önerilir.

## <a name="restore-function-apps"></a>Işlev uygulamalarını geri yükleme 
Azure Stack için App Service, Kiracı Kullanıcı uygulamalarının veya dosya paylaşma içeriği dışındaki verilerin geri yüklenmesini desteklemez. Diğer tüm veriler App Service yedekleme ve geri yükleme işlemleri dışında yedeklenmesi ve kurtarılması gerekir. Işlev uygulamaları depolaması için Azure Stack depolama kullanılmışsa, kayıp verileri kurtarmak için aşağıdaki adımlar gerçekleştirilmelidir:

1. İşlev Uygulaması tarafından kullanılacak yeni bir depolama hesabı oluşturun. Bu depolama alanı, depolama, Azure depolama veya uyumlu herhangi bir depolama Azure Stack olabilir.
2. Depolama için bağlantı dizesini alın.
3. İşlev portalını açın ve işlev uygulamasına gidin.
4. **Platform özellikleri** sekmesine gidin ve **uygulama ayarları**' na tıklayın.
5. **AzureWebJobsDashboard** ve **AzureWebJobsStorage** öğesini yeni bağlantı dizesine değiştirin ve **Kaydet**' e tıklayın.
6. **Genel Bakış ' a**geçin.
7. Uygulamayı yeniden başlatın. Tüm hataları temizlemek birkaç denemeden kaynaklanabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack’te App Service’a genel bakış](azure-stack-app-service-overview.md)