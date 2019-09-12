---
title: Azure Stack tümleşik sistemler dağıtımı için Azure Stack ortak anahtar altyapısı sertifikaları oluşturun | Microsoft Docs
description: Azure Stack tümleşik sistemler için Azure Stack PKI sertifikası dağıtım işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: justinha
ms.reviewer: ppacent
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: c9f14e643f886fab0fae148c5af8643890866fd6
ms.sourcegitcommit: 38f21e0bcf7b593242ad615c9d8ef8a1ac19c734
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70902694"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack sertifika imzalama isteği oluşturma

Azure Stack dağıtımına uygun sertifika Imzalama Istekleri (CSR) oluşturmak için Azure Stack hazırlık Denetleyicisi aracını kullanabilirsiniz. Sertifikaların dağıtımdan önce test edebilmesi için istenen, oluşturulması ve doğrulanması gerekir. Aracı [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker)alabilirsiniz.

Aşağıdaki sertifikaları istemek için Azure Stack hazırlık Denetleyicisi aracı 'nı (AzsReadinessChecker) kullanabilirsiniz:

- [Azure Stack dağıtımı IÇIN PKI sertifikaları oluşturmaya](azure-stack-get-pki-certs.md)göre **standart sertifika istekleri** .
- **Hizmet olarak platform**: [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri-Isteğe bağlı PaaS sertifikaları](azure-stack-pki-certs.md#optional-paas-certificates)' nda belirtildiği gibi sertifikalar için hizmet olarak platform (PaaS) adları isteyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Sisteminiz, bir Azure Stack dağıtımı için PKI sertifikaları için CSR 'leri oluşturmadan önce aşağıdaki önkoşulları karşılamalıdır:

- Microsoft Azure Stack hazırlık denetleyicisi
- Sertifika öznitelikleri:
  - Bölge adı
  - Dış tam etki alanı adı (FQDN)
  - Subject
- Windows 10 veya Windows Server 2016 veya üzeri

  > [!NOTE]  
  > Sertifikalarınızı Sertifika yetkilinizden geri aldığınızda, [Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md) adımlarının aynı sistemde tamamlanması gerekir!

## <a name="generate-certificate-signing-requests"></a>Sertifika imzalama isteği oluştur

Azure Stack PKI sertifikalarını hazırlamak ve doğrulamak için aşağıdaki adımları kullanın:

1. Aşağıdaki cmdlet 'i çalıştırarak AzsReadinessChecker 'yi bir PowerShell isteminden (5,1 veya üzeri) çalıştırın:

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ```

2. **Konuyu** sıralı bir sözlük olarak bildirin. Örneğin:

    ```powershell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
    ```

    > [!note]  
    > Ortak bir ad (CN) sağlanırsa, bu, sertifika isteğinin ilk DNS adı ile üzerine yazılır.

3. Zaten var olan bir çıktı dizini bildirin. Örneğin:

    ```powershell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ```

4. Kimlik sistemini bildirme

    Azure Active Directory

    ```powershell
    $IdentitySystem = "AAD"
    ```

    Active Directory Federasyon Hizmetleri

    ```powershell
    $IdentitySystem = "ADFS"
    ```

5. Azure Stack dağıtımı için tasarlanan **bölge adı** ve **dış FQDN** bildirin.

    ```powershell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ```

    > [!note]  
    > `<regionName>.<externalFQDN>`Azure Stack içindeki tüm dış DNS adlarının oluşturulduğu temeli formlarda, bu örnekte Portal olur `portal.east.azurestack.contoso.com`.  

6. Her DNS adı için sertifika imzalama istekleri oluşturmak için:

    ```powershell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    PaaS hizmetlerini dahil etmek için anahtarı belirtin```-IncludePaaS```

7. Alternatif olarak, geliştirme ve test ortamları için, birden çok konu diğer adıyla tek bir sertifika isteği oluşturmak üzere Add **-RequestType SingleCSR** parametresi ve değeri **(üretim** ortamları için önerilmez):

    ```powershell  
    New-AzsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    PaaS hizmetlerini dahil etmek için anahtarı belirtin```-IncludePaaS```

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

9. Öğesini gönderebilirsiniz **.** Sertifika yetkiliniz (iç ya da genel) için oluşturulan REQ dosyası.  **New-Azscercertificateatesigningrequest** çıkış dizini, bir sertifika yetkilisine göndermek IÇIN gereken CSR 'leri içerir.  Dizin, başvuru için sertifika isteği oluşturma sırasında kullanılan INF dosyalarını içeren bir alt dizin olan başvurunuz için de içerir. Sertifika yetkilinizin, [Azure Stack PKI gereksinimlerini](azure-stack-pki-certs.md)karşılayan, oluşturulan isteğinizi kullanarak sertifikalar üretdiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md)
