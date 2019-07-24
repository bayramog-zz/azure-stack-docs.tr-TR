---
title: Azure Stack’te tanılama
description: Azure Stack 'de Tanılama için günlük dosyalarını toplama
services: azure-stack
author: justinha
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 05/29/2019
ms.author: justinha
ms.reviewer: adshar
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: b37c9599028cef0cc8d85bcd8004c5290604c1a3
ms.sourcegitcommit: 2063332b4d7f98ee944dd1f443847eea70eb5614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303136"
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack tanılama araçları

Azure Stack, birlikte çalışan ve birbirleriyle etkileşim kuran büyük bir bileşen koleksiyonudur. Tüm bu bileşenler kendi benzersiz günlüklerini oluşturur. Bu durum, özellikle birden çok, etkileşim Azure Stack bileşenlerinden gelen hatalar için, özellikle de zorlayıcı bir görev için sorunları tanılamayı kolaylaştırabilir.

Tanılama araçlarımız, günlük toplama mekanizmalarının kolay ve verimli olmasını sağlamaya yardımcı olur. Aşağıdaki diyagramda Azure Stack günlük toplama araçlarının nasıl çalıştığı gösterilmektedir:

![Azure Stack tanılama araçları](media/azure-stack-diagnostics/get-azslogs.png)

## <a name="trace-collector"></a>İzleme Toplayıcısı

İzleme Toplayıcısı varsayılan olarak etkindir ve Azure Stack bileşen hizmetlerinden Windows için olay Izleme (ETW) günlüklerini toplamak üzere arka planda sürekli olarak çalışır. ETW günlükleri, beş günlük yaş sınırına sahip ortak bir yerel paylaşımda depolanır. Bu sınıra ulaşıldığında, yeni olanlar oluşturulmuşdan en eski dosyalar silinir. Her dosya için izin verilen varsayılan en büyük boyut 200 MB 'tır. Boyut denetimi her 2 dakikada bir gerçekleşir ve geçerli dosya > = 200 MB ise, kaydedilir ve yeni bir dosya oluşturulur. Ayrıca, olay oturumu başına oluşturulan toplam dosya boyutu için 8 GB bir sınır vardır.

## <a name="log-collection-tool"></a>Günlük toplama aracı

**Get-AzureStackLog** PowerShell cmdlet 'i, bir Azure Stack ortamındaki tüm bileşenlerin günlüklerini toplamak için kullanılabilir. Bunları, Kullanıcı tanımlı bir konumdaki ZIP dosyalarına kaydeder. Azure Stack teknik destek ekibinin bir sorunu gidermeye yardımcı olmak için günlüklerinizi ihtiyacı varsa, bu aracı çalıştırmanızı isteyebilir.

> [!CAUTION]
> Bu günlük dosyaları, kişisel olarak tanımlanabilir bilgiler (PII) içerebilir. Herhangi bir günlük dosyasını genel olarak göndermeden önce bunu hesaba gönderin.

Aşağıda, toplanan bazı örnek günlük türleri verilmiştir:

* **Azure Stack dağıtım günlükleri**
* **Windows olay günlükleri**
* **Panther günlükleri**
* **Küme günlükleri**
* **Depolama tanılama günlükleri**
* **ETW günlükleri**

Bu dosyalar, İzleme Toplayıcısı bir paylaşımında toplanır ve kaydedilir. **Get-AzureStackLog** PowerShell cmdlet 'i, gerektiğinde bunları toplamak için kullanılabilir.

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemlerde Get-AzureStackLog komutunu çalıştırmak için

Günlük toplama aracını tümleşik bir sistemde çalıştırmak için ayrıcalıklı uç noktaya (PEP) erişiminizin olması gerekir. Aşağıda, tümleşik bir sistemde günlükleri toplamak için PEP kullanarak çalıştırabileceğiniz örnek bir betik verilmiştir:

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

### <a name="run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Azure Stack Geliştirme Seti (ASDK) sisteminde Get-AzureStackLog komutunu çalıştırma

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
  > Bu yordam, Microsoft Desteği bir servis talebi açtığınızda ve günlükleri karşıya yüklemek isteyip istemediğiniz durumlarda faydalıdır. ERCS sanal makineden erişilebilen bir SMB paylaşımınız olmasa da, ERCS sanal makinenizin internet erişimi yoksa, günlükleri aktarmak için Azure Stack bir BLOB depolama hesabı oluşturabilir ve ardından bu günlükleri almak ve Microsoft 'a yüklemek için istemcinizi kullanabilirsiniz.  

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

  |   |   |   |    |
  | - | - | - | -  |
  |ACS                   |CA                             |HRP                            |OboService|
  |ACSBlob               |CacheService                   |IBC                            |OEM|
  |ACSDownloadService    |İşlem                        |InfraServiceController         |OnboardRP|
  |ACSFabric             |CPI                            |KeyVaultAdminResourceProvider  |PXE|
  |ACSFrontEnd           |CRP                            |Keyvaultcontroldüzlemi           |QueryServiceCoordinator|
  |Acsölçümler            |DeploymentMachine              |Keyvaultdatadüzlemi              |QueryServiceWorker|
  |ACSMigrationService   |DiskRP                         |Keyvaultınternalcontroldüzlemi   |SeedRing|
  |ACSMonitoringService  |Etki Alanı                         |Keyvaultınternaldatadüzlemi      |SeedRingServices|
  |ACSSettingsService    |ECE                            |KeyVaultNamingService          |SLB|
  |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL|
  |ACSTableServer        |EventRP                        |MetricsAdminRP                 |'DE   |
  |ACSWac                |ExternalDNS                    |MetricsRP                      |Depolama|
  |ADFS                  |FabricRing                     |MetricsServer                  |StorageController   |
  |ApplicationController |FabricRingServices             |MetricsStoreService            |URP   |
  |Aşama Ppgateway          |FirstTierAggregationService    |MonAdminRP                     |UsageBridge|
  |AzureBridge           |FRP                            |MonRP                          |VirtualMachines   |
  |AzureMonitor          |Ağ geçidi                        |KSK                             |BULUNAMADI|
  |BareMetal             |'De               |NonPrivilegedAppGateway        |WASPUBLIK|
  |BRP                   |HintingServiceV2               |NRP                            |   |
  |   |   |   |    |

### <a name="additional-considerations"></a>Diğer konular

* Komutun hangi rol (ler) topladıkları role göre çalıştırılması biraz zaman alır. Ayrıca, katkıda bulunan faktörler günlük toplama için belirtilen süre ve Azure Stack ortamındaki düğümlerin sayısı da dahil değildir.
* Günlük koleksiyonu çalışırken, komutta belirtilen **Outputsharepath** parametresinde oluşturulan yeni klasörü kontrol edin.
* Her rolün günlüğü ayrı ZIP dosyaları içinde vardır. Toplanan günlüklerin boyutuna bağlı olarak, bir rolün günlükleri birden çok ZIP dosyasına bölünmüş olabilir. Bu tür bir rol için, tüm günlük dosyalarını tek bir klasöre geri yüklemek istiyorsanız, toplu olarak (örneğin, 7zip) sıkıştırılmış bir araç kullanın. Rolün tüm daraltılmış dosyalarını seçin ve **buradan Ayıkla**' yı seçin. Bu, tek bir birleştirilmiş klasörde söz konusu rolün tüm günlük dosyalarını kaldırır.
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

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Azure Stack sorunlarını giderme](azure-stack-troubleshooting.md)
