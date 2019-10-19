---
title: Ayrıcalıklı uç noktayı kullanarak Azure Stack güncelleştirmeleri izleme | Microsoft Docs
description: Azure Stack tümleşik sistemlerin güncelleştirme durumunu izlemek için ayrıcalıklı uç noktayı nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: d99a49676f9ab684c5b83e8e68cf58f86efc948f
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534050"
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanarak Azure Stack güncelleştirmeleri izleme

*Uygulama hedefi: Azure Stack tümleşik sistemler*

Azure Stack güncelleştirme çalıştırmasının ilerlemesini izlemek için [ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md) kullanabilirsiniz. Ayrıca, son başarılı adımdan başarısız bir güncelleştirme çalıştırmasını yeniden başlatmak için ayrıcalıklı uç noktasını da kullanabilirsiniz Azure Stack portalı kullanılamaz hale gelir. Azure Stack portalını kullanmak, Azure Stack güncelleştirmeleri yönetmek için önerilen yöntemdir.

Güncelleştirme yönetimi için aşağıdaki yeni PowerShell cmdlet 'leri Azure Stack tümleşik sistemler için 1710 güncelleştirmesine dahildir.

| Cmdlet  | Açıklama  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Şu anda çalışan, tamamlanan veya başarısız güncelleştirme durumunu döndürür. , Güncelleştirme işleminin ve hem geçerli adımı hem de karşılık gelen durumu açıklayan bir XML belgesinin üst düzey durumunu sağlar. |
| `Resume-AzureStackUpdate` | Başarısız olan bir güncelleştirmeyi başarısız olduğu noktada sürdürür. Bazı senaryolarda, güncelleştirmeyi sürdürmeye başlamadan önce risk azaltma adımlarını tamamlamanızı gerekebilir.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Cmdlet 'lerinin kullanılabilir olduğunu doğrulayın
Cmdlet 'ler Azure Stack için 1710 güncelleştirme paketinde yeni olduklarından, 1710 güncelleştirme işleminin izleme yeteneği kullanılabilir olmadan önce belirli bir noktaya sahip olması gerekir. Genellikle, yönetici portalındaki durum 1710 güncelleştirmesinin **depolama Konakları yeniden Başlat** adımında olduğunu gösteriyorsa cmdlet 'ler kullanılabilir. Özellikle, **Adım: 2,6-güncelleştirme PrivilegedEndpoint beyaz listesini çalıştıran adım**sırasında cmdlet güncelleştirmesi oluşur.

Ayrıca, ayrıcalıklı uç noktadan komut listesini sorgulayarak cmdlet 'lerin programlı olarak kullanılabilir olup olmayacağını belirleyebilirsiniz. Bu sorguyu yapmak için, donanım yaşam döngüsü konaktan veya ayrıcalıklı erişim Iş Istasyonundan aşağıdaki komutları çalıştırın. Ayrıca, ayrıcalıklı uç noktanın güvenilen bir konak olduğundan emin olun. Daha fazla bilgi için bkz. [ayrıcalıklı uç noktaya erişim](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)1. adım.

1. Azure Stack ortamınızdaki her bir ERCS sanal makinesi (VM) üzerinde bir PowerShell oturumu oluşturun (*ön ek*-ERCS01, *ön ek*-ERCS02 veya *önek*-ERCS03). *Öneki* , ORTAMıNıZA özgü VM ön eki dizesiyle değiştirin.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   Kimlik bilgileri istendiğinde, &lt;*Azure Stack etki alanı* &gt; \cloudadmin hesabını ya da CloudAdmins grubunun üyesi olan bir hesabı kullanın. CloudAdmin hesabı için, AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında girilen parolayı girin.

2. Ayrıcalıklı uç noktada kullanılabilen komutların tam listesini alın.

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Ayrıcalıklı uç noktanın güncelleştirilip güncelleştirilmediğini belirleme.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Microsoft. AzureStack. UpdateManagement modülüne özgü komutları listeleyin.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Örnek:
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>Güncelleştirme yönetimi cmdlet 'lerini kullanma

> [!NOTE]
> Donanım yaşam döngüsü konaktan veya ayrıcalıklı erişim Iş Istasyonundan aşağıdaki komutları çalıştırın. Ayrıca, ayrıcalıklı uç noktanın güvenilen bir konak olduğundan emin olun. Daha fazla bilgi için bkz. [ayrıcalıklı uç noktaya erişim](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)1. adım.

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Ayrıcalıklı uç noktaya bağlanma ve oturum değişkenini atama

Azure Stack ortamınızdaki ERCS sanal makinelerinden (*ön ek*-ERCS01, *ön*ek-ERCS02 veya *ön ek*-ERCS03) bir PowerShell oturumu oluşturmak ve bir oturum değişkeni atamak için aşağıdaki komutları çalıştırın.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 Kimlik bilgileri istendiğinde, &lt;*Azure Stack etki alanı* &gt; \cloudadmin hesabını ya da CloudAdmins grubunun üyesi olan bir hesabı kullanın. CloudAdmin hesabı için, AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında girilen parolayı girin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Geçerli güncelleştirme çalıştırmasının üst düzey durumunu al

Geçerli güncelleştirme çalıştırmasının üst düzey durumunu almak için aşağıdaki komutları çalıştırın:

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Olası değerler şunlardır:

- Çalışıyor
- Tamamlandı
- Başarısız 
- İptal edildi

En güncel durumu görmek için, bu komutları tekrar tekrar çalıştırabilirsiniz. Yeniden denetlemek için bir bağlantıyı yeniden oluşturmanız gerekmez.

### <a name="get-the-full-update-run-status-with-details"></a>Ayrıntıları içeren tam güncelleştirme çalıştırması durumunu alın

Tam güncelleştirme çalıştırması özetini bir XML dizesi olarak alabilirsiniz. İnceleme için bir dosyaya dize yazabilir veya bir XML belgesine dönüştürebilir ve PowerShell kullanarak bunu ayrıştırmaya ekleyebilirsiniz. Aşağıdaki komut, şu anda çalışan adımların hiyerarşik bir listesini almak için XML 'i ayrıştırır:

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

Aşağıdaki örnekte, üst düzey adım (bulut güncelleştirmesi), depolama konaklarınızı güncelleştirmek ve yeniden başlatmak için bir alt plana sahiptir. Yeniden başlatma depolama Konakları planının, konaklardan birindeki blob Storage hizmetini güncelleştirdiğinden emin olur.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']") 

    FullStepIndex : 2
    Index         : 2
    Name          : Cloud Update
    Description   : Perform cloud update.
    StartTimeUtc  : 2017-10-13T12:50:39.9020351Z
    Status        : InProgress
    Task          : Task
    
    FullStepIndex  : 2.9
    Index          : 9
    Name           : Restart Storage Hosts
    Description    : Restart Storage Hosts.
    EceErrorAction : Stop
    StartTimeUtc   : 2017-10-13T15:44:06.7431447Z
    Status         : InProgress
    Task           : Task
    
    FullStepIndex : 2.9.2
    Index         : 2
    Name          : PreUpdate ACS Blob Service
    Description   : Check function level, update deployment artifacts, configure Blob service settings
    StartTimeUtc  : 2017-10-13T15:44:26.0708525Z
    Status        : InProgress
    Task          : Task
```

### <a name="resume-a-failed-update-operation"></a>Başarısız bir güncelleştirme işlemini sürdürür

Güncelleştirme başarısız olursa, güncelleştirme çalıştırmasını kaldığınız yerden sürdürebilirsiniz.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Sorun giderme

Ayrıcalıklı uç nokta, Azure Stack ortamındaki tüm ERCS sanal makinelerinde kullanılabilir. Bağlantı, yüksek oranda kullanılabilir bir uç noktada yapılmadığından, zaman zaman kesintiler, uyarı veya hata iletileriyle karşılaşabilirsiniz. Bu iletiler, oturumun bağlantısının kesileceğini veya ECE hizmetiyle iletişim kurulurken bir hata olduğunu gösteriyor olabilir. Bu beklenen bir davranıştır. İşlemi birkaç dakika içinde yeniden deneyebilir veya diğer ERCS sanal makinelerinden birinde yeni bir ayrıcalıklı uç nokta oturumu oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmeleri yönetme](azure-stack-updates.md)


