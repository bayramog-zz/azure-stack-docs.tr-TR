---
title: Azure Stack bir Ethereum blok zinciri ağı dağıtma | Microsoft Docs
description: Azure Stack bir konsorsiyumum blok zinciri ağını dağıtmak ve yapılandırmak için özel çözüm şablonları kullanma öğreticisi.
services: azure-stack
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 06/03/2019
ms.topic: tutorial
ms.service: azure-stack
ms.reviewer: seyadava
ms.custom: mvc
manager: femila
ms.lastreviewed: 06/03/2019
ms.openlocfilehash: b68a6df35b5345d3e1f00be126cdae24e87d3d0b
ms.sourcegitcommit: 637018771ac016b7d428174e88d4dcb131b54959
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842974"
---
# <a name="deploy-an-ethereum-blockchain-network-on-azure-stack"></a>Azure Stack bir Ethereum blok zinciri ağı dağıtma

Ethereum çözüm şablonu, çok kullanıcılı bir konsorsiyum blok zinciri ağını daha kolay ve hızlı bir şekilde dağıtmayı ve yapılandırmayı kolaylaştırmak için tasarlanmıştır.

Azure Stack Kiracı portalı aracılığıyla Kullanıcı girişleri ve tek tıklamayla dağıtım ile her üye, kendi ağ parmak izini sağlayabilir. Her üyenin ağ parmak izi üç adımdan oluşur:

1. Bir uygulamanın veya kullanıcının işlemleri göndermek için etkileşime girebileceği, yük dengeli bir işlem düğümleri kümesi.
2. İşlemleri kaydetmek için bir araştırma düğümleri kümesi.
3. Bir ağ sanal gereci (NVA).

Daha sonraki bir bağlantı adımı, tam olarak yapılandırılmış bir çoklu üye blok zinciri ağı oluşturmak için NVA 'lar 'e bağlanır.

Ayarlamak için:

- Bir dağıtım mimarisi seçin.
- Tek başına, konsorsiyumun veya konsorsiyum üye ağını dağıtın.

## <a name="prerequisites"></a>Önkoşullar

[Market 'ten](../operator/azure-stack-download-azure-marketplace-item.md)en son öğeleri indirin:

- Ubuntu Server 16,04 LTS
- Windows Server 2016
- Linux 2,0 için özel betik
- Windows için özel Betik uzantısı

Blok zinciri senaryoları hakkında daha fazla bilgi için bkz. [Ethereum yetki kanıtlama için bir çözüm şablonu](/azure/blockchain/templates/ethereum-poa-deployment).

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Bu çözüm şablonu, tek veya çok üyeli bir Ethereum Konsorsiyumu ağını dağıtabilir. Sanal ağ, ağ sanal Gereç ve bağlantı kaynakları kullanılarak bir zincir topolojisine bağlanır.

## <a name="deployment-use-cases"></a>Dağıtım kullanım örnekleri

Şablon, öncü ve üye için çeşitli yollarla Ethereum Konsorsiyumu dağıtabilir. Test ettikleriniz şunlardır:

- Azure AD veya AD FS ile çok düğümlü Azure Stack, aynı abonelik veya farklı aboneliklerle bir müşteri adayı ve üye dağıtın.
- Tek düğümlü bir Azure Stack (Azure AD ile), aynı aboneliği kullanarak lider ve üye dağıtın.

### <a name="standalone-and-consortium-leader-deployment"></a>Tek başına ve konsorsiyum öncü dağıtımı

Konsorsiyum öncü şablonu, ağdaki ilk üyenin parmak izini yapılandırır. 

1. [GitHub 'dan öncü şablonu](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/ConsortiumLeader/mainTemplate.json)indirin.
2. Azure Stack kiracı portalında + bir özel şablondan dağıtmak için **+ kaynak oluştur > Şablon dağıtımı** ' ı seçin.
3. Yeni özel şablonu düzenlemek için **Şablonu Düzenle** ' yi seçin.
4. Sağdaki düzen bölmesinde, daha önce indirdiğiniz öncü şablon JSON ' ı kopyalayıp yapıştırın.
    
    ![Öncü şablonu Düzenle](./media/azure-stack-ethereum/edit-leader-template.png)

5. **Kaydet**’i seçin.
6. **Parametreleri Düzenle** ' yi seçin ve dağıtımınız için şablon parametrelerini doldurun.
    
    ![Öncü şablon parametrelerini Düzenle](./media/azure-stack-ethereum/edit-leader-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Dağıtılan kaynakları adlandırmak için temel olarak kullanılan dize. | 1 ile 6 uzunluğunda alfasayısal karakterler. | leri
    AUTHTYPE | VM 'de kimlik doğrulama yöntemi. | Parola veya SSH ortak anahtarı. | istemcisiyle yönetilen bir cihaz için)
    ADMINUSERNAME | Dağıtılan her VM 'nin Yönetici Kullanıcı adı. | 1-64 karakter. | gethadmin
    ADMINPASSWORD (kimlik doğrulama türü = parola)| Dağıtılan VM 'lerin her biri için yönetici hesabının parolası. Parolanın aşağıdaki gereksinimlerin 3 ' ü içermesi gerekir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm VM 'Ler başlangıçta aynı parolaya sahip olsa da, sağlama sonrasında parolayı değiştirebilirsiniz.|12-72 karakter. |
    ADMINSSHKEY (kimlik doğrulama türü = sshPublicKey) | Uzaktan oturum açma için kullanılan güvenli kabuk anahtarı. | |
    GENESISBLOCK | Özel genesare bloğunu temsil eden JSON dizesi.  Bu parametre için bir değer belirtilmesi isteğe bağlıdır. | |
    ETHEREUMACCOUNTPSSWD | Ethereum hesabını güvenli hale getirmek için kullanılan yönetici parolası. | |
    ETHEREUMACCOUNTPAROLA | Ethereum hesabıyla ilişkili özel anahtar oluşturmak için kullanılan parola. | |
    ETHEREUMNETWORKID | Konsorsiyumun ağ KIMLIĞI. | 5 ile 999.999.999 arasında bir değer kullanın. | 72
    CONSORTIUMÜYEKODU | Konsorsiyum ağının her üyesiyle ilişkili KIMLIK.   | Bu KIMLIK ağda benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğümlerinin sayısı. | 2 ile 15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerinin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Araştırma düğümlerinin depolama performansı. | | Standard_LRS
    NUMTXNODES | İşlem düğümlerinin sayısı. | 1 ile 5 arasında. | 1\.
    TXNODEVMSIZE | İşlem düğümlerinin VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | İşlem düğümlerinin depolama performansı. | | Standard_LRS
    BASEURL | Dağıtım şablonlarını almak için temel URL. | Dağıtım şablonlarını özelleştirmek istemediğiniz müddetçe varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. **Özel dağıtımda** **abonelik**, **kaynak grubu**ve **kaynak grubu konumunu**belirtin.
    
    ![Öncü dağıtım parametreleri](./media/azure-stack-ethereum/leader-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Subscription | Consortium ağının dağıtılacağı abonelik. | | Tüketim aboneliği
    Kaynak Grubu | Consortium ağının dağıtılacağı kaynak grubu. | | EthereumResources
    Location | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

Dağıtım tamamlandıktan sonra, kaynak grubunun dağıtım bölümünde **Microsoft. Template** dağıtım özetini gözden geçirin. Özet, konsorsiyum üyelerini birleştirmek için kullanılan çıktı değerlerini içerir.

Öncü dağıtımı doğrulamak için öncü yönetim sitesine gidin. Yönetim sitesi adresi, **Microsoft. Template** dağıtımının çıkış bölümünde bulunur.  

![Öncü dağıtım Özeti](./media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>Konsorsiyumun üye dağıtımını birleştirme

1. GitHub ' [dan konsorsiyum üye şablonunu](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/JoiningMember/mainTemplate.json)indirin.
2. Azure Stack kiracı portalında + bir özel şablondan dağıtmak için **+ kaynak oluştur > Şablon dağıtımı** ' ı seçin.
3. Yeni özel şablonu düzenlemek için **Şablonu Düzenle** ' yi seçin.
4. Sağdaki düzen bölmesinde, daha önce indirdiğiniz öncü şablon JSON 'sini kopyalayıp yapıştırın.
5. **Kaydet**’i seçin.
6. **Parametreleri Düzenle** ' yi seçin ve dağıtımınız için şablon parametrelerini doldurun.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Dağıtılan kaynakları adlandırmak için temel olarak kullanılan dize. | 1 ile 6 uzunluğunda alfasayısal karakterler. | leri
    AUTHTYPE | VM 'de kimlik doğrulama yöntemi | Parola veya SSH ortak anahtarı. | istemcisiyle yönetilen bir cihaz için)
    ADMINUSERNAME | Dağıtılan her VM 'nin Yönetici Kullanıcı adı | 1-64 karakter. | gethadmin
    ADMINPASSWORD (kimlik doğrulama türü = parola)| Dağıtılan VM 'lerin her biri için yönetici hesabının parolası. Parolanın aşağıdaki gereksinimlerin 3 ' ü içermesi gerekir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm VM 'Ler başlangıçta aynı parolaya sahip olsa da, sağlama sonrasında parolayı değiştirebilirsiniz.|12-72 karakter. |
    ADMINSSHKEY (kimlik doğrulama türü = sshPublicKey) | Uzaktan oturum açma için kullanılan güvenli kabuk anahtarı. | |
    CONSORTIUMÜYEKODU | Konsorsiyum ağının her üyesiyle ilişkili KIMLIK.   | Bu KIMLIK ağda benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğümlerinin sayısı. | 2 ile 15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerinin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Araştırma düğümlerinin depolama performansı. | | Standard_LRS
    NUMTXNODES | İşlem düğümlerinin sayısı. | 1 ile 5 arasında. | 1\.
    TXNODEVMSIZE | İşlem düğümlerinin VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | İşlem düğümlerinin depolama performansı. | | Standard_LRS
    CONSORTIUMDATA | Başka bir üyenin dağıtımı tarafından sunulan ilgili konsorsiyum yapılandırma verilerine işaret eden URL. Bu değer, öncü dağıtım çıkışında bulunabilir. | |
    REMOTEMEMBERVNETADDRESSSPACE | Önün NVA IP adresi. Bu değer, öncü dağıtım çıkışında bulunabilir. | | 
    REMOTEMEMBERNVAPUBLICIP | Önün NVA IP adresi. Bu değer, öncü dağıtım çıkışında bulunabilir. | | 
    CONNECTIONSHAREDKEY | Bir bağlantı kurmaya yönelik konsorsiyum ağı üyeleri arasında önceden oluşturulan bir gizli dizi. | |
    BASEURL | Şablonun temel URL 'SI. | Dağıtım şablonlarını özelleştirmek istemediğiniz müddetçe varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. **Özel dağıtımda** **abonelik**, **kaynak grubu**ve **kaynak grubu konumunu**belirtin.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Subscription | Consortium ağının dağıtılacağı abonelik. | | Tüketim aboneliği
    Kaynak Grubu | Consortium ağının dağıtılacağı kaynak grubu. | | MemberResources
    Location | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

Dağıtım tamamlandıktan sonra, kaynak grubunun dağıtım bölümünde **Microsoft. Template** dağıtım özetini gözden geçirin. Özet, konsorsiyum üyelerini bağlamak için kullanılan çıkış değerlerini içerir.

Üyenin dağıtımını doğrulamak için üyenin yönetim sitesine gidin. Yönetim sitesi adresini **Microsoft. Template** dağıtımının çıkış bölümünde bulabilirsiniz.

![Üye dağıtım Özeti](./media/azure-stack-ethereum/ethereum-node-status-2.png)

Resimde gösterildiği gibi, üyenin düğümlerin durumu **çalışmıyor**. Bu durum, üye ve öncü arasındaki bağlantının kurulu olmaması nedeniyle oluşur. Üye ve öncü arasındaki bağlantı iki yönlü bir bağlantıdır. Üye dağıttığınızda, şablon otomatik olarak üyenin bağlantısını lider olarak oluşturur. Liderden üyeye bağlantı oluşturmak için bir sonraki adıma gidin.

### <a name="connect-member-and-leader"></a>Üye ve öncü bağla

Bu şablon, önyüklemeden uzak üyeye bir bağlantı oluşturur. 

1. [Üye ve öncü şablonunu GitHub 'dan](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/Connection/mainTemplate.json)indirin.
2. Azure Stack kiracı portalında + bir özel şablondan dağıtmak için **+ kaynak oluştur > Şablon dağıtımı** ' ı seçin.
3. Yeni özel şablonu düzenlemek için **Şablonu Düzenle** ' yi seçin.
4. Sağdaki düzen bölmesinde, daha önce indirdiğiniz öncü şablon JSON 'sini kopyalayıp yapıştırın.
    
    ![Bağlama şablonunu Düzenle](./media/azure-stack-ethereum/edit-connect-template.png)

5. **Kaydet**’i seçin.
6. **Parametreleri Düzenle** ' yi seçin ve dağıtımınız için şablon parametrelerini doldurun.
    
    ![Bağlanma şablonu parametrelerini Düzenle](./media/azure-stack-ethereum/edit-connect-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Öncü adının ön eki. Bu değer, öncü dağıtım çıkışında bulunabilir.  | 1 ile 6 uzunluğunda alfasayısal karakterler. | |
    MEMBERROUTETABLENAME | Öncü yol tablosunun adı. Bu değer, öncü dağıtım çıkışında bulunabilir. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | Üyenin adres alanı. Bu değer, üyenin dağıtım çıkışında bulunabilir. | |
    CONNECTIONSHAREDKEY | Bir bağlantı kurmaya yönelik konsorsiyum ağı üyeleri arasında önceden oluşturulan bir gizli dizi.  | |
    REMOTEMEMBERNVAPUBLICIP | Üyenin NVA IP adresi. Bu değer, üyenin dağıtım çıkışında bulunabilir. | |
    MEMBERNVAPRIVATEIP | Öncü özel NVA IP adresi. Bu değer, öncü dağıtım çıkışında bulunabilir. | |
    KONUM | Azure Stack ortamınızın konumu. | | yerel
    BASEURL | Şablonun temel URL 'SI. | Dağıtım şablonlarını özelleştirmek istemediğiniz müddetçe varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. **Özel dağıtımda** **abonelik**, **kaynak grubu**ve **kaynak grubu konumunu**belirtin.
    
    ![Dağıtım parametrelerini bağla](./media/azure-stack-ethereum/connect-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Subscription | Lider aboneliği. | | Tüketim aboneliği
    Kaynak Grubu | Öncü kaynak grubu. | | EthereumResources
    Location | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtım tamamlandıktan sonra, öncü ve üyenin iletişimin başlatılması birkaç dakika sürer. Dağıtımı doğrulamak için, üyenin yönetici sitesini yenileyin. Üyenin düğümlerinin durumu çalışıyor olmalıdır.

![Dağıtımı doğrulama](./media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Sonraki adımlar

Ethereum ve Azure hakkında daha fazla bilgi edinmek için bkz. [blok zinciri teknolojisi ve uygulamalar](https://azure.microsoft.com/solutions/blockchain/).
