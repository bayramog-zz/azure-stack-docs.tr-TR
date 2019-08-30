---
title: Çevrimdışı güncelleştirme Azure App Service | Microsoft Docs
description: Azure Stack çevrimdışı Azure App Service güncelleştirme hakkında ayrıntılı kılavuz
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
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.openlocfilehash: 5482d078e4edbda10806220c989909424d9fcd95
ms.sourcegitcommit: 701685f0b59e5a3d1a8d39fe477b8df701a51cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70159584"
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Azure Stack Azure App Service çevrimdışı güncelleştirmesi

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> 1907 güncelleştirmesini veya üstünü Azure Stack tümleşik sisteminize uygulayın veya en son Azure Stack geliştirme setini Azure App Service 1,7 ' i dağıtmadan önce dağıtın.

Bu makaledeki yönergeleri izleyerek, aşağıdaki bir Azure Stack ortamında dağıtılan [App Service kaynak sağlayıcısını](azure-stack-app-service-overview.md) yükseltebilirsiniz:

* Internet 'e bağlı değil
* Active Directory Federasyon Hizmetleri (AD FS) tarafından güvenli hale getirilir (AD FS).

> [!IMPORTANT]
> Yükseltmeyi çalıştırmadan önce, [Azure Stack kaynak sağlayıcısında Azure App Service dağıtımını](azure-stack-app-service-deploy-offline.md) zaten tamamladığınızdan ve yeni hakkında bilgi edinmek için 1,7 sürümüyle birlikte gelen [sürüm notlarını](azure-stack-app-service-release-notes-update-seven.md)okuduğunuzdan emin olun. işlevselliği, düzeltmeleri ve dağıtımınızı etkileyebilecek bilinen sorunları.

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyicisini çalıştırma

Azure Stack ortamında App Service kaynak sağlayıcısını yükseltmek için, şu görevleri gerçekleştirmeniz gerekir:

1. [App Service yükleyicisini](https://aka.ms/appsvcupdate7installer)indirin.
2. Çevrimdışı yükseltme paketi oluşturun.
3. App Service yükleyicisini (appservice. exe) çalıştırın ve yükseltmeyi doldurun.

Bu işlem sırasında yükseltme şu şekilde olur:

* App Service önceki dağıtımını Algıla
* Depolama alanına yükle
* Tüm App Service rollerini (denetleyiciler, yönetim, ön uç, yayımcı ve çalışan rolleri) yükseltin
* App Service ölçek kümesi tanımlarını güncelleştirme
* App Service kaynak sağlayıcısı bildirimini Güncelleştir

## <a name="create-an-offline-upgrade-package"></a>Çevrimdışı yükseltme paketi oluşturma

Bağlantısı kesilen bir ortamda App Service yükseltmek için, önce Internet 'e bağlı bir makinede bir çevrimdışı yükseltme paketi oluşturmanız gerekir.

1. Appservice. exe ' yi yönetici olarak çalıştırma

    ![App Service yükleyicisi][1]

2. **Gelişmiş** > **Çevrimdışı paket oluştur** ' a tıklayın

    ![App Service yükleyicisi gelişmiş][2]

3. App Service yükleyicisi, çevrimdışı bir yükseltme paketi oluşturur ve bu paketin yolunu görüntüler.  Dosya Gezgini 'nde klasörü açmak için **klasörü aç** ' a tıklayabilirsiniz.

4. Yükleyiciyi (AppService. exe) ve çevrimdışı yükseltme paketini Azure Stack konak makinenize kopyalayın.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Azure Stack App Service yükseltmesini doldurun

> [!IMPORTANT]
> App Service yükleyici, Azure Stack yönetici Azure Resource Manager uç noktasına ulaşabilirler bir makinede çalıştırılmalıdır.
>
>

1. Appservice. exe ' yi yönetici olarak çalıştırın.

    ![App Service yükleyicisi][1]

2. **Gelişmiş** > **çevrimdışı yükleme veya yükseltme**' ye tıklayın.

    ![App Service yükleyicisi gelişmiş][2]

3. Daha önce oluşturduğunuz çevrimdışı yükseltme paketinin konumuna gidin ve ardından **İleri**' ye tıklayın.

4. Microsoft yazılımı lisans koşulları 'nı gözden geçirip kabul edin ve **İleri**' ye tıklayın.

5. Üçüncü taraf lisans koşullarını gözden geçirip kabul edin ve **İleri**' ye tıklayın.

6. Azure Stack Azure Resource Manager uç noktası ve Active Directory kiracı bilgilerinin doğru olduğundan emin olun. Azure Stack Geliştirme Seti dağıtımı sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri burada kabul edebilirsiniz. Ancak, Azure Stack dağıtılan seçenekleri özelleştirdiyseniz, Bu penceredeki değerleri düzenlemeniz gerekir. Örneğin, *MyCloud.com*etki alanı sonekini kullanırsanız, Azure Stack Azure Resource Manager uç noktasının *Management.Region.MyCloud.com*olarak değiştirilmesi gerekir. Bilgilerinizi doğruladıktan sonra **İleri**' ye tıklayın.

    ![Azure Stack bulut bilgileri][3]

7. Sonraki sayfada:

   1. **Azure Stack abonelikler** kutusunun yanındaki **Bağlan** düğmesine tıklayın.
      * Azure Active Directory (Azure AD) kullanıyorsanız, Azure Stack dağıtırken verdiğiniz Azure AD yönetici hesabını ve parolasını girin. **Oturum aç**' a tıklayın.
      * Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanıyorsanız, yönetici hesabınızı girin. Örneğin, _cloudadmin@azurestack.local_ . Parolanızı girin ve **oturum aç**' a tıklayın.
   2. **Azure Stack abonelikler** kutusunda **varsayılan sağlayıcı aboneliğini**seçin.
   3. **Azure Stack konumları** kutusunda, dağıtmakta olduğunuz bölgeye karşılık gelen konumu seçin. Örneğin, Azure Stack Geliştirme Seti dağıtdıysanız **Yerel** ' i seçin.
   4. Mevcut bir App Service dağıtımı algılanırsa, kaynak grubu ve depolama hesabı doldurulur ve gri kalır.
   5. Yükseltme özetini gözden geçirmek için **İleri** ' ye tıklayın.

      ![App Service yükleme algılandı][4]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için önceki düğmeleri kullanarak önceki sayfaları ziyaret edin.
   2. Yapılandırma doğruysa onay kutusunu seçin.
   3. Yükseltmeyi başlatmak için **İleri**' ye tıklayın.

       ![App Service yükseltme Özeti][5]

9. Yükseltme ilerleme durumu sayfası:
    1. Yükseltme ilerlemesini izleyin. Azure Stack App Service yükseltmenin süresi, dağıtılan rol örneklerinin sayısına bağlı olarak değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra **Çıkış**' a tıklayın.

        ![App Service yükseltme Ilerlemesi][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack App Service için ek yönetici işlemlerine hazırlanma

* [Ek kapasite planlaması](azure-stack-app-service-capacity-planning.md)
* [Ek kapasite Ekle](azure-stack-app-service-add-worker-roles.md)
