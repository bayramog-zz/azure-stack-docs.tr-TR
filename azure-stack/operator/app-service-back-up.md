---
title: Azure Stack App Service yedekleme | Microsoft Docs
description: Azure Stack üzerinde uygulama hizmetlerini nasıl yedekleyeceğinizi öğrenin.
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
ms.date: 04/23/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/21/2019
ms.openlocfilehash: a41943a598545b1a4c5dbe6325307a8fa3594cd5
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975031"
---
# <a name="back-up-app-service-on-azure-stack"></a>Azure Stack App Service yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Bu belge Azure Stack App Service nasıl yedekleyeceğiniz hakkında yönergeler sağlar.

> [!IMPORTANT]
> Azure Stack App Service, [Azure Stack altyapı yedeklemesinin](azure-stack-backup-infrastructure-backup.md)bir parçası olarak yedeklenmez. Azure Stack operatörü olarak, gerekirse App Service başarıyla kurtarılabilmesini sağlamak için adımlar uygulamanız gerekir.

Azure Stack Azure App Service olağanüstü durum kurtarma planlaması yaparken dikkate alınması gereken dört ana bileşen vardır:
1. Kaynak Sağlayıcısı Altyapısı; sunucu rolleri, çalışan katmanları ve benzeri. 
2. App Service gizli dizileri.
3. App Service SQL Server barındırma ve ölçüm veritabanları.
4. App Service Kullanıcı iş yükü içeriği App Service dosya paylaşımında depolanır.

## <a name="back-up-app-service-secrets"></a>App Service gizli dizileri yedekleme
Yedeklemeden App Service kurtarılırken ilk dağıtım tarafından kullanılan App Service anahtarlarını sağlamanız gerekir. App Service başarılı bir şekilde dağıtılıp, güvenli bir konuma depolandıktan sonra bu bilgiler kaydedilmelidir. Kaynak sağlayıcısı altyapı yapılandırması, kurtarma sırasında App Service Recovery PowerShell cmdlet 'leri kullanılarak yedekten yeniden oluşturulur.

Aşağıdaki adımları izleyerek App Service gizli dizilerini yedeklemek için yönetim portalını kullanın: 

1. Hizmet Yöneticisi olarak Azure Stack yönetici portalında oturum açın.

2. **App Service** -> **gizli**anahtarlara gidin. 

3. **Gizli dizileri indir**' i seçin.

   ![Azure Stack yönetici portalında gizli dizileri indirin](./media/app-service-back-up/download-secrets.png)

4. Gizli dizileri indirmeye hazırsanız, **Kaydet** ' e tıklayın ve App Service gizli dizi (**SYSTEMGIZLILIKLER. JSON**) dosyasını güvenli bir konumda saklayın. 

   ![Azure Stack yönetici portalında gizli dizileri kaydetme](./media/app-service-back-up/save-secrets.png)

> [!NOTE]
> App Service gizli dizileri her döndürüldüğünde bu adımları yineleyin.

## <a name="back-up-the-app-service-databases"></a>App Service veritabanlarını yedekleme
App Service geri yüklemek için **Appservice_hosting** ve **Appservice_metering** veritabanı yedeklerinin olması gerekir. Bu veritabanlarının yedeklenmesini ve düzenli olarak güvenli bir şekilde kaydedilmesini sağlamak için SQL Server bakım planlarını veya Azure Backup Sunucusu kullanmanızı öneririz. Ancak, düzenli SQL yedeklemelerinin oluşturulmasını sağlamaya yönelik herhangi bir yöntem kullanılabilir.

SQL Server oturum açarken bu veritabanlarını el ile yedeklemek için aşağıdaki PowerShell komutlarını kullanın:

  ```powershell
  $s = "<SQL Server computer name>"
  $u = "<SQL Server login>" 
  $p = read-host "Provide the SQL admin password"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_hosting TO DISK = '<path>\hosting.bak'"
  sqlcmd -S $s -U $u -P $p -Q "BACKUP DATABASE appservice_metering TO DISK = '<path>\metering.bak'"
  ```

> [!NOTE]
> SQL AlwaysOn veritabanlarını yedeklemeniz gerekiyorsa, [Bu yönergeleri](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server?view=sql-server-2017)izleyin. 

Tüm veritabanları başarıyla yedeklendikten sonra,. bak dosyalarını App Service gizli dizi bilgileriyle birlikte güvenli bir konuma kopyalayın.

## <a name="back-up-the-app-service-file-share"></a>App Service dosya paylaşımının yedeklenmesi
App Service, kiracı uygulama bilgilerini dosya paylaşımında depolar. Bu dosya paylaşımının, geri yükleme gerektiğinde olabildiğince az veri kaybolması için App Service veritabanlarıyla birlikte düzenli olarak yedeklenmesi gerekir.

App Service dosya paylaşma içeriğini yedeklemek için, dosya paylaşma içeriğini önceki tüm kurtarma bilgilerini kaydettiğiniz konuma düzenli olarak kopyalamak üzere Azure Backup Sunucusu veya başka bir yöntem kullanın.

Örneğin, bir Windows PowerShell (PowerShell ıSE) konsol oturumundan Robocopy kullanmak için bu adımları kullanabilirsiniz:

```powershell
$source = "<file share location>"
$destination = "<remote backup storage share location>"
net use $destination /user:<account to use to connect to the remote share in the format of domain\username> *
robocopy $source $destination
net use $destination /delete
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack App Service geri yükleme](app-service-recover.md)