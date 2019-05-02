---
title: Dağıtım bir C# ASP .net web uygulaması bir sanal makineye Azure Stack'te | Microsoft Docs
description: Dağıtım bir C# Azure Stack'te bir VM için ASP.net web uygulaması.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 3f925d7c6a7f08257dbcb054044403e1488d38cb
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482132"
---
# <a name="how-to-deploy-a-c-aspnet-web-app-to-a-vm-in-azure-stack"></a>Nasıl dağıtılacağı bir C# Azure Stack'te bir VM için ASP.net web uygulaması

Konak için bir VM oluşturabilirsiniz, C# Azure Stack'te (ASP.NET) Web uygulaması. Bu makalede, izleyecek server'ı ayarlarken barındırmak için sunucu yapılandırma adımları inceler, C# (ASP.NET) web uygulamasını ve ardından uygulamanızı doğrudan Visual Studio'dan dağıtma.

C#bir genel amaçlı, çoklu paradigma programlama dili kapsamlı güçlü yazım, yazım, sözcüksel olarak kapsamlı, kesinlik temelli, bildirim temelli, işlev, genel, nesne yönelimli ve bileşen odaklı programlama disiplinlerin içindir. Bilgi edinmek için C# programlama dilini ve ek kaynaklar için bulma C#, bkz: [ C# Kılavuzu](https://docs.microsoft.com/dotnet/csharp/).

Bu makalede kullanacağı bir C# 6.0 bir Windows 2016 sunucusu üzerinde çalışan ASP.NET Core 2.2 kullanan uygulama.

## <a name="create-a-vm"></a>VM oluşturma

1. Oluşturma bir [Windows Server VM](azure-stack-quick-windows-portal.md).

2. Sanal makinenizde bileşenleri yüklemek için aşağıdaki betiği çalıştırın. Komut dosyası:
      - IIS (Yönetim konsoluyla) yükleyin.
      - ASP.NET 4.6 yükleyin.

        ```PowerShell  
        # Install IIS (with Management Console)
        Install-WindowsFeature -name Web-Server -IncludeManagementTools
        
        # Install ASP.NET 4.6
        Install-WindowsFeature Web-Asp-Net45
        
        # Install Web Management Service
        Install-WindowsFeature -Name Web-Mgmt-Service
        ```

3. İndirme [3.6 Web dağıtımı için MSI](https://www.microsoft.com/download/details.aspx?id=43717). Konumundan MSI yükleyin ve ardından tüm özelliklerini etkinleştirin.

4. .NET Core barındırma paket 2.2 için sunucunuza yükleyin. Adımlar için bkz: [.NET Core yükleyici](https://dotnet.microsoft.com/download/dotnet-core/2.2). Geliştirme makinenizde hem de hedef sunucunuz üzerinde .NET Core sürümüyle aynı sürümü kullandığınızdan emin olun.

5. Azure Stack Portalı'na dönün ve bağlantı noktaları, sanal Makinenizin ağ ayarlarını açın.

    1. Kiracınız için Azure Stack portalını açın.
    2. Sanal makinenizin bulun. VM, panoya sabitlenmiş veya içinde VM için arama yapabilirsiniz **kaynak Ara** kutusu.
    3. Seçin **ağ**.
    4. Seçin **gelen bağlantı noktası kuralı Ekle** VM altında.
    1. Aşağıdaki bağlantı noktaları için bir gelen güvenlik kuralı ekleyin:

    | Bağlantı noktası | Protokol | Açıklama |
    | --- | --- | --- |
    | 80 | HTTP | Köprü Metni Aktarım Protokolü (HTTP), dağıtılmış, işbirliğine dayalı, Hiper medyayı bilgi sistemlerine yönelik bir uygulama protokolüdür. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal Makinenizin bağlanır. |
    | 443 | HTTPS | Köprü Metni Aktarım Protokolü güvenli (HTTPS), Köprü Metni Aktarım Protokolü (HTTP) bir uzantısıdır. Bir bilgisayar ağ üzerinden güvenli iletişim için kullanılır. İstemciler, web uygulamanıza ya da genel IP veya DNS adı ile sanal makinenizin bağlanır. |
    | 22 | SSH | Güvenli Kabuk (SSH) ağ hizmetleri güvenli bir şekilde güvenli olmayan bir ağ üzerinden işletim bir şifreli ağ protokolüdür. VM yapılandırma ve uygulamayı dağıtmak için bir SSH istemcisi ile bu bağlantıyı kullanır. |
    | 3389 | RDP | İsteğe bağlı. Uzak Masaüstü Protokolü bir grafik kullanıcı arabirimi kullanılacak Uzak Masaüstü bağlantısı için makinenizi sağlar.   |

    Her bağlantı noktası:

    1. Seçin **herhangi** kaynağı için.
    1. Tür `*` için kaynak bağlantı noktası aralığı.
    1. Seçin **herhangi** için **hedef**.
    1. Açmak istediğiniz bağlantı noktasını ekleyin **hedef bağlantı noktası aralığı**.
    1. Seçin **herhangi** için **Protokolü**.
    1. Seçin **izin** için **eylem**.
    1. İçin varsayılan değeri bırakın **öncelik**.
    1. **Adı** ve sağlayan bir **açıklama** bağlantı noktası neden açtığınız not yapabilmeleri.
    1. Ekle'yi seçin.

5.  İçinde **ağ** Azure Stack, VM için ayarları sunucunuz için bir DNS adı oluşturun. Kullanıcılar, URL'yi kullanarak web sitenize bağlanabilir.

    1. Kiracınız için Azure Stack portalını açın.
    1. Sanal makinenizin bulun. VM, panoya sabitlenmiş veya içinde VM için arama yapabilirsiniz **kaynak Ara** kutusu.
    1. **Genel Bakış**’ı seçin.
    1. Seçin **yapılandırma** VM altında.
    1. Seçin **dinamik** için **atama**.
    1. DNS ad etiketi aşağıdaki gibi yazın `mywebapp` tam URL'nizi böylece aşağıdakine benzer: `mywebapp.local.cloudapp.azurestack.external`.

## <a name="create-an-app"></a>Uygulama oluşturma 

Kendi Web uygulamanızla kullanabilir veya örneğe kullanın [Visual Studio ile Azure'a bir ASP.NET Core uygulaması yayımlama](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-2.2&tabs=visual-studio
).

Bu makalede, oluşturma ve Visual Studio 2017'de Microsoft Azure sanal makineleri Yayımlama özelliğini kullanarak bir Azure sanal makine (VM) için bir ASP.NET web uygulaması yayımlama açıklanır. Emin olun, uygulamanızı yerel olarak çalıştığı ve yükledikten sonra Azure Sack içinde Windows VM'ye yayımlama hedef güncelleştirir.

## <a name="deploy-and-run-the-app"></a>Uygulamayı dağıtma ve çalıştırma

Yayımlama hedefi vm'niz Azure Stack'te oluşturun.

1. Çözüm Gezgini'nde projeye sağ tıklayıp **Yayımla**.

    ![ASP.NET web uygulamasını Azure Stack'e dağıtma yayımlama](media/azure-stack-dev-start-howto-vm-dotnet/deploy-app-to-azure-stack.png)

2.  Yayımlama penceresinde, yeni bir profil seçin.
3. IIS, FTP, vb. seçin.
4. Select yayımlayın.

5.  Seçin **Web dağıtımı** için **yayımlama yöntemi**.
6.  İçin **sunucu** daha önce tanımladığınız DNS adını girin gibi `w21902.local.cloudapp.azurestack.external`
7.  İçin **Site adı** türü `Default Web Site`.
8.  İçin **kullanıcı adı** makine için kullanıcı adını yazın.
9.  İçin **parola**, makine için parolayı yazın.
10. İçin **hedef URL** sitenin URL'sini yazın `mywebapp.local.cloudapp.azurestack.external`.

    ![ASP.NET web uygulaması dağıtma - Web dağıtımı yapılandırma](media/azure-stack-dev-start-howto-vm-dotnet/configure-web-deploy.png)

9. Seçin **bağlantısını doğrulama** doğrulamak için web dağıtımı yapılandırma. ve ardından **sonraki**.
10. Ayarlama, **yapılandırma** olarak **yayın**.
11. Ayarlama **hedef Framework** olarak **netcoreapp2.2**.
12. Ayarlama **hedef çalışma zamanı** olarak **taşınabilir**.
13. **Kaydet**’i seçin.
14. **Yayımla**’yı seçin.
15. Yeni sunucunuza gidin. Çalışan web uygulamanızın görmeniz gerekir.

```HTTP  
    mywebapp.local.cloudapp.azurestack.external
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md)
- Hakkında bilgi edinin [Iaas olarak Azure Stack için ortak dağıtımları](azure-stack-dev-start-deploy-app.md).