---
title: Azure Stack veri merkezi tümleştirme Kılavuzu | Microsoft Docs
description: Dağıtım sonrası, veri merkezinizdeki Azure Stack başarılı bir yerinde dağıtımı için ne bekleneceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: asganesh
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 57e92f877ab9516b7b4978b5a919b18dad9b60ea
ms.sourcegitcommit: c2ea4ffb42563c26faaf2993ba7b484bcb6d5cb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71342851"
---
# <a name="azure-stack-datacenter-integration-walkthrough"></a>Azure Stack veri merkezi tümleştirme Kılavuzu

Bu makalede, bir çözüm sağlayıcısı tarafından başarılı bir yerinde dağıtıma aracılığıyla tümleşik bir sistem satın alma aşamasından uçtan uca Azure Stack müşteri deneyimi açıklanmaktadır. Bu bilgileri, yolculuğun hızını kolaylaştırmak ve bir Azure Stack müşteri olarak beklentilerini ayarlamanıza yardımcı olmak için kullanın.

Azure Stack müşteri olarak, aşağıdaki aşamaları tahmin etmeniz gerekir:

|     |Planlama aşaması|Siparişi işleme|Dağıtım öncesi|Fabrika işlemi|Donanım teslimi|Yerinde dağıtım|
|-----|-----|-----|-----|-----|-----|-----|
|**Microsoft**|Ön satış desteği sağlamak için iş ortağıyla birlikte çalışın.|Yazılım Lisanslama ve sözleşmelerini gereken şekilde hazırlayın.|Veri merkezi Tümleştirme gereksinimlerini ve müşteri belgelerini toplamak için gerekli araçları sağlayın.|En son temel yapıları ve araç zinciri güncelleştirmelerini aylık bir temposunda sağlayın.|Yok|Microsoft destek mühendisleri, herhangi bir dağıtım sorununu gidermeye yardımcı olur.|
|**İş ortağı**|Müşteri gereksinimlerine bağlı olarak çözüm seçeneklerini tavsiye edin.<br><br>Gerekirse kavram kanıtı (POC) önerin.<br><br>İş ilişkisi oluşturun.<br><br>Destek düzeyine karar verin.|Müşteriyle gerekli sözleşmeleri hazırlayın.<br><br>Müşteri satın alma siparişi oluştur.<br><br>Teslim zaman çizelgesine karar verin.<br><br>Gerekirse, müşteriyi Microsoft ile bağlayın.|Tüm dağıtım önkoşulları ve veri merkezi tümleştirme seçeneklerinin anlaşılmasını sağlamak için müşteriyi gerekli eğitimle sağlayın.<br><br>Eksiksiz ve doğruluğu sağlamak üzere toplanan verilerin doğrulanması için müşteriye yardımcı olun.|Son doğrulanan temel derlemeyi uygulayın.<br><br>Gerekli Microsoft dağıtım araç setini uygulayın.|Donanımı müşteri sitesine gönder.|Yerinde mühendis tarafından işlenen dağıtım.<br><br>Raf ve yığın.<br><br>Donanım yaşam döngüsü ana bilgisayarı (HLH) dağıtımı.<br><br>Azure Stack dağıtımı.<br><br>Müşteriye teslim.|
|**Müşteri**|Amaçlanan kullanım örneklerini tanıtın ve gereksinimleri belirtin.|Sözleşmeleri kullanmak, gözden geçirmek ve onaylamak için faturalandırma modelini belirleme.|[Dağıtım çalışma sayfasını](azure-stack-deployment-worksheet.md)tamamlayıp tüm dağıtım önkoşullarının karşılandığından ve dağıtıma uygun olduğundan emin olun.|Yok|Tüm gerekli güç ve soğutma, sınır bağlantısı ve diğer gerekli veri merkezi tümleştirme gereksinimlerinin yerinde olmasını sağlayarak veri merkezini hazırlayın.|Dağıtım sırasında abonelik kimlik bilgileri ve sağlanan veriler hakkında sorular varsa destek sağlamak için kullanılabilir.|
| | | | | | | |


## <a name="planning-phase"></a>Planlama aşaması
Planlama aşaması Microsoft 'un veya Azure Stack çözüm iş ortağının sizin için doğru çözüm Azure Stack olup olmadığını tespit etmek üzere gereksinimlerinizi değerlendirmek ve anlamak için sizinle birlikte çalışacaksınız:

Bunlar, aşağıdaki sorulara karar vermenize yardımcı olur:

-   Kuruluşunuza doğru çözüm Azure Stack mı?

-   Hangi boyut çözümüne ihtiyacınız olacak?

-   Kuruluşunuz için ne tür bir faturalandırma ve lisanslama modeli çalışacaktır?

-   Güç ve soğutma gereksinimleri nelerdir?

Donanım çözümünün gereksinimlerinize en uygun şekilde uyum sağlaması için [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) , Azure Stack donanım çözümünüz için uygun kapasiteyi ve yapılandırmayı belirlemede ön satın alma planlamasına yardımcı olmak için kullanılır.

Elektronik tablo, gereksinimlerinize en uygun donanım çözümlerini araştırmanıza ve analizinize alternatif olarak kullanılmak *üzere tasarlanmamıştır.* Azure Stack dağıtımı için planlama yaparken, Azure Stack tümleşik sistemler için [genel veri merkezi tümleştirme konularını](azure-stack-datacenter-integration.md) da gözden geçirmeniz gerekir.

## <a name="order-process-phase"></a>Sıra işlem aşaması
Bu aşamada, söz konusu olduğu gibi sorularınızın birçoğu yanıtlanmıştır. Azure Stack satın almaya hazır olduğunuza ve gerekli sözleşmelerin ve satın alma siparişlerinin tümünün imzalandıktan sonra, çözüm sağlayıcınızda tümleştirme gereksinimleri verilerini sağlamanız istenecektir.

## <a name="pre-deployment-phase"></a>Dağıtım öncesi aşaması
Bu aşamada, Azure Stack veri merkezinizle nasıl tümleştirmenize karar vermeniz gerekir. Bu işlemi kolaylaştırmak için, Microsoft, ortamınızda tümleşik bir sistem dağıtımı planlamak üzere gerekli bilgileri toplamanıza yardımcı olmak üzere bir gereksinimler şablonunu birlikte koymakta. Bu gereksinimler şablonu, çözüm sağlayıcılarıyla işbirliği sırasında yapılmıştır.

[Genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) makalesi, dağıtım çalışma sayfası olarak bilinen şablonu tamamlamanıza yardımcı olacak bilgiler sağlar.

> [!IMPORTANT]
> Bu aşamada, tüm önkoşul bilgilerinin araştırılması ve çözüm sıralanmadan önce karar vermiş olması önemlidir. Bu adımın zaman alıcı olduğunu ve kuruluşunuzun içindeki birden çok disiplinden veri toplamaya ihtiyaç duyduğuna dikkat edin. Yanlış veya tamamlanmamış bilgiler, daha uzun bir dağıtıma neden olabilir. 

Dağıtım öncesi aşamasında, aşağıdaki öğelere karar vermeniz gerekir:

- **Azure Stack bağlantı modeli ve kimlik sağlayıcısı**. [İnternet 'e bağlı olan veya bağlantısı kesilen](azure-stack-connection-models.md)Azure Stack dağıtmayı seçebilirsiniz. Hibrit senaryolar da dahil olmak üzere Azure Stack en avantajını almak için Azure 'a bağlı dağıtım yapmak istersiniz. Active Directory Federasyon Hizmetleri (AD FS) (AD FS) veya Azure Active Directory (Azure AD) seçeneğinin belirlenmesi, dağıtım zamanında yapmanız gereken tek seferlik bir karardır. **Tüm sistemi yeniden dağıtmaya gerek kalmadan, kimlik sağlayıcınızı daha sonra değiştiremezsiniz**.

- **Lisanslama modeli**. Arasından seçim yapabileceğiniz lisanslama modeli seçenekleri, sahip olduğunuz dağıtım türüne bağlıdır. Kimlik sağlayıcınız tercih ettiğiniz kiracı sanal makineleri veya kimlik sistemi ve kullandıkları hesaplar üzerinde bir pul yoktur.
    - [Bağlantısı kesilmiş bir dağıtımda](azure-stack-disconnected-deployment.md) olan müşteriler yalnızca bir seçeneğe sahiptir: kapasite tabanlı faturalandırma.

    - [Bağlı bir dağıtımda](azure-stack-connected-deployment.md) bulunan müşteriler, kapasite tabanlı faturalandırma ve kullandıkça öde arasında seçim yapabilir. Kapasite tabanlı faturalandırma, kayıt için bir Kurumsal Anlaşma (EA) Azure aboneliği gerektirir. Azure aboneliği aracılığıyla Azure Marketi 'ndeki öğelerin kullanılabilirliğini sağlayan kayıt için bu gereklidir.

- **Ağ tümleştirmesi**. [Ağ tümleştirmesi](azure-stack-network.md) , Azure Stack sistemlerinin dağıtımı, çalışması ve yönetimi için önemlidir. Azure Stack çözümünün dayanıklı olduğundan ve işlemlerini desteklemek için yüksek oranda kullanılabilir fiziksel bir altyapıya sahip olduğundan emin olmanın çeşitli konuları vardır.

- **Güvenlik Duvarı tümleştirmesi**. Azure Stack güvenli hale getirmek için [bir güvenlik duvarı kullanmanız](azure-stack-firewall.md) önerilir. Güvenlik duvarları, DDOS saldırılarını, yetkisiz giriş algılamayı ve içerik incelemesini önlemeye yardımcı olabilir. Ancak, Azure depolama hizmetleri için bir verimlilik sorunu haline gelebileceğini not edilmelidir.


- **Sertifika gereksinimleri**. Dağıtım için veri merkezinize bir yerinde mühendisin ulaşmadan *önce* tüm [gerekli sertifikaların](azure-stack-pki-certs.md) kullanılabilir olması önemlidir.

Önkoşul bilgileri dağıtım çalışma sayfası aracılığıyla toplandıktan sonra çözüm sağlayıcısı, veri merkezinize Azure Stack başarıyla tümleştirilmesine olanak sağlamak üzere toplanan verileri temel alarak fabrika işlemini otomatik hale bırakır.

## <a name="hardware-delivery-phase"></a>Donanım teslimi aşaması
Çözüm sağlayıcınız, çözüm özelliğinize geldiğinde zamanlama için sizinle birlikte çalışacaktır. Aldıktan ve yerine koyduktan sonra, Azure Stack dağıtımını gerçekleştirmek için bir mühendisin yerinde olması için çözüm sağlayıcısıyla zaman planlaması yapmanız gerekir.

Tüm önkoşul verilerinin *, yerinde mühendisin çözümü dağıtmaya ulaşmadan önce*kilitlenmesi ve kullanılabilir olması **önemlidir** .

-   Tüm sertifikaların satın alınması ve hazırlanmalıdır.

-   Bölge adı üzerinde karar verilmelidir.

-   Tüm ağ tümleştirme parametreleri sonlandırılır ve çözüm sağlayıcınızla paylaştığınız verilerle eşleşir.

> [!TIP]
> Bu bilgilerden herhangi biri değiştiyse, gerçek dağıtımı zamanlamadan önce değişikliği çözüm sağlayıcısıyla iletdiğinizden emin olun.

## <a name="onsite-deployment-phase"></a>Yerinde dağıtım aşaması
Azure Stack dağıtmak için, dağıtım için donanım çözüm sağlayıcınızdan bir yerinde mühendisin mevcut olması gerekir. Başarılı bir dağıtım sağlamak için, dağıtım çalışma sayfası aracılığıyla sunulan tüm bilgilerin değiştirilmediğinden emin olun.

Aşağıdaki denetimler, dağıtım deneyimi sırasında yerinde mühendisden beklemeniz gereken şeydir:

- Çözümün doğru bir şekilde yerleştirdiğinden ve gereksinimlerinizi karşıladığından emin olmak için tüm kablolama ve sınır bağlantılarını denetleyin.
- Varsa, HLH (donanım yaşam döngüsü Konağı) çözümünü yapılandırın.
- Tüm BMC, BIOS ve ağ ayarlarının doğru olduğundan emin olmak için denetleyin.
- Tüm bileşenler için bellenimin çözüm tarafından onaylanan en son sürümde olduğundan emin olun.
- Dağıtımı başlatın.

> [!NOTE]
> Yerinde mühendisin bir dağıtım yordamı, tamamlanması için bir iş haftası gerektirebilir.

## <a name="post-deployment-phase"></a>Dağıtım sonrası aşaması
Çözüm, tümleştirme sonrası aşamasında müşteriye teslim edilmeden önce iş ortağı tarafından birkaç adım gerçekleştirilmelidir. Bu aşamada, sistemin dağıtıldığından ve doğru şekilde uygulandığından emin olmak için doğrulama önemlidir. 

OEM Iş ortağı tarafından alınması gereken eylemler şunlardır:

- [Test-azurestack komutunu çalıştırın](azure-stack-diagnostic-test.md).

- [Azure 'A kaydolma](azure-stack-registration.md).

- [Market dağıtımı](azure-stack-download-azure-marketplace-item.md#use-the-marketplace-syndication-tool-to-download-marketplace-items).

- Yedekleme anahtarı yapılandırması ve HLH yapılandırma dosyaları.

- DVD 'yi kaldırın.

- Dağıtım için bir müşteri Özeti hazırlayın.

- [Çözüm yazılımının en son sürüme güncelleştirildiğinden emin olmak için güncelleştirmeleri denetleyin](./azure-stack-updates.md).

Yükleme türüne bağlı olarak gerekli veya isteğe bağlı birkaç adım vardır.

- Dağıtım [AD FS](azure-stack-integrate-identity.md)kullanılarak tamamlanırsa, Azure Stack damgasının müşterinin kendi AD FS tümleştirilmesi gerekir.

  > [!NOTE]
  > Bu adım müşterinin sorumluluğundadır, ancak iş ortağı isteğe bağlı olarak hizmet sunmayı tercih edebilir.

- İlgili iş ortağından mevcut bir izleme sistemiyle tümleştirme.

  -   [System Center Operations Manager tümleştirme](azure-stack-integrate-monitor.md) , Ayrıca, fleyönetim özelliklerini de destekler.

  -   [Nagios tümleştirmesi](azure-stack-integrate-monitor.md#integrate-with-nagios).

## <a name="overall-timeline"></a>Genel zaman çizelgesi

![Azure Stack site dağıtımı için genel zaman çizelgesi](./media/azure-stack-datacenter-integration-walkthrough/image1.png)

## <a name="support"></a>Destek
Azure Stack, tam sistem yaşam döngüsünü ele alan Azure ile tutarlı, tümleşik bir destek deneyimi sunar. Azure Stack tümleşik sistemleri tam olarak desteklemek için müşterilerin iki destek sözleşmesi vardır: Azure Hizmetleri desteği için Microsoft (veya bulut çözümü sağlayıcısı) ve sistem desteği için bir donanım sağlayıcısı. Tümleşik destek deneyimi, müşterilerin ilk çağırdıkları bağımsız olarak tutarlı bir destek deneyimi alması için Eşgüdümlü yükseltme ve çözümleme sağlar. Zaten Premier, Azure-Standard/ProDirect veya Microsoft ile Iş ortağı desteği olan müşteriler için Azure Stack yazılım desteği dahildir.

Tümleşik destek deneyimi, Microsoft ile donanım ortağı arasındaki destek durumlarının ve büyük/küçük harf güncelleştirmelerinin çift yönlü olarak aktarılması için bir servis talebi değişim mekanizması kullanır. Microsoft Azure Stack [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881)takip edecektir.

## <a name="next-steps"></a>Sonraki adımlar
[Genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md)hakkında daha fazla bilgi edinin.
