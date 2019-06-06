---
title: Microsoft Azure Stack Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeleri, düzeltmeleri ve bilinen sorunlar için Azure Stack Geliştirme Seti.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 2ca85da5d9fde42fb06eef149e7304ab08bc32ee
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691207"
---
# <a name="asdk-release-notes"></a>ASDK sürüm notları

Bu makalede, değişiklikler, düzeltmeleri ve bilinen sorunlar Azure Stack geliştirme Seti'ni (ASDK) hakkında bilgi sağlar. Hangi sürümü çalıştırdığınızdan emin değilseniz yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](../operator/azure-stack-updates.md#determine-the-current-version).

Abone olarak ASDK yenilikler ile güncel kalın [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS akışı](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11905040"></a>Derleme 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özellikler listesi için bkz. [Bu bölümde](../operator/azure-stack-release-notes-1905.md#whats-in-this-update) Azure yığını sürüm notları.

### <a name="fixed-and-known-issues"></a>Sabit ve bilinen sorunlar

- Kayıt betiği için çalıştırırken bir hizmet sorumlusu zaman aşımı nedeniyle [ASDK kaydetme](asdk-register.md) başarıyla düzenlemelisiniz **RegisterWithAzure.psm1** PowerShell Betiği. Şunları yapın:

  1. Dosyayı ASDK ana bilgisayarda açın **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1** yükseltilmiş izinlerle bir düzenleyicide.
  2. 1249 satırına ekleme bir `-TimeoutInSeconds 1800` sonunda parametresi. Bu kayıt komut dosyası çalıştırılırken bir hizmet sorumlusu zaman aşımı nedeniyle gereklidir. Satır 1249 artık şu şekilde görünmelidir:

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- Bu sürümde giderilen diğer Azure Stack sorunların bir listesi için bkz. [Bu bölümde](../operator/azure-stack-release-notes-1905.md#fixes) Azure yığını sürüm notları.
- Bilinen sorunların bir listesi için bkz. [bu makalede](../operator/azure-stack-release-notes-known-issues-1905.md).
- Unutmayın [kullanılabilir Azure Stack düzeltmelerin](../operator/azure-stack-release-notes-1905.md#hotfixes) Azure Stack ASDK için geçerli değildir.

## <a name="build-11904036"></a>Derleme 1.1904.0.36

<!-- ### Changes -->

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özellikler listesi için bkz. [Bu bölümde](../operator/azure-stack-release-notes-1904.md#whats-in-this-update) Azure yığını sürüm notları.

### <a name="fixed-and-known-issues"></a>Sabit ve bilinen sorunlar

- Kayıt betiği için çalıştırırken bir hizmet sorumlusu zaman aşımı nedeniyle [ASDK kaydetme](asdk-register.md) başarıyla düzenlemelisiniz **RegisterWithAzure.psm1** PowerShell Betiği. Şunları yapın:

  1. Dosyayı ASDK ana bilgisayarda açın **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1** yükseltilmiş izinlerle bir düzenleyicide.
  2. 1249 satırına ekleme bir `-TimeoutInSeconds 1800` sonunda parametresi. Bu kayıt komut dosyası çalıştırılırken bir hizmet sorumlusu zaman aşımı nedeniyle gereklidir. Satır 1249 artık şu şekilde görünmelidir:

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- Tanımlanan VPN bağlantısı sorunu düzeltildi [burada 1902 serbest bırakmak](#known-issues).

- Bu sürümde giderilen diğer Azure Stack sorunların bir listesi için bkz. [Bu bölümde](../operator/azure-stack-release-notes-1904.md#fixes) Azure yığını sürüm notları.
- Bilinen sorunların bir listesi için bkz. [bu makalede](../operator/azure-stack-release-notes-known-issues-1904.md).
- Unutmayın [kullanılabilir Azure Stack düzeltmelerin](../operator/azure-stack-release-notes-1904.md#hotfixes) Azure Stack ASDK için geçerli değildir.

## <a name="build-1903"></a>Derleme 1903

1903 yükü ASDK yayın içermez.

## <a name="build-11902069"></a>Derleme 1.1902.0.69

### <a name="new-features"></a>Yeni Özellikler

- 1902 derleme planlar, teklifler, kotalar ve eklenti planı oluşturmak için Azure Stack Yönetici portalında yeni bir kullanıcı arabirimi sunar. Ekran görüntüleri de dahil daha fazla bilgi için bkz. [planlar, teklifler ve kotalar oluşturma](../operator/azure-stack-create-plan.md).

- Diğer değişiklikler ve geliştirmeler bu sürümde bir listesi için bkz. [Bu bölümde](../operator/azure-stack-update-1902.md#improvements) Azure Stack'te sürüm notları.

<!-- ### New features

- For a list of new features in this release, see [this section](../operator/azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../operator/azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../operator/azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../operator/azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>Bilinen sorunlar

- İçindeki adımları kullanarak ASDK içine başka bir konaktan bir VPN bağlantısı kurulurken bir sorun var. [bu makalede](asdk-connect.md). VPN istemcinizi ASDK ortama bağlanma girişiminde bulunduğunuzda bir hata görürsünüz, **kullanıcı adı veya parola yanlış**, kullanılan doğru hesabı ve parola doğru yazdığınızdan emin olduğunuzda bile. Kimlik bilgileriniz ile değil, ancak bunun yerine bir değişiklik ASDK VPN bağlantısı için kullanılan kimlik doğrulama protokolü ile bir sorundur. Bu sorunu geçici olarak çözmek için aşağıdaki adımları gerçekleştirin:

   ASDK sunucu tarafında kullanılan kimlik doğrulama protokolü için ilk olarak, değişiklikleri yapın:

   1. RDP ASDK konağa.
   2. Olarak AzureStack\AzureStackAdmin, dağıtım sırasında sağladığınız parola kullanarak oturum açmayı yükseltilmiş bir PowerShell oturumu açın.
   3. Aşağıdaki komutları çalıştırın:

      ```powershell
      netsh nps set np name = "Connections to Microsoft Routing and Remote Access server" profileid = "0x100a" profiledata = "1A000000000000000000000000000000" profileid = "0x1009" profiledata = "0x5"
      restart-service remoteaccess -force
      ```

   Ardından, istemci-tarafı bağlantı komut dosyasını değiştirin. Bunu yapmanın en kolay yolu, doğrudan C:\AzureStack-Tools-master\connect\azurestack.connect.psm1 betik modülündeki değişiklikler yapmak için verilmiştir:

   1. Değiştirme **Ekle AzsVpnConnection** değiştirmek için cmdlet `AuthenticationMethod` parametresinden `MsChapv2` için `EAP`:

      ```powershell
      $connection = Add-VpnConnection -Name $ConnectionName -ServerAddress $ServerAddress -TunnelType L2tp -EncryptionLevel Required -AuthenticationMethod Eap -L2tpPsk $PlainPassword -Force -RememberCredential -PassThru -SplitTunneling
      ```

   2. Değişiklik **Connect AzsVpn** cmdlet'ini kullanarak `rasdial @ConnectionName $User $PlainPassword` kullanarak `rasphone`, etkileşimli oturum açma EAP gerektirir:

      ```powershell
      rasphone $ConnectionName
      ```

   3. Yaptığınız değişiklikleri kaydedin ve yeniden içeri **azurestack.connect.psm1** modülü.
   4. Bölümündeki yönergeleri [bu makalede](asdk-connect.md#set-up-vpn-connectivity).
   5. Windows için giderek VPN aracılığıyla ASDK bağlandığınızda, bağlama **ağ ve Internet ayarları**, ardından **VPN**, görev çubuğundan emin olmak için istenir bağlanmak yerine kimlik bilgileri.

- Soruna hangi paketlerin 1450 bayt bir iç yük dengeleyici (ILB) için bırakılan belirlenmiştir. Sorunu 1901 itibarıyla konağa taşınamaz rolü, geçiş yapan kapsüllenmiş VXLAN paket uyum sağlamak için çok düşük olan konak üzerindeki MTU ayarı kaynaklanır. İçinde bu sorunu kendisini bildirim gördük karşılaşabileceğiniz en az iki senaryo vardır:

  - Bir iç yük dengeleyici (ILB) arkasında ve 660 bayt üzerinde olduğundan SQL her zaman açık SQL sorguları.
  - Birden çok ana sunucu etkinleştirmeye Kubernetes dağıtımları başarısız.  

  Aynı sanal ağda ancak farklı alt ağlarda bir ILB ile bir VM arasındaki iletişimi varsa sorun oluşur. Bu sorunu geçici olarak ASDK ana bilgisayarda yükseltilmiş komut isteminde aşağıdaki komutları çalıştırarak çalışabilirsiniz:

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```
