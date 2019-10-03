---
title: Sistem durumunu doğrulamak için Azure Stack doğrulama aracını kullanma | Microsoft Docs
description: Sistem durumunu doğrulamak için Azure Stack doğrulama aracının nasıl kullanılacağını öğrenin.
services: azure-stack
author: justinha
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 06/26/2019
ms.author: justinha
ms.reviewer: adshar
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 194af241480cce42273ff81d91213a63b1b9fd59
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829161"
---
# <a name="validate-azure-stack-system-state"></a>Azure Stack sistem durumunu doğrula

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak, sisteminizin sistem durumunu ve durumunu isteğe bağlı olarak belirleyebilmekte ve gereklidir. Azure Stack doğrulama aracı (**Test-AzureStack**), sisteminizde bulunan hataların belirlenmesi için sisteminizde bir dizi test çalıştırmanızı sağlayan bir PowerShell cmdlet 'i. Genellikle, Microsoft Müşteri Hizmetleri desteği (CSS) ile bir sorunla iletişime geçerek bu aracı [ayrıcalıklı uç noktası (PEP)](azure-stack-privileged-endpoint.md) üzerinden çalıştırmanız istenir. Aynı anda sistem genelinde sistem durumu ve durum bilgileri ile CSS, ayrıntılı günlükleri toplayıp çözümleyebilir, hatanın gerçekleştiği alana odaklanabilir ve sorunu gidermek için sizinle birlikte çalışabilir.

## <a name="running-the-validation-tool-and-accessing-results"></a>Doğrulama aracını çalıştırma ve sonuçlara erişme

Yukarıda belirtildiği gibi, doğrulama aracı PEP aracılığıyla çalıştırılır. Her test, PowerShell penceresinde bir **geçiş/başarısızlık** durumu döndürür. Uçtan uca doğrulama test sürecinin ana hattı aşağıda verilmiştir:

1. PEP 'ye erişin. Bir PEP oturumu oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Bir Azure Stack Geliştirme Seti (ASDK) ana bilgisayarındaki PEP 'ye erişmek için AzS-ERCS01 for-ComputerName kullanın.

2. PEP ' de olduktan sonra şunu çalıştırın:

   ```powershell
   Test-AzureStack
   ```

   Daha fazla bilgi için bkz. [parametre konuları](azure-stack-diagnostic-test.md#parameter-considerations) ve [kullanım örneği örnekleri](azure-stack-diagnostic-test.md#use-case-examples).

3. Herhangi bir test raporu **başarısız**olursa, `Get-AzureStackLog` ' i çalıştırın. Tümleşik bir sistemle ilgili yönergeler için bkz. [Get-azurestacklog Azure Stack tümleşik sistemlerde](azure-stack-configure-on-demand-diagnostic-log-collection.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems)veya asdk 'de çalıştırmak için, bkz. [bir asdk sisteminde Get-azurestacklog çalıştırma](azure-stack-configure-on-demand-diagnostic-log-collection.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system).

   Cmdlet 'i test-AzureStack tarafından oluşturulan günlükleri toplar. Testler **rapor**uyarımız durumunda günlükleri toplamayın ve CSS 'ye başvurmamanızı öneririz.

4. Doğrulama aracını CSS tarafından çalıştırmanız istenirse, CSS temsilcisi sorununuzu gidermeye devam etmek için topladığınız günlükleri ister.

## <a name="tests-available"></a>Kullanılabilir testler

Doğrulama Aracı, size geçerli durum hakkında bilgi sağlayan ve sisteminizdeki sorunları düzeltmenize olanak tanıyan bir dizi sistem düzeyi testi ve temel bulut senaryosunu çalıştırmanızı sağlar.

### <a name="cloud-infrastructure-tests"></a>Bulut altyapısı testleri

Bu düşük etkili testler bir altyapı düzeyinde çalışır ve çeşitli sistem bileşenleri ve işlevleri hakkında bilgi sağlar. Şu anda, testler aşağıdaki kategorilerde gruplandırılır:

| Test kategorisi                                        | -Include ve-Ignore bağımsız değişkeni |
| :--------------------------------------------------- | :-------------------------------- |
| Azure Stack ACS Özeti                              | AzsAcsSummary                     |
| Azure Stack Active Directory Özeti                 | AzsAdSummary                      |
| Azure Stack uyarı Özeti                            | Azsalcertsummary                   |
| Azure Stack uygulama kilitlenme Özeti                | AzsApplicationCrashSummary        |
| Azure Stack yedekleme paylaşımının erişilebilirlik Özeti       | AzsBackupShareAccessibility       |
| Azure Stack BMC Özeti                              | AzsStampBMCSummary                |
| Azure Stack bulut barındırma altyapısı Özeti     | AzsHostingInfraSummary            |
| Azure Stack bulut barındırma altyapısı kullanımı | AzsHostingInfraUtilization        |
| Azure Stack denetim düzlemi Özeti                    | Azscontroldüzlemi                   |
| Azure Stack Defender Özeti                         | Azssavunma Dersummary                |
| Azure Stack barındırma altyapısı üretici yazılımı Özeti  | AzsHostingInfraFWSummary          |
| Azure Stack altyapı kapasitesi                  | Azsınfracapacity                  |
| Altyapı performansını Azure Stack               | Azsınfraperformance               |
| Azure Stack altyapısı rolü Özeti              | AzsInfraRoleSummary               |
| Azure Stack Network Infra                            | AzsNetworkInfra                   |
| Azure Stack Portal ve API Özeti                   | AzsPortalAPISummary               |
| Azure Stack ölçek birimi VM olayları                     | AzsScaleUnitEvents                |
| Azure Stack ölçek birimi VM kaynakları                  | AzsScaleUnitResources             |
| Azure Stack senaryolar                                | Azssenaryolar                      |
| Azure Stack SDN doğrulama özeti                   | AzsSDNValidation                  |
| Azure Stack Service Fabric Rol Özeti              | Azssfrotasummary                  |
| Depolama veri düzlemi Azure Stack                       | Azsstoragedatadüzlemi               |
| Azure Stack Storage Services Özeti                 | AzsStorageSvcsSummary             |
| SQL Mağazası özetini Azure Stack                        | AzsStoreSummary                   |
| Azure Stack güncelleştirme Özeti                           | Azsınfraupdatesummary             |
| Azure Stack VM yerleştirme Özeti                     | Azsvmplaçatma                    |

### <a name="cloud-scenario-tests"></a>Bulut senaryosu testleri

Yukarıdaki altyapı testlerine ek olarak, altyapı bileşenlerinde işlevselliği denetlemek için bulut senaryosu testlerini de çalıştırabilirsiniz. Bulut Yöneticisi kimlik bilgileri, kaynak dağıtımını içerdiğinden bu testlerin çalıştırılması için gereklidir.

> [!NOTE]
> Şu anda Active Directory Federasyon Hizmetleri (AD FS) kimlik bilgilerini kullanarak bulut senaryosu testlerini çalıştıramazsınız.

Aşağıdaki bulut senaryoları doğrulama aracı tarafından test edilmiştir:
- Kaynak grubu oluşturma
- Planı oluşturma
- Teklif oluşturma
- Depolama hesabı oluşturma
- Sanal makine oluşturma (VM)
- BLOB depolama işlemi
- Kuyruk depolama işlemi
- Tablo depolama işlemi

## <a name="parameter-considerations"></a>Parametre konuları

- Parametre **listesi** , tüm kullanılabilir test kategorilerini göstermek için kullanılabilir.

- **Include** ve **Ignore** parametreleri, test kategorilerini dahil etmek veya hariç tutmak için kullanılabilir. Bu bağımsız değişkenler hakkında daha fazla bilgi için aşağıdaki bölüme bakın.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- Bir Kiracı VM 'si, bulut senaryosu testlerinin bir parçası olarak dağıtılır. Bu VM dağıtımını devre dışı bırakmak için **Donotdeploytenantvm** ' i kullanabilirsiniz.

- Bulut senaryosu testlerini [kullanım örneği örnekleri](azure-stack-diagnostic-test.md#use-case-examples) bölümünde açıklandığı gibi çalıştırmak Için **serviceadmincredential** parametresini sağlamanız gerekir.

- **Backupsharepath** ve **Backupsharecredential** , [kullanım örneği örnekleri](azure-stack-diagnostic-test.md#use-case-examples) bölümünde gösterildiği gibi altyapı yedekleme ayarları test edilirken kullanılır.

- **DetailedResults** , her teste yönelik geçiş/başarısız/uyarı bilgilerini ve genel çalıştırmayı almak için kullanılabilir. Belirtilmediğinde, **Test-AzureStack** , bir başarısızlık yoksa **$true** ve başarısızlık varsa **$false** döndürür.
- **TimeoutSeconds** , her grubun tamamlaması için belirli bir zaman ayarlamak üzere kullanılabilir.

- Doğrulama aracı Ayrıca ortak PowerShell parametrelerini de destekler: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable ve OutVariable. Daha fazla bilgi için bkz. [ortak parametreler hakkında](https://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Kullanım örneği örnekleri

### <a name="run-validation-without-cloud-scenarios"></a>Doğrulaması bulut senaryoları olmadan Çalıştır

Çalışan bulut senaryosu testlerini atlamak için, **Serviceadmincredential** parametresi olmadan doğrulama aracını çalıştırın: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Bulut senaryolarıyla doğrulama çalıştırma

**Service Admincredentials** parametresiyle doğrulama aracını sağlamak, bulut senaryosu testlerini varsayılan olarak çalıştırır: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

Yalnızca testlerin geri kalanını çalıştırmadan yalnızca bulut senaryoları çalıştırmak istiyorsanız, bunu yapmak için **Include** parametresini kullanabilirsiniz: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

Bulut Yöneticisi Kullanıcı adının UPN biçiminde yazılması gerekir: serviceadmin@contoso.onmicrosoft.com (Azure AD). İstendiğinde, bulut Yöneticisi hesabının parolasını yazın.

### <a name="groups"></a>Gruplar

İşleç deneyimini geliştirmek için, bir **Grup** parametresi birden çok test kategorisini aynı anda çalıştıracak şekilde etkinleştirildi. Şu anda, tanımlı üç grup vardır: **Varsayılan**, **updatereadsürekliliği**ve **secretrotationreadiness**.

- **Varsayılan**: **Test-AzureStack**'in standart çalıştırması olarak kabul edilir. Bu grup, başka bir grup seçilmezse varsayılan olarak çalıştırılır.
- **Updatereadsürekliliği**: Azure Stack örneğinin güncelleştirilip güncelleştirilmediğini görmek için bir denetim. **Updaterekurban** grubu çalıştırıldığında, uyarılar konsol çıkışında hata olarak görüntülenir ve güncelleştirme için engelleyiciler olarak değerlendirilmelidir. Aşağıdaki kategoriler, **Updatereadılmi** grubunun bir parçasıdır:

  - **AzsAcsSummary**
  - **Azssavunma Dersummary**
  - **AzsHostingInfraSummary**
  - **Azsınfracapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **Azssfrotasummary**
  - **AzsStoreSummary**

- **Secretrotationreadiness**: Azure Stack örneğinin gizli döndürmenin çalıştırılabileceği bir durumda olup olmadığını denetleyin. **Secretrotationreadiness** grubu çalıştırıldığında, uyarılar konsol çıkışında hata olarak görüntülenir ve gizli bir döndürme için engelleyiciler olarak değerlendirilmelidir. Aşağıdaki kategoriler, SecretRotationReadiness grubunun bir parçasıdır:

  - **AzsAcsSummary**
  - **Azssavunma Dersummary**
  - **AzsHostingInfraSummary**
  - **Azsınfracapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **Azssfrotasummary**
  - **AzsStorageSvcsSummary**
  - **AzsStoreSummary**

#### <a name="group-parameter-example"></a>Grup parametresi örneği

Aşağıdaki örnek, **Grup**kullanarak bir güncelleştirmeyi veya düzeltmeyi yüklemeden önce sistemin hazır olduğunu sınamak için **Test-azurestack** ' i çalıştırır. Bir güncelleştirmeyi veya düzeltmeyi yüklemeye başlamadan önce, Azure Stack durumunu denetlemek için **Test-AzureStack** komutunu çalıştırın:

```powershell
Test-AzureStack -Group UpdateReadiness
```

Azure Stack 1811 öncesi bir sürüm çalıştırıyorsa, **Test-AzureStack**çalıştırmak Için aşağıdaki PowerShell komutlarını kullanın:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Altyapı yedekleme ayarlarını test etmek için doğrulama aracını çalıştırma

Altyapı yedeklemesini yapılandırmadan *önce* , **Azsbackupshareaccessibility** testini kullanarak yedekleme paylaşım yolunu ve kimlik bilgilerini test edebilirsiniz:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential $using:backupcred
  ```

Yedeklemeyi yapılandırdıktan *sonra* , bu paylaşımın ercs 'den erişilebilir olduğunu doğrulamak Için **Azsbackupshareaccessibility** komutunu çalıştırabilirsiniz:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Yapılandırılan yedekleme paylaşımıyla yeni kimlik bilgilerini test etmek için şunu çalıştırın: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```

### <a name="run-validation-tool-to-test-network-infrastructure"></a>Ağ altyapısını sınamak için doğrulama aracını çalıştırma

Bu test, Azure Stack yazılım tanımlı ağ (SDN) atlayarak ağ altyapısının bağlantısını denetler. Ortak bir VIP 'den yapılandırılmış DNS ileticilerine, NTP sunucularına ve kimlik doğrulama uç noktalarına olan bağlantıyı gösterir. Bu, kimlik sağlayıcısı olarak AD FS kullanırken Azure AD 'yi kimlik sağlayıcısı veya federasyon sunucusu olarak kullanırken Azure 'a bağlantı içerir.

Komutun ayrıntılı bir çıkışını almak için hata ayıklama parametresini ekleyin:

```powershell
Test-AzureStack -Include AzsNetworkInfra -Debug
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack tanılama araçları ve sorun günlüğü hakkında daha fazla bilgi edinmek için bkz. [Azure Stack tanılama araçları](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs).

Sorun giderme hakkında daha fazla bilgi için bkz. [Microsoft Azure Stack sorun giderme](azure-stack-troubleshooting.md).
