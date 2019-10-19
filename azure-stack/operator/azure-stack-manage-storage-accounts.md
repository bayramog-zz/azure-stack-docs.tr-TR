---
title: Azure Stack depolama hesaplarını yönetme | Microsoft Docs
description: Azure Stack depolama hesaplarını bulma, yönetme, kurtarma ve geri kazanma hakkında bilgi edinin.
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
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: 9af79442a0fb56e4d6a9cef99741b0180e84304c
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534195"
---
# <a name="manage-azure-stack-storage-accounts"></a>Azure Stack depolama hesaplarını yönetme

Azure Stack depolama hesaplarını yönetmeyi öğrenin. İş ihtiyaçlarına göre depolama kapasitesini bulun, kurtarın ve geri kazanın.

## <a name="find-a-storage-account"></a>Depolama hesabı bulma

Bölgedeki depolama hesaplarının listesi aşağıdaki adımları izleyerek Azure Stack ' de görüntülenebilir:

1. [Yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.

2. **Tüm hizmetler**  > **depolama**  > **depolama hesapları**' nı seçin.

   ![Azure Stack depolama hesapları](media/azure-stack-manage-storage-accounts/image4.png)

Varsayılan olarak, ilk 10 hesap görüntülenir. Listenin en altındaki **daha fazla yükle** bağlantısına tıklayarak daha fazlasını getirmeyi tercih edebilirsiniz.

VEYA

Belirli bir depolama hesabıyla ilgileniyorsanız, yalnızca **ilgili hesaplara filtre uygulayabilir ve bunları getirebilirsiniz** .

**Hesapları filtrelemek için:**

1. Bölmenin en üstünde bulunan **filtre** ' yi seçin.
2. Filtre bölmesinde, görüntülenecek depolama hesapları listesinde ince ayar yapmak için **Hesap adı**, **abonelik kimliği**veya **durum** belirtmenize olanak tanır. Bunları uygun şekilde kullanın.
3. Siz yazarken, liste otomatik olarak filtreyi uygular.

    ![Azure Stack depolama hesaplarını filtrele](media/azure-stack-manage-storage-accounts/image5.png)

4. Filtreyi sıfırlamak için: **filtre**' yi seçin, seçimleri kaldırın ve güncelleştirin.

Arama metin kutusu (depolama hesapları liste bölmesinin üst kısmında), hesaplar listesinde seçilen metni vurgulamanızı sağlar. Tam adı veya KIMLIĞI kolayca kullanılabilir olmadığında bunu kullanabilirsiniz.

İlgilendiğiniz hesabı bulmaya yardımcı olması için burada ücretsiz metin kullanabilirsiniz.

![Azure Stack depolama hesabı bulun](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Hesap ayrıntılarına bakın
Görüntülemekte olduğunuz hesapları bulduktan sonra, belirli ayrıntıları görüntülemek için belirli hesabı seçebilirsiniz. Hesap ayrıntılarının bulunduğu yeni bir bölme açılır. Bu ayrıntılar hesap türü, oluşturma saati, konum vb. içerir.

![Depolama hesabı ayrıntıları](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Silinen bir hesabı kurtarma
Silinen bir hesabı kurtarmanız gereken bir durumda olabilirsiniz.

Azure Stack, bunu yapmanın basit bir yolu vardır:

1. Depolama hesapları listesine gidin. Daha fazla bilgi için bu makalenin en üstündeki [depolama hesabı bulma](azure-stack-manage-storage-accounts.md) bölümüne bakın.
2. Listede bu belirli hesabı bulun. Filtrelemeniz gerekebilir.
3. Hesabın *durumunu* kontrol edin. **Silinmelidir**.
4. Hesap ayrıntıları bölmesini açan hesabı seçin.
5. Bu bölmenin en üstünde, **kurtar** düğmesini bulun ve seçin.
6. Onaylamak için **Evet**'i seçin.

   ![Depolama hesabı kurtarmayı onaylama](media/azure-stack-manage-storage-accounts/image8.png)

7. Kurtarma işlemi artık sürüyor. Başarılı olduğunu belirten bir gösterge bekleyin. Ayrıca, devam eden göstergeleri görüntülemek için portalın üst kısmındaki "zil" simgesini de seçebilirsiniz.

   ![Depolama hesabı kurtarma başarılı](media/azure-stack-manage-storage-accounts/image9.png)

   Kurtarılan hesap başarıyla eşitlendikten sonra yeniden kullanılabilir.

### <a name="some-gotchas"></a>Bazı gotchas
* Silinen hesabınız **, bekletme dışında**durumunu gösterir.
  
  Saklama süresi, silinen hesabın bekletme süresini aşması ve kurtarılamadığını gösterir.

* Silinen hesabınız hesaplar listesinde gösterilmez.
  
  Silinen hesap zaten atık olarak toplanmışsa hesabınız hesap listesinde görüntülenmeyebilir. Bu durumda, kurtarılamaz. Daha fazla bilgi için bu makaledeki [kapasiteyi geri](#reclaim) alma bölümüne bakın.

## <a name="set-the-retention-period"></a>Saklama süresini ayarlama
Saklama süresi ayarı bulut operatörünün silinen hesaplar için olası kurtarma süresi olarak gün cinsinden bir süre (0 ile 9999 gün arası) belirtmesine olanak tanır. Varsayılan saklama süresi 0 güne ayarlanmıştır. Değerin "0" olarak ayarlanması, silinen hesapların hemen saklama dışında tutulacağı ve düzenli atık toplama için işaretleneceği anlamına gelir.

**Saklama süresini değiştirmek için:**

1. [Yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.
2. **Yönetim**altındaki **tüm hizmetler**  > **Bölge yönetimi** ' ni seçin.
3. **Kaynak sağlayıcıları**  > **depolama**  > **ayarları**' nı seçin. Yolunuz, ana > *bölgesi* -kaynak sağlayıcıları > depolama alanı.
4. **Yapılandırma** ' yı seçin ve Bekletme dönemi değerini düzenleyin.

   Gün sayısını ayarlayın ve sonra kaydedin.

   Bu değer hemen etkindir ve tüm bölgeniz için ayarlanır.

   ![Yönetici portalında saklama süresini düzenleme](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Kapasiteyi geri kazanma
Saklama dönemi elde eden yan etkilerden biri, silinen bir hesabın, bekletme döneminin dışına çıkana kadar kapasiteyi tüketmeye devam etmektedir. Bulut operatörü olarak, saklama süresi henüz dolmadığı halde, silinen hesap alanını geri kazanmak için bir yol gerekebilir.

Portalı veya PowerShell 'i kullanarak kapasiteyi geri kazanabilirsiniz.

**Portalı kullanarak kapasiteyi geri kazanmak için:**
1. Depolama hesapları bölmesine gidin. Bkz. depolama hesabı bulma.
2. Bölmenin en üstünde **yer alan geri kazanmak** ' ı seçin.
3. İletiyi okuyun ve **Tamam**' ı seçin.

    ![Depolama hesaplarında alanı geri alma](media/azure-stack-manage-storage-accounts/image11.png)

4. Başarı bildirimini bekle. Portalda zil simgesine bakın.

    ![Alan başarıyla geri kazanılır](media/azure-stack-manage-storage-accounts/image12.png)

5. Depolama hesapları sayfasını yenileyin. Silinen hesaplar temizlendikleri için listede artık gösterilmemektedir.

Ayrıca, bekletme süresini açıkça geçersiz kılmak ve kapasiteyi hemen geri kazanmak için PowerShell 'i de kullanabilirsiniz.

**PowerShell kullanarak kapasiteyi geri kazanmak için:**

1. Azure PowerShell yüklendiğini ve yapılandırıldığını doğrulayın. Aksi takdirde, aşağıdaki yönergeleri kullanın: 
   * En son Azure PowerShell sürümünü yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Azure Resource Manager cmdlet 'leri hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile Azure PowerShell kullanma](https://go.microsoft.com/fwlink/?LinkId=394767).
2. Aşağıdaki cmdlet 'leri çalıştırın:

> [!NOTE]  
> Bu cmdlet 'leri çalıştırırsanız, hesabı ve içeriğini kalıcı olarak silersiniz. Kurtarılamaz. Bunu dikkatli kullanın.

```powershell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
```

Daha fazla bilgi için bkz. [PowerShell belgeleri Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
 

## <a name="next-steps"></a>Sonraki adımlar

 - İzinleri yönetme hakkında bilgi için bkz. [rol tabanlı erişim denetimi kullanarak erişim Izinlerini ayarlama](azure-stack-manage-permissions.md).
 - Azure Stack için depolama kapasitesini yönetme hakkında bilgi için bkz. [Azure Stack depolama kapasitesini yönetme](azure-stack-manage-storage-shares.md).
