---
title: Azure Stack 'de GRE kullanarak VPN tüneli oluşturma | Microsoft Docs
description: Azure Stack içinde GRE kullanarak bir VPN tüneli oluşturmayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: c9c599a7547b2000eb146a7e3b7783ba057d23b3
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167701"
---
# <a name="how-to-create-a-vpn-tunnel-using-gre-in-azure-stack"></a>Azure Stack içinde GRE kullanarak VPN tüneli oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Aynı Azure Stack ortamı içinde iki Azure Stack sanal ağı bağlamak için bu çözümdeki Azure Stack Kaynak Yöneticisi şablonunu kullanabilirsiniz. Yerleşik sanal ağ geçidini kullanarak [vnet Azure Stack bağlanamazsınız](https://docs.microsoft.com/azure-stack/user/azure-stack-network-differences) . Şimdilik iki Azure Stack VNET arasında bir VPN tüneli oluşturmak için ağ sanal gereçlerini (NVA) kullanmanız gerekir. Çözüm şablonu RRAS yüklü iki Windows Server 2016 VM dağıtır. Çözüm iki RRAS sunucusunu iki sanal ağ arasında bir S2SVPN Ikev2 tüneli kullanacak şekilde yapılandırır. Her bir sanal ağ üzerindeki alt ağlar arasında **iç** yönlendirmeye izin vermek için uygun NSG ve UDR kuralları oluşturulur 

Bu dağıtım stili, yalnızca Azure Stack Örneğinizde değil, aynı zamanda Azure Stack örnekleri ve diğer kaynaklar arasında (örneğin, Windows RRAS S2S VPN kullanımı ile şirket içi ağlarınız gibi) VPN tünelleri oluşturmanıza imkan tanıyan temel bir temelidir Lerden.

Şablonları [Azure akıllı kenar desenleri](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) GitHub deposunda bulabilirsiniz. Şablon **RRAS-GRE-VNET-VNET** klasöründedir. 

![alternatif metin](./media/azure-stack-network-howto-vpn-tunnel-gre/overview.png)

## <a name="requirements"></a>Gereksinimler

- En son güncelleştirmeler uygulanmış olan ASDK veya Azure Stack tümleşik sistem. 
- Gerekli Azure Stack Market öğeleri:
    -  Windows Server 2016 Datacenter (en son derleme önerilir)
    -  Özel Betik Uzantısı

## <a name="things-to-consider"></a>Dikkate alınması gereken noktalar

- Bir ağ güvenlik grubu, şablon tüneli alt ağına uygulanır. Ek NSG ile her VNet 'teki iç alt ağın güvenliğini sağlayın.
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

Bu şablon, VNet adlandırma ve IP adresleme için varsayılan değerleri sağlar. Yönetici (rksadmin) için bir parola gerektirir ve ayrıca, SAS belirteciyle kendi depolama blobunu kullanma olanağı sunar. Dağıtım başarısız olabileceğinden, bu değerleri geçerli aralıklar içinde tutmaya dikkat edin. PowerShell DSC paketi her RRAS sanal makinesinde yürütülür ve Yönlendirme ve tüm gerekli bağımlı hizmetler ve özellikler yüklenir. Bu DSC, gerekirse daha fazla özelleştirilebilir. Özel Betik uzantısı aşağıdaki betiği çalıştırın ve Add-Site2SiteGRE. ps1, paylaşılan bir anahtarla iki RRAS sunucusu arasında VPNS2S tünelini yapılandırır. VPN tüneli yapılandırmasının sonuçlarını görmek için özel betik uzantısından ayrıntılı çıktıyı görüntüleyebilirsiniz

![alternatif metin](./media/azure-stack-network-howto-vpn-tunnel-gre/s2svpntunnel.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  
[Birden çok siteden siteye VPN tüneli ayarlama](network-howto-vpn-tunnel.md)