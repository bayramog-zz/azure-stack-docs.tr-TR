---
title: Azure Stack bir VM görüntüsü ekleme | Microsoft Docs
description: Azure Stack için bir VM görüntüsü ekleme veya kaldırma hakkında bilgi edinin.
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
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 8fec1b3702aa7c8c55f1a90167b1ac13f0ac8847
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271768"
---
# <a name="add-a-vm-image-to-azure-stack"></a>Azure Stack bir VM görüntüsü ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, kullanıcılarınıza kullanılabilir hale getirmek için Market 'e bir sanal makine (VM) görüntüsü ekleyebilirsiniz. Görüntüler, Azure Stack için Azure Resource Manager şablonlar kullanılarak eklenir. Ayrıca, yönetim portalı veya Windows PowerShell kullanarak Azure Marketi Kullanıcı arabirimine Market öğesi olarak VM görüntüleri ekleyebilirsiniz. Küresel Azure Marketi 'nden veya kendi özel sanal makine görüntbir görüntüsünü kullanın.

## <a name="add-a-vm-image-through-the-portal"></a>Portal aracılığıyla VM görüntüsü ekleme

> [!NOTE]  
> Bu yöntemle Market öğesini ayrı olarak oluşturmanız gerekir.

Görüntülere bir BLOB depolama URI 'SI tarafından başvurulabilmelidir. Bir Windows veya Linux işletim sistemi görüntüsünü VHD biçiminde hazırlayın (VHDX değil) ve ardından görüntüyü Azure veya Azure Stack bir depolama hesabına yükleyin. Resminiz Azure veya Azure Stack BLOB depolama alanına zaten yüklenmişse, 1. adımı atlayabilirsiniz.

1. [WINDOWS VM görüntüsünü kaynak yöneticisi dağıtımlar Için Azure 'A yükleyin](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) veya Linux görüntüsü için, [Azure Stack Linux VM 'leri dağıtma](azure-stack-linux.md)bölümünde açıklanan yönergeleri izleyin. Görüntüyü karşıya yüklemeden önce aşağıdaki faktörleri göz önünde bulundurmanız önemlidir:

   - Azure Stack, sabit disk VHD biçiminde yalnızca bir (1) VM oluşturmayı destekler. Sabit biçim, disk uzaklığında x blob uzaklığında depolanacak şekilde mantıksal diski dosya içinde doğrusal olarak bir şekilde alır. Blob sonundaki küçük bir alt bilgi, VHD 'nin özelliklerini açıklar. Diskinizin düzeltildiğini onaylamak için, [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell komutunu kullanın.  

     > [!IMPORTANT]  
     >  Azure Stack, dinamik disk VHD 'lerini desteklemez. Bir VM 'ye bağlı dinamik bir diskin yeniden boyutlandırılması VM 'nin başarısız bir durumda kalmasını sağlar. Bu sorunu azaltmak için, depolama hesabındaki bir VHD blobu olan VM 'nin diskini silmeden VM 'yi silin. Ardından, VHD 'yi dinamik bir diskten sabit bir diske dönüştürüp VM 'yi yeniden oluşturun.

   - Görüntünün Azure Stack görüntü deposuna gönderimi daha az zaman alacağından, bir görüntüyü Azure Blob depolama alanına göre Azure Stack blob depolamaya yüklemek daha etkilidir.

   - [WINDOWS VM görüntüsünü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/)karşıya yüklediğinizde, [Azure Stack Işlecinin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) adımını kullanarak **oturum açma adımını Azure 'a** geçdiğinizden emin olun.  

   - Görüntüyü karşıya yüklediğiniz BLOB depolama URI 'sini bir yere getirin. BLOB depolama URI 'si şu biçimdedir:  *&lt;storageaccount/&gt;&gt;&lt;blobcontainer/targetvhdname&gt;. vhd.&lt;*

   - Blobu anonim olarak erişilebilir hale getirmek için VM görüntüsü VHD 'sinin karşıya yüklendiği depolama hesabı blob kapsayıcısına gidin. **BLOB**' u ve ardından **erişim ilkesi**' ni seçin. İsteğe bağlı olarak, kapsayıcı için bir paylaşılan erişim imzası oluşturabilir ve bunu blob URI 'sinin bir parçası olarak dahil edebilirsiniz. Bu adım, Blobun kullanılabilir olduğundan emin olur. Blob anonim olarak erişilebilir değilse, VM görüntüsü başarısız bir durumda oluşturulur.

     ![Depolama hesabı bloblarına git](./media/azure-stack-add-vm-image/image1.png)

     ![Blob erişimini herkese ayarla](./media/azure-stack-add-vm-image/image2.png)

2. İşleç olarak Azure Stack için oturum açın. Menüsünde > , **işlem**Ekle > altındaki tüm hizmetler**görüntüleri** ' ni seçin.

3. **Görüntü oluştur**altında, ad, abonelik, kaynak grubu, konum, işletim sistemi diski, işletim sistemi türü, Depolama Blobu URI, hesap türü ve konak önbelleğe alma ' yı girin. Ardından, VM görüntüsünü oluşturmaya başlamak için **Oluştur** ' u seçin.

   ![Görüntüyü oluşturmaya başla](./media/azure-stack-add-vm-image/image4.png)

   Görüntü başarıyla oluşturulduğunda, VM görüntüsü durumu **başarılı**olarak değişir.

4. VM görüntüsünü Kullanıcı ARABIRIMINDE Kullanıcı tüketimine daha kolay kullanılabilir hale getirmek için [Market öğesi oluşturmak](azure-stack-create-and-publish-marketplace-item.md)iyi bir fikirdir.

## <a name="remove-a-vm-image-through-the-portal"></a>Portal aracılığıyla VM görüntüsünü kaldırma

1. Yönetim portalını adresinde [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external)açın.

2. **Market yönetimi**' ni seçin ve ardından sılmek istediğiniz VM 'yi seçin.

3. Tıklayın **Sil**.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak Market 'e VM görüntüsü ekleme

> [!Note]  
> Bir görüntü eklediğinizde yalnızca Azure Resource Manager tabanlı şablonlar ve PowerShell dağıtımları için kullanılabilir. Bir görüntüyü kullanıcılarınız için Market öğesi olarak kullanılabilir hale getirmek için, bu makaledeki adımları kullanarak Market öğesini yayımlayın: [Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)

1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).  

2. Azure Stack için bir operatör olarak oturum açın. Yönergeler için bkz. [Azure Stack bir operatör olarak oturum açma](azure-stack-powershell-configure-admin.md).

3. PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>" `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
   ```

   **Add-Azsplatformımage** cmdlet 'i, Azure Resource Manager ŞABLONLARı tarafından VM görüntüsüne başvurmak için kullanılan değerleri belirtir. Değerler şunlardır:
   - **'ın**  
     Örneğin, `Canonical`  
     Kullanıcıların görüntüyü dağıtırken kullandıkları VM görüntüsünün yayımcı adı segmenti. **Microsoft**bir örnektir. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **sunar**  
     Örneğin, `UbuntuServer`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün teklif adı segmenti. Bir örnek, **windowsserver**' dır. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **isteyin**  
     Örneğin, `14.04.3-LTS`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün SKU ad segmenti. Örnek olarak **Datacenter2016**. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **Sürüm**  
     Örneğin, `1.0.0`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün sürümü. Bu sürüm biçimindedir *\#.\#.\#* . Örnek olarak **1.0.0**. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **osType**  
     Örneğin, `Linux`  
     Görüntünün osType öğesi **Windows** ya da **Linux**olmalıdır.  
   - **Osurı**  
     Örneğin, `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
     İçin bir BLOB depolama URI 'SI belirtebilirsiniz `osDisk`.  

     Daha fazla bilgi için bkz. [Add-Azsplatformımage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet 'ı ve [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet 'i için PowerShell Başvurusu.

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak Market 'e özel bir VM görüntüsü ekleme
 
1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).

   ```powershell
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
   ```

2. **Active Directory Federasyon Hizmetleri (AD FS) (AD FS)** kullanıyorsanız, aşağıdaki cmdlet 'i kullanın:

   ```powershell
   # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
   $ArmEndpoint = "<Resource Manager endpoint for your environment>"

   # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
   $GraphAudience = "<GraphAudience endpoint for your environment>"

   # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external
    ```

3. Azure Stack için bir operatör olarak oturum açın. Yönergeler için bkz. [Azure Stack bir operatör olarak oturum açma](azure-stack-powershell-configure-admin.md).

4. Özel VM görüntünüzü depolamak için genel Azure veya Azure Stack depolama hesabı oluşturun. Yönergeler için bkz [. hızlı başlangıç: Azure portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)kullanarak Blobları karşıya yükleyin, indirin ve listeleyin.

5. Bir Windows veya Linux işletim sistemi görüntüsünü VHD biçiminde hazırlayın (VHDX değil), görüntüyü depolama hesabınıza yükleyin ve VM görüntüsünün PowerShell tarafından alınabileceği URI 'yi alın.  

   ```powershell
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
   ```

6. I Bir veri diski dizisini VM görüntüsünün bir parçası olarak karşıya yükleyebilirsiniz. New-DataDiskObject cmdlet 'ini kullanarak veri disklerinizi oluşturun. PowerShell 'i yükseltilmiş bir komut isteminden açın ve şunu çalıştırın:

   ```powershell
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
   ```

7. PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>" -OSType "<ostype>" -OSUri "<osuri>"
   ```

    Add-Azsplatformımage cmdlet 'i ve New-DataDiskObject cmdlet 'i hakkında daha fazla bilgi için bkz. Microsoft PowerShell [Azure Stack operatör modülü belgeleri](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>PowerShell kullanarak VM görüntüsünü kaldırma

Karşıya yüklediğiniz VM görüntüsüne artık ihtiyacınız kalmadığında, aşağıdaki cmdlet 'i kullanarak bunu Market 'ten silebilirsiniz:

1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).

2. Azure Stack için bir operatör olarak oturum açın.

3. PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:

   ```powershell  
   Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
   ```
   **Remove-Azsplatformımage** cmdlet 'i, Azure Resource Manager ŞABLONLARı tarafından VM görüntüsüne başvurmak için kullanılan değerleri belirtir. Değerler şunlardır:
   - **'ın**  
     Örneğin, `Canonical`  
     Kullanıcıların görüntüyü dağıtırken kullandıkları VM görüntüsünün yayımcı adı segmenti. **Microsoft**bir örnektir. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **sunar**  
     Örneğin, `UbuntuServer`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün teklif adı segmenti. Bir örnek, **windowsserver**' dır. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **isteyin**  
     Örneğin, `14.04.3-LTS`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün SKU ad segmenti. Örnek olarak **Datacenter2016**. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **Sürüm**  
     Örneğin, `1.0.0`  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün sürümü. Bu sürüm biçimindedir *\#.\#.\#* . Örnek olarak **1.0.0**. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
    
     Remove-Azsplatformımage cmdlet 'i hakkında daha fazla bilgi için bkz. Microsoft PowerShell [Azure Stack operatör modülü belgeleri](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](../user/azure-stack-create-vm-template.md)
