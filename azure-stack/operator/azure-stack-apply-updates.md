---
title: Güncelleştirmeleri Azure Stack uygulama | Microsoft Docs
description: Azure Stack tümleştirilmiş bir sistem için Microsoft Update paketlerini içeri aktarmayı ve yüklemeyi öğrenin.
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
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: justini
ms.lastreviewed: 02/11/2019
ms.openlocfilehash: 04494c3f394fb5a3b836c8fcf67cd02fb2900910
ms.sourcegitcommit: 4f3e161e7632c8a6e3d41946b09f22b5bdb08d36
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413175"
---
# <a name="apply-updates-in-azure-stack"></a>Azure Stack güncelleştirmeleri uygulama

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack için Microsoft veya OEM güncelleştirme paketlerini uygulamak üzere yönetim portalındaki **güncelleştirme** kutucuğunu kullanabilirsiniz.

Tümleşik sistemler 1807 veya önceki bir sürümünü kullanıyorsanız güncelleştirme paketini indirmeniz, paket dosyalarını Azure Stack içine aktarmanız ve ardından güncelleştirme paketini kurmanız gerekir. Yönergeler için bkz [. paketi indirerek Azure Stack güncelleştirme](#update-azure-stack-by-downloading-the-package)

Bu yükseltme yönergeleri Azure Stack tümleşik sistemlerle çalışır. Azure Stack geliştirme sistemi kullanıyorsanız, geçerli sürümün yükleme paketini indirmeniz gerekir. Yönergeler için bkz [. Azure Stack geliştirme seti yüklemesi](../asdk/asdk-install.md)

## <a name="update-azure-stack"></a>Güncelleştirme Azure Stack

### <a name="select-and-apply-an-update-package"></a>Güncelleştirme paketi seçme ve uygulama

1. Yönetim portalını açın.

2. **Pano**seçeneğini belirleyin. **Güncelleştirme** kutucuğunu seçin.

    ![Azure Stack güncelleştirme var](media/azure-stack-apply-updates/azure-stack-updates-1901-dashboard.png)

3. Azure Stack geçerli sürümünü unutmayın. Bir sonraki tam sürüme güncelleştirebilirsiniz. Örneğin Azure Stack 1811 çalıştırıyorsanız, sonraki yayınlanan sürüm 1901 ' dir.

    ![Azure Stack güncelleştirme Uygula](media/azure-stack-apply-updates/azure-stack-updates-1901-updateavailable.png)

4. Güncelleştirmeler listesinden bir sonraki kullanılabilir sürümü seçin. Sürüm notlarını gözden geçirmek istiyorsanız sürüm notları sütununda **Görünüm** ' ü seçerek sürümü için sürüm notları konusunu açın.

5. Şimdi Güncelleştir ' i seçin. Güncelleştirme başlayacaktır.

### <a name="review-update-history"></a>Güncelleştirme geçmişini gözden geçirme

1. Yönetim portalını açın.

2. **Pano**seçeneğini belirleyin. **Güncelleştirme** kutucuğunu seçin.

3. **Güncelleştirme geçmişi**' ni seçin.

![Azure Stack güncelleştirme geçmişi](media/azure-stack-apply-updates/azure-stack-update-history.PNG)

## <a name="update-azure-stack-by-downloading-the-package"></a>Paketi indirerek Azure Stack güncelleştirme

Tümleşik sistemler 1807 veya önceki bir sürümünü kullanıyorsanız güncelleştirme paketini indirmeniz, paket dosyalarını Azure Stack içine aktarmanız ve ardından güncelleştirme paketini kurmanız gerekir.

## <a name="download-the-update-package"></a>Güncelleştirme paketini indirin

Azure Stack için bir Microsoft veya OEM güncelleştirme paketi kullanılabilir olduğunda, paketi Azure Stack erişilebilen bir konuma indirin ve paket içeriğini gözden geçirin. Bir güncelleştirme paketi genellikle aşağıdaki dosyalardan oluşur:

- Kendiliğinden ayıklanan `<PackageName>.zip` dosya. Bu dosya, güncelleştirmenin yükünü içerir, örneğin Windows Server için en son toplu güncelleştirme.

- Karşılık `<PackageName>.bin` gelen dosyalar. Bu dosyalar, *PackageName*. zip dosyasıyla ilişkili yük için sıkıştırma sağlar.

- Bir `Metadata.xml` dosya. Bu dosya, güncelleştirmeyle ilgili temel bilgileri içerir, örneğin Yayımcı, ad, önkoşul, boyut ve destek yolu URL 'SI.

> [!IMPORTANT]  
> Azure Stack 1901 güncelleştirme paketi uygulandıktan sonra, Azure Stack güncelleştirme paketlerinin paketleme biçimi. zip,. bin (s) ve. XML biçiminden. zip (ler) ve. xml biçimine taşınacaktır. Bağlı damgalar içeren Azure Stack işleçleri etkilenmez. Bağlantısı kesilen Azure Stack işleçleri, aşağıda açıklanan işlemi kullanarak yalnızca. xml ve. zip dosyalarını içeri aktaracaktır.

## <a name="import-and-install-updates"></a>Güncelleştirmeleri içeri ve dışarı aktarma

Aşağıdaki yordamda, güncelleştirme paketlerinin yönetici portalında nasıl içeri aktarılacağı ve yükleneceği gösterilmektedir.

> [!IMPORTANT]  
> Her türlü bakım işlemini kullanıcılara bilgilendirmenizi ve normal bakım pencerelerini iş dışı saatler sırasında mümkün olduğunca düzenli olarak zamanlamanızı kesinlikle öneririz. Bakım işlemleri, hem Kullanıcı iş yüklerini hem de Portal işlemlerini etkileyebilir.

1. Yönetici portalında, **tüm hizmetler**' i seçin. Ardından, **veri + depolama** kategorisi altında **depolama hesapları**' nı seçin. (Veya, filtre kutusunda **depolama hesapları**yazmaya başlayın ve bunu seçin.)

    ![Portalda depolama hesaplarının nerede bulunacağını gösterir](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Filtre kutusuna **Güncelleştir**yazın ve **updateadminaccount** depolama hesabını seçin.

3. Depolama hesabı ayrıntılarında, **Hizmetler**altında Bloblar ' ı seçin.
 
    ![Depolama hesabı için bloblara nasıl alınacağını gösterir](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. **BLOB hizmeti**altında, kapsayıcı oluşturmak Için **+ kapsayıcı** ' yı seçin. Bir ad girin (örneğin, *Update-1811*) ve ardından **Tamam**' ı seçin.
 
     ![Depolama hesabında nasıl kapsayıcı ekleneceğini gösterir](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Kapsayıcı oluşturulduktan sonra kapsayıcı adına tıklayın ve ardından paket dosyalarını kapsayıcıya yüklemek için **karşıya yükle** ' ye tıklayın.
 
    ![Paket dosyalarını karşıya yüklemeyi gösterir](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. **Blobu karşıya yükle**altında klasör simgesine tıklayın, güncelleştirme paketinin. zip dosyasına gidin ve dosya Gezgini penceresinde **Aç** ' a tıklayın.
  
7. **BLOB yükle**altında **karşıya yükle**' ye tıklayın.
  
    ![Her paket dosyasının nereye yükleneceğini gösterir](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. *PackageName*. bin ve Metadata. xml dosyaları için 6 ve 7. adımları yineleyin. Dahil edilen ek bildirim. txt dosyasını içeri aktarmayın. Bu dosyalar. bin ve. zip 'in aksine, 1901 adresinden başlayarak. xml ' i her zamanki gibi içeri aktarmaya devam eder.

9. İşiniz bittiğinde, bildirimleri (portalın sağ üst köşesindeki zil simgesi) gözden geçirebilirsiniz. Bildirimlerin karşıya yükleme işleminin tamamlandığını belirtmesi gerekir.
10. Panodaki güncelleştirme kutucuğuna geri gidin. Kutucuk bir güncelleştirmenin kullanılabildiğini göstermelidir. Yeni eklenen güncelleştirme paketini gözden geçirmek için kutucuğa tıklayın.
11. Güncelleştirmeyi yüklemek için **hazır** olarak işaretlenen paketi seçin ve paketi sağ tıklayın ve **Şimdi Güncelleştir**' i seçin ya da en üstteki **güncelleştirme Şimdi Güncelleştir** eylemine tıklayın.
12. Güncelleştirme paketini yükleme ' ye tıkladığınızda, durumu **güncelleştirme çalışma ayrıntıları** alanında görüntüleyebilirsiniz. Buradan, günlük dosyalarını indirmek için **Özet indir** ' e de tıklayabilirsiniz. Güncelleştirme çalıştırmalarının günlükleri, deneme sona erdikten sonra 6 ay boyunca kullanılabilir. 
13. Güncelleştirme tamamlandığında, güncelleştirme kutucuğu güncelleştirilmiş Azure Stack sürümünü gösterir.

Azure Stack üzerine yüklendikten sonra, güncelleştirmeleri depolama hesabından el ile silebilirsiniz. Azure Stack, eski güncelleştirme paketlerini düzenli olarak denetler ve depolama alanından kaldırır. Eski paketlerin kaldırılması iki hafta Azure Stack sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmelerini yönetmeye genel bakış](azure-stack-updates.md)
- [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md)
