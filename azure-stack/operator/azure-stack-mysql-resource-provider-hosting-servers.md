---
title: Azure Stack MySQL barındırma sunucuları Ekle | Microsoft Docs
description: MySQL bağdaştırıcı kaynak sağlayıcısı aracılığıyla sağlama için MySQL barındırma sunucuları eklemeyi öğrenin.
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
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 632528519bbca9286ab72fb806ee756ccdb6a166
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682226"
---
# <a name="add-mysql-hosting-servers-in-azure-stack"></a>Azure Stack MySQL barındırma sunucuları ekleme

MySQL kaynak sağlayıcısı örneğe bağlanabildiği sürece, [Azure Stack](azure-stack-overview.md)veya Azure Stack ortamınızın dışındaki bir sanal makinede bir MySQL barındırma sunucusu örneğini barındırabilirsiniz.

> [!NOTE]
> MySQL barındırma sunucuları faturalanabilir, Kullanıcı abonelikleri ' nde oluşturulması gerektiği sürece MySQL kaynak sağlayıcısı varsayılan sağlayıcı aboneliğinde oluşturulmalıdır. Kaynak sağlayıcısı sunucusu, Kullanıcı veritabanlarını barındırmak için kullanılmamalıdır.

Barındırma sunucularınız için 5,6, 5,7 ve 8,0 MySQL sürümleri kullanılabilir. MySQL RP, caching_sha2_password kimlik doğrulamasını desteklemiyor; Bu, sonraki sürüme eklenecektir. MySQL 8,0 sunucularının mysql_native_password kullanacak şekilde yapılandırılması gerekir. MariaDB de desteklenir.

## <a name="connect-to-a-mysql-hosting-server"></a>MySQL barındırma sunucusuna bağlan

Sistem Yöneticisi ayrıcalıklarına sahip bir hesabın kimlik bilgilerine sahip olduğunuzdan emin olun. Bir barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Azure Stack yönetici portalında hizmet yöneticisi olarak oturum açın.
2. **Tüm Hizmetler**’i seçin.
3. **YÖNETIM kaynakları** kategorisi altında **MySQL barındırma sunucuları**  >  **+ Ekle**' yi seçin. Aşağıdaki ekran yakalama bölümünde gösterilen **MySQL barındırma sunucusu Ekle** iletişim kutusu açılır.

   ![MySQL barındırma sunucusu yapılandırma](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. MySQL Server örneğinizin bağlantı ayrıntılarını sağlayın.

   * **MySQL barındırma sunucu adı**için tam etki alanı adı (FQDN) veya geçerli bir IPv4 adresi sağlayın. Kısa VM adını kullanmayın.
   * Azure Stack marketi 'nde bulunan BitNami MySQL görüntüleri için varsayılan yönetici **Kullanıcı adı** *kök*.
   * Kök **parolayı**bilmiyorsanız, nasıl alınacağını öğrenmek Için [BitNami belgelerine](https://docs.bitnami.com/azure/faq/#how-to-find-application-credentials) bakın.
   * Varsayılan bir MySQL örneği sağlanmadığından, **barındırma sunucusunun boyutunu GB olarak**belirtmeniz gerekir. Veritabanı sunucusunun kapasitesine yakın bir boyut girin.
   * **Abonelik**için varsayılan ayarı koruyun.
   * **Kaynak grubu**için, yeni bir tane oluşturun veya var olan bir grubu kullanın.

   > [!NOTE]
   > MySQL örneğine kiracı ve yönetici Azure Resource Manager erişilebiliyorsa, kaynak sağlayıcısı denetiminin altına koyabilirsiniz. Ancak, MySQL örneği yalnızca kaynak sağlayıcısına **ayrılmalıdır** .

5. **SKU oluştur** iletişim kutusunu açmak Için **SKU 'ları** seçin.

   ![MySQL SKU 'SU oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   Kullanıcıların veritabanlarını uygun SKU 'ya dağıtabilmesi için SKU **adı** SKU 'nun özelliklerini yansıtmalıdır.

6. SKU 'YU oluşturmak için **Tamam ' ı** seçin.
   > [!NOTE]
   > SKU 'Ların portalda görünür olması bir saate kadar sürebilir. SKU dağıtılmadan ve çalışmadığı sürece bir veritabanı oluşturamazsınız.

7. **MySQL barındırma sunucusu Ekle**altında **Oluştur**' u seçin.

Sunucu eklerken, hizmet tekliflerini ayırt etmek için bunları yeni veya mevcut bir SKU 'ya atayın. Örneğin, daha fazla veritabanı ve otomatik yedeklemeler sağlayan bir MySQL kurumsal örneğine sahip olabilirsiniz. Bu yüksek performanslı sunucuyu, kuruluşunuzdaki farklı departmanlar için ayırabilirsiniz.

## <a name="security-considerations-for-mysql"></a>MySQL için güvenlik konuları

Aşağıdaki bilgiler, RP ve MySQL barındırma sunucuları için geçerlidir:

* Tüm barındırma sunucularının TLS 1,2 kullanılarak iletişim için yapılandırıldığından emin olun. Bkz. [şifrelenmiş bağlantıları kullanmak Için MySQL yapılandırma](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html).
* [Saydam veri şifrelemesi](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html).
* MySQL RP, caching_sha2_password kimlik doğrulamasını desteklemiyor.

## <a name="increase-backend-database-capacity"></a>Arka uç veritabanı kapasitesini artırma

Azure Stack portalında daha fazla MySQL sunucusu dağıtarak arka uç veritabanı kapasitesini artırabilirsiniz. Bu sunucuları yeni veya mevcut bir SKU 'ya ekleyin. Var olan bir SKU 'ya sunucu eklerseniz, sunucu özelliklerinin SKU 'daki diğer sunucularla aynı olduğundan emin olun.

## <a name="sku-notes"></a>SKU notları
SKU 'da kapasite ve performans gibi sunucuların yeteneklerini açıklayan bir SKU adı kullanın. Ad, kullanıcıların veritabanlarını uygun SKU 'ya dağıtmalarına yardımcı olmak için bir yardım görevi görür. Örneğin, hizmet tekliflerini aşağıdaki özelliklerle ayırt etmek için SKU adlarını kullanabilirsiniz:
  
* yüksek kapasite
* yüksek performans
* yüksek kullanılabilirlik

En iyi uygulama olarak, bir SKU 'daki tüm barındırma sunucuları aynı kaynak ve performans özelliklerine sahip olmalıdır.

SKU 'Lar belirli kullanıcılara veya gruplara atanamaz.

Bir SKU 'yu düzenlemek için, **tüm hizmetler**  > **MySQL bağdaştırıcısı**  > **SKU 'larına**gidin. Değiştirilecek SKU 'yu seçin, tüm gerekli değişiklikleri yapın ve değişiklikleri kaydetmek için **Kaydet** ' e tıklayın. 

Artık gerekli olmayan bir SKU 'YU silmek için, **tüm hizmetler**  > **MySQL bağdaştırıcısı**  > **SKU 'larına**gidin. SKU adına sağ tıklayın **ve Sil ' i seçerek silin** .

> [!IMPORTANT]
> Yeni SKU 'Ların Kullanıcı portalında kullanılabilmesi bir saate kadar sürebilir.

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL veritabanı sunucularını kullanıcılarınız için kullanılabilir hale getirme

MySQL veritabanı sunucularını kullanıcılar için kullanılabilir hale getirmek için planlar ve teklifler oluşturun. Plana Microsoft. MySqlAdapter hizmetini ekleyin ve yeni bir kota oluşturun. MySQL, veritabanlarının boyutunu sınırlandırmaya izin vermez.

> [!IMPORTANT]
> Yeni kotaların Kullanıcı portalında veya değiştirilen bir kota zorlanmadan önce kullanılabilmesi iki saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL veritabanı oluşturma](azure-stack-mysql-resource-provider-databases.md)
