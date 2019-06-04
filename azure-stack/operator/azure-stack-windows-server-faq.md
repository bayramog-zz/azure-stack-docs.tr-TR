---
title: Azure Stack Windows Server ilgili sık sorulan sorular | Microsoft Docs
description: Windows Server için Azure Stack Market SSS listesi
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
ms.date: 05/31/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 11/12/2018
ms.openlocfilehash: ffb58392075521b81d9b0cd71928ac0db680991f
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66459078"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Windows Server'da Azure Stack marketini SSS

Bu makalede, Windows Server görüntülerinin hakkında sık sorulan bazı sorular yanıtlanmaktadır [Azure Stack marketini](azure-stack-marketplace.md).

## <a name="marketplace-items"></a>Market öğesi

### <a name="how-do-i-update-to-a-newer-windows-image"></a>Daha yeni bir Windows görüntüsüne nasıl güncelleştirebilirim?

İlk olarak, herhangi bir Azure Resource Manager şablonları için belirli sürümler başvuruyorsa, belirleyin. Bu durumda, bu şablonların güncelleştirin veya eski görüntü sürümleri tutun. Kullanmak en iyisidir **sürümü: en son**.

Ardından, tüm sanal makine ölçek kümeleri belirli bir sürümüne başvuruyorsa, mi bunlar daha sonra ölçeklendirilir ve eski sürümlerini karar hakkında almalısınız. Bu koşulların hiçbiri uygularsanız, yeni olanlara indirmeden önce eski Market görüntüleri silin. Market yönetim özgün nasıl yüklenmiş ise, bunları silmek için kullanın. Ardından yeni sürümü indirin.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>Windows Server Market görüntüleri için Azure Stack'te lisanslama seçenekleri nelerdir?

Microsoft Azure Stack marketten Windows Server görüntülerini iki sürümünü sunar:

- **Kullandıkça Öde**: Bu görüntüler, tam fiyat Windows ölçümleri çalıştırın.
   Kimin kullanmanız gerekir: Kullanan Kurumsal Anlaşma (EA) müşterileri *tüketim faturalandırma modeli*; SPLA lisans kullanmak istiyor musunuz CSP'ler.
- **Kendi lisansınızı getirin (BYOL)** : Bu görüntüler, temel ölçümleri çalıştırın.
   Kimin kullanmanız gerekir: EA müşterileri bir Windows Server lisansına sahip; SPLA lisansı kullanan CSP'ler.

Azure karma kullanım Avantajı'nı (AHUB), Azure Stack üzerinde desteklenmiyor. "Kapasite" modeli aracılığıyla lisans müşteriler KLG görüntüsü kullanmanız gerekir. Azure Stack geliştirme Seti'ni (ASDK) ile test ediyorsanız, bu seçeneklerden birini kullanabilirsiniz.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>Kiracılar/kullanıcılarımın sunmak için yanlış sürüm karşıdan ne olur?

Yanlış sürüm Market yönetiminden silin. Tam olarak tamamlanmasını bekleyin (tamamlama, bildirimleri bakın değil **Market Yönetim** dikey penceresinde). Ardından doğru sürümünü indirin.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>My kullanıcı yanlış önceki Windows "Bir lisansım" kutuya kullanıma ne oluşturur ve bir lisansına sahip olmayan?

Bkz: [Kullandıkça Öde dön teklifi'nden yararlanarak Windows Server Vm'lerinin Dönüştür](/azure/virtual-machines/windows/hybrid-use-benefit-licensing#powershell-1).

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-enterprise-agreement-entitlement"></a>Ne eski görüntüyü sahibim ve "Bir lisansım" kutusu ya da biz kendi görüntülerinizi kullanın ve Kurumsal Anlaşma yetkilendirmesini sahibiz denetlemek benim kullanıcı mı unuttunuz?

Bkz: [Windows Server için Azure hibrit Avantajı'nı kullanarak mevcut bir VM'ye dönüştürmek](/azure/virtual-machines/windows/hybrid-use-benefit-licensing#convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server). Azure hibrit avantajı, Azure Stack için geçerli değildir, ancak bu ayar etkisini uygulandığını unutmayın.

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>Windows Server, SQL veya Machine Learning sunucusu gibi diğer Vm'lere hakkında neler diyeceksiniz?

Bu görüntüleri geçerli **licenseType** , Kullandıkça Ödeme olduklarından parametresi. Bu parametre ayarlayabilirsiniz (yanıt önceki SSS Bölümüne bakın). Bu yalnızca Windows Server yazılımı, kendi lisansını Getir gerektiren katmanlı ürün, SQL gibi geçerlidir. Lisanslama Kullandıkça Ödeme katmanlı yazılım ürünleri için geçerli değildir.

### <a name="i-have-an-enterprise-agreement-ea-and-will-be-using-my-ea-windows-server-license-how-do-i-make-sure-images-are-billed-correctly"></a>Ben bir Kurumsal Anlaşma (EA) sahip ve EA Windows Server Lisansımı kullanmak; görüntüler doğru faturalandırılır nasıl emin olabilirim?

Ekleyebileceğiniz **licenseType: Windows_Server** bir Azure Resource Manager şablonunda. Her sanal makine kaynak bloğu için bu ayarı eklenmesi gerekir.

## <a name="activation"></a>Etkinleştirme

Bir Windows Server sanal makinesini Azure Stack'te etkinleştirmek için aşağıdaki koşulların doğru olması gerekir:

- OEM Azure Stack'te her bir konak sisteminde uygun BIOS işaret ayarladı.
- Windows Server 2012 R2 ve Windows Server 2016 kullanmalıdır [otomatik sanal makine etkinleştirmesi](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)). Anahtar Yönetim Hizmeti (KMS) ve diğer Etkinleştirme Hizmetleri, Azure Stack üzerinde desteklenmez.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>Sanal Makinem etkinleştirildiğini nasıl doğrulayabilirim?

Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın:

```shell
slmgr /dlv
```

Doğru bir şekilde etkinleştirildi, bu açıkça belirtilen görürsünüz ve ana bilgisayar adı görüntülenen `slmgr` çıktı. Güncel olmayabilir veya arkasında sizinki farklı bir sanal makineden gösteren ekranındaki filigranlar bağımlı değil.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>Sanal Makinem AVMA, nasıl düzeltebilirim kullanmak için ayarlanmamış?

Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın:

```shell
slmgr /ipk <AVMA key>
```

Bkz: [otomatik sanal makine etkinleştirmesi](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) makale görüntünüzü kullanmak anahtarlar için.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Kendi Windows Server görüntülerini oluşturabilirim, AVMA kullandıkları nasıl emin?

Yürütme, önerilen `slmgr /ipk` komut satırını çalıştırmadan önce uygun anahtarla `sysprep` komutu. Veya herhangi bir Unattend.exe Kurulum dosyasında AVMA anahtarını içerir.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Azure üzerinde oluşturulmuş Windows Server 2016 Görüntümü kullanılacak getirmeye çalışıyorum ve değil etkinleştirme veya KMS etkinleştirme kullanma.

`slmgr /ipk` komutunu çalıştırın. Azure görüntüleri doğru geri için avma'yı olamaz, ancak Azure KMS sistem erişebiliyorsa etkinleştirir. Bu VM'ler avma'yı kullanmak için ayarlandığından emin olmak önerilir.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>Tüm adımları gerçekleştirdiniz ancak sanal makinelerim hala değil etkinleştirmekte olduğunuz.

BIOS işaretçileri doğru yüklendiğini doğrulamak için donanım satıcısına başvurun.

### <a name="what-about-earlier-versions-of-windows-server"></a>Windows Server'ın önceki sürümleri hakkında neler diyeceksiniz?

[Otomatik sanal makine etkinleştirmesi](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) Windows Server'ın önceki sürümlerinde desteklenmez. Sanal makineleri el ile etkinleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Stack Marketini genel bakış](azure-stack-marketplace.md)
- [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
