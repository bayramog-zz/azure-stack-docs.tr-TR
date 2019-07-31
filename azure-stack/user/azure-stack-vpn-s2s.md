---
title: IPSec/ıKE siteden siteye VPN bağlantılarını yapılandırma | Microsoft Docs
description: Azure Stack ' de siteden siteye VPN veya VNet-VNet bağlantıları için IPSec/ıKE ilkesi hakkında bilgi edinin ve yapılandırın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: sethm
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 0c9c1af77ecf2bdf1c8da23cc7ab9e8d281067ea
ms.sourcegitcommit: b3dac698f2e1834491c2f9af56a80e95654f11f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658705"
---
# <a name="configure-ipsecike-policy-for-site-to-site-vpn-connections"></a>Siteden siteye VPN bağlantıları için IPSec/ıKE ilkesini yapılandırma

Bu makalede, Azure Stack ' de siteden siteye (S2S) VPN bağlantıları için IPSec/ıKE ilkesi yapılandırma adımları gösterilmektedir.

>[!NOTE]
> Bu özelliği kullanmak için Azure Stack Build **1809** veya sonraki bir sürümü çalıştırıyor olmanız gerekir.  Şu anda 1809 ' den önceki bir derlemeyi çalıştırıyorsanız, bu makaledeki adımlara geçmeden önce Azure Stack sisteminizi en son yapıya güncelleştirin.

## <a name="ipsec-and-ike-policy-parameters-for-vpn-gateways"></a>VPN ağ geçitleri için IPSec ve ıKE ilke parametreleri

IPSec ve ıKE protokol standardı çeşitli birleşimlerde çok sayıda şifreleme algoritmasını destekler. Uyumluluk veya güvenlik gereksinimlerinizi karşılayabilmeniz için Azure Stack hangi parametrelerin desteklendiğini görmek için bkz. [IPSec/IKE parametreleri](azure-stack-vpn-gateway-settings.md#ipsecike-parameters).

Bu makalede bir IPSec/ıKE ilkesi oluşturma ve yapılandırma ve yeni veya mevcut bir bağlantıya uygulama hakkında yönergeler sağlanmaktadır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu ilkeleri kullanırken aşağıdaki önemli noktalara dikkat edin:

- IPSec/ıKE ilkesi yalnızca *Standart* ve *HighPerformance* (rota tabanlı) ağ SKU 'larında kullanılabilir.

- Belirli bir bağlantı için yalnızca **bir** ilke birleşimi belirtebilirsiniz.

- IKE (ana mod) ve IPSec (hızlı mod) için tüm algoritmaları ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.

- İlkenin şirket içi VPN cihazlarınızda desteklendiğinden emin olmak için, VPN cihazınızın satıcı belirtimlerinize başvurun. İlkeler uyumsuz ise siteden siteye bağlantılar oluşturulamaz.

## <a name="part-1---workflow-to-create-and-set-ipsecike-policy"></a>Bölüm 1-IPSec/ıKE ilkesi oluşturmak ve ayarlamak için Iş akışı

Bu bölüm, siteden siteye VPN bağlantısında IPSec/ıKE ilkesi oluşturmak ve güncelleştirmek için gereken iş akışını özetler:

1. Bir sanal ağ ve VPN Ağ Geçidi oluşturun.

2. Şirketler arası bağlantı için yerel ağ geçidi oluşturun.

3. Seçili algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturun.

4. IPSec/ıKE ilkesiyle bir IPSec bağlantısı oluşturun.

5. Mevcut bir bağlantı için IPSec/ıKE İlkesi Ekle/Güncelleştir/Kaldır.

Bu makaledeki yönergeler, aşağıdaki şekilde gösterildiği gibi IPSec/ıKE ilkelerini ayarlamanıza ve yapılandırmanıza yardımcı olur:

![IPSec/ıKE ilkelerini ayarlama ve yapılandırma](media/azure-stack-vpn-s2s/site-to-site.png)

## <a name="part-2---supported-cryptographic-algorithms-and-key-strengths"></a>Bölüm 2-desteklenen şifreleme algoritmaları ve anahtar güçleri

Aşağıdaki tabloda, Azure Stack müşterileri tarafından yapılandırılabilen desteklenen şifreleme algoritmaları ve anahtar güçleri listelenmektedir:

| IPSec/Ikev2                                          | Seçenekler                                                                  |
|------------------------------------------------------|--------------------------------------------------------------------------|
| IKEv2 Şifrelemesi                                     | AES256, AES192, AES128, DES3, DES                                        |
| IKEv2 Bütünlüğü                                      | SHA384, SHA256, SHA1, MD5                                                |
| DH Grubu                                             | ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None         |
| IPsec Şifrelemesi                                     | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None |
| IPsec Bütünlüğü                                      | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                       |
| PFS Grubu                                            | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Hiçbiri                         |
| QM SA Yaşam Süresi                                       | (İsteğe bağlı: belirtilmemişse varsayılan değerler kullanılır)<br />                         Saniye (tamsayı; en az 300/varsayılan 27000 saniye)<br />                         Kbayt (tamsayı; en az 1024/varsayılan 102400000 Kbayt) |
| Trafik Seçicisi                                     | İlke tabanlı trafik seçicileri Azure Stack desteklenmez.         |

- Şirket içi VPN cihazı yapılandırmanızın Azure IPsec/IKE ilkesinde belirttiğiniz şu algoritmalar ve parametrelerle eşleşmesi ya da bunları içermesi gerekir:

  - IKE şifreleme algoritması (ana mod/Aşama 1).
  - IKE bütünlük algoritması (ana mod/Aşama 1).
  - DH grubu (ana mod/Aşama 1).
  - IPSec şifreleme algoritması (hızlı mod/aşama 2).
  - IPSec bütünlük algoritması (hızlı mod/aşama 2).
  - PFS Grubu (hızlı mod/aşama 2).
  - SA yaşam süreleri yalnızca yerel belirtimlerdir, bunların eşleşmesi gerekmez.

- IPSec şifreleme algoritması için GCMAES kullanılıyorsa, IPSec bütünlüğü için aynı GCMAES algoritmasını ve anahtar uzunluğunu seçmeniz gerekir. Örneğin: her ikisi için GCMAES128 kullanma.

- Yukarıdaki tabloda:

  - Ikev2, ana moda veya 1. aşamaya karşılık gelir.
  - IPSec, hızlı moda veya Aşama 2 ' ye karşılık gelir.
  - DH grubu, ana modda veya 1. aşamada kullanılan Diffie-Hellmence grubunu belirtir.
  - PFS Grubu hızlı modda veya Aşama 2 ' de kullanılan Diffie-Hellmence grubunu belirtir.

- Ikev2 ana mod SA yaşam süresi Azure Stack VPN ağ geçitlerinde 28.800 saniye içinde düzeltilir.

Aşağıdaki tabloda özel ilke tarafından desteklenen karşılık gelen Diffie-Hellman grupları listelenmektedir:

| Diffie-Hellman Grubu | DHGroup   | PFSGroup      | Anahtar uzunluğu    |
|----------------------|-----------|---------------|---------------|
| 1\.                    | DHGroup1  | PFS1          | 768 bit MODP  |
| 2                    | DHGroup2  | PFS2          | 1024 bit MODP |
| 14                   | DHGroup14<br/>DHGroup2048 | PFS2048       | 2048 bit MODP |
| 19                   | ECP256    | ECP256        | 256 bit ECP   |
| 20                   | ECP384    | ECP384        | 384 bit ECP   |
| 24                   | DHGroup24 | PFS24         | 2048 bit MODP |

Daha fazla bilgi için bkz. [RFC3526](https://tools.ietf.org/html/rfc3526) ve [RFC5114](https://tools.ietf.org/html/rfc5114).

## <a name="part-3---create-a-new-site-to-site-vpn-connection-with-ipsecike-policy"></a>3\. kısım-IPSec/ıKE ilkesiyle yeni siteden siteye VPN bağlantısı oluşturma

Bu bölüm, IPSec/ıKE ilkesiyle siteden siteye VPN bağlantısı oluşturma adımlarında size yol gösterir. Aşağıdaki adımlar, aşağıdaki şekilde gösterildiği gibi bağlantıyı oluşturur:

![siteden siteye-ilke](media/azure-stack-vpn-s2s/site-to-site.png)

Siteden siteye VPN bağlantısı oluşturmaya yönelik daha ayrıntılı adım adım yönergeler için bkz. siteden [sıteye VPN bağlantısı oluşturma](/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell).

### <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

- Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/)için kaydolabilirsiniz.

- Azure Resource Manager PowerShell cmdlet 'leri. PowerShell cmdlet 'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure Stack Için PowerShell 'ı yükleme](../operator/azure-stack-powershell-install.md).

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>1\. adım-sanal ağ, VPN Gateway ve yerel ağ geçidi oluşturma

#### <a name="1-declare-variables"></a>1. Değişkenleri bildirin

Bu alıştırma için aşağıdaki değişkenleri bildirerek başlayın. Üretim için yapılandırma sırasında yer tutucuları kendi değerlerinizle değiştirdiğinizden emin olun:

```powershell
$Sub1 = "<YourSubscriptionName>"
$RG1 = "TestPolicyRG1"
$Location1 = "East US 2"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPconf1 = "gw1ipconf1"
$Connection16 = "VNet1toSite6"
$LNGName6 = "Site6"
$LNGPrefix61 = "10.61.0.0/16"
$LNGPrefix62 = "10.62.0.0/16"
$LNGIP6 = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için bkz. [PowerShell ile Kullanıcı olarak Azure Stack bağlama](azure-stack-powershell-configure-user.md).

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Sanal ağ, VPN Gateway ve yerel ağ geçidi oluşturma

Aşağıdaki örnek, **TestVNet1**sanal ağını üç alt ağ ve VPN ağ geçidi ile birlikte oluşturur. Değerleri değiştirirken, ağ geçidi alt ağınızın özel bir **gatewaysubnet**'i her zaman yazmanız önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1

$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" `
-VirtualNetwork $vnet1

$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 `
-Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 `
-Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix `
$LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-site-to-site-vpn-connection-with-an-ipsecike-policy"></a>2\. adım-IPSec/ıKE ilkesiyle siteden siteye VPN bağlantısı oluşturma

#### <a name="1-create-an-ipsecike-policy"></a>1. IPSec/ıKE ilkesi oluşturma

Bu örnek betik, aşağıdaki algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturur:

- Ike AES128, SHA1, DHGroup14
- Bkz AES256, SHA256, None, SA ömür 14400 saniye ve 102400000KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup none -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

IPSec için GCMAES kullanıyorsanız, hem IPSec şifrelemesi hem de bütünlüğü için aynı GCMAES algoritmasını ve anahtar uzunluğunu kullanmanız gerekir.

#### <a name="2-create-the-site-to-site-vpn-connection-with-the-ipsecike-policy"></a>2. IPSec/ıKE ilkesiyle siteden siteye VPN bağlantısı oluşturma

Siteden siteye VPN bağlantısı oluşturun ve daha önce oluşturduğunuz IPSec/ıKE ilkesini uygulayın.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'Azs123'
```

> [!IMPORTANT]
> Bir bağlantı üzerinde IPSec/ıKE ilkesi belirtilmişse, Azure VPN ağ geçidi yalnızca belirtilen şifreleme algoritmalarıyla IPSec/ıKE teklifini ve söz konusu bağlantıda anahtar güçlerini gönderir veya kabul eder. Bağlantı için şirket içi VPN cihazınızın, tam ilke birleşimini kullandığından veya kabul ettiğinden emin olun, aksi takdirde siteden siteye VPN tüneli kurulmaz.

## <a name="part-4---update-ipsecike-policy-for-a-connection"></a>Bölüm 4-bağlantı için IPSec/ıKE ilkesini güncelleştirme

Önceki bölümde, var olan siteden siteye bağlantı için IPSec/ıKE ilkesini yönetme konusu gösterildi. Aşağıdaki bölümde bir bağlantıda aşağıdaki işlemler gösterilmektedir:

1. Bir bağlantının IPSec/ıKE ilkesini görüntüleyin.
2. IPSec/ıKE ilkesini bir bağlantıya ekleyin veya güncelleştirin.
3. IPSec/ıKE ilkesini bir bağlantıdan kaldırın.

> [!NOTE]
> IPSec/ıKE ilkesi yalnızca *Standart* ve *HIGHPERFORMANCE* rota tabanlı VPN ağ geçitlerinde desteklenir. *Temel* ağ geçidi SKU 'su üzerinde çalışmaz.

### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Bir bağlantının IPSec/ıKE ilkesini göster

Aşağıdaki örnek, bir bağlantıda yapılandırılan IPSec/ıKE ilkesinin nasıl alınacağını gösterir. Betikler, önceki alýþtýrmalara de devam eder:

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Son komut, varsa, bağlantıda yapılandırılan geçerli IPSec/ıKE ilkesini listeler. Aşağıdaki örnek bağlantı için bir örnek çıktıdır:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

IPSec/IKE ilkesi yapılandırılmamışsa, komut `$connection6.policy` boş bir dönüş alır. IPSec/ıKE 'nin bağlantıda yapılandırılmadığı anlamına gelmez; özel bir IPSec/ıKE ilkesi olmadığı anlamına gelir. Gerçek bağlantı, şirket içi VPN cihazınız ile Azure VPN ağ geçidi arasında anlaştığınız varsayılan ilkeyi kullanır.

### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Bir bağlantı için IPSec/ıKE ilkesi ekleme veya güncelleştirme

Yeni bir ilke ekleme veya bir bağlantıda var olan bir ilkeyi güncelleştirme adımları aynı. yeni bir ilke oluşturun ve ardından yeni ilkeyi bağlantıya uygulayın.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

$connection6.SharedKey = "AzS123"

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

İlkenin güncelleştirilip güncelleştirilmediğini denetlemek için bağlantıyı bir daha alabilirsiniz:

```powershell
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Aşağıdaki örnekte gösterildiği gibi son satırdan çıktıyı görmeniz gerekir:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Bir IPSec/ıKE ilkesini bir bağlantıdan kaldırma

Özel ilkeyi bir bağlantıdan kaldırdığınızda, Azure VPN ağ geçidi [varsayılan IPSec/IKE teklifine](azure-stack-vpn-gateway-settings.md#ipsecike-parameters)geri döner ve ŞIRKET içi VPN cihazınızdan yeniden anlaşmaya varılır.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.SharedKey = "AzS123"
$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

İlkenin bağlantıdan kaldırılıp kaldırılmadığını denetlemek için aynı betiği kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için VPN gateway yapılandırma ayarları](azure-stack-vpn-gateway-settings.md)
