---
title: Azure Stack için en iyi hizmet uygulamaları Infrastructure Backup | Microsoft Docs
description: Çok önemli bir hata olursa veri kaybını azaltmaya yardımcı olmak üzere Azure Stack dağıtırken ve yönetirken bu en iyi uygulamaları izleyin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 0b843018a90e0a07a1d1135099275288d4a4ce62
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974751"
---
# <a name="infrastructure-backup-service-best-practices"></a>Infrastructure Backup hizmeti en iyi uygulamaları

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Çok önemli bir hata olursa veri kaybını azaltmaya yardımcı olmak üzere Azure Stack dağıtırken ve yönetirken bu en iyi uygulamaları izleyin.

İşlem akışında değişiklik yapıldığında yüklemenizin hala uyumlu olduğunu doğrulamak için en iyi uygulamaları düzenli olarak gözden geçirin. Bu en iyi yöntemleri uygularken herhangi bir sorunla karşılaşırsanız yardım için Microsoft Desteği başvurun.

## <a name="configuration-best-practices"></a>En iyi yapılandırma uygulamaları

### <a name="deployment"></a>Dağıtım

Her Azure Stack bulutun dağıtımından sonra Infrastructure Backup etkinleştirin. Azure Stack PowerShell kullanarak, işletmen yönetimi API uç noktasına erişimi olan herhangi bir istemciden/sunucudan yedeklemeleri zamanlayabilirsiniz.

### <a name="networking"></a>Ağ

Yolun evrensel adlandırma kuralı (UNC) dizesi bir tam etki alanı adı (FQDN) kullanmalıdır. Ad çözümlemesi mümkün değilse, IP adresi kullanılabilir. UNC dizesi, paylaşılan dosyalar veya cihazlar gibi kaynakların konumunu belirtir.

### <a name="encryption"></a>Şifreleme

#### <a name="version-1901-and-newer"></a>Sürüm 1901 ve üzeri

Şifreleme sertifikası, dış depolamaya aktarılmış yedekleme verilerini şifrelemek için kullanılır. Sertifika yalnızca anahtarları aktarmak için kullanıldığından, sertifika otomatik olarak imzalanan bir sertifika olabilir. Sertifika oluşturma hakkında daha fazla bilgi için New SelfSignedCertificate bölümüne bakın.
  
Anahtar, güvenli bir konumda (örneğin, genel Azure Key Vault sertifikası) depolanmalıdır. Sertifikanın CER biçimi verileri şifrelemek için kullanılır. Yedekleme verilerinin şifresini çözmek için Azure Stack bulut kurtarma dağıtımı sırasında PFX biçiminin kullanılması gerekir.

![Sertifika güvenli bir konumda depolanıyor.](media/azure-stack-backup/azure-stack-backup-encryption-store-cert.png)

#### <a name="1811-and-older"></a>1811 ve üzeri

Şifreleme anahtarı, dış depolamaya aktarılmış olan yedekleme verilerini şifrelemek için kullanılır. Anahtar, [PowerShell ile Azure Stack için yedeklemeyi etkinleştirmenin](azure-stack-backup-enable-backup-powershell.md)bir parçası olarak oluşturulur.

Anahtar, güvenli bir konumda (örneğin, genel Azure Key Vault gizli) depolanmalıdır. Bu anahtarın Azure Stack yeniden dağıtımı sırasında kullanılması gerekir.

![Anahtara güvenli bir konum depolandı.](media/azure-stack-backup/azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>İşletimsel en iyi uygulamalar

### <a name="backups"></a>Yedeklemeler

 - Yedekleme işleri sistem çalışırken yürütülür, bu sayede yönetim deneyimleri veya kullanıcı uygulamaları için kapalı kalma süresi yoktur. Yedekleme işlerinin makul yük altında olan bir çözüm için 20-40 dakika sürer.
 - OEM tarafından sunulan yönergeleri kullanarak, el ile yedeklenen ağ anahtarları ve donanım yaşam döngüsü ana bilgisayarı (HLH), Infrastructure Backup denetleyicisinin denetim düzlemi yedekleme verilerini depoladığı aynı yedekleme paylaşımında depolanmalıdır. Anahtar ve HLH yapılandırmalarının bölge klasöründe depolanmasını göz önünde bulundurun. Aynı bölgede birden çok Azure Stack örneizin varsa, bir ölçek birimine ait olan her yapılandırma için bir tanımlayıcı kullanmayı düşünün.

### <a name="folder-names"></a>Klasör adları

 - Altyapı, MASBACKUP klasörünü otomatik olarak oluşturur. Bu, Microsoft tarafından yönetilen bir paylaşımdır. MASBACKUP ile aynı düzeyde paylaşımlar oluşturabilirsiniz. Azure Stack oluşturmayan MASBACKUP içinde klasörler veya depolama verileri oluşturmanız önerilmez.
 -  Farklı bulutlardan yedekleme verilerini ayırt etmek için klasörünüzün adındaki Kullanıcı FQDN 'SI ve bölgesi. Azure Stack dağıtımınızın ve uç noktalarınızın FQDN 'SI, Region parametresinin ve dış etki alanı adı parametresinin birleşimidir. Daha fazla bilgi için bkz. [Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md).

Örneğin, yedek paylaşımında fileserver01.contoso.com üzerinde barındırılan AzSBackups vardır. Bu dosya paylaşımında, dış etki alanı adını ve bölge adını kullanan bir alt klasörü kullanarak Azure Stack dağıtımı başına bir klasör olabilir.

FQDN: contoso.com  
Bölge: NYC


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

MASBackup klasörü Azure Stack yedekleme verilerini nerede depoladığını gösterir. Kendi verilerinizi depolamak için bu klasörü kullanmayın. OEM 'Ler, yedekleme verilerini depolamak için bu klasörü kullanmaz.

OEM 'Lerin, bileşenleri için yedekleme verilerini bölge klasörü altında depolaması önerilir. Her ağ anahtarı, donanım yaşam döngüsü ana bilgisayarı (HLH) ve benzeri, kendi alt klasöründe depolanabilir. Örneğin:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>İzleme

Aşağıdaki uyarılar sistem tarafından desteklenir:

| Uyarı                                                   | Açıklama                                                                                     | Düzeltme                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Dosya paylaşımının kapasitesi tükenmediği için yedekleme başarısız oldu. | Dosya paylaşımının kapasitesi kalmadı ve yedekleme denetleyicisi yedekleme dosyalarını konuma veremiyor. | Daha fazla depolama kapasitesi ekleyin ve yeniden yedeklemeyi deneyin. Alanı boşaltmak için mevcut yedeklemeleri silin (en eskiden başlayarak).                    |
| Bağlantı sorunları nedeniyle yedekleme başarısız oldu.             | Azure Stack ile dosya paylaşma arasındaki ağda sorun yaşanıyor.                          | Ağ sorununu çözün ve yedeklemeyi yeniden deneyin.                                                                                            |
| Yoldaki bir hata nedeniyle yedekleme başarısız oldu.                | Dosya paylaşımının yolu çözümlenemiyor.                                                          | Paylaşımın erişilebilir olduğundan emin olmak için, paylaşıma farklı bir bilgisayardan eşleyin. Artık geçerli değilse yolu güncelleştirmeniz gerekebilir.       |
| Yedekleme, kimlik doğrulama sorunu nedeniyle başarısız oldu.               | Kimlik bilgilerini etkileyen kimlik bilgileriyle veya ağ sorunuyla ilgili bir sorun olabilir.    | Paylaşımın erişilebilir olduğundan emin olmak için, paylaşıma farklı bir bilgisayardan eşleyin. Artık geçerli olmadıklarında kimlik bilgilerini güncelleştirmeniz gerekebilir. |
| Genel bir hata nedeniyle yedekleme başarısız oldu.                    | Başarısız istek aralıklı bir sorundan kaynaklanıyor olabilir. Yeniden yedeklemeyi deneyin.                    | Desteği arayın.                                                                                                                               |

## <a name="next-steps"></a>Sonraki adımlar

[Infrastructure Backup hizmeti](azure-stack-backup-reference.md)için başvuru malzemesini gözden geçirin.

[Infrastructure Backup hizmetini](azure-stack-backup-enable-backup-console.md)etkinleştirin.
