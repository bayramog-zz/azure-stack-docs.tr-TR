---
title: Azure Stack yönetim temel bilgileri | Microsoft Docs
description: Azure Stack yönetmek için bilmeniz gerekenleri öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2019
ms.author: justinha
ms.lastreviewed: 05/29/2019
ms.openlocfilehash: 3887712d2c7f14498536e5ad22494bedaa41197c
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691678"
---
# <a name="azure-stack-administration-basics"></a>Azure Stack yönetim temel bilgileri

Azure Stack yönetim yeniyseniz, bilmeniz gereken birkaç şey vardır. Bu makalede, Azure Stack operatörü ve bunları üretken yardımcı olabilmemiz için kullanıcılarınıza söylemeniz gerekenler olarak sizin rolünüze genel bir bakış sağlar.

## <a name="understand-the-builds"></a>Yapıları anlama

### <a name="integrated-systems"></a>Tümleşik sistemler

Güncelleştirme paketleri, bir Azure Stack tümleşik sistemi kullanıyorsanız, Azure Stack güncelleştirilmiş sürümlerini dağıtın. Bu paketleri içeri aktarabilir ve bunları kullanarak uygulama **güncelleştirmeleri** kutucuğuna Yönetici portalı'nda.
 
### <a name="development-kit"></a>Geliştirme Seti

Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, gözden [Azure Stack nedir?](../asdk/asdk-what-is.md) amacını ASDK ve kısıtlamalarını öğrenin. ASDK olarak kullanabileceğiniz bir *korumalı alan*, burada Azure Stack'i değerlendirin ve geliştirebilir ve bir üretim dışı ortamda uygulamalarınızı test edin. Dağıtım bilgileri için bkz. [Azure Stack geliştirme Seti'ni dağıtım](../asdk/asdk-install.md).

Azure'da olduğu gibi biz yenilikleri hızlıca sunun. Biz yeni derlemeler düzenli olarak kullanıma sunarız. ASDK çalıştırıyorsanız ve taşımak en son sürüme gerekir istediğiniz [Azure Stack'i yeniden dağıtma](../asdk/asdk-redeploy.md). Güncelleştirme paketleri uygulanamıyor. Bu işlem zaman alır, ancak en son özellikleri deneyin avantajdır. Web sitemizde ASDK belgelerine en son sürüm yapısı yansıtır.

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetleri hakkında bilgi edinin

Hangi Hizmetleri, kullanıcılarınıza sunabileceğiniz bir farkındalık gerekir. Azure Stack, Azure hizmetlerin bir alt kümesini destekler. Desteklenen hizmet listesini gelişmeye devam eder.

**Temel Hizmetleri**

Varsayılan olarak, Azure Stack şu "temel" Hizmetleri Azure Stack dağıtırken:

- İşlem
- Depolama
- Ağ
- Key Vault

Bu temel hizmetlerle minimal yapılandırma ile kullanıcılarınıza-olarak-hizmet altyapı (Iaas) sunabilir.

**Ek hizmetler**

Şu anda aşağıdaki ek olarak bir-hizmet Platform (PaaS) Hizmetleri destekler:

- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları
- Kubernetes (önizlemede)

Bu hizmetler, bunları kullanıcılarınıza sunabileceğiniz önce ek yapılandırma gerektirir. Daha fazla bilgi için "Öğreticiler" ve müşterilerimizin Azure Stack operatör belgeleri "nasıl yapılır guides\Offer Hizmetleri" bölümlerine bakın.

**Hizmet yol haritası**

Azure Stack, Azure Hizmetleri için destek eklemeye devam edeceğiz. Gelecekteki yol haritası için bkz. [Azure Stack: Bir Azure uzantısı](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) teknik incelemesi. Ayrıca izleyebilirsiniz [Azure Stack blog gönderilerini](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) yeni duyuruları.

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure Stack yönetirken dikkat edilmesi gereken birkaç hesabında dikkate alınacak noktalar vardır. Dağıtımlarda özellikle Windows Server Active Directory Federasyon Hizmetleri (ADFS) yerine Azure Active Directory (Azure AD) kimlik sağlayıcısı olarak kullanma. Azure Stack tümleşik sistemleri ve ASDK dağıtımları için aşağıdaki hesabı maddeler geçerlidir:


|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (. \Administrator)|ASDK konak yönetici|ASDK konak yönetici|
|AzureStack\AzureStackAdmin|ASDK konak yönetici<br><br>Azure Stack yönetim portalında oturum açmak için kullanılabilir<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim|ASDK konak yönetici<br><br>Azure Stack yönetim portalına erişim yok<br><br>Service Fabric halkaları yönetmek ve görüntülemek için erişim<br><br>Artık sahibi varsayılan sağlayıcı aboneliği (DPS)|
|AzureStack\CloudAdmin|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın|Erişebilir ve ayrıcalıklı uç nokta içinde izin verilen komutları çalıştırın<br><br>ASDK ana bilgisayara oturum açamıyorum<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|
|Azure AD genel Yöneticisi|Yükleme sırasında kullanılan<br><br>Varsayılan sağlayıcı aboneliği (yu DPS) sahibi|Geçerli değil|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi araçları kullanabilir?
 
Kullanabileceğiniz [Yönetici portalı](azure-stack-manage-portals.md) veya Azure Stack yönetmek için PowerShell'i. Temel kavramları öğrenmenin en kolay yolu portalı kullanmaktır. PowerShell kullanmak istiyorsanız, hazırlık adımları vardır. Başlamadan önce PowerShell Azure Stack üzerinde nasıl kullanıldığını ile hakkında bilgi edinmek isteyebilirsiniz. Daha fazla bilgi için [Azure Stack de PowerShell ile çalışmaya başlama](../user/azure-stack-powershell-overview.md).

Azure Stack, Azure Resource Manager, temel alınan dağıtımı, yönetimi ve kuruluş mekanizması olarak kullanır. Azure Stack yönetmek ve kullanıcıların desteklemeye yardımcı olmak için kullanacaksanız, Resource Manager hakkında bilgi edinebilirsiniz. Bkz: [Azure Resource Manager ile Başlarken](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) teknik incelemesi.

## <a name="your-typical-responsibilities"></a>Tipik sizin Sorumluluklarınız

Kullanıcıların hizmetleri kullanmak istiyorsunuz. Kendi bakış açısına ana rolünüz hizmetlerin kullanılabilir olmasını sağlamaktır. Hizmetleri sunmaya karar verin ve planlar, teklifler ve kotalar oluşturarak bu hizmetleri kullanılabilir yapın. Daha fazla bilgi için [Azure Stack'te hizmetleri sunan genel bakış](azure-stack-offer-services-overview.md). 

Öğe ekleme gerekecektir [Market](azure-stack-marketplace.md), sanal makine görüntüleri ister. En kolay yolu [Azure Stack için Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> Planlar, teklifler ve Hizmetleri test etmek isterseniz, kullanabileceğiniz [kullanıcı portalı](azure-stack-manage-portals.md); olmayan Yönetici portalı.

Hizmetleri sağlamaya ek olarak, Azure Stack çalışmaya tutmak için bir işleç normal görevlerini yapmanız gerekir. Bu görevler aşağıdakileri içerir:

- Kullanıcı hesaplarını ekleyin (için [Azure Active Directory](azure-stack-add-new-user-aad.md) dağıtım veya [Active Directory Federasyon Hizmetleri](azure-stack-add-users-adfs.md) dağıtım)
- [Rol tabanlı erişim denetimi (RBAC) Rolleri Ata](azure-stack-manage-permissions.md) (Bu yöneticilerine sınırlı değildir.)
- [Altyapı sistem durumunu izleme](azure-stack-monitor-health.md)
- Yönetme [ağ](azure-stack-viewing-public-ip-address-consumption.md) ve [depolama](azure-stack-manage-storage-accounts.md) kaynakları
- Hatalı donanım değiştirin; örneğin [hatalı bir diski değiştirme](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Kullanıcılarınıza söylemeniz gerekenler

Azure Stack'te hizmetler ile nasıl çalışılacağını ve ortamına bağlanmak nasıl tekliflere abone olma, kullanıcılarınıza gerekecektir. Özel belgeler yanı sıra kullanıcılara sağlamak istediğiniz kullanıcıları Azure Stack kullanıcı belgeleri sitesine yönlendirebilir.

**Azure Stack'te hizmetler ile nasıl çalışılacağını anlama**

Hizmetleri kullanın ve Azure Stack'te uygulamalar oluşturmak için önce kullanıcılarınızın anlamalısınız bilgisi yoktur. Örneğin, belirli PowerShell ve API sürüm gereksinimleri vardır. Ayrıca, bir Azure hizmeti ve Azure stack'teki eşdeğer hizmeti arasında bazı özellik farkları vardır. Kullanıcılarınızın aşağıdaki makaleleri gözden emin olun:

- [Önemli noktalar yer: Hizmetleri kullanarak veya Azure Stack için uygulamalar oluşturma](../user/azure-stack-considerations.md)
- [Azure Stack'te sanal makineler için dikkat edilmesi gerekenler](../user/azure-stack-vm-considerations.md)
- [Depolama: farklılıklar ve dikkat edilmesi gerekenler](../user/azure-stack-acs-differences.md)

Bilgiler Bu makaleler, bir Azure hizmeti ve Azure Stack arasındaki farklar özetlenmektedir. Bu, genel Azure belgeleri bir Azure hizmeti için kullanılabilir olan bilgileri sağlar.

**Bir kullanıcı olarak Azure Stack'e bağlanma**

ASDK ortamında bir kullanıcı ASDK konağına bağlanmak için Uzak Masaüstü kullanmıyorsa Azure Stack'e bağlanmak için bir sanal özel ağ (VPN) bağlantısı yapılandırabilirsiniz. Bkz: [Azure Stack'e bağlanma](../asdk/asdk-connect.md). 

Kullanıcılarınızın kattığını bilmek isteyecektir nasıl [kullanıcı portalı](../user/azure-stack-use-portal.md) veya PowerShell üzerinden bağlanma. Tümleşik sistemleri ortamında dağıtım Kullanıcı Portalı adresi değişir. Kullanıcılarınızın doğru URL ile sağlamanız gerekir.

PowerShell kullanarak, kullanıcıların Hizmetleri kullanabilmeniz için önce kaynak sağlayıcılarını kaydetme gerekebilir. (Bir kaynak sağlayıcısı, bir hizmet yönetir. Ağ kaynak sağlayıcısı sanal ağlar, ağ arabirimleri ve yük Dengeleyiciler gibi kaynaklara for example, yönetir.) Bunlar gerekir [yükleme](azure-stack-powershell-install.md) PowerShell [indirme](azure-stack-powershell-download.md) ek modüller ve [yapılandırma](../user/azure-stack-powershell-configure-user.md) (kaynak Sağlayıcısı kaydı içeren) PowerShell.

**Bir teklife abone olma**

Bir kullanıcı Hizmetleri kullanabilmeniz için önce bunlar gerekir [teklife abone olma](azure-stack-subscribe-plan-provision-vm.md) operatör oluşturduğunuz.

## <a name="where-to-get-support"></a>Nereden destek

> [!Note]  
> Azure Stack (pre-1905) önceki sürümler için destek bilgileri bulmak için bkz: [Yardım ve Destek önceki sürümleri (pre-1905) Azure Stack için](azure-stack-servicing-policy.md).

### <a name="integrated-systems"></a>Tümleşik sistemler

Tümleşik bir sistem için Eşgüdümlü yükseltme ve özgün ekipman üreticisi (OEM) donanım iş ortaklarımız ile Microsoft arasındaki çözümleme işlemi yoktur.

Bulut Hizmetleri sorun varsa, Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla destek sunulur. Yönetici portalını sağ üst köşesinde bulunan Yardım ve Destek simgesini (soru işareti) seçin ve ardından **Yardım + Destek** seçip **yeni destek isteği** altında**Destek** bölümü. Bir destek isteği açın.

Dağıtım, düzeltme eki ve güncelleştirme ile ilgili bir sorun varsa, donanım (alan değiştirebilen birim dahil) ve donanım markalı yazılımları, donanım yaşam döngüsü konak üzerinde çalışan yazılımı gibi ilk OEM donanım satıcınıza başvurun.

Dağıtım ile ilgili bir sorun varsa, düzeltme eki ve güncelleştirme, donanım (alan değiştirebilen birim dahil) ve donanım markalı yazılımları, donanım yaşam döngüsü konak üzerinde çalışan yazılımı gibi ilk OEM donanım satıcınıza başvurun. Diğer her şey için Microsoft CSS ile iletişime geçin.

### <a name="azure-stack-development-kit-asdk"></a>Azure Stack Geliştirme Seti (ASDK)

ASDK için de destek ile ilgili sorular sorabilirsiniz [Microsoft forumları](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Yönetici portalını sağ üst köşesinde bulunan Yardım ve Destek simgesini (soru işareti) seçin ve ardından **Yardım + Destek** seçip **MSDN Forumları** altında  **Destek** bölümü.  Forum sitesini açar. Bu Forum düzenli olarak izlenir. ASDK değerlendirme ortamı olduğundan, Microsoft CSS sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)