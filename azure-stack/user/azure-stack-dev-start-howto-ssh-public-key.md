---
title: Azure Stack ile SSH ortak anahtarını kullanma | Microsoft Docs
description: SSH ortak anahtarını kullanma
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 3d2854511415421b69a6972cd807132639300f96
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824500"
---
# <a name="use-an-ssh-public-key"></a>SSH ortak anahtarı kullanma

Geliştirme makinenizden Web uygulamanızı barındıran Azure Stack örneğindeki sunucu VM 'sine açık bir SSH bağlantısı kullanmak için, bir Secure Shell (SSH) ortak ve özel anahtar çifti oluşturmanız gerekebilir. 

Bu makalede, anahtarlarınızı oluşturup sunucunuza bağlanmak için kullanacaksınız. Linux sunucusunda bir bash istemi almak için bir SSH istemcisi kullanabilir veya bir güvenli FTP (SFTP) istemcisini kullanarak dosyaları sunucuya ve sunucudan taşıyabilirsiniz.

## <a name="create-an-ssh-public-key-on-windows"></a>Windows 'da SSH ortak anahtarı oluşturma

Bu bölümde, Azure Stack örneğinizdeki Linux makinelere güvenli bir bağlantı oluştururken kullanılacak ortak bir SSH anahtarı ve özel anahtar çifti oluşturmak için PuTTY anahtar oluşturucuyu kullanırsınız. PuTTY, SSH ve Telnet aracılığıyla bir sunucuya bağlanmanıza izin veren ücretsiz bir Terminal öykünücüdür.

1. [Makinenize PuTTY indirin ve yükleyin.](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. PuTTY anahtar oluşturucuyu açın.

    ![Boş anahtar kutusuyla PuTTY anahtar Oluşturucu](media/azure-stack-dev-start-howto-ssh-public-key/001-putty-key-gen-start.png)

1. **Parametreler**altında **RSA**' yı seçin.

1. **Oluşturulan bir anahtardaki bit sayısı** kutusuna **2048**girin.  

1. **Oluştur**' u seçin.

1. **Anahtar** alanında, imleci boş alanın üzerine taşıyarak rastgele karakterler oluşturun.

    ![Doldurulmuş anahtar kutusuyla PuTTY anahtar Oluşturucu](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-key-gen-result.png)

1. Bir **anahtar parolası** girin ve parolayı **Onayla** kutusunda onaylayın. Daha sonra kullanmak üzere parolanızı aklınızda edin.

1. **Ortak anahtarı kaydet**' i seçin ve bunu erişebileceğiniz bir konuma kaydedin.

1. **Özel anahtarı kaydet**' i seçin ve bunu erişebileceğiniz bir konuma kaydedin. Ortak anahtara ait olduğunu unutmayın.

Ortak anahtarınız, kaydettiğiniz metin dosyasında depolanır. Metin aşağıdakine benzer:

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

Bir uygulama anahtarı istediğinde, metin dosyasının tüm içeriğini kopyalayıp yapıştırın.

## <a name="connect-with-ssh-by-using-putty"></a>PuTTY kullanarak SSH ile bağlanma

PuTTY 'yi yüklediğinizde, hem PuTTY anahtar Oluşturucu hem de bir SSH istemcisidir. Bu bölümde, SSH istemcisini, PuTTY 'ı açar ve bağlantı değerlerinizi ve SSH anahtarınızı yapılandırırsınız. Azure Stack örneğiniz ile aynı ağ üzerinde çalışıyorsanız, sanal makinenize bağlanırsınız.

Bağlanmadan önce şunları yapmanız gerekir:
- PuTTY
- Kimlik doğrulama türü olarak SSH ortak anahtarını kullanan Azure Stack örneğindeki Linux makinenin IP adresi ve Kullanıcı adı.
- Makine için 22 numaralı bağlantı noktası açık.
- Makineyi oluştururken kullandığınız ortak SSH anahtarı.
- PuTTY çalıştıran istemci makine, Azure Stack örneğiniz ile aynı ağda olmalıdır.

1. PuTTY’yi açın.

    ![PuTTY yapılandırma bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-connect.png)

2. **Ana bilgisayar adı (veya IP adresi)** kutusuna makinenin Kullanıcı adını ve genel IP adresini girin (örneğin, **username@192.XXX.XXX.XX** ). 
3. **Bağlantı noktasının** **22** olduğunu ve **bağlantı türünün** **SSH**olduğunu doğrulayın.
4. **Kategori** ağacında **SSH** ve **AUTH**' ı genişletin.

    ![PuTTY yapılandırma bölmesi-SSH özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-set-private-key.png)

5. **Kimlik doğrulaması Için özel anahtar dosyası** ' nın yanındaki **Git ' i seçin ve**ardından ortak ve özel anahtar çiftiniz için özel anahtar dosyasını ( *\<filename >. PPK*) arayın.
6. **Kategori** ağacında **oturum**' yi seçin.

    ![PuTTY yapılandırma bölmesi "kayıtlı oturumlar" kutusu](media/azure-stack-dev-start-howto-ssh-public-key/003-puTTY-save-session.png)

7. **Kaydedilen oturumlar**' ın altında, oturum için bir ad girin ve ardından **Kaydet**' i seçin.
8. **Kaydedilen oturumlar** listesinde, oturumunuzun adını seçin ve ardından **Yükle**' yi seçin.
9. **Aç**'ı seçin. SSH oturumu açılır.

## <a name="connect-with-sftp-with-filezilla"></a>FileZilla ile SFTP ile bağlanma

Linux makinenize dosya taşımak için güvenli FTP 'yi (SFTP) destekleyen bir FTP istemcisi olan FileZilla kullanabilirsiniz. FileZilla, Windows 10, Linux ve macOS üzerinde çalışır. FileZilla istemcisi FTP, TLS üzerinden FTP (FTPS) ve SFTP 'yi destekler. GNU Genel genel lisansı koşullarına göre ücretsiz olarak dağıtılan açık kaynaklı bir yazılımdır.

### <a name="set-your-connection"></a>Bağlantınızı ayarlama

1. [FileZilla indirin ve yükleyin](https://filezilla-project.org/download.php).
1. FileZilla 'i açın.
1. @No__t-1**Site Manager** **dosyasını**seçin.

    ![FileZilla Site Manager bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/005-filezilla-file-manager.png)

1. **Protokol** aşağı açılan listesinde **SFTP-SSH dosya aktarım protokolü**öğesini seçin.
1. **Ana bilgisayar** kutusuna MAKINENIZIN genel IP adresini girin.
1. **Oturum açma türü** kutusunda **normal**' i seçin.
1. Kullanıcı adınızı ve parolanızı girin.
1. **Tamam**’ı seçin.
1. @No__t-1**ayarlarını** **Düzenle**' yi seçin.

    ![FileZilla ayarlar bölmesi](media/azure-stack-dev-start-howto-ssh-public-key/006-filezilla-add-private-key.png)

1. **Sayfa seç** ağacında, **bağlantı**' yı genişletin ve ardından **SFTP**' yi seçin.
1. **Anahtar dosyası Ekle**' yi seçin ve ardından özel anahtar dosyanızı (örneğin, *\<filename >. PPK*) girin.
1. **Tamam**’ı seçin.

### <a name="open-your-connection"></a>Bağlantınızı açın

1. FileZilla 'i açın.
1. @No__t-1**Site Manager** **dosyasını**seçin.
1. Sitenizin adını seçin ve ardından **Bağlan**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack bir geliştirme ortamı ayarlamayı](azure-stack-dev-start.md)öğrenin.
