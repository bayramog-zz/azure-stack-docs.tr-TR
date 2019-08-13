---
title: Azure Stack bir Market öğesi oluşturma ve yayımlama | Microsoft Docs
description: Azure Stack bir Market öğesi oluşturun ve yayımlayın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 24fc0f7993001ce95a21e175c84f37d755a5ce6c
ms.sourcegitcommit: ec38ec569ad2193369c438f55e5c190aa5f0efd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68956594"
---
# <a name="create-and-publish-a-marketplace-item"></a>Market öğesi oluşturma ve yayımlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="create-a-marketplace-item"></a>Bir Market öğesi oluşturma

1. [Azure Gallery Packager aracını](https://www.aka.ms/azurestackmarketplaceitem) ve örnek Azure Stack Market öğesini indirin.
2. Örnek Market öğesini açın ve **Simplevmtemplate** klasörünü yeniden adlandırın. Market öğesiyle aynı adı kullanın; Örneğin, **contoso. ToDoList**. Bu klasör şunları içerir:

   ```shell
   /Contoso.TodoList/
   /Contoso.TodoList/Manifest.json
   /Contoso.TodoList/UIDefinition.json
   /Contoso.TodoList/Icons/
   /Contoso.TodoList/Strings/
   /Contoso.TodoList/DeploymentTemplates/
   ```

3. [Bir Azure Resource Manager şablonu oluşturun](/azure/azure-resource-manager/resource-group-authoring-templates) veya GitHub 'dan bir şablon seçin. Market öğesi bu şablonu kullanarak bir kaynak oluşturur.

    > [!NOTE]  
    > Ürün anahtarları, parola gibi herhangi bir parolayı veya Azure Resource Manager şablonunda hiçbir müşteri tarafından tanımlanabilir bilgileri hiçbir şekilde kalıcı olarak kodlayın. Şablon JSON dosyalarına, galeride yayımlandıktan sonra kimlik doğrulamaya gerek olmadan erişilebilir. Tüm gizli dizileri [Key Vault](/azure/azure-resource-manager/resource-manager-keyvault-parameter) depolayın ve şablon içinden çağırın.

4. Kaynağın başarıyla dağıtılabilmesi için Microsoft Azure Stack API 'Leriyle şablonu test edin.
5. Şablonunuz bir sanal makine görüntüsünü kullanıyorsa, [Azure Stack bir sanal makine görüntüsü eklemek](azure-stack-add-vm-image.md)için yönergeleri izleyin.
6. Azure Resource Manager şablonunuzu **/contoso.ToDoList/DeploymentTemplates/** klasörüne kaydedin.
7. Market öğesi için simgeleri ve metni seçin. **Simgeler** klasörüne simgeler ekleyin ve **dizeler** klasöründeki **kaynaklar** dosyasına metin ekleyin. Simgeler için **küçük**, **Orta**, **büyük**ve **geniş** adlandırma kurallarını kullanın. Bu boyutlarda ayrıntılı bir açıklama için [Market öğesi Kullanıcı arabirimi başvurusuna](#reference-marketplace-item-ui) bakın.

   > [!NOTE]
   > Market öğesini doğru bir şekilde oluşturmak için dört, orta, büyük, geniş) tüm dört simge boyutu gereklidir.
   >
   >

8. **Manifest. JSON** dosyasında **ad** ' ı Market öğesinin adıyla değiştirin. Ayrıca, **yayımcıyı** adınız veya şirketiniz olarak değiştirin.
9. **Yapıtlar**bölümünde **adı** ve **yolu** , eklediğiniz Azure Resource Manager şablonu için doğru bilgiler olarak değiştirin:

   ```json
   "artifacts": [
      {
          "name": "Your template name",
          "type": "Template",
          "path": "DeploymentTemplates\\your path",
          "isDefault": true
      }
   ```

10. Market **öğelerinizi** , Market öğelerinizin görünmesi gereken kategorilerin bir listesiyle değiştirin:

    ```json
    "categories":[
    "My Marketplace Items"
    ],
    ```

11. Manifest. JSON hakkında daha fazla düzenleme için, [başvuruya bakın: Market öğe bildirimi. JSON](#reference-marketplace-item-manifestjson).

12. Klasörleri bir. azpkg dosyasına paketlemek için, bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    ```shell
    AzureGalleryPackager.exe package -m <absolute path to manifest.json> -o <output location for the package>
    ```

    > [!NOTE]
    > Manifest. json dosyasının yanı sıra çıkış paketinin da tam yolu var olmalıdır. Örneğin, çıkış yolu C:\marketplaceıtem\yourpackage.exe. azpkg ise, **C:\marketplaceitem** klasörü mevcut olmalıdır.
    >
    >

## <a name="publish-a-marketplace-item"></a>Bir Market öğesi yayımlama

1. Market öğesini (. azpkg) Azure Blob depolama alanına yüklemek için PowerShell veya Azure Depolama Gezgini kullanın. Yerel Azure Stack depolama alanına yükleyebilir veya Azure Storage 'a yükleyebilirsiniz; Bu, paket için geçici bir konum olur. Blobun genel olarak erişilebilir olduğundan emin olun.
2. Microsoft Azure Stack ortamındaki istemci sanal makinesinde, PowerShell oturumunuzun Hizmet Yöneticisi kimlik bilgilerinizle ayarlandığından emin olun. PowerShell [ile bir şablon dağıtma](../user/azure-stack-deploy-template-powershell.md)Azure Stack içinde PowerShell kimlik doğrulaması hakkında yönergeler bulabilirsiniz.
3. [PowerShell 1.3.0](azure-stack-powershell-install.md) veya üstünü kullandığınızda, market öğesini Azure Stack yayımlamak için **Add-Azsgalleryıtem** PowerShell cmdlet 'ini kullanabilirsiniz. PowerShell 1.3.0 kullanmadan önce Add- **Azurermgalleryıtem** cmdlet 'ini **Add-azsgalleryıtem**yerine kullanın. Örneğin, PowerShell 1.3.0 veya üstünü kullandığınızda:

   ```powershell
   Add-AzsGalleryItem -GalleryItemUri `
   https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg -Verbose
   ```

   | Parametre | Açıklama |
   | --- | --- |
   | SubscriptionID |Yönetici abonelik KIMLIĞI. PowerShell 'i kullanarak alabilirsiniz. Portal 'da almayı tercih ediyorsanız, sağlayıcı aboneliğine gidin ve abonelik KIMLIĞINI kopyalayın. |
   | Gallerıtemuri |Zaten depolamaya yüklenmiş olan Galeri paketiniz için blob URI 'SI. |
   | Apiversion |**2015-04-01**olarak ayarlayın. |

4. Portala gidin. Artık Portal 'daki Market öğesini bir operatör veya Kullanıcı olarak görebilirsiniz. Paketin görüntülenmesi birkaç dakika sürebilir.

5. Market aboneliğiniz artık Azure Stack Market 'e kaydedildi. BLOB depolama konumunuzda silmeyi seçebilirsiniz.

    > [!CAUTION]  
    > Tüm varsayılan Galeri yapıtlarına ve özel galeri yapılarınıza artık aşağıdaki URL 'Ler altında kimlik doğrulaması yapılmadan erişilebilir:  
`https://adminportal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`
`https://portal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`
`https://systemgallery.blob.[Region].[external FQDN]/dev20161101-microsoft-windowsazure-gallery/[Template Name]/UiDefinition.json`

6. **Remove-AzureRMGalleryItem** cmdlet 'Ini kullanarak Market öğesini kaldırabilirsiniz. Örneğin:

   ```powershell
   Remove-AzsGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  -Verbose
   ```

   > [!NOTE]
   > Bir öğeyi kaldırdıktan sonra Market Kullanıcı arabirimi bir hata gösterebilir. Hatayı onarmak için portalda **Ayarlar** ' a tıklayın. Ardından, **Portal özelleştirmesi**altında **değişiklikleri at** ' ı seçin.
   >
   >

## <a name="reference-marketplace-item-manifestjson"></a>Referans: Market öğe bildirimi. JSON

### <a name="identity-information"></a>Kimlik bilgileri

| Ad | Gerekli | Type | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| Ad |X |Dize |[A-Za-z0-9]+ | |
| Yayımcı |X |Dize |[A-Za-z0-9]+ | |
| Version |X |Dize |[SemVer v2](https://semver.org/) | |

### <a name="metadata"></a>Meta Veriler

| Ad | Gerekli | Type | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |80 karakter önerisi |Portal 80 karakterden uzunsa öğe adınızı doğru görüntülenmeyebilir. |
| PublisherDisplayName |X |Dize |30 karakterlik öneri |Portal, 30 karakterden uzunsa Yayımcı adınızı doğru şekilde görüntülemeyebilir. |
| PublisherLegalName |X |Dize |En fazla 256 karakter | |
| Özet |X |Dize |60-100 karakter | |
| LongSummary |X |Dize |140-256 karakter |Henüz Azure Stack için geçerli değildir. |
| Açıklama |X |[HTML](https://github.com/Azure/portaldocs/blob/master/gallery-sdk/generated/index-gallery.md#gallery-item-metadata-html-sanitization) |500-5.000 karakter | |

### <a name="images"></a>Görüntüler

Market aşağıdaki simgeleri kullanır:

| Ad | Genişlik | Yükseklik | Notlar |
| --- | --- | --- | --- |
| Geniş |255 px |115 piksel |Her zaman gerekli |
| Büyük |115 piksel |115 piksel |Her zaman gerekli |
| Orta |90 piksel |90 piksel |Her zaman gerekli |
| Küçük |40 piksel |40 piksel |Her zaman gerekli |
| Ekran Görüntüsü |533 piksel |324 piksel |Her zaman gerekli |

### <a name="categories"></a>Categories

Her Market öğesi, öğenin Portal Kullanıcı arabiriminde nerede göründüğünü tanımlayan bir kategori ile etiketlenmelidir. Azure Stack (**işlem**, **veri + depolama**, vb.) içindeki mevcut kategorilerden birini seçebilir veya yeni bir tane seçebilirsiniz.

### <a name="links"></a>Bağlantılar

Her Market öğesi, ek içeriğe yönelik çeşitli bağlantılar içerebilir. Bağlantılar adların ve URI 'lerin listesi olarak belirtilir:

| Ad | Gerekli | Type | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 64 karakter. | |
| Uri |X |URI | | |

### <a name="additional-properties"></a>Ek özellikler

Market yazarları, önceki meta verilere ek olarak aşağıdaki biçimde özel anahtar/değer çifti verileri sağlayabilir:

| Ad | Gerekli | Type | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 25 karakter. | |
| Value |X |Dize |En fazla 30 karakter. | |

### <a name="html-sanitization"></a>HTML temizleme

HTML 'ye izin veren herhangi bir alan için aşağıdaki [öğelere ve özniteliklere izin verilir](https://github.com/Azure/portaldocs/blob/master/gallery-sdk/generated/index-gallery.md#gallery-item-metadata-html-sanitization):

`h1, h2, h3, h4, h5, p, ol, ul, li, a[target|href], br, strong, em, b, i`

## <a name="reference-marketplace-item-ui"></a>Referans: Market öğesi Kullanıcı arabirimi

Azure Stack portalında görüldüğü gibi Market öğeleri için simgeler ve metin aşağıdaki gibidir.

### <a name="create-blade"></a>Dikey pencere oluşturma

![Dikey pencere oluşturma](media/azure-stack-create-and-publish-marketplace-item/image1.png)

### <a name="marketplace-item-details-blade"></a>Market öğesi ayrıntıları dikey penceresi

![Market öğesi ayrıntıları dikey penceresi](media/azure-stack-create-and-publish-marketplace-item/image3.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md)
* [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
