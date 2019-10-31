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
ms.openlocfilehash: 20d5d2d962fbe85db5d4725c864defe84c2c152c
ms.sourcegitcommit: cc3534e09ad916bb693215d21ac13aed1d8a0dde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73167352"
---
# <a name="validate-oem-packages"></a>OEM paketlerini doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Tamamlanmış bir çözüm doğrulamasının belleniminde veya sürücülerinde değişiklik yapıldığında yeni bir OEM paketini test edebilirsiniz. Paketiniz testi geçtiğinde, Microsoft tarafından imzalanır. Testiniz, Windows Server logosu ve bılgısayar testlerini geçen sürücüler ve üretici yazılımıyla güncelleştirilmiş OEM uzantı paketini içermelidir.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Paketleri karşıya yüklemeden veya göndermeden önce, beklenen paket biçimi ve içerikleri için [BIR OEM paketi oluştur](azure-stack-vaas-create-oem-package.md) ' u gözden geçirin.

## <a name="managing-packages-for-validation"></a>Paketleri doğrulama için yönetme

Paketi doğrulamak için **paket doğrulama** iş akışını kullanırken bir **Azure Storage blob 'una**URL sağlamanız gerekir. Bu blob, güncelleştirme sürecinin bir parçası olarak yüklenecek test imzalı OEM paketidir. Kurulum sırasında oluşturduğunuz Azure Depolama hesabını kullanarak blobu oluşturun (bkz. [doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Önkoşul: depolama kapsayıcısı sağlama

Paket Blobları için depolama hesabınızda bir kapsayıcı oluşturun. Bu kapsayıcı, tüm paket doğrulama çalışmalarınız için kullanılabilir.

1. [Azure Portal](https://portal.azure.com), [doğrulamanız hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)bölümünde oluşturulan depolama hesabına gidin.

2. **BLOB hizmeti**altındaki sol dikey pencerede **kapsayıcılar**' ı seçin.

3. Menü çubuğundan **+ kapsayıcı** ' yı seçin.
    1. Kapsayıcı için bir ad sağlayın (örneğin, `vaaspackages`).
    1. VaaS gibi kimliği doğrulanmamış istemciler için istenen erişim düzeyini seçin. Her senaryodaki paketlere VaaS erişimi verme hakkında daha fazla bilgi için bkz. [kapsayıcı erişim düzeyini işleme](#handling-container-access-level).

### <a name="upload-package-to-storage-account"></a>Paketi depolama hesabına yükle

1. Doğrulamak istediğiniz paketi hazırlayın. Bu, içeriği [BIR OEM paketi oluşturma](azure-stack-vaas-create-oem-package.md)bölümünde açıklanan yapıyla eşleşmesi gereken bir `.zip` dosyasıdır.

    > [!NOTE]
    > Lütfen `.zip` içeriğinin `.zip` dosyasının köküne yerleştirildiğinden emin olun. Pakette hiçbir alt klasör olmaması gerekir.

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

#### <a name="option-1-generate-a-blob-sas-url"></a>Seçenek 1: blob SAS URL 'SI oluşturma

Depolama kapsayıcının erişim düzeyi **Private**olarak ayarlandıysa, kapsayıcının kapsayıcıya veya bloblarına genel okuma erişimini etkinleştirmediğinden bu seçeneği kullanın.

> [!NOTE]
> Bu yöntem, *etkileşimli* testler için çalışmayacaktır. Bkz. [2. seçenek: KAPSAYıCı SAS URL 'si oluşturma](#option-2-construct-a-container-sas-url).

1. [Azure Portal](https://portal.azure.com/)depolama hesabınıza gidin ve paketinizi içeren. zip sayfasına gidin.

2. Bağlam menüsünden **SAS oluştur** ' u seçin.

3. Izinlerle **Oku** öğesiniseçin.

4. **Başlangıç** saatini geçerli saate ve **bitiş zamanı** ' nı **başlangıç zamanından**en az 48 saat olacak şekilde ayarlayın. Aynı pakete sahip başka iş akışları oluşturuyorsanız, test ettiğiniz sürenin uzunluğu için **bitiş saatini** artırmayı düşünün.

5. **BLOB SAS belirteci oluştur ve URL 'yi**seçin.

Portala paket blobu URL 'Leri sağlarken **BLOB SAS URL 'sini** kullanın.

#### <a name="option-2-construct-a-container-sas-url"></a>2\. seçenek: kapsayıcı SAS URL 'SI oluşturun

Depolama kapsayıcının erişim düzeyi **özel** olarak ayarlandıysa ve *etkileşimli* teste bir paket blobu URL 'si sağlamanız gerekiyorsa bu seçeneği kullanın. Bu URL, iş akışı düzeyinde de kullanılabilir.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. **Izin verilen hizmetler seçeneklerinden** **BLOB** ' u seçin. Kalan seçeneklerin seçimini kaldırın.

1. **Izin verilen kaynak türlerinden** **kapsayıcı** ve **nesne** ' yi seçin.

1. **Izin verilen Izinlerle** **Oku** ve **Listele** öğesini seçin. Kalan seçeneklerin seçimini kaldırın.

1. Başlangıç zamanından itibaren geçerli saat ve **bitiş zamanı** için en az 14 gün olarak **Başlangıç saati** **seçin.** Aynı paketle diğer testleri çalıştırıyorsanız, test süresinin uzunluğu için **bitiş saatini** artırmayı düşünün. **Bitiş zamanından** sonra Vaas aracılığıyla zamanlanan tüm testler başarısız olur ve yenı bir SAS oluşturulması gerekir.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    Biçim aşağıdaki gibi görünmelidir: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Oluşturulan SAS URL 'sini, paket kapsayıcısını, `{containername}`ve paket Blobun adını (`{mypackage.zip}`) içerecek şekilde değiştirin: `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

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

6. AzureStack güncelleştirme paketi klasörünü DVD üzerindeki yerel bir dizine kopyalayın. ' AzureStack güncelleştirme paketi klasör yolu ' için üst dizin yolunu girin

7. Yukarıda oluşturulan OEM paketi klasörünü DVD üzerindeki yerel bir dizine kopyalayın. ' OEM güncelleştirme paketi klasör yolu ' için üst dizin yolunu girin

    > [!NOTE]
    > AzureStack Update ve OEM Update 'i **2 ayrı** üst dizine kopyalayın.

8. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Ortam parametreleri, iş akışı oluşturulduktan sonra değiştirilemez.

9. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

10. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testler özet sayfasına yönlendirilirsiniz.

## <a name="required-tests"></a>Gerekli testler

Aşağıdaki testlerin, OEM paketi doğrulaması için belirtilen sırayla çalıştırılması gerekir:

- 1\. adım-aylık AzureStack güncelleştirme doğrulaması
- Adım 2-OEM Uzantı paketi doğrulaması
- 3\. adım-OEM-Cloud simülasyon altyapısı

## <a name="run-package-validation-tests"></a>Paket doğrulama testlerini Çalıştır

1. **Paket doğrulama sınamaları Özeti** sayfasında, senaryolarınız için uygun olan listelenmiş testlerin bir alt kümesini çalıştırırsınız.

    Doğrulama iş akışlarında, bir testin **zamanlanması** , iş akışı oluşturma sırasında belirttiğiniz iş akışı düzeyi ortak parametrelerini kullanır (bkz. [hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametresi değeri geçersiz hale gelirse, [iş akışı parametrelerini değiştirme](azure-stack-vaas-monitor-test.md#change-workflow-parameters)bölümünde belirtildiği gibi onları yeniden silmelisiniz.

    > [!NOTE]
    > Mevcut bir örnek üzerinde bir doğrulama testinin zamanlanması, portalda eski örnek yerine yeni bir örnek oluşturur. Eski örnek için Günlükler korunur, ancak portaldan erişilebilir olmayacaktır.  
    > Bir sınama başarıyla tamamlandıktan sonra, **zamanlama** eylemi devre dışı bırakılır.

2. Paket doğrulaması için, **gerekli testleri**listelenen sırayla çalıştırırsınız.

    > [!CAUTION]
    > VaaS, testleri zamanlandığı sırada çalıştırır. Testlerin belirtilen sırada zamanlanmaları gerekir.

3. Testi çalıştıracak aracıyı seçin. Yerel test yürütme aracıları ekleme hakkında daha fazla bilgi için bkz. [Yerel aracıyı dağıtma](azure-stack-vaas-local-agent.md).

4. Test çalıştırmasını zamanlamak için, bağlam menüsünden **zamanlama** ' yı seçerek test örneğini zamanlamaya yönelik bir istem açın.

5. Testi zamanlamak için test parametrelerini gözden geçirin ve ardından **Gönder** ' i seçin.

6. Bir sonraki testi planlamadan önce testin tamamlanmasını beklemeniz gerekmez. Tüm **gerekli** testleri yukarıda belirtilen sırayla zamanlayın.

7. **Gerekli** testlerin sonuçlarını gözden geçirin.

Bir paket imzalama isteği göndermek için, bu çalıştırma ile ilişkili çözüm adı ve paket doğrulama adı [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [VaaS portalındaki testleri izleme ve yönetme](azure-stack-vaas-monitor-test.md)
