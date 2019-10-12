---
title: Azure Stack için özel bir VM görüntüsü ekleme | Microsoft Docs
description: Azure Stack için özel bir VM görüntüsü ekleme veya kaldırma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: Justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: Justinha
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 9dc5039a2c8b74b14da59573758a4cf8d1a3657a
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282648"
---
# <a name="add-a-custom-vm-to-azure-stack"></a>Azure Stack için özel VM ekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, özel sanal makine (VM) görüntünüzü Market 'e ekleyebilir ve kullanıcılarınız için kullanılabilir hale getirebilirsiniz. Görüntüler, Azure Stack için Azure Resource Manager şablonlar kullanılarak eklenir. Ayrıca, yönetim portalı veya Windows PowerShell kullanarak Azure Marketi Kullanıcı arabirimine Market öğesi olarak VM görüntüleri ekleyebilirsiniz. Küresel Azure Marketi 'nden veya kendi özel VM yansımanıza ait bir görüntü kullanın.

## <a name="generalize-the-vm-image"></a>VM görüntüsünü genelleştirin

### <a name="windows"></a>Windows

Özel bir Genelleştirilmiş VHD oluşturun. VHD, Azure dışından ise, genelleştirilmiş bir VHD 'yi karşıya yükleme ' deki adımları izleyin ve VHD 'nizi doğru şekilde **Sysprep** yapmak için [Azure 'Da yeni VM 'ler oluşturmak üzere kullanın](/azure/virtual-machines/windows/upload-generalized-managed) ve bunu Genelleştirilmiş hale getirin.

VHD Azure 'dan ise, Azure Stack 'e aktarmadan önce, [Sysprep kullanarak kaynak VM 'Yi genelleştirerek](/azure/virtual-machines/windows/upload-generalized-managed#generalize-the-source-vm-by-using-sysprep) bu yönergeleri izleyin.

### <a name="linux"></a>Linux

VHD, Azure dışından ise, VHD 'yi genelleştirmek için ilgili yönergeleri izleyin:

- [CentOS tabanlı dağıtımlar](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Linux 'u kaldırma](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](/azure/azure-stack/azure-stack-redhat-create-upload-vhd)
- [SLES veya openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu sunucusu](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

VHD Azure 'dan ise, VHD 'yi genelleştirmek için aşağıdaki yönergeleri izleyin:

1. **Waagent** hizmetini durdurun:

   ```bash
   # sudo waagent -force -deprovision
   # export HISTSIZE=0
   # logout
   ```

2. VM 'yi kapatın ve VHD 'YI indirin. VHD 'nizi Azure 'dan getiriyoruz, [Azure 'Dan WINDOWS VHD 'Yi indirme](/azure/virtual-machines/windows/download-vhd)bölümünde gösterildiği gibi, bu işlemi disk dışarı aktarma kullanarak yapabilirsiniz.

### <a name="common-steps-for-both-windows-and-linux"></a>Hem Windows hem de Linux için ortak adımlar

Görüntüyü karşıya yüklemeden önce, aşağıdakileri göz önünde bulundurmanız önemlidir:

- Azure Stack, sabit disk VHD biçiminde yalnızca bir (1) VM oluşturulmasını destekler. Sabit biçim, *disk uzaklığında x* blob uzaklığında depolanacak şekilde mantıksal diski dosya içinde doğrusal olarak bir şekilde *alır.* Blob sonundaki küçük bir alt bilgi, VHD 'nin özelliklerini açıklar. Diskinizin düzeltildiğini onaylamak için, **Get-VHD** PowerShell cmdlet 'ini kullanın.

- Azure Stack, dinamik disk VHD 'lerini desteklemez. Bir VM 'ye bağlı dinamik bir diskin yeniden boyutlandırılması VM 'nin başarısız bir durumda kalmasını sağlar. Bu sorunu azaltmak için, depolama hesabındaki bir VHD blobu olan VM 'nin diskini silmeden VM 'yi silin. Ardından, VHD 'yi dinamik bir diskten sabit bir diske dönüştürüp VM 'yi yeniden oluşturun.

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-the-portal"></a>Portalı kullanarak bir Azure Stack işleci olarak VM görüntüsü ekleme

1. Görüntülere bir BLOB depolama URI 'SI tarafından başvurulabilmelidir. Bir Windows veya Linux işletim sistemi görüntüsünü VHD biçiminde hazırlayın (VHDX değil) ve ardından görüntüyü Azure veya Azure Stack bir depolama hesabına yükleyin.

   - VHD Azure 'deyken, bağlı bir Azure Stack çalıştırıyorsanız, bir Azure ile Azure Stack depolama hesabınız arasında VHD 'YI doğrudan aktarmak için [AzCopy](/azure/storage/common/storage-use-azcopy) gibi bir araç kullanabilirsiniz.

   - VHD 'niz Azure 'deyken, bağlantısı kesilen Azure Stack VHD 'yi hem Azure hem de Azure Stack bağlantısı olan bir makineye indirmeniz gerekir. Daha sonra, VHD 'yi Azure ve Azure Stack üzerinde kullanılabilen ortak [depolama veri aktarımı araçlarından](../user/azure-stack-storage-transfer.md) herhangi birini kullanarak Azure Stack için Azure 'dan bu makineye kopyalayın.

2. Bir VHD 'yi Azure Stack yönetici portalındaki bir depolama hesabına yüklemek için [Bu örneği](/powershell/module/azurerm.compute/add-azurermvhd?view=azurermps-6.13.0) izleyebilirsiniz.

   - Görüntünün Azure Stack görüntü deposuna gönderimi daha az zaman alacağından, bir görüntüyü Azure Blob depolama alanına göre Azure Stack blob depolamaya yüklemek daha etkilidir.

   - [WINDOWS VM görüntüsünü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/)karşıya yüklediğinizde, [Azure Stack Işlecinin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) adımını kullanarak **oturum açma adımını Azure 'a** geçdiğinizden emin olun.  

3. Görüntüyü karşıya yüklediğiniz BLOB depolama URI 'sini bir yere getirin. BLOB depolama URI 'SI şu biçimdedir: *&lt;storageAccount @ no__t-2 @ no__t-3 @ no__t-4blobContainer @ no__t-5 @ no__t-6 @ no__t-7targetVHDName @ no__t-8*. vhd.

4. Blobu anonim olarak erişilebilir hale getirmek için VM görüntüsü VHD 'sinin karşıya yüklendiği depolama hesabı blob kapsayıcısına gidin. **BLOB**' u ve ardından **erişim ilkesi**' ni seçin. İsteğe bağlı olarak, kapsayıcı için bir paylaşılan erişim imzası oluşturabilir ve bunu blob URI 'sinin bir parçası olarak dahil edebilirsiniz. Bu adım, Blobun kullanılabilir olduğundan emin olur. Blob anonim olarak erişilebilir değilse, VM görüntüsü başarısız bir durumda oluşturulur.

   ![Depolama hesabı bloblarına git](./media/azure-stack-add-vm-image/image1.png)

   ![Blob erişimini herkese ayarla](./media/azure-stack-add-vm-image/image2.png)

5. İşleç olarak Azure Stack için oturum açın. Menüsünde, **işlem** > **Ekle**altında **tüm hizmetler** > **görüntü** ' i seçin.

6. **Görüntü oluştur**altında, ad, abonelik, kaynak grubu, konum, işletim sistemi diski, işletim sistemi türü, Depolama Blobu URI, hesap türü ve konak önbelleğe alma ' yı girin. Ardından, VM görüntüsünü oluşturmaya başlamak için **Oluştur** ' u seçin.

   ![Görüntüyü oluşturmaya başla](./media/azure-stack-add-vm-image/image4.png)

   Görüntü başarıyla oluşturulduğunda, VM görüntüsü durumu **başarılı**olarak değişir.

7. Bir görüntü eklediğinizde yalnızca Azure Resource Manager tabanlı şablonlar ve PowerShell dağıtımları için kullanılabilir. Bir görüntüyü kullanıcılarınız için Market öğesi olarak kullanılabilir hale getirmek için Market öğesi [oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)makalesindeki adımları kullanarak Market öğesini yayımlayın. **Yayımcı**, **teklif**, **SKU**ve **Sürüm** değerlerini notdığınızdan emin olun. Bu, Resource Manager şablonunu ve manifest. json ' u düzenlerken, Custom. azpkg içinde gerekli olacaktır.

## <a name="remove-the-vm-image-as-an-azure-stack-operator-using-the-portal"></a>Portalı kullanarak VM görüntüsünü Azure Stack işletmeni olarak kaldırma

1. Azure Stack [Yönetici portalı](https://adminportal.local.azurestack.external)' nı açın.

2. VM görüntüsünün ilişkili bir Market öğesi varsa, **Market yönetimi**' ni seçin ve ardından sılmek istediğiniz VM marketi öğesini seçin.

3. VM görüntüsünün ilişkili bir Market öğesi yoksa, **tüm hizmetler > işlem > VM görüntüleri**' ne gidin ve ardından VM görüntüsünün yanındaki üç nokta ( **...** ) simgesini seçin.

4. **Sil**' i seçin.

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>PowerShell kullanarak bir Azure Stack işleci olarak VM görüntüsü ekleme

1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).  

2. Azure Stack için bir operatör olarak oturum açın. Yönergeler için bkz. [Azure Stack bir operatör olarak oturum açma](azure-stack-powershell-configure-admin.md).

3. PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<Offer>" `
      -sku "<SKU>" `
      -version "<#.#.#>" `
      -OSType "<OS type>" `
      -OSUri "<OS URI>"
   ```

   **Add-Azsplatformımage** cmdlet 'i, Azure Resource Manager ŞABLONLARı tarafından VM görüntüsüne başvurmak için kullanılan değerleri belirtir. Değerler şunlardır:
   - **'ın**  
     Örneğin: `Canonical`.  
     Kullanıcıların görüntüyü dağıtırken kullandıkları VM görüntüsünün **Yayımcı** adı segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **sunar**  
     Örneğin: `UbuntuServer`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün **teklif** adı segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **isteyin**  
     Örneğin: `14.04.3-LTS`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün **SKU** ad segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **version**  
     Örneğin: `1.0.0`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün sürümü. Bu sürüm *\#. \#. \#* biçimindedir. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **osType**  
     Örneğin: `Linux`.  
     Görüntünün **OSType öğesi** **Windows** ya da **Linux**olmalıdır.  
   - **Osurı**  
     Örneğin: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`.  
     @No__t için bir BLOB depolama URI 'SI belirtebilirsiniz.  

     Daha fazla bilgi için bkz. [Add-Azsplatformımage](/powershell/module/azs.compute.admin/add-azsplatformimage) ve [New-DataDiskObject](/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet 'leri için PowerShell Başvurusu.

## <a name="remove-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>PowerShell kullanarak bir VM görüntüsünü Azure Stack işleci olarak kaldırma

Karşıya yüklediğiniz VM görüntüsüne artık ihtiyacınız kalmadığında, aşağıdaki cmdlet 'i kullanarak bunu Market 'ten silebilirsiniz:

1. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md).

2. Azure Stack için bir operatör olarak oturum açın.

3. PowerShell 'i yükseltilmiş bir komut istemiyle açın ve şunu çalıştırın:

   ```powershell  
   Remove-AzsPlatformImage `
    -publisher "<Publisher>" `
    -offer "<Offer>" `
    -sku "<SKU>" `
    -version "<Version>" `
   ```

   **Remove-Azsplatformımage** cmdlet 'i, Azure Resource Manager ŞABLONLARı tarafından VM görüntüsüne başvurmak için kullanılan değerleri belirtir. Değerler şunlardır:
   - **'ın**  
     Örneğin: `Canonical`.  
     Kullanıcıların görüntüyü dağıtırken kullandıkları VM görüntüsünün **Yayımcı** adı segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **sunar**  
     Örneğin: `UbuntuServer`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün **teklif** adı segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **isteyin**  
     Örneğin: `14.04.3-LTS`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün **SKU** ad segmenti. Bu alana boşluk veya diğer özel karakterler eklemeyin.  
   - **version**  
     Örneğin: `1.0.0`.  
     Kullanıcıların VM görüntüsünü dağıtırken kullandıkları VM görüntüsünün sürümü. Bu sürüm *\#. \#. \#* biçimindedir. Bu alana boşluk veya diğer özel karakterler eklemeyin.  

     **Remove-Azsplatformımage** cmdlet 'i hakkında daha fazla bilgi için bkz. Microsoft PowerShell [Azure Stack operatör modülü belgeleri](/powershell/module/).

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bir Azure Stack Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
- [Sanal makine sağlama](../user/azure-stack-create-vm-template.md)
