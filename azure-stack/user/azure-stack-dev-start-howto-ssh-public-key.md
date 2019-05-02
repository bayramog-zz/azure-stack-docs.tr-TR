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
ms.openlocfilehash: 0f4133052022963e1ed0457dd479a00bcc5bb749
ms.sourcegitcommit: 0d8ccf2a32b08ab9bcbe13d54c7c3dce2379757f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64490057"
---
# <a name="how-to-use-an-ssh-public-key"></a>SSH ortak anahtarı kullanma

Web uygulamanızı barındıran Azure Stack'te geliştirme makineniz ve sunucusu VM'sine açık bir SSH bağlantısı kullanmak için bir SSH ortak ve özel anahtar çifti oluşturmanız gerekebilir. Bu makalede, anahtarları alma ve sunucunuza bağlanmak için anahtarlar kullanılarak adımlarında size kılavuzluk eder. Linux sunucusu üzerinde bir bash istemi almak için bir SSH istemcisi ya da bir SFTP sunucusuna gelen ve giden dosyaları taşıma istemciye kullanabilirsiniz.

## <a name="create-an-ssh-public-key-on-windows"></a>Windows üzerinde SSH ortak anahtarı oluşturma

Bu bölümde PuTTY'ın anahtar Oluşturucu bir ortak SSH anahtarının ve Azure Stack Linux makineleri güvenli bir bağlantı oluşturulurken kullanılacak özel anahtar çifti oluşturmak için kullanın. PuTTY SSH için Telnet Windows ve Unix platformlarında, ücretsiz bir uygulama ile birlikte olan bir `xterm` terminal öykünücü.

1. [Putty'yi indirin ve makinenize için yükleyin.](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. PuTTY anahtar Oluşturucu açın.

1. Ayarlama **parametreleri** için **RSA**.

1. Oluşturulan anahtarı bit sayısı kadar kümesindeki `2048`.  

    ![SSH ortak anahtarı oluşturmak için PuTTY kullanın](media/azure-stack-dev-start-howto-ssh-public-key/001-putty-key-gen-start.png)

1. Seçin **oluşturmak**. İçinde **anahtarı** alanı boş alanın üzerine imleci hareket ettirerek bazı rastgele oluşturur.

1. Ekleme bir **anahtarı parola** ve içinde onaylayın **Onayla** kutusu. Parolanız not edin.
    ![SSH ortak anahtarı oluşturmak için PuTTY kullanın](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-key-gen-result.png)

1. Seçin **ortak anahtarı Kaydet** erişebileceğiniz da bir konuma kaydedin.

1. Seçin **özel anahtarı Kaydet** ve burada erişebilmesi ve ortak anahtar ile ait olduğunu unutmayın bir konuma kaydedin.

Ortak anahtarınızı kaydettiğiniz metin dosyasında depolanır. Bunu açarsanız, şuna benzer bir metin içerir:

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

Ortak anahtarı kullanılırken kopyalayın ve bir uygulama anahtarı istediğinde, metin kutusunun tüm içerikler değeri olarak yapıştırın.

<!-- 
## Create an SSH public key on Linux

ToDo: I need to write this section.

-->
## <a name="connect-with-ssh-using-putty"></a>PuTTY kullanarak SSH ile bağlanma

PuTTY yüklediyseniz, anahtarı Oluşturucu hem bir SSH istemcisi vardır. PuTTY SSH istemcisini açın, SSH anahtarı ve bağlantı değerleri yapılandırın ve Azure Stack ile aynı ağa kullanıyorsanız, sanal Makinenize bağlanın.

Bağlanmadan önce ihtiyacınız olacak:
- PuTTY
- Kimlik doğrulaması türü olarak bir SSH ortak anahtarı kullanır, Azure stack'teki Linux makinesi için kullanıcı adı ve IP adresi.
- Bağlantı noktası 22 makine için açık olması gerekir.
- Makine oluştururken kullanılan genel SSH anahtarı.
- Azure Stack ile aynı ağda olması için PuTTY çalıştıran istemci makinenizde.

### <a name="connect-via-ssh-with-putty"></a>PuTTy SSH aracılığıyla bağlanın

1. Putty'yi açın.

    ![Bağlanmak için PuTTY kullanın](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-connect.png)

2. Kullanıcı adı ve makinenin genel IP adresini ekleyin. Örneğin, `username@192.XXX.XXX.XX` için **ana bilgisayar adı**. 
3. Doğrulamak **bağlantı noktası** `22` ayarlanır ve **bağlantı türü** ayarlanır `SSH`.
4. Genişletin **SSH** > **Auth** içinde **kategori** ağaç.

    ![SSH özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-set-private-key.png)

5. Seçin **Gözat** ve özel anahtar dosyanıza (filename.ppk) ortak ve özel anahtar çifti bulun.
6. Oturum kategori ağacında seçin.

    ![SSH ortak anahtarı ile özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/003-puTTY-save-session.png)

7. Oturumu için bir ad yazın **kayıtlı oturumlar** seçip **Kaydet**.
8. Oturumun adı seçin ve **yük**.
9. **Aç**'ı seçin. SSH oturumu açılır.

## <a name="connect-with-sftp-with-filezilla"></a>SFTP FileZilla ile bağlantı kurun

Filezilla Linux makinenize gelen ve giden dosyaları taşımak için SFTP destekleyen bir FTP istemcisi olarak kullanabilirsiniz. Windows 10, Linux ve Macos'ta FileZilla çalıştırır. FileZilla istemci FTP yanı sıra FTP (FTPS) TLS ve SFTP üzerinden destekler. Açık kaynak yazılım dağıtılmış olan ücretsiz olarak GNU Genel Kamu Lisansı Koşulları altında.

### <a name="set-your-connection"></a>Bağlantınızı ayarlayın

1. [FileZilla yükleyip](https://filezilla-project.org/download.php).
1. Open FileZilla.
1. Seçin **dosya** > **Site Yöneticisi**.

    ![SSH ortak anahtarı ile özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/005-filezilla-file-manager.png)

1. Seçin **SFTP - SSH Dosya Aktarım Protokolü** için **Protokolü**.
1. Makinenizde için genel IP adresi Ekle **konak** kutusu.
1. Seçin **Normal** için **türü oturum**.
1. Kullanıcı adı ve parola ekleyin.
1. **Tamam**’ı seçin.
1. Seçin **Düzenle** > **ayarları**.

    ![SSH ortak anahtarı ile özel anahtarı](media/azure-stack-dev-start-howto-ssh-public-key/006-filezilla-add-private-key.png)

1. Genişletin **bağlantı** içinde **seçin sayfasında** ağaç. Seçin **SFTP**.
1. Seçin **Ekle anahtar dosyası** filename.ppk gibi özel anahtar dosyanıza ekleyin.
1. **Tamam**’ı seçin.

### <a name="open-your-connection"></a>Bağlantınızı açın

1. Open FileZilla.
1. Seçin **dosya** > **Site Yöneticisi**.
1. Sitenizin adını seçip **Connect**.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [Azure Stack için geliştirme](azure-stack-dev-start.md)