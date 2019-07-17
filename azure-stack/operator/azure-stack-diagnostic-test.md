---
title: Azure Stack doğrulama aracını | Microsoft Docs
description: Azure Stack'te tanılama günlük dosyaları toplamak nasıl.
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
ms.openlocfilehash: a582e1d9abbd690a62f27f6bcaee8c2dd2e6be4b
ms.sourcegitcommit: 90ed5aa051d0756b2432c8aca0e2232c8ec493a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68239459"
---
# <a name="validate-azure-stack-system-state"></a>Azure Stack sistem durumu doğrulama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak isteğe bağlı olarak sisteminizin durumunu saptayabilmeniz çok önemlidir. Azure Stack doğrulama aracını (**Test AzureStack**) olanak tanıyan bir PowerShell cmdlet'i bir dizi test hataları tanımlamak için sisteminizde çalıştırılır. Bir sorunla ilgili olarak Microsoft Müşteri Hizmetleri Desteği'ne (CSS) başvurduğunuzda normalde [ayrıcalıklı uç nokta (PEP)](azure-stack-privileged-endpoint.md) üzerinden bu aracı çalıştırmanız istenir. Sistem genelinde durum bilgileri elinizde olduğunda CSS ayrıntılı günlükleri toplayabilir ve analiz edebilir, hatanın oluştuğu alana odaklanabilir ve sizinle birlikte çalışarak sorunu çözebilir.

## <a name="running-the-validation-tool-and-accessing-results"></a>Doğrulama aracını çalıştırma ve sonuçları erişme

Daha önce belirtildiği gibi doğrulama aracını CESARETLENDİRİCİ çalıştırılır. Her test döndürür bir **GEÇER/başarısız** durumu PowerShell penceresinde. Aşağıda, test etme işleminin uçtan uca doğrulama özetini verilmiştir: 

1. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) erişin. CESARETLENDİRİCİ oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > ASDK konak bilgisayardaki CESARETLENDİRİCİ erişmek için ERCS01 AzS - ComputerName için kullanın.

2. İçinde CESARETLENDİRİCİ olduktan sonra çalıştırın: 

   ```powershell
   Test-AzureStack
   ```

   Başvurmak [parametresi konuları](azure-stack-diagnostic-test.md#parameter-considerations) ve [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümlerde daha fazla bilgi için.

3. Aşağıdakilerden sınamaktadır rapor **başarısız**çalıştırın `Get-AzureStackLog`. Tümleşik bir sistem hakkında yönergeler için bkz. [tümleştirilmiş sistemlerle, Get-AzureStackLog Azure Stack'te çalıştırılacak](azure-stack-diagnostics.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems), veya ASDK üzerinde [Get AzureStackLog çalıştıran bir Azure Stack geliştirme Seti'ni (ASDK) sisteminde](azure-stack-diagnostics.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system).

   Cmdlet Test-AzureStack tarafından oluşturulan günlükleri toplar. Günlükleri toplayın veya gerekir CSS başvurun testleri rapor **UYAR**.

4. CSS tarafından doğrulama aracını çalıştırmak için istendiyse, sorun giderme devam etmek için toplanan günlüklerde CSS temsilcisi ister.

## <a name="tests-available"></a>Testleri kullanılabilir

Doğrulama Aracı, bir dizi sistem düzeyindeki test çalıştırmanıza olanak tanır ve geçerli bir öngörü sağlayan temel bulut senaryolarına durum ve sisteminizdeki sorunları belirlemek.

### <a name="cloud-infrastructure-tests"></a>Bulut altyapısı testleri

Bu düşük etki testleri, bir altyapı düzeyinde çalışmak ve çeşitli sistem bileşenleri ve işlevleri hakkında bilgi sağlar. Şu anda testleri aşağıdaki kategorilere ayrılır:

| Test kategorisi                                        | Bağımsız değişken-içerir ve - yoksay |
| :--------------------------------------------------- | :-------------------------------- |
| Azure Stack ACS özeti                              | AzsAcsSummary                     |
| Azure Stack Active Directory özeti                 | AzsAdSummary                      |
| Azure Stack uyarı özeti                            | AzsAlertSummary                   |
| Azure Stack uygulama kilitlenme özeti                | AzsApplicationCrashSummary        |
| Azure Stack yedekleme paylaşımına erişilebilirlik özeti       | AzsBackupShareAccessibility       |
| Azure Stack BMC özeti                              | AzsStampBMCSummary                |
| Azure Stack bulut barındırma altyapısını özeti     | AzsHostingInfraSummary            |
| Azure Stack bulut barındırma altyapısını kullanımı | AzsHostingInfraUtilization        |
| Azure Stack denetim düzlemi özeti                    | AzsControlPlane                   |
| Azure Stack Defender özeti                         | AzsDefenderSummary                |
| Azure Stack altyapısını bellenim özeti barındırma  | AzsHostingInfraFWSummary          |
| Azure Stack altyapısını kapasite                  | AzsInfraCapacity                  |
| Azure Stack altyapısı performansı               | AzsInfraPerformance               |
| Azure Stack altyapısını Rol Özeti              | AzsInfraRoleSummary               |
| Azure Stack Portal ve API özeti                   | AzsPortalAPISummary               |
| Azure Stack ölçek birimi VM olayları                     | AzsScaleUnitEvents                |
| Azure Stack ölçek birimi VM kaynakları                  | AzsScaleUnitResources             |
| Azure Stack senaryoları                                | AzsScenarios                      |
| Azure Stack SDN doğrulama özeti                   | AzsSDNValidation                  |
| Azure Stack Service Fabric Rol Özeti              | AzsSFRoleSummary                  |
| Azure Stack depolama veri düzlemi                       | AzsStorageDataPlane               |
| Azure Stack depolama hizmetler özeti                 | AzsStorageSvcsSummary             |
| Azure Stack SQL Store özeti                        | AzsStoreSummary                   |
| Azure Stack güncelleştirme özeti                           | AzsInfraUpdateSummary             |
| Azure Stack VM yerleştirme özeti                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Bulut senaryosu testleri

Yukarıdaki altyapı testleri yanı sıra ayrıca altyapı bileşenlerinde işlevselliği kontrol etmek için bulut senaryosu testleri çalıştırma olanağı vardır. Bulut Yöneticisi kimlik bilgileri, kaynak dağıtımını ilgili olarak bu testleri çalıştırmak için gereklidir.

> [!NOTE]
> Şu anda Active Directory Federasyon Hizmetleri'nde (AD FS) kimlik bilgilerini kullanarak bulut senaryosu testleri çalıştıramazsınız. 

Aşağıdaki bulut senaryoları doğrulama aracı tarafından test edilen:
- Kaynak grubu oluşturma   
- Plan oluşturma              
- Teklif oluşturma            
- Depolama hesabı oluşturma   
- Sanal makine oluşturma 
- BLOB depolama işlemi   
- Kuyruk depolama işlemi  
- Tablo depolama işlemi  

## <a name="parameter-considerations"></a>Parametre konuları

- Parametre **listesi** tüm kullanılabilir test kategorileri görüntülemek için kullanılabilir.

- Parametreleri **INCLUDE** ve **Yoksay** dahil edilecek veya hariç test kategorileri için kullanılabilir. Bu bağımsız değişkenlerle kullanılacak bilgiler hakkında daha fazla bilgi için aşağıdaki bölüme bakın.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- VM bir parçası dağıtılan bir kiracı, bulut senaryosu sınar. Kullanabileceğiniz **DoNotDeployTenantVm** bu devre dışı bırakmak için.

- Sağlamanız gereken **ServiceAdminCredential** açıklandığı gibi bulut senaryosu testleri çalıştırmak için parametre [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümü.

- **BackupSharePath** ve **BackupShareCredential** altyapısını Yedekleme ayarlarını gösterildiği gibi test ederken kullanılan [büyük örneklerde](azure-stack-diagnostic-test.md#use-case-examples) bölümü.

- **DetailedResults** her test, yanı sıra genel çalıştırma başarılı/başarısız/uyarı bilgilerini almak için kullanılabilir. Belirtilmediğinde, **Test AzureStack** döndürür **$true** herhangi bir hata varsa ve **$false** hata varsa.
- **TimeoutSeconds** tamamlamak, her grup için belirli bir süre için kullanılabilir.

- Doğrulama Aracı Ayrıca ortak PowerShell parametrelerini destekler: Ayrıntılı, hata ayıklama, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable ve OutVariable. Daha fazla bilgi için [ortak parametreleri hakkında](https://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Büyük/küçük harf örneklerini kullanın

### <a name="run-validation-without-cloud-scenarios"></a>Doğrulama bulut senaryoları olmadan çalıştırın

Olmadan doğrulama aracını çalıştırma **ServiceAdminCredential** bulut senaryosu testleri atlamak için parametre: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Doğrulama ile bulut senaryoları çalıştırın

Doğrulama Aracı ile sağlama **ServiceAdminCredentials** parametre varsayılan olarak bulut senaryosu testler çalıştırır: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

YALNIZCA bulut senaryolarına kalan testler çalıştırmadan çalıştırmak istiyorsanız, kullanabileceğiniz **INCLUDE** Bunu yapmak için parametre: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

Bulut yönetici kullanıcı adı UPN biçiminde yazılmalıdır: serviceadmin@contoso.onmicrosoft.com (Azure AD). İstendiğinde, bulut yönetici hesabının parolasını yazın.

### <a name="groups"></a>Gruplar

Operatör deneyimi geliştirmek için bir **grubu** parametresi, aynı anda birden çok test kategorisi çalıştırmak için etkinleştirildi. Şu anda tanımlanmış 3 grubu vardır: **Varsayılan**, **UpdateReadiness** ve **SecretRotationReadiness**.

- **Varsayılan**: Standart çalıştırma kabul **Test AzureStack**. Diğer bir grup yok seçildiğinde, bu grup varsayılan olarak çalıştırılır.
- **UpdateReadiness**: Damga güncelleştirilebilir olup olmadığını görmek için denetleyin. Zaman **UpdateReadiness** grubu çalıştırın, uyarıları konsol çıkışında hata olarak görüntülenir ve engelleyicileri güncelleştirmesi olarak düşünülmelidir. Aşağıdaki kategorilerde parçası olan **UpdateReadiness** Grup:

  - **AzsAcsSummary**
  - **AzsDefenderSummary**
  - **AzsHostingInfraSummary**
  - **AzsInfraCapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **AzsSFRoleSummary**
  - **AzsStoreSummary**

- **SecretRotationReadiness**: Damga içinde olup olmadığını görmek için bir onay bir gizli dizi hangi döndürme Çalıştır. Zaman **SecretRotationReadiness** grubu çalıştırın, uyarıları konsol çıkışında hataları olarak görüntülenir ve engelleyicileri gizli döndürme için olarak düşünülmelidir. Aşağıdaki kategorilerde SecretRotationReadiness grubun bir parçasıdır:

  - **AzsAcsSummary**
  - **AzsDefenderSummary**
  - **AzsHostingInfraSummary**
  - **AzsInfraCapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **AzsSFRoleSummary**
  - **AzsStorageSvcsSummary**
  - **AzsStoreSummary**

#### <a name="group-parameter-example"></a>Grubu parametre örneği

Aşağıdaki örnek çalıştıran **Test AzureStack** bir güncelleştirme veya düzeltme kullanarak yüklemeden önce sistem hazırlığı test etmek için **grubu**. Bir güncelleştirme veya düzeltme yüklemesi başlamadan önce çalıştırmalısınız **Test AzureStack** Azure Stack durumunu denetlemek için:

```powershell
Test-AzureStack -Group UpdateReadiness
```

Azure Stack 1811 aşağıda bir sürümünü çalıştırıyorsa, ancak aşağıdaki PowerShell komutlarını çalıştırmak için kullanın **Test AzureStack**:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Altyapı Yedekleme ayarlarını test etmek için doğrulama aracını çalıştırma

*Önce* altyapı yedeklemeyi yapılandırma, yedekleme paylaşım yolu test edebilir ve kullanarak kimlik bilgisi **AzsBackupShareAccessibility** test: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential $using:backupcred
  ```

*Sonra* çalıştırabileceğiniz yedeklemeyi yapılandırma, **AzsBackupShareAccessibility** paylaşım doğrulamak için ERCS erişilebilir:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Yeni kimlik bilgileriyle yapılandırılmış yedekleme paylaşımına test etmek için çalıştırın: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Sonraki adımlar

Azure Stack'te tanılama araçları ve sorunu günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Azure Stack'te tanılama araçları](azure-stack-diagnostics.md).

Sorun giderme hakkında daha fazla bilgi için bkz: [Microsoft Azure Stack sorunlarını giderme](azure-stack-troubleshooting.md).
