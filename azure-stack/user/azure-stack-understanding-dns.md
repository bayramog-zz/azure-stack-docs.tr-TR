---
title: Azure Stack 'da IDNs kullanma | Microsoft Docs
description: Azure Stack ' de iDNS özelliklerini ve yeteneklerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: e8a1e40ec5b333862eaca59d7269a46a91460237
ms.sourcegitcommit: 72d45bb935db0db172d4d7c37d8e48e79e25af64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68376787"
---
# <a name="use-idns-in-azure-stack"></a>Azure Stack 'da IDNs kullanma 

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

IDNs, dış DNS adlarını çözmenize olanak tanıyan bir Azure Stack ağ özelliğidir (örneğin, https:\//www.Bing.com.) Ayrıca, iç sanal ağ adlarını kaydetmenizi sağlar. Bunu yaparak, aynı sanal ağdaki sanal makineleri (VM) IP adresi yerine adıyla çözebilirsiniz. Bu yaklaşım, özel DNS sunucusu girişleri sağlama gereksinimini ortadan kaldırır. DNS hakkında daha fazla bilgi için bkz. [Azure DNS genel bakış](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>IDNs ne yapar?

Azure Stack, iDNS ile özel DNS sunucusu girişleri belirtmeye gerek kalmadan aşağıdaki özellikleri alırsınız:

- Kiracı iş yükleri için paylaşılan DNS ad çözümleme hizmetleri.
- Kiracı sanal ağı içindeki ad çözümlemesi ve DNS kaydı için yetkili DNS hizmeti.
- Kiracı VM 'lerinden internet adlarının çözümlenmesi için özyinelemeli DNS hizmeti. Kiracıların Internet adlarını çözümlemek için özel DNS girdileri belirtmesi gerekmez (örneğin, www.bing.com.)

Yine de kendi DNS 'nizi alabilir ve özel DNS sunucuları kullanabilirsiniz. Ancak, IDNs kullanarak, özel DNS girişleri oluşturmaya gerek kalmadan internet DNS adlarını çözümleyebilir ve aynı sanal ağdaki diğer VM 'lere bağlanabilirsiniz.

## <a name="what-doesnt-idns-do"></a>IDNs ne yapmaz?

IDNs, sanal ağ dışından çözülemeyen bir ad için DNS kaydı oluşturmanıza izin vermez.

Azure 'da, bir genel IP adresiyle ilişkili bir DNS ad etiketi belirtme seçeneğiniz vardır. Etiketi (ön ek) seçebilirsiniz, ancak Azure, genel IP adresini oluşturduğunuz bölgeyi temel alan son eki seçer.

![DNS ad etiketi örneği](media/azure-stack-understanding-dns-in-tp2/image3.png)

Önceki görüntüde gösterildiği gibi Azure, **westus.cloudapp.Azure.com**bölgesi altında belirtilen DNS ad etıketı için DNS 'de bir "A" kaydı oluşturacaktır. Önek ve sonek, genel İnternet 'te herhangi bir yerden çözümlenelebilecek bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) oluşturmak için birleştirilir.

Azure Stack yalnızca iç ad kaydı için IDNs 'yi destekler, bu nedenle aşağıdaki işlemleri yapamıyor:

- Var olan bir barındırılan DNS bölgesi (örneğin, Local. azurestack. external) altında bir DNS kaydı oluşturun.
- DNS bölgesi oluşturun (örneğin, Contoso.com.)
- Kendi özel DNS bölgeniz altında bir kayıt oluşturun.
- Etki alanı adları satın almayı destekler.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS kullanımı](azure-stack-dns.md)
