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
ms.date: 11/07/2019
ms.author: mabrigg
ms.reviewer: asganesh
ms.lastreviewed: 11/07/2019
ms.openlocfilehash: 6bcdbcb03cdd4151978e9eeee645a0d4ab488fe3
ms.sourcegitcommit: ed44d477b9fd11573d1e0d1ed3a3c0ef4512df53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73845763"
---
# <a name="azure-stack-datacenter-integration-walkthrough"></a>Azure Stack veri merkezi tümleştirme Kılavuzu

Bu makalede, satın alma sonrası destek 'e kadar Azure Stack veri merkezi tümleştirmesi için uçtan uca işlem açıklanmaktadır. Tümleştirme, müşteri, çözüm sağlayıcısı ve Microsoft arasında işbirliği yapılan bir projem. Projenin her üyesine yönelik belirli adımları görmek için aşağıdaki sekmelere tıklayın ve proje zaman çizelgesi için farklı aşamaların Özeti için bir sonraki bölüme bakın. 

# <a name="customertabcustomer"></a>[Müşterisi](#tab/customer)

1. Kullanım örneklerini ve gereksinimleri açıkla
1. Faturalandırma modelini belirleme
1. Sözleşmeleri gözden geçirme ve onaylama
1. [Dağıtım çalışma sayfasını](azure-stack-deployment-worksheet.md) doldurun
1. Dağıtım önkoşullarının karşılandığından emin olun
1. Veri merkezini hazırlama 
1. Dağıtım sırasında abonelik bilgilerini sağlama
1. Belirtilen verilerle ilgili tüm soruları çözün

# <a name="partnertabpartner"></a>[Ortağınız](#tab/partner)

1. Müşteri gereksinimlerine bağlı olarak çözüm seçeneklerini önerme
1. Kavram kanıtı öner (POC) 
1. Destek düzeyine karar verme
1. Müşteri ile sözleşmeleri hazırlama
1. Müşteri satın alma siparişi oluştur
1. Teslim zamanlaması belirleyin
1. Müşteriyi Microsoft ile bağlama 
1. Müşteriye dağıtım üzerinde eğitme 
1. Müşteri tarafından toplanan verileri doğrulama yardım
1. Temel derlemeyi ve Microsoft dağıtım araç setini yükleyip doğrulama
1. Donanımı müşteri sitesine gönder
1. Yerinde mühendis sağlama
1. Raf ve yığın
1. Donanım yaşam döngüsü konağını (HLH) dağıtma 
1. Azure Stack dağıtma
1. Müşteriye teslim

# <a name="microsofttabmicro"></a>[MICROSOFT](#tab/micro)

1. Ön satış desteği için iş ortağına katılın
2. Yazılım Lisanslama ve sözleşmelerini hazırlama
3. Veri merkezi Tümleştirme gereksinimlerini toplamak için Araçlar sağlama
4. Aylık temel yapılar ve araç zinciri güncelleştirmeleri sağlama
5. Microsoft destek mühendisleri her türlü dağıtım sorununu gidermeye yardımcı olur

---

## <a name="planning"></a>Planlama
Microsoft veya bir Azure Stack çözüm iş ortağı hedeflerinizi değerlendirmenize yardımcı olur. Şu gibi sorulara karar vermenize yardımcı olur:

-   Kuruluşunuza doğru çözüm Azure Stack mı?
-   Kuruluşunuz için ne tür bir faturalandırma ve lisanslama modeli çalışacaktır?
-   Hangi boyut çözümüne ihtiyacınız olacak?
-   Güç ve soğutma gereksinimleri nelerdir?

Gereksinimlerinize uygun en iyi donanım kapasitesini ve yapılandırmayı araştırmak ve analiz etmek için [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) kullanın. 

## <a name="ordering"></a>Sıralama
Kuruluşunuz Azure Stack satın alma, sözleşmeleri ve satın alma siparişlerini imzalar ve tümleştirme gereksinimleri verilerini çözüm sağlayıcısına sağlar.

## <a name="pre-deployment"></a>Dağıtım öncesi
Azure Stack veri merkezinizle nasıl tümleştirileceğine karar verirsiniz. Microsoft, gerekli bilgileri toplamanıza yardımcı olmak için bir [dağıtım çalışma sayfası](azure-stack-deployment-worksheet.md) yayınlamak üzere çözüm sağlayıcılarıyla birlikte işbirliği yapılır.
[Genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) makalesi, dağıtım çalışma sayfası olarak bilinen şablonu tamamlamanıza yardımcı olacak bilgiler sağlar.

> [!IMPORTANT]
> Dağıtım gecikmelerini önlemeye yardımcı olmak için çözüm sipariş etmeden önce tüm Önkoşullar araştırılır. Önkoşulları doğrulamak zaman alabilir ve kuruluşunuzdaki farklı departmanlardan koordinasyon ve veri toplamayı gerektirebilir. 

Aşağıdaki öğeleri seçersiniz:

- **Azure Stack bağlantı modeli ve kimlik sağlayıcısı**. [İnternet 'e bağlı olan veya bağlantısı kesilen](azure-stack-connection-models.md)Azure Stack dağıtmayı seçebilirsiniz. Hibrit senaryolar da dahil olmak üzere Azure Stack en avantajını almak için Azure 'a bağlı dağıtım yapmak istersiniz. Active Directory Federasyon Hizmetleri (AD FS) (AD FS) veya Azure Active Directory (Azure AD) seçeneğinin belirlenmesi, dağıtım zamanında yapmanız gereken tek seferlik bir karardır. **Tüm sistemi yeniden dağıtmaya gerek kalmadan, kimlik sağlayıcınızı daha sonra değiştiremezsiniz**.

- **Lisanslama modeli**. Arasından seçim yapabileceğiniz lisanslama modeli seçenekleri, sahip olduğunuz dağıtım türüne bağlıdır. Kimlik sağlayıcınız tercih ettiğiniz kiracı sanal makineleri veya kimlik sistemi ve kullandıkları hesaplar üzerinde bir pul yoktur.
    - [Bağlantısı kesilmiş bir dağıtımda](azure-stack-disconnected-deployment.md) olan müşteriler yalnızca bir seçeneğe sahiptir: kapasite tabanlı faturalandırma.

    - [Bağlı bir dağıtımda](azure-stack-connected-deployment.md) bulunan müşteriler, kapasite tabanlı faturalandırma ve kullandıkça öde arasında seçim yapabilir. Kapasite tabanlı faturalandırma, kayıt için bir Kurumsal Anlaşma (EA) Azure aboneliği gerektirir. Azure aboneliği aracılığıyla Azure Marketi 'ndeki öğelerin kullanılabilirliğini sağlayan kayıt için bu gereklidir.

- **Ağ tümleştirmesi**. [Ağ tümleştirmesi](azure-stack-network.md) , Azure Stack sistemlerinin dağıtımı, çalışması ve yönetimi için önemlidir. Azure Stack çözümünün dayanıklı olduğundan ve işlemlerini desteklemek için yüksek oranda kullanılabilir fiziksel bir altyapıya sahip olduğundan emin olmanın çeşitli konuları vardır.

- **Güvenlik Duvarı tümleştirmesi**. Azure Stack güvenli hale getirmek için [bir güvenlik duvarı kullanmanız](azure-stack-firewall.md) önerilir. Güvenlik duvarları, DDOS saldırılarını, yetkisiz giriş algılamayı ve içerik incelemesini önlemeye yardımcı olabilir. Ancak, Azure depolama hizmetleri için bir verimlilik sorunu haline gelebileceğini not edilmelidir.

- **Sertifika gereksinimleri**. Dağıtım için veri merkezinize bir yerinde mühendisin ulaşmadan *önce* tüm [gerekli sertifikaların](azure-stack-pki-certs.md) kullanılabilir olması önemlidir.

Önkoşul bilgileri dağıtım çalışma sayfası aracılığıyla toplandıktan sonra çözüm sağlayıcısı, veri merkezinize Azure Stack başarıyla tümleştirilmesine olanak sağlamak üzere toplanan verileri temel alarak fabrika işlemini otomatik hale bırakır.

## <a name="hardware-delivery"></a>Donanım teslimi 
Çözüm sağlayıcınız, çözüm özelliğinize geldiğinde zamanlama için sizinle birlikte çalışacaktır. Aldıktan ve yerine koyduktan sonra, Azure Stack dağıtımını gerçekleştirmek için bir mühendisin yerinde olması için çözüm sağlayıcısıyla zaman planlaması yapmanız gerekir.

Tüm önkoşul verilerinin *, yerinde mühendisin çözümü dağıtmaya ulaşmadan önce*kilitlenmesi ve kullanılabilir olması **önemlidir** .

-   Tüm sertifikaların satın alınması ve hazırlanmalıdır.

-   Bölge adı üzerinde karar verilmelidir.

-   Tüm ağ tümleştirme parametreleri sonlandırılır ve çözüm sağlayıcınızla paylaştığınız verilerle eşleşir.

> [!TIP]
> Bu bilgilerden herhangi biri değiştiyse, gerçek dağıtımı zamanlamadan önce değişikliği çözüm sağlayıcısıyla iletdiğinizden emin olun.

## <a name="onsite-deployment"></a>Yerinde dağıtım 
Azure Stack dağıtmak için, dağıtım için donanım çözüm sağlayıcınızdan bir yerinde mühendisin mevcut olması gerekir. Başarılı bir dağıtım sağlamak için, dağıtım çalışma sayfası aracılığıyla sunulan tüm bilgilerin değiştirilmediğinden emin olun.

Aşağıdaki denetimler, dağıtım deneyimi sırasında yerinde mühendisden beklemeniz gereken şeydir:

- Çözümün doğru bir şekilde yerleştirdiğinden ve gereksinimlerinizi karşıladığından emin olmak için tüm kablolama ve sınır bağlantılarını denetleyin.
- Varsa, HLH (donanım yaşam döngüsü Konağı) çözümünü yapılandırın.
- Tüm BMC, BIOS ve ağ ayarlarının doğru olduğundan emin olmak için denetleyin.
- Tüm bileşenler için bellenimin çözüm tarafından onaylanan en son sürümde olduğundan emin olun.
- Dağıtımı başlatın.

> [!NOTE]
> Yerinde mühendisin bir dağıtım yordamı, tamamlanması için bir iş haftası gerektirebilir.

## <a name="post-deployment"></a>Dağıtım sonrası 
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

## <a name="schedule"></a>Zamanlama

![Azure Stack site dağıtımı için genel zaman çizelgesi](./media/azure-stack-datacenter-integration-walkthrough/image1.png)

## <a name="support"></a>Destek
Azure Stack, tam sistem yaşam döngüsünü ele alan Azure ile tutarlı, tümleşik bir destek deneyimi sunar. Azure Stack tümleşik sistemleri tam olarak desteklemek için müşterilerin iki destek sözleşmesi vardır: Azure Hizmetleri desteği için Microsoft (veya bulut çözümü sağlayıcısı) ve sistem desteği için bir donanım sağlayıcısı. Tümleşik destek deneyimi, müşterilerin ilk çağırdıkları bağımsız olarak tutarlı bir destek deneyimi alması için Eşgüdümlü yükseltme ve çözümleme sağlar. Zaten Premier, Azure-Standard/ProDirect veya Microsoft ile Iş ortağı desteği olan müşteriler için Azure Stack yazılım desteği dahildir.

Tümleşik destek deneyimi, Microsoft ile donanım ortağı arasındaki destek durumlarının ve büyük/küçük harf güncelleştirmelerinin çift yönlü olarak aktarılması için bir servis talebi değişim mekanizması kullanır. Microsoft Azure Stack [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881)takip edecektir.

## <a name="next-steps"></a>Sonraki adımlar
[Genel veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md)hakkında daha fazla bilgi edinin.
