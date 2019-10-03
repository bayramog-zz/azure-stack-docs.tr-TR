---
title: Azure 'dan Market öğelerini indirin ve Azure Stack yayımlayın | Microsoft Docs
description: Azure 'dan Market öğelerini indirme ve Azure Stack yayımlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: justinha
ms.reviewer: ihcherie
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: f4ffb166d4ae45ca8d7ef335d81e51e2775eb985
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829124"
---
# <a name="download-marketplace-items-from-azure-and-publish-to-azure-stack"></a>Azure 'dan Market öğelerini indirin ve Azure Stack yayımlayın

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bulut operatörü olarak, Azure Marketi 'nden öğeleri indirebilir ve Azure Stack ' de kullanılabilir hale getirebilirsiniz. Seçebileceğiniz öğeler, önceden test edilmiş ve Azure Stack ile çalışmak üzere desteklenen Azure Marketi öğelerinin seçkin bir listesidir. Ek öğeler genellikle bu listeye eklendiyse, bu nedenle yeni içerik için yeniden kullanıma devam edin.

Azure Market 'e bağlanmak için iki senaryo vardır:

- **Bağlı senaryo** -Azure Stack ortamının internet 'e bağlanmasını gerektirir. Öğeleri bulmak ve indirmek için Azure Stack portalını kullanın.
- **Bağlantısı kesilmiş veya kısmen bağlı senaryo** -Market öğelerini Indirmek için Market dağıtım aracını kullanarak İnternet 'e erişmenizi gerektirir. Ardından, indirilenlerinizi bağlantısı kesilen Azure Stack yüklemenize aktarırsınız. Bu senaryo PowerShell kullanır.

İndirebileceğiniz Market öğelerinin tüm listesi için bkz. [Azure Marketi öğeleri Azure Stack](azure-stack-marketplace-azure-items.md) . Azure Stack Market 'Teki son ekleme, silme ve güncelleştirme işlemlerinin bir listesi için [Azure Stack Market Changes](azure-stack-marketplace-changes.md) makalesine bakın.

## <a name="connected-scenario"></a>Bağlantılı senaryo

Azure Stack internet 'e bağlanırsa, Market öğelerini indirmek için Yönetici portalını kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

Azure Stack dağıtımınız Internet bağlantısına sahip olmalı ve [Azure 'a kayıtlı](azure-stack-registration.md)olmalıdır.

### <a name="use-the-portal-to-download-marketplace-items"></a>Market öğelerini indirmek için portalı kullanma
  
1. Azure Stack yönetici portalında oturum açın.

2. Market öğelerini indirmeden önce kullanılabilir depolama alanını gözden geçirin. Daha sonra, indirmek üzere öğeler ' i seçtiğinizde, indirme boyutunu kullanılabilir depolama kapasiteniz ile karşılaştırabilirsiniz. Kapasite sınırlıysa, [kullanılabilir alanı yönetmeye](azure-stack-manage-storage-shares.md#manage-available-space)yönelik seçenekleri göz önünde bulundurun.

    Kullanılabilir alanı gözden geçirmek için: **Bölge yönetimi**'nde, araştırmak istediğiniz bölgeyi seçin ve ardından **kaynak sağlayıcıları** > **depolama**alanına gidin:

    ![Azure Stack yönetici portalındaki depolama alanını gözden geçirme](media/azure-stack-download-azure-marketplace-item/storage.png)

3. Azure Stack marketi 'ni açın ve Azure 'a bağlanın. Bunu yapmak için **Market yönetimi** hizmetini seçin, **Market öğeleri**' ni seçin ve ardından **Azure 'dan Ekle**' yi seçin:

    ![Azure 'dan Market öğeleri ekleme](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    Portal, Azure Marketi 'nden yüklenebilecek öğelerin listesini görüntüler. Ürünleri ada, yayımcıya ve/veya ürün türüne göre filtreleyebilirsiniz. Ayrıca, açıklamasını ve yükleme boyutu dahil ek bilgileri görüntülemek için her bir öğeyi de seçebilirsiniz:

    ![Azure Market öğeleri listesi ](media/azure-stack-download-azure-marketplace-item/image03.PNG)

4. İstediğiniz öğeyi seçin ve ardından **İndir**' i seçin. İndirme süreleri farklılık gösterir.

    ![Azure Marketi öğesi indiriliyor](media/azure-stack-download-azure-marketplace-item/image04.png)

    Yükleme tamamlandıktan sonra, yeni Market öğesini bir Azure Stack işleci veya bir kullanıcı olarak dağıtabilirsiniz.

5. İndirilen öğeyi dağıtmak için **+ kaynak oluştur**' u seçin ve ardından yeni Market öğesiyle ilgili kategoriler arasında arama yapın. Sonra, dağıtım işlemine başlamak için öğeyi seçin. İşlem, farklı Market öğeleri için farklılık gösterir.

## <a name="disconnected-or-a-partially-connected-scenario"></a>Bağlantısı kesik veya kısmen bağlı bir senaryo

Azure Stack bağlantısı kesik bir modundaysanız, Market öğelerini internet bağlantısı olan bir makineye indirmek için PowerShell ve *Market dağıtım aracı* 'nı kullanırsınız. Daha sonra öğeleri Azure Stack ortamınıza aktarabilirsiniz. Bağlantısı kesilmiş bir ortamda, Azure Stack portalını kullanarak Market öğelerini indiremez.

Market dağıtım aracı, bağlı bir senaryoda da kullanılabilir.

Bu senaryonun iki bölümü vardır:

- **1. Bölüm:** Azure Marketi 'nden indirin. İnternet erişimi olan bilgisayarda PowerShell 'i yapılandırın, dağıtım aracını indirin ve ardından Azure Marketi 'nden öğeleri indirin.  
- **Bölüm 2:** Azure Stack Market 'e yükleyin ve yayımlayın. İndirdiğiniz dosyaları Azure Stack ortamınıza taşıyın, Azure Stack içeri aktarıp Azure Stack Market 'Te yayımlayabilirsiniz.  

### <a name="prerequisites"></a>Önkoşullar

- Azure Stack dağıtımınızın [Azure 'a kayıtlı](azure-stack-registration.md)olması gerekir.

- İnternet bağlantısına sahip bilgisayarda **Azure Stack PowerShell modülü sürüm 1.2.11** veya üzeri olmalıdır. Henüz yoksa, [Azure Stack belirli PowerShell modüllerini yükler](azure-stack-powershell-install.md).  

- İndirilen bir market öğesinin içeri aktarımını etkinleştirmek için [Azure Stack Işlecinin PowerShell ortamının](azure-stack-powershell-configure-admin.md) yapılandırılması gerekir.  

- Genel olarak erişilebilen bir kapsayıcıya (Depolama Blobu) sahip Azure Stack bir [depolama hesabınız](azure-stack-manage-storage-accounts.md) olması gerekir. Kapsayıcıyı Market öğeleri Galeri dosyaları için geçici depolama alanı olarak kullanırsınız. Depolama hesapları ve kapsayıcıları hakkında bilgi sahibi değilseniz bkz. Azure belgelerindeki [Bloblarla çalışma-Azure Portal](/azure/storage/blobs/storage-quickstart-blobs-portal) .

- Market dağıtım aracı ilk yordam sırasında indirilir.

- En iyi indirme performansı için [AzCopy](/azure/storage/common/storage-use-azcopy) 'i yükleyebilirsiniz, ancak bu gerekli değildir.

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Market öğelerini indirmek için Market dağıtım aracını kullanma

1. İnternet bağlantısı olan bir bilgisayarda, yönetici olarak bir PowerShell konsolu açın.

2. Azure Stack kaydetmek için kullandığınız Azure hesabını ekleyin. Hesabı eklemek için, PowerShell 'de herhangi bir `Add-AzureRmAccount` parametre olmadan çalıştırın. Hesabınızın yapılandırmasına bağlı olarak, Azure hesabı kimlik bilgilerinizi girmeniz istenir ve iki öğeli kimlik doğrulaması kullanmanız gerekebilir.

   [!include[Remove Account](../../includes/remove-account.md)]

3. Birden çok aboneliğiniz varsa, kayıt için kullanmış olduğunuz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID 'Your Azure Subscription GUID' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Aşağıdaki betiği kullanarak Market dağıtım aracı 'nın en son sürümünü indirin:  

   ```powershell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Dağıtım modülünü içeri aktarın ve ardından aşağıdaki komutları çalıştırarak aracı başlatın. @No__t-0 ' yı, Azure Marketi 'nden yüklediğiniz dosyaları depolamak için bir konumla değiştirin.

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
   ```

   Bulut ortamını belirten ek `-cloud` bir bayrak olduğunu unutmayın. `Export-AzSOfflineMarketplaceItem` Varsayılan olarak, **azurecyüksek**.

6. Araç çalıştırıldığında, kullanılabilir Market öğelerinin listesi ile aşağıdaki görüntüye benzer bir ekran görmeniz gerekir:

   [![Azure Market öğeleri açılan menüsü](media/azure-stack-download-azure-marketplace-item/image05.png "Azure Market öğeleri")](media/azure-stack-download-azure-marketplace-item/image05.png#lightbox)

7. İndirmek istediğiniz öğeyi seçin ve **sürümü**bir yere dikkat edin. Birden çok görüntü seçmek için **CTRL** tuşunu basılı tutabilirsiniz. Bir sonraki yordamda öğeyi içeri aktardığınızda *sürüme* başvurırsınız.

   Ayrıca, **Ölçüt Ekle** seçeneğini kullanarak görüntü listesine filtre uygulayabilirsiniz.

8. **Tamam**' ı seçin ve yasal koşulları gözden geçirin ve kabul edin.

9. İndirme süresi öğenin boyutuna bağlıdır. İndirme tamamlandıktan sonra öğe, betikte belirttiğiniz klasörde kullanılabilir. İndirme, bir VHD dosyası (sanal makineler için) veya bir. zip dosyası (sanal makine uzantıları için) içerir. Tek bir. zip dosyası olan *. azpkg* biçiminde bir galeri paketi de içerebilir.

10. İndirme başarısız olursa, aşağıdaki PowerShell cmdlet 'ini yeniden çalıştırarak tekrar deneyebilirsiniz:

    ```powershell
    Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
    ```

    Yeniden denemeden önce, indirmesi başarısız olan ürün klasörünü kaldırın. Örneğin, indirme sırasında `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1`karşıdan yükleme betiği başarısız olursa, `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` klasörü kaldırın ve ardından cmdlet 'i yeniden çalıştırın.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1811-and-higher"></a>Azure Stack marketi 'ne (1811 ve üzeri) indirme ve yayımlamayı içeri aktarma

1. [Daha önce](#use-the-marketplace-syndication-tool-to-download-marketplace-items) yerel olarak indirdiğiniz dosyaları Azure Stack ortamınızda kullanılabilir olacak şekilde taşımanız gerekir. İçeri aktarma işlemini gerçekleştirmek için aracı kullanmanız gerektiğinden, Market dağıtım aracı Azure Stack ortamınız için de kullanılabilir olmalıdır.

   Aşağıdaki görüntüde bir klasör yapısı örneği gösterilmektedir. `D:\downloadfolder`indirilen tüm Market öğelerini içerir. Her alt klasör, ürün KIMLIĞI tarafından adlandırılan bir market öğesidir (örneğin, `microsoft.custom-script-linux-arm-2.0.3`). Her alt klasörün içinde Market öğesinin indirilen içeriği vardır.

   [![Market indirme dizin yapısı](media/azure-stack-download-azure-marketplace-item/mp1sm.png "Market indirme dizin yapısı")](media/azure-stack-download-azure-marketplace-item/mp1.png#lightbox)

2. Azure Stack Işleci PowerShell oturumunu yapılandırmak için [Bu makaledeki](azure-stack-powershell-configure-admin.md) yönergeleri izleyin.

3. Dağıtım modülünü içeri aktarın ve ardından aşağıdaki betiği çalıştırarak Market dağıtım aracı 'nı başlatın:

   ```powershell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

   @No__t-0 parametresi, indirilen tüm ürünleri içeren en üst düzey klasörü belirtir; Örneğin, `"D:\downloadfolder"`.

   `-AzsCredential` Parametresi isteğe bağlıdır. Süresi dolmuşsa, erişim belirtecini yenilemek için kullanılır. @No__t-0 parametresi belirtilmemişse ve belirtecin süresi dolarsa, işleç kimlik bilgilerini girmek için bir istem alırsınız.

    > [!NOTE]  
    > AD FS yalnızca Kullanıcı kimlikleriyle etkileşimli kimlik doğrulamasını destekler. Bir kimlik bilgisi nesnesi gerekliyse, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS kimlik yönetimi hizmetiniz olarak bir hizmet sorumlusu ayarlama hakkında daha fazla bilgi için bkz. [AD FS hizmet sorumlusunu yönetme](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal).

4. Betik başarıyla tamamlandıktan sonra, öğe Azure Stack marketi 'nde kullanılabilir olmalıdır.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1809-and-lower"></a>Azure Stack marketi 'ne (1809 ve daha düşük) indirme ve yayımlamayı içeri aktarma

1. [Daha önce indirdiğiniz](#use-the-marketplace-syndication-tool-to-download-marketplace-items) sanal makine görüntülerinin veya çözüm şablonlarının dosyaları, Azure Stack ortamınızda yerel olarak kullanılabilir hale getirilmelidir.  

2. Market öğe paketini (. azpkg dosyası) ve sanal sabit disk görüntüsünü (. vhd dosyası) blob depolamaya Azure Stack yüklemek için Yönetici portalını kullanın. Paketin ve disk dosyalarının karşıya yüklenmesi, daha sonra öğeyi Azure Stack Market 'Te yayımlayabilmeniz için Azure Stack kullanılabilir hale getirir.

   Karşıya yükleme, genel olarak erişilebilen bir kapsayıcıya sahip bir depolama hesabınız olmasını gerektirir (Bu senaryoya yönelik önkoşullara bakın).  
   1. Azure Stack Yönetici portalı ' nda **tüm hizmetler** ' e gidin ve ardından **veri + depolama** kategorisi altında **depolama hesapları**' nı seçin.  

   2. Aboneliğinizden bir depolama hesabı seçin ve ardından **BLOB hizmeti**altında **kapsayıcılar**' ı seçin.  
      [![Depolama hesabı ve BLOB hizmeti](media/azure-stack-download-azure-marketplace-item/blob-service.png "BLOB hizmeti") seçin](media/azure-stack-download-azure-marketplace-item/blob-service.png#lightbox)  

   3. Kullanmak istediğiniz kapsayıcıyı seçin ve ardından **karşıya** Yükle ' yi seçerek **BLOB yükle** bölmesini açın.  
      [![Kapsayıcı seçin ve BLOB kapsayıcısını karşıya yükleyin](media/azure-stack-download-azure-marketplace-item/container.png "")](media/azure-stack-download-azure-marketplace-item/container.png#lightbox)  

   4. Blobu karşıya yükle bölmesinde, depolama alanına yüklemek için paket ve disk dosyalarına gidip **karşıya yükle**' yi seçin: [![Karşıya yükleme blobu bölmesini](media/azure-stack-download-azure-marketplace-item/uploadsm.png "karşıya yükle")](media/azure-stack-download-azure-marketplace-item/upload.png#lightbox)  

   5. Karşıya yüklediğiniz dosyalar kapsayıcı bölmesinde görünür. Bir dosya seçin ve sonra **BLOB özellikleri** bölmesinden URL 'yi kopyalayın. Market öğesini Azure Stack aktardığınızda bir sonraki adımda bu URL 'YI kullanacaksınız.  Aşağıdaki görüntüde kapsayıcı **BLOB-test-Storage** , dosya ise **Microsoft. WindowsServer2016DatacenterServerCore-ARM. 1.0.801. azpkg**olur. Dosya URL 'SI **https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg** .  
      [![BLOB özellikleri](media/azure-stack-download-azure-marketplace-item/blob-storagesm.png "BLOB özellikleri")](media/azure-stack-download-azure-marketplace-item/blob-storage.png#lightbox)  

3. **Add-Azsplatformımage** cmdlet 'INI kullanarak VHD görüntüsünü Azure Stack içeri aktarın. Bu cmdlet 'i kullandığınızda `publisher`, `offer` ve diğer parametre değerlerini, içeri aktardığınız görüntünün değerleriyle değiştirin.

   . Azpkg dosyasıyla `publisher`indirilen `offer`metin dosyasından `sku` görüntünün, ve değerlerini alabilirsiniz. Metin dosyası hedef konumda depolanır. `version` Değer, önceki yordamda Azure 'dan öğe indirilirken belirtilen sürümdür.

   Aşağıdaki örnek betikte, Windows Server 2016 Datacenter-Server Core sanal makinesi için değerler kullanılır. Değeri `-Osuri` , öğe için BLOB depolama konumunun örnek bir yoludur.

   Bu betiğe alternatif olarak, Azure portal kullanarak VHD görüntüsünü içeri aktarmak için [Bu makalede açıklanan yordamı](azure-stack-add-vm-image.md#add-a-vm-image-through-the-portal) kullanabilirsiniz.

   ```powershell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.vhd"  
   ```

   **Çözüm şablonları hakkında:** Bazı şablonlar, **fixed3. vhd**adlı küçük 3 MB 'LıK bir VHD dosyası içerebilir. Bu dosyayı Azure Stack aktarmanız gerekmez. Fixed3. vhd dosyası, Azure Marketi için yayımlama gereksinimlerini karşılamak üzere bazı çözüm şablonlarına dahildir.

   Şablon açıklaması ' nı gözden geçirin ve ardından çözüm şablonuyla çalışması gereken VHD 'ler gibi ek gereksinimleri içeri aktarın.  

   **Uzantılar hakkında:** Sanal makine görüntü uzantılarıyla çalışırken aşağıdaki parametreleri kullanın:
   - *Yayımcı*
   - *Tür*
   - *Sürüm*  

   Uzantılar için *teklif* kullanmayın.

4. **Add-Azsgalleritem** cmdlet 'ini kullanarak market öğesini Azure Stack yayımlamak için PowerShell kullanın. Örneğin:

    ```powershell
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     -Verbose
    ```

5. Bir galeri öğesini yayımladıktan sonra, artık kullanılabilir. Galeri öğesinin yayımlandığını doğrulamak için, **tüm hizmetler**' e gidin ve **genel** kategorisi altında **Market**' i seçin.  İndirmeniz bir çözüm şablonu ise, bu çözüm şablonu için herhangi bir bağımlı VHD görüntüsü eklediğinizden emin olun.  
  [![Market 'ı görüntüle](media/azure-stack-download-azure-marketplace-item/view-marketplacesm.png "Market 'ı görüntüle")](media/azure-stack-download-azure-marketplace-item/view-marketplace.png#lightbox)  

Azure Stack PowerShell 1.3.0 sürümü ile artık sanal makine uzantıları ekleyebilirsiniz. Örneğin:

```powershell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
```

## <a name="next-steps"></a>Sonraki adımlar

[Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
