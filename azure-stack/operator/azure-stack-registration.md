---
title: Register Azure Stack with Azure
titleSuffix: Azure Stack
description: Learn how to register Azure Stack integrated systems with Azure so you can download Azure Marketplace items and set up data reporting.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2019
ms.author: mabrigg
ms.reviewer: avishwan
ms.lastreviewed: 03/04/2019
ms.openlocfilehash: d777827e6c700167dff6f203045277353837beef
ms.sourcegitcommit: 284f5316677c9a7f4c300177d0e2a905df8cb478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465433"
---
# <a name="register-azure-stack-with-azure"></a>Register Azure Stack with Azure

Register Azure Stack with Azure so you can download Azure Marketplace items from Azure and set up commerce data reporting back to Microsoft. After you register Azure Stack, usage is reported to Azure commerce and you can see it under the Azure billing Subscription ID used for registration.

The information in this article describes registering Azure Stack integrated systems with Azure. For information about registering the ASDK with Azure, see [Azure Stack registration](../asdk/asdk-register.md) in the ASDK documentation.

> [!IMPORTANT]  
> Registration is required to support full Azure Stack functionality, including offering items in the marketplace. You'll be in violation of Azure Stack licensing terms if you don't register when using the pay-as-you-use billing model. To learn more about Azure Stack licensing models, see the [How to buy page](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Önkoşullar

You need the following prerequisites in place before you register:

- Verify your credentials.
- Set the PowerShell language mode.
- Install PowerShell for Azure Stack.
- Download the Azure Stack tools.
- Determine your registration scenario.

### <a name="verify-your-credentials"></a>Verify your credentials

Before registering Azure Stack with Azure, you must have:

- The subscription ID for an Azure subscription. Only EA, CSP, or CSP shared services subscriptions are supported for registration. CSPs need to decide whether to [use a CSP or APSS subscription](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription).<br><br>To get the ID, sign in to Azure, click **All services**. Then, under the **GENERAL** category, select **Subscriptions**, click the subscription you want to use, and under **Essentials** you can find the Subscription ID. As a best practice, use separate subscriptions for production and dev or test environments. 

  > [!Note]  
  > Germany cloud subscriptions aren't currently supported.

- The username and password for an account that's an owner for the subscription.

- The user account needs to have access to the Azure subscription and have permissions to create identity apps and service principals in the directory associated with that subscription. We recommend that you register Azure Stack with Azure using least-privilege administration. For more information on how to create a custom role definition that limits access to your subscription for registration, see [create a registration role for Azure Stack](azure-stack-registration-role.md).

- Registered the Azure Stack resource provider (see the following Register Azure Stack Resource Provider section for details).

After registration, Azure Active Directory (Azure AD) global administrator permission isn't required. However, some operations may require the global admin credential (for example, a resource provider installer script or a new feature requiring a permission to be granted). You can either temporarily reinstate the account's global admin permissions or use a separate global admin account that's an owner of the *default provider subscription*.

The user who registers Azure Stack is the owner of the service principal in Azure AD. Only the user who registered Azure Stack can modify the Azure Stack registration. If a non-admin user that's not an owner of the registration service principal attempts to register or re-register Azure Stack, they may come across a 403 response. A 403 response indicates the user has insufficient permissions to complete the operation.

If you don't have an Azure subscription that meets these requirements, you can [create a free Azure account here](https://azure.microsoft.com/free/?b=17.06). Registering Azure Stack incurs no cost on your Azure subscription.

> [!NOTE]
> If you have more than one Azure Stack, a best practice is to register each Azure Stack to its own subscription. This makes it easier for you to track usage.

### <a name="powershell-language-mode"></a>PowerShell language mode

To successfully register Azure Stack, the PowerShell language mode must be set to **FullLanguageMode**.  To verify that the current language mode is set to full, open an elevated PowerShell window and run the following PowerShell cmdlets:

```powershell  
$ExecutionContext.SessionState.LanguageMode
```

Ensure the output returns **FullLanguageMode**. If any other language mode is returned, registration needs to be run on another machine or the language mode needs to be set to **FullLanguageMode** before continuing.

### <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell yükleme

Use the latest PowerShell for Azure Stack to register with Azure.

If the latest version isn't already installed, see [install PowerShell for Azure Stack](azure-stack-powershell-install.md).

### <a name="download-the-azure-stack-tools"></a>Download the Azure Stack tools

The Azure Stack tools GitHub repository contains PowerShell modules that support Azure Stack functionality, including registration functionality. During the registration process, you need to import and use the **RegisterWithAzure.psm1** PowerShell module (found in the Azure Stack tools repository) to register your Azure Stack instance with Azure.

To ensure you're using the latest version, delete any existing versions of the Azure Stack tools and [download the latest version from GitHub](azure-stack-powershell-download.md) before registering with Azure.

### <a name="determine-your-registration-scenario"></a>Determine your registration scenario

Your Azure Stack deployment may be *connected* or *disconnected*.

- **Connected**  
 Connected means you've deployed Azure Stack so that it can connect to the internet and to Azure. You either have Azure AD or Active Directory Federation Services (AD FS) for your identity store. With a connected deployment, you can choose from two billing models: pay-as-you-use or capacity-based.
  - [Register a connected Azure Stack with Azure using the **pay-as-you-use** billing model](#register-connected-with-pay-as-you-go-billing).
  - [Register a connected Azure Stack with Azure using the **capacity** billing model](#register-connected-with-capacity-billing).

- **Disconnected**  
 With the disconnected from Azure deployment option, you can deploy and use Azure Stack without a connection to the internet. However, with a disconnected deployment, you're limited to an AD FS identity store and the capacity-based billing model.
  - [Register a disconnected Azure Stack using the **capacity** billing model ](#register-disconnected-with-capacity-billing).

### <a name="determine-a-unique-registration-name-to-use"></a>Determine a unique registration name to use

When you register Azure Stack with Azure, you must provide a unique registration name. An easy way to associate your Azure Stack subscription with an Azure registration is to use your Azure Stack **Cloud ID**.

> [!NOTE]
> Azure Stack registrations using the capacity-based billing model will need to change the unique name when re-registering after those yearly subscriptions expire unless you [delete the expired registration](azure-stack-registration.md#change-the-subscription-you-use) and re-register with Azure.

To determine the Cloud ID for your Azure Stack deployment, open PowerShell as an admin on a computer that can access the Privileged Endpoint, run the following commands, and then record the **CloudID** value:

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: Get-AzureStackStampInformation
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Register connected with pay-as-you-go billing

Use these steps to register Azure Stack with Azure using the pay-as-you-use billing model.

> [!Note]  
> All these steps must be run from a computer that has access to the privileged endpoint (PEP). For details about the PEP, see [Using the privileged endpoint in Azure Stack](azure-stack-privileged-endpoint.md).

Connected environments can access the internet and Azure. For these environments, you need to register the Azure Stack resource provider with Azure and then configure your billing model.

1. To register the Azure Stack resource provider with Azure, start PowerShell ISE as an administrator and use the following PowerShell cmdlets with the **EnvironmentName** parameter set to the appropriate Azure subscription type (see parameters below).

2. Add the Azure account that you used to register Azure Stack. To add the account, run the **Add-AzureRmAccount** cmdlet. You're prompted to enter your Azure account credentials and you may have to use two-factor authentication based on your account's configuration.

   ```powershell
   Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | The Azure cloud subscription environment name. Supported environment names are **AzureCloud**, **AzureUSGovernment**, or if using a China Azure Subscription, **AzureChinaCloud**.  |

   >[!Note]
   > If your session expires, your password has changed, or you simply wish to switch accounts, run the following cmdlet before you sign in using Add-AzureRmAccount: `Remove-AzureRmAccount-Scope Process`

3. If you have multiple subscriptions, run the following command to select the one you want to use:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Run the following command to register the Azure Stack resource provider in your Azure subscription:

   ```powershell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Start PowerShell ISE as an administrator and navigate to the **Registration** folder in the **AzureStack-Tools-master** directory created when you downloaded the Azure Stack tools. Import the **RegisterWithAzure.psm1** module using PowerShell:

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Next, in the same PowerShell session, ensure you're signed in to the correct Azure PowerShell context. This context would be the Azure account that was used to register the Azure Stack resource provider previously. Powershell to run:

   ```powershell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | The Azure cloud subscription environment name. Supported environment names are **AzureCloud**, **AzureUSGovernment**, or if using a China Azure Subscription, **AzureChinaCloud**.  |

7. In the same PowerShell session, run the **Set-AzsRegistration** cmdlet. PowerShell to run:  

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   For more information on the Set-AzsRegistration cmdlet, see [Registration reference](#registration-reference).

   The process takes between 10 and 15 minutes. When the command completes, you see the message **"Your environment is now registered and activated using the provided parameters."**

## <a name="register-connected-with-capacity-billing"></a>Register connected with capacity billing

Use these steps to register Azure Stack with Azure using the pay-as-you-use billing model.

> [!Note]  
> All these steps must be run from a computer that has access to the privileged endpoint (PEP). For details about the PEP, see [Using the privileged endpoint in Azure Stack](azure-stack-privileged-endpoint.md).

Connected environments can access the internet and Azure. For these environments, you need to register the Azure Stack resource provider with Azure and then configure your billing model.

1. To register the Azure Stack resource provider with Azure, start PowerShell ISE as an administrator and use the following PowerShell cmdlets with the **EnvironmentName** parameter set to the appropriate Azure subscription type (see parameters below).

2. Add the Azure account that you used to register Azure Stack. To add the account, run the **Add-AzureRmAccount** cmdlet. You're prompted to enter your Azure account credentials and you may have to use two-factor authentication based on your account's configuration.

   ```powershell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | The Azure cloud subscription environment name. Supported environment names are **AzureCloud**, **AzureUSGovernment**, or if using a China Azure Subscription, **AzureChinaCloud**.  |

3. If you have multiple subscriptions, run the following command to select the one you want to use:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Run the following command to register the Azure Stack resource provider in your Azure subscription:

   ```powershell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Start PowerShell ISE as an administrator and navigate to the **Registration** folder in the **AzureStack-Tools-master** directory created when you downloaded the Azure Stack tools. Import the **RegisterWithAzure.psm1** module using PowerShell:

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity `
      -RegistrationName $RegistrationName
   ```
   > [!Note]  
   > You can disable usage reporting with the UsageReportingEnabled parameter for the **Set-AzsRegistration** cmdlet by setting the parameter to false. 
   
   For more information on the Set-AzsRegistration cmdlet, see [Registration reference](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Register disconnected with capacity billing

If you're registering Azure Stack in a disconnected environment (with no internet connectivity), you need to get a registration token from the Azure Stack environment. Then use that token on a computer that can connect to Azure and has PowerShell for Azure Stack installed.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Get a registration token from the Azure Stack environment

1. Start PowerShell ISE as an administrator and navigate to the **Registration** folder in the **AzureStack-Tools-master** directory created when you downloaded the Azure Stack tools. Import the **RegisterWithAzure.psm1** module:  

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. To get the registration token, run the following PowerShell cmdlets:  

   ```Powershell
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   For more information on the Get-AzsRegistrationToken cmdlet, see [Registration reference](#registration-reference).

   > [!Tip]  
   > The registration token is saved in the file specified for *$FilePathForRegistrationToken*. You can change the filepath or filename at your discretion.

3. Save this registration token for use on the Azure-connected machine. You can copy the file or the text from *$FilePathForRegistrationToken*.

### <a name="connect-to-azure-and-register"></a>Connect to Azure and register

On the computer that is connected to the internet, do the same steps to import the RegisterWithAzure.psm1 module and sign in to the correct Azure Powershell context. Then call Register-AzsEnvironment. Specify the registration token to register with Azure. If you're registering more than one instance of Azure Stack using the same Azure Subscription ID, specify a unique registration name.

You need your registration token and a unique token name.

1. Start PowerShell ISE as an administrator and navigate to the **Registration** folder in the **AzureStack-Tools-master** directory created when you downloaded the Azure Stack tools. Import the **RegisterWithAzure.psm1** module:  

   ```powershell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Then run the following PowerShell cmdlets:  

  ```powershell  
  $RegistrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

Optionally, you can use the Get-Content cmdlet to point to a file that contains your registration token.

You need your registration token and a unique token name.

1. Start PowerShell ISE as an administrator and navigate to the **Registration** folder in the **AzureStack-Tools-master** directory created when you downloaded the Azure Stack tools. Import the **RegisterWithAzure.psm1** module:  

  ```powershell  
  Import-Module .\RegisterWithAzure.psm1
  ```

2. Then run the following PowerShell cmdlets:  

  ```powershell  
  $RegistrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > Save the registration resource name and the registration token for future reference.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Retrieve an Activation Key from Azure Registration Resource

Next, you need to retrieve an activation key from the registration resource created in Azure during Register-AzsEnvironment.

To get the activation key, run the following PowerShell cmdlets:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > The activation key is saved in the file specified for *$KeyOutputFilePath*. You can change the filepath or filename at your discretion.

### <a name="create-an-activation-resource-in-azure-stack"></a>Create an Activation Resource in Azure Stack

Return to the Azure Stack environment with the file or text from the activation key created from Get-AzsActivationKey. Next create an activation resource in Azure Stack using that activation key. To create an activation resource, run the following PowerShell cmdlets:

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

Optionally, you can use the Get-Content cmdlet to point to a file that contains your registration token:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Verify Azure Stack registration

You can use the **Region management** tile to verify that the Azure Stack registration was successful. This tile is available on the default dashboard in the administrator portal. The status can be registered, or not registered. If registered, it also shows the Azure subscription ID that you used to register your Azure Stack along with the registration resource group and name.

1. Sign in to the [Azure Stack administrator portal](https://adminportal.local.azurestack.external).

2. From the Dashboard, select **Region management**.

3. **Özellikler**’i seçin. This blade shows the status and details of your environment. The status can be **Registered**, **Not registered**, or **Expired**.

    [![Region management tile in Azure Stack administrator portal](media/azure-stack-registration/admin1sm.png "Region management tile")](media/azure-stack-registration/admin1.png#lightbox)

    If registered, the properties include:
    
    - **Registration subscription ID**: The Azure subscription ID registered and associated to Azure Stack.
    - **Registration resource group**: The Azure resource group in the associated subscription containing the Azure Stack resources.

4. You can use the Azure portal to view Azure Stack registration resources, and then verify that the registration succeeded. Sign in to the [Azure portal](https://portal.azure.com) using an account associated to the subscription you used to register Azure Stack. Select **All resources**, enable the **Show hidden types** checkbox, and select the registration name.

5. If the registration didn't succeed, you must re-register by following the [steps here](#change-the-subscription-you-use) to resolve the issue.  

Alternatively, you can verify if your registration was successful by using the Marketplace management feature. If you see a list of marketplace items in the Marketplace management blade, your registration was successful. However, in disconnected environments, you can't see marketplace items in Marketplace management.

> [!NOTE]
> After registration is complete, the active warning for not registering will no longer appear. In Azure Stack releases before 1904, in disconnected scenarios, you see a message in Marketplace management asking you to register and activate your Azure Stack, even if you have registered successfully. This message doesn't appear in release 1904 and later.

## <a name="renew-or-change-registration"></a>Renew or change registration

### <a name="renew-or-change-registration-in-connected-environments"></a>Renew or change registration in connected environments

You need to update or renew your registration in the following circumstances:

- After you renew your capacity-based yearly subscription.
- When you change your billing model.
- When you scale changes (add/remove nodes) for capacity-based billing.

#### <a name="change-the-subscription-you-use"></a>Change the subscription you use

If you want to change the subscription you use, you must first run the **Remove-AzsRegistration** cmdlet, then ensure you're signed in to the correct Azure PowerShell context. Then run **Set-AzsRegistration** with any changed parameters including `<billing model>`:

  ```powershell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -RegistrationName $RegistrationName
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Change the billing model or how to offer features

If you want to change the billing model or how to offer features for your installation, you can call the registration function to set the new values. You don't need to first remove the current registration:

  ```powershell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Renew or change registration in disconnected environments

You need to update or renew your registration in the following circumstances:

- After you renew your capacity-based yearly subscription.
- When you change your billing model.
- When you scale changes (add/remove nodes) for capacity-based billing.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Remove the activation resource from Azure Stack

You first need to remove the activation resource from Azure Stack, and then the registration resource in Azure.  

To remove the activation resource in Azure Stack, run the following PowerShell cmdlets in your Azure Stack environment:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Next, to remove the registration resource in Azure, ensure you're on an Azure-connected computer, sign in to the correct Azure PowerShell context, and run the appropriate PowerShell cmdlets as described below.

You can use the registration token used to create the resource:  

  ```Powershell
  $RegistrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $RegistrationToken
  ```

Or you can use the registration name:

  ```Powershell
  $RegistrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $RegistrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Re-register using disconnected steps

You've now completely unregistered in a disconnected scenario and must repeat the steps for registering an Azure Stack environment in a disconnected scenario.

### <a name="disable-or-enable-usage-reporting"></a>Disable or enable usage reporting

For Azure Stack environments that use a capacity billing model, turn off usage reporting with the **UsageReportingEnabled** parameter using either the **Set-AzsRegistration** or the **Get-AzsRegistrationToken** cmdlets. Azure Stack reports usage metrics by default. Operators with capacity uses or supporting a disconnected environment need to turn off usage reporting.

#### <a name="with-a-connected-azure-stack"></a>With a connected Azure Stack

   ```powershell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>With a disconnected Azure Stack

1. To change the registration token, run the following PowerShell cmdlets:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > The registration token is saved in the file specified for *$FilePathForRegistrationToken*. You can change the filepath or filename at your discretion.

2. Save this registration token for use on the Azure connected machine. You can copy the file or the text from *$FilePathForRegistrationToken*.

## <a name="move-a-registration-resource"></a>Move a registration resource

Moving a registration resource between resource groups under the same subscription **is** supported for all environments. However, moving a registration resource between subscriptions is only supported for CSPs when both subscriptions resolve to the same Partner ID. For more information about moving resources to a new resource group, see [Move resources to new resource group or subscription](/azure/azure-resource-manager/resource-group-move-resources).

> [!IMPORTANT]
> To prevent accidental deletion of registration resources on the portal, the registration script automatically adds a lock to the resource. You must remove this lock before moving or deleting it. It's recommended that you add a lock to your registration resource to prevent accidental deletion.

## <a name="registration-reference"></a>Registration reference

### <a name="set-azsregistration"></a>Set-AzsRegistration

You can use **Set-AzsRegistration** to register Azure Stack with Azure and enable or disable the offer of items in the marketplace and usage reporting.

To run the cmdlet, you need:

- A global Azure subscription of any type.
- To be signed in to Azure PowerShell with an account that's an owner or contributor to that subscription.

```powershell
Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
```

| Parametre | Tür | Açıklama |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | The credentials used to [access the privileged endpoint](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). The username is in the format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Dize | A pre-configured remote PowerShell console that provides you with capabilities like log collection and other post deployment tasks. To learn more, refer to the [using the privileged endpoint](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) article. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | The billing model that your subscription uses. Allowed values for this parameter are: Capacity, PayAsYouUse, and Development. |
| MarketplaceSyndicationEnabled | True/False | Determines if the marketplace management feature is available in the portal. Set to true if registering with internet connectivity. Set to false if registering in disconnected environments. For disconnected registrations, the [offline syndication tool](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) can be used for downloading marketplace items. |
| UsageReportingEnabled | True/False | Azure Stack reports usage metrics by default. Operators with capacity uses or supporting a disconnected environment need to turn off usage reporting. Allowed values for this parameter are: True, False. |
| AgreementNumber | Dize |  |
| RegistrationName | Dize | Set a unique name for the registration if you're running the registration script on more than one instance of Azure Stack using the same Azure Subscription ID. The parameter has a default value of **AzureStackRegistration**. However, if you use the same name on more than one instance of Azure Stack, the script fails. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken generates a registration token from the input parameters.

```powershell  
Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parametre | Tür | Açıklama |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | The credentials used to [access the privileged endpoint](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). The username is in the format **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Dize |  A pre-configured remote PowerShell console that provides you with capabilities like log collection and other post deployment tasks. To learn more, refer to the [using the privileged endpoint](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) article. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | The billing model that your subscription uses. Allowed values for this parameter are: Capacity, PayAsYouUse, and Development. |
| MarketplaceSyndicationEnabled | True/False |  |
| UsageReportingEnabled | True/False | Azure Stack reports usage metrics by default. Operators with capacity uses or supporting a disconnected environment need to turn off usage reporting. Allowed values for this parameter are: True, False. |
| AgreementNumber | Dize |  |

## <a name="registration-failures"></a>Kayıt hataları

You might see one of the errors below while attempting to register your Azure Stack:

- Could not retrieve mandatory hardware info for $hostName. Check physical host and connectivity then try to rerun registration.

- Cannot connect to $hostName to get hardware info - please check physical host and connectivity then try to rerun registration.

> Cause: this is typically because we try to obtain hardware details such as UUID, Bios, and CPU from the hosts to attempt activation and weren't able to due to the inability to connect to the physical host.

When trying to access Marketplace management, an error occurs when trying to syndicate products. 
> Cause: this usually happens when Azure Stack is unable to access the registration resource. One common reason for this is that when an Azure subscription's directory tenant changes, it resets the registration. You can't access the Azure Stack marketplace or report usage if you've changed the subscription's directory tenant. You need to re-register to fix this issue.

Marketplace management still asks you to register and activate your Azure Stack even when you've already registered your stamp using the disconnected process.
> Cause: this is a known issue for disconnected environments. You can verify your registration status by following [these steps](azure-stack-registration.md#verify-azure-stack-registration). In order to use Marketplace management, you need to use [the offline tool](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario).

## <a name="next-steps"></a>Sonraki adımlar

[Download marketplace items from Azure](azure-stack-download-azure-marketplace-item.md)
