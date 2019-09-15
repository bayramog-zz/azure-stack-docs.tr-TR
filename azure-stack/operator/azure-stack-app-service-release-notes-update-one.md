---
title: App Service Azure Stack güncelleştirme 1 sürüm notları | Microsoft Docs
description: Azure Stack App Service için güncelleştirme 1 ' deki geliştirmeler, düzeltmeler ve bilinen sorunlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: bryanla
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/20/2018
ms.openlocfilehash: 7be74701b3e25658258abc7102668346e584ab39
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974818"
---
# <a name="app-service-on-azure-stack-update-1-release-notes"></a>App Service Azure Stack güncelleştirme 1 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure App Service Azure Stack güncelleştirme 1 ' de iyileştirmeler, düzeltmeler ve bilinen sorunları anlatmaktadır. Bilinen sorunlar üç bölüme ayrılmıştır: doğrudan dağıtım, güncelleştirme işlemiyle ilgili sorunlar ve yapıyla ilgili sorunlar (yükleme sonrası).

> [!IMPORTANT]
> 1802 güncelleştirmesini Azure Stack tümleşik sisteminize uygulayın veya Azure App Service dağıtmadan önce en son Azure Stack Geliştirme Seti (ASDK) dağıtın.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 1 ' deki App Service derleme numarası **69.0.13698.9**.

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure Stack Azure App Service yeni dağıtımları artık, kudu için SSO 'nun Azure App Service işlendiği şekilde iyileştirmeler nedeniyle [üç konuyla ilgili bir joker sertifikaya](azure-stack-app-service-before-you-get-started.md#get-certificates) gerek duyar. Yeni konu  **\*. SSO.\< appservice. Bölge\>.\< DomainName\>.\< uzantının\>**

Dağıtıma başlamadan önce [Azure Stack App Service dağıtmaya yönelik önkoşulları](azure-stack-app-service-before-you-get-started.md) inceleyin.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 1 ' deki Azure App Service aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **Azure App Service yüksek kullanılabilirliği** -Azure Stack 1802 güncelleştirme etkin iş yükleri hata etki alanları arasında dağıtılarak App Service altyapısının hata etki alanlarına dağıtıldığı için hataya dayanıklı olmasını sağlar. Varsayılan olarak, Azure App Service tüm yeni dağıtımları bu özelliğe sahiptir. Ancak, Azure Stack 1802 güncelleştirmesi uygulanmadan önce tamamlanan dağıtımlar için, [App Service hata etki alanı belgelerine](azure-stack-app-service-before-you-get-started.md)bakın.

- **Var olan sanal ağda dağıt** -müşteriler artık, mevcut bir sanal ağ içindeki Azure Stack App Service dağıtabilir. Mevcut bir sanal ağda dağıtmak, müşterilerin özel bağlantı noktaları üzerinden Azure App Service için gereken SQL Server ve dosya sunucusuna bağlanmasını sağlar. Dağıtım sırasında, müşteriler mevcut bir sanal ağda dağıtmayı seçebilir, ancak dağıtımdan önce [App Service tarafından kullanılmak üzere alt ağlar oluşturmaları gerekir](azure-stack-app-service-before-you-get-started.md#virtual-network) .

- **Kiracı, yönetici, işlevler portalları ve kudu araçları App Service**güncelleştirmeler. Azure Stack Portal SDK sürümü ile tutarlıdır.

- **Azure işlevleri çalışma zamanını** **v 1.0.11388**'a güncelleştirir.

- **Aşağıdaki uygulama çerçeveleri ve araçları Için güncelleştirmeler**:
    - **.NET Core 2,0** desteği eklendi.
    - **Node. js** sürümleri eklendi:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - **NPM** sürümleri eklendi:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - **Php** güncelleştirmeleri eklendi:
        - 5.6.32
        - 7.0.26 (x86 ve x64)
        - 7.1.12 (x86 ve x64)
    - **Windows Için git** , v 2.14.1 'ye güncelleştirildi
    - **Mercurial** , v 4.5.0 'ye güncelleştirildi

  - App Service kiracı portalındaki özel etki alanı özelliği içinde **yalnızca https** özelliği için destek eklendi.

  - Azure Işlevleri için özel depolama seçicisine depolama bağlantısı doğrulaması eklendi.

#### <a name="fixes"></a>Düzeltmeleri

- Çevrimdışı bir dağıtım paketi oluştururken, müşteriler artık App Service yükleyicisinden klasör açarken erişim reddedildi hata iletisi almaz.

- App Service kiracı portalındaki özel etki alanları özelliğinde çalışırken oluşan sorunlar çözüldü.

- Müşterilerin kurulum sırasında ayrılmış yönetici adlarını kullanmalarını engelleyin.

- **Etki alanına katılmış** dosya sunucusuyla App Service dağıtımı etkinleştirildi.

- Komut dosyasında Azure Stack kök sertifikanın ve App Service yükleyicisindeki kök sertifikayı doğrulama özelliği ile geliştirilmiş bir şekilde alımı.

- Microsoft. Web ad alanındaki kaynakları içeren bir abonelik silindiğinde Azure Resource Manager için hatalı durum düzeltildi.

### <a name="known-issues-with-the-deployment-process"></a>Dağıtım işlemi ile ilgili bilinen sorunlar

- Sertifika doğrulama hataları.

    Bazı müşteriler, yükleyicideki fazla kısıtlayıcı doğrulama nedeniyle, tümleşik bir sisteme dağıtım yaparken App Service yükleyiciye sertifika sağlarken sorunlarla karşılaşmıştır. App Service yükleyicisi yeniden yayımlanmıştır ve müşteriler [güncelleştirilmiş yükleyiciyi indirmelidir](https://aka.ms/appsvconmasinstaller). Güncelleştirilmiş yükleyiciyle sertifikaları doğrularken sorunlarla karşılaşmaya devam ederseniz, desteğe başvurun.

- Tümleşik sistemden Azure Stack kök sertifika alınırken sorun oluştu.

    Get-AzureStackRootCert. ps1 içindeki bir hata, müşterilerin kök sertifikası yüklü olmayan bir makinede betiği yürütürken Azure Stack kök sertifikayı almamasına neden oldu. Bu komut dosyası artık sorunu çözen yeniden yayımlanmıştır. [Güncelleştirilmiş yardımcı betikleri buraya indirin](https://aka.ms/appsvconmashelpers). Güncelleştirilmiş betikle kök sertifikayı alma sorunlarını yaşamaya devam ederseniz, desteğe başvurun.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Azure Stack güncelleştirme 1 ' de Azure App Service güncelleştirmesi için bilinen bir sorun yoktur.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Yuva değiştirme işlevi çalışmıyor.

Site yuvası değiştirme bu sürümde bozuk. İşlevselliği geri yüklemek için şu adımları izleyin:

1. App Service denetleyicisi örneklerine Uzak Masaüstü bağlantılarına **izin** vermek Için ControllersNSG ağ güvenlik grubunu değiştirin. AppService. Local ' i App Service dağıttığınız kaynak grubunun adıyla değiştirin.

    ```powershell
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

2. Azure Stack yönetici portalındaki sanal makineler altında **CN0-VM** ' y e gidin ve denetleyici örneğiyle Uzak Masaüstü oturumu açmak için **Bağlan ' a tıklayın** . App Service dağıtımı sırasında belirtilen kimlik bilgilerini kullanın.
3. **PowerShell 'ı yönetici olarak** başlatın ve aşağıdaki betiği yürütün:

    ```powershell
        Import-Module appservice

        $sm = new-object Microsoft.Web.Hosting.SiteManager

        if($sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$true)
        {
          $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$false
        #  'Slot swap mode reverted'
        }
        
        # Confirm new setting is false
        $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus
        
        # Commit Changes
        $sm.CommitChanges()

        Get-AppServiceServer -ServerType ManagementServer | ForEach-Object Repair-AppServiceServer
        
    ```

4. Uzak Masaüstü oturumunu kapatın.
5. App Service denetleyicisi örneklerine yönelik uzak masaüstü bağlantılarını **reddetmek** Için ControllersNSG ağ güvenlik grubunu dönün. AppService. Local ' i App Service dağıttığınız kaynak grubunun adıyla değiştirin.

    ```powershell

        Add-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

6. Mevcut bir sanal ağda App Service dağıtıldığında ve dosya sunucusu yalnızca özel ağda kullanılabilir olduğunda çalışanlar dosya sunucusuna ulaşamamakta olur.

Dosya sunucunuza bağlanmak için mevcut bir sanal ağa ve bir iç IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği sağlayan bir giden güvenlik kuralı eklemeniz gerekir. Yönetici portalında WorkersNsg adresine gidin ve aşağıdaki özelliklerle bir giden güvenlik kuralı ekleyin:

- Kaynak: Any
- Kaynak bağlantı noktası aralığı: *
- Hedef: IP adresleri
- Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
- Hedef bağlantı noktası aralığı: 445
- Protokol: TCP
- Eylem: Allow
- Öncelik: 700
- Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Cloud Admins işletim Azure App Service ilgili bilinen sorunlar

[Azure Stack 1802 sürüm notlarındaki](azure-stack-update-1903.md) belgelere bakın

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service genel bakış için bkz. [Azure Stack genel bakış Azure App Service](azure-stack-app-service-overview.md).
- Azure Stack App Service dağıtmaya hazırlanma hakkında daha fazla bilgi için bkz. [Azure Stack üzerinde App Service dağıtmak Için Önkoşullar](azure-stack-app-service-before-you-get-started.md).
