---
title: Azure Stack ile bir SSH ortak anahtarı kullanmayı | Microsoft Docs
description: SSH ortak anahtarı kullanma
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: c0b55579c5103c7bb1073546243dbfcc0b700b4a
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838383"
---
# <a name="use-an-ssh-public-key"></a>SSH ortak anahtarı kullanma

Barındıran bir SSH bağlantısı açma, geliştirme makinenizde Azure Stack Örneğinizdeki VM sunucusu kullanmak için web uygulamanızı bir güvenli Kabuk (SSH) ortak ve özel anahtar çifti oluşturmak ihtiyacınız olabilecek. 

Bu makalede, anahtarlarınızı oluşturup sunucunuza bağlanmak için kullanabilirsiniz. Linux sunucusu üzerinde bir bash istemi almak veya dosyaları için ve sunucudan taşımak için bir güvenli FTP (SFTP) istemci için bir SSH İstemcisi'ni kullanabilirsiniz.

## <a name="create-an-ssh-public-key-on-windows"></a>Windows üzerinde SSH ortak anahtarı oluşturma

Bu bölümde, bir ortak SSH anahtarının ve Azure Stack Örneğinizde Linux makineleri güvenli bir bağlantı oluşturduğunuzda kullanılacak özel anahtar çifti oluşturmak için PuTTY anahtar Oluşturucu kullanın. PuTTY SSH ve Telnet aracılığıyla bir sunucuya bağlanmasına izin verebilirsiniz ücretsiz bir terminal öykünücü ' dir.

1. [Putty'yi indirin ve makinenize için yükleyin.](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. PuTTY anahtar Oluşturucu açın.

    ![PuTTY anahtar Oluşturucu boş bir anahtar kutusu](media/azure-stack-dev-start-howto-ssh-public-key/001-putty-key-gen-start.png)

1. Altında **parametreleri**seçin **RSA**.

1. İçinde **oluşturulan anahtarı bit sayısı** kutusuna **2048**.  

1. Seçin **oluşturmak**.

1. İçinde **anahtarı** alanı boş alanın üzerine imleci hareket ettirerek bazı rastgele karakterler oluşturur.

    ![PuTTY anahtar Oluşturucu doldurulmuş bir anahtar kutusu](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-key-gen-result.png)

1. Girin bir **anahtarı parola** ve içinde onaylayın **parolayı onayla** kutusu. Daha sonra kullanmak için parolayı not edin.

1. Seçin **ortak anahtarı Kaydet**, erişebileceğiniz da bir konuma kaydedin.

1. Seçin **özel anahtarı Kaydet**, erişebileceğiniz da bir konuma kaydedin. Ortak anahtar ile ait olduğunu unutmayın.

Ortak anahtarınızı kaydettiğiniz metin dosyasında depolanır. Metin aşağıdakine benzer olacaktır:

```text  
---- BEGIN SSH2 PUBLIC KEY ----
Comment: "rsa-key-20190330"
THISISANEXAMPLEDONOTUSE AAAAB3NzaC1yc2EAAAABJQAAAQEAthW2CinpqhXq
9uSa8/lSH7tLelMXnFljSrJIcpxp3MlHlYVbjHHoKfpvQek8DwKdOUcFIEzuStfT
Z8eUI1s5ZXkACudML68qQT8R0cmcFBGNY20K9ZMz/kZkCEbN80DJ+UnWgjdXKLvD
Dwl9aQwNc7W/WCuZtWPazee95PzAShPefGZ87Jp0OCxKaGYZ7UXMrCethwfVumvU
aj+aPsSThXncgVQUhSf/1IoRtnGOiZoktVvt0TIlhxDrHKHU/aZueaFXYqpxDLIs
BvpmONCSR3YnyUtgWV27N6zC7U1OBdmv7TN6M7g01uOYQKI/GQ==
---- END SSH2 PUBLIC KEY ----
```

Bir uygulama anahtarı istediğinde kopyalayın ve tüm içeriğini metin dosyasına yapıştırın.

## <a name="connect-with-ssh-by-using-putty"></a>PuTTY kullanarak SSH ile bağlanma

PuTTY yüklediğinizde, hem PuTTY anahtar oluşturucu ve bir SSH istemcisi vardır. Bu bölümde, SSH istemcisi açın, PuTTY, bağlantı değerlerinin ve SSH anahtarı ve yapılandırma. Azure Stack örneğinizin aynı ağ üzerinde kullanıyorsanız, sanal Makinenize bağlanın.

Bağlanmadan önce ihtiyacınız olacak:
- PuTTY
- Linux makine Örneğinizde SSH ortak anahtarı kimlik doğrulaması türü olarak kullanan Azure Stack için kullanıcı adı ve IP adresi.
- Bağlantı noktası 22 makine için açık olması.
- Makineyi oluştururken kullandığınız genel SSH anahtarı.
- Azure Stack örneğinizin aynı ağ üzerinde olması için PuTTY çalıştıran istemci makine.

1. PuTTY’yi açın.

    ![PuTTY yapılandırma bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-connect.png)

2. İçinde **ana bilgisayar adı (veya IP adresi)** kutusunda, kullanıcı adı ve makinenin genel IP adresi girin (örneğin, **username@192.XXX.XXX.XX**). 
3. Doğrulamak **bağlantı noktası** olduğu **22** ve **bağlantı türü** olduğu **SSH**.
4. İçinde **kategori** ağacında, genişletme **SSH** ve **Auth**.

    ![PuTTY yapılandırma bölmesinde - SSH özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-set-private-key.png)

5. Yanındaki **kimlik doğrulaması için özel anahtar dosyası** kutusunda **Gözat**ve sonra özel anahtar dosyası için arama yapın (*\<filename > .ppk*), genel ve özel anahtar çifti.
6. İçinde **kategori** seçim ağacında **oturumu**.

    ![PuTTY yapılandırma bölmesinde "Kayıtlı oturumlar" kutusu](media/azure-stack-dev-start-howto-ssh-public-key/003-puTTY-save-session.png)

7. Altında **kayıtlı oturumlar**oturumu için bir ad girin ve ardından **Kaydet**.
8. İçinde **kayıtlı oturumlar** listesinde, oturumunuz adını seçin ve ardından **yük**.
9. **Aç**'ı seçin. SSH oturumu açılır.

## <a name="connect-with-sftp-with-filezilla"></a>SFTP FileZilla ile bağlantı kurun

Linux makinenize gelen ve giden dosyaları taşımak için FileZilla, güvenli FTP (SFTP) destekleyen bir FTP istemcisi kullanabilirsiniz. Windows 10, Linux ve Macos'ta FileZilla çalıştırır. FileZilla istemci, FTP, SFTP ve TLS (FTPS) üzerinden FTP destekler. Dağıtılan açık kaynak yazılım olan ücretsiz olarak GNU Genel Kamu Lisansı Koşulları altında.

### <a name="set-your-connection"></a>Bağlantınızı ayarlayın

1. [FileZilla yükleyip](https://filezilla-project.org/download.php).
1. Open FileZilla.
1. Seçin **dosya** > **Site Yöneticisi**.

    ![FileZilla Site Yöneticisi bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/005-filezilla-file-manager.png)

1. İçinde **Protokolü** aşağı açılan listesinden **SFTP - SSH Dosya Aktarım Protokolü**.
1. İçinde **konak** kutusuna, makineniz için genel IP adresini girin.
1. İçinde **oturum açma türü** kutusunda **Normal**.
1. Kullanıcı adı ve parola girin.
1. **Tamam**’ı seçin.
1. Seçin **Düzenle** > **ayarları**.

    ![FileZilla ayarları bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/006-filezilla-add-private-key.png)

1. İçinde **seçin sayfasında** ağacında, genişletme **bağlantı**ve ardından **SFTP**.
1. Seçin **Ekle anahtar dosyası**, özel anahtar dosyanıza girin (örneğin,  *\<filename > .ppk*).
1. **Tamam**’ı seçin.

### <a name="open-your-connection"></a>Bağlantınızı açın

1. Open FileZilla.
1. Seçin **dosya** > **Site Yöneticisi**.
1. Sitenizin adını seçin ve ardından **Connect**.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure Stack geliştirme ortamında ayarlama](azure-stack-dev-start.md).
