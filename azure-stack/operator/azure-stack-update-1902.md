---
title: Azure Stack 1902 güncelleştirmesi | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1902 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
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
ms.topic: article
ms.date: 05/30/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 67e0981d5fbd1d6872c9115b8becd2d5ec8b5d8e
ms.sourcegitcommit: cf9440cd2c76cc6a45b89aeead7b02a681c4628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66469112"
---
# <a name="azure-stack-1902-update"></a>Azure Stack 1902 güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede 1902 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme geliştirmeleri ve düzeltmeleri bu sürümü, Azure Stack için yeni özellikler içerir. Bu makalede ayrıca bu sürümdeki bilinen sorunlara açıklar ve güncelleştirmeyi indirmek için bir bağlantı içerir. Bilinen sorunlar doğrudan güncelleştirme işlemiyle ilgili sorunları ve yapı (yükleme sonrası) ile ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="archived-release-notes"></a>Arşivlenmiş sürüm notları

Gördüğünüz [Azure Stack eski sürümlerinin sürüm notları TechNet Galerisi'ndeki](http://aka.ms/azsarchivedrelnotes). Bu sürüm notları yalnızca başvuru amacıyla sağlanmıştır ve bu sürümleri için destek anlamına gelmediğini arşivlenir. Daha fazla yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1902 güncelleştirmenin yapı numarasıdır **1.1902.0.69**.

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Yüklediğinizden emin olun [en son Azure Stack düzeltme](#azure-stack-hotfixes) Azure Stack için 1902 güncelleştirmeden önce 1901 için.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Azure Stack düzeltmeler

- **1901**: [KB 4500636 - Azure Stack düzeltme 1.1901.5.109](https://support.microsoft.com/help/4500636)
- **1902**: [KB 4500637 - Azure Stack düzeltme 1.1902.3.75](https://support.microsoft.com/help/4500637)

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> 1902 doğrudan yükleyebilirsiniz [1.1901.0.95 veya 1.1901.0.99](azure-stack-update-1901.md#build-reference) 1901 düzeltme yüklemeden serbest bırakın. Ancak, eski yüklediyseniz **1901.2.103** düzeltmeyi yüklemelisiniz yeni [1901.3.105 düzeltme](https://support.microsoft.com/help/4495662) önce 1902 için devam ediliyor.

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin:

    ```powershell
    Test-AzureStack -Include AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

  Varsa `AzsControlPlane` parametresi, dahil edilen zaman **Test AzureStack** olan yürütüldü,'nde aşağıdaki hatayı görürsünüz **Test AzureStack** çıktı: **BAŞARISIZ Azure Stack denetim düzlemi Web siteleri özeti**. Bu belirli hatayı güvenle yok sayabilirsiniz.

- Azure Stack System Center Operations Manager (SCOM) tarafından yönetildiğinde güncelleştirdiğinizden emin olun [Microsoft Azure Stack için Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=55184) sürümüne 1.0.3.11 1902 uygulamadan önce.

- Azure Stack güncelleştirmesi için paket biçimi değiştiğinde **.bin/.exe/.xml** için **.zip/.xml** 1902 sürümünden başlayarak. Bağlı Azure Stack ölçek birimleri ile müşteriler **güncelleştirme kullanılabilir** portalında iletisi. Bağlı olmayan müşterileri, artık yalnızca indirin ve karşılık gelen .xml ile .zip dosyasını içeri aktarın.

<!-- ## New features -->

<!-- ## Fixed issues -->

## <a name="improvements"></a>Geliştirmeleri

- 1902 derleme planlar, teklifler, kotalar ve eklenti planı oluşturmak için Azure Stack Yönetici portalında yeni bir kullanıcı arabirimi sunar. Ekran görüntüleri de dahil daha fazla bilgi için bkz. [planlar, teklifler ve kotalar oluşturma](azure-stack-create-plan.md).

<!-- 1460884    Hotfix: Adding StorageController service permission to talk to ClusterOrchestrator  Add node -->
- Kapasite genişletmesi "Expanding depolama" ölçek birimi durumu "Çalışıyor" için geçiş yaparken bir ekleme düğüm işlemi sırasında güvenilirlik geliştirmeleri.

<!--
1426197 3852583: Increase Global VM script mutex wait time to accommodate enclosed operation timeout    PNU
1399240 3322580: [PNU] Optimize the DSC resource execution on the Host  PNU
1398846 Bug 3751038: ECEClient.psm1 should provide cmdlet to resume action plan instance    PNU
1398818 3685138, 3734779: ECE exception logging, VirtualMachine ConfigurePending should take node name from execution context   PNU
1381018 [1902] 3610787 - Infra VM creation should fail if the ClusterGroup already exists   PNU
-->
- Paket bütünlüğü ve güvenlik yanı sıra, daha kolay yönetim için çevrimdışı alımı geliştirmek için Microsoft güncelleştirme paketinin .exe ve .bin dosyalarından bir .zip dosyası olarak değişti. Yeni biçime ek güvenilirliğini zamanlarda, güncelleştirme hazırlığı kabin neden olabilecek unpacking işlem ekler. Aynı paket biçimi OEM paketlerden güncelleştirmek için de geçerlidir.
- Test AzureStack çalıştırırken Azure Stack operatör deneyimini iyileştirmek için işleçleri artık yalnızca kullanabilirsiniz, "Test-AzureStack-Grup UpdateReadiness" aksine, bir dahil etme deyiminden sonra on ek parametre geçirme.

  ```powershell
    Test-AzureStack -Group UpdateReadiness  
  ```  
  
- Güncelleştirme işlemi sırasında genel güvenilirlik ve çekirdek altyapı hizmetleri kullanılabilirliğini artırmak için güncelleştirme eylemi planının parçası olarak yerel güncelleştirme kaynak sağlayıcısı algılar ve otomatik genel düzeltmeler gerektiğinde çağırır. Genel düzeltme "onarım" iş akışları içerir:

  - Altyapı sanal makineleri için uygun olmayan bir durumda olan ve gerektiğinde bunları onarmayı dener denetleniyor.
  - SQL hizmet sorunları için Denetim planının bir parçası denetleyin ve gerektiğinde bunları onarmaya çalışır.
  - Yazılım yük dengeleyici (SLB) hizmetinin durumunu, Ağ denetleyicisi (NC) bir parçası olarak denetleyin ve gerektiğinde bunları onarmaya çalışır.
  - Ağ denetleyicisi (NC) hizmet durumunu denetleyin ve gerektiğinde onarmaya çalışır
  - Acil Durum Kurtarma Konsolu hizmeti (ERCS) service fabric düğümleri durumunu denetleyin ve bunları gerektiği gibi onarın.
  - Altyapı rolü durumunu denetleyin ve gerektiğinde onarın.
  - Azure tutarlı depolama (ACS) service fabric düğümleri durumunu denetleyin ve bunları gerektiği gibi onarın.

<!-- 
1426690 [SOLNET] 3895478-Get-AzureStackLog_Output got terminated in the middle of network log   Diagnostics
1396607 3796092: Move Blob services log from Storage role to ACSBlob role to reduce the log size of Storage Diagnostics
1404529 3835749: Enable Group Policy Diagnostic Logs    Diagnostics
1436561 Bug 3949187: [Bug Fix] Remove AzsStorageSvcsSummary test from SecretRotationReadiness Test-AzureStack flag  Diagnostics
1404512 3849946: Get-AzureStackLog should collect all child folders from c:\Windows\Debug   Diagnostics 
-->
- Azure'a geliştirmeleri günlük koleksiyonu güvenilirlik ve performansını geliştirmek için tanılama araçları yığın. Ek ağ iletişimi ve kimlik Hizmetleri için günlüğe kaydetme. 

<!-- 1384958    Adding a Test-AzureStack group for Secret Rotation  Diagnostics -->
- Test geliştirmeleri güvenilirlik, Test AzureStack gizli döndürme hazırlığı için.

<!-- 1404751    3617292: Graph: Remove dependency on ADWS.  Identity -->
- Müşterinin Active Directory ortam ile iletişim kurarken AD Graph güvenilirliğini artırmaya yönelik geliştirmeler

<!-- 1391444    [ISE] Telemetry for Hardware Inventory - Fill gap for hardware inventory info   System info -->
- Get-AzureStackStampInformation içindeki donanım envanteri koleksiyonunu geliştirmeleri.

- ERCS altyapısı üzerinde çalışan işlemlerinin güvenilirliğini artırmak için 12 GB ile 8 GB ile her ERCS örneği için bellek artırır. Azure Stack tümleşik sistemleri yüklemede, bu 12 GB Boyutundaki artış genel sonuçlanır.

<!-- 110303935 IcM Reported by HKEX -->
- 1902 denetleyicileri VSwitch belirli bir düğümdeki tüm sanal makineleri çevrimdışına geçtiğindeki alanında ağ hizmeti, bir sorunu giderir.  Sorun nerede birincil iletişim kurulamıyor ancak rol üzerinden yalnızca Microsoft Destek Hizmetleri ile irtibat tarafından çözümlenemedi, başka bir, sağlıklı örneğine devredildikten değil bir birincil kaybı durumunda takılı kalmasına neden.

> [!IMPORTANT]
> Azure Stack damganız 12 GB'den fazla kullanılabilir alana sahip olduğundan emin olun düzeltme eki ve güncelleştirme işleminin sonuçlarını en az miktarda Kiracı kapalı kalma süresi emin olmak için **kapasite** dikey penceresi. Bu bellek, yansıtılan artırmak gördüğünüz **kapasite** dikey penceresinde güncelleştirmenin başarıyla yüklenmesini sonra.

## <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı

Bu güncelleştirme, aşağıdaki güvenlik güncelleştirmeleri yükler:  
- [ADV190005](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190006)
- [CVE 2019 0595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0595)
- [CVE 2019 0596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0596)
- [CVE 2019 0597](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0597)
- [CVE 2019 0598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0598)
- [CVE 2019 0599](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0599)
- [CVE 2019 0600](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0600)
- [CVE 2019 0601](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0601)
- [CVE 2019 0602](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0602)
- [CVE 2019 0615](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0615)
- [CVE 2019 0616](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0616)
- [CVE 2019 0618](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0618)
- [CVE 2019 0619](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0619)
- [CVE 2019 0621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0621)
- [CVE 2019 0623](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0623)
- [CVE 2019 0625](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0625)
- [CVE 2019 0626](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0626)
- [CVE 2019 0627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0627)
- [CVE 2019 0628](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0628)
- [CVE 2019 0630](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0630)
- [CVE 2019 0631](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0631)
- [CVE 2019 0632](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0632)
- [CVE 2019 0633](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0633)
- [CVE 2019 0635](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0635)
- [CVE 2019 0636](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0636)
- [CVE 2019 0656](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0656)
- [CVE 2019 0659](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0659)
- [CVE 2019 0660](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0660)
- [CVE 2019 0662](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0662)
- [CVE 2019 0663](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0663)

Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalelerine bakın [4487006](https://support.microsoft.com/en-us/help/4487006).

## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Azure Stack güncelleştirmesi yüklenmeye çalışılırken olmadığında güncelleştirme durumu ve durumuna değiştirin **PreparationFailed**. Bu, düzgün bir şekilde işlemek için bir iç altyapı paylaşımına depolama kapsayıcısından dosya aktarımı erişememe güncelleştirme kaynağı sağlayıcısı tarafından (URP) kaynaklanır. 1901 (1.1901.0.95) sürümünden itibaren kullanarak bu sorunu geçici olarak tıklayarak çalışabilirsiniz **Şimdi Güncelleştir** yeniden (değil **sürdürme**). URP ardından önceki girişim dosyalarından temizler ve indirmeyi yeniden başlatır.

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md), temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz `Error - Template for FaultType UserAccounts.New is missing.` Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra uyarıları otomatik olarak kapanır.

## <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

- Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için [düzeltmeleri](#hotfixes), hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  

- Rest şifreleme anahtarları, veri almak ve bunları Azure Stack dağıtımınıza dışında güvenli bir şekilde depolayın. İzleyin [anahtarlarını almak yönergeler](azure-stack-security-bitlocker.md).

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2931230 - IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- 3557860 - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- 1663805 - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici çözüm olarak, [izinleri doğrulamak için PowerShell](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

<!-- Daniel 3/28 -->
- Bir depolama hesabındaki bir bloba gidin ve açmaya çalıştığınızda, Kullanıcı Portalı'nda **erişim ilkesi** Gezinti ağacından sonraki pencereyi yüklenemiyordur. Bu sorunu geçici olarak çözmek için aşağıdaki PowerShell cmdlet'lerini oluşturma, alma, ayarlama ve erişim ilkeleri, sırasıyla silme etkinleştir:

  - [New-AzureStorageContainerStoredAccessPolicy](/powershell/module/azure.storage/new-azurestoragecontainerstoredaccesspolicy)
  - [Get-AzureStorageContainerStoredAccessPolicy](/powershell/module/azure.storage/get-azurestoragecontainerstoredaccesspolicy)
  - [Set-AzureStorageContainerStoredAccessPolicy](/powershell/module/azure.storage/set-azurestoragecontainerstoredaccesspolicy)
  - [Remove-AzureStorageContainerStoredAccessPolicy](/powershell/module/azure.storage/remove-azurestoragecontainerstoredaccesspolicy)

<!-- ### Health and monitoring -->

### <a name="compute"></a>İşlem

- Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   Bir VM'de önyükleme tanılamalarını etkinleştirir, ancak önyükleme tanılama depolama hesabınızı silerseniz bu hata oluşur. Bu sorunu çözmek için önceden kullanılmış şekilde aynı ada sahip depolama hesabını yeniden oluşturun.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi veya markette dağıtımdan önce indirdiğiniz başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanma işleci.  

<!-- TBD - IS ASDK --> 
- Güncelleştirme 1902 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Seçin **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)** , doğrulayın **Azure Stack - yönetilen Disk** listelenir.
   - Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

- Bir Ubuntu 18.04 etkinleştirilmiş SSH yetkilendirme ile oluşturulan VM, oturum açmak için SSH anahtarları kullanmak izin vermez. Geçici bir çözüm olarak VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için kullanmak veya parola tabanlı kimlik doğrulaması kullanın.

- Bir ölçek kümesi kaldırılamıyor **sanal makine ölçek kümeleri** dikey penceresi. Geçici çözüm olarak, Ölçek kümesini kaldırmak isteyip istemediğiniz seçin ardından **Sil** düğmesini **genel bakış** bölmesi.

- 3 hata etki alanı'bir kullanılabilirlik kümesindeki Vm'leri oluşturma ve oluşturma bir sanal makine ölçek kümesi örneği başarısız oluyor bir **FabricVmPlacementErrorUnsupportedFaultDomainSize** bir 4 düğümlü Azure Stack'te güncelleştirme işlemi sırasında hata oluştu ortam. Bir kullanılabilirlik ile 2 hata etki alanı başarıyla ayarlandı tek VM'ler oluşturabilirsiniz. Bununla birlikte, Ölçek kümesi örnek oluşturma bir 4 düğümlü Azure Stack'te güncelleştirme işlemi sırasında hala kullanılamıyor.

### <a name="networking"></a>Ağ  

<!-- 3239127 - IS, ASDK -->
- Azure Stack portalında bir VM örneğine iliştirilmiş bir ağ bağdaştırıcısına bağlı bir IP yapılandırması için statik bir IP adresi değiştirdiğinizde bildiren bir uyarı iletisi görürsünüz 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`.

    Bu iletiyi güvenle yoksayabilirsiniz; sanal makine örneği yeniden başlatma olsa bile IP adresi değişir.

<!-- 3632798 - IS, ASDK -->
- Portalda, bir gelen güvenlik kuralı ekleyin ve seçerseniz, **hizmet etiketi** çeşitli seçenekler görüntülenir, kaynak olarak **kaynak etiketi** Azure Stack için kullanılabilir değil bir listesi. Azure Stack'te geçerli yalnızca seçenekleri aşağıdaki gibidir:

  - **Internet**
  - **VirtualNetwork**
  - **AzureLoadBalancer**
  
  Diğer seçenekler olarak Azure Stack'te kaynak etiketleri desteklenmez. Benzer şekilde, bir giden güvenlik kuralı ekleyin ve seçin, **hizmet etiketi** hedef aynı seçeneklerinin listesi olarak **kaynak etiketi** görüntülenir. Aynı yalnızca geçerli seçenekler şunlardır **kaynak etiketi**önceki listede açıklandığı gibi.

- Azure Stack'te aynı şekilde olarak genel Azure ağ güvenlik grupları (Nsg'ler) çalışmaz. Azure'da birden çok bağlantı noktası üzerinde bir NSG kuralı ayarlayabilirsiniz (kullanarak portal, PowerShell ve Resource Manager şablonlarını). Azure Stack'te ancak Şirket portalı aracılığıyla bir NSG kuralı birden çok bağlantı noktası ayarlanamıyor. Bu sorunu çözmek için bu ek kuralları ayarlamak için bir Resource Manager şablonu veya PowerShell kullanın.

<!-- 3203799 - IS, ASDK -->
- Azure Stack, 4'ten fazla ağ arabirimlerini (NIC'ler) bir sanal makine örneğine Bugün, örnek boyutu ne olursa olsun bağlanmasını desteklemez.

- Kullanıcı portalında eklemeye çalışırsa bir **arka uç havuzu** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...**  Bu sorunu çözmek için arka uç havuzuna bir yük dengeleyici kaynağı ile ilişkilendirmek için PowerShell, CLI veya Azure Resource Manager şablonu kullanın.

- Oluşturmayı denerseniz, Kullanıcı Portalı'nda bir **gelen NAT kuralı** için bir **yük dengeleyici**, işlem hata iletisiyle başarısız **yük dengeleyici güncelleştirilemedi...**  Bu sorunu çözmek için arka uç havuzuna bir yük dengeleyici kaynağı ile ilişkilendirmek için PowerShell, CLI veya Azure Resource Manager şablonu kullanın.

- Kullanıcı Portalı'nda **Load Balancer oluşturma** penceresi oluşturma seçeneğiniz gösterir bir **standart** yük dengeleyici SKU. Bu seçenek, Azure Stack'te desteklenmiyor.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- İlk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısını kaydetmeniz gerekir.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

### <a name="syslog"></a>Syslog 

- Syslog yapılandırmasını syslog istemci yapılandırmasını ve iletilen durdurmak için syslog iletileri kaybetmenize neden olan bir güncelleştirme döngüsü boyunca kalıcı olmaz. Bu sorun, syslog istemci (1809) genel kullanım tüm Azure Stack sürümleri için geçerlidir. Bu sorunu çözmek için bir Azure Stack güncelleştirme uygulandıktan sonra syslog istemci yeniden yapılandırın.

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1902 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload). 

Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Daha fazla bilgi için [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
