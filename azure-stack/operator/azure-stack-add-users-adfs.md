---
title: AD FS Azure Stack Kullanıcı ekleme | Microsoft Docs
description: Active Directory Federasyon Hizmetleri (AD FS) (AD FS) dağıtımları için Azure Stack kullanıcıları nasıl ekleyeceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2019
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 06/03/2019
ms.openlocfilehash: 4411290b075e105a827de8fb2c8295dfd84e3b50
ms.sourcegitcommit: e8f7fe07b32be33ef621915089344caf1fdca3fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70118642"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>AD FS Azure Stack Kullanıcı ekleme
**Active Directory Kullanıcıları ve bilgisayarları** ek bileşenini, kimlik sağlayıcısı olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanarak Azure Stack ortama ek kullanıcı eklemek için kullanabilirsiniz.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory Kullanıcıları ekle
> [!TIP]
> Bu örnek, varsayılan azurestack. Local ASDK Active Directory 'yi kullanır. 

1. Windows Yönetim araçlarına erişim sağlayan ve yeni bir Microsoft Yönetim Konsolu (MMC) açan bir hesap ile bilgisayarda oturum açın.
2. **Ek bileşen Ekle veya kaldır > dosya '** yı seçin.
3. **Active Directory Kullanıcıları ve bilgisayarları** > **azurestack. Local** > **Users**' ı seçin.
4. **Eylem** > yeniKullanıcı > ' yı seçin.
5. Yeni nesne-Kullanıcı ' da kullanıcı ayrıntıları sağlayın. **İleri**’yi seçin.
6. Bir parola sağlayın ve onaylayın.
7. Değerleri doldurmak için **İleri ' yi** seçin. Kullanıcıyı oluşturmak için **son** ' u seçin.


## <a name="next-steps"></a>Sonraki adımlar
[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)