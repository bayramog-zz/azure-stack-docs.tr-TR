---
title: ASDK Yönetim Temelleri | Microsoft Docs
description: Azure Stack Geliştirme Seti (ASDK) için temel yönetici görevleri yapmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 2982012901b410b9e6e3830aa24fe4c62be8aa4c
ms.sourcegitcommit: 4eb1766c7a9d1ccb1f1362ae1211ec748a7d708c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69579052"
---
# <a name="asdk-admin-basics"></a>ASDK Yönetim Temelleri
Azure Stack Geliştirme Seti (ASDK) yönetimi için yeni başladığınızı bilmeniz gereken birkaç nokta vardır. Bu kılavuz, değerlendirme ortamında rolünüze bir Azure Stack işletmeni olarak bir genel bakış sağlar. Bu bilgiyle kendinizi alıştırarak, test kullanıcılarınızın mümkün olduğunca hızlı bir şekilde üretken hale gelmesini sağlar.

İlk olarak, ASDK 'nin amacını ve sınırlamalarını anladığınızdan emin olmak için [Azure Stack geliştirme seti nedir?](asdk-what-is.md) makalesini gözden geçirmeniz gerekir. Geliştirme setini, uygulamalarınızı üretim dışı bir ortamda geliştirmek ve test etmek için Azure Stack değerlendirebileceğiniz bir "korumalı alan" olarak kullanmanız gerekir. 

Azure gibi Azure Stack hızlı bir şekilde, ASDK 'nin yeni yapılarını düzenli olarak yayınlarız. Ancak, tümleşik sistem dağıtımlarını Azure Stack gibi, ASDK 'yi yükseltemezsiniz. Bu nedenle, en son yapıya geçmek istiyorsanız, [ASDK](asdk-redeploy.md)'yi tamamen yeniden dağıtmanız gerekir. Güncelleştirme paketlerini uygulayamazsınız. Bu işlem zaman alır, ancak en son özellikleri kullanılabilir duruma geldiğinde deneyebileceğiniz avantajdır. 

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure Stack yönetirken dikkat etmeniz gereken birkaç hesap dikkate alınması gerekir. Bu özellikle, Azure Active Directory (Azure AD) yerine kimlik sağlayıcısı olarak Windows Server Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanan dağıtımlar için geçerlidir. Azure Stack tümleşik sistemler ve ASDK dağıtımları için aşağıdaki hesap değerlendirmeleri geçerlidir:

|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (.\administrator)|ASDK konak Yöneticisi|ASDK konak Yöneticisi|
|AzureStack\AzureStackAdmin|ASDK konak Yöneticisi<br><br>Azure Stack yönetici portalında oturum açmak için kullanılabilir<br><br>Service Fabric halkalarını görüntüleme ve yönetmeye erişme|ASDK konak Yöneticisi<br><br>Azure Stack yönetici portalına erişim yok<br><br>Service Fabric halkalarını görüntüleme ve yönetmeye erişme<br><br>Artık varsayılan sağlayıcı Aboneliğinin sahibi yok (DPS)|
|AzureStack\CloudAdmin|Ayrıcalıklı uç nokta içinde izin verilen komutlara erişebilir ve bunları çalıştırabilir|Ayrıcalıklı uç nokta içinde izin verilen komutlara erişebilir ve bunları çalıştırabilir<br><br>ASDK konağında oturum açılamıyor<br><br>Varsayılan sağlayıcı Aboneliğinin sahibi (DPS)|
|Azure AD Genel Yöneticisi|Yükleme sırasında kullanılır<br><br>Varsayılan sağlayıcı Aboneliğinin sahibi (DPS)|Geçerli değil|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi araçları kullanabilirim?
Azure Stack yönetmek için [Azure Stack yönetim portalını](https://adminportal.local.azurestack.external) veya PowerShell 'i kullanabilirsiniz. Temel kavramları öğrenmenin en kolay yolu Portal üzerinden yapılır. PowerShell 'i kullanmak istiyorsanız, [Azure Stack Için PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) 'i yüklemeniz ve [Azure Stack araçlarını GitHub 'dan indirmeniz](asdk-post-deploy.md#download-the-azure-stack-tools)gerekir.

Azure Stack, temel aldığı dağıtım, yönetim ve kuruluş mekanizması olarak Azure Resource Manager kullanır. Azure Stack yönetiyoruz ve kullanıcıları desteklemeye yardımcı olmaya devam ediyorsanız, Azure Resource Manager hakkında bilgi almanız gerekir. [Azure Resource Manager Ile çalışmaya başlama teknik](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf)incelemesini okuyarak daha fazla bilgi edinebilirsiniz.

## <a name="your-typical-responsibilities"></a>Tipik sorumluluklarınız
Kullanıcılarınız hizmetleri kullanmak istiyor. Perspektifinden, ana rolünüzün bu hizmetleri kendileri için kullanılabilir hale getirmek gerekir. ASDK 'yi kullanarak hangi hizmetlerin sunduğunu ve [planlar, teklifler ve Kotalar oluşturarak](../operator/azure-stack-tutorial-tenant-vm.md)bu hizmetleri nasıl kullanılabilir hale ayarlayacağınızı öğrenebilirsiniz. Ayrıca, sanal makine (VM) görüntüleri gibi Market 'e öğe eklemeniz gerekir. En kolay yol, [Market öğelerini](../operator/azure-stack-create-and-publish-marketplace-item.md) Azure 'dan Azure Stack 'e indirmenin en kolay yoludur.

> [!NOTE]
> Planlarınızı, tekliflerinizi ve hizmetlerinizi test etmek isterseniz, [Kullanıcı portalını](https://portal.local.azurestack.external)kullanmanız gerekir; [Yönetici portalı](https://adminportal.local.azurestack.external)değildir.

Hizmetleri sağlamaya ek olarak, bir Azure Stack Işlecinin tüm olağan görevlerini gerçekleştirerek, bu da ASDK çalışır durumda tutun. Bu görevler aşağıdakileri içerir:
- Azure AD veya AD FS dağıtımları için Kullanıcı hesapları ekleyin.
- Rol tabanlı erişim denetimi (RBAC) rolleri atayın (Bu yalnızca yöneticilerle sınırlı değildir).
- Altyapı durumunu izleyin.
- Ağ ve depolama kaynaklarını yönetme.
- Başarısız olan geliştirme seti Konak bilgisayar donanımını değiştirin.

## <a name="where-to-get-support"></a>Nereden destek alınacağı
ASDK için, [Azure Stack MSDN forumundaki](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)destekle ilgili sorularınızı sorabilirsiniz.

Ayrıca, yönetim portalının sağ üst köşesindeki **Yardım** (soru işareti) seçeneğine tıklayarak forumlara erişebilirsiniz. Ardından Yardım + destek ' e tıklayarak yardım + destek' i açın ve bu da forumuna bir bağlantı içerir. MSDN forumları düzenli olarak izlenir.  

> [!IMPORTANT]
> ASDK bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri (CSS) ile sunulan resmi bir destek yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[ASDK 'yi dağıtma](asdk-install.md)

