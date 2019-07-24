---
title: Azure Stack depolama hesaplarını yönetme | Microsoft Docs
description: Azure Stack depolama hesaplarını bulma, yönetme, kurtarma ve geri kazanma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: 4f9e9c4f79a06e0f1f74db8152047beb3af07b75
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68417536"
---
# <a name="manage-azure-stack-storage-accounts"></a>Azure Stack depolama hesaplarını yönetme

Azure Stack depolama hesaplarını yönetmeyi öğrenin. İş ihtiyaçlarına göre depolama kapasitesini bulun, kurtarın ve geri kazanın.

## <a name="find-a-storage-account"></a>Depolama hesabı bulma

Bölgesindeki depolama hesaplarının listesi, Azure Stack tarafından görüntülenebilir:

1. [Yönetim portalında](https://adminportal.local.azurestack.external)oturum açın.

2. **Tüm hizmetler** > depolamadepolama > **hesapları**' nı seçin.

   ![Azure Stack depolama hesapları](media/azure-stack-manage-storage-accounts/image4.png)

Varsayılan olarak, ilk 10 hesap görüntülenir. Listenin en altındaki **daha fazla yükle** bağlantısına tıklayarak daha fazlasını getirmeyi tercih edebilirsiniz.

OR

Belirli bir depolama hesabıyla ilgileniyorsanız, yalnızca **ilgili hesaplara filtre uygulayabilir ve bunları getirebilirsiniz** .


**Hesapları filtrelemek için:**

1. Bölmenin en üstünde bulunan **filtre** ' yi seçin.
2. Filtre bölmesinde, görüntülenecek depolama hesapları listesinde ince ayar yapmak için **Hesap adı**, **abonelik kimliği**veya **durum** belirtmenize olanak tanır. Bunları uygun şekilde kullanın.
3. Siz yazarken, liste otomatik olarak filtreyi uygular.  .
   
    ![Azure Stack depolama hesaplarını filtrele](media/azure-stack-manage-storage-accounts/image5.png)

4. Filtreyi sıfırlamak için: **filtre**' yi seçin, seçimleri kaldırın ve güncelleştirin.

Arama metin kutusu (depolama hesapları liste bölmesinin üst kısmında), hesaplar listesinde seçilen metni vurgulamanızı sağlar. Tam adı veya KIMLIĞI kolayca kullanılabilir olmadığında bunu kullanabilirsiniz.

İlgilendiğiniz hesabı bulmaya yardımcı olması için burada ücretsiz metin kullanabilirsiniz.

![Azure Stack depolama hesabı bulun](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Hesap ayrıntılarına bakın
Görüntülemekte olduğunuz hesapları bulduktan sonra, belirli ayrıntıları görüntülemek için belirli bir hesap seçebilirsiniz. Hesap ayrıntılarıyla birlikte yeni bir bölme açılır: hesabın türü, oluşturma saati, konum, vb.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Silinen bir hesabı kurtarma
Silinen bir hesabı kurtarmanız gereken bir durumda olabilirsiniz.

Azure Stack, bunu yapmanın basit bir yolu vardır:

1. Depolama hesapları listesine gidin. Daha fazla bilgi için bu makaledeki depolama hesabı bulma bölümüne bakın.
2. Listede bu belirli hesabı bulun. Filtrelemeniz gerekebilir.
3. Hesabın *durumunu* kontrol edin. **Silinmelidir**.
4. Hesap ayrıntıları bölmesini açan hesabı seçin.
5. Bu bölmenin en üstünde, **kurtar** düğmesini bulun ve seçin.
6. Onaylamak için **Evet**’i seçin.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Kurtarma işlemi şu anda *işlemde...* başarılı olduğunu belirten bir gösterge bekleyin.
   Ayrıca, devam eden göstergeleri görüntülemek için portalın üst kısmındaki "zil" simgesini de seçebilirsiniz.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Kurtarılan hesap başarıyla eşitlendikten sonra yeniden kullanılabilir.

### <a name="some-gotchas"></a>Bazı gotchas
* Silinen hesabınız **, bekletme dışında**durumunu gösterir.
  
  Saklama süresi, silinen hesabın bekletme süresini aşması ve kurtarılamadığını gösterir.
* Silinen hesabınız hesaplar listesinde gösterilmez.
  
  Silinen hesap zaten atık olarak toplanmışsa hesabınız hesap listesinde görüntülenmeyebilir. Bu durumda, kurtarılamaz. Bu makaledeki [kapasiteyi geri](#reclaim) alma bölümüne bakın.

## <a name="set-the-retention-period"></a>Saklama süresini ayarlama
Bekletme dönemi ayarı, bir bulut işlecinin, silinen hesabın büyük olasılıkla kurtarılabileceği gün (0 ve 9999 gün arasında) cinsinden bir süre belirtmesini sağlar. Varsayılan saklama süresi 0 gün olarak ayarlanır. Değerin "0" olarak ayarlanması, silinen her hesabın hemen bekletme dışı ve düzenli çöp toplama için işaretlenme anlamına gelir.

**Saklama süresini değiştirmek için:**

1. [Yönetim portalında](https://adminportal.local.azurestack.external)oturum açın.
2. **Yönetim**altındaki **tüm hizmetler** > **Bölge yönetimi** ' ni seçin.
3. **Kaynak sağlayıcıları** > depolamaAyarları > ' nı seçin. Yolunuz, ana > *bölgesi* -kaynak sağlayıcıları > depolama alanı.
4. **Yapılandırma** ' yı seçin ve Bekletme dönemi değerini düzenleyin.

   Gün sayısını ayarlayın ve sonra kaydedin.
   
   Bu değer hemen etkindir ve tüm bölgeniz için ayarlanır.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Kapasiteyi geri kazanma
Saklama dönemi elde eden yan etkilerden biri, silinen bir hesabın, bekletme döneminin dışına çıkana kadar kapasiteyi tüketmeye devam etmektedir. Bulut operatörü olarak, saklama süresi henüz sona ermediği halde, silinen hesap alanını geri kazanmak için bir yol gerekebilir.

Portalı veya PowerShell 'i kullanarak kapasiteyi geri kazanabilirsiniz.

**Portalı kullanarak kapasiteyi geri kazanmak için:**
1. Depolama hesapları bölmesine gidin. Bkz. depolama hesabı bulma.
2. Bölmenin en üstünde **yer alan geri kazanmak** ' ı seçin.
3. İletiyi okuyun ve **Tamam**' ı seçin.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Başarı bildirimini bekle portalda zil simgesine bakın.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Depolama hesapları sayfasını yenileyin. Silinen hesaplar temizlendikleri için listede artık gösterilmemektedir.

Ayrıca, bekletme süresini açıkça geçersiz kılmak ve kapasiteyi hemen geri kazanmak için PowerShell 'i de kullanabilirsiniz.

**PowerShell kullanarak kapasiteyi geri kazanmak için:**   

1. Azure PowerShell yüklendiğini ve yapılandırıldığını doğrulayın. Aksi takdirde, aşağıdaki yönergeleri kullanın: 
   * En son Azure PowerShell sürümünü yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Azure Resource Manager cmdlet 'leri hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile Azure PowerShell kullanma](https://go.microsoft.com/fwlink/?LinkId=394767)
2. Aşağıdaki cmdlet 'leri çalıştırın:

> [!NOTE]  
> Bu cmdlet 'leri çalıştırırsanız, hesabı ve içeriğini kalıcı olarak silersiniz. Kurtarılamaz. Bunu dikkatli kullanın.

```powershell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
```

Daha fazla bilgi için bkz. [PowerShell belgeleri Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
 

## <a name="next-steps"></a>Sonraki adımlar

 - İzinleri yönetme hakkında bilgi için bkz. [rol tabanlı Access Control yönetme](azure-stack-manage-permissions.md).
 - Azure Stack için depolama kapasitesini yönetme hakkında bilgi için bkz. [Azure Stack depolama kapasitesini yönetme](azure-stack-manage-storage-shares.md).
