---
title: Azure Stack tümleşik sistemler için Azure bağlantısı kesildi dağıtım kararları | Microsoft Docs
description: Azure Stack tümleşik sistemlerin Azure bağlantısı kesik dağıtımı ve göz önünde bulundurulması gereken planlama kararları hakkında bilgi edinin.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 61d938ef1a3039c4b685c4df8f09577d9c7d3f50
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909280"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemler için Azure bağlantısı kesik dağıtım planlama kararları
[Karma bulut ortamınıza Azure Stack nasıl tümleştirileceğini](azure-stack-connection-models.md)karardıktan sonra, Azure Stack dağıtım kararlarınızı bitirebileceksiniz.

Azure Stack, internet bağlantısı olmadan dağıtabilir ve kullanabilirsiniz. Ancak, bağlantısı kesilen bir dağıtımla Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kimlik deposu ve kapasite tabanlı faturalandırma modeliyle sınırlı olursunuz. Çoklu Kiralama Azure Active Directory (Azure AD) kullanımını gerektirdiğinden, bağlantısı kesilen dağıtımlar için Çoklu Kiralama desteklenmez.

Şu durumlarda bu seçeneği belirleyin:
- İnternet 'e bağlı olmayan bir ortamda Azure Stack dağıtmanızı gerektiren güvenlik veya başka kısıtlamalar vardır.
- Verilerin (kullanım verileri dahil) Azure 'a gönderilmesini engellemek istiyorsunuz.
- Yalnızca kurumsal intranetinize dağıtılan ve Karma senaryolar ile ilgilenmeyen özel bir bulut çözümü olarak Azure Stack kullanmak istersiniz.

> [!TIP]
> Bazen, bu tür bir ortam de bir *alt deniz senaryosu*olarak adlandırılır.

Bağlantısı kesilen bir dağıtım, karma Kiracı VM senaryoları için Azure Stack örneğinizi daha sonra Azure 'a bağlanmanızı kısıtlayamaz. Bu, dağıtım sırasında Azure bağlantınız olmadığı veya Azure AD 'yi kimlik deposu olarak kullanmak istemediğiniz anlamına gelir.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Bağlantısı kesilmiş dağıtımlarda Engelli veya kullanılamayan özellikler 
Azure Stack Azure 'a bağlıyken en iyi şekilde çalışacak şekilde tasarlanmıştır. bu nedenle, bağlantısı kesik modda çalışmayan veya tamamen kullanılamayan bazı özellikler ve işlevler olduğunu unutmayın.

|Özellik|Bağlantısı kesik modda etki|
|-----|-----|
|VM dağıtım sonrası dağıtımı yapılandırmak için DSC uzantılı VM dağıtımı|Yerinde-DSC Uzantısı, en son WMF için internet 'e bakar.|
|Docker komutlarını çalıştırmak için Docker uzantısıyla VM dağıtımı|Engelli-Docker, en son sürüm için internet 'i denetlecektir ve bu denetim başarısız olur.|
|Azure Stack portalındaki belge bağlantıları|Kullanılamıyor-internet URL 'SI kullanan geri bildirim, yardım ve hızlı başlangıç gibi bağlantılar çalışmayacak.|
|Çevrimiçi düzeltme kılavuzuna başvuruda bulunan uyarı düzeltme/azaltma|Kullanılamıyor-internet URL 'SI kullanan herhangi bir uyarı düzeltme bağlantısı çalışmaz.|
|Market-Galeri paketlerini doğrudan Azure Marketi 'nden seçme ve ekleme özelliği|Engelliler-Azure Stack, bağlantısı kesik modda dağıtırken Market öğelerini Azure Stack portalını kullanarak indiremez. Ancak, Market öğelerini internet bağlantısı olan bir makineye indirmek ve sonra Azure Stack ortamınıza aktarmak için [Market dağıtım aracı](azure-stack-download-azure-marketplace-item.md) 'nı kullanabilirsiniz.|
|Azure Stack dağıtımını yönetmek için Azure AD Federasyon hesaplarını kullanma|Kullanılamıyor-bu özellik Azure bağlantısı gerektiriyor. Bunun yerine yerel bir Active Directory örneği ile AD FS kullanılmalıdır.|
|Uygulama Hizmetleri|Engelliler-WebApps, güncelleştirilmiş içerik için internet erişimi gerektirebilir.|
|Komut Satırı Arabirimi (CLI)|Engelli CLı, hizmet sorumlularının kimlik doğrulaması ve sağlama işlevlerini düşürür.|
|Visual Studio-bulut bulma|Engelli-Cloud Discovery, farklı bulutlar bulacak veya hiç çalışmayacak.|
|Visual Studio-AD FS|Yalnızca Engelliler Visual Studio Enterprise ve AD FS kimlik doğrulamasını destekler Visual Studio Code.
Telemetri|Kullanım dışı-telemetri verilerine bağlı olan Azure Stack ve üçüncü taraf Galeri paketleri için telemetri verileri.|
|Sertifikalar|Kullanılamıyor-HTTPS bağlamında sertifika Iptal listesi (CRL) ve çevrimiçi sertifika durumu Protokolü (OSCP) Hizmetleri için internet bağlantısı gereklidir.|
|Key Vault|Engelli-Key Vault için ortak kullanım örneği, çalışma zamanında bir uygulamanın gizli dizileri okumalıdır. Bu kullanım örneği için, uygulamanın dizinde bir hizmet sorumlusu olması gerekir. Azure AD 'de, normal kullanıcılar (yönetici olmayanlar) varsayılan olarak hizmet sorumlusu eklemesine izin verilir. Azure AD 'de (AD FS kullanarak), bunlar değildir. Bu, her zaman bir uygulama eklemek için her zaman bir dizin Yöneticisi üzerinden gitmesi gerektiğinden, bu, uçtan uca deneyimde bir yere yerleştirmelidir.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfasına bakın.
- Azure Stack tümleşik sistemler için yol haritası ve coğrafi kullanılabilirlik hakkında daha fazla bilgi için, bkz. Teknik İnceleme: [Azure Stack: Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)uzantısı. 
- Microsoft Azure Stack paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [. PDF dosyasını indirin](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri merkezi ağ tümleştirmesi](azure-stack-network.md)
