---
title: Windows Server ile ilgili SSS Azure Stack | Microsoft Docs
description: Windows Server için Azure Stack marketi SSS listesi
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
ms.date: 08/29/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 08/29/2019
ms.openlocfilehash: b71065d4a5af880fe5fb9a48d78a0e2821822b56
ms.sourcegitcommit: 5efa09034a56eb2f3dc0c9da238fe60cff0c67ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70143980"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Azure Stack Market 'te Windows Server SSS

Bu makalede, [Azure Stack Market](azure-stack-marketplace.md)'Teki Windows Server görüntüleri hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="marketplace-items"></a>Marketplace öğeleri

### <a name="how-do-i-update-to-a-newer-windows-image"></a>Nasıl yaparım? yeni bir Windows görüntüsüne güncelleştirmek mi istiyorsunuz?

İlk olarak, Azure Resource Manager şablonlarının belirli sürümlere başvurmadığını saptayın. Varsa, bu şablonları güncelleştirin veya eski görüntü sürümlerini saklayın. En iyisi **Sürüm: en son**kullanılır.

Daha sonra, herhangi bir sanal makine ölçek kümesi belirli bir sürüme başvurursanız, bunların daha sonra ölçeklendirilmesine ve eski sürümlerin tutulup tutulmayacağını istemediğinize karar vermeniz gerekir. Bu koşulların hiçbiri geçerli değilse, daha yeni olanları indirmeden önce Market 'teki eski görüntüleri silin. Bu, orijinalin İndirilme biçimi ise, bunları silmek için Market yönetimi 'ni kullanın. Daha sonra yeni sürümü indirin.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>Azure Stack Windows Server Market görüntüleri için lisanslama seçenekleri nelerdir?

Microsoft, Azure Stack Marketi aracılığıyla Windows Server görüntülerinin iki sürümünü sunmaktadır. Azure Stack ortamında bu görüntünün yalnızca bir sürümü kullanılabilir.  

- **Kullandıkça Öde**: Bu görüntüler, tam fiyatlı Windows ölçümlerini çalıştırır.
   Kimler kullanmalıdır: *Tüketim faturalandırma modelini*kullanan kurumsal anlaşma (EA) müşterileri; SPLA lisansı kullanmak istemeyen CSP 'Ler.
- **Kendi lisansınızı getirin (KLG)** : Bu görüntüler temel ölçümleri çalıştırır.
   Kimler kullanmalıdır: Windows Server lisansına sahip EA müşterileri; SPLA lisansı kullanan CSP 'Ler.

Azure hibrit kullanım avantajı (AHUB) Azure Stack desteklenmez. "Kapasite" modeli üzerinden lisans veren müşterilerin KLG görüntüsünü kullanması gerekir. Azure Stack Geliştirme Seti (ASDK) ile test ediyorsanız, bu seçeneklerden birini kullanabilirsiniz.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>Kiracılar/Kullanıcılarımı sunmak için yanlış sürümü indirdiğimde ne yapmalıyım?

Market yönetimi ile önce yanlış sürümü silin. Tamamlanmasını bekleyin ( **Market yönetim** dikey penceresine değil, tamamlamaya yönelik bildirimlere bakın). Ardından doğru sürümü indirin.

Görüntünün her iki sürümünü de indirdiğinizde, Market galerisindeki son müşteriler yalnızca en son sürüm tarafından görülebilir.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>Kullanıcı önceki Windows Derlemeleriyle "lisanslıyorum" kutusunu yanlış denetlerse ve lisanslarsa ne yapmalıyım?

Aşağıdaki betiği çalıştırarak, lisans modeli özniteliğini, kendi lisansını getir (KLG) öğesini Kullandıkça Öde (PAYG) modeline geçiş yapacak şekilde değiştirebilirsiniz:

```powershell
vm= Get-Azurermvm -ResourceGroup "<your RG>" -Name "<your VM>"
$vm.LicenseType = "None"
Update-AzureRmVM -ResourceGroupName "<your RG>" -VM $vm
```

Aşağıdaki komutları çalıştırarak sanal makinenizin lisans türünü kontrol edebilirsiniz. Lisans modeli **Windows_Server**DIYORSA, KLG fiyatı için ücretlendirilirsiniz; aksi takdırde, PAYG modeli başına Windows ölçümü için ücretlendirilirsiniz:

```powershell
$vm | ft Name, VmId,LicenseType,ProvisioningState
```

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-enterprise-agreement-entitlement"></a>Daha eski bir görüntünüz varsa ve kullanıcım "bir lisans aldım" kutusunu denetlemeyi unutdum veya kendi görüntülerimizi kullanıyoruz ve Kurumsal Anlaşma yetkilendirmemiz gerekiyor mu?

Aşağıdaki komutları çalıştırarak lisans modeli özniteliğini kendi lisans modelinizi getir olarak değiştirebilirsiniz:

```powershell
$vm= Get-Azurermvm -ResourceGroup "<your RG>" -Name "<your VM>"
$vm.LicenseType = "Windows_Server"
Update-AzureRmVM -ResourceGroupName "<your RG>" -VM $vm
```

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>SQL veya Machine Learning Server gibi Windows Server kullanan diğer VM 'Ler hakkında ne olacak?

Bu görüntüler **LicenseType** parametresini uygular, bu nedenle kullandığınız şekilde ödeirler. Bu parametreyi ayarlayabilirsiniz (önceki SSS yanıtına bakın). Bu, yalnızca Windows Server yazılımı için geçerlidir, SQL gibi katmanlı ürünlere değil, kendi lisansınızı getirmelerini gerektirir. Lisans Kullandıkça öde, katmanlı yazılım ürünleri için de geçerlidir.

### <a name="i-have-an-enterprise-agreement-ea-and-will-be-using-my-ea-windows-server-license-how-do-i-make-sure-images-are-billed-correctly"></a>Bir Kurumsal Anlaşma (EA) var ve EA Windows Server lisansımı kullanacak görüntülerin doğru şekilde faturalandırılmaya nasıl emin olabilirim?

LicenseType ekleyebilirsiniz **: Azure Resource Manager** şablonda Windows_Server. Bu ayar, her bir sanal makine kaynak bloğuna eklenmelidir.

## <a name="activation"></a>Etkinleştirme

Azure Stack bir Windows Server sanal makinesini etkinleştirmek için aşağıdaki koşulların doğru olması gerekir:

- OEM, Azure Stack her konak sisteminde uygun BIOS işaretini ayarladı.
- Windows Server 2012 R2 ve Windows Server 2016 [Otomatik sanal makine etkinleştirmesi](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11))kullanmalıdır. Anahtar yönetimi hizmeti (KMS) ve diğer etkinleştirme Hizmetleri Azure Stack desteklenmez.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>Sanal makinımın etkin olduğunu nasıl doğrulayabilirim?

Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın:

```shell
slmgr /dlv
```

Doğru bir şekilde etkinleştirildiyse, bu işlemi açıkça gösterir ve `slmgr` çıktıda gösterilen ana bilgisayar adı görüntülenir. Güncel olmadıkları veya kendi arkasındaki farklı bir sanal makineden gösterdiği için, ekrandaki filigranlara dayanmayın.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>VM My AVMA 'yı kullanacak şekilde ayarlanmadı, nasıl çözebilirim?

Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın:

```shell
slmgr /ipk <AVMA key>
```

Görüntünüz için kullanılacak anahtarlar için [Otomatik sanal makine etkinleştirme](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) makalesine bakın.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Kendi Windows Server görüntülerimi oluşturdum, AVMA 'yı nasıl kullandıkları nasıl emin olabilirim?

`slmgr /ipk` Komutu`sysprep` çalıştırmadan önce komut satırını uygun anahtarla yürütmeniz önerilir. Ya da, herhangi bir Unattend. exe kurulum dosyasına AVMA anahtarını dahil edin.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Azure 'da oluşturulan Windows Server 2016 görüntümi kullanmaya çalışıyorum ve KMS etkinleştirme 'yi etkinleştirmiyor veya kullanmıyor.

`slmgr /ipk` komutunu çalıştırın. Azure görüntüleri AVMA 'ya doğru geri dönemeyebilir, ancak Azure KMS sistemine ulaşabiliyorlarsa etkinleştirecelerdir. Bu VM 'Lerin AVMA kullanacak şekilde ayarlandığından emin olmanız önerilir.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>Bu adımların tümünü gerçekleştirdim ancak sanal makinelerim hala etkinleştirilmiyor.

Doğru BIOS işaretçilerinin yüklendiğini doğrulamak için donanım tedarikçinize başvurun.

### <a name="what-about-earlier-versions-of-windows-server"></a>Windows Server 'ın eski sürümleri ne?

[Otomatik sanal makine etkinleştirmesi](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) , Windows Server 'ın önceki sürümlerinde desteklenmez. VM 'Leri el ile etkinleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Stack marketi 'ne genel bakış](azure-stack-marketplace.md)
- [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
