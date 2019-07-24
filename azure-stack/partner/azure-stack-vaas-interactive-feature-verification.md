---
title: Hizmet olarak Azure Stack doğrulamada etkileşimli Özellik doğrulama testi | Microsoft Docs
description: Hizmet olarak doğrulama ile Azure Stack için etkileşimli Özellik doğrulama testleri oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: af36943ebaa78ad1838506d6614a7fd6671afe4f
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418479"
---
# <a name="interactive-feature-verification-testing"></a>Etkileşimli Özellik doğrulama testi  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sisteminiz için testler istemek üzere etkileşimli Özellik doğrulama test çerçevesini kullanabilirsiniz. Bir test istediğinizde, Microsoft, el ile etkileşimli adımlar gerektiren testleri hazırlamak için Framework 'ü kullanır. Microsoft, birçok tek başına otomatikleştirilmiş testi birlikte zincirlemek için çerçevesini kullanabilir.

Bu makalede, basit bir el ile senaryo açıklanmaktadır. Test denetimleri Azure Stack bir diski değiştirir. Framework, her adımda tanılama günlüklerini toplar. Sorunları bulduğunuz hata ayıklaması yapabilirsiniz. Çerçeve ayrıca diğer araçlar veya süreçler tarafından oluşturulan günlüklerin paylaşılmasını sağlar ve senaryo hakkında geri bildirim sağlamanıza olanak tanır.

> [!Important]  
> Bu makale, disk kimliği gerçekleştirme adımlarına başvurur. Test geçiş iş akışından toplanan tüm sonuçlar yeni çözüm doğrulaması için kullanılamaz olabileceğinden bu yalnızca bir gösterimdir.

## <a name="overview-of-interactive-testing"></a>Etkileşimli teste genel bakış

Disk değiştirme için bir test yaygın bir senaryodur. Bu örnekte, testte beş adım vardır:

1. Yeni bir **test geçiş** iş akışı oluşturun.
2. **Disk tanımlama testini**seçin.
3. İstendiğinde el ile adımı doldurun.
4. Senaryonun sonucunu denetleyin.
5. Test sonucunu Microsoft 'a gönderin.

## <a name="create-a-new-test-pass"></a>Yeni test geçişi oluştur

Mevcut bir test geçişiniz yoksa, lütfen [bir testi zamanlamaya](azure-stack-vaas-schedule-test-pass.md)yönelik yönergeleri izleyin.

## <a name="schedule-the-test"></a>Testi zamanlayın

1. **Disk kimlik testi**seçin.

    > [!Note]  
    > Test yardımcı kullanımına yönelik iyileştirmeler yapıldıktan sonra testin sürümü artacaktır. Microsoft tarafından aksi belirtilmedikçe en yüksek sürüm her zaman kullanılmalıdır.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image4.png)

1. **Düzenle**seçeneğini belirleyerek etki alanı yönetici kullanıcı adını ve parolasını sağlayın.

1. Testi başlatmak için uygun test yürütme aracısını/DVD 'yi seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image5.png)

1. Testi başlatmak için **Gönder** ' i seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image6.png)

1. Önceki adımda seçilen aracıdan etkileşimli test için Kullanıcı arabirimine erişin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image8.png)

1. Bu senaryonun nasıl gerçekleştirileceği konusunda Microsoft 'un yönergelerini gözden geçirmek için **belge** ve **doğrulama** bağlantılarını izleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image9.png)

1. **İleri**’yi seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image10.png)

1. Ön denetim betiğini çalıştırmak için yönergeleri izleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image11.png)

1. Ön denetim betiği başarıyla tamamlandıktan sonra, **bilgi** sekmesindeki **belge** ve **doğrulama** bağlantıları için el ile senaryoyu (disk değiştirme) çalıştırın.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image12.png)

    > [!Important]  
    > El ile senaryoyu gerçekleştirirken iletişim kutusunu kapatmayın.

1. El ile senaryoyu tamamladıktan sonra, yönergeleri izleyerek gönderi denetim betiğini çalıştırın.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image13.png)

1. El ile senaryonun başarıyla tamamlanmasında (disk değiştirme), **İleri**' yi seçin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image14.png)

    > [!Important]  
    > Pencereyi kapatırsanız, test tamamlanmadan önce durur.

1. Test deneyimi için geri bildirim sağlayın. Bu sorular, Microsoft 'un, senaryonun başarı oranını ve yayın kalitesini değerlendirmesine yardımcı olur.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image15.png)

1. Microsoft 'a göndermek istediğiniz günlük dosyalarını ekleyin.

    ![Alternatif metin](media/azure-stack-vaas-interactive-feature-verification/image16.png)

1. Geri bildirim gönderimi EULA 'sını kabul edin.

1. Sonuçları Microsoft 'a göndermek için **Gönder** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)
