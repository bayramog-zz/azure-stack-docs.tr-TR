---
title: Azure Stack 'de çok kiracılı
description: Azure Stack birden çok Azure Active Directory dizinini desteklemeyi öğrenin
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: patricka
ms.reviewer: bryanr
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: d8fbcba9a635d47927b1d6eb08336e0959704cfd
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68417188"
---
# <a name="multi-tenancy-in-azure-stack"></a>Azure Stack 'de çok kiracılı

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, birden çok Azure Active Directory (Azure AD) kiracısından Azure Stack hizmetleri kullanmak için kullanıcıları destekleyecek şekilde yapılandırabilirsiniz. Örneğin, aşağıdaki senaryoyu göz önünde bulundurun:

- Azure Stack yüklendiği yerde contoso.onmicrosoft.com hizmet yöneticisi olursunuz.
- Mary, konuk kullanıcıların bulunduğu fabrikam.onmicrosoft.com 'in Dizin yöneticisidir.
- Mary şirketinin şirketi, şirketinizdeki IaaS ve PaaS hizmetlerini alır ve konuk dizinindeki (fabrikam.onmicrosoft.com) kullanıcıların oturum açmasını ve contoso.onmicrosoft.com 'de Azure Stack kaynaklarını kullanmasına izin vermesi gerekir.

Bu kılavuz, Azure Stack içinde çok kiracılı olarak yapılandırmak için bu senaryonun bağlamında gerekli adımları sağlar. Bu senaryoda, siz ve Mary kullanıcıların, contoso 'daki Azure Stack dağıtımından oturum açmasını ve hizmetleri kullanmasını sağlamak için adımları tamamlaması gerekir.  

## <a name="enable-multi-tenancy"></a>Çok kiracılı modeli etkinleştirme

Azure Stack ' de çok kiracılı yapılandırmadan önce hesabına yönelik birkaç önkoşul vardır:
  
 - Siz ve Mary, her iki dizin Azure Stack de (contoso) ve konuk dizini (Fabrikam) üzerinde yönetim adımlarını koordine etmelidir.  
 - Azure Stack için PowerShell 'i [yükleyip](azure-stack-powershell-install.md) [yapılandırdığınızdan](azure-stack-powershell-configure-admin.md) emin olun.
 - [Azure Stack araçlarını indirin](azure-stack-powershell-download.md)ve Connect ve Identity modüllerini içeri aktarın:

    ```powershell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ```

### <a name="configure-azure-stack-directory"></a>Azure Stack dizinini yapılandırma

Bu bölümde, Fabrikam Azure AD dizin kiracılarındaki oturum açma işlemleri için Azure Stack yapılandırırsınız.

Konuk Dizin kiracısından kullanıcıları ve hizmet sorumlularını kabul etmek üzere Azure Resource Manager yapılandırarak Azure Stack Konuk Dizin kiracısını (Fabrikam) ekleyin.

Contoso.onmicrosoft.com Hizmet Yöneticisi aşağıdaki komutları çalıştırır.

```powershell  
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group. 
$location = "local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName
```

### <a name="configure-guest-directory"></a>Konuk dizinini yapılandırma

Azure Stack Yöneticisi/operatörü Azure Stack ile kullanılacak fabrikam dizinini etkinleştirdikten sonra, Mary fabrikam 'ın Dizin kiracısıyla Azure Stack kaydolmalıdır.

#### <a name="registering-azure-stack-with-the-guest-directory"></a>Konuk diziniyle Azure Stack kaydetme

Fabrikam Dizin Yöneticisi, fabrikam.onmicrosoft.com Konuk dizininde aşağıdaki komutları çalıştırır.

```powershell
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
```

> [!IMPORTANT]
> Azure Stack yöneticiniz gelecekte yeni hizmet veya güncelleştirme yüklerse, bu betiği yeniden çalıştırmanız gerekebilir.
>
> Dizininizde Azure Stack uygulamaların durumunu denetlemek için bu betiği dilediğiniz zaman yeniden çalıştırın.
>
> Yönetilen disklerde (1808 güncelleştirmesinde tanıtılan) VM oluşturmayla ilgili sorunlar olduğunu fark ediyorsanız, yeni bir **disk kaynak sağlayıcısı** eklenmiştir ve bu betiğin yeniden çalıştırılmasını gerektirir.

### <a name="direct-users-to-sign-in"></a>Kullanıcıların oturum açması için doğrudan

Artık ve Mary Mary 'nin dizinini ekleme adımlarını tamamladığınıza göre, Mary fabrikam kullanıcılarını oturum açmaya yönlendirebilir.  Fabrikam kullanıcıları (yani, fabrikam.onmicrosoft.com soneki olan kullanıcılar) https\://Portal.Local.azurestack.external. ziyaret ederek oturum açtığında  

Mary, Fabrikam dizinindeki tüm [yabancı sorumluları](/azure/role-based-access-control/rbac-and-directory-admin-roles) (diğer bir deyişle, Fabrikam dizinindeki kullanıcılar fabrikam.onmicrosoft.com) https\://Portal.Local.azurestack.external/kullanarak oturum açmak için yönlendirir. fabrikam.onmicrosoft.com.  Bu URL 'YI kullanmazsa, varsayılan dizinine (Fabrikam) gönderilir ve yöneticisinin yönetici tarafından ayrılmadığını belirten bir hata alırlar.

## <a name="disable-multi-tenancy"></a>Çoklu kirayı devre dışı bırak

Azure Stack birden çok kiracıyı artık istemiyorsanız, aşağıdaki adımları sırasıyla yaparak çoklu kiracıyı devre dışı bırakabilirsiniz:

1. Konuk dizinin Yöneticisi olarak (Bu senaryoda Mary), *Unregister-AzsWithMyDirectoryTenant*komutunu çalıştırın. Cmdlet 'i yeni dizindeki tüm Azure Stack uygulamalarını kaldırır.

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantName = "fabrikam.onmicrosoft.com"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. Azure Stack hizmet yöneticisi olarak (Bu senaryoda), *Unregister-AzSGuestDirectoryTenant*komutunu çalıştırın. 

    ``` PowerShell  
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.onmicrosoft.com"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.onmicrosoft.com"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > Çok kiracılı olmayan adımların devre dışı bırakılması, sırasıyla gerçekleştirilmelidir. Adım #2 önce tamamlandığında adım #1 başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Temsil edilen sağlayıcıları yönetme](azure-stack-delegated-provider.md)
- [Azure Stack temel kavramlar](azure-stack-overview.md)
- [Bulut Hizmeti Sağlayıcısı olarak Azure Stack kullanım ve faturalandırma özelliklerini yönetme](azure-stack-add-manage-billing-as-a-csp.md)
- [Azure Stack’e kullanım ve fatura kiracısı ekleme](azure-stack-csp-howto-register-tenants.md)
