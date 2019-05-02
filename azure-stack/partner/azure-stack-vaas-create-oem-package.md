---
title: Azure Stack doğrulama en iyi yöntemler | Microsoft Docs
description: Bu makalede, bir hizmet olarak doğrulama kullanmak için en iyi uygulamalar açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 568817c6e25952f15a396e5748d314187345945b
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "64298673"
---
# <a name="create-an-oem-package"></a>OEM paketi oluşturma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack OEM uzantı paketi, hangi OEM tarafından kullanılmak üzere güncelleştirme genişletme ve alan değiştirme gibi işletimsel işlemlerin yanı sıra, dağıtım, Azure Stack altyapısının belirli içerik eklenir yönelik mekanizmadır.

## <a name="creating-the-package"></a>Paket oluşturuluyor

Oluşturup doğrulanmış OEM uzantı paketi VaaS içinde kullanılabilir.  Devam etmeden önce adımları tamamladığınızdan emin olun [OEM paket oluşturma](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true). Paket imzalama paket doğrulama iş akışı içinde VaaS test sonuçları ile birlikte Microsoft sonra gönderilir. Aşağıdaki adımlarda oluşturulan dosyalarının VaaS tüketebileceği bir tek bir ZIP dosyasına nasıl gruplanacağını ayrıntılı olarak açıklanmaktadır.

1. Aşağıdaki içerik paketi için tanımlayın:
    - Paket içeriğini içeren bir zip dosyası
    - Adlı bir bildirim dosyası `oemMetadata.xml`, olacağı paket içeriğini kökünde metadata.xml dosyaya içerik olarak aynı.

2. İçerik dosyaları seçin ve içeriğini bir zip dosyası oluşturun:

    ![ZIP dosyasının içeriğini](media/vaas-create-oem-package-1.png) ![öğesi içeriği sıkıştır](media/vaas-create-oem-package-2.png)

3. Sonuç dosyası tanımlamak için açıklayıcı kadar olacak şekilde yeniden adlandırın.

## <a name="verifying-the-contents"></a>İçeriği doğrulanıyor

Zip dosyanızın yapısı doğrulamak için bu inceleme ve hiçbir alt klasör olduğunu kontrol edin. Sıkıştırılmış içerik TLD sahiptir. Geçerli paket yapısı aşağıda gösterilmiştir.
> [!IMPORTANT]
> Üst klasör içeriklerini yerine sıkıştırma paketi imzalama başarısız olmasına neden olur.

![Düzgün sıkıştırılmış paket içeriği](media/vaas-create-oem-package-3.png)

Zip dosyası artık VaaS için karşıya yüklenen ve paket doğrulama iş akışında Microsoft tarafından imzalanmış.

## <a name="next-steps"></a>Sonraki adımlar

- [OEM paketi doğrulama](azure-stack-vaas-validate-oem-package.md)
