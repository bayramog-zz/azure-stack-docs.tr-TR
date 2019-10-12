---
title: Azure Stack Yönetim Temelleri | Microsoft Docs
description: Azure Stack yönetimi hakkında temel bilgileri öğrenin.
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
ms.openlocfilehash: f9b9d6d1474c22c8e31d24ae08faf1aac6d5e9c1
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72283491"
---
# <a name="azure-stack-administration-basics"></a>Azure Stack Yönetim Temelleri

Azure Stack yönetimi için yeni başladıysanız bilmeniz gereken birkaç şey vardır. Bu makalede, rolünüze bir Azure Stack operatörü olarak bir genel bakış sağlanır ve kullanıcılarınıza üretken olmalarını sağlamak için gerekenler sunulmaktadır.

## <a name="understand-the-builds"></a>Derlemeleri anlama

### <a name="integrated-systems"></a>Tümleşik sistemler

Azure Stack tümleşik bir sistem kullanıyorsanız güncelleştirme paketleri, Azure Stack güncelleştirilmiş sürümlerini dağıtır. Bu paketleri içeri aktarabilir ve yönetici portalındaki **güncelleştirmeler** kutucuğunu kullanarak uygulayabilirsiniz.
 
### <a name="development-kit"></a>Geliştirme Seti

Azure Stack Geliştirme Seti (ASDK) kullanıyorsanız, ASDK 'nin amacını ve kısıtlamalarını öğrenmek için [Azure Stack nedir?](../asdk/asdk-what-is.md) seçeneğini gözden geçirin. ASDK 'yi bir *korumalı alan*olarak kullanabilirsiniz; burada Azure Stack değerlendirebilir ve uygulamalarınızı üretim dışı bir ortamda geliştirebilir ve test edebilirsiniz. Dağıtım bilgileri için bkz. [dağıtım Azure Stack geliştirme seti](../asdk/asdk-install.md).

Azure gibi hızla yenilik yapın. Yeni yapıları düzenli olarak yayımlayacaktır. ASDK çalıştırıyorsanız ve en son yapıya taşımak istiyorsanız, [Azure Stack yeniden dağıtmanız](../asdk/asdk-redeploy.md)gerekir. Güncelleştirme paketlerini uygulayamazsınız. Bu işlem zaman alır, ancak en son özellikleri deneyebilmeniz avantajıdır. Web sitemizdeki ASDK belgeleri en son sürüm derlemesini yansıtır.

## <a name="learn-about-available-services"></a>Kullanılabilir hizmetler hakkında bilgi edinin

Kullanıcılarınız için kullanabileceğiniz Hizmetleri bilmeniz gerekir. Azure Stack, Azure hizmetlerinin bir alt kümesini destekler. Desteklenen hizmetlerin listesi gelişmeye devam edecektir.

**Temel hizmetler**

Varsayılan olarak, Azure Stack Azure Stack dağıtırken aşağıdaki "temel hizmetler" i içerir:

- Hesaplamasını
- Depolama
- Ağ Oluşturma
- Key Vault

Bu temel hizmetlerle, en az yapılandırmayla kullanıcılarınıza hizmet olarak altyapı (IaaS) sunabilirsiniz.

**Ek hizmetler**

Şu anda aşağıdaki ek hizmet olarak platform (PaaS) hizmetlerini destekliyoruz:

- App Service
- Azure İşlevleri
- SQL ve MySQL veritabanları
- Kubernetes (önizlemede)

Bu hizmetler, kullanıcılarınız için kullanılabilir hale gelmeden önce ek yapılandırma gerektirir. Daha fazla bilgi için, Azure Stack operatör belgelerimizin "öğreticiler" ve "nasıl yapılır Kılavuzu \ teklif Hizmetleri" bölümlerine bakın.

**Hizmet yol haritası**

Azure Stack Azure hizmetleri için destek eklemeye devam edecektir. Tasarlanan yol haritası için bkz. [Azure Stack: Azure](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) teknik incelemesi uzantısı. Ayrıca, yeni duyurular için [Azure Stack blog gönderilerini](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) izleyebilirsiniz.

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure Stack yönetirken bilinmesi gereken birkaç hesap konusu vardır. Özellikle, Azure Active Directory (Azure AD) yerine kimlik sağlayıcısı olarak Windows Server Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanan dağıtımlarda. Azure Stack tümleşik sistemler ve ASDK dağıtımları için aşağıdaki hesap değerlendirmeleri geçerlidir:


|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (.\administrator)|ASDK konak Yöneticisi.|ASDK konak Yöneticisi.|
|AzureStack\AzureStackAdmin|ASDK konak Yöneticisi.<br><br>, Azure Stack yönetici portalında oturum açmak için kullanılabilir.<br><br>Service Fabric halkalarını görüntüleme ve yönetme erişimi.|ASDK konak Yöneticisi.<br><br>Azure Stack yönetici portalına erişim yok.<br><br>Service Fabric halkalarını görüntüleme ve yönetme erişimi.<br><br>Artık varsayılan sağlayıcı aboneliğinin (DPS) sahibi yok.|
|AzureStack\CloudAdmin|Ayrıcalıklı uç nokta içinde izin verilen komutlara erişebilir ve bunları çalıştırabilir.|Ayrıcalıklı uç nokta içinde izin verilen komutlara erişebilir ve bunları çalıştırabilir.<br><br>ASDK konağında oturum açılamıyor.<br><br>Varsayılan sağlayıcı aboneliğinin (DPS) sahibi.|
|Azure AD Genel Yöneticisi|Yükleme sırasında kullanılır.<br><br>Varsayılan sağlayıcı aboneliğinin (DPS) sahibi.|Yok.|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi araçları kullanabilirim?
 
Azure Stack yönetmek için [Yönetici portalını](azure-stack-manage-portals.md) veya PowerShell 'i kullanabilirsiniz. Temel kavramları öğrenmenin en kolay yolu Portal üzerinden yapılır. PowerShell 'i kullanmak istiyorsanız, hazırlık adımları vardır. Başlamadan önce, PowerShell 'in Azure Stack nasıl kullanıldığını öğrenmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Azure Stack PowerShell ile çalışmaya başlama](../user/azure-stack-powershell-overview.md).

Azure Stack, temel aldığı dağıtım, yönetim ve kuruluş mekanizması olarak Azure Resource Manager kullanır. Azure Stack yönetiyoruz ve kullanıcıları desteklemeye yardımcı olmaya devam ediyorsanız, Kaynak Yöneticisi hakkında bilgi edinebilirsiniz. [Azure Resource Manager Ile çalışmaya](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) başlama hakkında bilgi için bkz. Teknik İnceleme.

## <a name="your-typical-responsibilities"></a>Tipik sorumluluklarınız

Kullanıcılarınız hizmetleri kullanmak istiyor. Perspektifinden, ana rolünüzün bu hizmetleri kendileri için kullanılabilir hale getirmek gerekir. Hangi hizmetlere sunabileceğinize karar verin ve planlar, teklifler ve Kotalar oluşturarak bu hizmetleri kullanılabilir hale getirin. Daha fazla bilgi için bkz. [Azure Stack hizmetleri sunma konusuna genel bakış](service-plan-offer-subscription-overview.md). 

Ayrıca, [Azure Stack Market](azure-stack-marketplace.md)'e öğe eklemeniz gerekir. En kolay yol, [Market öğelerini Azure 'dan Azure Stack 'e indirmenin](azure-stack-download-azure-marketplace-item.md)en kolay yoludur.

> [!NOTE]
> Planlarınızı, tekliflerinizi ve hizmetlerinizi test etmek isterseniz, [Kullanıcı portalını](azure-stack-manage-portals.md)kullanabilirsiniz; Yönetici portalı değildir.

Hizmet sağlamaya ek olarak, Azure Stack çalışır durumda tutmak için bir işlecin düzenli görevlerini yapmanız gerekir. Bu görevler aşağıdaki görevleri içerir:

- Kullanıcı hesapları ekleyin ( [Azure AD](azure-stack-add-new-user-aad.md) dağıtımı için veya [AD FS](azure-stack-add-users-adfs.md) dağıtımı için).
- [Rol tabanlı erişim denetimi (RBAC) rolleri atama](azure-stack-manage-permissions.md) (Bu görev Yöneticiler ile sınırlı değildir.)
- [Altyapı durumunu izleyin](azure-stack-monitor-health.md).
- [Ağ](azure-stack-viewing-public-ip-address-consumption.md) ve [depolama](azure-stack-manage-storage-accounts.md) kaynaklarını yönetme.
- Hatalı donanımı değiştirme. Örneğin, [Hatalı bir diski değiştirin](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Kullanıcılarınıza söylemeniz gerekenler

Kullanıcılarınıza Azure Stack hizmetlerle nasıl çalışabileceklerini, ortama nasıl bağlanabileceğinizi ve tekliflere nasıl abone olacağınızı bilmeleri gerekir. Kullanıcılarınıza sağlamak isteyebileceğiniz özel belgelerin yanı sıra, kullanıcıları [Azure Stack kullanıcı belgelerine](https://docs.microsoft.com/en-us/azure-stack/user/)yönlendirebilirsiniz.

**Azure Stack 'de hizmetlerle nasıl çalışacağınızı anlayın**

Kullanıcılarınızın Hizmetleri kullanmadan önce anlaması gereken bilgiler ve Azure Stack uygulamalar oluşturun. Örneğin, belirli PowerShell ve API sürümü gereksinimleri vardır. Ayrıca, Azure 'daki bir hizmet ve Azure Stack ' deki eşdeğer hizmet arasında bazı özellikler değişimleri vardır. Kullanıcılarınızın aşağıdaki makaleleri gözden geçirdiğinizden emin olun:

- [Önemli konular: Hizmetleri kullanma veya Azure Stack için uygulama oluşturma](../user/azure-stack-considerations.md)
- [Azure Stack 'daki sanal makinelere yönelik konular](../user/azure-stack-vm-considerations.md)
- [Depolama: farklar ve konular](../user/azure-stack-acs-differences.md)

Bu makalelerdeki bilgiler, Azure 'daki bir hizmet ve Azure Stack arasındaki farkları özetler. Küresel Azure belgelerindeki bir Azure hizmeti için kullanılabilen bilgileri tamamlar.

**Azure Stack Kullanıcı olarak bağlanma**

Bir ASDK ortamında, bir Kullanıcı, ASDK konağına bağlanmak için Uzak Masaüstü kullanmıyorsa, Azure Stack bağlanmak için bir sanal özel ağ (VPN) bağlantısı yapılandırabilirler. Bkz. [Connect Azure Stack](../asdk/asdk-connect.md).

Kullanıcılarınız [Kullanıcı portalına nasıl erişebileceklerini](../user/azure-stack-use-portal.md) veya PowerShell üzerinden nasıl bağlanabileceğinizi öğrenmek ister. Tümleşik sistemler ortamında, Kullanıcı Portalı adresi dağıtım başına farklılık gösterir. Kullanıcılarınıza doğru URL 'yi sağlamanız gerekir.

PowerShell kullanılıyorsa, kullanıcıların hizmetleri kullanabilmesi için önce kaynak sağlayıcılarını kaydetmesi gerekebilir. Bir kaynak sağlayıcısı bir hizmeti yönetir. Örneğin, ağ kaynak sağlayıcısı, sanal ağlar, ağ arabirimleri ve yük dengeleyiciler gibi kaynakları yönetir. PowerShell [yüklemeleri](azure-stack-powershell-install.md) , ek modüller [indirmesi](azure-stack-powershell-download.md) ve PowerShell 'i (kaynak sağlayıcısı kaydını da içerir) [yapılandırması](../user/azure-stack-powershell-configure-user.md) gerekir.

**Teklife abone olma**

Bir kullanıcının Hizmetleri kullanabilmesi için, bir operatör olarak oluşturduğunuz [bir teklife abone](azure-stack-subscribe-plan-provision-vm.md) olmaları gerekir.

## <a name="where-to-get-support"></a>Nereden destek alınacağı

> [!Note]  
> Azure Stack (Pre-1905) sürümleri için destek bilgilerini bulmak için bkz. [önceki sürümler Için yardım ve destek Azure Stack (önceden 1905)](azure-stack-servicing-policy.md).

### <a name="integrated-systems"></a>Tümleşik sistemler

Tümleşik bir sistem için, Microsoft ve özgün ekipman üreticisi (OEM) donanım iş ortakları arasında eşgüdümlü bir ilerletme ve çözüm süreci vardır.

Bir bulut hizmetleri sorunu varsa, destek Microsoft Müşteri Destek Hizmetleri (CSS) aracılığıyla sunulur. Bir destek isteği açmak için, yönetici portalının sağ üst köşesindeki Yardım ve destek simgesini (soru işareti) seçin, **Yardım + Destek**' i seçin ve ardından **destek** bölümünde **Yeni destek isteği** ' ni seçin.

Dağıtım, düzeltme eki ve güncelleştirme, donanım yaşam döngüsü ana bilgisayarında çalışan yazılımlar gibi donanım markalı yazılımlar gibi bir sorun varsa, önce OEM donanım satıcınıza başvurun.

Başka bir şey için Microsoft CSS ile iletişim kurun.

### <a name="azure-stack-development-kit-asdk"></a>Azure Stack Geliştirme Seti (ASDK)

ASDK için, [Microsoft forumlarında](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)destekle ilgili sorularınızı sorabilirsiniz. Forumlara ulaşmak için yönetici portalının sağ üst köşesindeki Yardım ve destek simgesini (soru işareti) seçin, sonra **Yardım + Destek**' i seçin ve ardından **destek** bölümünün altındaki **MSDN forumları** ' nı seçin. Bu Forumlar düzenli olarak izlenir. ASDK bir değerlendirme ortamı olduğundan, Microsoft CSS aracılığıyla sunulan resmi bir destek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack 'de bölge yönetimi](azure-stack-region-management.md)