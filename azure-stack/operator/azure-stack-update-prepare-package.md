---
title: Azure Stack güncelleştirme paketi hazırlama | Microsoft Docs
description: Azure Stack güncelleştirme paketini hazırlamayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2019
ms.author: mabrigg
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: 969aea2134f7980eb2b3a5b6e8d00a987c410744
ms.sourcegitcommit: b8260ef3e43f3703dd0df16fb752610ec8a86942
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70010236"
---
# <a name="prepare-an-azure-stack-update-package"></a>Azure Stack güncelleştirme paketi hazırlama

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Bu makalede, Azure Stack ortamınızı güncelleştirmek için kullanılabilmesi için Azure Stack güncelleştirme paketlerinin hazırlanmasında bir genel bakış sunulmaktadır. Bu işlem, internet 'e bağlı sistemlerdeki Azure Stack yazılım güncelleştirmeleri ve düzeltmeleri için otomatik olarak yapılır. Tüm OEM paket güncelleştirmeleri ve Azure Stack yazılım güncelleştirmeleri ve sistemlerinde, zayıf veya aralıklı Internet-connectivty ile yazılım güncelleştirmeleri ve düzeltmeleri için güncelleştirme paketleri, Azure Stack güncelleştirme paketi veya OEM güncelleştirmesi yüklenerek hazırlanır paketi, Azure Stack güncelleştirme sağlayıcısı tarafından erişilebilmesi için paket ve depoya içeri aktarma. Tablo, bu makaledeki adımların gerçekleştirilmesi gereken zaman görüntülenir:

| Güncelleştirme türü | Internet bağlantısı | Action |
| --- | --- | --- |
| Yazılım güncelleştirmelerini Azure Stack | Bağlı | Güncelleştirme uygulandığında güncelleştirme otomatik olarak indirilir ve hazırlanır. |
| Azure Stack düzeltmeler | Bağlı | Güncelleştirme uygulandığında güncelleştirme otomatik olarak indirilir ve hazırlanır. |
| OEM paket güncelleştirmeleri | Bağlı | Güncelleştirme paketi hazırlanmalıdır. Bu makaledeki adımları izleyin. |
| Yazılım güncelleştirmelerini Azure Stack | Bağlantısı kesik veya zayıf bağlantı | Güncelleştirme paketi hazırlanmalıdır. Bu makaledeki adımları izleyin. |
| Azure Stack düzeltmeler | Bağlantısı kesik veya zayıf bağlantı | Güncelleştirme paketi hazırlanmalıdır. Bu makaledeki adımları izleyin. |
| OEM paket güncelleştirmeleri | Bağlantısı kesik veya zayıf bağlantı | Güncelleştirme paketi hazırlanmalıdır. Bu makaledeki adımları izleyin. |

## <a name="download-the-update-package"></a>Güncelleştirme paketini indirin
Azure Stack güncelleştirmeleri ve düzeltmeleri güncelleştirme paketi, bağlı sistemler için güncelleştirme dikey penceresinde kullanılabilir. Bir OEM paketini güncelleştiriyorsanız veya bağlantısı kesilen bir sistemi destekliyorsanız, paketi indirmeniz ve paketi Azure Stack örneğiniz tarafından erişilebilen bir konuma taşımanız gerekir. Ayrıca, aralıklı bağlantıyla bir sistem çalıştırıyorsa, paketi bir erişilebilir konuma indirip yüklemeniz gerekebilir.

Paket içeriğini gözden geçirin. Bir güncelleştirme paketi genellikle aşağıdaki dosyalardan oluşur:

-   **Kendiliğinden ayıklanan \<bir PackageName >. zip dosyası**. Bu dosya güncelleştirme yükünü içerir.
- **Meta veri. xml dosyası**. Bu dosya, güncelleştirme ile ilgili temel bilgileri içerir, örneğin Yayımcı, ad, önkoşul, boyut ve destek yolu URL 'SI.

## <a name="azure-stack-software-updates"></a>Yazılım güncelleştirmelerini Azure Stack

Azure Stack yazılım güncelleştirmeleri güvenli bir Azure uç noktasında barındırılır. Bağlı örneklerle Azure Stack işleçler, Azure Stack güncelleştirmeleri otomatik olarak yönetim portalı 'nda **bulunan Ileti güncelleştirmesi**ile görüntülenir. Güncelleştirme uygulandığında Azure Stack güncelleştirmeler Internet 'e bağlı sistemlere otomatik olarak indirilir. İnternet bağlantısı kesilmiş sistemler veya zayıf internet bağlantısı olan sistemler için güncelleştirme paketleri [Azure Stack Updates Downloader aracı](https://aka.ms/azurestackupdatedownload)kullanılarak indirilebilir. Azure Stack yazılım güncelleştirme paketleri, Azure Stack hizmetlerine yönelik güncelleştirmeleri ve Azure Stack ölçek birimlerinin işletim sistemine yönelik güncelleştirmeleri içerebilir.

### <a name="azure-stack-hotfixes"></a>Azure Stack düzeltmeler 

Düzeltme güncelleştirme paketleri aynı güvenli Azure uç noktasında barındırılır. Bağlı örneklerle Azure Stack işleçler, düzeltmeler, **kullanılabilir Ileti güncelleştirmesiyle**birlikte otomatik olarak yönetim portalında görünür. Azure Stack düzeltmeler, güncelleştirme uygulandığında Internet 'e bağlı sistemlere otomatik olarak indirilir. İlgili düzeltme KB makalelerinin her birinde ekli bağlantıları kullanarak bunları indirebilirsiniz; Örneğin, [1.1906.11.52 düzeltmesini Azure Stack](https://support.microsoft.com/help/4515650). Düzeltmeleri Azure Stack sürümünüze karşılık gelen sürüm notlarında bulabilirsiniz. OEM donanım satıcısı tarafından belirtilen güncelleştirmeler

Ayrıca, OEM satıcınız sürücü ve bellenim güncelleştirmeleri gibi güncelleştirmeleri de serbest bırakacaktır. Bu güncelleştirmeler satıcıya göre ayrı paketler olarak teslim edilirken, bazıları Microsoft 'tan güncelleştirme paketleriyle aynı şekilde alınır, yüklenir ve yönetilir. Satıcı iletişim bağlantılarının bir listesini, [Azure Stack özgün ekipman üreticisi (OEM) güncelleştirmeleri uygulayın](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-oem#oem-contact-information)' de bulabilirsiniz.

## <a name="import-and-install-updates"></a>Güncelleştirmeleri içeri ve dışarı aktarma

Aşağıdaki yordamda, yönetim portalı 'nda güncelleştirme paketlerinin nasıl içeri aktarılacağı ve yükleneceği gösterilmektedir.

> [!Important]  
> Her türlü bakım işlemini kullanıcılara bildirin ve iş dışı saatlerde normal bakım pencerelerini mümkün olduğunca zamanlayın. Bakım işlemleri, hem Kullanıcı iş yüklerini hem de Portal işlemlerini etkileyebilir.

1.  Yönetim portalında, **tüm hizmetler**' i seçin. Ardından, **veri + depolama** kategorisi altında **depolama hesapları**' nı seçin. (Veya, filtre kutusunda **depolama hesapları**yazmaya başlayın ve bunu seçin.)

    ![Azure Stack güncelleştirme](./media/azure-stack-update-prepare-package/image1.png) 

1.  Filtre kutusuna **Güncelleştir**yazın ve **updateadminaccount** depolama hesabını seçin.

2.  Depolama hesabı ayrıntılarında, **Hizmetler**altında Bloblar ' ıseçin.

    ![Azure Stack Update-blob](./media/azure-stack-update-prepare-package/image2.png)

1.  **BLOB hizmeti**altında, kapsayıcı oluşturmak Için **+ kapsayıcı** ' yı seçin. Bir ad girin (örneğin, *Update-1811*) ve ardından **Tamam**' ı seçin.

    ![Azure Stack Update-Container](./media/azure-stack-update-prepare-package/image3.png)

1.  Kapsayıcı oluşturulduktan sonra kapsayıcı adına tıklayın ve ardından paket dosyalarını kapsayıcıya yüklemek için **karşıya yükle** ' ye tıklayın.

    ![Azure Stack Update-karşıya yükle](./media/azure-stack-update-prepare-package/image4.png)

1.  **Blobu karşıya yükle**altında klasör simgesine tıklayın, güncelleştirme paketinin. zip dosyasına gidin ve dosya Gezgini penceresinde **Aç** ' a tıklayın.

2.  **BLOB yükle**altında **karşıya yükle**' ye tıklayın.

    ![Azure Stack Update-blobu karşıya yükle](./media/azure-stack-update-prepare-package/image5.png)

1.  Güncelleştirme paketindeki Metadata. xml dosyası ve diğer tüm. zip dosyaları için 6 ve 7. adımları yineleyin. Dahil edilen ek bildirim. txt dosyasını içeri aktarmayın.

2.  İşiniz bittiğinde, bildirimleri (portalın sağ üst köşesindeki zil simgesi) gözden geçirebilirsiniz. Bildirimlerin karşıya yükleme işleminin tamamlandığını belirtmesi gerekir.

3.  Panodaki güncelleştirme dikey penceresine geri gidin. Dikey pencere, bir güncelleştirmenin kullanılabildiğini göstermelidir. Bu, güncelleştirmenin başarıyla hazırlandığını gösterir. Yeni eklenen güncelleştirme paketini gözden geçirmek için dikey penceresine tıklayın.

4.  Güncelleştirmeyi yüklemek için **hazır** olarak işaretlenen paketi seçin ve paketi sağ tıklayın ve **Şimdi Güncelleştir**' i seçin ya da en üstteki **güncelleştirme Şimdi Güncelleştir** eylemine tıklayın.

5.  Güncelleştirme paketini yükleme ' ye tıkladığınızda, durumu **güncelleştirme çalışma ayrıntıları** alanında görüntüleyebilirsiniz. Buradan, günlük dosyalarını indirmek için **Özet indir** ' e de tıklayabilirsiniz. Güncelleştirme çalıştırmalarının günlükleri, deneme sona erdikten sonra 6 ay boyunca kullanılabilir.

6.  Güncelleştirme tamamlandığında, güncelleştirme dikey penceresinde güncelleştirilmiş Azure Stack sürümü gösterilir.

Azure Stack üzerine yüklendikten sonra, güncelleştirmeleri depolama hesabından el ile silebilirsiniz. Azure Stack, eski güncelleştirme paketlerini düzenli olarak denetler ve depolama alanından kaldırır. Eski paketlerin kaldırılması iki hafta Azure Stack sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Güncelleştirmeyi Uygula](azure-stack-apply-updates.md)
