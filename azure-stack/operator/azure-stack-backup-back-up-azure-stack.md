---
title: Yedekleme Azure Stack | Microsoft Docs
description: Azure Stack üzerinde isteğe bağlı yedekleme yapmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 01a4ff62b7cc340a0cf0f98298ee28425d6df892
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974745"
---
# <a name="back-up-azure-stack"></a>Azure yığını yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack bir isteğe bağlı yedeklemenin nasıl yapılacağı gösterilmektedir. PowerShell ortamını yapılandırma hakkında yönergeler için bkz. [Azure Stack Için PowerShell 'ı yükler](azure-stack-powershell-install.md). Azure Stack oturum açmak için, bkz. [Azure Stack 'da Yönetici portalını kullanma](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Azure Stack Yedeklemeyi Başlat

### <a name="start-a-new-backup-without-job-progress-tracking"></a>İş ilerlemesi izleme olmadan yeni bir yedekleme Başlat
İş ilerlemesi izleme olmadan hemen yeni bir yedekleme başlatmak için Start-AzSBackup komutunu kullanın.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>İş ilerleme durumu izleme ile Azure Stack Yedeklemeyi başlatma
**-AsJob** parametresiyle yeni bir yedekleme başlatmak ve yedekleme işinin ilerlemesini izlemek için bir değişken olarak kaydetmek için Start-AzSBackup kullanın.

> [!NOTE]
> Yedekleme işiniz, portalda işlem tamamlanmadan önce 10-15 dakika boyunca başarıyla tamamlandı olarak görünür.
>
> Asıl durum aşağıdaki kodla daha iyi gözlemlenmiştir.

> [!IMPORTANT]
> İlk 1 milisaniyelik gecikme, kod işi doğru bir şekilde kaydetmek için çok hızlı olduğundan ve **Psbegintime** olmadan geri dönüp ve işin **durumu** olmadan geri geldiğinden ortaya çıkarılmıştır.

```powershell
    $BackupJob = Start-AzsBackup -Force -AsJob
    While (!$BackupJob.PSBeginTime) {
        Start-Sleep -Milliseconds 1
    }
    Write-Host "Start time: $($BackupJob.PSBeginTime)"
    While ($BackupJob.State -eq "Running") {
        Write-Host "Job is currently: $($BackupJob.State) - Duration: $((New-TimeSpan -Start ($BackupJob.PSBeginTime) -End (Get-Date)).ToString().Split(".")[0])"
        Start-Sleep -Seconds 30
    }

    If ($BackupJob.State -eq "Completed") {
        Get-AzsBackup | Where-Object {$_.BackupId -eq $BackupJob.Output.BackupId}
        $Duration = $BackupJob.Output.TimeTakenToCreate
        $Pattern = '^P?T?((?<Years>\d+)Y)?((?<Months>\d+)M)?((?<Weeks>\d+)W)?((?<Days>\d+)D)?(T((?<Hours>\d+)H)?((?<Minutes>\d+)M)?((?<Seconds>\d*(\.)?\d*)S)?)$'
        If ($Duration -match $Pattern) {
            If (!$Matches.ContainsKey("Hours")) {
                $Hours = ""
            } 
            Else {
                $Hours = ($Matches.Hours).ToString + 'h '
            }
            $Minutes = ($Matches.Minutes)
            $Seconds = [math]::round(($Matches.Seconds))
            $Runtime = '{0}{1:00}m {2:00}s' -f $Hours, $Minutes, $Seconds
        }
        Write-Host "BackupJob: $($BackupJob.Output.BackupId) - Completed with Status: $($BackupJob.Output.Status) - It took: $($Runtime) to run" -ForegroundColor Green
    }
    ElseIf ($BackupJob.State -ne "Completed") {
        $BackupJob
        $BackupJob.Output
    }
```

## <a name="confirm-backup-has-completed"></a>Yedeklemenin tamamlandığını onaylayın

### <a name="confirm-backup-has-completed-using-powershell"></a>Yedeklemenin PowerShell kullanılarak tamamlandığını onaylayın
Yedeklemenin başarıyla tamamlandığından emin olmak için aşağıdaki PowerShell komutlarını kullanın:

```powershell
   Get-AzsBackup
```

Sonuç aşağıdaki çıktı gibi görünmelidir:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administrator-portal"></a>Yönetici portalında yedeklemenin tamamlandığını onaylayın
Aşağıdaki adımları izleyerek yedeklemenin başarıyla tamamlandığını doğrulamak için Azure Stack Yönetici portalını kullanın:

1. [Azure Stack Yönetici portalı](azure-stack-manage-portals.md)' nı açın.
2. **Tüm hizmetler**' i seçin ve ardından **yönetim** kategorisinin altında > **altyapı yedeklemesi**' ni seçin. **Altyapı yedeklemesi** dikey penceresinde **yapılandırma** ' yı seçin.
3. **Kullanılabilir yedeklemeler** listesinde yedeklemenin **tamamlanma** **adını** ve tarihini bulun.
4. **Durumun** **başarılı**olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

[Veri kaybı olayından kurtarmak](azure-stack-backup-recover-data.md)için iş akışı hakkında daha fazla bilgi edinin.
