---
title: Azure Stack bir fiziksel diski değiştirme | Microsoft Docs
description: Azure Stack bir fiziksel diskin nasıl değiştirileceği hakkında bir işlem özetlenmektedir.
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
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 5da479853487dfd93467bd1413159d6e602b93c6
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277661"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Azure Stack bir fiziksel diski değiştirme

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack bir fiziksel diskin yerini alacak genel işlem açıklanır. Fiziksel bir disk başarısız olursa, bunu en kısa sürede değiştirmelisiniz.

Bu yordamı tümleşik sistemler ve dinamik olarak değiştirilebilen diskler içeren geliştirme seti dağıtımları için kullanabilirsiniz.

Gerçek disk değiştirme adımları, özgün ekipman üreticisi (OEM) donanım satıcınıza göre değişir. Sisteminize özgü ayrıntılı adımlar için satıcınızın alan değiştirilebilir birimi (FRU) belgelerine bakın.

## <a name="review-disk-alert-information"></a>Disk uyarı bilgilerini gözden geçirme
Bir disk başarısız olduğunda, bağlantının fiziksel diske kaybolduğunu bildiren bir uyarı alırsınız.

![Fiziksel diske bağlantı kaybı gösteren uyarı](media/azure-stack-replace-disk/DiskAlert.png)

Uyarıyı açarsanız, uyarı açıklaması, değiştirmeniz gereken disk için ölçek birimi düğümünü ve tam fiziksel yuva konumunu içerir. Azure Stack, LED gösterge yeteneklerini kullanarak başarısız diski belirlemenize yardımcı olur.

## <a name="replace-the-disk"></a>Diski değiştirme

Gerçek disk değişikliği için OEM Donanım satıcınızın FRU yönergelerini izleyin.

> [!note]
> Tek seferde bir ölçek birimi düğümü için diskleri değiştirin. Sonraki ölçek birimi düğümüne geçmeden önce sanal disk onarım işlerinin tamamlanmasını bekleyin

Tümleşik bir sistemde desteklenmeyen bir diskin kullanılmasını engellemek için, sistem, satıcınız tarafından desteklenmeyen diskleri engeller. Desteklenmeyen bir disk kullanmaya çalışırsanız, yeni bir uyarı, desteklenmeyen bir model veya üretici yazılımı nedeniyle bir diskin karantinaya alındığını söyler.

Diski değiştirdikten sonra, Azure Stack otomatik olarak yeni diski bulur ve sanal disk onarım işlemini başlatır.

## <a name="check-the-status-of-virtual-disk-repair-using-azure-stack-powershell"></a>Azure Stack PowerShell kullanarak sanal disk onarımı durumunu denetleme

Diski değiştirdikten sonra, sanal disk sistem durumunu izleyebilir ve Azure Stack PowerShell kullanarak işin ilerlemesini onarabilirsiniz.

1. Azure Stack PowerShell 'in yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure Stack Için PowerShell 'ı Install](azure-stack-powershell-install.md).
2. PowerShell ile Azure Stack bir operatör olarak bağlanın. Daha fazla bilgi için bkz. [PowerShell ile Azure Stack 'ye bir operatör olarak bağlanma](azure-stack-powershell-configure-admin.md).
3. Sanal disk durumunu ve onarım durumunu doğrulamak için aşağıdaki cmdlet 'leri çalıştırın:
    ```powershell  
    $scaleunit=Get-AzsScaleUnit
    $StorageSubSystem=Get-AzsStorageSubSystem -ScaleUnit $scaleunit.Name
    Get-AzsVolume -StorageSubSystem $StorageSubSystem.Name -ScaleUnit $scaleunit.name | Select-Object VolumeLabel, OperationalStatus, RepairStatus
    ```

    ![Azure Stack birimleri sistem durumu](media/azure-stack-replace-disk/get-azure-stack-volumes-health.png)

4. Sistem durumunu Azure Stack doğrulayın. Yönergeler için bkz. [sistem durumunu doğrulama Azure Stack](azure-stack-diagnostic-test.md).
5. İsteğe bağlı olarak, değişen fiziksel diskin durumunu doğrulamak için aşağıdaki komutu çalıştırabilirsiniz.

```powershell  
$scaleunit=Get-AzsScaleUnit
$StorageSubSystem=Get-AzsStorageSubSystem -ScaleUnit $scaleunit.Name

Get-AzsDrive -StorageSubSystem $StorageSubSystem.Name -ScaleUnit $scaleunit.name | Sort-Object StorageNode,MediaType,PhysicalLocation | Format-Table Storagenode, Healthstatus, PhysicalLocation, Model, MediaType,  CapacityGB, CanPool, CannotPoolReason
```

![Azure Stack fiziksel diskler değiştirilmiş](media/azure-stack-replace-disk/check-replaced-physical-disks-azure-stack.png)

## <a name="check-the-status-of-virtual-disk-repair-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanarak sanal disk onarımı durumunu denetleme
 
Diski değiştirdikten sonra, sanal disk sistem durumunu izleyebilir ve ayrıcalıklı uç noktayı kullanarak işin ilerlemesini onarabilirsiniz. Ayrıcalıklı uç noktaya ağ bağlantısı olan herhangi bir bilgisayardan bu adımları izleyin.

1. Bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktaya bağlanın.
    ```powershell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ``` 
  
2. Sanal disk sistem durumunu görüntülemek için aşağıdaki komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster
    ```
   ![Get-VirtualDisk komutunun PowerShell çıkışı](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Geçerli depolama işinin durumunu görüntülemek için şu komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ```
      ![Get-StorageJob komutunun PowerShell çıkışı](media/azure-stack-replace-disk/GetStorageJobOutput.png)

4. Azure Stack sistem durumunu doğrulayın. Yönergeler için bkz. [sistem durumunu doğrulama Azure Stack](azure-stack-diagnostic-test.md).


## <a name="troubleshoot-virtual-disk-repair-using-the-privileged-endpoint"></a>Ayrıcalıklı uç nokta kullanılarak sanal disk onarımı sorunlarını giderme

Sanal disk onarımı işi takılı görünüyorsa, işi yeniden başlatmak için aşağıdaki komutu çalıştırın:
  ```powershell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ``` 
