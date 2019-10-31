---
title: Azure Stack 'de depolama hesaplarınızı yedekleme | Microsoft Docs
description: Depolama hesaplarınızı Azure Stack nasıl yedekleyeceğinizi öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/19/2019
ms.openlocfilehash: 3f6ed5604bd2d32d9d030ceb4b5f67567362cc34
ms.sourcegitcommit: 4d7611d81da6f2f8ef50adab3c09f9122a75bc9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145767"
---
# <a name="back-up-your-storage-accounts-on-azure-stack"></a>Depolama hesaplarınızı Azure Stack yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makale, Azure Stack üzerindeki bir Azure depolama hesabı içindeki depolama hesaplarının korunmasına ve kurtarılmasına bakar.

## <a name="elements-of-the-solution"></a>Çözüm öğeleri

Bu bölüm, çözümün ve önemli parçaların genel yapısına bakar.

![Azure Stack depolama yedeklemesi](./media/azure-stack-network-howto-backup-storage/azure-stack-storage-backup.png)

### <a name="application-layer"></a>Uygulama katmanı

Veriler, birden çok konuma nesne yazmak için birden çok [PUT blobu](https://docs.microsoft.com/rest/api/storageservices/put-blob) veya [PUT bloğu](https://docs.microsoft.com/rest/api/storageservices/put-block) işlemleri vererek ayrı Azure Stack ölçek birimlerindeki depolama hesapları arasında çoğaltılabilir. Alternatif olarak, uygulama, birincil hesaba Put işlemi tamamlandıktan sonra blob 'u ayrı bir ölçek biriminde barındırılan bir depolama hesabına kopyalamak için [BLOB kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemini verebilir.

### <a name="scheduled-copy-task"></a>Zamanlanmış kopyalama görevi

AzCopy, verileri yerel dosya sistemlerinden, Azure bulut depolama, Azure Stack Storage ve S3 'tan kopyalamak için kullanılabilen harika bir araçtır. Şu anda AzCopy iki Azure Stack depolama hesabı arasında veri kopyalayamıyor. Nesneleri bir kaynak Azure Stack depolama hesabından hedef Azure Stack depolama hesabına kopyalamak bir ara yerel dosya sistemi gerektirir.

Daha fazla bilgi için [Azure Stack Storage 'da veri aktarma araçları 'Nı kullanma](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer?view=azs-1908#azcopy) makalesindeki AzCopy bölümüne bakın.

### <a name="azure-stack-source"></a>Azure Stack (kaynak)

Bu, yedeklemek istediğiniz depolama hesabı verilerinin kaynağıdır.

Kaynak depolama hesabı URL 'SI ve SAS belirtecine ihtiyacınız olacaktır. Depolama hesabıyla çalışma yönergeleri için bkz. [Azure Stack Storage geliştirme araçları](azure-stack-storage-dev.md)'nı kullanmaya başlama.

### <a name="azure-stack-target"></a>Azure Stack (hedef)

Bu, yedeklemek istediğiniz hesap verilerini depolayacak olan hedeftir. Hedef Azure Stack örneği, hedef Azure Stack farklı bir konumda olmalıdır. Ve kaynağın hedefe bağlanabiliyor olması gerekir.

Kaynak depolama hesabı URL 'SI ve SAS belirtecine ihtiyacınız olacaktır. Depolama hesabıyla çalışma yönergeleri için bkz. [Azure Stack Storage geliştirme araçları](azure-stack-storage-dev.md)'nı kullanmaya başlama.

### <a name="intermediary-local-filesystem"></a>Ara yerel dosya sistemi

AzCopy çalıştırmak ve kaynağınızdan kopyalama sırasında verileri depolamak ve ardından hedef Azure Stack yazmak için bir yer gerekecektir. Bu, kaynak Azure Stack bir ara sunucu.

Ara sunucunuz olarak bir Linux veya Windows Server oluşturabilirsiniz. Sunucuda, kaynak depolama hesabı kapsayıcılarındaki tüm nesneleri depolamak için yeterli alan olması gerekir.
- Linux sunucusu kurma yönergeleri için, bkz. [Azure Stack portalını kullanarak bir Linux sunucu VM 'Si oluşturma](azure-stack-quick-linux-portal.md).  
- Windows Server ayarlama yönergeleri için, bkz. [Azure Stack portalı Ile Windows Server VM oluşturma](azure-stack-quick-windows-portal.md).  

Windows Server 'ı ayarladıktan sonra [Azure Stack PowerShell](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install?toc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fbreadcrumb%2Ftoc.json) ve [Azure Stack araçları](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-download?toc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fbreadcrumb%2Ftoc.json)'nı yüklemeniz gerekir.

## <a name="set-up-backup-for-storage-accounts"></a>Depolama hesapları için yedeklemeyi ayarlama

1. Kaynak ve hedef depolama hesapları için blob uç noktasını alın.

    ![Azure Stack depolama yedeklemesi](./media/azure-stack-network-howto-backup-storage/back-up-step1.png)

2. Kaynak ve hedef depolama hesapları için SAS belirteçleri oluşturun ve kaydedin.

    ![Azure Stack depolama yedeklemesi](./media/azure-stack-network-howto-backup-storage/back-up-step2.png)

3. Ara sunucuya [AzCopy](https://github.com/Azure/azure-storage-azcopy) 'i kurun ve apı sürümünü Azure Stack depolama hesapları için hesap olarak ayarlayın.

    - Bir Windows Server için:

    ```PowerShell  
    set AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09 PowerShell use: $env:AZCOPY_DEFAULT_SERVICE_API_VERSION="2017-11-09"
    ```

    - Bir Linux (Ubuntu) sunucusu için:

    ```bash  
    export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09
    ```

4. Ara sunucunuzda betik oluşturun. Bu komutu **depolama hesabınız**, **SAS anahtarınız**ve **Yerel Dizin yolınızla**güncelleştirin. **Kaynak** depolama hesabından artımlı olarak veri kopyalamak için betiği çalıştırırsınız.

    ```
    azcopy sync "https:/<storagaccount>/<container>?<SAS Key>" "C:\\myFolder" --recursive=true --delete-destination=true
    ```

5.  **Depolama hesabı**, * * SAS anahtarı * * ve * * yerel dizin yolunu girin.  Bunu, verileri artımlı olarak **hedef** depolama hesabına kopyalamak için kullanacaksınız
    
    ```
    azcopy sync "C:\\myFolder" "https:// <storagaccount>/<container>?<SAS Key>" --recursive=true --delete-destination=true
    ```

6.  Kaynak Azure Stack depolama hesabından ara sunucudaki yerel depolamaya olan kopyayı zamanlamak için cron veya Windows Görev Zamanlayıcı kullanın. Ardından ara sunucudaki yerel depolama alanından hedef Azure Stack depolama hesabına kopyalayın.

    Bu çözümle elde ettiğiniz RPO,/MO parametre değeri ve kaynak hesap ile ara sunucu ile ara sunucu ve hedef hesap arasındaki ağ bant genişliğine göre belirlenir.

    - Bir Linux (Ubuntu) sunucusu için:

    ```bash  
    schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\\&lt;script name>.bat
    ```

    | Parametre | Not | 
    | ---- | ---- |
    | /SC | Bir dakika zamanlaması kullanın. |
    | /AY | *Xx* dakikalık bir Aralık. |
    | /TN | Görev adı. |
    | /TR | `script.bat` dosyanın yolu. |


    - Bir Windows Server için:

    Windows görev zamanlamasını kullanma hakkında daha fazla bilgi için bkz. [geliştiriciler için Görev Zamanlayıcı](https://docs.microsoft.com/windows/win32/taskschd/task-scheduler-start-page)
    

## <a name="use-your-storage-account-in-a-disaster"></a>Depolama hesabınızı bir olağanüstü durumda kullanın

Her bir Azure Stack depolama hesabı, Azure Stack bölgesinin kendisinden türetilmiş benzersiz bir DNS adı vardır, örneğin, `https://krsource.blob.east.asicdc.com/`. Bu DNS adından yazma ve okuma uygulamalarının, hedef hesap, örneğin, bir olağanüstü durum sırasında kullanılması gereken `https://krtarget.blob.west.asicdc.com/` depolama hesabı DNS adı değişikliğine uyum sağlaması gerekir.

Uygulama bağlantı dizeleri, bir olağanüstü durum, nesnelerin yeniden konumlandırılmasını hesaba göre bildirildiğinde değiştirilebilir veya bir CNAME kaydı, kaynak ve hedef depolama hesaplarını ön sona erdirmeden önce bir yük dengeleyicinin önünde kullanılıyorsa, yük dengeleyici yapılandırılabilir yöneticinin hedefi bildirmesine izin veren bir el ile yük devretme algoritması ile

SAS, AAD veya AD FS yerine uygulama tarafından kullanılıyorsa Yukarıdaki yöntem çalışmaz ve uygulama bağlantı dizelerinin, hedef depolama hesabı URL 'SI ve hedef depolama hesabı için oluşturulan SAS anahtarları ile güncelleştirilmeleri gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Storage geliştirme araçları 'nı kullanmaya başlama](azure-stack-storage-dev.md)