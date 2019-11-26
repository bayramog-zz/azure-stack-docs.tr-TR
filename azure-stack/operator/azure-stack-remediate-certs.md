---
title: Fix common issues with PKI certificates
titleSuffix: Azure Stack
description: Fix common issues with Azure Stack PKI certificates using the Azure Stack Readiness Checker.
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
ms.date: 10/03/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/19/2018
ms.openlocfilehash: 3cff638fa242e4d70230062e7d54eef0c9c66802
ms.sourcegitcommit: 284f5316677c9a7f4c300177d0e2a905df8cb478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465406"
---
# <a name="fix-common-issues-with-azure-stack-pki-certificates"></a>Fix common issues with Azure Stack PKI certificates

The information in this article helps you understand and resolve common issues with Azure Stack PKI certificates. You can discover issues when you use the Azure Stack Readiness Checker tool to [validate Azure Stack PKI certificates](azure-stack-validate-pki-certs.md). The tool checks if the certificates meet the PKI requirements of an Azure Stack deployment and Azure Stack secret rotation, and then logs the results to a [report.json file](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>PFX Encryption

**Issue** - PFX encryption isn't TripleDES-SHA1.

**Fix** - Export PFX files with **TripleDES-SHA1** encryption. This is the default encryption for all Windows 10 clients when exporting from certificate snap-in or using `Export-PFXCertificate`.

## <a name="read-pfx"></a>Read PFX

**Warning** - Password only protects the private information in the certificate.  

**Fix** - Export PFX files with the optional setting for **Enable certificate privacy**.  

**Issue** - PFX file invalid.  

**Fix** - Re-export the certificate using the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>Signature algorithm

**Issue** - Signature algorithm is SHA1.

**Fix** - Use the steps in Azure Stack certificates signing request generation to regenerate the certificate signing request (CSR) with the signature algorithm of SHA256. Then resubmit the CSR to the certificate authority to reissue the certificate.

## <a name="private-key"></a>Private key

**Issue** - The private key is missing or doesn't contain the local machine attribute.  

**Fix** - From the computer that generated the CSR, re-export the certificate using the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment). These steps include exporting from the local machine certificate store.

## <a name="certificate-chain"></a>Certificate chain

**Issue** - Certificate chain isn't complete.  

**Fix** - Certificates should contain a complete certificate chain. Re-export the certificate using the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) and select the option **Include all certificates in the certification path if possible**.

## <a name="dns-names"></a>DNS names

**Issue** - The **DNSNameList** on the certificate doesn't contain the Azure Stack service endpoint name or a valid wildcard match. Wildcard matches are only valid for the left-most namespace of the DNS name. For example, `*.region.domain.com` is only valid for `portal.region.domain.com`, not `*.table.region.domain.com`.

**Fix** - Use the steps in Azure Stack certificates signing request generation to regenerate the CSR with the correct DNS names to support Azure Stack endpoints. Resubmit the CSR to a certificate authority. Then follow the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) to export the certificate from the machine that generated the CSR.  

## <a name="key-usage"></a>Key usage

**Issue** - Key usage is missing digital signature or key encipherment, or enhanced key usage is missing server authentication or client authentication.  

**Fix** - Use the steps in [Azure Stack certificates signing request generation](azure-stack-get-pki-certs.md) to regenerate the CSR with the correct key usage attributes. Resubmit the CSR to the certificate authority and confirm that a certificate template isn't overwriting the key usage in the request.

## <a name="key-size"></a>Key size

**Issue** - Key size is smaller than 2048.

**Fix** - Use the steps in [Azure Stack certificates signing request generation](azure-stack-get-pki-certs.md) to regenerate the CSR with the correct key length (2048), and then resubmit the CSR to the certificate authority.

## <a name="chain-order"></a>Chain order

**Issue** - The order of the certificate chain is incorrect.  

**Fix** - Re-export the certificate using the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) and select the option **Include all certificates in the certification path if possible**. Ensure that only the leaf certificate is selected for export.

## <a name="other-certificates"></a>Other certificates

**Issue** - The PFX package contains certificates that aren't the leaf certificate or part of the certificate chain.  

**Fix** - Re-export the certificate using the steps in [Prepare Azure Stack PKI certificates for deployment](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment), and select the option **Include all certificates in the certification path if possible**. Ensure that only the leaf certificate is selected for export.

## <a name="fix-common-packaging-issues"></a>Fix common packaging issues

The **AzsReadinessChecker** tool contains a helper cmdlet called **Repair-AzsPfxCertificate**, which can import and then export a PFX file to fix common packaging issues, including:

- **PFX encryption** isn't TripleDES-SHA1.
- **Private key** is missing local machine attribute.
- **Certificate chain** is incomplete or wrong. The local machine must contain the certificate chain if the PFX package doesn't.
- **Other certificates**

**Repair-AzsPfxCertificate** can't help if you need to generate a new CSR and reissue a certificate.

### <a name="prerequisites"></a>Önkoşullar

The following prerequisites must be in place on the computer on which the tool runs:

- Windows 10 or Windows Server 2016, with internet connectivity.
- PowerShell 5.1 or later. To check your version, run the following PowerShell cmdlet and then review the *Major** and **Minor** versions:

   ```powershell
   $PSVersionTable.PSVersion
   ```

- Configure [PowerShell for Azure Stack](azure-stack-powershell-install.md).
- Download the latest version of the [Azure Stack readiness checker](https://aka.ms/AzsReadinessChecker) tool.

### <a name="import-and-export-an-existing-pfx-file"></a>Import and export an existing PFX File

1. On a computer that meets the prerequisites, open an elevated PowerShell prompt, and then run the following command to install the Azure Stack readiness checker:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. From the PowerShell prompt, run the following cmdlet to set the PFX password. Replace `PFXpassword` with the actual password:

   ```powershell
   $password = Read-Host -Prompt PFXpassword -AsSecureString
   ```

3. From the PowerShell prompt, run the following command to export a new PFX file:

   - For `-PfxPath`, specify the path to the PFX file you're working with. In the following example, the path is `.\certificates\ssl.pfx`.
   - For `-ExportPFXPath`, specify the location and name of the PFX file for export. In the following example, the path is `.\certificates\ssl_new.pfx`:

   ```powershell
   Repair-AzsPfxCertificate -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
   ```  

4. After the tool completes, review the output for success:

   ```shell
   Repair-AzsPfxCertificate v1.1809.1005.1 started.
   Starting Azure Stack Certificate Import/Export
   Importing PFX .\certificates\ssl.pfx into Local Machine Store
   Exporting certificate to .\certificates\ssl_new.pfx
   Export complete. Removing certificate from the local machine store.
   Removal complete.
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Repair-AzsPfxCertificate Completed
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-rotate-secrets.md)
