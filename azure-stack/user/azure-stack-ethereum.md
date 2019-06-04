---
title: Azure Stack Ethereum blockchain çözümü şablonu
description: Özel çözüm şablonlarını kullanarak dağıtma ve Azure Stack'te consortium Ethereum blok zinciri ağ yapılandırma Öğreticisi
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
ms.openlocfilehash: 8f173ad486ed7fc1a4dae3c9a1c57f77ca623f0d
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66459086"
---
# <a name="deploy-an-ethereum-blockchain-network-on-azure-stack"></a>Azure Stack'te bir Ethereum blok zinciri ağ dağıtma

Ethereum çözüm şablonu, daha kolay ve hızlı dağıtma ve Azure ve Ethereum minimum bilgi ile çok üye consortium Ethereum blok zinciri ağ yapılandırma olmak için tasarlanmıştır.

Birkaç kullanıcı girişleri ve Azure Stack Kiracı Portalı aracılığıyla tek tıklamayla dağıtım ile üyelerin kendi ağ kaplama alanını sağlayabilirsiniz. Her üyenin ağ kaplama alanını yük dengeli işlem düğümlerinin bir dizi oluşur. sahip olan bir uygulama ya da kullanıcı işlemleri, bir dizi kayıt işlemleri için araştırma düğümü ve bir ağ sanal Gereci (NVA) göndermek için etkileşim kurabilirsiniz. Nva'ların tam olarak yapılandırılmış birden çok üye blockchain ağ oluşturmak için bir sonraki bağlantı adım bağlanır.

Ayarlamak için:

- Dağıtım mimarisi seçin
- Tek başına, consortium öncü ya da consortium üye ağ dağıtma

## <a name="prerequisites"></a>Önkoşullar

En son öğeleri indirmek [marketten](../operator/azure-stack-download-azure-marketplace-item.md):

- Ubuntu Server 16.04 LTS
- Windows Server 2016
- Linux 2.0 için özel betik
- Windows için özel betik uzantısı

Blok zinciri senaryoları hakkında daha fazla bilgi için bkz. [Ethereum yetkilisi kavram consortium çözüm şablonu](/azure/blockchain/templates/ethereum-poa-deployment).

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Bu çözüm şablonu, tek veya çok üye Ethereum Konsorsiyum ağı dağıtabilirsiniz. Sanal ağ, ağ sanal Gereci ve bağlantı kaynakları kullanan bir zincir topolojisinde bağlıdır. 

## <a name="deployment-use-cases"></a>Dağıtım kullanım örneklerinize

Şablonu Ethereum consortium lider ve üye birleştirme çeşitli yollarla dağıtabilirsiniz, Test ettiğimiz olanları şunlardır:

- Sağlama ve üye aynı aboneliği kullanarak Azure AD veya AD FS, bir çok düğümlü Azure Stack'te dağıtın veya farklı abonelikler.
- (Azure AD ile) bir tek düğümlü Azure Stack üzerinde sağlama ve üye aynı aboneliği kullanarak dağıtın.

### <a name="standalone-and-consortium-leader-deployment"></a>Tek başına ve consortium lider dağıtımı

Consortium lider şablonu ağ ilk üyenin ayak izini yapılandırır. 

1. İndirme [lider şablonunu github'dan](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/ConsortiumLeader/mainTemplate.json)
2. Azure Stack Kiracı portalında **+ kaynak Oluştur > şablon dağıtımı** özel bir şablondan dağıtma.
3. Seçin **şablonu Düzen** yeni bir özel şablon düzenlenecek.
4. Düzenleme bölmesinde sağ taraftaki kopyalayın ve daha önce indirdiğiniz JSON lider şablon yapıştırın.
    
    ![Lider Şablonu Düzenle](./media/azure-stack-ethereum/edit-leader-template.png)

5. **Kaydet**’i seçin.
6. Seçin **parametreleri Düzenle** ve dağıtımınız için şablon parametreleri doldurun.
    
    ![Lider şablon parametreleri Düzenle](./media/azure-stack-ethereum/edit-leader-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Temel olarak dağıtılan kaynaklar adlandırmak için kullanılan dize. | Alfasayısal karakterlerle başlayıp uzunluğu 1 ile 6 | Eth
    AUTHTYPE | Sanal makinenin kimliğini doğrulamak için yöntem. | Parola veya SSH ortak anahtarı | Parola
    ADMINUSERNAME | Dağıtılan her VM'nin yönetici kullanıcı adı | 1 - 64 karakter | gethadmin
    ADMINPASSWORD (kimlik doğrulaması türü = parola)| Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm VM'lerin aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|12 - 72 karakter|
    ADMINSSHKEY (kimlik doğrulaması türü sshPublicKey =) | Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı. | |
    GENESISBLOCK | Özel genesis blok temsil eden JSON dizesi.  Bu parametre için bir değer belirterek isteğe bağlıdır. | |
    ETHEREUMACCOUNTPSSWD | Ethereum hesabının güvenliğini sağlamak için kullanılan yönetici parolası. | |
    ETHEREUMACCOUNTPASSPHRASE | Ethereum hesapla ilişkili özel anahtarı oluşturmak için kullanılan parola. | |
    ETHEREUMNETWORKID | Consortium ağ kimliği. | 999,999,999 ile 5 arasındaki herhangi bir değer kullanın | 72
    CONSORTIUMMEMBERID | Her üye Konsorsiyum ağı ile ilişkili kimlik.   | Bu kimlik ağ içinde benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğüm sayısı. | 2 ile 15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Depolama performansı araştırma düğümleri. | | Standard_LRS
    NUMTXNODES | İşlem düğümleri sayısı. | 1 ile 5 arasında. | 1
    TXNODEVMSIZE | İşlem düğümleri VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | İşlem düğümlerinin depolama performansı. | | Standard_LRS
    BASEURL | Bağlı Şablonlar'dan almak için temel URL. | Dağıtım şablonlarını özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.
    
    ![Lider dağıtım parametreleri](./media/azure-stack-ethereum/leader-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Aboneliği Konsorsiyum ağı dağıtmak için | | Tüketim abonelik
    Kaynak Grubu | Hangi Konsorsiyum ağı dağıtmak kaynak grubu. | | EthereumResources
    Location | Kaynak grubu için bir Azure bölgesi. | | Yerel

8. **Oluştur**’u seçin.

Dağıtım, 20 dakika sürebilir veya tamamlanması daha uzun.

Dağıtım tamamlandıktan sonra dağıtım için özeti gözden geçirebilirsiniz **Microsoft. Şablon** kaynak grubunun dağıtım bölümünde. Özet consortium üyeleri katılmak için kullanılan çıkış değerleri içerir.

Lider'ın dağıtımı doğrulamak için lider'ın yönetici sitesi göz atın. Yönetici sitesi adresini çıktı bölümünde bulabilirsiniz **Microsoft.Template** dağıtım.  

![Lider dağıtım özeti](./media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>Consortium üye dağıtım katılma

1. İndirme [github'dan consortium üye şablonu](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/JoiningMember/mainTemplate.json)
2. Azure Stack Kiracı portalında **+ kaynak Oluştur > şablon dağıtımı** özel bir şablondan dağıtma.
3. Seçin **şablonu Düzen** yeni bir özel şablon düzenlenecek.
4. Düzenleme bölmesinde sağ taraftaki kopyalayın ve daha önce indirdiğiniz JSON lider şablon yapıştırın.
5. **Kaydet**’i seçin.
6. Seçin **parametreleri Düzenle** ve dağıtımınız için şablon parametreleri doldurun.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Temel olarak dağıtılan kaynaklar adlandırmak için kullanılan dize. | Alfasayısal karakterlerle başlayıp uzunluğu 1 ile 6 | Eth
    AUTHTYPE | Sanal makinenin kimliğini doğrulamak için yöntem. | Parola veya SSH ortak anahtarı | Parola
    ADMINUSERNAME | Dağıtılan her VM'nin yönetici kullanıcı adı | 1 - 64 karakter | gethadmin
    ADMINPASSWORD (kimlik doğrulaması türü = parola)| Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm VM'lerin aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|12 - 72 karakter|
    ADMINSSHKEY (kimlik doğrulaması türü sshPublicKey =) | Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı. | |
    CONSORTIUMMEMBERID | Her üye Konsorsiyum ağı ile ilişkili kimlik.   | Bu kimlik ağ içinde benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğüm sayısı. | 2 ile 15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Depolama performansı araştırma düğümleri. | | Standard_LRS
    NUMTXNODES | İşlem düğümleri sayısı. | 1 ile 5 arasında. | 1
    TXNODEVMSIZE | İşlem düğümleri VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | İşlem düğümlerinin depolama performansı. | | Standard_LRS
    CONSORTIUMDATA | Başka bir üyesinin dağıtım tarafından sağlanan ilgili consortium yapılandırma verilerini işaret eden URL. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir. | |
    REMOTEMEMBERVNETADDRESSSPACE | Öncü NVA IP adresi. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir. | | 
    REMOTEMEMBERNVAPUBLICIP | Öncü NVA IP adresi. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir. | | 
    CONNECTIONSHAREDKEY | Önceden oluşturulmuş bir gizli dizi arasında bağlantı kurma Konsorsiyum ağı üyeleri. | |
    BASEURL | Şablon için temel URL. | Dağıtım şablonlarını özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Aboneliği Konsorsiyum ağı dağıtmak için | | Tüketim abonelik
    Kaynak Grubu | Hangi Konsorsiyum ağı dağıtmak kaynak grubu. | | MemberResources
    Location | Kaynak grubu için bir Azure bölgesi. | | Yerel

8. **Oluştur**’u seçin.

Dağıtım, 20 dakika sürebilir veya tamamlanması daha uzun.

Dağıtım tamamlandıktan sonra dağıtım için özeti gözden geçirebilirsiniz **Microsoft.Template** kaynak grubunun dağıtım bölümünde. Özet consortium üyeleri bağlanmak için kullanılan çıkış değerleri içerir.

Üyenin dağıtımı doğrulamak için üyenin yönetici sitesi göz atın. Yönetici sitesi adresini Microsoft.Template dağıtım çıktı bölümünde bulabilirsiniz.

![Üye dağıtım özeti](./media/azure-stack-ethereum/ethereum-node-status-2.png)

Resimde gösterildiği gibi üyenin düğüm durumu: **çalışmıyor**. Bu durum, üye ve öncü arasındaki bağlantı kurulmasa olmasıdır. Üye ve öncü arasındaki bağlantıyı iki yönlü bir bağlantıdır. Üye dağıttığınızda şablonu otomatik olarak bağlantı üyesi için öncü oluşturur. Bağlantı öncüsü üye oluşturmak için sonraki adıma gidin.

### <a name="connect-member-and-leader"></a>Üye ve öncü bağlanma

Bu şablon, uzak bir üyesine öncü bir bağlantı oluşturur. 

1. İndirme [üyesi ve öncü şablonunu Github'dan bağlanma](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/Connection/mainTemplate.json)
2. Azure Stack Kiracı portalında **+ kaynak Oluştur > şablon dağıtımı** özel bir şablondan dağıtma.
3. Seçin **şablonu Düzen** yeni bir özel şablon düzenlenecek.
4. Düzenleme bölmesinde sağ taraftaki kopyalayın ve daha önce indirdiğiniz JSON lider şablon yapıştırın.
    
    ![Bağlanma Şablonu Düzenle](./media/azure-stack-ethereum/edit-connect-template.png)

5. **Kaydet**’i seçin.
6. Seçin **parametreleri Düzenle** ve dağıtımınız için şablon parametreleri doldurun.
    
    ![Şablon parametreleri bağlanma Düzenle](./media/azure-stack-ethereum/edit-connect-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Lider'ın adı ön eki. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir.  | Alfasayısal karakterlerle başlayıp uzunluğu 1 ile 6 | |
    MEMBERROUTETABLENAME | Öncü ait yol tablosunun adı. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | Adres alanı üyesi. Bu değeri üyenin dağıtım çıkışı üzerinde bulunabilir. | |
    CONNECTIONSHAREDKEY | Önceden oluşturulmuş bir gizli dizi arasında bağlantı kurma Konsorsiyum ağı üyeleri.  | |
    REMOTEMEMBERNVAPUBLICIP | Üyenin NVA IP adresi. Bu değeri üyenin dağıtım çıkışı üzerinde bulunabilir. | |
    MEMBERNVAPRIVATEIP | Lider'ın özel NVA IP adresi. Bu değer, öncü'nın dağıtım çıkışı üzerinde bulunabilir. | |
    KONUM | Azure Stack ortamınıza konumu. | | Yerel
    BASEURL | Şablon için temel URL. | Dağıtım şablonlarını özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.
    
    ![Dağıtım parametreleri bağlanma](./media/azure-stack-ethereum/connect-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Öncü kişinin abonelik. | | Tüketim abonelik
    Kaynak Grubu | Öncü ait kaynak grubu. | | EthereumResources
    Location | Kaynak grubu için bir Azure bölgesi. | | Yerel

8. **Oluştur**’u seçin.

Dağıtım tamamlandıktan sonra öncü ve üye iletişim başlatmak birkaç dakika sürer. Dağıtımı doğrulamak için üyenin yönetici sitesi yenileyin. Üye düğümlerinin durumunu çalıştırıyor olmalıdır. 

![Dağıtımı doğrulama](./media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Sonraki adımlar

Ethereum ve Azure hakkında daha fazla bilgi için bkz: [blok zinciri teknolojisi ve uygulamaları](https://azure.microsoft.com/solutions/blockchain/).
