---
title: Azure Stack | SQL bağdaştırıcısı kaynak sağlayıcısı tarafından sunulan veritabanlarını kullanma | Microsoft Docs
description: SQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan SQL veritabanlarını oluşturma ve yönetme
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: dfa8e778b21826ff1f589efbf2b49097d5ee3685
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829335"
---
# <a name="create-sql-databases"></a>SQL veritabanı oluşturma

Kullanıcı portalında self servis veritabanlarını oluşturabilir ve yönetebilirsiniz. Azure Stack kullanıcının SQL veritabanı hizmetini içeren bir teklifiyle bir aboneliğe ihtiyacı vardır.

1. [Azure Stack](azure-stack-overview.md) Kullanıcı portalında oturum açın.

2. **+ Yeni** &gt;**veri + depolama** &gt; **SQL Server veritabanı** &gt; **Ekle**' yi seçin.

3. **Veritabanı oluştur**' un altında, **veritabanı adı** ve **en büyük boyut olarak MB cinsinden**gerekli bilgileri girin.

   >[!NOTE]
   >Veritabanı boyutunun en az 64 MB olması gerekir, bu da veritabanını dağıttıktan sonra artırılabilir.

   Ortamınız için gereken diğer ayarları yapılandırın.

4. **Veritabanı oluştur**altında **SKU**' yı seçin. **SKU seçin**altında veritabanınız için SKU 'yu seçin.

   ![Veritabanı Oluştur](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Barındırma sunucuları Azure Stack eklendikçe, bunlara bir SKU atanır. Veritabanları, bir SKU 'daki barındırma sunucuları havuzunda oluşturulur.

5. **Oturum aç**' ı seçin.
6. **Oturum aç**' ın altında, mevcut bir oturum açma veya **+ Yeni bir oturum oluştur**' u seçin.
7. **Yeni oturum açma**altında, **veritabanı oturum açma** ve **parola**için bir ad girin.

   >[!NOTE]
   >Bu ayarlar, yalnızca bu veritabanına erişim için oluşturulan SQL kimlik doğrulama kimlik bilgileridir. Oturum açma Kullanıcı adı genel olarak benzersiz olmalıdır. Aynı SKU 'YU kullanan diğer veritabanları için oturum açma ayarlarını yeniden kullanabilirsiniz.

   ![Yeni bir veritabanı oturumu oluştur](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Veritabanını dağıtmaya son vermek için **Tamam ' ı** seçin.

Veritabanı dağıtıldıktan sonra gösterilen **temel parçalar**altında **bağlantı dizesini**de göz önünde edin. Bu dizeyi, SQL Server veritabanına erişmesi gereken herhangi bir uygulamada kullanabilirsiniz.

![Bağlantı dizesi alma](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always on veritabanları

Tasarım, her zaman tek başına sunucu ortamından farklı şekilde işlenir. Daha fazla bilgi için bkz. [Azure sanal makinelerinde Always on kullanılabilirlik grupları SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>SQL Always on veritabanlarını doğrulama

Aşağıdaki ekran yakalama, SQL her zaman açık ' de veritabanı durumuna bakmak için SQL Server Management Studio nasıl kullanabileceğinizi gösterir.

![AlwaysOn veritabanı durumu](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

Her zaman açık veritabanları, tüm SQL örneklerinde eşitlenmiş ve kullanılabilir olarak gösterilmelidir ve kullanılabilirlik gruplarında görünürler. Önceki ekran yakalamada, veritabanı örneği newdb1 ve durumu **newdb1 (eşitlenmiş)** olur.

### <a name="delete-an-alwayson-database"></a>AlwaysOn veritabanını silme

Kaynak sağlayıcıdan bir SQL AlwaysOn veritabanını sildiğinizde, SQL veritabanını birincil çoğaltmadan ve kullanılabilirlik grubundan siler.

Ardından, SQL veritabanını diğer çoğaltmalarda geri yükleme durumuna geçirir ve tetiklenmediği takdirde veritabanını bırakmaz. Veritabanı bırakılmazsa, ikincil çoğaltmalar eşitleme olmayan bir duruma geçer.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server kaynak sağlayıcısını koruyun](azure-stack-sql-resource-provider-maintain.md)
