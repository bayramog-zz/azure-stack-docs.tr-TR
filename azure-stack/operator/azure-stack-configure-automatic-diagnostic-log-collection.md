---
title: Otomatik Azure Stack günlük toplamayı yapılandırma | Microsoft Docs
description: Azure Stack yardım + destek ' te otomatik günlük toplamayı yapılandırma.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: efab23f12086fee2e4f5c14a70f95717ac9669b9
ms.sourcegitcommit: b752f4e6733d9ebe56dbd171a14528dcb9a693fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68522052"
---
# <a name="configure-automatic-azure-stack-diagnostic-log-collection"></a>Otomatik Azure Stack tanılama günlüğü toplamayı yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Günlük koleksiyonunuzu ve müşteri destek deneyiminizi kolaylaştırmak için otomatik tanılama günlüğü toplama özelliğini yapılandırmanızı öneririz. Sistem durumu koşullarının araştırılması gerekiyorsa, Günlükler Microsoft Müşteri Destek Hizmetleri (CSS) tarafından analiz edilmek üzere otomatik olarak karşıya yüklenebilir. 

## <a name="create-an-azure-blob-container-sas-url"></a>Azure Blob kapsayıcısı SAS URL 'SI oluşturma 

Otomatik günlük toplamayı yapılandırmadan önce, bir blob kapsayıcısı için paylaşılan erişim imzası (SAS) almanız gerekir. SAS, hesap anahtarlarınızı paylaşmadan Depolama hesabınızdaki kaynaklara erişim izni vermenizi sağlar. Azure Stack günlük dosyalarını Azure 'daki bir blob kapsayıcısına kaydedebilir ve ardından CSS 'nin günlükleri toplayabileceği SAS URL 'sini sağlayabilirsiniz. 

### <a name="prerequisites"></a>Önkoşullar

Azure 'da yeni veya mevcut bir blob kapsayıcısı kullanabilirsiniz. Azure 'da bir blob kapsayıcısı oluşturmak için, en azından [Depolama Blobu katılımcısı rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) veya [belirli izne](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)sahip olmanız gerekir. Genel Yöneticiler de gerekli izne sahiptir. 

Otomatik günlük toplama depolama hesabı için parametreleri seçme hakkında en iyi uygulamalar için bkz. [otomatik Azure Stack günlük toplama Için en iyi uygulamalar](azure-stack-best-practices-automatic-diagnostic-log-collection.md). Depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview)

### <a name="create-a-blob-storage-account"></a>BLOB depolama hesabı oluşturma
 
1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Depolama hesapları** > **Ekle**' ye tıklayın. 
1. Şu ayarlarla bir blob kapsayıcısı oluşturun:
   - **Abonelik**: Azure aboneliğinizi seçin.
   - **Kaynak grubu**: Bir kaynak grubu belirtin.
   - **Depolama hesabı adı**: Benzersiz bir depolama hesabı adı belirtin.
   - **Konum**: Şirket ilkenize uygun olarak bir veri merkezi seçin.
   - **Performans**: Standart öğesini seçin.
   - **Hesap türü** StorageV2 (genel amaçlı v2) öğesini seçin. 
   - **Çoğaltma**: Yerel olarak yedekli depolama (LRS) seçin
   - **Erişim katmanı**: Cool seçin

   ![Blob kapsayıcı özelliklerini gösteren ekran görüntüsü](media/azure-stack-automatic-log-collection/azure-stack-log-collection-create-storage-account.png)

1. **Gözden geçir + oluştur** ' a ve ardından **Oluştur**' a tıklayın.  

### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma 

1. Dağıtım başarılı olduktan sonra **Kaynağa Git ' e**tıklayın. Ayrıca, kolay erişim için depolama hesabını panoya sabitleyebilirsiniz. 
1. **Depolama Gezgini (Önizleme)** seçeneğine tıklayın, **BLOB kapsayıcıları**' na sağ tıklayın ve **BLOB kapsayıcısı oluştur**' a tıklayın. 
1. Yeni kapsayıcı için bir ad girin ve **Tamam**' a tıklayın.

## <a name="create-a-sas-url"></a>SAS URL 'SI oluşturma

1. Kapsayıcıya sağ tıklayın, **paylaşılan erişim Imzasını al**' a tıklayın. 
   
   ![Blob kapsayıcısının paylaşılan erişim imzasını almayı gösteren ekran görüntüsü](media/azure-stack-automatic-log-collection/get-sas.png)

1. Şu özellikleri seçin:
   - Başlangıç saati: İsteğe bağlı olarak başlangıç saatini geri taşıyabilirsiniz 
   - Süre sonu: İki yıl
   - Saat dilimi: UTC
   - İzinler: Okuma, yazma ve listeleme

   ![Paylaşılan erişim imzası özelliklerini gösteren ekran görüntüsü](media/azure-stack-automatic-log-collection/sas-properties.png) 

1.           **Oluştur**'a tıklayın.  

[Otomatik günlük toplamayı yapılandırırken](azure-stack-configure-automatic-diagnostic-log-collection.md)URL 'yi kopyalayın ve girin. SAS URL 'Leri hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1). 


## <a name="steps-to-configure-automatic-log-collection"></a>Otomatik günlük toplamayı yapılandırma adımları

SAS URL 'sini günlük koleksiyonu Kullanıcı arabirimine eklemek için aşağıdaki adımları izleyin: 

1. Azure Stack yönetici portalında oturum açın.
1. **Yardım ve desteğe genel bakış '** ı açın.
1. **Otomatik koleksiyon ayarları**' na tıklayın.

   ![Ekran görüntüsünde, yardım ve destek 'te günlük koleksiyonunun etkinleştirileceği gösterilir](media/azure-stack-automatic-log-collection/azure-stack-automatic-log-collection.png)

1. Otomatik günlük toplamayı **etkin**olarak ayarlayın.
1. Depolama hesabı blob kapsayıcısının paylaşılan erişim imzası (SAS) URL 'sini girin.

   ![Ekran görüntüsü blob SAS URL 'sini gösterir](media/azure-stack-automatic-log-collection/azure-stack-enable-automatic-log-collection.png)

>[!NOTE]
>Otomatik günlük toplama devre dışı bırakılabilir ve her zaman yeniden etkinleştirilebilir. SAS URL 'SI yapılandırması değişmez. Otomatik günlük toplama yeniden etkinleştirildiyse, daha önce girilen SAS URL 'SI aynı doğrulama denetimlerine sahip olur ve zaman aşımına uğradı bir SAS URL 'SI reddedilir. 


## <a name="view-log-collection"></a>Günlük toplamayı görüntüle

Azure Stack toplanan günlüklerin geçmişi, yardım ve Destek bölümündeki **günlük koleksiyonu** sayfasında aşağıdaki tarih ve saatlerle görüntülenir:

- **Toplama zamanı**: Günlük toplama işlemi başladığında 
- **Kimden tarihi**: Toplamak istediğiniz zaman döneminin başlangıcı
- Bitiş **tarihi**: Zaman döneminin sonu

![Ekran görüntüsü günlük koleksiyonlarını gösterir](media/azure-stack-automatic-log-collection/azure-stack-log-collection.png)

Tanılama günlüğü koleksiyonu başarısız olursa SAS URL 'sinin geçerli olduğunu doğrulayın. Hata devam ederse veya birden çok hata görürseniz yardım için Microsoft CSS 'yi çağırın. 

İşleçler Ayrıca, otomatik olarak toplanan Günlükler için depolama hesabını denetleyebilir. Örneğin, bu ekran görüntüsünde Azure portal Depolama Gezgini Önizlemesi kullanılarak günlük koleksiyonları gösterilmektedir:

![Ekran görüntüsü günlük koleksiyonlarını gösterir](media/azure-stack-automatic-log-collection/check-storage-account.png)


## <a name="see-also"></a>Ayrıca bkz.

[Azure Stack günlüğü ve müşteri verileri işleme](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

[Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[Otomatik Azure Stack günlük toplama için en iyi yöntemler](azure-stack-best-practices-automatic-diagnostic-log-collection.md)





