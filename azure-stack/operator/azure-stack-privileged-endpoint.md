---
title: Azure Stack 'da ayrıcalıklı uç nokta kullanma | Microsoft Docs
description: Azure Stack (Azure Stack işleci için) ayrıcalıklı uç noktanın (PEP) nasıl kullanılacağını gösterir.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/03/2019
ms.author: justinha
ms.reviewer: fiseraci
ms.lastreviewed: 09/03/2019
ms.openlocfilehash: a278a918100619953b2b7eb9b288236625968187
ms.sourcegitcommit: 38f21e0bcf7b593242ad615c9d8ef8a1ac19c734
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70902617"
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Azure Stack 'da ayrıcalıklı uç noktası kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak, gündelik yönetim görevlerinin çoğunda yönetici portalını, PowerShell'i veya Azure Resource Manager API'lerini kullanmanız gerekir. Ancak, bazı daha az yaygın işlemler için *ayrıcalıklı uç noktayı* (Pep) kullanmanız gerekir. PEP, gerekli bir görevi gerçekleştirmenize yardımcı olmak için size yeterli yetenek sağlayan, önceden yapılandırılmış bir uzak PowerShell konsoludur. Uç nokta, yalnızca kısıtlı bir cmdlet kümesini kullanıma sunmak için [PowerShell JEA 'yı (yeterli yönetim)](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview) kullanır. PEP 'ye erişmek ve kısıtlanmış cmdlet 'ler kümesini çağırmak için düşük ayrıcalıklı bir hesap kullanılır. Yönetici hesabı gerekli değildir. Ek güvenlik için komut dosyasına izin verilmez.

PEP 'yi aşağıdakiler gibi görevleri gerçekleştirmek için kullanabilirsiniz:

- [Tanılama günlüklerini toplama](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep)gibi alt düzey görevleri gerçekleştirmek için.
- Dağıtımdan sonra etki alanı adı sistemi (DNS) ileticileri ekleme, Microsoft Graph tümleştirme, Active Directory Federasyon Hizmetleri (AD FS) (AD FS) tümleştirmesi gibi tümleşik sistemler için çok dağıtım sonrası veri merkezi tümleştirme görevlerini gerçekleştirmek için Sertifika döndürme, vb.
- Tümleşik bir sistemin derinlemesine sorunlarını gidermek için geçici ve yüksek düzeyde erişim sağlamak amacıyla desteğiyle birlikte çalışmak için.

PEP, PowerShell oturumunda gerçekleştirdiğiniz her eylemi (ve ilgili çıktıyı) günlüğe kaydeder. Bu, tam saydamlığın yanı sıra işlemlerin tam denetimini sağlar. Bu günlük dosyalarını gelecekteki denetimler için koruyabilirsiniz.

> [!NOTE]
> Azure Stack Geliştirme Seti (ASDK) ' de, PEP 'de bulunan komutlardan bazılarını, geliştirme seti konağındaki bir PowerShell oturumundan doğrudan çalıştırabilirsiniz. Ancak, tümleşik sistemler ortamında belirli işlemleri gerçekleştirmek için kullanılabilecek tek yöntem olduğundan, günlük toplama gibi PEP 'yi kullanarak bazı işlemleri test etmek isteyebilirsiniz.

## <a name="access-the-privileged-endpoint"></a>Ayrıcalıklı uç noktaya erişin

Pep 'yi, PEP 'yi barındıran sanal makinede bulunan uzak bir PowerShell oturumu aracılığıyla erişirsiniz. Bu sanal makine, ASDK 'de **AZS-ERCS01**olarak adlandırılır. Tümleşik bir sistem kullanıyorsanız, her biri bir sanal makinede (*ön ek*-ERCS01, *ön*ek-ERCS02 veya *ön*ek-ERCS03) esneklik için farklı konaklarda çalışan Pep 'nin üç örneği vardır. 

Tümleşik bir sistem için bu yordama başlamadan önce, PEP 'ye IP adresine veya DNS aracılığıyla erişebildiğinizden emin olun. İlk Azure Stack dağıtımından sonra, DNS tümleştirmesi henüz ayarlanmadığından PEP 'ye yalnızca IP adresi ile erişebilirsiniz. OEM donanım satıcınız, PEP IP adreslerini içeren **Azurestackstampdeploymentınfo** ADLı bir JSON dosyası sağlar.


> [!NOTE]
> Güvenlik nedenleriyle, PEP 'yi yalnızca donanım yaşam döngüsü konağının üzerinde çalışan sağlamlaştırılmış bir sanal makineden veya [ayrıcalıklı erişim Iş istasyonu](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)gibi adanmış, güvenli bir bilgisayardan bağlanmanız gerekir. Donanım yaşam döngüsü konağının özgün yapılandırması, yeni yazılım yükleme de dahil olmak üzere özgün yapılandırmasından değiştirilmemelidir ve PEP 'ye bağlanmak için kullanılması gerekir.

1. Güveni oluşturun.

    - Tümleşik bir sistemde, PEP 'yi, donanım yaşam döngüsü konağında veya ayrıcalıklı erişim Iş Istasyonunda çalışan sağlamlaştırılmış sanal makineye güvenilir bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumundan aşağıdaki komutu çalıştırın.

      ```powershell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ```
    - ASDK çalıştırıyorsanız, geliştirme seti ana bilgisayarında oturum açın.

2. Donanım yaşam döngüsü konağı veya ayrıcalıklı erişim Iş Istasyonu üzerinde çalışan sağlamlaştırılmış sanal makinede, bir Windows PowerShell oturumu açın. PEP 'yi barındıran sanal makinede uzak bir oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
   - Tümleşik bir sistemde:
     ```powershell
       $cred = Get-Credential

       Enter-PSSession -ComputerName <IP_address_of_ERCS> `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ```
     `ComputerName` Parametresi, Pep 'yi barındıran sanal makinelerden birinin IP adresi ya da DNS adı olabilir. 

     >[!NOTE]
     >Azure Stack, PEP kimlik bilgisi doğrulanırken uzak bir çağrı yapmaz. Bunu yapmak için yerel olarak depolanmış bir RSA ortak anahtarını kullanır.
     
   - ASDK çalıştırıyorsanız:
     
     ```powershell
       $cred = Get-Credential

       Enter-PSSession -ComputerName azs-ercs01 `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ``` 
     İstendiğinde, aşağıdaki kimlik bilgilerini kullanın:

     - **Kullanıcı adı**:  **&lt; *Azure Stack*Domain\cloudadminbiçimindeCloudAdminhesabınıbelirtin&gt;** . (ASDK için, Kullanıcı adı **azurestack\cloudadmin**.)
     - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında girilen parolayı girin.

     > [!NOTE]
     > ERCS uç noktasına bağlanamıyorsanız, farklı bir ERCS VM IP adresi ile bir ve iki adımı yeniden deneyin.

3. Bağlandıktan sonra, istem [ ***IP adresi veya ercs VM adı*] olarak değişir: PS >** **veya [AZS-ercs01]: PS,** ortama göre >. Buradan, kullanılabilir cmdlet `Get-Command` 'lerin listesini görüntülemek için öğesini çalıştırın.

   Bu cmdlet 'lerin birçoğu yalnızca tümleşik sistem ortamları için tasarlanmıştır (örneğin, veri merkezi tümleştirmesiyle ilgili cmdlet 'ler). ASDK 'de, aşağıdaki cmdlet 'ler doğrulandıktan sonra:

   - Şifresiz ana bilgisayar
   - Close-PrivilegedEndpoint
   - Çıkış-PSSession
   - Get-AzureStackLog
   - Get-AzureStackStampInformation
   - Get-Command
   - Get-FormatData
   - Yardım alın
   - On-üç Dpartybildirimleri
   - Ölçü-nesne
   - New-CloudAdminUser
   - Dışarı-varsayılan
   - Remove-CloudAdminUser
   - Select-Object
   - Set-CloudAdminUserPassword
   - Test-AzureStack
   - Stop-AzureStack
   - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktayı kullanmaya yönelik ipuçları 

Yukarıda belirtildiği gibi, PEP bir [PowerShell JEA](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview) uç noktasıdır. Güçlü bir güvenlik katmanı sağlarken, bir JEA uç noktası, komut dosyası veya sekme tamamlama gibi bazı temel PowerShell özelliklerini azaltır. Herhangi bir tür betik işlemi denerseniz, işlem **Scriptsnotallowed**hatasıyla başarısız olur. Bu beklenen bir davranıştır.

Bu nedenle, örneğin, belirli bir cmdlet 'in parametre listesini almak için aşağıdaki komutu çalıştırın:

```powershell
    Get-Command <cmdlet_name> -Syntax
```

Alternatif olarak, tüm PEP cmdlet 'lerini yerel makinenizde geçerli oturuma aktarmak için [Import-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) cmdlet 'ini de kullanabilirsiniz. Bunu yaparak, PEP 'nin tüm cmdlet 'leri ve işlevleri artık yerel makinenizde, sekme tamamlama ve genel, betik oluşturma ile birlikte kullanılabilir. 

Yerel makinenizde PEP oturumunu içeri aktarmak için aşağıdaki adımları uygulayın:

1. Güveni oluşturun.

    -Tümleşik bir sistemde, PEP 'yi, donanım yaşam döngüsü konağında veya ayrıcalıklı erişim Iş Istasyonunda çalışan sağlamlaştırılmış sanal makineye güvenilir bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumunda aşağıdaki komutu çalıştırın.

      ```powershell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ```
    - ASDK çalıştırıyorsanız, geliştirme seti ana bilgisayarında oturum açın.

2. Donanım yaşam döngüsü konağı veya ayrıcalıklı erişim Iş Istasyonu üzerinde çalışan sağlamlaştırılmış sanal makinede, bir Windows PowerShell oturumu açın. PEP 'yi barındıran sanal makinede uzak bir oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
   - Tümleşik bir sistemde:
     ```powershell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName <IP_address_of_ERCS> `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ```
     `ComputerName` Parametresi, Pep 'yi barındıran sanal makinelerden birinin IP adresi ya da DNS adı olabilir. 
   - ASDK çalıştırıyorsanız:
     
     ```powershell
      $cred = Get-Credential

      $session = New-PSSession -ComputerName azs-ercs01 `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ``` 
     İstendiğinde, aşağıdaki kimlik bilgilerini kullanın:

     - **Kullanıcı adı**:  **&lt; *Azure Stack*Domain\cloudadminbiçimindeCloudAdminhesabınıbelirtin&gt;** . (ASDK için, Kullanıcı adı **azurestack\cloudadmin**.)
     - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında girilen parolayı girin.

3. PEP oturumunu yerel makinenize aktarma
     ```powershell 
        Import-PSSession $session
   ```
4. Şimdi, Azure Stack güvenlik duruşunu azaltmadan, her zamanki gibi, PEP 'nin tüm işlevleri ve cmdlet 'leri ile birlikte sekme tamamlama ve komut dosyası oluşturma 'yı kullanabilirsiniz. Keyfini çıkarın!


## <a name="close-the-privileged-endpoint-session"></a>Ayrıcalıklı uç nokta oturumunu kapatma

 Daha önce belirtildiği gibi, PEP, PowerShell oturumunda gerçekleştirdiğiniz her eylemi (ve ilgili çıktıyı) günlüğe kaydeder. `Close-PrivilegedEndpoint` Cmdlet 'ini kullanarak oturumu kapatmanız gerekir. Bu cmdlet, uç noktayı doğru bir şekilde kapatır ve bekletme için günlük dosyalarını bir dış dosya paylaşımında aktarır.

Uç nokta oturumunu kapatmak için:

1. PEP tarafından erişilebilen bir dış dosya paylaşma oluşturun. Bir geliştirme seti ortamında, yalnızca geliştirme seti ana bilgisayarında bir dosya paylaşma oluşturabilirsiniz.
2. Aşağıdaki cmdlet'i çalıştırın: 
     ```powershell
     Close-PrivilegedEndpoint -TranscriptsPathDestination "\\fileshareIP\SharedFolder" -Credential Get-Credential
     ```
   , aşağıdaki tablodaki parametreleri kullanır.

   | Parametre | Açıklama | Type | Gerekli |
   |---------|---------|---------|---------|
   | *TranscriptsPathDestination* | "Fileshareıp\shareklasöradı" olarak tanımlanan dış dosya paylaşımının yolu | Dize | evet|
   | *Kimlik bilgisi* | dosya paylaşımıyla erişim için kimlik bilgileri | SecureString |   evet |


Döküm dosyası günlük dosyaları başarıyla dosya paylaşımında aktarıldıktan sonra, PEP 'den otomatik olarak silinir. 

> [!NOTE]
> Cmdlet 'lerini `Exit-PSSession` `Exit`kullanarak Pep oturumunu kapatır veya yalnızca PowerShell konsolunu kapatırsanız, döküm günlükleri bir dosya paylaşımında aktarılmaz. Bunlar PEP içinde kalır. Bir sonraki çalıştırışınızda `Close-PrivilegedEndpoint` ve bir dosya paylaşımının dahil olması halinde, önceki oturumlardan gelen döküm günlükleri de aktarılır. Pep oturumunu `Exit-PSSession` kapatmak `Exit` için veya kullanmayın; bunun yerine kullanın `Close-PrivilegedEndpoint` .


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack tanılama araçları](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep)
