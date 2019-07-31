---
title: Azure Stack 'de DNS | Microsoft Docs
description: Azure Stack ve DNS bölgelerini oluşturma ve yönetme hakkında bilgi edinin.
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
ms.date: 06/05/2019
ms.author: sethm
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 8bfe15ad19e4aaec45492aa98cfb2ef02294742a
ms.sourcegitcommit: b3dac698f2e1834491c2f9af56a80e95654f11f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658485"
---
# <a name="use-dns-in-azure-stack"></a>Azure Stack 'de DNS kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack aşağıdaki Azure DNS özelliklerini destekler:

* DNS ana bilgisayar adı çözümlemesi.
* API kullanarak DNS bölgeleri ve kayıtları oluşturun ve yönetin.

## <a name="support-for-dns-hostname-resolution"></a>DNS konak adı çözümlemesi desteği

Genel IP kaynakları için bir DNS etki alanı adı etiketi belirtebilirsiniz. Azure Stack, etiket adı için **domainnamelabel. Location. cloudapp. azurestack. external** kullanır ve bunu Azure Stack tarafından yönetilen DNS SUNUCULARıNDAKI genel IP adresiyle eşler.

Örneğin, yerel Azure Stack konumunda bir etki alanı adı etiketi olarak **contoso** ile BIR genel IP kaynağı oluşturursanız, **contoso. Local. cloudapp. azurestack. external** [tam etki alanı adı (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) genel IP 'ye çözümlenmelidir kaynağın adresi. Bu FQDN 'yi, Azure Stack genel IP adresini işaret eden özel bir etki alanı CNAME kaydı oluşturmak için kullanabilirsiniz.

Ad çözümlemesi hakkında daha fazla bilgi için bkz. [DNS çözümleme](/azure/dns/dns-for-azure-services?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) makalesi.

> [!IMPORTANT]
> Oluşturduğunuz her etki alanı adı etiketinin Azure Stack konumu içinde benzersiz olması gerekir.

Aşağıdaki ekran görüntüsünde, portalı kullanarak genel IP adresi oluşturmak için **genel IP adresi oluştur** iletişim kutusu gösterilmektedir:

![Ortak IP adresi oluştur](media/azure-stack-dns/image01.png)

### <a name="example-scenario"></a>Örnek senaryo

Bir Web uygulamasından bir yük dengeleyici işleme istekleriniz var. Yük dengeleyicinin arkasında bir veya daha fazla sanal makinede çalışan bir Web sitesi vardır. Yük dengeli Web sitesine bir IP adresi yerine bir DNS adı kullanarak erişebilirsiniz.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>API kullanarak DNS bölgeleri ve kayıtları oluşturma ve yönetme

Azure Stack, DNS bölgeleri ve kayıtları oluşturabilir ve yönetebilirsiniz.

Azure Stack, Azure DNS API 'Leriyle tutarlı API 'Ler kullanarak Azure 'a benzer bir DNS hizmeti sağlar.  Etki alanlarınızı Azure Stack DNS 'de barındırarak DNS kayıtlarınızı aynı kimlik bilgilerini, API 'Leri ve araçları kullanarak yönetebilirsiniz. Aynı faturalandırma ve desteği diğer Azure hizmetlerinize göre de kullanabilirsiniz.

Azure Stack DNS altyapısı, Azure 'dan daha küçük. Bir Azure Stack dağıtımının boyutu ve konumu, DNS kapsamını, ölçeği ve performansı etkiler. Bu Ayrıca, performans, kullanılabilirlik, genel dağıtım ve yüksek kullanılabilirlik dağıtımının dağıtıma değişebileceği anlamına gelir.

## <a name="comparison-with-azure-dns"></a>Azure DNS ile karşılaştırma

Azure Stack DNS, Azure 'daki DNS ile benzerdir, ancak bazı önemli özel durumlar vardır:

* **Aaaa kayıtlarını desteklemez**: Azure Stack IPv6 adreslerini desteklemediğinden Azure Stack AAAA kayıtlarını desteklemez. Bu, Azure 'daki DNS ve Azure Stack arasındaki önemli farktır.

* **Çok kiracılı değil**: Azure Stack 'deki DNS hizmeti çok kiracılı değildir. Kiracılar aynı DNS bölgesini oluşturamaz. Yalnızca bölgeyi oluşturmaya çalışır olan ilk abonelik başarılı olur ve daha sonra istekler başarısız olur. Bu, Azure ile Azure Stack DNS arasındaki başka bir temel farktır.

* **Etiketler, meta veriler ve eetiketler**: Azure Stack Etiketler, meta veriler, Eetiketler ve limitleri nasıl işleyeceğinden küçük farklılıklar vardır.

Azure DNS hakkında daha fazla bilgi edinmek için bkz. [DNS bölgeleri ve kayıtları](/azure/dns/dns-zones-records).

### <a name="tags"></a>Tags

DNS Azure Stack DNS bölge kaynaklarında Azure Resource Manager etiketlerin kullanılmasını destekler. DNS kayıt kümelerinde etiketleri desteklemez. Alternatif olarak, sonraki bölümde açıklandığı gibi DNS kayıt kümelerinde **meta veriler** desteklenir.

### <a name="metadata"></a>Meta Veriler

Kayıt kümesi etiketlerinin bir alternatifi olarak Azure Stack DNS, *meta verileri*kullanarak kayıt kümelerine açıklama eklemeyi destekler. Etiketlere benzer şekilde meta veriler, ad-değer çiftlerini her bir kayıt kümesiyle ilişkilendirmenizi sağlar. Örneğin, meta veriler her bir kayıt kümesinin amacını kaydetmek için yararlıdır. Etiketlerin aksine, Azure faturanızda filtrelenmiş bir görünüm sağlamak için meta verileri kullanamazsınız ve meta veriler Azure Resource Manager ilkesinde belirtilemez.

### <a name="etags"></a>ETag 'ler

İki kişinin veya iki işlemin aynı anda bir DNS kaydını değiştirmeyi deneydiğini varsayalım. Tek bir WINS mi? Ayrıca, başka biri tarafından oluşturulan değişikliklerin üzerine yazıldıklarından emin misiniz?

Azure Stack DNS, aynı kaynakta yapılan eşzamanlı değişiklikleri güvenle işlemek için *ETags* kullanır. ETags Azure Resource Manager *etiketlerden*farklıdır. Her DNS kaynağına (bölge veya kayıt kümesi) ilişkili bir ETag öğesi vardır. Bir kaynak alındığında ETag de alınır. Bir kaynağı güncelleştirdiğinizde, DNS Azure Stack sunucudaki ETag 'in eşleştirdiğini doğrulayabilmesi için ETag 'i geri geçirmeye seçebilirsiniz. Bir kaynaktaki her güncelleştirme ETag 'in yeniden üretildiğinden dolayı bir ETag uyumsuzluğu, eşzamanlı bir değişikliğin gerçekleştiğini gösterir. Kaynak zaten mevcut olmadığından emin olmak için yeni bir kaynak oluşturduğunuzda ETags de kullanılabilir.

Varsayılan olarak, Azure Stack DNS PowerShell cmdlet 'leri bölge ve kayıt kümelerinde eşzamanlı değişiklikleri engellemek için ETags kullanır. ETag denetimlerini gizlemek için isteğe `-Overwrite` bağlı anahtarı kullanabilirsiniz. Hiçbir ETag denetimi olmadan, gerçekleşen tüm eşzamanlı değişiklikler üzerine yazılır.

Azure Stack DNS REST API düzeyinde ETags, HTTP üst bilgileri kullanılarak belirtilir. Davranışları aşağıdaki tabloda açıklanmıştır:

| Üstbilgi | Davranış|
|--------|---------|
| Yok.   | PUT her zaman başarılı olur (ETag denetimi yok).|
| IF-Match| PUT yalnızca kaynak varsa ve ETag eşleşiyorsa başarılı olur.|
| IF-Match *| PUT yalnızca kaynak mevcutsa başarılı olur.|
| If-None-Match *| PUT yalnızca kaynak yoksa başarılı olur.|

### <a name="limits"></a>Sınırlar

Azure Stack DNS kullanılırken aşağıdaki varsayılan sınırlar geçerlidir:

| Resource| Varsayılan limit|
|---------|--------------|
| Abonelik başına bölge| 100|
| Her bölge için kayıt kümesi| 5000|
| Kayıt kümesi başına kayıt sayısı| 20|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için iDNS ile tanışın](azure-stack-understanding-dns.md)
