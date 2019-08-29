---
title: Azure Active Directory yeni bir Azure Stack kiracı hesabı ekleyin | Microsoft Docs
description: Kiracı portalını keşfedebilmeniz için, ASDK 'de kiracı hesabı oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: a75d5c88-5b9e-4e9a-a6e3-48bbfa7069a7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: 5d5292753b6fedfb7468a0bc68dd821a2da4cd54
ms.sourcegitcommit: e8f7fe07b32be33ef621915089344caf1fdca3fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118705"
---
# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Azure Active Directory yeni bir Azure Stack kiracı hesabı ekleyin

[Azure Stack geliştirme seti (ASDK) dağıttıktan](../asdk/asdk-install.md)sonra, kiracı portalını araştırabilmeniz ve tekliflerinizi ve planlarınızı test edebilmeniz için bir Kiracı Kullanıcı hesabına ihtiyacınız vardır. [Azure Portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) veya PowerShell 'i kullanarak Azure Active Directory (Azure AD) içinde bir kiracı hesabı oluşturabilirsiniz.

## <a name="create-an-azure-stack-tenant-account-by-using-the-azure-portal"></a>Azure portal kullanarak Azure Stack kiracı hesabı oluşturun

Azure portal kullanmak için bir Azure aboneliğinizin olması gerekir.

1. [Azure](https://portal.azure.com)'da oturum açın.
2. Sol gezinti çubuğunda **Active Directory** ' yi seçin ve Azure Stack için kullanmak istediğiniz dizine geçin (veya yeni bir tane oluşturun).
3. **Azure Active Directory** > kullanıcılarıYeni > **Kullanıcı**' yı seçin.

    ![Kullanıcılar - yeni kullanıcı ile vurgulanmış tüm kullanıcılar sayfası](media/azure-stack-add-new-user-aad/new-user-all-users.png)

4. **Kullanıcı** sayfasında, gerekli bilgileri doldurun.

    ![Yeni kullanıcı, kullanıcı bilgileri kullanıcının Sayfası Ekle](media/azure-stack-add-new-user-aad/new-user-user.png)

   - **Ad (gerekli)** : Yeni kullanıcı ilk ve son adı. Örneğin, Gamze Parker.
   - **Kullanıcı adı (gerekli)** : Yeni kullanıcının kullanıcı adı. Örneğin, mary@contoso.com.
       Kullanıcı adının etki alanı bölümünü ya da ilk varsayılan etki alanı adı kullanmanız gerekir <_etkialanıadınız_>. onmicrosoft.com ya da bir özel etki alanı adı contoso.com gibi. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için bkz. [Azure AD 'ye özel etki alanı adı ekleme](/azure/active-directory/fundamentals/add-custom-domain).
   - **Profil**: İsteğe bağlı olarak, Kullanıcı hakkında daha fazla bilgi ekleyebilirsiniz. Ayrıca, daha sonra Kullanıcı bilgileri de ekleyebilirsiniz. Kullanıcı bilgilerini ekleme hakkında daha fazla bilgi için bkz. [Kullanıcı profili bilgilerini ekleme veya değiştirme](/azure/active-directory/fundamentals/active-directory-users-profile-azure-portal).
   - **Dizin rolü**: **Kullanıcı**' yı seçin.

5. **Parolayı göster** ' i kontrol edin ve **parola** kutusunda belirtilen otomatik oluşturulan parolayı kopyalayın. İlk oturum açma işlemi için bu parolaya ihtiyacınız olacak.

6. **Oluştur**’u seçin.

    Kullanıcı oluşturulur ve Azure AD kiracınıza eklenir.

7. Azure portal yeni hesapla oturum açın. İstendiğinde parolayı değiştirin.
8. `https://portal.local.azurestack.external` Kiracı portalını görmek için yeni hesapla oturum açın.

## <a name="create-an-azure-stack-user-account-using-powershell"></a>PowerShell kullanarak Azure Stack Kullanıcı hesabı oluşturma

Azure aboneliğiniz yoksa, Kiracı Kullanıcı hesabı eklemek için Azure portal kullanamazsınız. Bu durumda, bunun yerine Windows PowerShell için Azure AD modülünü kullanabilirsiniz.

> [!NOTE]
> ASDK 'yi dağıtmak için Microsoft hesabı kullanıyorsanız, kiracı hesabı oluşturmak için Azure AD PowerShell kullanamazsınız.

1. [BT UZMANLARı RTW Için Microsoft Online Services oturum açma Yardımcısı](https://go.microsoft.com/fwlink/p/?LinkId=286152)'nın **64 bitlik** sürümünü yükler.

2. Windows PowerShell için Microsoft Azure AD Modülü şu adımlarla birlikte yüklemelisiniz:

    - Yükseltilmiş bir Windows PowerShell komut istemi açın (Windows PowerShell 'i yönetici olarak çalıştırın).
    - **Install-Module MSOnline** komutunu çalıştırın.
    - NuGet sağlayıcısını yüklemek isteyip istemediğiniz sorulursa **Y** ' yi seçin ve **girin**.
    - Bu modülü PSGallery 'den yüklemek isteyip istemediğiniz sorulursa **Y** ' yi seçin ve **girin**.

3. Aşağıdaki cmdlet 'leri çalıştırın:

    ```powershell
    # Provide the Azure AD credential you use to deploy the ASDK.

            $msolcred = get-credential

    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".

            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId

    ```

1. Yeni hesapla Azure 'da oturum açın. İstendiğinde parolayı değiştirin.
2. `https://portal.local.azurestack.external` Kiracı portalını görmek için yeni hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

[AD FS'ye Azure Stack kullanıcıları ekleme](azure-stack-add-users-adfs.md)
