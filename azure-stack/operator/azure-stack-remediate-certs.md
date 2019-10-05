---
title: Azure Stack için sertifika sorunlarını düzeltin | Microsoft Docs
description: Sertifika sorunlarını gözden geçirmek ve düzeltmek için Azure Stack hazırlık denetleyicisi ' ni kullanın.
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
ms.openlocfilehash: 6e8adbc0d84c7816a081e751473764aab79cfcf2
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961907"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Azure Stack PKI sertifikaları için sık karşılaşılan sorunları düzeltin

Bu makaledeki bilgiler, Azure Stack PKI sertifikalarıyla ilgili yaygın sorunları anlamanıza ve çözmenize yardımcı olabilir. [Azure Stack PKI sertifikalarını doğrulamak](azure-stack-validate-pki-certs.md)için Azure Stack hazırlık Denetleyicisi aracını kullandığınızda sorunları bulabilirsiniz. Araç, sertifikaların Azure Stack dağıtım Azure Stack ve gizli anahtar döndürme PKI gereksinimlerini karşıladığından emin olup sonuçları bir [Report. JSON dosyasına](azure-stack-validation-report.md)kaydeder.  

## <a name="pfx-encryption"></a>PFX şifrelemesi

**Hata** -PFX şifrelemesi, TripleDES-SHA1 değildir.

**Düzeltme** -PFX dosyalarını **TRIPLEDES-SHA1** şifrelemesi ile dışarı aktarın. Bu, Sertifika ek bileşeninden dışarı aktarırken veya `Export-PFXCertificate` kullanılarak tüm Windows 10 istemcileri için varsayılandır.

## <a name="read-pfx"></a>PFX oku

**Uyarı** -parola yalnızca sertifikadaki özel bilgileri korur.  

**Düzeltme** -PFX dosyalarını, **sertifika gizliliğini etkinleştir**için isteğe bağlı ayarı ile dışarı aktarın.  

**Hata** -pfx dosyası geçersiz.  

**Düzeltme** - [DAĞıTıM için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md)bölümündeki adımları kullanarak sertifikayı yeniden dışarı aktarın.

## <a name="signature-algorithm"></a>İmza algoritması

**Hata** -Imza algoritması SHA1.

**Düzeltme** -sertifika imzalama isteği 'NI (CSR) SHA256 imza algoritması ile yeniden oluşturmak için Azure Stack sertifikaları imzalama isteği oluşturma ' daki adımları kullanın. Ardından, sertifikayı yeniden göndermek için CSR 'yi sertifika yetkilisine yeniden gönderin.

## <a name="private-key"></a>Özel anahtar

**Hata** -özel anahtar eksik veya yerel makine özniteliğini içermiyor.  

**Düzeltme** -CSR 'yi oluşturan bilgisayardan, [DAĞıTıM için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment)bölümündeki adımları kullanarak sertifikayı yeniden dışarı aktarın. Bu adımlar yerel makine sertifika deposundan dışarı aktarmayı içerir.

## <a name="certificate-chain"></a>Sertifika zinciri

**Hata** -sertifika zinciri tamamlanmadı.  

**Düzeltme** -sertifikalar tam bir sertifika zinciri içermelidir. [Dağıtım için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) bölümündeki adımları kullanarak sertifikayı yeniden dışarı aktarın ve **Mümkünse sertifika yolundaki tüm sertifikaları Ekle**seçeneğini belirleyin.

## <a name="dns-names"></a>DNS adları

**Hata** -sertifikadaki **Dnsnamelist** , Azure Stack hizmet uç noktası adı veya geçerli bir joker karakter eşleşmesi içermiyor. Joker karakter eşleşmeleri yalnızca, DNS adının sol üst ad alanı için geçerlidir. Örneğin, `*.region.domain.com` yalnızca `portal.region.domain.com` için geçerlidir, `*.table.region.domain.com` ' dir.

**Düzeltme** -Azure Stack uç noktaları desteklemek üzere CSR 'YI doğru DNS adlarıyla yeniden oluşturmak için Azure Stack sertifikaları imzalama isteği oluşturma ' daki adımları kullanın. CSR 'yi bir sertifika yetkilisine yeniden gönderin ve ardından, sertifikayı CSR 'yi oluşturan makineden dışarı aktarmak için [dağıtım için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) bölümündeki adımları uygulayın.  

## <a name="key-usage"></a>Anahtar kullanımı

**Hata** -anahtar kullanımında dijital imza veya anahtar şifrelemesi eksik ya da Gelişmiş anahtar kullanımında sunucu kimlik doğrulaması veya istemci kimlik doğrulaması eksik.  

**Düzeltme** -CSR 'yi doğru anahtar kullanımı öznitelikleriyle yeniden oluşturmak için [Azure Stack sertifikaları imzalama isteği oluşturma](azure-stack-get-pki-certs.md) ' daki adımları kullanın. CSR 'yi sertifika yetkilisine yeniden gönderin ve bir sertifika şablonunun istekteki anahtar kullanımının üzerine yazılmadığından emin olun.

## <a name="key-size"></a>Anahtar boyutu

**Hata** -anahtar boyutu 2048 ' den küçük.

**Düzeltme** -CSR 'yi doğru anahtar uzunluğu (2048) ile yeniden oluşturmak için [Azure Stack sertifikaları imzalayan istek oluşturma](azure-stack-get-pki-certs.md) ' daki ADıMLARı kullanın ve ardından CSR 'yi sertifika yetkilisine yeniden gönderin.

## <a name="chain-order"></a>Zincir sırası

**Hata** -Sertifika zincirinin sırası yanlış.  

**Düzeltme** - [DAĞıTıM için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) bölümündeki adımları kullanarak sertifikayı yeniden dışarı aktarın ve **Mümkünse sertifika yolundaki tüm sertifikaları Ekle**seçeneğini belirleyin. Dışarı aktarma için yalnızca yaprak sertifikanın seçildiğinden emin olun.

## <a name="other-certificates"></a>Diğer sertifikalar

**Hata** -PFX paketi, yaprak sertifikası veya Sertifika zincirinin bir parçası olmayan sertifikalar içeriyor.  

**Düzeltme** - [DAĞıTıM için Azure Stack PKI sertifikalarını hazırlama](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment)bölümündeki adımları kullanarak sertifikayı yeniden dışarı aktarın ve **Mümkünse sertifika yolundaki tüm sertifikaları Ekle**seçeneğini belirleyin. Dışarı aktarma için yalnızca yaprak sertifikanın seçildiğinden emin olun.

## <a name="fix-common-packaging-issues"></a>Yaygın paketleme sorunlarını giderme

**Azsreadinesschecker** Aracı, bir PFX dosyasını içeri aktarıp daha sonra ortak paketleme sorunlarını gidermek IÇIN bir PFX dosyasını içeri aktarıp dışarı aktarabilen **Repair-AzsPfxCertificate**adlı bir yardımcı cmdlet içerir:

- **PFX şifrelemesi** , TripleDES-SHA1 değildir.
- **Özel anahtarda** yerel makine özniteliği eksik.
- **Sertifika zinciri** eksik veya yanlış. PFX paketi yoksa yerel makinenin sertifika zincirini içermesi gerekir.
- **Diğer sertifikalar**

Yeni bir CSR oluşturmanız ve sertifikayı yeniden oluşturmanız gerekiyorsa, **Repair-AzsPfxCertificate** yardımcı olamaz.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların, aracın çalıştığı bilgisayarda olması gerekir:

- Internet bağlantısı ile Windows 10 veya Windows Server 2016.
- PowerShell 5,1 veya sonraki bir sürümü. Sürümünüzü denetlemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın ve ardından *ana** ve **İkincil** sürümleri gözden geçirin:

   ```powershell
   $PSVersionTable.PSVersion
   ```

- [Azure Stack Için PowerShell](azure-stack-powershell-install.md)'i yapılandırın.
- [Azure Stack hazır olma denetleyicisi](https://aka.ms/AzsReadinessChecker) aracının en son sürümünü indirin.

### <a name="import-and-export-an-existing-pfx-file"></a>Mevcut bir PFX dosyasını içeri ve dışarı aktarma

1. Önkoşulları karşılayan bir bilgisayarda, yükseltilmiş bir PowerShell istemi açın ve sonra Azure Stack hazırlık denetleyicisi 'ni yüklemek için aşağıdaki komutu çalıştırın:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. PowerShell komut isteminde, PFX parolasını ayarlamak için aşağıdaki cmdlet 'i çalıştırın. @No__t-0 değerini gerçek parolayla değiştirin:

   ```powershell
   $password = Read-Host -Prompt PFXpassword -AsSecureString
   ```

3. PowerShell komut isteminde, yeni bir PFX dosyasını dışarı aktarmak için aşağıdaki komutu çalıştırın:

   - @No__t-0 için, çalıştığınız PFX dosyasının yolunu belirtin. Aşağıdaki örnekte, yol `.\certificates\ssl.pfx` ' dır.
   - @No__t-0 için, dışarı aktarma için PFX dosyasının konumunu ve adını belirtin. Aşağıdaki örnekte, yol `.\certificates\ssl_new.pfx` ' dır:

   ```powershell
   Repair-AzsPfxCertificate -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
   ```  

4. Araç tamamlandıktan sonra, başarı için çıktıyı gözden geçirin:

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
