---
title: Azure Stack Azure App Service güncelleştirme | Microsoft Docs
description: Azure Stack Azure App Service güncelleştirmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 05/28/2019
ms.openlocfilehash: 768a6270021d6a87be3d2d28508288836ffadd29
ms.sourcegitcommit: 7d7a4c8c46613b6104caf23763bfd2275f6a826b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70808248"
---
# <a name="update-azure-app-service-on-azure-stack"></a>Azure Stack Azure App Service güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

> [!IMPORTANT]
> 1904 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service 1,7 ' i dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

Bu makalede, internet 'e bağlı bir Azure Stack ortamında dağıtılan [App Service kaynak sağlayıcısını](azure-stack-app-service-overview.md) nasıl yükselteceğiniz gösterilmektedir.

> [!IMPORTANT]
> Yükseltmeyi çalıştırmadan önce [Azure Stack Azure App Service dağıtımını](azure-stack-app-service-deploy.md)zaten tamamladığınızdan emin olun. Ayrıca, yeni işlevleri, düzeltmeleri ve dağıtımınızı etkileyebilecek bilinen sorunları öğrenmek için 1,7 sürümüne eşlik eden [sürüm notlarını](azure-stack-app-service-release-notes-update-seven.md) da okumalısınız.

## <a name="run-the-app-service-resource-provider-installer"></a>App Service kaynak sağlayıcısı yükleyicisini çalıştırma

Bu işlem sırasında yükseltme şu şekilde olur:

* App Service önceki dağıtımını algıla.
* Tüm güncelleştirme paketlerini ve dağıtılacak tüm OSS kitaplıklarının yeni sürümlerini hazırlayın.
* Depolama alanına yükleyin.
* Tüm App Service rollerini (denetleyiciler, yönetim, ön uç, yayımcı ve çalışan rolleri) yükseltin.
* App Service ölçek kümesi tanımlarını güncelleştirin.
* App Service kaynak sağlayıcısı bildirimini güncelleştirin.

> [!IMPORTANT]
> App Service yükleyicisi, Azure Stack yönetici Azure Resource Manager uç noktasına ulaşabilirler bir makinede çalıştırılmalıdır.

Azure Stack App Service dağıtımınızı yükseltmek için aşağıdaki adımları izleyin:

1. [App Service yükleyicisini](https://aka.ms/appsvcupdate7installer)indirin.

2. Appservice. exe ' yi yönetici olarak çalıştırın.

    ![App Service yükleyicisi][1]

3. App Service Dağıt ' a tıklayın **veya en son sürüme yükseltin.**

4. Microsoft yazılımı lisans koşulları 'nı gözden geçirip kabul edin ve **İleri**' ye tıklayın.

5. Üçüncü taraf lisans koşullarını gözden geçirip kabul edin ve **İleri**' ye tıklayın.

6. Azure Stack Azure Resource Manager uç noktası ve Active Directory kiracı bilgilerinin doğru olduğundan emin olun. ASDK dağıtımı sırasında varsayılan ayarları kullandıysanız, varsayılan değerleri burada kabul edebilirsiniz. Ancak, Azure Stack dağıtılan seçenekleri özelleştirdiyseniz, Bu penceredeki değerleri düzenlemeniz gerekir. Örneğin, *MyCloud.com*etki alanı sonekini kullanırsanız, Azure Stack Azure Resource Manager uç noktasının *Management.Region.MyCloud.com*olarak değiştirilmesi gerekir. Bilgilerinizi doğruladıktan sonra **İleri**' ye tıklayın.

    ![Azure Stack bulut bilgileri][2]

7. Sonraki sayfada:

   1. **Azure Stack abonelikler** kutusunun yanındaki **Bağlan** düğmesine tıklayın.
        * Azure Active Directory (Azure AD) kullanıyorsanız, Azure Stack dağıtırken verdiğiniz Azure AD yönetici hesabını ve parolasını girin. **Oturum aç**' a tıklayın.
        * Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanıyorsanız, yönetici hesabınızı girin. Örneğin, *cloudadmin\@azurestack. Local*. Parolanızı girin ve **oturum aç**' a tıklayın.
   2. **Azure Stack abonelikler** kutusunda **varsayılan sağlayıcı aboneliğini**seçin.
   3. **Azure Stack konumları** kutusunda, dağıtmakta olduğunuz bölgeye karşılık gelen konumu seçin. Örneğin, ASDK 'ye dağıtım yapıyorsanız **Yerel** ' i seçin.
   4. Mevcut bir App Service dağıtımı algılanırsa, kaynak grubu ve depolama hesabı doldurulur ve kullanılamaz.
   5. Yükseltme özetini gözden geçirmek için **İleri** ' ye tıklayın.

      ![App Service yükleme algılandı][3]

8. Özet sayfasında:
   1. Yaptığınız seçimleri doğrulayın. Değişiklik yapmak için **önceki düğmeleri kullanarak önceki sayfaları** ziyaret edin.
   2. Yapılandırma doğruysa onay kutusunu seçin.
   3. Yükseltmeyi başlatmak için **İleri**' ye tıklayın.

       ![App Service yükseltme Özeti][4]

9. Yükseltme ilerleme durumu sayfası:
    1. Yükseltme ilerlemesini izleyin. Azure Stack App Service yükseltmenin süresi, dağıtılan rol örneklerinin sayısına bağlı olarak değişir.
    2. Yükseltme başarıyla tamamlandıktan sonra **Çıkış**' a tıklayın.

        ![App Service yükseltme Ilerlemesi][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack App Service için ek yönetici işlemlerine hazırlanın:

* [Ek kapasite planlaması](azure-stack-app-service-capacity-planning.md)
* [Ek kapasite Ekle](azure-stack-app-service-add-worker-roles.md)
