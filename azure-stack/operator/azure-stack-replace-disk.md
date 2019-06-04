---
title: Azure stack'teki fiziksel disk değiştirme | Microsoft Docs
description: Azure Stack fiziksel diskleri değiştirme işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: bfe18e0aa59f81f614ae00057b2c1f287b1257da
ms.sourcegitcommit: cf9440cd2c76cc6a45b89aeead7b02a681c4628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66469311"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Azure stack'teki fiziksel disk değiştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack'te fiziksel diski değiştirmek için genel süreç açıklanır. Bir fiziksel disk başarısız olursa, olabildiğince çabuk değiştirmelisiniz.

Tümleşik sistemler ve kullanılan diskleri olan Geliştirme Seti dağıtımlar için bu yordamı kullanabilirsiniz.

Gerçek disk değiştirme adımları farklılık gösterir, orijinal ekipman üreticisi (OEM) donanım satıcınıza temel. Sisteme özgü ayrıntılı adımlar için satıcınızın alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerine bakın.

## <a name="review-disk-alert-information"></a>Disk uyarı bilgileri gözden geçirin
Bir disk başarısız olduğunda, bir fiziksel disk için bağlantı kaybedildi bildiren bir uyarı alırsınız.

![Fiziksel diske uyarı gösteren bağlantısı kesilmiş](media/azure-stack-replace-disk/DiskAlert.png)

Uyarı açarsanız, uyarı açıklaması ölçek birimi düğüm ve tam fiziksel yuvası konumunu değiştirmeniz gereken diski içerir. Daha fazla Azure Stack LED göstergesi özelliklerini kullanarak hatalı bir diski tanımlamanıza yardımcı olur.

## <a name="replace-the-disk"></a>Disk değiştirme

Gerçek disk değiştirme OEM donanım satıcınızın FRU yönergelerini izleyin.

> [!note]
> Bir ölçek birimi düğümü disklerinde aynı anda değiştirin. Sanal disk onarma işleri sonraki ölçek birimi düğüme geçmeden önce tamamlanması bekle

Tümleşik bir sistemde desteklenmeyen bir disk kullanımını önlemek için sistemi satıcınız tarafından desteklenmeyen diskleri engeller. Desteklenmeyen bir disk kullanmayı denerseniz, bir diski bir desteklenmeyen model veya üretici yazılımı nedeniyle karantinaya alındı, yeni bir uyarı bildirir.

Disk değiştirdikten sonra Azure Stack, otomatik olarak yeni disk bulur ve sanal disk onarım işlemini başlatır.

## <a name="check-the-status-of-virtual-disk-repair-using-azure-stack-powershell"></a>Azure Stack PowerShell kullanarak sanal disk onarma durumunu denetleyin

Disk değiştirdikten sonra Azure Stack PowerShell kullanarak sanal diski sistem durumu ve onarım iş ilerleme durumunu izleyebilirsiniz.

1. Azure Stack PowerShell'in yüklü olup olmadığını denetleyin. Daha fazla bilgi için [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
2. PowerShell ile Azure Stack operatör bağlanın. Daha fazla bilgi için [Azure Stack operatör olarak PowerShell ile bağlanma](azure-stack-powershell-configure-admin.md).
3. Sanal disk sistem durumunu doğrulayın ve durumu onarmak için aşağıdaki cmdlet'leri çalıştırın:
    ```powershell  
    $scaleunit=Get-AzsScaleUnit
    $StorageSubSystem=Get-AzsStorageSubSystem -ScaleUnit $scaleunit.Name
    Get-AzsVolume -StorageSubSystem $StorageSubSystem.Name -ScaleUnit $scaleunit.name | Select-Object VolumeLabel, OperationalStatus, RepairStatus
    ```

    ![Azure Stack birimleri sistem durumu](media/azure-stack-replace-disk/get-azure-stack-volumes-health.png)

4. Azure Stack sistem durumunu doğrulayın. Yönergeler için [Azure Stack doğrulama sistem durumu](azure-stack-diagnostic-test.md).
5. İsteğe bağlı olarak, değiştirilen fiziksel disk durumunu doğrulamak için aşağıdaki komutu çalıştırabilirsiniz.

```powershell  
$scaleunit=Get-AzsScaleUnit
$StorageSubSystem=Get-AzsStorageSubSystem -ScaleUnit $scaleunit.Name

Get-AzsDrive -StorageSubSystem $StorageSubSystem.Name -ScaleUnit $scaleunit.name | Format-Table Storagenode, Healthstatus, PhysicalLocation, Model, MediaType,  CapacityGB, CanPool, CannotPoolReason
```

![Azure Stack fiziksel diskler değiştirildi](media/azure-stack-replace-disk/get-azure-stack-volumes-health.png)

## <a name="check-the-status-of-virtual-disk-repair-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanarak sanal disk onarma durumunu denetleyin
 
Disk değiştirdikten sonra sanal disk sistem durumunu izleyebilir ve ayrıcalıklı uç nokta kullanarak işin ilerleme durumunu onarın. Ayrıcalıklı uç noktasına ağ bağlantısı olan herhangi bir bilgisayardan aşağıdaki adımları izleyin.

1. Bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.
    ```powershell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ``` 
  
2. Sanal disk durumunu görüntülemek için aşağıdaki komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster
    ```
   ![PowerShell Get-VirtualDisk komutunun çıktısı](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Geçerli depolama iş durumunu görüntülemek için aşağıdaki komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ```
      ![PowerShell Get-StorageJob komutunun çıktısı](media/azure-stack-replace-disk/GetStorageJobOutput.png)

4. Azure Stack sistem durumunu doğrulayın. Yönergeler için [Azure Stack doğrulama sistem durumu](azure-stack-diagnostic-test.md).


## <a name="troubleshoot-virtual-disk-repair-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanarak sanal disk onarım sorunlarını giderme

Sanal disk onarma, işi takılan, olarak gösterilir ve işi yeniden başlatmak için aşağıdaki komutu çalıştırın:
  ```powershell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ``` 
