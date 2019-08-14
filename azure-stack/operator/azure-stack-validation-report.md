---
title: Azure Stack için doğrulama raporu | Microsoft Docs
description: Doğrulama sonuçlarını gözden geçirmek için Azure Stack hazırlık denetleyicisi raporunu kullanın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: c00ce005ac72fcde34b58a1afe7e134c27274247
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991733"
---
# <a name="azure-stack-validation-report"></a>Azure Stack doğrulama raporu

Bir Azure Stack ortamının dağıtımını ve bakımını destekleyen doğrulamaları çalıştırmak için *Azure Stack hazırlık denetleyicisi* aracını kullanın. Araç sonuçları bir. JSON rapor dosyasına yazar. Rapor, Azure Stack dağıtımına yönelik önkoşulların durumu hakkında ayrıntılı ve özetlenmiş verileri görüntüler. Raporda ayrıca mevcut Azure Stack dağıtımları için gizli dizi dönüşü hakkında bilgiler de görüntülenir.  

## <a name="where-to-find-the-report"></a>Raporun nerede bulunacağı

Araç çalıştırıldığında sonuçları **AzsReadinessCheckerReport. JSON**öğesine kaydeder. Araç ayrıca **Azsreadinesschecker. log**adlı bir günlük oluşturur. Bu dosyaların konumu, PowerShell 'deki doğrulama sonuçlarıyla birlikte görüntülenir:

![çalıştırma-doğrulama](./media/azure-stack-validation-report/validation.png)

Her iki dosya da aynı bilgisayarda çalıştırıldığında sonraki doğrulama denetimlerinin sonuçlarını kalıcı hale getirin. Örneğin, araç, sertifikaları doğrulamak, Azure kimliğini doğrulamak için yeniden çalıştırmak ve sonra kaydın doğrulanması için üçüncü bir kez çalıştırılabilir. Üç doğrulamalarının tümünün sonuçları elde edilen. JSON raporunda bulunur.  

Varsayılan olarak, her iki dosya da **C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.JSON**yazılır.  

- Farklı bir rapor konumu belirtmek için komut satırının sonundaki parametresinikullanın.`-OutputPath <path>`
- **AzsReadinessCheckerReport. JSON**adresinden aracın önceki çalıştırmaları hakkındaki bilgileri temizlemek için komut satırının sonundaki parametreyikullanın.`-CleanReport`

## <a name="view-the-report"></a>Raporu görüntüleyin

Raporu PowerShell 'de görüntülemek için, raporun yolunu bir değer `-ReportPath`olarak sağlayın. Bu komut, raporun içeriğini görüntüler ve henüz sonuçları olmayan doğrulamaları tanımlar.

Örneğin, raporu raporun bulunduğu konuma açık bir PowerShell isteminden görüntülemek için aşağıdaki komutu çalıştırın:

```powershell
Read-AzsReadinessReport -ReportPath .\AzsReadinessReport.json
```

Çıktı aşağıdaki örneğe benzer:

```shell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation results not available.

############### Registration Validation Summary ###############

Azure Registration Validation results not available.

############### Azure Identity Results ###############

Test                          : ServiceAdministrator
Result                        : OK
AAD Service Admin             : admin@contoso.onmicrosoft.com
Azure Environment             : AzureCloud
Azure Active Directory Tenant : contoso.onmicrosoft.com
Error Details                 : 

############### Azure Identity Validation Summary ###############

Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.

############### AzsReadiness Job Summary ###############

Index             : 0
Operations        : 
StartTime         : 2018/10/22 14:24:16
EndTime           : 2018/10/22 14:24:19
Duration          : 3
PSBoundParameters :
```

## <a name="view-the-report-summary"></a>Rapor özetini görüntüleme

Raporun özetini görüntülemek için, PowerShell komutunun sonuna `-summary` parametresini ekleyebilirsiniz. Örneğin:

```powershell
Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -summary
```

Özet, sonuçları olmayan doğrulamaları gösterir ve tamamlanan doğrulamaları için başarılı veya başarısız olduğunu gösterir. Çıktı aşağıdaki örneğe benzer:

```shell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation found no errors or warnings.

############### Registration Validation Summary ###############

Registration Validation found no errors or warnings.

############### Azure Identity Validation Summary ###############

Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.
```

## <a name="view-a-filtered-report"></a>Filtrelenmiş bir raporu görüntüleme

Tek bir doğrulama türüyle filtrelenmiş bir raporu görüntülemek için, aşağıdaki değerlerden biriyle **-ReportSections** parametresini kullanın:

- Sertifika
- AzureRegistration
- AzureIdentity
- Graf
- ADFS
- İşler
- Tümü  

Örneğin, yalnızca sertifikaların rapor özetini görüntülemek için aşağıdaki PowerShell komut satırını kullanın:

```powershell
Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -ReportSections Certificate - Summary
```
