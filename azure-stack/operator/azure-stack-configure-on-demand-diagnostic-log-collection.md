---
title: Azure Stack tanılama günlüklerini şimdi toplayın | Microsoft Docs
description: Azure Stack yardım + destek içinde isteğe bağlı tanılama günlüğü koleksiyonunu yapılandırma.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/31/2019
ms.openlocfilehash: 007703facf882eccd594dfb6f78941570c9cfb02
ms.sourcegitcommit: 8de4c18b25bd1047fc270812a795f24e8f1e9244
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68865898"
---
# <a name="collect-azure-stack-diagnostic-logs-now-on-demand"></a>Azure Stack tanılama günlüklerini şimdi toplayın (isteğe bağlı)

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Sorun giderme kapsamında, Microsoft Müşteri Destek Hizmetleri (CSS) tanılama günlüklerini analiz etmeniz gerekebilir. Azure Stack işleçleri, 1907 sürümünden itibaren, **Yardım ve destek**kullanarak Azure 'daki bir blob kapsayıcısına isteğe bağlı tanılama günlüklerini karşıya yükleyebilir. Portal kullanılamıyorsa alternatif olarak, işleçler ayrıcalıklı uç nokta (PEP) aracılığıyla get-AzureStackLog komutunu kullanarak günlük toplayabilirler. Bu konu, istek üzerine tanılama günlükleri toplamanın her iki yolunu da içerir.

## <a name="using-help-and-support"></a>Yardım ve destek kullanma

Bir sorunu gidermek için CSS, önceki haftadan belirli bir zaman penceresi için isteğe bağlı olarak tanılama günlüklerini toplamak üzere bir Azure Stack işleci isteyebilir. Bu durumda, CSS koleksiyonu karşıya yüklemek için bir SAS URL 'SI ile işleci sağlar. CSS 'den SAS URL 'sini kullanarak isteğe bağlı günlük toplamayı yapılandırmak için aşağıdaki adımları kullanın:

1. **Yardım ve desteğe genel bakış** ' ı açın ve **günlükleri topla**' ya tıklayın. 
1. Son yedi günden sonraki bir 1-4 saat kayan penceresi seçin. 
1. Yerel Saat dilimini seçin.
1. CSS 'nin sağlandığı SAS URL 'sini girin.

   ![İsteğe bağlı günlük toplama ekran görüntüsü](media/azure-stack-automatic-log-collection/collect-logs-now.png)

>[!NOTE]
>Otomatik tanılama günlüğü koleksiyonu etkinleştirilirse, günlük toplama işlemi devam ederken **Yardım ve destek** görüntülenir. Otomatik günlük toplama işlemi devam ederken belirli bir zamandan günlükleri toplamak için **günlükleri topla** ' ya tıklarsanız, otomatik günlük toplama tamamlandıktan sonra isteğe bağlı koleksiyon başlar. 

## <a name="using-pep"></a>PEP kullanma

<!--how do you look up the PEP IP address. You look up the azurestackstampinfo.json--->

Azure Stack tanılama araçları, günlük toplamayı kolay ve verimli hale getirmenize yardımcı olur. Aşağıdaki diyagramda, tanılama araçlarının nasıl çalıştığı gösterilmektedir:

![Azure Stack tanılama araçları](media/azure-stack-diagnostics/get-azslogs.png)

### <a name="trace-collector"></a>İzleme Toplayıcısı

İzleme Toplayıcısı varsayılan olarak etkindir ve Azure Stack bileşen hizmetlerinden Windows için olay Izleme (ETW) günlüklerini toplamak üzere arka planda sürekli olarak çalışır. ETW günlükleri, beş günlük yaş sınırına sahip ortak bir yerel paylaşımda depolanır. Bu sınıra ulaşıldığında, yeni olanlar oluşturulmuşdan en eski dosyalar silinir. Her dosya için izin verilen varsayılan en büyük boyut 200 MB 'tır. Boyut denetimi her 2 dakikada bir gerçekleşir ve geçerli dosya > = 200 MB ise, kaydedilir ve yeni bir dosya oluşturulur. Ayrıca, olay oturumu başına oluşturulan toplam dosya boyutu için 8 GB bir sınır vardır.

### <a name="get-azurestacklog"></a>Get-AzureStackLog

Get-AzureStackLog PowerShell cmdlet 'i, bir Azure Stack ortamındaki tüm bileşenlerin günlüklerini toplamak için kullanılabilir. Bunları, Kullanıcı tanımlı bir konumdaki ZIP dosyalarına kaydeder. Azure Stack teknik destek ekibinin bir sorunu gidermeye yardımcı olmak için günlüklerinizi ihtiyacı varsa, Get-AzureStackLog komutunu çalıştırmanızı isteyebilir.

> [!CAUTION]
> Bu günlük dosyaları, kişisel olarak tanımlanabilir bilgiler (PII) içerebilir. Herhangi bir günlük dosyasını genel olarak göndermeden önce bunu hesaba gönderin.

Aşağıda, toplanan bazı örnek günlük türleri verilmiştir:

* **Azure Stack dağıtım günlükleri**
* **Windows olay günlükleri**
* **Panther günlükleri**
* **Küme günlükleri**
* **Depolama tanılama günlükleri**
* **ETW günlükleri**

Bu dosyalar, İzleme Toplayıcısı bir paylaşımında toplanır ve kaydedilir. Get-AzureStackLog daha sonra gerektiğinde bunları toplamak için kullanılabilir.

#### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemlerde Get-AzureStackLog komutunu çalıştırmak için

Tümleşik bir sistemde Get-AzureStackLog komutunu çalıştırmak için ayrıcalıklı uç noktaya (PEP) erişiminizin olması gerekir. Aşağıda, tümleşik bir sistemde günlükleri toplamak için PEP kullanarak çalıştırabileceğiniz örnek bir betik verilmiştir:

```powershell
$ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

$password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)

$shareCred = Get-Credential

$session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2) # Provide the time that includes the period for your issue

Invoke-Command -Session $session { Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if ($session) {
    Remove-PSSession -Session $session
}
```

#### <a name="run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Azure Stack Geliştirme Seti (ASDK) sisteminde Get-AzureStackLog komutunu çalıştırma

Bir asdk ana bilgisayarında `Get-AzureStackLog` çalıştırmak için bu adımları kullanın.

1. ASDK ana bilgisayarında **AzureStack\CloudAdmin** olarak oturum açın.
2. Yönetici olarak yeni bir PowerShell penceresi açın.
3. **Get-AzureStackLog** PowerShell cmdlet 'ini çalıştırın.

#### <a name="examples"></a>Örnekler

* Tüm roller için tüm günlükleri topla:

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred
  ```

* VirtualMachines ve BareMetal rollerinin günlüklerini toplayın:

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal
  ```

* Son 8 saat için günlük dosyaları için tarih filtreleyerek VirtualMachines ve BareMetal rollerinin günlüklerini toplayın:

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

* 8 saat önce ve 2 saat önce bir süre boyunca günlük dosyaları için tarih filtreleyerek VirtualMachines ve BareMetal rollerinin günlüklerini toplayın:

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

* Günlükleri toplayın ve belirtilen Azure Storage blob kapsayıcısında depolayın. Bu işlemin genel sözdizimi şöyledir:

  ```powershell
  Get-AzureStackLog -OutputSasUri "<Blob service SAS Uri>"
  ```

  Örneğin:

  ```powershell
  Get-AzureStackLog -OutputSasUri "https://<storageAccountName>.blob.core.windows.net/<ContainerName><SAS token>"
  ```

  > [!NOTE]
  > Bu yordam, günlükleri karşıya yüklemek için yararlıdır. Erişilebilir bir SMB paylaşımınız veya internet erişimi olmasa bile, günlükleri aktarmak için Azure Stack bir BLOB depolama hesabı oluşturabilir ve ardından bu günlükleri almak için istemcinizi kullanabilirsiniz.  

  Depolama hesabı için SAS belirtecini oluşturmak üzere aşağıdaki izinler gereklidir:

  * BLOB depolama hizmetine erişim
  * Kapsayıcı kaynak türüne erişim

  `-OutputSasUri` Parametresi için kullanılmak üzere bir SAS URI değeri oluşturmak için aşağıdaki adımları uygulayın:

  1. [Bu makaledeki](/azure/storage/common/storage-quickstart-create-account)adımları Izleyerek bir depolama hesabı oluşturun.
  2. Azure Depolama Gezgini bir örneğini açın.
  3. Adım 1 ' de oluşturulan depolama hesabına bağlanın.
  4. **Depolama Hizmetleri**'Nde **BLOB kapsayıcıları** ' na gidin.
  5. **Yeni kapsayıcı oluştur**' u seçin.
  6. Yeni kapsayıcıyı sağ tıklatın ve ardından **paylaşılan erişim Imzasını al**' ı tıklatın.
  7. Gereksinimlerinize bağlı olarak geçerli bir **Başlangıç saati** ve **bitiş saati**seçin.
  8. Gerekli izinler için **Oku**, **yaz**ve **Listele**' yi seçin.
  9. **Oluştur**’u seçin.
  10. Paylaşılan erişim Imzası alacaksınız. URL bölümünü kopyalayın ve `-OutputSasUri` parametresine girin.

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Hem ASDK hem de tümleşik sistemler için parametre konuları

* **Outputsharepath** ve **Outputsharecredential** parametreleri, günlükleri Kullanıcı tarafından belirtilen bir konumda depolamak için kullanılır.

* **FromDate** ve **ToDate** parametreleri belirli bir zaman aralığı için günlükleri toplamak üzere kullanılabilir. Bu parametreler belirtilmezse, günlükler varsayılan olarak son dört saat için toplanır.

* Günlükleri bilgisayar adına göre filtrelemek için **FilterByNode** parametresini kullanın. Örneğin:

    ```powershell
    Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByNode azs-xrp01
    ```

* Günlükleri türe göre filtrelemek için **FilterByLogType** parametresini kullanın. Dosya, paylaşma veya WindowsEvent göre filtrelemeyi seçebilirsiniz. Örneğin:

    ```powershell
    Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByLogType File
    ```

* Günlük toplama için zaman aşımını ayarlamak üzere **Timeoutınminutes** parametresini kullanabilirsiniz. Varsayılan olarak 150 (2,5 saat) olarak ayarlanır.
* Döküm dosyası günlük toplama varsayılan olarak devre dışıdır. Etkinleştirmek için **ıncludedumpfile** Switch parametresini kullanın.
* Şu anda, **FilterByRole** parametresini kullanarak günlük toplamayı aşağıdaki rollere göre filtreleyebilirsiniz:

  |   |   |   |    |     |
  | - | - | - | -  |  -  |
  |ACS                   |CA                             |HRP                            |OboService                |VirtualMachines|
  |ACSBlob               |CacheService                   |IBC                            |OEM                       |BULUNAMADI            |
  |ACSDownloadService    |İşlem                        |InfraServiceController         |OnboardRP                 |WASPUBLIK|
  |ACSFabric             |CPI                            |KeyVaultAdminResourceProvider  |PXE                       |         |
  |ACSFrontEnd           |CRP                            |Keyvaultcontroldüzlemi           |QueryServiceCoordinator   |         | 
  |Acsölçümler            |DeploymentMachine              |Keyvaultdatadüzlemi              |QueryServiceWorker        |         |
  |ACSMigrationService   |DiskRP                         |Keyvaultınternalcontroldüzlemi   |SeedRing                  |         |
  |ACSMonitoringService  |Etki Alanı                         |Keyvaultınternaldatadüzlemi      |SeedRingServices          |         |
  |ACSSettingsService    |ECE                            |KeyVaultNamingService          |SLB                       |         |
  |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL                       |         |
  |ACSTableServer        |EventRP                        |MetricsAdminRP                 |'DE                       |         |
  |ACSWac                |ExternalDNS                    |MetricsRP                      |Depolama                   |         |
  |ADFS                  |FabricRing                     |MetricsServer                  |StorageController         |         |
  |ApplicationController |FabricRingServices             |MetricsStoreService            |URP                       |         |
  |Aşama Ppgateway          |FirstTierAggregationService    |MonAdminRP                     |Supportköprücontroller   |         |
  |AzureBridge           |FRP                            |MonRP                          |Destektring               |         |
  |AzureMonitor          |Ağ geçidi                        |KSK                             |SupportRingServices       |         |
  |BareMetal             |'De               |NonPrivilegedAppGateway        |SupportBridgeRP           |         |
  |BRP                   |HintingServiceV2               |NRP                            |UsageBridge               |         |
  |   |   |   |    |     | 

### <a name="additional-considerations"></a>Diğer konular

* Komutun hangi rol (ler) topladıkları role göre çalıştırılması biraz zaman alır. Ayrıca, katkıda bulunan faktörler günlük toplama için belirtilen süre ve Azure Stack ortamındaki düğümlerin sayısı da dahil değildir.
* Günlük koleksiyonu çalışırken, komutta belirtilen **Outputsharepath** parametresinde oluşturulan yeni klasörü kontrol edin.
* Her rolün günlüğü ayrı ZIP dosyaları içinde vardır. Toplanan günlüklerin boyutuna bağlı olarak, bir rolün günlükleri birden çok ZIP dosyasına bölünmüş olabilir. Bu tür bir rol için, tüm günlük dosyalarını tek bir klasöre geri yüklemek istiyorsanız, toplu olarak sıkıştırılmış bir araç kullanın. Rolün tüm daraltılmış dosyalarını seçin ve **buradan Ayıkla**' yı seçin. Bu, tek bir birleştirilmiş klasörde söz konusu rolün tüm günlük dosyalarını kaldırır.
* **Get-AzureStackLog_Output. log** adlı bir dosya, daraltılmış günlük dosyalarını içeren klasörde da oluşturulur. Bu dosya, günlük toplama sırasında sorun giderme için kullanılabilen komut çıkışının bir günlüğü. Günlük toplama çalıştıktan sonra beklenen `PS>TerminatingError` günlük dosyaları eksik olmadığı takdirde, günlük dosyası bazen güvenle yoksayılabilir olan girişleri içerir.
* Belirli bir sorunu araştırmak için, birden fazla bileşenden günlük gerekebilir.

  * Tüm altyapı VM 'Leri için sistem ve olay günlükleri **Virtualmachines** rolünde toplanır.
  * Tüm konaklar için sistem ve olay günlükleri **Baremetal** rolünde toplanır.
  * Yük devretme kümesi ve Hyper-V olay günlükleri **depolama** rolünde toplanır.
  * ACS günlükleri **depolama** ve **ACS** rollerinde toplanır.

> [!NOTE]
> Boyut ve yaş sınırları, depolama alanınızı, Günlükler ile taşmadığından emin olmak için verimli bir şekilde kullanımının güvence altına almak açısından, toplanan günlüklerde zorlanır. Ancak, bir sorunu tanılarken bazen bu limitlerin nedeniyle mevcut olmayan günlüklere ihtiyacınız olabilir. Bu nedenle, günlüklerinizi her 8-12 saate kadar bir harici depolama alanına (Azure 'daki bir depolama hesabı, şirket içi depolama cihazı vb.) boşaltmasını ve bunları, gereksinimlerinize bağlı olarak 1-3 ay süreyle saklamanızı öneririz. Ayrıca, bu depolama konumunun şifrelendiğinden emin olun.

### <a name="invoke-azurestackondemandlog"></a>Invoke-Azurestackonisteğgünlüğü

Belirli roller için isteğe bağlı Günlükler oluşturmak üzere **Invoke-Azurestackondıonlog** cmdlet 'ini kullanabilirsiniz (Bu bölümün sonundaki listeye bakın). Bu cmdlet tarafından oluşturulan Günlükler, **Get-AzureStackLog** cmdlet 'ini çalıştırdığınızda aldığınız günlük grubundaki varsayılan olarak mevcut değildir. Ayrıca, bu günlükleri yalnızca Microsoft destek ekibi tarafından istenerek toplamanız önerilir.

Şu anda, aşağıdaki rollere sahip `-FilterByRole` günlük toplamayı filtrelemek için parametresini kullanabilirsiniz:

* OEM
* KSK
* SLB
* Ağ geçidi

#### <a name="example-of-collecting-on-demand-logs"></a>İsteğe bağlı günlükleri toplama örneği

```powershell
$ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

$password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)

$shareCred = Get-Credential

$session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2) # Provide the time that includes the period for your issue

Invoke-Command -Session $session {
   Invoke-AzureStackOnDemandLog -Generate -FilterByRole "<on-demand role name>" # Provide the supported on-demand role name e.g. OEM, NC, SLB, Gateway
   Get-AzureStackLog -OutputSharePath "<external share address>" -OutputShareCredential $using:shareCred -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate
}

if ($session) {
   Remove-PSSession -Session $session
}
```

