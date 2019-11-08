---
title: Dağıtım veya döndürme için Azure Stack PKI sertifikalarını hazırlama | Microsoft Docs
titleSuffix: Azure Stack
description: Azure Stack tümleşik sistem dağıtımı için PKI sertifikalarını hazırlama veya var olan bir Azure Stack ortamında gizli dizileri döndürme hakkında bilgi edinin.
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
ms.date: 09/16/2019
ms.author: justinha
ms.reviewer: ppacent
ms.lastreviewed: 09/16/2019
ms.openlocfilehash: a63e0e3a2246cc3c3c659f9671afdf4be0cc93cd
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802382"
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment-or-rotation"></a>Dağıtım veya döndürme için Azure Stack PKI sertifikalarını hazırlama

[Seçtiğiniz Sertifika yetkilinizden (CA) alınan](azure-stack-get-pki-certs.md) sertifika dosyaları, Azure Stack sertifika gereksinimleriyle eşleşen özelliklerle içeri ve içeri aktarılmalıdır.

## <a name="prepare-certificates-for-deployment"></a>Dağıtım için sertifikaları hazırlama

Yeni bir Azure Stack ortamı dağıtmak veya var olan bir Azure Stack ortamında gizli dizileri döndürmek için kullanılacak Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın.

### <a name="import-the-certificate"></a>Sertifikayı içeri aktar

1. [SEÇTIĞINIZ CA 'dan elde edilen](azure-stack-get-pki-certs.md) özgün sertifika sürümlerini dağıtım ana bilgisayarındaki bir dizine kopyalayın. 
   > [!WARNING]
   > Doğrudan CA tarafından sunulan dosyalardan herhangi bir şekilde içeri aktarılmış, aktarılan veya değiştirilen dosyaları kopyalamayın.

1. Sertifikanın CA 'nızdan nasıl teslim edildiğini bağlı olarak sertifikaya sağ tıklayın ve **sertifikayı** aç veya **PFX 'i yüklensin**' i seçin.

1. **Sertifika Içeri aktarma sihirbazında**, içeri aktarma konumu olarak **yerel makine** ' yi seçin. **İleri**’yi seçin. Aşağıdaki ekranda, ileri ' yi seçin.

    ![Sertifika için yerel makine içeri aktarma konumu](./media/prepare-pki-certs/1.png)

1. **Tüm sertifikayı aşağıdaki depoya yerleştir** ' i seçin ve ardından konum olarak **Kurumsal güven** ' i seçin. Sertifika deposu seçimi iletişim kutusunu kapatmak için **Tamam** ' ı seçin ve ardından **İleri**' yi seçin.

   ![Sertifika içeri aktarma için sertifika deposunu yapılandırma](./media/prepare-pki-certs/3.png)

   a. PFX 'yi içeri aktarıyorsanız, ek bir iletişim kutusu gösterilir. **Özel anahtar koruma** sayfasında, sertifika dosyalarınızın parolasını girin ve ardından **Bu anahtarı dışarı aktarılabilir olarak işaretle ' yi etkinleştirin. Bu sayede, anahtarlarınızı daha sonraki bir zamanda yedekleyebilir veya taşıma** seçeneğiniz vardır. **İleri**’yi seçin.

   ![Anahtarı verilebilir olarak işaretle](./media/prepare-pki-certs/2.png)

1. İçeri aktarmayı gerçekleştirmek için **son** ' u seçin.

> [!NOTE]
> Azure Stack bir sertifikayı içeri aktardıktan sonra, sertifikanın özel anahtarı kümelenmiş depolamada PKCS 12 dosyası (PFX) olarak depolanır.

### <a name="export-the-certificate"></a>Sertifikayı dışarı aktarma

Certificate Manager MMC konsolunu açın ve yerel makine sertifika deposuna bağlanın.

1. Microsoft Yönetim Konsolu 'nu açın. Konsolu Windows 10 ' da açmak için **Başlat menüsüne**sağ tıklayın, **Çalıştır**' ı seçin ve ardından **MMC** yazın ve ENTER tuşuna basın.

2. **Ek bileşen ekle/kaldır** > **Dosya** ' yı seçin ve ardından **Sertifikalar** ' ı seçip **Ekle**' yi seçin.

    ![Microsoft Yönetim Konsolu 'nda Sertifika ek bileşeni ekleme](./media/prepare-pki-certs/mmc-2.png)

3. **Bilgisayar hesabı**' nı seçin ve ardından **İleri**' yi seçin. **Yerel bilgisayar** ' ı seçin ve ardından **sona**. Ek bileşen Ekle/Kaldır sayfasını kapatmak için **Tamam ' ı** seçin.

    ![Microsoft Yönetim Konsolu 'nda sertifika ekle ek bileşeni için hesap seçin](./media/prepare-pki-certs/mmc-3.png)

4.  > **Kurumsal güven** > **sertifika konumunu** **sertifikalara** inceleyin. Sertifikanızı sağda görmediğinizi doğrulayın.

5. Sertifika Yöneticisi konsol görev çubuğundan **Eylemler** ' i > **Tüm görevler** > **dışarı aktar**' ı seçin. **İleri**’yi seçin.

   > [!NOTE]
   > Kaç Azure Stack sertifikaya sahip olduğunuza bağlı olarak, bu işlemi birden çok kez gerçekleştirmeniz gerekebilir.

6. **Evet, özel anahtarı dışarı aktar**' ı seçin ve ardından **İleri**' ye tıklayın.

7. Dışarı aktarma dosyası biçimi bölümünde:
    
   - **Mümkünse, sertifikaya tüm sertifikaları dahil et '** i seçin.  
   - **Tüm genişletilmiş özellikleri dışarı aktar**' ı seçin.  
   - **Sertifika gizliliğini etkinleştir**' i seçin.  
   - **İleri**’ye tıklayın.  
    
     ![Seçili seçeneklerle sertifika dışarı aktarma Sihirbazı](./media/prepare-pki-certs/azure-stack-save-cert.png)

8. **Parola** ' yı seçin ve sertifikalar için bir parola belirtin. Aşağıdaki parola karmaşıklığı gereksinimlerini karşılayan bir parola oluşturun:

    * En az sekiz karakter uzunluğunda.
    * Aşağıdakilerden en az üçünü: büyük harf, küçük harf, 0-9 arasındaki sayılar, özel karakterler, büyük harf veya küçük harf olmayan alfabetik karakter.

    Bu parolayı unutmayın. Bunu bir dağıtım parametresi olarak kullanacaksınız.

9. **İleri**’yi seçin.

10. Dışarı aktarılacak PFX dosyası için bir dosya adı ve konum seçin. **İleri**’yi seçin.

11. **Son**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

[PKI sertifikalarını doğrulama](azure-stack-validate-pki-certs.md)