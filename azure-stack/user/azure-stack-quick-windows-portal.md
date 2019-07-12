---
title: Azure Stack portal ile Windows VM oluşturma | Microsoft Docs
description: Azure Stack portal ile Windows Server 2016 sanal makine (VM) oluşturmayı öğrenin.
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: mabrigg
ms.custom: mvc
ms.reviewer: kivenkat
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: cc9a6baa3c71e58c2671b1f1b221e18a0c4f38c1
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816176"
---
# <a name="quickstart-create-a-windows-server-vm-with-the-azure-stack-portal"></a>Hızlı Başlangıç: Azure Stack portal ile Windows server VM oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portalını kullanarak bir Windows Server 2016 sanal makine (VM) oluşturmayı öğrenin.

> [!NOTE]  
> Bu makalede ekran görüntüleri, 1808 Azure Stack sürümüyle kullanıma sunulan kullanıcı arabirimini eşleşecek şekilde güncelleştirilir. 1808 kullanma desteği ekler *yönetilen diskler* yönetilmeyen diskler yanı sıra. Önceki bir sürümünü kullanıyorsanız, disk seçimi gibi bazı görüntüleri bu makalede gösterilen değerinden farklı olacaktır.  


## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack portalında oturum açın

Azure Stack portalında oturum açın. Azure Stack portal'ın adresi, Azure Stack ürünlere ilişkin bağlanmakta olduğunuz bağlıdır:

* Azure Stack geliştirme Seti'ni (ASDK için), Git: https://portal.local.azurestack.external.
* Bir Azure Stack tümleşik sistemi için Azure Stack operatörü sağlanan URL'sine gidin.

## <a name="create-a-vm"></a>VM oluşturma

1. Tıklayın **+ kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter --,-kullandıkça**  >   **Oluşturma**. <br> Görmüyorsanız **Windows Server 2016 Datacenter --,-kullandıkça** girişi, Azure Stack operatörü başvurun ve bunlar Market'te açıklandığı şekilde eklemenizi isteyin [Windows Server 2016 VM görüntüsüne ekleyin Azure Stack marketini](../operator/azure-stack-create-and-publish-marketplace-item.md) makalesi.

    ![Bir Windows VM portal oluşturma adımları](media/azure-stack-quick-windows-portal/image01.png)

2. Altında **Temelleri**, tür a **adı**, **kullanıcı adı**, ve **parola**. Seçin bir **abonelik**. Oluşturma bir **kaynak grubu**, veya varolan bir adet seçin bir **konumu**ve ardından **Tamam**.

    ![Temel ayarları yapılandırma](media/azure-stack-quick-windows-portal/image02.png)

3. Altında **boyutu**seçin **standart D1**ve ardından **seçin**.  

    ![VM boyutunu seçin](media/azure-stack-quick-windows-portal/image03.png)

4. Üzerinde **ayarları** sayfasında, varsayılan olarak istediğiniz değişiklikleri yapın.
   - Yapılandırabileceğiniz 1808 Azure Stack sürümünden başlayarak **depolama** seçebileceğiniz kullanılacak *yönetilen diskler*. Yönetilmeyen diskler yalnızca 1808 önce sürümlerinde kullanılabilir.  

   ![VM ayarlarını yapılandırma](media/azure-stack-quick-windows-portal/image04.png)  

   Yapılandırmalarınızı hazır olduğunuzda seçin **Tamam** devam etmek için.

5. Altında **özeti**, tıklayın **Tamam** VM oluşturmak için.
    ![Özet görüntüleyin ve VM oluşturma](media/azure-stack-quick-windows-portal/image05.png)

6. Yeni sanal makinenize görmek için tıklayın **tüm kaynakları**VM adını arayın ve arama sonuçlarında seçin.

    ![VM bakın](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

VM kullanarak tamamladığınızda, VM ve kaynaklarını silin. Bunu yapmak için VM sayfasında kaynak grubunu seçin ve **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel Windows Server VM'sine dağıttınız. Azure Stack sanal makineleri hakkında daha fazla bilgi için devam [Azure Stack'te Vm'lerde dikkate alınacak noktalar](azure-stack-vm-considerations.md).
