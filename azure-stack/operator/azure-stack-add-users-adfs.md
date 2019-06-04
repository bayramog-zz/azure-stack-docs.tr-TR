---
title: Azure Stack ADFS için kullanıcılar ekleyin | Microsoft Docs
description: Azure Stack, ADFS dağıtımları için kullanıcı ekleme hakkında bilgi edinin
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
ms.openlocfilehash: d50eb52de39c789498928a7b5e2227998872b937
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66458970"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>AD FS, Azure Stack kullanıcıları ekleme
Kullanabileceğiniz **Active Directory Kullanıcıları ve Bilgisayarları** AD FS, kimlik sağlayıcısı olarak kullanarak bir Azure Stack ortamınıza ek kullanıcılar eklemek için ek bileşenini.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory kullanıcısı Ekle
> [!TIP]
> Bu örnek, varsayılan azurestack.local ASDK active directory kullanır. 

1. Windows Yönetim Araçları için erişim sağlayan bir hesapla bir bilgisayarda oturum açın ve yeni bir Microsoft Yönetim Konsolu (MMC) açın.
2. Seçin **Dosya > Ekle veya Kaldır ek bileşenini**.
3. Seçin **Active Directory Kullanıcıları ve Bilgisayarları** > **AzureStack.local** > **kullanıcılar**.
4. Seçin **eylem** > **yeni** > **kullanıcı**.
5. Yeni nesne - kullanıcı, kullanıcı bilgilerini ayrıntıları sağlayın. **İleri**’yi seçin.
6. Sağlar ve bir parolayı onaylayın.
7. Seçin **sonraki** değerleri sonlandırmak için. Seçin **son** kullanıcı oluşturmak için.


## <a name="next-steps"></a>Sonraki adımlar
[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)