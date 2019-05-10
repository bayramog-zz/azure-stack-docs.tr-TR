---
title: Dağıtım çalışma sayfası için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Azure Stack dağıtmak için dağıtım çalışma aracı yükleyip kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: wamota
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2019
ms.author: wamota
ms.reviewer: wamota
ms.lastreviewed: 04/19/2019
ms.openlocfilehash: d75915f110b6316f4621f66b1f91b010f735d165
ms.sourcegitcommit: ccd86bd0862c45de1f6a4993f783ea2e186c187a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65172659"
---
# <a name="deployment-worksheet-for-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemleri için dağıtım çalışma sayfası

Azure Stack dağıtım çalışma sayfası, tüm gerekli dağıtım bilgileri ve kararları tek bir yerde toplayan bir Windows Forms uygulamasıdır. Planlama işlemi sırasında dağıtım çalışma sayfası tamamlamak ve dağıtım başlamadan önce gözden geçirin.

Çalışma sayfası tarafından gerekli bilgiler, ağ, güvenlik ve kimlik bilgilerini ele alınmaktadır. Bilgi Bankası pek çok farklı alanlarda gerekebilir önemli kararlar gerektiriyor; Bu nedenle, çalışma tamamlamak için bu alanlarda uzmanlığı işlediği takımlarla başvurun isteyebilirsiniz.

Çalışma sayfasını doldurma sırasında ağ ortamınıza bazı dağıtım öncesi yapılandırma değişiklikleri yapmanız gerekebilir. Bu IP adresi alanları Azure Stack çözüm için koruma ve yönlendiriciler, anahtarlar ve yeni Azure Stack çözüm bağlantısı hazırlamak için güvenlik duvarları, yapılandırma içerebilir.

> [!NOTE]
> Dağıtım çalışma Aracı'nı tamamlama hakkında daha fazla bilgi için bkz. [bu makalede Azure Stack belgeleri](azure-stack-datacenter-integration.md).

[![Dağıtım çalışma sayfası](media/azure-stack-deployment-worksheet/depworksheet.png "dağıtım çalışma sayfası")](media/azure-stack-deployment-worksheet/depworksheet.png)

## <a name="installing-the-windows-powershell-module"></a>Windows PowerShell modülünü yükleme

Dağıtım çalışma sayfası her sürümü için bir Powershell modülü, dağıtım çalışma sayfası kullanmak istediğiniz her makine için tek seferlik bir yüklemesi gerçekleştirmeniz gerekir.

> [!NOTE]  
> Çalışmak için bu yöntem için internet için bilgisayarı yeniden bağlanması gerekir.

1. Yükseltilmiş bir PowerShell istemi açın.

2. PowerShell penceresinde modülünden yükleme [PowerShell Galerisi](https://www.powershellgallery.com/packages/Azs.Deployment.Worksheet/):

   ```PowerShell
   Install-Module -Name Azs.Deployment.Worksheet -Repository PSGallery
   ```

Güvenilmeyen bir depodan yükleme hakkında bir ileti alırsanız basın **Y** yüklemeye devam etmek için.

## <a name="use-the-deployment-worksheet-tool"></a>Dağıtım çalışma aracını kullanma

Başlatma ve dağıtım çalışma PowerShell Modülü yüklü bir bilgisayara dağıtım çalışma sayfası kullanmak için aşağıdaki adımları gerçekleştirin:

1. Windows PowerShell'i başlatın (beklenmedik sonuçlar oluşabilir gibi PowerShell ISE kullanmayın). PowerShell'i yönetici olarak çalıştırmak gerekli değildir.

2. İçeri aktarma **AzS.Deployment.Worksheet** PowerShell Modülü:

   ```PowerShell
   Import-Module AzS.Deployment.Worksheet
   ```

3. Modül içeri aktarıldıktan sonra dağıtım çalışma sayfası başlatın:

   ```PowerShell
   Start-DeploymentWorksheet
   ```

Dağıtım çalışma sayfası, ortam ayarları gibi toplamak için ayrı sekmeler oluşan **müşteri ayarları**, **ağ ayarlarını**, ve **ölçek birimi #**. Tüm değerler sağlamanız gerekir (herhangi işaretli dışında **isteğe bağlı**) tüm sekmeler önce herhangi bir yapılandırma veri dosyaları oluşturulabilir. Tüm gerekli değerleri araca girilmiş, kullanabileceğiniz **eylem** menüsüne **alma**, **dışarı**, ve **Oluştur**. Dağıtım için gereken JSON dosyaları aşağıdaki gibidir:

**İçeri aktarma**: Bu aracı veya dağıtım çalışma sayfası önceki bir sürümü tarafından oluşturulanlar tarafından oluşturulmuş bir Azure Stack yapılandırma veri dosyası (ConfigurationData.json) almanızı sağlar. Bir içeri aktarılmasına formları sıfırlar ve tüm daha önce girilen ayarlama siler veya veri oluşturulabilir.

**Dışarı aktarma**: Şu anda formlara girilen veriler doğrular, atamalarını ve IP alt ağları oluşturur ve sonra içeriği JSON biçimli yapılandırma dosyaları kaydeder. Ardından, ağ yapılandırması oluşturun ve Azure Stack yüklemek için bu dosyaları kullanabilirsiniz.

**Oluşturma**: Şu anda girilen verileri doğrular ve dağıtım JSON dosyaları vermeden ağ eşlemesi oluşturur. İki yeni sekmeler, oluşturulan **Oluştur** başarılı olur: **Alt ağ Özet** ve **IP atamalarının**. Ağ atamalarını beklendiği gibi olduğundan emin olmak için bu sekmelerdeki verileri analiz edebilirsiniz.

**Tümünü Temizle**: Şu anda formlara girilen tüm verileri temizler ve varsayılan değerleri döndürür.

**Kaydedin veya açın, iş devam eden**: Kaydet ve kısmen girilen verileri kullanarak, üzerinde çalıştığınız açın **Dosya -> Kaydet** ve **Dosya -> Aç** menüleri. Bu farklıdır **alma** ve **dışarı** çözülemediğinden ve girdiğiniz tüm verilerin bu şekilde çalışır. Aç/Kaydet doğrulamamanız ve Süren kaydetmek için girdiğiniz tüm alanları gerektirmez.

**Günlüğe kaydetme ve uyarı iletilerini**: Formun kullanılırken, PowerShell penceresinde görüntülenen kritik uyarı iletileri görebilirsiniz. Kritik Hatalar bir açılır ileti görüntülenir. Diske yazılan için günlük'dahil olmak üzere, isteğe bağlı ayrıntılı günlük sorun gidermeye yardımcı olması için etkin olmalıdır.

Ayrıntılı günlük kaydı ile aracını başlatmak için:

   ```PowerShell
   Start-DeploymentWorksheet -EnableLogging
   ```

Geçerli kullanıcının kayıtlı günlük bulabilirsiniz **Temp** dizin; örneğin: **C:\Users\me\AppData\Local\Temp\Microsoft_AzureStack\DeploymentWorksheet_Log.txt**.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack dağıtımı bağlantı modelleri](azure-stack-connection-models.md)
