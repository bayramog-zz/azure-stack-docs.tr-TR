---
title: Dağıtım bir C# ASP.NET web uygulamasını bir sanal makineye Azure Stack'te | Microsoft Docs
description: Dağıtım bir C# Azure Stack'te bir VM için ASP.NET web uygulaması.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: b41c64d64a2c2abe6d1f145f11c2d4d84686b207
ms.sourcegitcommit: 2a4321a9cf7bef2955610230f7e057e0163de779
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65617692"
---
# <a name="deploy-a-c-aspnet-web-app-to-a-vm-in-azure-stack"></a>Dağıtım bir C# Azure Stack'te bir VM için ASP.NET web uygulaması

Konağa sanal makine (VM) oluşturabilirsiniz, C# Azure Stack'te ASP.NET web uygulaması. Bu makalede, sunucunuzu ayarlarken izlemeniz gereken yönergeleri ele alınmaktadır barındırmak için yapılandırmak, C# ASP.NET web uygulaması ve sonra uygulamayı doğrudan Visual Studio'dan dağıtma.

C#bir genel amaçlı, çoklu paradigma programlama dili kapsamlı güçlü yazım, yazım, sözcüksel olarak kapsamlı, kesinlik temelli, bildirim temelli, işlev, genel, nesne yönelimli ve bileşen odaklı programlama disiplinlerin içindir. Bilgi edinmek için C# programlama dilini ve ek kaynaklar için bulma C#, bkz: [ C# Kılavuzu](https://docs.microsoft.com/dotnet/csharp/).

Bu makalede bir C# 6.0 kullanan bir Windows 2016 sunucusu üzerinde çalışan ASP.NET Core 2.2 uygulaması.

## <a name="create-a-vm"></a>VM oluşturma

1. Oluşturma bir [Windows Server VM](azure-stack-quick-windows-portal.md).

1. Sanal Makinenize (Yönetim konsoluyla) IIS ve ASP.NET 4.6 bileşenleri yüklemek için aşağıdaki betiği çalıştırın:

    ```PowerShell  
    # Install IIS (with Management Console)
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Install ASP.NET 4.6
    Install-WindowsFeature Web-Asp-Net45
    
    # Install Web Management Service
    Install-WindowsFeature -Name Web-Mgmt-Service
    ```

1. İndirme [Web dağıtımı v3.6](https://www.microsoft.com/download/details.aspx?id=43717). MSI dosyasından yükleyin ve ardından tüm özelliklerini etkinleştirin.

1. .NET Core 2.2 barındırma paket sunucunuza yükleyin. Yönergeler için [.NET Core yükleyici](https://dotnet.microsoft.com/download/dotnet-core/2.2). Geliştirme makinenizde hem de hedef sunucunuz üzerinde .NET Core sürümüyle aynı sürümü kullandığınızdan emin olun.

1. Azure Stack portalında sanal Makineniz için Ağ Ayarları'nda listelenen bağlantı noktaları açın.

    a. Kiracınız için Azure Stack portalını açın.

    b. Sanal makinenizin arayın. VM, panoya sabitlenmiş veya içinde arama yapabilirsiniz **kaynak Ara** kutusu.

    c. Seçin **ağ**.

    d. Seçin **gelen bağlantı noktası kuralı Ekle** VM altında.

    e. Aşağıdaki bağlantı noktaları için bir gelen güvenlik kuralı ekleyin:

    | Bağlantı noktası | Protocol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), dağıtılmış, işbirliğine dayalı, Hiper medyayı bilgi sistemlerine yönelik bir uygulama protokolüdür. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal Makinenizin bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), Köprü Metni Aktarım Protokolü (HTTP) bir uzantısıdır. Bir bilgisayar ağ üzerinden güvenli iletişim için kullanılır. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal Makinenizin bağlanır. |
    | 22 | SSH | Güvenli Kabuk (SSH) ağ hizmetleri güvenli bir şekilde güvenli olmayan bir ağ üzerinden işletim bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanın. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü (RDP) kullanarak makinenizi bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı sağlar.   |

    Her bağlantı noktası:

    a. İçin **kaynak**seçin **herhangi**.

    b. İçin **kaynak bağlantı noktası aralığı**, bir yıldız işareti girin (**\***).

    c. İçin **hedef**seçin **herhangi**.

    d. İçin **hedef bağlantı noktası aralığı**, açmak istediğiniz bağlantı noktasını ekleyin.

    e. İçin **Protokolü**seçin **herhangi**.

    f. **Eylem** alanında **İzin ver**'i seçin.

    g. İçin **öncelik**, varsayılan seçimi bırakın.

    h. Girin bir **adı** ve **açıklama** neden bağlantı noktasının açık olduğundan anımsamanıza yardımcı olacak.

    i. **Add (Ekle)** seçeneğini belirleyin.

1.  İçinde **ağ** Azure Stack, VM için ayarları sunucunuz için bir DNS adı oluşturun. Kullanıcılar Web sitenize URL kullanarak bağlanabilir.

    a. Kiracınız için Azure Stack portalını açın.

    b. Sanal makinenizin arayın. VM, panoya sabitlenmiş veya içinde arama yapabilirsiniz **kaynak Ara** kutusu.

    c. **Genel Bakış**’ı seçin.

    d. Altında **VM**seçin **yapılandırma**.

    e. İçin **atama**seçin **dinamik**.

    f. DNS ad etiketi girin **mywebapp şeklindedir**, tam URL'nizi haline gelebilmesi *mywebapp.local.cloudapp.azurestack.external*.

## <a name="create-an-app"></a>Uygulama oluşturma 

Kendi web uygulamanızla ya da örneğe kullanabileceğiniz [Visual Studio ile Azure'a bir ASP.NET Core uygulaması yayımlama](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-2.2&tabs=visual-studio
). Bu makalede, oluşturma ve Visual Studio 2017'de Azure sanal makineler Yayımlama özelliğini kullanarak bir Azure sanal makinesi için bir ASP.NET web uygulaması yayımlama açıklanır. Yüklü ve uygulamanızı yerel olarak çalıştığından emin olduktan sonra Windows VM yayımlama hedef Azure Stack Örneğinizde güncelleştireceksiniz.

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

Yayımlama hedefi vm'niz Azure Stack'te oluşturun.

1. İçinde **Çözüm Gezgini**, projenize sağ tıklayın ve ardından **Yayımla**.

    ![Bir ASP.NET web uygulamasını Azure Stack'e dağıtma yayımlama](media/azure-stack-dev-start-howto-vm-dotnet/deploy-app-to-azure-stack.png)

1. İçinde **Yayımla** penceresinde **yeni profili**.
1. Seçin **IIS**, **FTP**ve benzeri.
1. **Yayımla**’yı seçin.
1. İçin **yayımlama yöntemi**seçin **Web dağıtımı**.
1. İçin **sunucu** daha önce tanımladığınız DNS adını girin gibi *w21902.local.cloudapp.azurestack.external*.
1. İçin **Site adı**, girin **varsayılan Web sitesi**.
1. İçin **kullanıcı adı**, makine kullanıcı adını girin.
1. İçin **parola**, makine için parolayı girin.
1. İçin **hedef URL**, site için URL girin, gibi *mywebapp.local.cloudapp.azurestack.external*.

    ![ASP.NET web uygulamasını dağıtma - Web dağıtımı yapılandırma](media/azure-stack-dev-start-howto-vm-dotnet/configure-web-deploy.png)

1. Doğrulamak için web dağıtımı yapılandırması, seçin **bağlantısını doğrulama**ve ardından **sonraki**.
1. Ayarlama **yapılandırma** olarak **yayın**.
1. Ayarlama **hedef Framework** olarak **netcoreapp2.2**.
1. Ayarlama **hedef çalışma zamanı** olarak **taşınabilir**.
1. **Kaydet**’i seçin.
1. **Yayımla**’yı seçin.
1. Yeni sunucunuza gidin. Çalışan web uygulamanızın görmeniz gerekir.

    ```HTTP  
        mywebapp.local.cloudapp.azurestack.external
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Azure Stack geliştirme ortamında ayarlama](azure-stack-dev-start.md).
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).
