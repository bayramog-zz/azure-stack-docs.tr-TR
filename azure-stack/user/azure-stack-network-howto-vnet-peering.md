---
title: VNET eşlemesi aracılığıyla iki Azure yığınını bağlama | Microsoft Docs
description: VNET eşlemesi aracılığıyla iki Azure yığınını bağlamayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/03/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/03/2019
ms.openlocfilehash: 91a90146f1f62ebea980a785d8a8e347f04fe137
ms.sourcegitcommit: 58e1911a54ba249a82fa048c7798dadedb95462b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73064718"
---
# <a name="vnet-peering-in-azure-stack-with-vms"></a>VM 'lerle Azure Stack Sanal Ağ Eşleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

İki Azure Stack sanal ağı aynı Azure Stack ortamı içinde birbirine bağlayabilirsiniz. Yerleşik [sanal ağ geçidi](https://docs.microsoft.com/azure-stack/user/azure-stack-network-differences)kullanılarak Azure Stack sanal ağları bağlamak mümkün değildir. İki Azure Stack sanal ağ arasında bir VPN tüneli oluşturmak için NVA gereçlerini kullanmanız gerekir. Bu makaledeki şablon başvurularında, RRAS yüklenmiş olarak iki Windows Server 2016 VM dağıtılır. İki RRAS sunucusu iki sanal ağ arasında bir S2SVPN Ikev2 tüneli uygulayacak şekilde yapılandırılır. Uygun NSG ve UDR kuralları, **iç**olarak atanan her VNET 'teki alt ağlar arasında yönlendirmeye izin vermek için oluşturulur. 

Bu dağıtım stili, VPN tünellerinin yalnızca bir Azure Stack örneği içinde değil, aynı zamanda Azure Stack örnekleri ve şirket içi ağlar gibi Windows RRAS S2S VPN tünellerini kullanan diğer kaynaklar arasında oluşturulmasını sağlayan bir temel oluşturur. 

Şablonları [Azure akıllı kenar desenleri GitHub](https://github.com/lucidqdreams/azure-intelligent-edge-patterns
) deposunun **lucidqdreams** çatalından bulabilirsiniz. Şablon **S2SVPNTunnel** klasöründedir.

![alternatif metin](./media/azure-stack-network-howto-vnet-peering/overview.png)

## <a name="requirements"></a>Gereksinimler

- En son güncelleştirmeler uygulanmış olan ASDK veya Azure Stack tümleşik sistem. 
- Gerekli Azure Stack Market öğeleri:
    -  Windows Server 2016 Datacenter (en son derleme önerilir)
    -  Özel Betik Uzantısı

## <a name="things-to-consider"></a>Dikkate alınması gereken noktalar

- Bir ağ güvenlik grubu, şablon tüneli alt ağına uygulanır. Her VNet 'te iç alt ağın ek bir NSG ile güvenli hale kullanılması önerilir.
- Bir RDP reddetme kuralı, bir genel IP adresi aracılığıyla sanal makinelere erişmeyi düşünüyorsanız, bu,
- Bu çözüm, hesaba DNS çözümlemesi yapmaz
- VNet adı ve vmName birleşimi 15 karakterden kısa olmalıdır
- Bu şablon, VNet1 ve VNet2 için bir sanal ağ adı özelleştirilmek üzere tasarlanmıştır.
- Bu şablon KLG Windows kullanıyor
- Kaynak grubu silinirken şu anda (1907), DELETE kaynak grubunun tamamlanmasını sağlamak için NSG 'leri tünel alt ağından el ile ayırmanız gerekir
- Bu şablon bir DS3v2 VM kullanıyor. RRAS hizmeti, Windows iç SQL Server yükleyip çalıştırır. VM boyutunuz çok küçük olduğunda bu, bellek sorunlarına neden olabilir. VM boyutunu azaltmadan önce performansı doğrulayın.
- Bu, yüksek oranda kullanılabilir bir çözüm değildir. Daha fazla HA stil çözümüne ihtiyacınız varsa ikinci bir VM 'yi ekleyebilirsiniz, yol tablosundaki yolu ikincil arabirimin iç IP 'si ile el ile değiştirmeniz gerekir. Ayrıca, birden fazla tüneli çapraz bağlantı için yapılandırmanız gerekir.

## <a name="options"></a>Seçenekler

- _ArtifactsLocation ve _artifactsLocationSasToken parametrelerini kullanarak kendi BLOB depolama hesabınızı ve SAS belirtecinizi kullanabilirsiniz
- Bu şablon INTERNALSUBNETREFVNET1 ve INTERNALSUBNETREFVNET2, iç alt ağların kaynak kimlikleri olan, işlem hattı stili dağıtım düzeninde kullanmak istiyorsanız, bu şablonda iki çıkış vardır.

Şablon, VNet adlandırma ve IP adresleme için varsayılan değerleri sağlar. Yönetici (rksadmin) için bir parola gerektirir ve ayrıca, SAS belirteciyle kendi depolama blobunu kullanma olanağı sunar. Dağıtım başarısız olabileceğinden, bu değerleri geçerli aralıklar içinde tutmaya dikkat edin. PowerShell DSC paketi her RRAS sanal makinesinde yürütülür ve Yönlendirme ve tüm gerekli bağımlı hizmetler ve özellikler yüklenir. Bu DSC, gerekirse daha fazla özelleştirilebilir. Özel Betik uzantısı aşağıdaki betiği çalıştırır ve `Add-Site2Site.ps1`, iki RRAS sunucusu arasında paylaşılan bir anahtarla VPNS2S tüneli yapılandırır. VPN tüneli yapılandırmasının sonuçlarını görmek için özel betik uzantısından ayrıntılı çıktıyı görüntüleyebilirsiniz

![alternatif metin](./media/azure-stack-network-howto-vnet-peering/s2svpntunnels2.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  