---
title: Azure 'dan Market öğelerini indirin ve Azure Stack yayımlayın | Microsoft Docs
description: Azure 'dan Market öğelerini indirme ve Azure Stack yayımlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: sethm
ms.reviewer: ihcherie
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: bc696d4b14aecd5890893f00b64cf2c4a3804173
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74299227"
---
# <a name="download-existing-marketplace-items-from-azure-and-publish-to-azure-stack"></a>Azure 'dan mevcut Market öğelerini indirin ve Azure Stack yayımlayın

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

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

    Kullanılabilir alanı gözden geçirmek için: **Bölge yönetimi**'nde, araştırmak istediğiniz bölgeyi seçin ve ardından **kaynak sağlayıcıları** > **depolama alanına**gidin:

    ![Azure Stack yönetici portalındaki depolama alanını gözden geçirme](media/azure-stack-download-azure-marketplace-item/storage.png)

3. Azure Stack marketi 'ni açın ve Azure 'a bağlanın. Bunu yapmak için **Market yönetimi** hizmetini seçin, **Market öğeleri**' ni seçin ve ardından **Azure 'dan Ekle**' yi seçin:

    ![Azure 'dan Market öğeleri ekleme](media/azure-stack-download-azure-marketplace-item/marketplace.png)

4. Portal, Azure Marketi 'nden yüklenebilecek öğelerin listesini görüntüler. Ürünleri ada, yayımcıya ve/veya ürün türüne göre filtreleyebilirsiniz. Her satır öğesi de şu anda kullanılabilir sürümü gösterir. Market öğesinin birden fazla sürümü varsa, **Sürüm** sütununda **birden çok**görüntülenir. Açıklamasını ve yükleme boyutu da dahil olmak üzere ek bilgileri görüntülemek için her bir öğeye tıklayabilirsiniz:

    [![Market öğelerinin listesi](media/azure-stack-download-azure-marketplace-item/add-from-azure1sm.png "Market öğelerinin listesi")](media/azure-stack-download-azure-marketplace-item/add-from-azure1.png#lightbox)

5. Bir öğenin sürümü **birden çok**olarak gösteriliyorsa, bu öğeyi seçip ortaya çıkan sürüm Seçicisi açılır listesinden belirli bir sürümü seçebilirsiniz:

    [![Sürüm seçin](media/azure-stack-download-azure-marketplace-item/add-from-azure3sm.png "Sürüm seçin")](media/azure-stack-download-azure-marketplace-item/add-from-azure3.png#lightbox)

6. İstediğiniz öğeyi seçin ve ardından **İndir**' i seçin. İndirme süreleri farklılık gösterir.

    ![Azure Marketi öğesi indiriliyor](media/azure-stack-download-azure-marketplace-item/image04.png)

    Yükleme tamamlandıktan sonra, yeni Market öğesini bir Azure Stack işleci veya bir kullanıcı olarak dağıtabilirsiniz.

7. İndirilen öğeyi dağıtmak için **+ kaynak oluştur**' u seçin ve ardından yeni Market öğesiyle ilgili kategoriler arasında arama yapın. Sonra dağıtım işlemine başlamak için öğeyi seçin. İşlem, farklı Market öğeleri için farklılık gösterir.

## <a name="disconnected-or-a-partially-connected-scenario"></a>Bağlantısı kesik veya kısmen bağlı bir senaryo

Azure Stack bağlantısı kesik bir modundaysanız, Market öğelerini internet bağlantısı olan bir makineye indirmek için PowerShell ve *Market dağıtım aracı* 'nı kullanırsınız. Daha sonra öğeleri Azure Stack ortamınıza aktarabilirsiniz. Bağlantısı kesilmiş bir ortamda, Azure Stack portalını kullanarak Market öğelerini indiremez.

Market dağıtım aracı, bağlı bir senaryoda da kullanılabilir.

Bu senaryonun iki bölümü vardır:

- **1. Bölüm:** Azure Marketi 'nden indirin. İnternet erişimi olan bilgisayarda PowerShell 'i yapılandırın, dağıtım aracını indirin ve ardından Azure Marketi 'nden öğeleri indirin.  
- **Bölüm 2:** Azure Stack Market 'e yükleyin ve yayımlayın. İndirdiğiniz dosyaları Azure Stack ortamınıza taşıyın, Azure Stack içeri aktarıp Azure Stack Market 'Te yayımlayabilirsiniz.  

### <a name="prerequisites"></a>Önkoşullar

- Bağlı bir ortam (Azure Stack olması gerekmez). Azure 'dan ayrıntıların bulunduğu ürünlerin listesini almak ve her şeyi yerel olarak indirmek için bağlantı gerekir. Bu işlem yapıldıktan sonra, yordamın geri kalanı herhangi bir internet bağlantısı gerektirmez. Bu, bağlantısı kesilen ortamınızda kullanabilmeniz için daha önce indirdiğiniz öğelerin bir kataloğunu oluşturur.

- Bağlantısı kesilen ortamınıza bağlanacak ve tüm gerekli yapıtları aktarabilen bir USB anahtar veya harici sürücü.

- Aşağıdaki önkoşullara sahip bağlantısı kesik Azure Stack ortamı:
  - Azure Stack dağıtımınızın [Azure 'a kayıtlı](azure-stack-registration.md)olması gerekir.
  - İnternet bağlantısına sahip bilgisayarda **Azure Stack PowerShell modülü sürüm 1.2.11** veya üzeri olmalıdır. Henüz yoksa, [Azure Stack özel PowerShell modülleri 'ni yükler](azure-stack-powershell-install.md).
  - İndirilen bir market öğesinin içeri aktarımını etkinleştirmek için [Azure Stack Işlecinin PowerShell ortamının](azure-stack-powershell-configure-admin.md) yapılandırılması gerekir.
  - [Azure Stack araçları](https://github.com/Azure/AzureStack-Tools) GitHub deposunu kopyalayın.

- Genel olarak erişilebilen bir kapsayıcıya (Depolama Blobu) sahip Azure Stack bir [depolama hesabınız](azure-stack-manage-storage-accounts.md) olması gerekir. Kapsayıcıyı Market öğeleri Galeri dosyaları için geçici depolama alanı olarak kullanırsınız. Depolama hesapları ve kapsayıcıları hakkında bilgi sahibi değilseniz bkz. Azure belgelerindeki [Bloblarla çalışma-Azure Portal](/azure/storage/blobs/storage-quickstart-blobs-portal) .

- Market dağıtım aracı ilk yordam sırasında indirilir.

- En iyi indirme performansı için [AzCopy](/azure/storage/common/storage-use-azcopy) 'i yükleyebilirsiniz, ancak bu gerekli değildir.

Kaydolduktan sonra, bu, bağlantısı kesik kullanım durumu ile ilgili olmadığından Market yönetimi dikey penceresinde görüntülenen aşağıdaki iletiyi yoksayabilirsiniz:

[![Kayıtlı değil iletisi](media/azure-stack-download-azure-marketplace-item/toolsmsgsm.png "Kayıtlı değil iletisi")](media/azure-stack-download-azure-marketplace-item/toolsmsg.png#lightbox)

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Market öğelerini indirmek için Market dağıtım aracını kullanma

> [!IMPORTANT]
> Market öğelerini, bağlantısı kesilen bir senaryoya her indirişinizde Market dağıtım aracı 'nı indirdiğinizden emin olun. Bu betikte sık yapılan değişiklikler yapıldı ve en güncel sürüm her indirme için kullanılmalıdır.

1. Internet bağlantısı olan bir bilgisayarda, yönetici olarak bir PowerShell konsolu açın.

2. Azure Stack kaydetmek için kullandığınız Azure hesabını ekleyin. Hesabı eklemek için, PowerShell Run `Add-AzureRmAccount` parametresi olmadan. Hesabınızın yapılandırmasına bağlı olarak, Azure hesabı kimlik bilgilerinizi girmeniz istenir ve iki öğeli kimlik doğrulaması kullanmanız gerekebilir.

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
     -DestinationPath `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master
   ```

5. Dağıtım modülünü içeri aktarın ve ardından aşağıdaki komutları çalıştırarak aracı başlatın. `Destination folder path`, Azure Marketi 'nden yüklediğiniz dosyaları depolamak için bir konumla değiştirin.

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
   ```

   `Export-AzSOfflineMarketplaceItem`, bulut ortamını belirten ek bir `-cloud` bayrağına sahip olduğunu unutmayın. Varsayılan olarak, **azurecyüksek**.

6. Araç çalıştırıldığında, kullanılabilir Azure Marketi öğelerinin listesiyle birlikte aşağıdaki görüntüye benzer bir ekran görmeniz gerekir:

   [![Azure Market öğeleri açılan menüsü](media/azure-stack-download-azure-marketplace-item/tool1sm.png "Azure Market öğeleri")](media/azure-stack-download-azure-marketplace-item/tool1.png#lightbox)

7. Market öğesinin birden fazla sürümü varsa, **Sürüm** sütunu **birden çok sürümü**gösterir. Bir öğenin sürümü **birden çok sürüm**olarak gösteriliyorsa, bu öğeyi seçip ortaya çıkan sürüm Seçicisi penceresinden belirli bir sürümü seçebilirsiniz:

   [![Sürüm seçici](media/azure-stack-download-azure-marketplace-item/tool2sm.png "Sürüm seçin")](media/azure-stack-download-azure-marketplace-item/tool2.png#lightbox)

7. Azure Storage araçları 'nı yüklemediyseniz aşağıdaki iletiyi alırsınız. Bu araçları yüklemek için [AzCopy](/azure/storage/common/storage-use-azcopy#download-azcopy)'i indirdiğinizden emin olun:

   ![Depolama araçları](media/azure-stack-download-azure-marketplace-item/vmnew1.png)

8. İndirmek istediğiniz öğeyi seçin ve **sürümü**bir yere dikkat edin. Birden çok görüntü seçmek için **CTRL** tuşunu basılı tutabilirsiniz. Bir sonraki yordamda öğeyi içeri aktardığınızda *sürüme* başvurırsınız.

   Ayrıca, **Ölçüt Ekle** seçeneğini kullanarak görüntü listesine filtre uygulayabilirsiniz.

9. **Tamam**' ı seçin ve yasal koşulları gözden geçirin ve kabul edin.

10. İndirmenin aldığı zaman öğenin boyutuna bağlıdır. İndirme tamamlandıktan sonra öğe, betikte belirttiğiniz klasörde kullanılabilir. İndirme, bir VHD dosyası (sanal makineler için) veya bir. zip dosyası (sanal makine uzantıları için) içerir. Tek bir. zip dosyası olan *. azpkg* biçiminde bir galeri paketi de içerebilir.

11. İndirme başarısız olursa, aşağıdaki PowerShell cmdlet 'ini yeniden çalıştırarak yeniden deneyebilirsiniz:

    ```powershell
    Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
    ```

    Yeniden denemeden önce, indirmesi başarısız olan ürün klasörünü kaldırın. Örneğin, indirme betiği `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1`karşıdan yüklenirken başarısız olursa, `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` klasörünü kaldırın ve ardından cmdlet 'i yeniden çalıştırın.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-using-powershell"></a>PowerShell kullanarak indirme ve Azure Stack marketi 'ne yayımlama

1. [Daha önce](#use-the-marketplace-syndication-tool-to-download-marketplace-items) yerel olarak indirdiğiniz dosyaları Azure Stack ortamınızda kullanılabilir olmaları için taşımanız gerekir. İçeri aktarma işlemini gerçekleştirmek için aracı kullanmanız gerektiğinden, Market dağıtım aracı Azure Stack ortamınız için de kullanılabilir olmalıdır.

   Aşağıdaki görüntüde bir klasör yapısı örneği gösterilmektedir. `D:\downloadfolder` indirilen tüm Market öğelerini içerir. Her alt klasör, ürün KIMLIĞI tarafından adlandırılan bir market öğesidir (örneğin, `microsoft.custom-script-linux-arm-2.0.3`). Her alt klasörün içinde Market öğesinin indirilen içeriği vardır.

   [![Market indirme dizin yapısı](media/azure-stack-download-azure-marketplace-item/mp1sm.png "Market indirme dizin yapısı")](media/azure-stack-download-azure-marketplace-item/mp1.png#lightbox)

2. Azure Stack Işleci PowerShell oturumunu yapılandırmak için [Bu makaledeki](azure-stack-powershell-configure-admin.md) yönergeleri izleyin.

3. Dağıtım modülünü içeri aktarın ve ardından aşağıdaki betiği çalıştırarak Market dağıtım aracı 'nı başlatın:

   ```powershell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

   `-origin` parametresi, indirilen tüm ürünleri içeren en üst düzey klasörü belirtir; Örneğin, `"D:\downloadfolder"`.

   `-AzsCredential` parametresi isteğe bağlıdır. Süresi dolmuşsa, erişim belirtecini yenilemek için kullanılır. `-AzsCredential` parametresi belirtilmemişse ve belirtecin süresi dolarsa, işleç kimlik bilgilerini girmek için bir istem alırsınız.

    > [!NOTE]  
    > AD FS yalnızca Kullanıcı kimlikleriyle etkileşimli kimlik doğrulamasını destekler. Bir kimlik bilgisi nesnesi gerekliyse, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS kimlik yönetimi hizmetiniz olarak bir hizmet sorumlusu ayarlama hakkında daha fazla bilgi için bkz. [AD FS hizmet sorumlusunu yönetme](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal).

4. Betik başarıyla tamamlandıktan sonra, öğe Azure Stack marketi 'nde kullanılabilir olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bir sanal makine görüntüsü ekleme](azure-stack-add-vm-image.md)
- [Özel bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
