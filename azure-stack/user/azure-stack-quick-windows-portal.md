---
title: Azure Stack portalı ile Windows VM oluşturma | Microsoft Docs
description: Azure Stack portalı ile Windows Server 2016 sanal makinesi (VM) oluşturmayı öğrenin.
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 10/02/2019
ms.author: mabrigg
ms.custom: mvc
ms.reviewer: kivenkat
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 95fef782ca7efe09f7c93fbf0e28e81ed34d8166
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71823919"
---
# <a name="quickstart-create-a-windows-server-vm-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portalı ile Windows Server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Windows Server 2016 sanal makinesi (VM) oluşturmayı öğrenin.

> [!NOTE]  
> Bu makaledeki ekran görüntüleri, Azure Stack sürüm 1808 ile tanıtılan Kullanıcı arabirimiyle eşleşecek şekilde güncelleştirilir. 1808, yönetilmeyen disklere ek olarak *yönetilen disklerin* kullanılmasına yönelik destek ekler. Önceki bir sürümü kullanıyorsanız, disk seçimi gibi bazı görüntüler Bu makalede görüntülenenden farklı olacaktır.  


## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalında oturum açın. Azure Stack portalının adresi, bağlandığınız Azure Stack ürüne bağlıdır:

* Azure Stack Geliştirme Seti (ASDK) için şuraya gidin: https://portal.local.azurestack.external.
* Azure Stack tümleşik bir sistem için Azure Stack işletmenizin verdiğiniz URL 'ye gidin.

## <a name="create-a-vm"></a>VM oluşturma

1. **+ Kaynak oluştur** > **işlem** > **Windows Server 2016 Datacenter-Kullandıkça Öde** > **Oluştur**. <br> **Windows server 2016 Datacenter-Kullandıkça Öde** girişini görmüyorsanız Azure Stack işletmenize başvurun ve [WINDOWS Server 2016 VM görüntüsünü Azure Stack Market 'e ekleme](../operator/azure-stack-create-and-publish-marketplace-item.md) makalesinde açıklandığı şekilde bunu Market 'e eklemesini isteyin .

    ![Portalda Windows VM oluşturma adımları](media/azure-stack-quick-windows-portal/image01.png)

2. **Temel bilgiler**altında bir **ad**, **Kullanıcı adı**ve **parola**yazın. Bir **abonelik**seçin. Bir **kaynak grubu**oluşturun veya var olan bir grubu seçin, bir **konum**seçin ve ardından **Tamam**' a tıklayın.

    ![Temel ayarları yapılandırın](media/azure-stack-quick-windows-portal/image02.png)

3. **Boyut**bölümünde **D1 Standart**' ı seçin ve ardından **Seç**' e tıklayın.  

    ![VM boyutunu seçin](media/azure-stack-quick-windows-portal/image03.png)

4. **Ayarlar** sayfasında, varsayılanlar üzerinde istediğiniz değişiklikleri yapın.
   - Azure Stack sürüm 1808 ' den başlayarak, *yönetilen diskleri*kullanmayı seçebileceğiniz **depolama alanını** yapılandırabilirsiniz. 1808 öncesi sürümlerde, yalnızca yönetilmeyen diskler kullanılabilir.  

   ![VM ayarlarını yapılandırma](media/azure-stack-quick-windows-portal/image04.png)  

   Yapılandırmalarınız hazırlandığınızda, devam etmek için **Tamam** ' ı seçin.

5. **Özet**altında, VM oluşturmak için **Tamam** ' ı tıklatın.
    ![summary görüntüleme ve VM oluşturma @ no__t-1

6. Yeni VM 'nizi görmek için **tüm kaynaklar**' a tıklayın, VM adını arayın ve arama sonuçlarında bunu seçin.

    ![Bkz. VM](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

VM 'yi kullanmayı tamamladığınızda, VM 'yi ve kaynaklarını silin. Bunu yapmak için VM sayfasında kaynak grubunu seçin ve **Sil**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel bir Windows Server VM 'si dağıttınız. Azure Stack VM 'Ler hakkında daha fazla bilgi edinmek için [Azure Stack VM 'lerle Ilgili noktalara](azure-stack-vm-considerations.md)devam edin.
