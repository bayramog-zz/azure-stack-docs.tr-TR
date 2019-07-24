---
title: Azure Stack veri merkezi tümleştirmesi-DNS
description: Azure Stack DNS 'yi veri merkezi DNS ile tümleştirmeyi öğrenin
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 05/09/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 05/09/2019
keywords: ''
ms.openlocfilehash: 748da2aa4391d7f28e6d4273830d8d024021bb79
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68417493"
---
# <a name="azure-stack-datacenter-integration---dns"></a>Azure Stack veri merkezi tümleştirmesi-DNS

Azure Stack uç noktalarına erişebilmek için (**Portal**, **adminportal**, **Yönetim**, **adminmanagement**vb.)  Azure Stack dışından, Azure Stack DNS hizmetlerini Azure Stack 'de kullanmak istediğiniz DNS bölgelerini barındıran DNS sunucularıyla tümleştirmeniz gerekir.

## <a name="azure-stack-dns-namespace"></a>Azure Stack DNS ad alanı

Azure Stack dağıtırken DNS ile ilgili bazı önemli bilgileri sağlamanız gerekir.


|Alan  |Açıklama  |Örnek|
|---------|---------|---------|
|Bölge|Azure Stack dağıtımınızın coğrafi konumu.|`east`|
|Dış etki alanı adı|Azure Stack dağıtımınız için kullanmak istediğiniz bölgenin adı.|`cloud.fabrikam.com`|
|İç etki alanı adı|Azure Stack içinde altyapı hizmetleri için kullanılan iç bölgenin adı.  Dizin hizmeti ile tümleşik ve özel (Azure Stack dağıtımının dışından ulaşılamaz).|`azurestack.local`|
|DNS Ileticisi|Şirket intranetinde veya genel İnternet 'te Azure Stack dışında barındırılan DNS sorguları, DNS bölgeleri ve kayıtları iletmek için kullanılan DNS sunucuları.|`10.57.175.34`<br>`8.8.8.8`|
|Adlandırma ön eki (Isteğe bağlı)|Azure Stack altyapı rolü örneği makine adlarınızın sahip olmasını istediğiniz adlandırma ön eki.  Sağlanmazsa, varsayılan olur `azs`.|`azs`|

Azure Stack dağıtımınızın ve uç noktalarınızın tam etki alanı adı (FQDN), Region parametresinin ve dış etki alanı adı parametresinin birleşimidir. Önceki tablodaki örneklerden değerler kullanılarak bu Azure Stack dağıtımı için FQDN aşağıdaki ad olacaktır:

`east.cloud.fabrikam.com`

Bu nedenle, bu dağıtımın bazı uç noktaları örnekleri aşağıdaki URL 'Ler gibi görünür:

`https://portal.east.cloud.fabrikam.com`

`https://adminportal.east.cloud.fabrikam.com`

Azure Stack dağıtımı için bu örnek DNS ad alanını kullanmak için aşağıdaki koşullar gereklidir:

- Bölge `fabrikam.com` , ad çözümleme gereksinimlerinize bağlı olarak bir etki alanı kaydedicisi, dahili bir kurumsal DNS sunucusu veya her ikisiyle kaydedilir.
- Alt etki alanı `cloud.fabrikam.com` bölge `fabrikam.com`altında bulunur.
- Bölgeleri `fabrikam.com` barındıran DNS sunucularına ve `cloud.fabrikam.com` Azure Stack dağıtımından erişilebilir.

Azure Stack dışından Azure Stack uç noktaları ve örneklerin DNS adlarını çözümleyebilmek için, Azure Stack için dış DNS bölgesini barındıran DNS sunucularını kullanmak istediğiniz üst bölgeyi barındıran DNS sunucularıyla tümleştirmeniz gerekir.

### <a name="dns-name-labels"></a>DNS adı etiketleri

Azure Stack, genel IP adresleri için ad çözümlemesine izin vermek üzere genel IP adresine bir DNS ad etiketi eklenmesini destekler. Bu, kullanıcıların ada göre Azure Stack ' de barındırılan uygulama ve hizmetlere ulaşabilmesi için kullanışlı bir yoldur. DNS ad etiketi altyapı uç noktalarından biraz farklı bir ad alanı kullanır. Önceki örnek ad alanından sonra DNS adı etiketlerinin ad alanı şu şekilde görünür:

`*.east.cloudapp.cloud.fabrikam.com`

Bu nedenle, bir kiracı bir genel IP adresi kaynağının DNS adı etiketi alanında **MyApp** değerini gösteriyorsa, Azure Stack dış DNS sunucusundaki **East.cloudapp.Cloud.fabrikam.com** bölgesinde **MyApp** için bir kayıt oluşturur. Sonuç olarak tam etki alanı adı aşağıdaki gibi görünür:

`myapp.east.cloudapp.cloud.fabrikam.com`

Bu işlevden yararlanmak ve bu ad alanını kullanmak istiyorsanız, Azure Stack için dış DNS bölgesini barındıran DNS sunucularını da kullanmak istediğiniz üst bölgeyi barındıran DNS sunucularıyla tümleştirmeniz gerekir. Bu, Azure Stack hizmeti uç noktaları için ad alanından farklı bir ad alanıdır, bu nedenle ek bir temsili veya koşullu iletme kuralı oluşturmanız gerekir.

DNS adı etiketinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Stack DNS kullanma](../user/azure-stack-dns.md).

## <a name="resolution-and-delegation"></a>Çözümleme ve temsilci seçme

İki tür DNS sunucusu bulunur:

- Yetkili bir DNS sunucusu DNS bölgelerini barındırır. Bu sunucu, yalnızca bu bölgelerdeki kayıtlar için DNS sorgularını yanıtlar.
- Özyinelemeli bir DNS sunucusu DNS bölgelerini barındırmaz. Bu sunucu, tüm DNS sorgularını yanıtlamak için yetkili DNS sunucularını çağırarak ihtiyacı olan verileri toplar.

Azure Stack hem yetkili hem de özyinelemeli DNS sunucularını içerir. Özyinelemeli sunucular, iç özel bölge ve bu Azure Stack dağıtımı için dış genel DNS bölgesi dışındaki her şeyin adını çözümlemek için kullanılır. 

![Azure Stack DNS mimarisi](media/azure-stack-integrate-dns/Integrate-DNS-01.png)

## <a name="resolving-external-dns-names-from-azure-stack"></a>Azure Stack dış DNS adlarını çözümleme

Azure Stack dışındaki uç noktalar için DNS adlarını çözümlemek için (örneğin: www\.Bing.com), Azure Stack Azure Stack yetkili olmadığı DNS isteklerini iletmek için kullanabileceği DNS sunucuları sağlamanız gerekir. Dağıtım için, istekleri ileten DNS Azure Stack sunucuları dağıtım çalışma sayfasında (DNS Ileticisi alanında) gereklidir. Bu alanda hata toleransı için en az iki sunucu sağlayın. Bu değerler olmadan Azure Stack dağıtım başarısız olur.

### <a name="configure-conditional-dns-forwarding"></a>Koşullu DNS iletmeyi yapılandırma

> [!IMPORTANT]
> Bu yalnızca bir AD FS dağıtımı için geçerlidir.

Mevcut DNS altyapınızla ad çözümlemesini etkinleştirmek için koşullu iletme yapılandırın.

Koşullu iletici eklemek için ayrıcalıklı uç noktasını kullanmanız gerekir.

Bu yordamda, veri merkezi ağınızda Azure Stack ayrıcalıklı uç noktayla iletişim kurabilen bir bilgisayar kullanın.

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır) ve ayrıcalıklı uç noktanın IP adresine bağlanın. CloudAdmin kimlik doğrulaması için kimlik bilgilerini kullanın.

   ```
   $cred=Get-Credential 
   Enter-PSSession -ComputerName <IP Address of ERCS> -ConfigurationName PrivilegedEndpoint -Credential $cred
   ```

2. Ayrıcalıklı uç noktaya bağlandıktan sonra aşağıdaki PowerShell komutunu çalıştırın. Kullanmak istediğiniz DNS sunucularının etki alanı adınız ve IP adresleriyle birlikte sunulan örnek değerleri değiştirin.

   ```
   Register-CustomDnsServer -CustomDomainName "contoso.com" -CustomDnsIPAddresses "192.168.1.1","192.168.1.2"
   ```

## <a name="resolving-azure-stack-dns-names-from-outside-azure-stack"></a>Azure Stack DNS adlarını dış Azure Stack çözümleme
Yetkili sunucular, dış DNS bölge bilgilerini ve Kullanıcı tarafından oluşturulan tüm bölgeleri tutan olanlardır. Bölge temsilcisini veya koşullu yönlendirmeyi etkinleştirmek için bu sunucularla tümleştirin Azure Stack dışından Azure Stack DNS adlarını çözün.

## <a name="get-dns-server-external-endpoint-information"></a>DNS sunucusu dış uç nokta bilgilerini al

Azure Stack dağıtımınızı DNS altyapınızla tümleştirmek için aşağıdaki bilgilere ihtiyacınız vardır:

- DNS sunucusu FQDN 'leri
- DNS sunucusu IP adresleri

Azure Stack DNS sunucuları için FQDN 'Ler aşağıdaki biçimdedir:

`<NAMINGPREFIX>-ns01.<REGION>.<EXTERNALDOMAINNAME>`

`<NAMINGPREFIX>-ns02.<REGION>.<EXTERNALDOMAINNAME>`

Örnek değerleri kullanarak DNS sunucuları için FQDN 'Ler şunlardır:

`azs-ns01.east.cloud.fabrikam.com`

`azs-ns02.east.cloud.fabrikam.com`


Bu bilgiler, adlı `AzureStackStampInformation.json`bir dosyadaki tüm Azure Stack dağıtımlarının sonunda da oluşturulur. Bu dosya, dağıtım sanal makinesinin `C:\CloudDeployment\logs` klasöründe bulunur. Azure Stack dağıtımınız için hangi değerlerin kullanıldığından emin değilseniz, değerleri buradan edinebilirsiniz.

Dağıtım sanal makinesi artık kullanılabilir değilse veya erişilebilir durumda değilse, ayrıcalıklı uç noktaya bağlanarak ve `Get-AzureStackStampInformation` PowerShell cmdlet 'ini çalıştırarak değerleri elde edebilirsiniz. Daha fazla bilgi için bkz. [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md).

## <a name="setting-up-conditional-forwarding-to-azure-stack"></a>Azure Stack koşullu iletme ayarlanıyor

DNS altyapınızla Azure Stack tümleştirme yapmanın en kolay ve en güvenli yolu, bölgenin üst bölgeyi barındıran sunucudan koşullu olarak iletilmesidir. Azure Stack dış DNS ad alanınız için üst bölgeyi barındıran DNS sunucuları üzerinde doğrudan denetiminiz varsa bu yaklaşım önerilir.

DNS ile koşullu iletme yapma hakkında bilgi sahibi değilseniz, aşağıdaki TechNet makalesine bakın: [Bir etki alanı adı veya DNS çözümünüze özgü belgeler için bir koşullu Iletici atayın](https://technet.microsoft.com/library/cc794735).

Dış Azure Stack DNS bölgenizin şirket etki alanı adınızın alt etki alanı gibi görünmesini istediğiniz senaryolarda koşullu iletme kullanılamaz. DNS temsilcisinin yapılandırılması gerekir.

Örnek:

- Şirket DNS etki alanı adı:`contoso.com`
- Azure Stack dış DNS etki alanı adı:`azurestack.contoso.com`

## <a name="delegating-the-external-dns-zone-to-azure-stack"></a>Dış DNS bölgesinin yetkisini Azure Stack

Azure Stack dağıtımı dışında çözümlenebilmeniz için DNS adları için, DNS temsilcisini ayarlamanız gerekir.

Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt sahibinin DNS yönetimi sayfasında, NS kayıtlarını düzenleyin ve bölgeye ait NS kayıtlarını Azure Stack sahip olanlarla değiştirin.

Çoğu DNS kayıt şirketlerinde, temsilciyi tamamlaması için en az iki DNS sunucusu sağlamanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Duvarı tümleştirmesi](azure-stack-firewall.md)
