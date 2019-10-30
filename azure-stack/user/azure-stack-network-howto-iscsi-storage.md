---
title: Azure Stack ile Iscsı depolamaya bağlanma | Microsoft Docs
description: Azure Stack ile Iscsı depolamaya bağlanmayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: bed928bdd8ed7c521bd95ec005baafd42eb93047
ms.sourcegitcommit: 58e1911a54ba249a82fa048c7798dadedb95462b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73064806"
---
# <a name="how-to-connect-to-iscsi-storage-with-azure-stack"></a>Azure Stack ile Iscsı depolamaya bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makaledeki şablonu kullanarak, bir Azure Stack sanal makinesini (VM), Azure Stack dışında barındırılan depolamayı ve veri merkezinizde başka bir yerde barındırılan depolama alanını kullanacak şekilde ayarlayabilirsiniz. Bu makale, Iscsı hedefi olarak bir Windows makinesi kullanmaya bakar.

Şablonu [Azure akıllı kenar desenleri](https://github.com/lucidqdreams/azure-intelligent-edge-patterns) GitHub deposunun **lucidqdreams** çatalından bulabilirsiniz. Şablon, **depolama-iSCSI** klasöründedir. Şablon, bir Iscsı hedefine bağlanmak için Azure Stack tarafında gereken altyapıyı ayarlamak üzere tasarlanmıştır. Bu, eşlik eden VNet, NSG, PıP ve depolama alanı ile birlikte Iscsı Başlatıcısı görevi görecek bir sanal makine içerir. Şablon dağıtıldıktan sonra, yapılandırmayı gerçekleştirmek için iki PowerShell betiğin olması gerekir. Bir betik, şirket içi VM 'de (hedef) çalıştırılır ve biri Azure Stack VM 'de (Başlatıcı) çalıştırılır. Bu işlem tamamlandıktan sonra, Azure Stack sanal makinenize şirket içi depolama alanı eklenir. 

## <a name="overview"></a>Genel Bakış

Diyagramda barındırılan bir sanal makine, şirket içi bir Windows makinesinden (fiziksel veya sanal), Azure Stack depolamanın Iscsı protokolü üzerinden Azure Stack barındırılan sanal makinenizin içine bağlanmasını sağlayan bir Iscsı bağlı diski olan Azure Stack üzerinde barındırılan bir VM gösterir.

![alternatif metin](./media/azure-stack-network-howto-iscsi-storage/overview.png)

### <a name="requirements"></a>Gereksinimler

- Windows Server 2016 Datacenter veya Windows Server 2019 Datacenter çalıştıran bir şirket içi makine (fiziksel veya sanal).
- Gerekli Azure Stack Market öğeleri:
    -  Windows Server 2016 Datacenter veya Windows Server 2019 Datacenter (en son derleme önerilir).
    -  PowerShell DSC Uzantısı.
    -  Özel Betik uzantısı.
    -  Var olan bir sanal veya fiziksel makine. İdeal olarak bu makinenin iki ağ bağdaştırıcısı olacaktır. Bu, örneğin SAN gibi başka bir Iscsı hedefi de olabilir.

### <a name="things-to-consider"></a>Dikkate alınması gereken noktalar

- Şablon alt ağına bir ağ güvenlik grubu uygulanır. Bunu gözden geçirin ve gerektiğinde ek izinler yapın.
- Bir RDP reddetme kuralı, bir genel IP adresi aracılığıyla sanal makinelere erişmeyi düşünüyorsanız, bu, tünele NSG 'ye uygulanır ve bu makineye izin ver olarak ayarlanması gerekir.
- Bu çözüm, hesaba DNS çözümlemesi yapmaz.
- Bölümadı ve bölümi parolanızı değiştirmelisiniz. Bölümparolası 12 ile 16 karakter uzunluğunda olmalıdır.
- Bu şablon, Iscsı bağlantısı yapılandırmadaki yerel adresi kullandığında VM için statik bir IP adresi kullanıyor.
- Bu şablon KLG Windows lisansı kullanıyor.
- Ayrıca, Linux tabanlı sistemleri Iscsı hedeflerine bağlayabilirsiniz. Ubuntu belgelerindeki [Iscsı Başlatıcısı](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html) makalesindeki yönergeleri bulabilirsiniz.

### <a name="options"></a>Seçenekler

- **_Artifactslocation** ve **_Artifactslocationsastoken** parametrelerini kullanarak kendi BLOB depolama hesabınızı ve SAS belirtecinizi, SAS belirteci ile kendi depolama Blobun kullanabilme özelliğini kullanabilirsiniz.
- Bu şablon, VNet adlandırma ve IP adresleme için varsayılan değerleri sağlar.
- Bu yapılandırmanın yalnızca Iscsı istemcisinden gelen bir Iscsı NIC 'i vardır. Ayrı alt ağları ve NIC 'Leri kullanmak için birkaç yapılandırmayı test ettik, ancak birden çok ağ geçidi ile ilgili sorunlar yaşıyor ve trafiği yalıtmak için ayrı bir depolama alt ağı oluşturmaya çalışıyor ve gerçekten gerçekten yedekli olacak. 
- Dağıtım başarısız olabileceğinden, bu değerleri geçerli alt ağ ve adres aralıkları içinde tutmaya dikkat edin. 
- PowerShell DSC paketleri birincil amacı, bekleyen yeniden başlatmaları denetlerdedir. Bu DSC, gerekirse daha ayrıntılı şekilde özelleştirilebilir. Daha fazla bilgi için bkz. [Omputermanagementdsc](https://github.com/PowerShell/ComputerManagementDsc/).

### <a name="resource-group-template-iscsi-client"></a>Kaynak grubu şablonu (Iscsı istemcisi)

Diyagramda, Iscsı hedefine bağlanmak için kullanabileceğiniz Iscsı istemcisini oluşturmak üzere şablondan dağıtılan kaynaklar gösterilir. Bu şablon, VM 'yi ve diğer kaynakları dağıtır, Ayrıca, prepare-iSCSIClient. ps1 öğesini çalıştırır ve VM 'yi yeniden başlatır.

![alternatif metin](./media/azure-stack-network-howto-iscsi-storage/iscsi-file-server.png)

### <a name="the-deployment-process"></a>Dağıtım işlemi

Kaynak grubu şablonu, bir sonraki adımın girişi olması amaçlanan çıktıyı oluşturur. Genellikle, Iscsı trafiğinin kaynaklandığı sunucu adına ve Azure Stack genel IP adresine odaklanılmıştır. Bu örnek için:

1. Altyapı şablonunu dağıtın.
2. Veri merkezinizde başka bir yerde barındırılan bir VM 'ye Azure Stack VM dağıtın. 
3. IP adresini ve sunucu adını kullanarak `Create-iSCSITarget.ps1` çalıştırın, bu, bir sanal makine veya fiziksel sunucu olabilen, Iscsı hedefinde betik için yerleşik parametreler olarak şablondan çıkış verir.
4. `Connect-toiSCSITarget.ps1` betiğini çalıştırmak için, Iscsı hedef sunucusunun dış IP adresini veya adreslerini giriş olarak kullanın. 

![alternatif metin](./media/azure-stack-network-howto-iscsi-storage/process.png)

### <a name="inputs-for-azuredeployjson"></a>Azuredeploy. JSON girişleri

|**Parametreler**|**varsayılanını**|**açıklaması**|
|------------------|---------------|------------------------------|
|Windowsımagesku         |2019-veri merkezi   |Lütfen temel Windows VM görüntüsünü seçin
|VMSize                  |Standard_D2_v2    |Lütfen VM boyutunu girin
|VMName                  |Dosya        |VM adı
|adminUsername           |storageadmin      |Yeni sanal makinenin yöneticisinin adı
|adminPassword           |                  |Yeni VM 'lerin yönetici hesabının parolası. Varsayılan değer abonelik KIMLIĞIDIR
|VNetName                |Depolama           |VNet 'in adı. Bu işlem, kaynakları etiketlemek için kullanılacaktır
|VNetAddressSpace        |10.10.0.0/23      |VNet için adres alanı
|Vnetınternalsubnetname  |İç          |VNet Iç alt ağ adı
|VNetInternalSubnetRange |10.10.1.0/24      |VNet Iç alt ağının adres aralığı
|Internalvnetip          |10.10.1.4         |Dosya sunucusunun iç IP 'si için statik adres.
|_artifactsLocation      ||
|_artifactsLocationSasToken||

### <a name="deployment-steps"></a>Dağıtım adımları

1. `azuredeploy.json` kullanarak Iscsı istemci altyapısını dağıtma
2. Şirket içi sunucu Iscsı hedefinde `Create-iSCSITarget.ps1` çalıştırın. Şablon tamamlandığında, ilk adımdaki çıktılar ile şirket içi sunucu Iscsı hedefinde Create-iSCSITarget. ps1 ' yi çalıştırmanız gerekir
3. `Connect-toiSCSITarget.ps1` Iscsı istemcisinde çalıştırın. iSCSI hedefinin ayrıntıları ile Iscsı istemcisindeki Connect-toiSCSITarget. ps1 ' i kaldırın

## <a name="adding-iscsi-storage-to-existing-vms"></a>Mevcut VM 'lere Iscsı depolama ekleme

Ayrıca, Iscsı istemcisinden Iscsı hedefine bağlanmak için mevcut bir sanal makinede betikleri çalıştırabilirsiniz. Bu akış, Iscsı hedefini kendiniz oluşturuyorsanız olur. Bu diyagramda PowerShell betiklerinin yürütme akışı gösterilmektedir. Bu betikler betik dizininde bulunabilir:

![alternatif metin](./media/azure-stack-network-howto-iscsi-storage/script-flow.png)

### <a name="prepare-iscsiclientps1"></a>Prepare-iSCSIClient. ps1

`Prepare-iSCSIClient.ps1` betiği, Iscsı istemcisine önkoşulları yüklüyor; buna dahildir;
- Çok yollu GÇ Hizmetleri yüklemesi
- Iscsı Başlatıcısı hizmetini otomatik olarak ayarlama
- Iscsı için çok yollu MPIO desteğini etkinleştirme
- Tüm Iscsı birimlerinin otomatik olarak kullanımını etkinleştir
- Disk zaman aşımını 60 saniye olarak ayarlayın

Bu önkoşulların yüklenmesinden sonra sistemi yeniden başlatmak önemlidir. MPIO yük dengeleme ilkesi, ayarlanabilmesi için yeniden başlatma gerektirir.

### <a name="create-iscsitargetps1"></a>Create-iSCSITarget. ps1

`Create-iSCSITarget.ps1 `betiği, depolamaya hizmet veren sistemde çalıştırılır. Başlatıcılar tarafından kısıtlanan birden çok disk ve hedef oluşturabilirsiniz. Farklı hedeflere ekleyebileceğiniz birçok sanal disk oluşturmak için bu betiği birden çok kez çalıştırabilirsiniz. Birden çok diski bir hedefe bağlayabilirsiniz. 

|**Girdi**|**varsayılanını**|**açıklaması**|
|------------------|---------------|------------------------------|
|UzakSunucu         |Dosya               |Iscsı hedefine bağlanan sunucunun adı
|Remoteserverıp 'Leri      |1.1.1.1                  |Iscsı trafiğinin geldiği IP adresi
|DiskFolder           |C:\ıbir virtualdisks     |Sanal disklerin depolanacağı klasör ve sürücü
|DiskName             |DiskName                 |Disk VHDX dosyasının adı
|DiskSize             |5 GB                      |VHDX disk boyutu
|TargetName           |RemoteTarget01           |Iscsı istemcisinin hedef yapılandırmasını tanımlamak için kullanılan hedef adı. 
|ChapUsername         |kullanıcı adı                 |CHAP kimlik doğrulaması için Kullanıcı adı
|Bölüpassword         |userP@ssw0rd!            |CHAP kimlik doğrulaması için parola adı. 12 ile 16 karakter arasında olmalıdır

### <a name="connect-toiscsitargetps1"></a>Connect-toiSCSITarget. ps1

`Connect-toiSCSITarget.ps1`, Iscsı istemcisinde çalıştırılan ve iSCSI hedefi tarafından sunulan diski Iscsı istemcisine bağlayan son betiktir.

|**Girdi**|**varsayılanını**|**açıklaması**|
|------------------|---------------|------------------------------|
|Targetıscsiaddresses   |"2.2.2.2","2.2.2.3"    |Iscsı hedefinin IP adresleri
|LocalIPAddresses       |"10.10.1.4"            |Bu, Iscsı trafiğinin geldiği iç IP adresidir.
|LoadBalancePolicy      |C:\ıbir virtualdisks   |Iscsı trafiğinin geldiği IP adresi
|ChapUsername           |kullanıcı adı               |CHAP kimlik doğrulaması için Kullanıcı adı
|Bölüpassword           |userP@ssw0rd!          |CHAP kimlik doğrulaması için parola adı. 12 ile 16 karakter arasında olmalıdır

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  