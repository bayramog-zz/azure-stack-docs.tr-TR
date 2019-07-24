---
title: Hizmet olarak Azure Stack doğrulamasında özgün ekipman üreticisi (OEM) paketlerini doğrulama | Microsoft Docs
description: Bir hizmet olarak doğrulama ile orijinal ekipman üreticisi (OEM) paketlerini doğrulamayı öğrenin.
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
ms.openlocfilehash: 69bb9c89793789280debe13a142f4c96470f7c31
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418320"
---
# <a name="validate-oem-packages"></a>OEM paketlerini doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Tamamlanmış bir çözüm doğrulamasının belleniminde veya sürücülerinde değişiklik yapıldığında yeni bir OEM paketini test edebilirsiniz. Paketiniz testi geçtiğinde, Microsoft tarafından imzalanır. Testiniz, Windows Server logosu ve bılgısayar testlerini geçen sürücüler ve üretici yazılımıyla güncelleştirilmiş OEM uzantı paketini içermelidir.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Paketleri karşıya yüklemeden veya göndermeden önce, beklenen paket biçimi ve içerikleri için [BIR OEM paketi oluştur](azure-stack-vaas-create-oem-package.md) ' u gözden geçirin.

## <a name="managing-packages-for-validation"></a>Paketleri doğrulama için yönetme

Paketi doğrulamak için **paket doğrulama** iş akışını kullanırken bir **Azure Storage blob 'una**URL sağlamanız gerekir. Bu blob, güncelleştirme sürecinin bir parçası olarak yüklenecek test imzalı OEM paketidir. Kurulum sırasında oluşturduğunuz Azure Depolama hesabını kullanarak blobu oluşturun (bkz. [doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Koşul Depolama kapsayıcısı sağlama

Paket Blobları için depolama hesabınızda bir kapsayıcı oluşturun. Bu kapsayıcı, tüm paket doğrulama çalışmalarınız için kullanılabilir.

1. [Azure Portal](https://portal.azure.com), [doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)bölümünde oluşturulan depolama hesabına gidin.

2. **BLOB hizmeti**altındaki sol dikey pencerede **kapsayıcılar**' ı seçin.

3. Menü çubuğundan **+ kapsayıcı** ' yı seçin.
    1. Kapsayıcı için bir ad sağlayın (örneğin, `vaaspackages`).
    1. VaaS gibi kimliği doğrulanmamış istemciler için istenen erişim düzeyini seçin. Her senaryodaki paketlere VaaS erişimi verme hakkında daha fazla bilgi için bkz. [kapsayıcı erişim düzeyini işleme](#handling-container-access-level).

### <a name="upload-package-to-storage-account"></a>Paketi depolama hesabına yükle

1. Doğrulamak istediğiniz paketi hazırlayın. Bu, içeriği `.zip` [bir OEM paketi oluşturma](azure-stack-vaas-create-oem-package.md)bölümünde açıklanan yapıyla eşleşmesi gereken bir dosyadır.

    > [!NOTE]
    > Lütfen `.zip` içeriğin `.zip` dosyanın köküne yerleştirildiğinden emin olun. Pakette hiçbir alt klasör olmaması gerekir.

1. [Azure Portal](https://portal.azure.com), paket kapsayıcısını seçin ve menü çubuğunda **karşıya yükle** ' yi seçerek paketi yükleyin.

1. Karşıya yüklenecek paket `.zip` dosyasını seçin. **BLOB türü** ( **Blok Blobu**) ve **blok boyutu**için Varsayılanları tut.

### <a name="generate-package-blob-url-for-vaas"></a>VaaS için paket blobu URL 'SI oluştur

VaaS portalında bir **paket doğrulama** iş akışı oluştururken paketinizi Içeren Azure Storage blob 'UNA bir URL sağlamanız gerekir. **Aylık AzureStack güncelleştirme doğrulaması** ve **OEM uzantısı paket doğrulaması**dahil bazı *etkileşimli* testler, paket Blobları için bir URL gerektirir.

#### <a name="handling-container-access-level"></a>Kapsayıcı erişim düzeyini işleme

VaaS için gereken en düşük erişim düzeyi, bir paket doğrulama iş akışı oluşturup oluşturmadığınıza veya *etkileşimli* bir test zamanlamaya bağlı olarak değişir.

**Özel** ve **BLOB** erişim düzeyleri söz konusu olduğunda, Vaas [paylaşılan erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1?) (SAS) vererek paket blobuna geçici olarak erişim vermeniz gerekir. **Kapsayıcı** erişim DÜZEYI Için SAS URL 'leri oluşturmanız gerekmez, ancak kapsayıcıya ve bloblarına kimliği doğrulanmamış erişime izin verir.

|Erişim düzeyi | İş akışı gereksinimi | Test gereksinimi |
|---|---------|---------|
|Özel | Paket blobu başına SAS URL 'SI oluşturun ([seçenek 1](#option-1-generate-a-blob-sas-url)). | Hesap düzeyinde bir SAS URL 'SI oluşturun ve paket blobu adını ([seçenek 2](#option-2-construct-a-container-sas-url)) el ile ekleyin. |
|Blob | Blob URL özelliğini sağlayın ([seçenek 3](#option-3-grant-public-read-access)). | Hesap düzeyinde bir SAS URL 'SI oluşturun ve paket blobu adını ([seçenek 2](#option-2-construct-a-container-sas-url)) el ile ekleyin. |
|Kapsayıcı | Blob URL özelliğini sağlayın ([seçenek 3](#option-3-grant-public-read-access)). | Blob URL özelliğini sağlayın ([seçenek 3](#option-3-grant-public-read-access)).

Paketlerinize erişim verme seçenekleri, en az erişim için en az erişime göre sıralanır.

#### <a name="option-1-generate-a-blob-sas-url"></a>Seçenek 1: Blob SAS URL 'SI oluştur

Depolama kapsayıcının erişim düzeyi **Private**olarak ayarlandıysa, kapsayıcının kapsayıcıya veya bloblarına genel okuma erişimini etkinleştirmediğinden bu seçeneği kullanın.

> [!NOTE]
> Bu yöntem, *etkileşimli* testler için çalışmayacaktır. Bkz [. seçenek 2: Kapsayıcı SAS URL 'SI](#option-2-construct-a-container-sas-url)oluşturun.

1. [Azure Portal](https://portal.azure.com/)depolama hesabınıza gidin ve paketinizi içeren. zip sayfasına gidin.

2. Bağlam menüsünden **SAS oluştur** ' u seçin.

3. Izinlerle **Oku** öğesini seçin.

4. **Başlangıç** saatini geçerli saate ve **bitiş zamanı** ' nı **başlangıç zamanından**en az 48 saat olacak şekilde ayarlayın. Aynı pakete sahip başka iş akışları oluşturuyorsanız, test ettiğiniz sürenin uzunluğu için **bitiş saatini** artırmayı düşünün.

5. Seçin **blob SAS belirteci ve URL üretmek**.

Portala paket blobu URL 'Leri sağlarken **BLOB SAS URL 'sini** kullanın.

#### <a name="option-2-construct-a-container-sas-url"></a>Seçenek 2: Kapsayıcı SAS URL 'SI oluşturun

Depolama kapsayıcının erişim düzeyi **özel** olarak ayarlandıysa ve *etkileşimli* teste bir paket blobu URL 'si sağlamanız gerekiyorsa bu seçeneği kullanın. Bu URL, iş akışı düzeyinde de kullanılabilir.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. **Izin verilen hizmetler seçeneklerinden** **BLOB** ' u seçin. Kalan seçeneklerin seçimini kaldırın.

1. **Izin verilen kaynak türlerinden** **kapsayıcı** ve **nesne** ' yi seçin.

1. **Izin verilen Izinlerle** **Oku** ve **Listele** öğesini seçin. Kalan seçeneklerin seçimini kaldırın.

1. Başlangıç zamanından itibaren geçerli saat ve **bitiş zamanı** için en az 14 gün olarak **Başlangıç saati** **seçin.** Aynı paketle diğer testleri çalıştırıyorsanız, test süresinin uzunluğu için **bitiş saatini** artırmayı düşünün. **Bitiş zamanından** sonra Vaas aracılığıyla zamanlanan tüm testler başarısız olur ve yenı bir SAS oluşturulması gerekir.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    Biçim aşağıdaki gibi görünmelidir:`https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Oluşturulan SAS URL 'sini, paket kapsayıcısını, `{containername}`ve paket `{mypackage.zip}`Blobun adını aşağıdaki şekilde içerecek şekilde değiştirin:`https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Portala paket blobu URL 'Leri sağlarken bu değeri kullanın.

#### <a name="option-3-grant-public-read-access"></a>Seçenek 3: Genel okuma erişimi verme

Kimliği doğrulanmamış istemcilerin tek tek bloblara erişmesine izin vermek için kabul edilebilir veya *etkileşimli* testler söz konusu olduğunda kapsayıcı bu seçeneği kullanın.

> [!CAUTION]
> Bu seçenek, anonim salt okuma erişimi için bloba (ler) açar.

1. [Kapsayıcı ve bloblara anonim kullanıcılara Izin verme](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs)bölümündeki yönergeleri izleyerek paket kapsayıcısının erişim düzeyini **BLOB** veya **kapsayıcı** olarak ayarlayın.

    > [!NOTE]
    > *Etkileşimli* teste yönelik bir paket URL 'si sağlıyorsanız, teste devam etmek için kapsayıcıya **tam genel okuma erişimi** vermeniz gerekir.

1. Paket kapsayıcısında, Özellikler bölmesini açmak için paket blobu ' nı seçin.

1. **URL 'yi**kopyalayın. Portala paket blobu URL 'Leri sağlarken bu değeri kullanın.

## <a name="create-a-package-validation-workflow"></a>Paket doğrulama iş akışı oluşturma

1. [Vaas portalında](https://azurestackvalidation.com)oturum açın.

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. **Paket doğrulama** kutucuğunda **Başlat** ' ı seçin.

    ![Paket doğrulamaları iş akışı kutucuğu](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Microsoft 'tan imza gerektiren test imzalı OEM paketine Azure Depolama Blobu URL 'sini girin. Yönergeler için bkz. [VaaS için paket blobu URL 'Si oluşturma](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Ortam parametreleri, iş akışı oluşturulduktan sonra değiştirilemez.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testler özet sayfasına yönlendirilirsiniz.

## <a name="required-tests"></a>Gerekli testler

OEM paket doğrulaması için aşağıdaki testler gereklidir:

- OEM Uzantı paketi doğrulaması
- Bulut benzetimi altyapısı

## <a name="run-package-validation-tests"></a>Paket doğrulama testlerini Çalıştır

1. **Paket doğrulama sınamaları Özeti** sayfasında, senaryolarınız için uygun olan listelenmiş testlerin bir alt kümesini çalıştırırsınız.

    Doğrulama iş akışlarında, bir testin **zamanlanması** , iş akışı oluşturma sırasında belirttiğiniz iş akışı düzeyi ortak parametrelerini kullanır (bkz. [hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametresi değeri geçersiz hale gelirse, [iş akışı parametrelerini değiştirme](azure-stack-vaas-monitor-test.md#change-workflow-parameters)bölümünde belirtildiği gibi onları yeniden silmelisiniz.

    > [!NOTE]
    > Mevcut bir örnek üzerinde bir doğrulama testinin zamanlanması, portalda eski örnek yerine yeni bir örnek oluşturur. Eski örnek için Günlükler korunur, ancak portaldan erişilebilir olmayacaktır.  
    > Bir sınama başarıyla tamamlandıktan sonra, **zamanlama** eylemi devre dışı bırakılır.

2. Testi çalıştıracak aracıyı seçin. Yerel test yürütme aracıları ekleme hakkında daha fazla bilgi için bkz. [Yerel aracıyı dağıtma](azure-stack-vaas-local-agent.md).

3. OEM Uzantı paketi doğrulamasını gerçekleştirmek için, bağlam menüsünden **zamanla** ' yı seçerek test örneğini zamanlamaya yönelik bir istem açın.

4. Test parametrelerini gözden geçirin ve ardından **Gönder** ' i seçerek OEM Uzantı paketi doğrulamasını yürütme için zamanlayın.

    OEM Uzantı paketi doğrulaması iki el ile iki adıma ayrılır: Azure Stack Update ve OEM Update.

   1. **Seç** Ön denetim betiğini yürütmek için Kullanıcı arabiriminde "Çalıştır". Bu, yaklaşık 5 dakika süren ve eylem gerektirmeyen otomatikleştirilmiş bir sınamadır.

   1. Ön denetim betiği tamamlandıktan sonra, el ile adımı gerçekleştirin: Azure Stack portalını kullanarak en son kullanılabilir Azure Stack güncelleştirmesini **yükler** .

   1. **Çalıştır** Damgada test-AzureStack. Herhangi bir başarısızlık oluşursa, teste ve ilgili kişiyle [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com)devam etmez.

       Test-AzureStack komutunu çalıştırma hakkında daha fazla bilgi için bkz. [Azure Stack sistem durumunu doğrulama](../operator/azure-stack-diagnostic-test.md).

   1. **Seç** "İleri", postcheck betiğini yürütmek için. Bu otomatikleştirilmiş bir sınamadır ve Azure Stack güncelleştirme işleminin sonunu işaretler.

   1. **Seç** "Çalıştır" komutu, OEM güncelleştirmesi için önceden denetim betiğini yürütmek üzere.

   1. Ön denetim tamamlandığında, el ile adımı gerçekleştirin: Portal aracılığıyla OEM uzantısı paketini **yükler** .

   1. **Çalıştır** Damgada test-AzureStack.

      > [!NOTE]
      > Daha önce olduğu gibi, teste devam etmez ve başarısız olursa [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) test ve iletişim kurun. Bu adım, sizin için bir yeniden dağıtım kaydedileceği için önemlidir.

   1. **Seç** "İleri", postcheck betiğini yürütmek için. Bu, OEM güncelleştirme adımının sonunu işaretler.

   1. Testin sonunda kalan tüm soruları yanıtlayın ve "Gönder" **seçeneğini belirleyin** .

   1. Bu, etkileşimli testin sonunu işaretler.

5. OEM Uzantı paketi doğrulaması sonucunu gözden geçirin. Sınama başarılı olduktan sonra, bulut benzetimi altyapısını yürütme için zamanlayın.

Bir paket imzalama isteği göndermek için, bu [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) çalıştırmadan ilişkili çözüm adını ve paket doğrulama adını gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)
