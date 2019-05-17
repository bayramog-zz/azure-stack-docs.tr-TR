---
title: Azure Stack tümleşik sistemleri dağıtımı için Azure Stack ortak anahtar altyapısı sertifikalarını oluşturmak | Microsoft Docs
description: Azure Stack tümleşik sistemleri için Azure Stack PKI sertifika dağıtım işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: 1c342b1edb86629fff95dc04735fd5b6d98fc70a
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782265"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack sertifika imzalama isteği oluşturma

Sertifika imzalama isteği (CSR) bir Azure Stack dağıtımı uygun oluşturmak için Azure Stack hazırlık Denetleyicisi aracını kullanabilirsiniz. Sertifikaları istenen, oluşturulan ve dağıtımdan önce test etmek için yeterli zaman ile doğrulanması gerekir. Aracı alabilirsiniz [PowerShell Galerisi'ndeki](https://aka.ms/AzsReadinessChecker).

Aşağıdaki sertifikalar istemek için Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) kullanabilirsiniz:

- **Standart sertifika isteklerini** göre [Azure Stack dağıtımı için PKI sertifikaları oluşturmak](azure-stack-get-pki-certs.md).
- **Hizmet olarak Platform**: Hizmet olarak platform (PaaS) adları için belirtilen sertifika isteyebilir [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri - isteğe bağlı PaaS sertifikaları](azure-stack-pki-certs.md#optional-paas-certificates).

## <a name="prerequisites"></a>Önkoşullar

Sisteminizde Azure Stack dağıtımı için PKI sertifikaları için CSR(s) oluşturmadan önce aşağıdaki önkoşulları karşılamalıdır:

- Microsoft Azure Stack hazırlık denetleyicisi
- Sertifika öznitelikleri:
  - Bölge adı
  - Dış tam etki alanı adı (FQDN)
  - Subject
- Windows 10 veya Windows Server 2016

  > [!NOTE]  
  > Sertifikalarınızı aldığınızda adımlarda Sertifika yetkilinizden geri [hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md) aynı sistemde tamamlanması gerekir!

## <a name="generate-certificate-signing-requests"></a>Sertifika imzalama istekleri oluştur

Hazırlama ve Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın:

1. AzsReadinessChecker bir PowerShell İstemi'nden (5.1 veya üzeri), aşağıdaki cmdlet'i çalıştırarak yükleyin:

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ```

2. Bildirme **konu** sıralı bir sözlük olarak. Örneğin:

    ```powershell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
    ```

    > [!note]  
    > Ortak ad (CN) sağlandıysa Bu sertifika isteğini ilk DNS adı tarafından üzerine yazılır.

3. Zaten bir çıktı dizini bildirin. Örneğin:

    ```powershell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ```

4. Kimlik sistemi bildirme

    Azure Active Directory

    ```powershell
    $IdentitySystem = "AAD"
    ```

    Active Directory Federasyon Hizmetleri

    ```powershell
    $IdentitySystem = "ADFS"
    ```

5. Bildirme **bölge adı** ve **dış FQDN** Azure Stack dağıtım için hedeflenen.

    ```powershell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ```

    > [!note]  
    > `<regionName>.<externalFQDN>` temelini, tüm dış DNS adlarını Azure Stack'te oluşturulur, bu örnekte, portalı olacaktır `portal.east.azurestack.contoso.com`.  

6. Sertifika imzalama istekleri için her bir DNS adı oluşturmak için:

    ```powershell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    PaaS Hizmetleri dahil etmek için anahtar belirtin. ```-IncludePaaS```

7. Alternatif olarak, geliştirme ve Test ortamları için oluşturmak için birden çok konu diğer adları ile tek bir sertifika isteği eklenmesini **- RequestType SingleCSR** parametresi ve değeri (**değil** için önerilen Üretim ortamları için):

    ```powershell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    PaaS Hizmetleri dahil etmek için anahtar belirtin. ```-IncludePaaS```

8. Çıktıyı gözden geçirin:

    ```powershell  
    New-AzsCertificateSigningRequest v1.1809.1005.1 started.

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com*dn2=*.adminhosting.east.azurestack.contoso.com@dns=*.hosting.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    New-AzsCertificateSigningRequest Completed
    ```

9. Gönderme **. İstek** (dahili veya genel) CA için oluşturulan dosya.  Çıktı dizinine **yeni AzsCertificateSigningRequest** sertifika yetkilisine göndermek gerekli CSR(s) içerir.  Dizini Ayrıca, başvuru için sertifika isteği oluşturma sırasında kullanılan INF dosyaları içeren bir alt dizin içeriyor. CA'nız karşılayan oluşturulan isteğiniz kullanarak sertifikaları oluşturur mutlaka [Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI sertifikaları hazırlama](azure-stack-prepare-pki-certs.md)
