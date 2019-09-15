---
title: Yönetici portalından Azure Stack yedeklemeyi etkinleştirme | Microsoft Docs
description: Bir hata oluşursa Azure Stack geri yüklenebilmesi için yönetici portalından Infrastructure Backup hizmetini nasıl etkinleştirebileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 08/21/2019
ms.openlocfilehash: 43d2e79732e065342e3d8c5afdee5dff6b391634
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974766"
---
# <a name="enable-backup-for-azure-stack-from-the-administrator-portal"></a>Yönetici portalından Azure Stack için yedeklemeyi etkinleştir

Azure Stack altyapı yedeklemeleri oluşturabilmesi için yönetici portalından Infrastructure Backup hizmetini etkinleştirebilirsiniz. Donanım ortağı, bu yedeklemeleri, çok [zararlı bir hata](./azure-stack-backup-recover-data.md)durumunda bulut kurtarma kullanarak ortamınızı geri yüklemek için kullanabilir. Bulut kurtarma işleminin amacı, Kurtarma tamamlandıktan sonra işleçlerinizin ve kullanıcılarınızın portala oturum açabilmesini sağlamaktır. Kullanıcılara abonelikleri geri yüklenir, örneğin:

- Rol tabanlı erişim izinleri ve rolleri.
- Orijinal planlar ve teklifler.
- Önceden tanımlı işlem, depolama ve ağ kotaları.
- Gizli dizileri Key Vault.

Ancak Infrastructure Backup hizmeti, IaaS VM 'lerini, ağ yapılandırmasını ve depolama hesapları, Bloblar, tablolar gibi depolama kaynaklarını yedeketmez. Bulut kurtarmasından sonra oturum açan kullanıcılar, önceden varolan kaynakların hiçbirini görmez. Hizmet olarak platform (PaaS) kaynakları ve verileri de hizmet tarafından yedeklenmez.

Yöneticiler ve kullanıcılar, IaaS ve PaaS kaynaklarını altyapı yedekleme işlemlerinden ayrı olarak yedeklemeden ve geri yüklemeden sorumludur. IaaS ve PaaS kaynaklarını yedekleme hakkında bilgi için aşağıdaki bağlantılara bakın:

- [Azure Stack dağıtılan VM 'Leri koruma](../user/azure-stack-manage-vm-protect.md)
- [Uygulamanızı Azure’a yedekleme](https://docs.microsoft.com/azure/app-service/manage-backup)
- [Azure VM 'lerinde SQL Server nedir? Pencerelerin](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>Yedeklemeyi etkinleştirme veya yeniden yapılandırma

1. [Azure Stack Yönetici portalı](azure-stack-manage-portals.md)' nı açın.
2. **Tüm hizmetler**' i seçin ve ardından **Yönetim** kategorisi altında **altyapı yedeklemesi**' ni seçin. **Altyapı yedeklemesi** dikey penceresinde **yapılandırma** ' yı seçin.
3. **Yedekleme depolama konumunun**yolunu yazın. Ayrı bir cihazda barındırılan dosya paylaşımının yolu için bir evrensel adlandırma kuralı (UNC) dizesi kullanın. UNC dizesi, paylaşılan dosyalar veya cihazlar gibi kaynakların konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Bir olağanüstü durum sonrasında yedekleme verilerinin kullanılabilirliğini sağlamak için, cihazın ayrı bir konumda olması gerekir.

    > [!Note]  
    > Ortamınız Azure Stack Altyapı ağından kurumsal ortamınıza ad çözümlemesini destekliyorsa, IP yerine tam etki alanı adını (FQDN) kullanabilirsiniz.

4. Dosyaları okumak ve yazmak için etki alanını ve Kullanıcı adını kullanarak **Kullanıcı adını** yazın. Örneğin: `Contoso\backupshareuser`.
5. Kullanıcının **parolasını** yazın.
6. **Parolayı onaylamak**için parolayı yeniden yazın.
7. **Saat cinsinden sıklık** , yedeklemelerin oluşturulma sıklığını belirler. Varsayılan değer 12 ' dir. Zamanlayıcı en fazla 12 ve en az 4 destekler. 
8. **Gün cinsinden saklama süresi** , dış konumda kaç günlük yedeklemenin korunacağını belirler. Varsayılan değer 7 ' dir. Zamanlayıcı en fazla 14 ve en az 2 destekler. Saklama süresinden daha eski yedeklemeler, dış konumdan otomatik olarak silinir.

    > [!Note]  
    > Saklama süresinden daha eski olan yedeklemeleri arşivlemek istiyorsanız, Scheduler 'ın yedekleri silmesi için önce dosyaları yedeklediğinizden emin olun. Yedekleme saklama süresini (örn. 7 günden 5 güne kadar) azaldıysanız Zamanlayıcı, yeni saklama süresinden daha eski tüm yedeklemeleri siler. Bu değeri güncelleştirmeden önce yedeklerin silinme işlemiyle birlikte olduğunuzdan emin olun.

9. Şifreleme ayarları ' nda, Certificate. cer dosya kutusuna bir sertifika sağlayın. Yedekleme dosyaları, sertifikadaki bu ortak anahtar kullanılarak şifrelenir. Yedekleme ayarlarını yapılandırırken yalnızca ortak anahtar bölümünü içeren bir sertifika belirtin. Bu sertifikayı ilk kez ayarladıktan veya sertifikayı gelecekte döndürdüğünüzde, yalnızca sertifikanın parmak izini görüntüleyebilirsiniz. Karşıya yüklenen sertifika dosyasını indiremez veya görüntüleyemezsiniz. Sertifika dosyasını oluşturmak için, ortak ve özel anahtarlarla kendinden imzalı bir sertifika oluşturmak ve yalnızca ortak anahtar bölümüyle bir sertifikayı dışarı aktarmak için aşağıdaki PowerShell komutunu çalıştırın. Sertifikayı yönetici portalından erişilebilen her yere kaydedebilirsiniz.

    ```powershell

        $cert = New-SelfSignedCertificate `
            -DnsName "www.contoso.com" `
            -CertStoreLocation "cert:\LocalMachine\My"

        New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 
        Export-Certificate `
            -Cert $cert `
            -FilePath c:\certs\AzSIBCCert.cer 
    ```

   > [!Note]
   > **1901 ve üzeri**: Azure Stack, altyapı yedekleme verilerini şifrelemek için bir sertifika kabul eder. Sertifikayı ortak ve özel anahtarla güvenli bir yerde sakladığınızdan emin olun. Güvenlik nedenleriyle, yedekleme ayarlarını yapılandırmak için sertifikayı ortak ve özel anahtarlarla kullanmanız önerilmez. Bu sertifikanın yaşam döngüsünü yönetme hakkında daha fazla bilgi için bkz. [Infrastructure Backup Service en iyi yöntemleri](azure-stack-backup-best-practices.md).
   > 
   > **1811 veya önceki sürümler**: Azure Stack altyapı yedekleme verilerini şifrelemek için bir simetrik anahtar kabul eder. [Bir anahtar oluşturmak Için New-AzsEncryptionKey64 cmdlet 'ini](https://docs.microsoft.com/powershell/module/azs.backup.admin/new-azsencryptionkeybase64)kullanın. 1811 ' den 1901 ' ye yükselttikten sonra yedekleme ayarları şifreleme anahtarını korur. Bir sertifika kullanmak için yedekleme ayarlarını güncelleştirmenizi öneririz. Şifreleme anahtarı desteği artık kullanım dışıdır. Bir sertifika kullanmak için ayarları güncelleştirmek üzere en az 3 yayındır.

10. Yedekleme denetleyicisi ayarlarınızı kaydetmek için **Tamam ' ı** seçin.

![Azure Stack-yedekleme denetleyicisi ayarları](media/azure-stack-backup/backup-controller-settings-certificate.png)


## <a name="start-backup"></a>Yedeklemeyi Başlat
Bir yedekleme başlatmak için, isteğe bağlı bir yedekleme başlatmak üzere **Şimdi Yedekle** ' ye tıklayın. İsteğe bağlı yedekleme, bir sonraki zamanlanmış yedeklemenin saatini değiştirmez. Görev tamamlandıktan sonra, **temelleri**içindeki ayarları doğrulayabilirsiniz:

![İsteğe bağlı Azure Stack yedekleme](media/azure-stack-backup/scheduled-backup.png)

Ayrıca, Azure Stack yönetici bilgisayarınızda **Start-AzsBackup** PowerShell cmdlet 'ini de çalıştırabilirsiniz. Daha fazla bilgi için bkz. [yedekleme Azure Stack](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Otomatik yedeklemeleri etkinleştir veya devre dışı bırak
Yedeklemeyi etkinleştirdiğinizde yedeklemeler otomatik olarak zamanlanır. Yeni bir sonraki zamanlama yedekleme süresini **Essentials**' da denetleyebilirsiniz. 

![İsteğe bağlı Azure Stack yedekleme](media/azure-stack-backup/on-demand-backup.png)

Gelecekteki zamanlanmış yedeklemeleri devre dışı bırakmanız gerekirse, **Otomatik yedeklemeleri devre dışı bırak**' a tıklayın. Otomatik yedeklemeleri devre dışı bırakmak, yedekleme ayarlarının yapılandırılmasını önler ve yedekleme zamanlamasını korur. Bu eylem, Scheduler 'ın gelecekteki yedeklemeleri atlamasını söyler.

![Azure Stack-zamanlanmış yedeklemeleri devre dışı bırak](media/azure-stack-backup/disable-auto-backup.png)

Gelecekteki zamanlanmış yedeklemelerin **Essentials**'ta devre dışı bırakıldığını onaylayın:

![Azure Stack-yedeklemelerin devre dışı bırakıldığını onaylayın](media/azure-stack-backup/confirm-disable.png)

Zamanlayıcıyı zamanlanan zamanda gelecekteki yedeklemeleri başlatacak şekilde bildirmek için **Otomatik yedeklemeleri etkinleştir** ' e tıklayın. 

![Azure Stack-zamanlanmış yedeklemeleri etkinleştir](media/azure-stack-backup/enable-auto-backup.png)


> [!Note]  
> Altyapı yedeklemesini 1807 ' ye güncelleştirmeden önce yapılandırdıysanız otomatik yedeklemeler devre dışı bırakılır. Bu şekilde Azure Stack tarafından başlatılan yedeklemeler, bir dış görev zamanlama altyapısı tarafından başlatılan yedeklemeler ile çakışmaz. Herhangi bir dış görev zamanlayıcısını devre dışı bıraktıktan sonra **Otomatik yedeklemeleri etkinleştir**' e tıklayın.

## <a name="update-backup-settings"></a>Yedekleme ayarlarını Güncelleştir
1901 itibariyle şifreleme anahtarı desteği kullanım dışıdır. Yedekleme 'yi 1901 ' de ilk kez yapılandırıyorsanız bir sertifika kullanmanız gerekir. Azure Stack, şifreleme anahtarını yalnızca anahtar 1901 ' ye güncelleştirmeden önce yapılandırılırsa destekler. Geriye dönük uyumluluk modu, üç sürüm için devam edecektir. Bundan sonra, şifreleme anahtarları artık desteklenmeyecektir.

### <a name="default-mode"></a>Varsayılan mod
Şifreleme ayarları ' nda, 1901 ' a yükledikten veya güncelleştirdikten sonra altyapı yedeklemesini ilk kez yapılandırıyorsanız, yedeklemeyi bir sertifika ile yapılandırmanız gerekir. Şifreleme anahtarı kullanmak artık desteklenmiyor.

Yedekleme verilerini şifrelemek için kullanılan sertifikayı güncelleştirmek için yeni bir yükleyin. Ortak anahtar bölümü olan CER dosyası ve ayarları kaydetmek için Tamam ' ı seçin.

Yeni yedeklemeler, yeni sertifikada ortak anahtar kullanmaya başlar. Önceki sertifikayla oluşturulan tüm mevcut yedeklemelere hiçbir etkisi yoktur. Bulut kurtarması için ihtiyacınız olması durumunda eski sertifikayı güvenli bir yerde tutmaya dikkat edin.

![Azure Stack-sertifika parmak izini görüntüleme](media/azure-stack-backup/encryption-settings-thumbprint.png)

### <a name="backwards-compatibility-mode"></a>Geriye dönük uyumluluk modu
Yedekleme 'yi 1901 ' ye güncelleştirmeden önce yapılandırdıysanız, ayarlar davranış değişikliğine karşı taşınır. Bu durumda, geriye doğru uyumluluk için şifreleme anahtarı desteklenir. Şifreleme anahtarını güncelleştirebilir veya bir sertifika kullanmak için geçiş yapabilirsiniz. Şifreleme anahtarını güncelleştirmeye devam etmek için en az üç yayınınıza sahipsiniz. Bir sertifikaya geçiş yapmak için bu saati kullanın. Yeni bir şifreleme anahtarı oluşturmak için [New-AzsEncryptionKeyBase64](https://docs.microsoft.com/powershell/module/azs.backup.admin/new-azsencryptionkeybase64)kullanın.

![Azure Stack-şifreleme anahtarını geriye dönük uyumluluk modunda kullanın](media/azure-stack-backup/encryption-settings-backcompat-encryption-key.png)

> [!Note]  
> Şifreleme anahtarından sertifikaya güncelleştirme tek yönlü bir işlemdir. Bu değişikliği yaptıktan sonra şifreleme anahtarına geri geçiş yapamazsınız. Mevcut tüm yedeklemeler önceki şifreleme anahtarıyla şifrelenmiş olarak kalacak.

![Azure Stack-geri uyumluluk modunda şifreleme sertifikası kullan](media/azure-stack-backup/encryption-settings-backcompat-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir yedekleme çalıştırmayı öğrenin. Bkz. [yedekleme Azure Stack](azure-stack-backup-back-up-azure-stack.md).

Yedeğinizin çalıştığını doğrulamayı öğrenin. Bkz. [yönetici portalında yedeklemeyi onaylama tamamlandı](azure-stack-backup-back-up-azure-stack.md).
