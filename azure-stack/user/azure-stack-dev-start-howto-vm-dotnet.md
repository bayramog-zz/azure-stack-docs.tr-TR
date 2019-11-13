---
title: Azure Stack bir C# ASP.NET Web uygulamasını bir sanal makineye dağıtma | Microsoft Docs
description: Azure Stack bir C# sanal makineye bir ASP.NET Web uygulaması dağıtın.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 2b2d1bccbe41e57b81492e0ba0b201fe03df2d7d
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955768"
---
# <a name="deploy-a-c-aspnet-web-app-to-a-vm-in-azure-stack"></a>Azure Stack bir C# sanal makineye bir ASP.NET Web uygulaması dağıtma

Azure Stack 'de C# ASP.NET Web uygulamanızı barındırmak için bir sanal makıne (VM) oluşturabilirsiniz. Bu makalede, sunucunuzu ayarlarken izlenecek yönergeler ele alınmaktadır, C# ASP.NET Web uygulamanızı barındıracak şekilde yapılandırılır ve ardından uygulamayı doğrudan Visual Studio 'dan dağıtırsınız.

Bu makalede, bir C# Windows 2016 sunucusunda çalışan ASP.NET Core 2,2 kullanan 6,0 uygulaması kullanılmaktadır.

## <a name="create-a-vm"></a>VM oluşturma

1. Bir [Windows Server VM](azure-stack-quick-windows-portal.md)'si oluşturun.

1. IIS 'yi (Yönetim Konsolu ile) ve ASP.NET 4,6 bileşenlerini sanal makinenizde yüklemek için aşağıdaki betiği çalıştırın:

    ```PowerShell  
    # Install IIS (with Management Console)
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Install ASP.NET 4.6
    Install-WindowsFeature Web-Asp-Net45
    
    # Install Web Management Service
    Install-WindowsFeature -Name Web-Mgmt-Service
    ```

1. [Web dağıtımı v 3.6](https://www.microsoft.com/download/details.aspx?id=43717)indirin. MSI dosyasından yükleyip tüm özellikleri etkinleştirin.

1. Sunucunuza .NET Core 2,2 barındırma paketi 'ni yükler. Yönergeler için bkz. [.NET Core yükleyicisi](https://dotnet.microsoft.com/download/dotnet-core/2.2). Hem geliştirme makinenizde hem de hedef sunucunuzda .NET Core 'un aynı sürümünü kullandığınızdan emin olun.

1. Azure Stack portalında, sanal makinenizin ağ ayarlarında listelenen bağlantı noktalarını açın.

    a. Kiracınız için Azure Stack portalını açın.

    b. VM 'niz için arama yapın. Sanal makineyi panonuza sabitlemiş olabilirsiniz veya **arama kaynakları** kutusunda arama yapabilirsiniz.

    c. **Ağ**' ı seçin.

    d. VM altında **gelen bağlantı noktası kuralı ekle** ' yi seçin.

    e. Aşağıdaki bağlantı noktaları için bir gelen güvenlik kuralı ekleyin:

    | Bağlantı noktası | Protokol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), sunuculardan Web sayfaları teslim etmek için kullanılan protokoldür. İstemciler bir DNS adı veya IP adresi ile HTTP aracılığıyla bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), bir güvenlik sertifikası gerektiren ve şifreli bilgi iletimi sağlayan güvenli bir HTTP sürümüdür.  |
    | 22 | SSH | Secure Shell (SSH), güvenli iletişimler için şifreli bir ağ protokolüdür. Bu bağlantıyı, sanal makineyi yapılandırmak ve uygulamayı dağıtmak için bir SSH istemcisiyle kullanacaksınız. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü, bir uzak masaüstü bağlantısının makinenize bir grafik kullanıcı arabirimi kullanmasına izin verir.   |
    | 8172 | Özel | WebDeploy tarafından kullanılan bağlantı noktası. |

    Her bağlantı noktası için:

    a. **Kaynak**Için **herhangi bir**seçin.

    b. **Kaynak bağlantı noktası aralığı**için bir yıldız işareti ( **\*** ) yazın.

    c. **Hedef**Için **herhangi birini**seçin.

    d. **Hedef bağlantı noktası aralığı**için, açmak istediğiniz bağlantı noktasını ekleyin.

    e. **Protokol**Için **herhangi bir**seçin.

    f. **Eylem** alanında **İzin ver**'i seçin.

    g. **Öncelik**için varsayılan seçimi bırakın.

    h. Bağlantı noktasının neden açık olduğunu anımsamanıza yardımcı olması için bir **ad** ve **Açıklama** girin.

    i. **Add (Ekle)** seçeneğini belirleyin.

1.  SANAL makinenizin **ağ** ayarları Azure Stack ' de, sunucunuz IÇIN bir DNS adı oluşturun. Kullanıcılar, URL 'YI kullanarak Web sitenize bağlanabilir.

    a. Kiracınız için Azure Stack portalını açın.

    b. VM 'niz için arama yapın. Sanal makineyi panonuza sabitlemiş olabilirsiniz veya **arama kaynakları** kutusunda arama yapabilirsiniz.

    c. **Genel Bakış**’ı seçin.

    d. **VM**altında **Yapılandır**' ı seçin.

    e. **Atama**için **dinamik**' i seçin.

    f. Tam URL 'nizin *MyWebApp. Local. cloudapp. azurestack. external*haline gelmesi için **MYWEBAPP**gibi DNS ad etiketini girin.

## <a name="create-an-app"></a>Uygulama oluşturma 

[Visual Studio Ile Azure 'da kendi web uygulamanızı veya bir ASP.NET Core uygulaması yayımlama](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-2.2&tabs=visual-studio
)örneğini kullanabilirsiniz. Makalesinde, Visual Studio 2017 ' deki Azure sanal makineler yayımlama özelliğini kullanarak bir Azure sanal makinesine bir ASP.NET Web uygulaması oluşturma ve yayımlama açıklanmaktadır. ' Yi yükledikten ve uygulamanızın yerel olarak çalıştığından emin olduktan sonra, yayımlama hedefini Azure Stack örneğinizdeki Windows VM 'sine güncelleştirebilirsiniz.

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

Azure Stack ' de sanal makinenizde bir yayımlama hedefi oluşturun.

1. **Çözüm Gezgini**, projenize sağ tıklayın ve ardından **Yayımla**' yı seçin.

    ![Azure Stack yayımlamak için bir ASP.NET Web uygulaması dağıtma](media/azure-stack-dev-start-howto-vm-dotnet/deploy-app-to-azure-stack.png)

1. **Yayımla** penceresinde **Yeni profil**' i seçin.
1. **IIS, FTP, vb**. seçin.
1. **Yayımla**’yı seçin.
1. **Yayımla yöntemi**için **Web dağıtımı**' yi seçin.
1. **Sunucu** için, daha önce tanımladığınız DNS adını girin (örneğin, *w21902. Local. cloudapp. azurestack. external*).
1. **Site adı**Için **varsayılan Web sitesi**' ni girin.
1. **Kullanıcı adı**için makinenin Kullanıcı adını girin.
1. **Parola**için, makinenin parolasını girin.
1. **Hedef URL**'si için, sitenin URL 'sini girin (örneğin, *MyWebApp. Local. cloudapp. azurestack. external*).

    ![Bir ASP.NET Web uygulaması dağıtma-Web Dağıtımı yapılandırma](media/azure-stack-dev-start-howto-vm-dotnet/configure-web-deploy.png)

1. Web dağıtımı yapılandırmanızı doğrulamak için **bağlantıyı doğrula**' yı seçin ve ardından **İleri**' yi seçin.
1. **Yapılandırmayı** **yayın**olarak ayarlayın.
1. **Hedef çerçeveyi** **netcoreapp 2.2**olarak ayarlayın.
1. **Hedef çalışma zamanını** **Taşınabilir**olarak ayarlayın.
1. **Kaydet**’i seçin.
1. **Yayımla**’yı seçin.
1. Yeni sunucunuza gidin. Çalışan Web uygulamanızı görmeniz gerekir.

    ```http  
        mywebapp.local.cloudapp.azurestack.external
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack bir geliştirme ortamı ayarlamayı](azure-stack-dev-start.md)öğrenin.
- [Azure Stack için genel dağıtımlar IaaS olarak](azure-stack-dev-start-deploy-app.md)hakkında bilgi edinin.
- C# Programlama dilini öğrenmek ve için C#ek kaynaklar bulmak için [ C# kılavuzuna](https://docs.microsoft.com/dotnet/csharp/) bakın
