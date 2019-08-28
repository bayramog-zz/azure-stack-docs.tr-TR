---
title: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulayın | Microsoft Docs
description: Azure Stack için bir özgün ekipman üreticisi (OEM) güncelleştirmesi uygulamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2019
ms.author: mabrigg
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: 73671431c70960fa517ee83c68945f14e2621b46
ms.sourcegitcommit: 9cb82df1eccb0486bcabec0bd674162d4820c00c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70060177"
---
# <a name="apply-azure-stack-original-equipment-manufacturer-oem-updates"></a>Özgün donanım üreticisi (OEM) güncelleştirmelerini Azure Stack Uygula

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Kullanıcılarınızın etkilerini en aza indirerek güvenlik düzeltme eklerinin yanı sıra sürücü ve bellenim iyileştirmeleri almak için Azure Stack donanım bileşenlerine özgün ekipman üreticisi (OEM) güncelleştirmeleri uygulayabilirsiniz. Bu makalede, OEM güncelleştirmeleri, OEM iletişim bilgileri ve bir OEM güncelleştirmesinin nasıl uygulanacağı hakkında bilgi edinebilirsiniz.

## <a name="overview-of-oem-updates"></a>OEM güncelleştirmelerine genel bakış

Microsoft Azure Stack güncelleştirmelere ek olarak, birçok OEM, Azure Stack donanımınız için sürücü ve bellenim güncelleştirmeleri gibi düzenli güncelleştirmeleri de serbest bırakmasını sağlar. Bunlar **OEM paketi güncelleştirmeleri**olarak adlandırılır. OEM 'nin OEM paketi güncelleştirmelerini yayımlayıp güncelleştirmediğini anlamak için, [oem Azure Stack belgelerine](#oem-contact-information)bakın.

Bu OEM paketi güncelleştirmeleri, **updateadminaccount** depolama hesabına yüklenir ve Azure Stack Yönetici portalı aracılığıyla uygulanır. Daha fazla bilgi için bkz. [OEM güncelleştirmelerini uygulama](#apply-oem-updates).

OEM paketi güncelleştirme bildirimlerinin kuruluşunuza ulaşmasını sağlamak için, özgün ekipman üreticinizden (OEM) belirli bildirim işlemleri hakkında sorun.

Bazı donanım satıcıları, iç bellenim güncelleştirme işlemini işleyen bir *donanım satıcısı sanal makinesi* gerektirebilir. Daha fazla bilgi için bkz. [donanım SATıCıSı VM 'Sini yapılandırma](#configure-hardware-vendor-vm)

## <a name="oem-contact-information"></a>OEM iletişim bilgileri 

Bu bölüm, OEM iletişim bilgilerini ve OEM Azure Stack başvuru malzemelerinden bağlantıları içerir.

| Donanım Iş ortağı | Bölge | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | Tümü | [Cisco Azure Stack destek/bellenim güncelleştirmeleri-otomatik bildirim (hesap/oturum açma gereklidir)](https://software.cisco.com/download/redirect?i=!y&mdfid=283862063&softwareid=286320368&release=1.0(0)&os=)<br><br>[Microsoft Azure Stack için Cisco Tümleşik sistemi sürüm notları](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) |
| Dell EMC | Tümü | [Microsoft Azure Stack 14G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Microsoft Azure Stack 13G için bulut (hesap ve oturum açma gerekir)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | JAPONYA | [Fujitsu yönetilen hizmet destek masası (hesap ve oturum açma gereklidir)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | EMEA | [Fujitsu BT ürünlerini ve sistemlerini destekler](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) |
|  | AB | [Fujitsu MySupport (hesap ve oturum açma gereklidir)](https://support.ts.fujitsu.com/IndexMySupport.asp) |
| HPE | Tümü | [Microsoft Azure Stack için HPE ProLiant](http://www.hpe.com/info/MASupdates) |
| Lenovo | Tümü | [Ölçülü Kagile SXD En Iyi Tarifler](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)
| Wortmann |  | [OEM/bellenim paketi](https://drive.terracloud.de/dl/fiTdTb66mwDAJWgUXUW8KNsd/OEM)<br>[Terra Azure Stack belgeleri (FRU dahil)](https://drive.terracloud.de/dl/fiWGZwCySZSQyNdykXCFiVCR/TerraAzSDokumentation)

## <a name="apply-oem-updates"></a>OEM güncelleştirmelerini uygulama

Aşağıdaki adımlarla OEM paketlerini uygulayın:

1. Şu şekilde OEM 'nize başvurmanız gerekir:
      - OEM paketinizin güncel sürümünü saptayın.  
      - OEM paketinizi indirmek için en iyi yöntemi bulun.  
2. [Tümleşik sistemler için güncelleştirme paketlerini indirme](azure-stack-servicing-policy.md)bölümünde ÖZETLENEN adımlarla OEM paketinizi hazırlayın.
3. Güncelleştirmeleri [Azure Stack güncelleştirme uygulama](azure-stack-apply-updates.md)bölümünde özetlenen adımlarla uygulayın.

## <a name="configure-hardware-vendor-vm"></a>Donanım satıcısı VM 'sini yapılandırma

Bazı donanım satıcıları, OEM güncelleştirme işleminde yardımcı olması için bir VM gerektirebilir. **Set-oemexternalvm** cmdlet 'ini çalıştırırken ve için hangi kimlik bilgilerinin `ProxyVM` kullanılması `HardwareManager` gerektiği ve için **-vmtype** gerekliyse, bu VM 'leri oluşturmak ve belgelemek için donanım satıcınız sorumlu olacaktır.  **Kimlik bilgileri**. VM 'Ler oluşturulduktan sonra, bunları ayrıcalıklı uç noktadan **set-OEMExternalVM** ile yapılandırın.

Azure Stack ayrıcalıklı uç nokta hakkında daha fazla bilgi için, bkz. [Azure Stack ayrıcalıklı uç noktasını kullanma](azure-stack-privileged-endpoint.md).

1.  Ayrıcalıklı uç noktaya erişin.

    ```powershell  
    $cred = Get-Credential
    $session = New-PSSession -ComputerName <IP Address of ERCS>
    -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```

2. **Set-OEMExternalVM** cmdlet 'ini kullanarak donanım satıcısı VM 'sini yapılandırın. Cmdlet 'i **-vmtype** `ProxyVM`için IP adresini ve kimlik bilgilerini doğrular. For **-vmtype** `HardwareManager` cmdlet 'i girişi doğrulamaz. **Set-OEMExternalVM** için belirtilen **-Credential** parametresi, donanım satıcısı belgelerinin açıkça belgelendiği bir sağlayıcıdır.  Ayrıcalıklı uç noktayla veya diğer mevcut Azure Stack kimlik bilgileriyle kullanılan CloudAdmin kimlik bilgisi DEĞILDIR.

    ```powershell  
    $VmCred = Get-Credential
    Invoke-Command -Session $session
        { 
    Set-OEMExternalVM -VMType <Either "ProxyVM" or "HardwareManager">
        -IPAddress <IP Address of hardware vendor VM> -Credential $using:VmCred
        }
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güncelleştirmeleri](azure-stack-updates.md)
