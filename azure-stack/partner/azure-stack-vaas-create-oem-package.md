---
title: Azure Stack doğrulama en iyi yöntemleri | Microsoft Docs
description: Bu makalede, hizmet olarak doğrulama kullanımına yönelik en iyi yöntemler açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/28/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 2ee5a873a6281ed5d71db05b05abd15986043676
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167377"
---
# <a name="create-an-oem-package"></a>OEM paketi oluşturma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack OEM Uzantı paketi, dağıtımda kullanılmak üzere Azure Stack altyapısına ve güncelleştirme, genişletme ve alan değiştirme gibi işlemsel işlemlere yönelik OEM 'e özgü içeriğin eklendiği mekanizmadır.

## <a name="creating-the-package"></a>Paket oluşturuluyor

Oluşturulduktan ve doğrulandıktan sonra, OEM uzantısı paketi VaaS 'de kullanılabilir.  Devam etmeden önce, [OEM paketi oluşturma](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true)adımlarını tamamladığınızdan emin olun. Paket daha sonra paket doğrulama iş akışında oturum açmak için VaaS test sonuçlarıyla birlikte Microsoft 'a gönderilir. Aşağıdaki adımlar, oluşturulan dosyaların kullanabilen tek bir zip dosyasına nasıl paketlediğini ayrıntılandırır.

1. Paket için aşağıdaki içeriği tanımla:
    - Paket içeriğini içeren bir ZIP dosyası
    - Paket içeriğinin kökündeki Metadata. xml dosyasının içeriğiyle aynı olması gereken `oemMetadata.xml`adlı bir bildirim dosyası.

2. İçerik dosyalarını seçin ve içeriklerden bir ZIP dosyası oluşturun:

    ![ZIP dosya içerikleri](media/vaas-create-oem-package-1.png) ![öğe içeriklerini sıkıştır](media/vaas-create-oem-package-2.png)

3. Elde edilen dosyayı, tanımlamak için yeterince açıklayıcı olacak şekilde yeniden adlandırın.

## <a name="verifying-the-contents"></a>İçerik doğrulanıyor

ZIP dosyanızın yapısını doğrulamak için, dosyayı inceleyin ve alt klasör olmadığından emin olun. TLD 'nin daraltılmış içerikleri vardır. Geçerli bir paket yapısı aşağıda gösterilmiştir.
> [!IMPORTANT]
> İçerik yerine üst klasörü sıkıştırma işlemi, paket imzasının başarısız olmasına neden olur.

![Düzgün şekilde daraltılmış paket içeriği](media/vaas-create-oem-package-3.png)

ZIP dosyası artık paket doğrulama iş akışında VaaS 'ye yüklenebilir ve Microsoft tarafından imzalanmış olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [OEM paketini doğrulama](azure-stack-vaas-validate-oem-package.md)
