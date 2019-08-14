---
title: Visual Studio 'Yu yükleyip Azure Stack bağlanın | Microsoft Docs
description: Visual Studio 'Yu yüklemeyi ve Azure Stack bağlanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 19d532cd85244f69d1a969e92ca302822abf0133
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991673"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio 'Yu yükleyip Azure Stack bağlanın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager [şablonları](azure-stack-arm-templates.md) yazmak ve dağıtmak üzere Visual Studio 'yu kullanabilirsiniz. Bu makaledeki adımlarda, [VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)aracılığıyla Azure Stack kullanmayı planlıyorsanız, Visual Studio 'yu [Azure Stack](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) veya harici bir bilgisayara nasıl yükleyeceğiniz açıklanır.

## <a name="install-visual-studio"></a>Visual Studio'yu yükleme

1. [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx)indirip çalıştırın.  

2. **Microsoft Web Platformu Yükleyicisi**açın.

3. **MICROSOFT Azure SDK-2.9.6 Ile Visual Studio Community 2015**aratın. **Ekle**' ye ve ardından' a tıklayın.

4. Azure SDK 'nın bir parçası olarak yüklenen **Microsoft Azure PowerShell** kaldırın.

    ![WebPI install adımlarının ekran görüntüsü](./media/azure-stack-install-visual-studio/image1.png)

5. [Azure Stack Için PowerShell 'ı yükler](../operator/azure-stack-powershell-install.md).

6. Yükleme tamamlandıktan sonra işletim sistemini yeniden başlatın.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Azure AD ile Azure Stack bağlanma

1. Visual Studio'yu başlatın.

2. **Görünüm** menüsünde, **bulut Gezgini**' ni seçin.

3. Yeni bölmede **Hesap Ekle** ' yi seçin ve Azure Active Directory (Azure AD) kimlik bilgilerinizle oturum açın.  

    ![Oturum açıldıktan ve Azure Stack bağlandığında Cloud Explorer 'ın ekran görüntüsü](./media/azure-stack-install-visual-studio/image2.png)

Oturum açtıktan sonra, kendi şablonlarınızı oluşturmak için [şablonları dağıtabilir](azure-stack-deploy-template-visual-studio.md) veya kullanılabilir kaynak türlerine ve kaynak gruplarına gidebilirsiniz.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>AD FS Azure Stack bağlanma

1. Visual Studio'yu başlatın.

2. **Araçlar**' da **Seçenekler**' i seçin.

3. **Gezinti bölmesinde** **ortamı** genişletin ve **hesaplar**' ı seçin.

4. **Ekle**' yi seçin ve Kullanıcı Azure Resource Manager uç noktasını girin. Azure Stack Geliştirme Seti (ASDK) için URL: `https://management.local.azurestack/external`.  Azure Stack tümleşik sistemler için URL: `https://management.[Region}.[External FQDN]`.

    ![Yeni Azure Cloud Discovery uç noktası ekle](./media/azure-stack-install-visual-studio/image5.png)

5. **Add (Ekle)** seçeneğini belirleyin.  

    Visual Studio, Azure Resource Manager 'ı çağırır ve Azure Directory Federasyon Hizmetleri (AD FS) için kimlik doğrulama uç noktası da dahil olmak üzere uç noktaları bulur.

    ![Oturum açıldıktan ve Azure Stack bağlandığında Cloud Explorer 'ın ekran görüntüsü](./media/azure-stack-install-visual-studio/image6.png)

6. **Görünüm** menüsünden **bulut Gezgini** ' ni seçin.

7. **Hesap Ekle** ' yi seçin ve AD FS kimlik bilgilerinizle oturum açın.  

    ![Cloud Explorer 'da Visual Studio 'da oturum açın](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer, kullanılabilir abonelikleri sorgular. Yönetmek için kullanılabilir bir abonelik seçebilirsiniz.

    ![Cloud Explorer 'da yönetilecek abonelikleri seçin](./media/azure-stack-install-visual-studio/image8.png)

8. Var olan kaynaklarınıza, kaynak gruplarına veya şablonları dağıtmaya göre inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Visual Studio sürümleriyle yan [](/visualstudio/install/install-visual-studio-versions-side-by-side) yana Visual Studio hakkında daha fazla bilgi edinin.
- [Azure Stack için şablonlar geliştirin](azure-stack-develop-templates.md).
