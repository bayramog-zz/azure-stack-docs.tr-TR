---
title: Azure Stack vm'lere giriş | Microsoft Docs
description: Azure Stack VM'ler hakkında bilgi edinin.
services: azure-stack
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: f5086dcae534656cea4ef1addacae3f5acdcb2d6
ms.sourcegitcommit: be5382f715a9c1c18c660b630d8fcd823f13aae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66197395"
---
# <a name="introduction-to-azure-stack-vms"></a>Azure Stack vm'lere giriş

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack sanal makineleri (VM) isteğe bağlı ve ölçeklenebilir bilgi işlem kaynak türü sunar. Bilgi işlem ortamınız üzerinde daha fazla denetime ihtiyacınız olduğunda bir sanal makine seçebilirsiniz. Bu makalede, ilk VM'nizi oluşturmadan önce ayrıntıları sağlar.

Azure Stack sanal kümeler ya da makineleri tek tek yönetmek zorunda kalmadan sanallaştırma esnekliği sunar. Ancak, yine de yapılandırma, düzeltme eki uygulama ve üzerinde çalıştığı yazılım yükleme gibi görevleri gerçekleştirerek VM'nin sürdürmeniz gerekir.

Azure Stack Vm'leri çeşitli yollarla kullanabilirsiniz. Örneğin:

- **Geliştirme ve test**: Azure Stack sanal makineleri kodu için gereken belirli bir yapılandırmaya sahip bir bilgisayar oluşturmak ve bir uygulamayı test etmek etkinleştirin.

- **Bulut uygulamalarında**: Uygulamanız için isteğe bağlı dalgalanma çünkü bunu çalıştırmak için Azure Stack'te bir VM üzerinde ekonomik mantıklı olabilir. İhtiyaç kalmadığında bunları kapatırsınız kullandığınızda, ek VM'ler için ödeme yaparsınız.

- **Veri merkezini genişletme**: Vm'leri bir Azure Stack'te kuruluşunuzun ağına veya azure'a sanal ağa bağlanabilir.

Uygulamanızın kullandığı ölçeği büyütün veya ne olursa olsun, ihtiyaçlarınızı karşılamak için gerekli için ölçek genişletme, VM'ler.

## <a name="before-creating-a-vm"></a>VM oluşturmadan önce

Aynı zamanda Azure Stack'te uygulama altyapısı oluştururken her zaman tasarım konuları mevcuttur. Altyapınızı oluşturma işlemine başlamadan önce dikkat etmeniz gereken VM bu özellikleri şunlardır:

- Uygulama kaynaklarınızın adları.
- VM boyutu.
- Oluşturulabilecek VM'ler en fazla sayısı.
- VM çalışan işletim sistemi.
- VM'nin başlatıldıktan sonraki yapılandırması.
- VM'nin ihtiyaç duyduğu ilgili kaynaklar.

### <a name="naming"></a>Adlandırma

Bir VM'ye atanmış bir ad ve işletim sisteminin bir parçası olarak yapılandırılan bir bilgisayar adı sahiptir. VM adı en fazla 15 karakter uzunluğunda olabilir.

İşletim sistemi diski oluşturmak için Azure Stack kullanırsanız, bilgisayar adını ve VM adıyla aynıdır. Karşıya yükleme ve önceden yapılandırılmış bir işletim sistemini içeren kendi görüntünüzü kullanma ve bir VM oluşturmak için bunu kullanın, adları farklı olabilir. Kendi görüntü dosyanızı yüklediğinizde en iyi uygulama, işletim sisteminde bilgisayar adını VM adıyla eşleştiğinden emin olun.

### <a name="vm-size"></a>VM boyutu

Kullandığınız VM'nin boyutunu, çalıştırmak istediğiniz iş yüküne göre belirlenir. Seçtiğiniz boyut işlemci gücü, bellek ve depolama kapasitesi gibi ölçütleri belirler. Azure Stack, çeşitli sayıda kullanım türünü desteklemek üzere boyutları sunar.

### <a name="vm-limits"></a>VM sınırları

Aboneliğinizi yerinde projeniz için VM dağıtımını etkileyebilecek varsayılan kota sınırları vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.

### <a name="operating-system-disks-and-images"></a>İşletim sistemi diskleri ve görüntüleri

VM'ler, kendi işletim sistemi (OS) ve verilerini depolamak için sanal sabit diskleri (VHD) kullanın. VHD bir işletim sistemi yüklemek için arasından seçim görüntüler için de kullanılır. Azure Stack çeşitli türlerde işletim sistemleri ve sürümleri ile kullanmak için bir pazar sağlar. Market görüntüleri görüntü yayımcısı, teklif, SKU ve sürümü tarafından tanımlanır (genellikle en son sürümü olarak belirtilen **son**).

Aşağıdaki tabloda, görüntü bilgilerini bulmak gösterilmektedir:

|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Bir görüntüyü kullanmak istediğinizde değerler otomatik olarak belirtilir.|
|Azure Stack PowerShell|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST API'leri     |[Görüntü yayımcılarını listeleme](/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Görüntü tekliflerini listeleme](/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Görüntü SKU'ları Listele](/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

Karşıya yükleme ve kendi görüntünüzü kullanma seçebilirsiniz. Bunu yaparsanız, yayımcı adı, teklif ve SKU kullanılmaz.

### <a name="extensions"></a>Genişletmeler

VM uzantıları, aracılığıyla dağıtım sonrası yapılandırma ve otomatik görevlerle VM'nize yeni özellikler sağlar.
Uzantıları kullanarak şu genel görevleri gerçekleştirebilirsiniz:

- **Özel betik çalıştırma**: Özel betik uzantısı, VM hazırlandığında kendi betiğinizi çalıştırarak iş yükleri VM'de yapılandırmanıza yardımcı olur.

- **Yapılandırma dağıtma ve yönetme**: PowerShell Desired State Configuration (DSC) uzantısı yapılandırmalarını ve ortamları yönetmek için bir VM üzerinde DSC ayarlamanıza yardımcı olur.

- **Tanılama verilerini toplama**: Azure tanılama uzantısını VM, uygulamanızın durumunu izlemek için kullanılabilecek tanılama verilerini toplayacak şekilde yapılandırmanıza yardımcı olur.

### <a name="related-resources"></a>İlgili kaynaklar

Aşağıdaki tablodaki kaynaklar VM tarafından kullanılır ve mevcut veya sanal Makineyi oluştururken oluşturulması gerekir:

|Resource|Gerekli|Açıklama|
|---------|---------|---------|
|Kaynak grubu|Evet|VM bir kaynak grubunda yer almalıdır.|
|Depolama hesabı|Hayır|VM'yi yönetilen diskleri kullanıyorsanız, sanal sabit disklerini depolamak için depolama hesabı gerektirmez. <br>VM, yönetilmeyen diskler kullanıyorsanız, sanal sabit disklerini depolamak için depolama hesabınızın olması gerekir.|
|Sanal ağ|Evet|VM’in bir sanal ağa üye olması gerekir.|
|Genel IP adresi|Hayır|VM, uzaktan erişim için atanmış bir genel IP adresine sahip olabilir.|
|Ağ arabirimi|Evet|VM’in ağda iletişim kurabilmek için ağ arabirimine ihtiyacı vardır.|
|Veri diskleri|Hayır|VM, depolama olanaklarını genişletmek için veri disklerine sahip olabilir.|

## <a name="create-your-first-vm"></a>İlk VM'nizi oluşturun

Bir VM oluşturmak için birkaç seçeneğiniz vardır. Seçiminiz ortamınıza bağlıdır. Aşağıdaki tabloda VM'nizi oluşturmak kullanmaya başlamanıza yardımcı olmak için bilgiler sağlar:

|Yöntem|Makale|
|---------|---------|
|Azure Stack portalı|Azure Stack portal ile Windows VM oluşturma<br>[Azure Stack portalını kullanarak bir Linux VM oluşturma](azure-stack-quick-linux-portal.md)|
|Şablonlar|Azure Stack hızlı başlangıç şablonları şu adreste bulunabilir:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStarvirtualt-Templates)|
|PowerShell|[Azure Stack'te PowerShell kullanarak Windows VM oluşturma](azure-stack-quick-create-vm-windows-powershell.md)<br>[Azure Stack'te PowerShell kullanarak bir Linux VM oluşturma](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Azure Stack'te CLI kullanarak bir Windows VM oluşturma](azure-stack-quick-create-vm-windows-cli.md)<br>[Azure Stack'te CLI kullanarak Linux VM oluşturma](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>Sanal Makinenizi Yönetme

VM'ler tarayıcı tabanlı bir portal, betik oluşturma ya da doğrudan API'ler aracılığıyla desteğine sahip komut satırı araçları kullanarak yönetebilirsiniz. Bunu yapmanız gerekebilir genel yönetim görevlerinden bazıları şunlardır:

- Bir VM hakkında bilgi alma
- Bir sanal Makineye bağlanma
- Kullanılabilirliği yönetme
- Yedeklemeler yapma

### <a name="get-information-about-your-vm"></a>Sanal makinenizin hakkında bilgi edinin

Aşağıdaki tabloda VM hakkında bilgi edinme yöntemlerinden bazıları gösterilmektedir.

|Yöntem|Açıklama|
|---------|---------|
|Azure Stack portalı|Hub menüsünde **Sanal Makineler**’e tıklayıp açılan listeden VM’yi seçin. VM sayfasında, Özet bilgilere, ayar değerlerine ve izleme ölçümlerine erişiminiz.|
|Azure PowerShell|Vm'leri yönetme, Azure ve Azure Stack benzerdir. PowerShell kullanma hakkında daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|İstemci SDK'ları|Vm'leri yönetmek için C# kullanarak Azure ve Azure Stack benzerdir. Daha fazla bilgi için aşağıdaki Azure konuya bakın:<br>[C# kullanarak azure'da Windows Vm'leri oluşturma ve yönetme](/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

Kullanabileceğiniz **Connect** VM'nize bağlanmak için Azure Stack portalında düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack'te Vm'lerde dikkate alınacak noktalar](azure-stack-vm-considerations.md)
