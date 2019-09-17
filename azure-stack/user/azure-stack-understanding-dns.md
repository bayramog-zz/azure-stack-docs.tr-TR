---
title: Azure Stack 'da IDNs kullanma | Microsoft Docs
description: Azure Stack ' de iDNS özelliklerini ve yeteneklerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: Justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: Justinha
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 24dc5fc3ea57e1a849442fb02c118615bc8b60fa
ms.sourcegitcommit: ca5025fb04250271fe0b2b2df8ad0b3b9ed3e604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71020825"
---
# <a name="use-idns-in-azure-stack"></a>Azure Stack 'da IDNs kullanma 

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

IDNs, dış DNS adlarını çözmenize olanak tanıyan bir Azure Stack ağ özelliğidir (örneğin, https:\//www.Bing.com.) Ayrıca, iç sanal ağ adlarını kaydetmenizi sağlar. Bunu yaparak, aynı sanal ağdaki sanal makineleri (VM) IP adresi yerine adıyla çözebilirsiniz. Bu yaklaşım, özel DNS sunucusu girişleri sağlama gereksinimini ortadan kaldırır. DNS hakkında daha fazla bilgi için bkz. [Azure DNS genel bakış](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>IDNs ne yapar?

Azure Stack, iDNS ile özel DNS sunucusu girişleri belirtmeye gerek kalmadan aşağıdaki özellikleri alırsınız:

- Kiracı iş yükleri için paylaşılan DNS ad çözümleme hizmetleri.
- Kiracı sanal ağı içindeki ad çözümlemesi ve DNS kaydı için yetkili DNS hizmeti.
- Kiracı VM 'lerinden internet adlarının çözümlenmesi için özyinelemeli DNS hizmeti. Kiracıların Internet adlarını çözümlemek için özel DNS girdileri belirtmesi gerekmez (örneğin, www\.Bing.com.)

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

## <a name="demo-of-how-idns-works"></a>IDNs 'in nasıl çalıştığına ilişkin tanıtım

Sanal ağlardaki VM 'Ler için tüm konak adları, aynı bölge altında DNS kaynak kayıtları olarak depolanır. Bununla birlikte, VM 'nin dağıtıldığı SDN altyapısında VNET KIMLIĞIYLE ilişkili bir GUID olarak tanımlanan kendi benzersiz bölmelerini sağlar. Kiracı VM tam etki alanı adları (FQDN), GUID biçiminde, sanal ağın bilgisayar adından ve DNS sonek dizesinden oluşur.

<!--- what does compartment mean? Add a screenshot? can we clarify what we mean by host name and computer name. the description doesn't match the example in the table.--->
 
Bunun nasıl çalıştığını gösteren basit bir laboratuvar aşağıda verilmiştir. Bir sanal ağ üzerinde 3 VM ve ayrı bir sanal ağ üzerinde başka bir sanal makine oluşturduk:

<!--- Is DNS Label the right term? If so, we should define it. The column lists FQDNs, afaik. Where does the domain suffix come from? --->
 
|VM    |Adlı    |Özel IP   |Genel IP    | DNS Etiketi                                |
|------|--------|-------------|-------------|------------------------------------------|
|VM-A1 |Sanalağa   | 10.0.0.5    |172.31.12.68 |VM-a1-Label. lnv1. cloudapp. azscss. External |
|VM-A2 |Sanalağa   | 10.0.0.6    |172.31.12.76 |VM-a2-etiket. lnv1. cloudapp. azscss. External |
|VM-A3 |Sanalağa   | kısmına 10.0.0.7    |172.31.12.49 |VM-a3-etiket. lnv1. cloudapp. azscss. External |
|VM-B1 |Sanalağb   | 10.0.0.4    |172.31.12.57 |VM-B1-Label. lnv1. cloudapp. azscss. External |
 
 
|Sanal ağ  |GUID                                 |DNS son ek dizesi                                                  |
|------|-------------------------------------|-------------------------------------------------------------------|
|Sanalağa |e71e1db5-0a38-460d-8539-705457a4cf75 |e71e1db5-0a38-460D-8539-705457a4cf75. Internal. lnv1. azurestack. Local|
|Sanalağb |e8a6e386-bc7a-43e1-a640-61591b5c76dd |e8a6e386-bc7a-43e1-A640-61591b5c76dd. Internal. lnv1. azurestack. Local|
 
 
IDNs 'in nasıl çalıştığını daha iyi anlamak için bazı ad çözümlemesi testleri yapabilirsiniz:

<!--- why Linux?--->

VM-a1 (Linux VM): VM aranıyor-a2. VNetA için DNS son ekinin eklendiğini ve adının özel IP 'ye çözümlendiğini görebilirsiniz:
 
```console
carlos@VM-A1:~$ nslookup VM-A2
Server:         127.0.0.53
Address:        127.0.0.53#53
 
Non-authoritative answer:
Name:   VM-A2.e71e1db5-0a38-460d-8539-705457a4cf75.internal.lnv1.azurestack.local
Address: 10.0.0.6
```
 
VM 'yi aramak, FQDN 'yi sağlamadan a2 etiketi, beklendiği gibi başarısız olur:

```console 
carlos@VM-A1:~$ nslookup VM-A2-Label
Server:         127.0.0.53
Address:        127.0.0.53#53
 
** server can't find VM-A2-Label: SERVFAIL
```

DNS etiketi için FQDN sağlarsanız, ad genel IP olarak çözümlenir:

```console
carlos@VM-A1:~$ nslookup VM-A2-Label.lnv1.cloudapp.azscss.external
Server:         127.0.0.53
Address:        127.0.0.53#53
 
Non-authoritative answer:
Name:   VM-A2-Label.lnv1.cloudapp.azscss.external
Address: 172.31.12.76
```
 
Bu kayıt bu bölgede mevcut olmadığından, VM çözümlenmeye çalışılıyor-B1 (farklı bir VNet 'ten gelen) başarısız oluyor.

```console
carlos@caalcobi-vm4:~$ nslookup VM-B1
Server:         127.0.0.53
Address:        127.0.0.53#53
 
** server can't find VM-B1: SERVFAIL
```

VM için FQDN 'yi kullanma-B1, bu kayıt farklı bir bölgeden olduğundan yardım etmez.

```console 
carlos@VM-A1:~$ nslookup VM-B1.e8a6e386-bc7a-43e1-a640-61591b5c76dd.internal.lnv1.azurestack.local
Server:         127.0.0.53
Address:        127.0.0.53#53
 
** server can't find VM-B1.e8a6e386-bc7a-43e1-a640-61591b5c76dd.internal.lnv1.azurestack.local: SERVFAIL
```
 
DNS etiketi için FQDN kullanırsanız, başarıyla çözümlenir:

``` 
carlos@VM-A1:~$ nslookup VM-B1-Label.lnv1.cloudapp.azscss.external
Server:         127.0.0.53
Address:        127.0.0.53#53
 
Non-authoritative answer:
Name:   VM-B1-Label.lnv1.cloudapp.azscss.external
Address: 172.31.12.57
```
 
VM-a3 (Windows VM). Yetkili ve yetkili olmayan yanıtlar arasındaki farka dikkat edin.

İç kayıtlar:

```console
C:\Users\carlos>nslookup
Default Server:  UnKnown
Address:  168.63.129.16
 
> VM-A2
Server:  UnKnown
Address:  168.63.129.16
 
Name:    VM-A2.e71e1db5-0a38-460d-8539-705457ª4cf75.internal.lnv1.azurestack.local
Address:  10.0.0.6
```

Dış kayıtlar:

```console
> VM-A2-Label.lnv1.cloudapp.azscss.external
Server:  UnKnown
Address:  168.63.129.16
 
Non-authoritative answer:
Name:    VM-A2-Label.lnv1.cloudapp.azscss.external
Address:  172.31.12.76
``` 
 
Kısacası, yukarıdaki ' dan daha fazla bilgi alabilirsiniz:
 
*   Her VNet 'in, VM adından ve VNet 'in DNS sonekini (GUID 'SI) içeren, tüm özel IP adresleri için bir kayıt içeren kendi bölgesi vardır.
    *   \<VMName >. vnetguid\>. Internal.\< \< Bölge >. \<stackınternalfqdn >
    *   Bu otomatik olarak yapılır
*   Ortak IP adresleri kullanırsanız, bunlar için DNS etiketleri de oluşturabilirsiniz. Bunlar diğer dış adresler gibi çözümlenir.
 
 
- IDNs sunucuları, iç DNS bölgeleri için yetkili sunuculardır ve Kiracı VM 'Leri dış kaynaklara bağlanmayı denediklerinde ortak adlara yönelik bir çözümleyici olarak da davranır. Dış kaynak için bir sorgu varsa, IDNs sunucuları, isteği çözümlemek üzere yetkili DNS sunucularına iletir.
 
Laboratuvar sonuçlarından görebileceğiniz gibi, hangi IP 'nin kullanıldığını kontrol edebilirsiniz. VM adını kullanırsanız, özel IP adresini alırsınız ve DNS etiketini kullanırsanız genel IP adresini alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS kullanımı](azure-stack-dns.md)
