---
title: Azure Stack 'tan şirket Kullanıcı verilerini isteğe bağlı olarak temizleyin. | Microsoft Docs
description: Azure Stack operatörü olarak, Azure Stack kullanıcılar tarafından istendiğinde Portal Kullanıcı verilerini nasıl temizleyeceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.date: 09/10/2019
ms.author: sethm
ms.reviewer: troettinger
ms.lastreviewed: 09/10/2019
monikerRange: azs-1802
ms.openlocfilehash: 2dd88656491a474e4082ff4e8321af836776b1f0
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019124"
---
# <a name="clear-portal-user-data-from-azure-stack"></a>Azure Stack Portal Kullanıcı verilerini temizle

Azure Stack işleçler, Azure Stack kullanıcılar tarafından talep edildiğinde Portal Kullanıcı verilerini isteğe bağlı olarak temizleyebilir. Azure Stack Kullanıcı olarak, Portal kutucukları sabitleyerek ve Pano düzeni değiştirilerek özelleştirilebilir. Kullanıcılar ayrıca temayı değiştirebilir ve kişisel tercihleriyle eşleşecek şekilde varsayılan dili ayarlayabilir. 

Portal Kullanıcı verileri, Azure Stack Kullanıcı portalında sık kullanılanları ve en son erişilen kaynakları içerir. Bu makalede, Portal Kullanıcı verilerinin nasıl temizleneceğini açıklanmaktadır.

Portal Kullanıcı ayarlarının kaldırılması yalnızca kullanıcı aboneliği silindikten sonra yapılmalıdır.

> [!NOTE]
> Bu makaledeki kılavuzdan sonra bazı Kullanıcı verileri olay günlüklerinin sistem bölümünde bulunabilir. Günlükler otomatik olarak geri alınana kadar bu veriler birkaç gün boyunca kalabilir.

## <a name="requirements"></a>Gereksinimler

- [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).
- GitHub 'dan [en son Azure Stack araçları indirin](azure-stack-powershell-download.md) .
- Kullanıcı hesabının dizinde hala mevcut olması gerekir.
- Yönetici Kaynak Yöneticisi uç noktasına erişmek için yönetici kimlik bilgileri Azure Stack.

> [!NOTE]
> Bir konuk dizininden (çok kiracılı) davet edilen bir kullanıcıdan Portal Kullanıcı bilgilerini silmeye çalışırsanız, bu dizinde okuma izninizin olması gerekir. Daha fazla bilgi için [Bu makalenin devamındaki CSP senaryosuna](#clear-portal-user-data-in-guest-directory)bakın.

## <a name="clear-portal-user-data-using-a-user-principal-name"></a>Kullanıcı asıl adını kullanarak Portal Kullanıcı verilerini temizle

Bu senaryo, varsayılan sağlayıcı aboneliğinin ve kullanıcının aynı dizinin bir parçası olduğunu ya da kullanıcının bulunduğu dizine okuma erişiminizin olduğunu varsayar.

Devam etmeden önce GitHub 'dan [Azure Stack araçlarının en son sürümünü indirdiğinizden](azure-stack-powershell-download.md) emin olun.

Bu yordam için Azure Stack Yönetim Kaynak Yöneticisi uç noktasıyla iletişim kurabilen bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır), **Azurestack-Tools-Master** dizinindeki kök klasörüne gidin ve gerekli PowerShell modülünü içeri aktarın:

   ```powershell
   Import-Module .\DatacenterIntegration\Portal\PortalUserDataUtilities.psm1
   ```

2. Aşağıdaki komutları çalıştırın. Yer tutucuları ortamınızdan eşleşen değerlerle değiştirdiğinizden emin olun.

   ```powershell
   ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.

   $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

   ## Replace the following value with the Azure Stack directory tenant ID.
   $azureStackDirectoryTenantId = "f5025bf2-547f-4b49-9693-6420c1d5e4ca"

   ## Replace the following value with the user directory tenant ID.
   $userDirectoryTenantId = " 7ddf3648-9671-47fd-b63d-eecd82ed040e"

   ## Replace the following value with name of the user principal whose portal user data is to be cleared.
   $userPrincipalName = "myaccount@contoso.onmicrosoft.com"

   Clear-AzsUserDataWithUserPrincipalName -AzsAdminArmEndpoint $adminARMEndpoint `
    -AzsAdminDirectoryTenantId $azureStackDirectoryTenantId `
    -UserPrincipalName $userPrincipalName `
    -DirectoryTenantId $userDirectoryTenantId
   ```

   > [!NOTE]
   > `azureStackDirectoryTenantId` isteğe bağlıdır. Bu değeri belirtmezseniz, komut dosyası Azure Stack kayıtlı tüm kiracı dizinlerindeki Kullanıcı asıl adını arar ve ardından tüm eşleşen kullanıcılar için Portal verilerini temizler.

## <a name="clear-portal-user-data-in-guest-directory"></a>Konuk dizinindeki Portal Kullanıcı verilerini temizle

Bu senaryoda Azure Stack işlecinin, kullanıcının bulunduğu Konuk dizine erişimi yoktur. Bu, bir bulut çözümü sağlayıcısı (CSP) olduğunuzda yaygın bir senaryodur.

Portal Kullanıcı verilerini kaldırmak için bir Azure Stack işleci için, en azından Kullanıcı nesne KIMLIĞI gereklidir.

Kullanıcının nesne KIMLIĞINI sorgulaması ve Azure Stack işlecine sağlaması gerekir. İşlecin, kullanıcının bulunduğu dizine erişimi yok.

### <a name="user-retrieves-the-user-object-id"></a>Kullanıcı Kullanıcı nesne KIMLIĞINI alır

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır), **Azurestack-Tools-Master** dizinindeki kök klasöre gidin ve gerekli PowerShell modülünü içeri aktarın.

   ```powershell
   Import-Module .\DatacenterIntegration\Portal\PortalUserDataUtilities.psm1
   ```

2. Aşağıdaki komutları çalıştırın. Yer tutucuları ortamınızdan eşleşen değerlerle değiştirdiğinizden emin olun.

   ```powershell
   ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
   $userARMEndpoint = "https://management.local.azurestack.external"

   ## Replace the following value with the directory tenant ID, which contains the user account.
   $userDirectoryTenantId = "3160cbf5-c227-49dd-8654-86e924c0b72f"

   ## Replace the following value with the name of the user principal whose portal user data is to be cleared.
   $userPrincipleName = "myaccount@contoso.onmicrosoft.com"

   Get-UserObjectId -DirectoryTenantId $userDirectoryTenantId `
    -AzsArmEndpoint $userARMEndpoint `
    -UserPricinpalName $userPrincipleName
   ```

   > [!NOTE]
   > Bir kullanıcı olarak, önceki betiğin çıkışı olan kullanıcı nesne KIMLIĞINI Azure Stack işlecine sağlamanız gerekir.

## <a name="azure-stack-operator-removes-the-portal-user-data"></a>Azure Stack işleci Portal Kullanıcı verilerini kaldırır

Kullanıcı nesne KIMLIĞINI bir Azure Stack operatörü olarak aldıktan sonra, Portal Kullanıcı verilerini kaldırmak için aşağıdaki komutları çalıştırın:

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır), **Azurestack-Tools-Master** dizinindeki kök klasöre gidin ve gerekli PowerShell modülünü içeri aktarın.

   ```powershell
   Import-Module .\DatacenterIntegration\Portal\PortalUserDataUtilities.psm1
   ```

2. Aşağıdaki komutları çalıştırarak, parametresini ortamınıza uyacak şekilde ayarladığınızdan emin olun:

   ```powershell
   ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
   $AzsAdminARMEndpoint = "https://adminmanagement.local.azurestack.external"

   ## Replace the following value with the Azure Stack directory tenant ID.
   $AzsAdminDirectoryTenantId = "f5025bf2-547f-4b49-9693-6420c1d5e4ca"
   
   ## Replace the following value with the directory tenant ID of the user to clear.
   $DirectoryTenantId = "3160cbf5-c227-49dd-8654-86e924c0b72f"

   ## Replace the following value with the name of the user principal whose portal user data is to be cleared.
   $userObjectID = "s-1-*******"
   Clear-AzsUserDataWithUserObject -AzsAdminArmEndpoint $AzsAdminARMEndpoint `
    -AzsAdminDirectoryTenantId $AzsAdminDirectoryTenantId `
    -DirectoryTenantID $DirectoryTenantId `
    -UserObjectID $userObjectID `
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Azure Ile kaydedin](azure-stack-registration.md) ve kullanıcılarınıza sunmak için [Azure Stack marketini](azure-stack-marketplace.md) öğelerle doldurun.
