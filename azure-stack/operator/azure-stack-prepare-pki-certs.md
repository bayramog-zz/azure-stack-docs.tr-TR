---
title: Azure Stack tümleşik sistemler dağıtımı veya gizli anahtar için Azure Stack ortak anahtar altyapısı sertifikaları hazırlayın | Microsoft Docs
description: Azure Stack tümleşik sistemler için Azure Stack PKI sertifikalarının nasıl hazırlanacağını açıklar.
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
ms.date: 09/10/2019
ms.author: justinha
ms.reviewer: ppacent
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: 6b7d7a3e70d78b8ab943224babc515395319914e
ms.sourcegitcommit: 38f21e0bcf7b593242ad615c9d8ef8a1ac19c734
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70902729"
---
# <a name="prepare-azure-stack-pki-certificates-for-use-in-deployment-or-rotation"></a>Azure Stack PKI sertifikalarını dağıtımda veya dönüşte kullanılmak üzere hazırlama

[SEÇTIĞINIZ CA 'dan elde edilen](azure-stack-get-pki-certs.md) sertifika dosyaları içeri aktarılmalıdır ve Azure Stack sertifika gereksinimleriyle eşleşen özelliklerle birlikte verilmelidir.

## <a name="prepare-certificates-for-deployment"></a>Dağıtım için sertifikaları hazırlama

Yeni bir Azure Stack ortamı dağıtmak veya var olan bir Azure Stack ortamında gizli dizileri döndürmek için kullanılacak Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın. 


### <a name="import-the-certificate"></a>Sertifikayı içeri aktar

1. [SEÇTIĞINIZ CA 'dan elde edilen](azure-stack-get-pki-certs.md) özgün sertifika sürümlerini dağıtım ana bilgisayarındaki bir dizine kopyalayın. 
   > [!WARNING]
   > Doğrudan CA tarafından sunulan dosyalardan herhangi bir şekilde içeri aktarılmış, aktarılan veya değiştirilen dosyaları kopyalamayın.

1. Sertifikaya sağ tıklayın ve sertifikayı, sertifika YETKILINIZDEN nasıl teslim edildiğini bağlı olarak, **sertifikayı** veya **PFX 'yi yükler** ' i seçin.

1. **Sertifika Içeri aktarma sihirbazında**, içeri aktarma konumu olarak **yerel makine** ' yi seçin. **İleri**’yi seçin. Aşağıdaki ekranda İleri ' ye tıklayın.

    ![Yerel makine içeri aktarma konumu](./media/prepare-pki-certs/1.png)

1. **Tüm sertifikayı aşağıdaki depoya yerleştir** ' i seçin ve ardından konum olarak **Kurumsal güven** ' i seçin. **Tamam** ' a tıklayarak sertifika deposu seçimi iletişim kutusunu kapatın ve ardından **İleri**' ye tıklayın.

   ![Sertifika deposunu yapılandırma](./media/prepare-pki-certs/3.png)

   a. PFX 'yi içeri aktarıyorsanız, size ek bir iletişim kutusu gösterilir. **Özel anahtar koruma** sayfasında, sertifika dosyalarınızın parolasını girin ve ardından bu anahtarı dışarı aktarılabilir olarak **işaretle ' yi etkinleştirin. Bu sayede, anahtarlarınızı daha sonraki bir zamanda** yedekleyebilir veya taşıma seçeneğiniz vardır. **İleri**’yi seçin.

   ![Anahtarı verilebilir olarak işaretle](./media/prepare-pki-certs/2.png)

1. İçeri aktarmayı gerçekleştirmek için son ' a tıklayın.

> [!NOTE]
> Azure Stack bir sertifikayı içeri aktardıktan sonra, sertifikanın özel anahtarı kümelenmiş depolamada PKCS 12 dosyası (. pfx) olarak depolanır.

### <a name="export-the-certificate"></a>Sertifikayı dışarı aktarma

Certificate Manager MMC konsolunu açın ve yerel makine sertifika deposuna bağlanın.

1. Microsoft Yönetim Konsolu 'Nu açın, Windows 10 ' da Başlat menüsünde sağ tıklayın ve Çalıştır ' a tıklayın. **MMC** yazın, Tamam ' a tıklayın.

1. Dosya, ek bileşen Ekle/Kaldır ' a tıklayın ve ardından Sertifikalar ' ı seçin.

    ![Sertifika ek bileşeni ekleme](./media/prepare-pki-certs/mmc-2.png)
 
1. Bilgisayar hesabı ' nı seçin, ileri ' ye tıklayın ve sonra yerel bilgisayar ' ı seçin Ek bileşen Ekle/Kaldır sayfasını kapatmak için Tamam ' ı tıklatın.

    ![Sertifika ek bileşeni ekleme](./media/prepare-pki-certs/mmc-3.png)

1. > Kurumsal güven > sertifika konumunu sertifikalara inceleyin. Sertifikanızı sağda görmediğinizi doğrulayın.

1. Certificate Manager konsolunun görev çubuğundan **Eylemler** > **Tüm görevler** > **dışarı aktar**' ı seçin. **İleri**’yi seçin.

   > [!NOTE]
   > Kaç Azure Stack sertifikasına sahip olduğunuza bağlı olarak, bu işlemi birkaç kez gerçekleştirmeniz gerekebilir.

1. **Evet, özel anahtarı dışarı aktar**' ı seçin ve ardından **İleri**' ye tıklayın.

1. Dışarı aktarma dosyası biçimi bölümünde:
    
   - **Mümkünse, sertifikaya tüm sertifikaları dahil et '** i seçin.  
   - **Tüm genişletilmiş özellikleri dışarı aktar**' ı seçin.  
   - **Sertifika gizliliğini etkinleştir**' i seçin.  
   - **İleri**'ye tıklayın.  
    
     ![Seçili seçeneklerle sertifika dışarı aktarma Sihirbazı](./media/prepare-pki-certs/azure-stack-save-cert.png)

1. **Parola** ' yı seçin ve sertifikalar için bir parola belirtin. Aşağıdaki parola karmaşıklığı gereksinimlerini karşılayan bir parola oluşturun. En az sekiz karakter uzunluğunda. Parola şunlardan en az üçünü içerir: büyük harf, küçük harf, 0-9 arasındaki sayılar, özel karakterler, büyük harf veya küçük harf olmayan alfabetik karakter. Bu parolayı unutmayın. Bunu, bir dağıtım parametresi olarak kullanacaksınız.

1. **İleri**’yi seçin.

1. Dışarı aktarılacak pfx dosyası için bir dosya adı ve konum seçin. **İleri**’yi seçin.

1. **Son**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

[PKI sertifikalarını doğrulama](azure-stack-validate-pki-certs.md)