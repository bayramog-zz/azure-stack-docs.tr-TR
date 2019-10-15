---
title: Azure Stack bir Market öğesi oluşturma ve yayımlama | Microsoft Docs
description: Azure Stack Market öğesi oluşturma ve yayımlama hakkında bilgi edinin.
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
ms.date: 10/10/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 4a8f24c11f8e72c4b3e2b99ae6b2a417e3bd0cba
ms.sourcegitcommit: 5eae057cb815f151e6b8af07e3ccaca4d8e4490e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310592"
---
# <a name="create-and-publish-a-custom-azure-stack-marketplace-item"></a>Özel bir Azure Stack Market öğesi oluşturma ve yayımlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack marketi 'Nde yayımlanan her öğe Azure Galeri paketi (. azpkg) biçimini kullanır. *Azure Gallery Packager* Aracı, daha sonra kullanıcılar tarafından indirilebilen Azure Stack Market 'e yükleyebileceğiniz özel bir Azure Galeri paketi oluşturmanıza olanak sağlar. Dağıtım işlemi bir Azure Resource Manager şablonu kullanır.

## <a name="marketplace-items"></a>Market öğeleri

Bu makaledeki örneklerde, Windows veya Linux türünde tek bir VM marketi teklifinin nasıl oluşturulacağı gösterilmektedir.

## <a name="create-a-marketplace-item"></a>Bir Market öğesi oluşturma

> [!IMPORTANT]
> VM marketi öğesini oluşturmadan önce, [Azure Stack BIR VM görüntüsü ekleme](azure-stack-add-vm-image.md#add-a-vm-image-as-an-azure-stack-operator-using-the-portal)bölümündeki yönergeleri izleyerek Azure Stack PORTALıNA özel VM görüntüsünü yükleyin. Ardından, bu makaledeki yönergeleri izleyerek görüntüyü paketleyin (bir. azpkg oluşturun) ve Azure Stack Market 'e yükleyin.

Özel bir Market öğesi oluşturmak için aşağıdakileri yapın:

1. [Azure Gallery Packager aracını](https://aka.ms/azsmarketplaceitem) ve örnek Azure Stack Galerisi paketini indirin. Bu indirme, özel VM şablonlarını içerir. . Zip dosyasını ayıklayın ve **Simplevmtemplate** klasörünü Azure Stack portalınızdaki bir öğe adıyla yeniden adlandırın.

2. Windows/Linux için bir Azure Resource Manager şablonu oluşturun veya örnek şablonlarımızı kullanın. Bu örnek şablonlar, 1. adımda indirdiğiniz Paketleyici aracı. zip dosyasında verilmiştir. Şablonu kullanabilir ve metin alanlarını değiştirebilir ya da önceden yapılandırılmış bir şablonu GitHub 'dan indirebilirsiniz. Azure Resource Manager şablonları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları](/azure/azure-resource-manager/resource-group-authoring-templates).

3. Galeri paketi aşağıdaki yapıyı içermelidir:

   :::image type="content" source="media/azure-stack-create-and-publish-marketplace-item/gallerypkg1.png" alt-text="Galeri paketi":::

   Dağıtım şablonları dosya yapısı aşağıdaki gibi görünür:

   :::image type="content" source="media/azure-stack-create-and-publish-marketplace-item/gallerypkg2.png" alt-text="Galeri paketi":::

4. Manifest. JSON şablonundaki aşağıdaki vurgulanan değerleri (sayılarla), [özel görüntünüzü karşıya yüklerken](azure-stack-add-vm-image.md#add-a-vm-image-as-an-azure-stack-operator-using-the-portal)belirttiğiniz değerle değiştirin.

   > [!NOTE]  
   > Ürün anahtarları, parola gibi herhangi bir parolayı veya Azure Resource Manager şablonunda hiçbir müşteri tarafından tanımlanabilir bilgileri hiçbir şekilde kalıcı olarak kodlayın. Şablon JSON dosyalarına, galeride yayımlandıktan sonra kimlik doğrulamaya gerek olmadan erişilebilir. Tüm gizli dizileri [Key Vault](/azure/azure-resource-manager/resource-manager-keyvault-parameter) depolayın ve şablon içinden çağırın.

   Aşağıdaki şablon manifest. json dosyasının bir örneğidir:

    ```json
    {
       "$schema": "https://gallery.azure.com/schemas/2015-10-01/manifest.json#",
       "name": "Test", (1)
       "publisher": "<Publisher name>", (2)
       "version": "<Version number>", (3)
       "displayName": "ms-resource:displayName", (4)
       "publisherDisplayName": "ms-resource:publisherDisplayName", (5)
       "publisherLegalName": "ms-resource:publisherDisplayName", (6)
       "summary": "ms-resource:summary",
       "longSummary": "ms-resource:longSummary",
       "description": "ms-resource:description",
       "longDescription": "ms-resource:description",
       "uiDefinition": {
          "path": "UIDefinition.json" (7)
          },
       "links": [
        { "displayName": "ms-resource:documentationLink", "uri": "http://go.microsoft.com/fwlink/?LinkId=532898" }
        ],
       "artifacts": [
          {
             "name": "<Template name>",
             "type": "Template",
             "path": "DeploymentTemplates\\<Template name>.json", (8)
             "isDefault": true
          }
       ],
       "categories":[ (9)
          "Custom",
          "<Template name>"
          ],
       "images": [{
          "context": "ibiza",
          "items": [{
             "id": "small",
             "path": "icons\\Small.png", (10)
             "type": "icon"
             },
             {
                "id": "medium",
                "path": "icons\\Medium.png",
                "type": "icon"
             },
             {
                "id": "large",
                "path": "icons\\Large.png",
                "type": "icon"
             },
             {
                "id": "wide",
                "path": "icons\\Wide.png",
                "type": "icon"
             }]
        }]
    }
    ```

    Aşağıdaki listede örnek şablonda önceki numaralandırılmış değerler açıklanmaktadır:

    - (1) – teklifin adı.
    - (2) – bir boşluk olmadan yayımcının adı.
    - (3) – bir boşluk olmadan şablonunuzun sürümü.
    - (4) – müşterilerin göreceği ad.
    - (5) – müşterilerin göreceği yayımcı adı.
    - (6) – yayımcının yasal adı.
    - (7): **Uıdefinition. JSON** dosyanızın depolandığı yol.  
    - (8): JSON ana şablon dosyanızın yolu ve adı.
    - (9) – bu şablonun görüntülendiği kategorilerin adları.
    - (10) – her simgenin yolu ve adı.

5. **MS-Resource**' a başvuran tüm alanlar için **dizeler/kaynaklar. JSON** dosyası içinde uygun değerleri değiştirmeniz gerekir:

    ```json
    {
    "displayName": "<OfferName.PublisherName.Version>",
    "publisherDisplayName": "<Publisher name>",
    "summary": "Create a simple VM",
    "longSummary": "Create a simple VM and use it",
    "description": "<p>This is just a sample of the type of description you could create for your gallery item!</p><p>This is a second paragraph.</p>",
    "documentationLink": "Documentation"
    }
    ```

    ![Package görüntü @ no__t-1 ![Package Display @ no__t-3

6. Kaynağın başarıyla dağıtılabilmesi için [Azure Stack API 'leriyle](../user/azure-stack-profiles-azure-resource-manager-versions.md)şablonu test edin.

7. Şablonunuz bir sanal makine (VM) görüntüsünü kullanıyorsa, [Azure Stack BIR VM görüntüsü eklemek](azure-stack-add-vm-image.md)için yönergeleri izleyin.

8. Azure Resource Manager şablonunuzu **/contoso.ToDoList/DeploymentTemplates/** klasörüne kaydedin.

9. Market öğesi için simgeleri ve metni seçin. **Simgeler** klasörüne simgeler ekleyin ve **dizeler** klasöründeki **kaynaklar** dosyasına metin ekleyin. Simgeler için **küçük**, **Orta**, **büyük**ve **geniş** adlandırma kurallarını kullanın. Bu boyutlarda ayrıntılı bir açıklama için [Market öğesi Kullanıcı arabirimi başvurusuna](#reference-marketplace-item-ui) bakın.

    > [!NOTE]
    > Market öğesini doğru bir şekilde oluşturmak için dört, orta, büyük, geniş) tüm dört simge boyutu gereklidir.

10. Manifest. JSON hakkında daha fazla düzenleme için bkz. [Başvuru: market öğe bildirimi. JSON](#reference-marketplace-item-manifestjson).

11. Dosyalarınızı değiştirmeyi bitirdiğinizde bir. azpkg dosyasına dönüştürün. Dönüştürme işlemini **Azuregallerypackager. exe** aracını ve daha önce indirdiğiniz örnek Galeri paketini kullanarak gerçekleştirirsiniz. Şu komutu çalıştırın:

    ```shell
    .\AzureGalleryPackager.exe package –m c:\<path>\<gallery package name>\manifest.json –o c:\Temp
    ```

    > [!NOTE]
    > Çıkış yolu, seçtiğiniz herhangi bir yol olabilir ve C: sürücüsü altında olması gerekmez. Ancak, hem manifest. json dosyasının hem de çıkış paketinin tam yolu var olmalıdır. Örneğin, çıkış yolu `C:\<path>\galleryPackageName.azpkg` ise, `C:\<path>` klasörü var olmalıdır.
    >
    >

## <a name="publish-a-marketplace-item"></a>Bir Market öğesi yayımlama

1. Market öğesini (. azpkg) Azure Blob depolama alanına yüklemek için PowerShell veya Azure Depolama Gezgini kullanın. Yerel Azure Stack depolama alanına yükleyebilir veya paket için geçici bir konum olan Azure Storage 'a yükleyebilirsiniz. Blobun genel olarak erişilebilir olduğundan emin olun.

2. Galeri paketini Azure Stack içine aktarmak için ilk adım, yeni oluşturduğunuz dosyayı Azure Stack kopyalamak amacıyla istemci sanal makinesine uzaktan bağlanmamalıdır.

3. Bağlam ekleyin:

    ```powershell
    $ArmEndpoint = "https://adminmanagement.local.azurestack.external"
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
    ```

4. Kaynağı galerinize aktarmak için aşağıdaki betiği çalıştırın:

    ```powershell
    Add-AzsGalleryItem -GalleryItemUri `
    https://sample.blob.core.windows.net/<temporary blob name>/<offerName.publisherName.version>.azpkg –Verbose
    ```

5. Öğesini depolamak için kullanılabilir geçerli bir depolama hesabınız olduğunu doğrulayın. Azure Stack yönetici portalından `GalleryItemURI` değerini alabilirsiniz. **Depolama hesabı-> blob özellikleri-** . azpkg UZANTıSıYLA > URL 'yi seçin. Depolama hesabı, Market 'te yayımlamak için yalnızca geçici kullanım içindir.

   Galeri paketinizi tamamladıktan ve **Add-Azsgalleritem**kullanarak karşıya yükledikten sonra, özel VM 'Niz artık Market 'te ve **kaynak oluştur** görünümünde yer almalıdır. Özel Galeri paketinin **Market yönetimi**'nde görülemeyeceğini unutmayın.

   [![Özel Market öğesi karşıya yüklendi](media/azure-stack-create-and-publish-marketplace-item/pkg6sm.png "özel Market öğesi karşıya yüklendi")](media/azure-stack-create-and-publish-marketplace-item/pkg6.png#lightbox)

6. Öğe Market 'e başarıyla yayımlandıktan sonra, içeriği depolama hesabından silebilirsiniz.

   > [!CAUTION]  
   > Tüm varsayılan Galeri yapıtlarına ve özel galeri yapılarınıza artık aşağıdaki URL 'Ler altında kimlik doğrulaması yapılmadan erişilebilir:  
   `https://adminportal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`
   `https://portal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`

6. **Remove-AzureRMGalleryItem** cmdlet 'Ini kullanarak Market öğesini kaldırabilirsiniz. Örnek:

   ```powershell
   Remove-AzsGalleryItem -Name <Gallery package name> -Verbose
   ```

   > [!NOTE]
   > Bir öğeyi kaldırdıktan sonra Market Kullanıcı arabirimi bir hata gösterebilir. Hatayı onarmak için portalda **Ayarlar** ' a tıklayın. Ardından, **Portal özelleştirmesi**altında **değişiklikleri at** ' ı seçin.
   >
   >

## <a name="reference-marketplace-item-manifestjson"></a>Başvuru: market öğe bildirimi. JSON

### <a name="identity-information"></a>Kimlik bilgileri

| Adı | Gereklidir | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| Adı |X |Dize |[A-Za-z0-9] + | |
| Yayımcı |X |Dize |[A-Za-z0-9] + | |
| Sürüm |X |Dize |[SemVer v2](https://semver.org/) | |

### <a name="metadata"></a>Meta Veriler

| Adı | Gereklidir | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |80 karakter önerisi |Portal 80 karakterden uzunsa öğe adınızı doğru görüntülenmeyebilir. |
| PublisherDisplayName |X |Dize |30 karakterlik öneri |Portal, 30 karakterden uzun olursa Yayımcı adınızı doğru görüntülenmeyebilir. |
| PublisherLegalName |X |Dize |En fazla 256 karakter | |
| Özet |X |Dize |60-100 karakter | |
| LongSummary |X |Dize |140-256 karakter |Henüz Azure Stack için geçerli değildir. |
| Açıklama |X |[HTML](https://github.com/Azure/portaldocs/blob/master/gallery-sdk/generated/index-gallery.md#gallery-item-metadata-html-sanitization) |500-5.000 karakter | |

### <a name="images"></a>Resimler

Market aşağıdaki simgeleri kullanır:

| Adı | Genişlik | Yükseklik | Notlar |
| --- | --- | --- | --- |
| Geniş |255 piksel |115 piksel |Her zaman gerekli |
| Büyük |115 piksel |115 piksel |Her zaman gerekli |
| Orta |90 piksel |90 piksel |Her zaman gerekli |
| Küçük |40 piksel |40 piksel |Her zaman gerekli |
| Yakala |533 piksel |324 piksel |Her zaman gerekli |

### <a name="categories"></a>Kategoriler

Her Market öğesi, öğenin Portal Kullanıcı arabiriminde nerede göründüğünü tanımlayan bir kategori ile etiketlenmelidir. Azure Stack (**işlem**, **veri + depolama**vb.) var olan kategorilerden birini seçebilir veya yeni bir tane seçebilirsiniz.

### <a name="links"></a>Bağlantılar

Her Market öğesi, ek içeriğe yönelik çeşitli bağlantılar içerebilir. Bağlantılar adların ve URI 'lerin listesi olarak belirtilir:

| Adı | Gereklidir | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 64 karakter. | |
| Kullanılmamışsa |X |KULLANıLMAMıŞSA | | |

### <a name="additional-properties"></a>Ek özellikler

Market yazarları, önceki meta verilere ek olarak aşağıdaki biçimde özel anahtar/değer çifti verileri sağlayabilir:

| Adı | Gereklidir | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 25 karakter. | |
| Değer |X |Dize |En fazla 30 karakter. | |

### <a name="html-sanitization"></a>HTML temizleme

HTML 'ye izin veren herhangi bir alan için aşağıdaki [öğelere ve özniteliklere izin verilir](https://github.com/Azure/portaldocs/blob/master/gallery-sdk/generated/index-gallery.md#gallery-item-metadata-html-sanitization):

`h1, h2, h3, h4, h5, p, ol, ul, li, a[target|href], br, strong, em, b, i`

## <a name="reference-marketplace-item-ui"></a>Başvuru: market öğe kullanıcı arabirimi

Azure Stack portalında görüldüğü gibi Market öğeleri için simgeler ve metin aşağıdaki gibidir.

### <a name="create-blade"></a>Dikey pencere oluşturma

![Dikey pencere oluşturma — Market öğelerini Azure Stack](media/azure-stack-create-and-publish-marketplace-item/image1.png)

### <a name="marketplace-item-details-blade"></a>Market öğesi ayrıntıları dikey penceresi

![Azure Stack Market öğesi ayrıntıları dikey penceresi](media/azure-stack-create-and-publish-marketplace-item/image3.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md)
- [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
- [Azure Resource Manager şablonlarının biçimi ve yapısı](/azure/azure-resource-manager/resource-group-authoring-templates)
