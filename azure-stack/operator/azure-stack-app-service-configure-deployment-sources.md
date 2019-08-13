---
title: Azure Stack uygulama hizmetleri için dağıtım kaynaklarını yapılandırma | Microsoft Docs
description: Bir hizmet yöneticisinin uygulama hizmetleri için dağıtım kaynaklarını (git, GitHub, BitBucket, DropBox ve OneDrive) nasıl yapılandırabilirim Azure Stack
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
ms.date: 03/11/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 8512568c709770f736d6740d83578dee7391adff
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "66269253"
---
# <a name="configure-deployment-sources"></a>Dağıtım kaynaklarını yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

App Service Azure Stack birden çok kaynak denetimi sağlayıcısından isteğe bağlı dağıtımı destekler. Bu özellik, uygulama geliştiricilerinin doğrudan kaynak denetimi depolarından dağıtmasını sağlar. Kullanıcılar, depolarına bağlanmak üzere App Service yapılandırmak istiyorsam, bir bulut operatörü öncelikle Azure Stack ve kaynak denetimi sağlayıcısındaki App Service arasındaki tümleştirmeyi yapılandırmalıdır.  

Yerel git 'e ek olarak, aşağıdaki kaynak denetimi sağlayıcıları desteklenir:

* GitHub
* BitBucket
* OneDrive
* Kutusunun

## <a name="view-deployment-sources-in-app-service-administration"></a>App Service yönetiminde dağıtım kaynaklarını görüntüleme

1. Azure Stack yönetim portalında oturum açın (https://adminportal.local.azurestack.external) hizmet yöneticisi olarak).
2. **Tüm hizmetlere** gidin ve **App Service**seçin.
    ![App Service kaynak sağlayıcısı Yöneticisi][1]
3. **Kaynak denetimi yapılandırması**' na tıklayın. Tüm yapılandırılmış dağıtım kaynaklarının listesini görebilirsiniz.
    ![App Service kaynak sağlayıcısı Yöneticisi kaynak denetimi yapılandırması][2]

## <a name="configure-github"></a>GitHub 'ı yapılandırma

Bu görevi gerçekleştirmek için bir GitHub hesabınızın olması gerekir. Kişisel bir hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. GitHub 'da oturum açın, öğesine gidin https://www.github.com/settings/developers ve ardından **Yeni bir uygulama kaydet**' e tıklayın.
    ![GitHub-yeni bir uygulamayı kaydetme][3]
2. Bir **uygulama adı**girin; Örneğin, **Azure Stack App Service**.
3. **Giriş sayfası URL 'sini**girin. Giriş sayfası URL 'SI Azure Stack Portal adresi olmalıdır. Örneğin: https://portal.local.azurestack.external.
4. Bir **uygulama açıklaması**girin.
5. **Yetkilendirme geri çağırma URL 'sini**girin. Varsayılan bir Azure Stack dağıtımında URL biçiminde olur https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalıştırıyorsanız, yerel. azurestack. external için etki alanı adınızı yerine koyun.
6. Tıklayın **kaydetme uygulama**. Uygulama için **ISTEMCI kimliği** ve **istemci gizliliğini** listelemek üzere bir sayfa görüntülenir.
    ![GitHub-tamamlanmış uygulama kaydı][5]
7.  Yeni bir tarayıcı sekmesi veya penceresinde, Azure Stack yönetici portalında (https://adminportal.local.azurestack.external) hizmet yöneticisi olarak) oturum açın.
8.  **Kaynak sağlayıcıları**' na gidin ve **App Service kaynak sağlayıcısı Yöneticisi**' ni seçin.
9. **Kaynak denetimi yapılandırması**' na tıklayın.
10. **ISTEMCI kimliğini** ve **istemci gizli** anahtarını kopyalayıp GitHub için karşılık gelen giriş kutularına yapıştırın.
11. **Kaydet**’e tıklayın.

## <a name="configure-bitbucket"></a>BitBucket 'ı Yapılandır

Bu görevi gerçekleştirmek için bir BitBucket hesabınız olmalıdır. Kişisel bir hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. BitBucket 'ta oturum açın ve hesabınızda **tümleştirmelere** gidin.
    ![BitBucket panosu-tümleştirmeler][7]
2. Erişim yönetimi altında **OAuth** ' a tıklayın ve **Tüketici ekleyin**.
    ![BitBucket OAuth tüketicisi Ekle][8]
3. Tüketici için bir **ad** girin; Örneğin, **Azure Stack App Service**.
4. Uygulama için bir **Açıklama** girin.
5. **Geri çağırma URL 'sini**girin. Varsayılan Azure Stack dağıtımında, geri çağırma URL 'SI https://portal.local.azurestack.external/TokenAuthorize biçimindedir. Farklı bir etki alanı altında çalıştırıyorsanız, azurestack. Local için etki alanı adınızı yerine koyun. BitBucket tümleştirmesinin başarılı olması için, URL 'nin burada listelenen büyük küçük harfleri izlemesi gerekir.
6. **URL 'yi**girin. Bu URL Azure Stack Portal URL 'SI olmalıdır; Örneğin, https://portal.local.azurestack.external.
7. Gerekli **izinleri** seçin:
    - **Depolar**: *Okuma*
    - **Web kancaları**: *Okuma ve yazma*
8. **Kaydet**’e tıklayın. Artık bu yeni uygulamayı, **OAuth tüketicileri**altında anahtar ve **gizli** **anahtar** ile birlikte görürsünüz.
    ![BitBucket uygulama listesi][9]
9.  Yeni bir tarayıcı sekmesi veya penceresinde, Azure Stack yönetici portalında (https://adminportal.local.azurestack.external) hizmet yöneticisi olarak) oturum açın.
10.  **Kaynak sağlayıcıları** ' na gidin ve **App Service kaynak sağlayıcısı Yöneticisi**' ni seçin.
11. **Kaynak denetimi yapılandırması**' na tıklayın.
12. Anahtar **ISTEMCI kimliği** giriş kutusuna ve **gizli** **anahtarı** , Bitbucket için **istemci gizli anahtarı** giriş kutusuna kopyalayıp yapıştırın.
13. **Kaydet**’e tıklayın.

## <a name="configure-onedrive"></a>OneDrive 'ı yapılandırma

Bu görevi gerçekleştirmek için OneDrive hesabına bağlı bir Microsoft hesabınızın olması gerekir.  Kişisel bir hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

> [!NOTE]
> OneDrive Iş hesapları Şu anda desteklenmemektedir.

1. Microsoft hesabınızı kullanarak gidin veoturumaçın. https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm
2. **Uygulamalarım**altında **Uygulama Ekle**' ye tıklayın.
![OneDrive uygulamaları][10]
3. Yeni uygulama kaydı için bir **ad** girin: **Azure Stack App Service**girin ve ardından **uygulama oluştur** ' a tıklayın.
4. Sonraki ekranda, yeni uygulamanızın özellikleri listelenir. **Uygulama kimliğini** geçici bir konuma kaydedin.
![OneDrive uygulama özellikleri][11]
5. **Uygulama gizli**dizileri altında **Yeni parola oluştur**' a tıklayın. **Oluşturulan yeni parolayı**bir yere göz önünde alın. Bu, uygulamanızın gizli anahtarı ve **Tamam**' a tıkladıktan sonra alınabilir değildir.
6. **Platformlar**altında **Platform Ekle**' ye ve ardından **Web**' i seçin.
7. **Yeniden YÖNLENDIRME URI**'sini girin. Varsayılan bir Azure Stack dağıtımında, yeniden yönlendirme URI 'SI formundadır https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalıştırıyorsanız, azurestack. Local için etki alanı adınızı yerine koyun.
![OneDrive uygulaması-Web platformu ekleme][12]
8. **Temsilci izinleri** **Microsoft Graph** - ekleme
    - **Files.ReadWrite.AppFolder**
    - **Kullanıcısını. Okuyamaz**  
      ![OneDrive uygulaması-grafik Izinleri][13]
9. **Kaydet**’e tıklayın.
10.  Yeni bir tarayıcı sekmesi veya penceresinde, Azure Stack yönetici portalında (https://adminportal.local.azurestack.external) hizmet yöneticisi olarak) oturum açın.
11.  **Kaynak sağlayıcıları** ' na gidin ve **App Service kaynak sağlayıcısı Yöneticisi**' ni seçin.
12. **Kaynak denetimi yapılandırması**' na tıklayın.
13. **Uygulama kimliği** ' ni kopyalayın ve **istemci kimliği** giriş kutusuna ve **parolaya** OneDrive için **istemci gizli anahtar** kutusuna yapıştırın.
14. **Kaydet**’e tıklayın.

## <a name="configure-dropbox"></a>DropBox 'ı Yapılandır

> [!NOTE]
> Bu görevi gerçekleştirmek için bir DropBox hesabınız olmalıdır. Kişisel bir hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. Dropbox hesabı https://www.dropbox.com/developers/apps kimlik bilgilerinizi kullanarak gidin ve oturum açın.
2. **Uygulama oluştur**' a tıklayın.

    ![Dropbox uygulamaları][14]

3. **Dropbox API 'sini**seçin.
4. Erişim düzeyini **uygulama klasörü**olarak ayarlayın.
5. Uygulamanız için bir **ad** girin.
![Dropbox uygulama kaydı][15]
6. **Uygulama oluştur**' a tıklayın. Uygulama **anahtarı** ve **uygulama gizli**dizisi dahil olmak üzere uygulamanın ayarlarını listelerken size bir sayfa sunulur.
7. **Uygulama klasörü adının** **Azure Stack App Service**olarak ayarlandığından emin olun.
8. **OAuth 2 yeniden YÖNLENDIRME URI** 'sini ayarlayın ve ardından **Ekle**' ye tıklayın. Varsayılan bir Azure Stack dağıtımında, yeniden yönlendirme URI 'SI formundadır https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalıştırıyorsanız, etki alanınızı azurestack. Local olarak değiştirin.
![Dropbox uygulama yapılandırması][16]
9.  Yeni bir tarayıcı sekmesi veya penceresinde, Azure Stack yönetici portalında (https://adminportal.local.azurestack.external) hizmet yöneticisi olarak) oturum açın.
10.  **Kaynak sağlayıcıları** ' na gidin ve **App Service kaynak sağlayıcısı Yöneticisi**' ni seçin.
11. **Kaynak denetimi yapılandırması**' na tıklayın.
12. **Uygulama anahtarını** kopyalayıp Dropbox Için **istemci gizli** giriş kutusuna **Istemci kimliği** giriş kutusuna ve **uygulama gizli** anahtarına yapıştırın.
13. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılar artık [sürekli dağıtım](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment), [Yerel git dağıtımı](https://docs.microsoft.com/azure/app-service/deploy-local-git)ve [bulut klasörü eşitlemesi](https://docs.microsoft.com/azure/app-service/deploy-content-sync)gibi işlemler için dağıtım kaynaklarını kullanabilir.

<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png
