---
title: Azure Stack VM 'lere giriş | Microsoft Docs
description: Azure Stack VM 'Ler hakkında bilgi edinin.
services: azure-stack
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: de96b74351fa3becd0b066da4430e42cb2a9cea7
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961673"
---
# <a name="introduction-to-azure-stack-vms"></a>Azure Stack VM 'lerine giriş

*Için geçerli: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti @ no__t-0

Azure Stack, isteğe bağlı ve ölçeklenebilir bilgi işlem kaynağı türü olarak sanal makineler (VM) sağlar. Bilgi işlem ortamı üzerinde daha fazla denetime ihtiyacınız varsa, bir VM seçebilirsiniz. Bu makalede, ilk VM 'nizi oluşturmadan önce ayrıntılar sağlanmaktadır.

Azure Stack VM, kümeleri veya ayrı makineleri yönetmeye gerek kalmadan sanallaştırma esnekliği sağlar. Ancak, üzerinde çalışan yazılımı yapılandırma, düzeltme eki uygulama ve yükleme gibi görevleri gerçekleştirerek yine de VM 'yi korumanız gerekir.

Azure Stack VM 'Leri çeşitli yollarla kullanabilirsiniz. Örneğin:

- **Geliştirme ve test**: Azure Stack VM 'Ler, bir uygulamayı kod ve test etmek için belirli bir yapılandırmaya sahip bir bilgisayar oluşturmanızı sağlar.

- **Buluttaki uygulamalar**: Uygulamanız için talepler dalgalanabileceğinden, bunu Azure Stack sanal makinede çalıştırmak ekonomik bir anlam verebilir. İhtiyacınız olduğunda ek VM 'Ler için ödeme yaparsınız ve bunu yapmazsanız kapatıyor olursunuz.

- **Genişletilmiş veri merkezi**: Azure Stack bir sanal ağdaki VM 'Ler kuruluşunuzun ağına veya Azure 'a bağlanabilir.

Uygulamanızın kullandığı VM 'Ler, gereksinimlerinizi karşılamak için gerekli olan herhangi bir şekilde ölçeklenebilen veya ölçeklendirebilir.

## <a name="before-creating-a-vm"></a>VM oluşturmadan önce

Azure Stack içinde bir uygulama altyapısı oluştururken her zaman tasarım konuları vardır. Altyapınızı oluşturmaya başlamadan önce bir VM 'nin bu yönleri hakkında düşünmek önemlidir:

- Uygulama kaynaklarınızın adları.
- VM 'nin boyutu.
- Oluşturulabilen en fazla sanal makine sayısı.
- VM 'nin çalıştırdığı işletim sistemi.
- VM 'nin başladıktan sonra yapılandırması.
- VM 'nin ihtiyaç duyacağı ilgili kaynaklar.

### <a name="naming"></a>Adlandırma

Bir VM 'ye atanan bir adı vardır ve işletim sisteminin bir parçası olarak yapılandırılmış bir bilgisayar adı vardır. VM adı en fazla 15 karakter uzunluğunda olabilir.

İşletim sistemi diskini oluşturmak için Azure Stack kullanıyorsanız, bilgisayar adı ve VM adı aynıdır. Daha önce yapılandırılmış bir işletim sistemini içeren kendi görüntünüzü yükler ve kullanıyorsanız, bu adlar farklı olabilir. Kendi resim dosyanızı karşıya yüklediğinizde, en iyi uygulama olarak, işletim sistemindeki bilgisayar adının VM adıyla eşleştiğinden emin olun.

### <a name="vm-size"></a>VM boyutu

Kullandığınız sanal makinenin boyutu, çalıştırmak istediğiniz iş yüküne göre belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure Stack birçok tür kullanımı desteklemek için farklı boyutlarda türler sunmaktadır.

### <a name="vm-limits"></a>VM sınırları

Aboneliğiniz, projeniz için VM 'lerin dağıtımını etkileyebilecek varsayılan kota sınırlarına sahiptir. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri

VM 'Ler işletim sistemlerini (OS) ve verilerini depolamak için sanal sabit diskleri (VHD) kullanır. VHD 'ler, bir işletim sistemi yüklemek için seçtiğiniz görüntüler için de kullanılır. Azure Stack, çeşitli sürümler ve işletim sistemi türleriyle birlikte kullanılacak bir market sağlar. Market görüntüleri, görüntü yayımcısı, teklif, SKU ve sürüm tarafından tanımlanır (genellikle en son sürüm **en son**sürüm olarak belirtilir).

Aşağıdaki tabloda, bir görüntü için bilgilerin nasıl bulunacağı gösterilmektedir:

|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Bir görüntüyü kullanmak istediğinizde değerler otomatik olarak belirtilir.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST API'leri     |[Görüntü yayımcılarını listeleme](/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Görüntü tekliflerini listeleme](/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Liste resmi SKU 'Ları](/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

Kendi görüntünüzü karşıya yüklemeyi ve kullanmayı tercih edebilirsiniz. Bunu yaparsanız yayımcı adı, teklif ve SKU kullanılmaz.

### <a name="extensions"></a>Uzantılar

VM uzantıları, dağıtım sonrası yapılandırma ve otomatikleştirilmiş görevler aracılığıyla sanal makinenize ek yetenekler sağlar.
Uzantıları kullanarak şu genel görevleri gerçekleştirebilirsiniz:

- **Özel komut dosyalarını çalıştır**: Özel Betik uzantısı, VM sağlandığında komut dosyanızı çalıştırarak VM 'deki iş yüklerini yapılandırmanıza yardımcı olur.

- **Yapılandırma dağıtma ve yönetme**: PowerShell Istenen durum yapılandırması (DSC) uzantısı, yapılandırmaları ve ortamları yönetmek için bir VM 'de DSC ayarlamanıza yardımcı olur.

- **Tanılama verilerini topla**: Azure Tanılama uzantısı, sanal makineyi uygulamanızın durumunu izlemek için kullanılabilecek tanılama verilerini toplayacak şekilde yapılandırmanıza yardımcı olur.

### <a name="related-resources"></a>İlgili kaynaklar

Aşağıdaki tablodaki kaynaklar VM tarafından kullanılır ve VM oluşturulduğunda bulunmalı veya oluşturulması gerekir:

|Resource|Gerekli|Açıklama|
|---------|---------|---------|
|Resource group|Evet|VM bir kaynak grubunda yer almalıdır.|
|Depolama hesabı|Hayır|Yönetilen diskler kullanılıyorsa VM 'nin sanal sabit disklerini depolaması için depolama hesabının olması gerekmez. <br>Yönetilmeyen diskler kullanılıyorsa VM 'nin sanal sabit disklerini depolaması için depolama hesabının olması gerekir.|
|Sanal ağ|Evet|VM’in bir sanal ağa üye olması gerekir.|
|Ortak IP adresi|Hayır|VM, uzaktan erişim için atanmış bir genel IP adresine sahip olabilir.|
|Ağ arabirimi|Evet|VM’in ağda iletişim kurabilmek için ağ arabirimine ihtiyacı vardır.|
|Veri diskleri|Hayır|VM, depolama olanaklarını genişletmek için veri disklerine sahip olabilir.|

## <a name="create-your-first-vm"></a>İlk sanal makineyi oluşturma

VM oluşturmak için çeşitli seçenekleriniz vardır. Seçiminiz ortamınıza bağlıdır. Aşağıdaki tabloda, VM 'nizi oluşturmaya başlamanıza yardımcı olacak bilgiler verilmektedir:

|Yöntem|Makale|
|---------|---------|
|Azure Stack portalı|Azure Stack portalı ile Windows VM oluşturma<br>[Azure Stack portalını kullanarak bir Linux VM oluşturma](azure-stack-quick-linux-portal.md)|
|Şablonlar|Azure Stack hızlı başlangıç şablonları şurada bulunur:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStarvirtualt-Templates)|
|PowerShell|[Azure Stack 'de PowerShell kullanarak Windows VM oluşturma](azure-stack-quick-create-vm-windows-powershell.md)<br>[Azure Stack 'de PowerShell kullanarak bir Linux VM oluşturma](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Azure Stack 'de CLı kullanarak Windows VM oluşturma](azure-stack-quick-create-vm-windows-cli.md)<br>[Azure Stack 'de CLı kullanarak bir Linux VM oluşturma](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>VM 'nizi yönetme

Bir tarayıcı tabanlı portal, komut satırı araçlarını kullanarak veya doğrudan API 'Ler aracılığıyla VM 'Leri yönetebilirsiniz. Gerçekleştirebileceğiniz bazı tipik yönetim görevleri şunlardır:

- VM hakkında bilgi alma
- VM 'ye bağlanma
- Kullanılabilirliği yönetme
- Yedeklemeler yapılıyor

### <a name="get-information-about-your-vm"></a>VM 'niz hakkında bilgi alın

Aşağıdaki tabloda bir VM hakkında bilgi alabileceğiniz bazı yollar gösterilmektedir.

|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Hub menüsünde **Sanal Makineler**’e tıklayıp açılan listeden VM’yi seçin. VM 'nin sayfasında, genel bakış bilgilerine, ayar değerlerine ve izleme ölçümlerine erişebilirsiniz.|
|Azure PowerShell|VM 'Leri yönetmek Azure ve Azure Stack benzerdir. PowerShell kullanma hakkında daha fazla bilgi için aşağıdaki Azure konusuna bakın:<br>[Azure PowerShell modülü ile Windows VM 'Leri oluşturma ve yönetme](/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|İstemci SDK'ları|VM C# 'leri yönetmek Için kullanmak Azure ve Azure Stack benzerdir. Daha fazla bilgi için aşağıdaki Azure konusuna bakın:<br>[Kullanarak Azure 'da Windows VM 'Leri oluşturma ve yönetmeC#](/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

Sanal makinenize bağlanmak için Azure Stack portalındaki **Bağlan** düğmesini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack VM 'Lerle ilgili konular](azure-stack-vm-considerations.md)
