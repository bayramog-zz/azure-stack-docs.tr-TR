---
title: Azure Stack ile dış izleme çözümünü tümleştirmek | Microsoft Docs
description: Azure Stack, veri merkezinizdeki dış bir izleme çözümü ile tümleştirmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 06/05/2019
ms.author: jeffgilb
ms.reviewer: thoroet
ms.lastreviewed: 06/05/2019
ms.openlocfilehash: e0c3c4740a1bc8073e827ff9809cf1aafa029792
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691698"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Dış izleme çözümü, Azure Stack ile tümleştirin

Azure Stack altyapısını dış izleme için Azure Stack yazılımı, fiziksel bilgisayarları ve fiziksel ağ anahtarları izlemeniz gerekir. Bu alanların her biri, durum ve uyarı bilgilerini almak için bir yöntem sunar:

- Azure Stack yazılımı sistem durumu ve uyarıları almak için REST tabanlı bir API sunar. Depolama alanları doğrudan, depolama durumunu ve uyarılar gibi yazılım tanımlı teknolojilerinin kullanılması yazılım izleme bir parçası olan.
- Fiziksel bilgisayar sistem durumu ve uyarı bilgilerini temel kart yönetim denetleyicileri (Bmc'ler) aracılığıyla kullanılabilir hale getirir.
- Fiziksel ağ aygıtlarının durumunu ve uyarı bilgileri SNMP protokolü aracılığıyla kullanılabilir hale getirebilirsiniz.

Her Azure Stack çözüm donanım yaşam döngüsü konak ile birlikte gelir. Bu konak fiziksel sunucuları ve ağ cihazlarını izleme yazılımı orijinal ekipman üreticisi (OEM) donanım satıcısının çalıştırır. Lütfen izleme çözümleri, veri merkezinizdeki mevcut izleme çözümleriyle tümleştirilebilir, OEM sağlayıcınıza başvurun.

> [!IMPORTANT]
> Kullandığınız dış izleme çözümü aracısız olması gerekir. Azure Stack bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz.

Aşağıdaki diyagramda bir Azure Stack tümleşik sistemi, donanım yaşam döngüsü konak, dış izleme çözümünü ve bir dış bilet oluşturma/veri toplama sistem arasındaki trafik akışını gösterir.

![Azure Stack, izleme ve bilet oluşturma çözümü arasındaki trafiği gösteren diyagram.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

> [!NOTE]
> Fiziksel sunucuları ile doğrudan dış Monitoring Integration değil izin ve erişim denetim listeleri tarafından (ACL'ler) etkin bir şekilde engellendi.  Dış izleme tümleştirmesiyle doğrudan fiziksel ağ aygıtlarını desteklenmiyor, lütfen bu özelliği etkinleştirmek nasıl OEM sağlayıcınızla denetleyin.

Bu makalede, Azure Stack, System Center Operations Manager ve Nagios gibi dış izleme çözümleriyle tümleştirmek açıklanmaktadır. Ayrıca uyarılarla PowerShell kullanarak veya REST API çağrıları üzerinden programlı bir şekilde çalışmanın nasıl içerir.

## <a name="integrate-with-operations-manager"></a>Operations Manager ile tümleştirme

Azure Stack dış izlemek için Operations Manager kullanabilirsiniz. System Center Yönetim Paketi Microsoft Azure Stack için tek bir Operations Manager örneği ile birden çok Azure Stack dağıtımı izlemenizi sağlar. Yönetim Paketi, Azure Stack ile iletişim kurmak için sistem durumu kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı REST API'leri kullanır. Donanım yaşam döngüsü konakta çalışan yazılım izleme OEM atlama planlıyorsanız, fiziksel sunucuları izlemek için satıcı yönetim paketlerini yükleyebilirsiniz. Operations Manager ağ cihazı bulma, ağ anahtarlarını izlemek için de kullanabilirsiniz.

Azure Stack için yönetim paketi aşağıdaki özellikleri sağlar:

- Birden çok Azure Stack dağıtımı yönetebilirsiniz.
- Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) için destek
- Alabilir ve Uyarıları Kapat
- Bir sistem durumu ve kapasite Panosu
- Düzeltme eki ve güncelleştirme (P & U) olduğunda ediyor için otomatik Bakım modu algılama dahildir
- Dağıtım ve bölge için zorla güncelleştirme görevleri içerir
- Bir bölgeye özel bilgiler ekleyebilirsiniz
- Bildirim destekler ve Raporlama

System Center Yönetim Paketi, Microsoft Azure Stack ve ilişkili indirebilirsiniz [Kullanıcı Kılavuzu](https://www.microsoft.com/en-us/download/details.aspx?id=55184), veya doğrudan Operations Manager.

Bilet oluşturma çözümü için Operations Manager, System Center Service Manager ile tümleştirebilirsiniz. Tümleşik bir ürün bağlayıcısının Service Manager'da hizmet isteği çözdükten sonra Azure Stack ve Operations Manager bir uyarı kapatmak izin veren iki yönlü iletişimi sağlar.

Aşağıdaki diyagram, mevcut bir System Center dağıtım ile Azure Stack tümleştirme gösterir. Service Manager daha fazla Azure Stack'te işlemleri çalıştırmak için System Center Orchestrator'ı veya Service Management Automation (SMA) ile otomatik hale getirebilirsiniz.

![SMA OM ve Service Manager ile tümleştirme gösteren diyagram.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios ile tümleştirin

Ayarlayın ve Microsoft Azure Stack için Nagios eklentiyi yapılandırma.

Eklenti izleme bir Nagios İhtiyari ücretsiz yazılım lisansı altında - MIT (Massachusetts Teknoloji Enstitüsü'nün) kullanılabilir olan iş ortağı Cloudbase çözümleriyle birlikte sunulmuştur geliştirilmiştir.

Eklenti Python'da yazılmıştır ve sistem durumu kaynak sağlayıcısı REST API'si yararlanır. Alma ve Azure stack'teki uyarıları kapatmak için temel işlevleri sunar. System Center Yönetim Paketi gibi birden fazla Azure Stack dağıtım eklemek ve bildirim göndermek için sağlar.

Azure Stack – sürüm 1.2 Nagios eklentisi Microsoft ADAL kitaplığı yararlanır ve bir parola veya sertifika ile hizmet sorumlusunu kullanarak kimlik doğrulamasını destekler. Ayrıca, yapılandırma, tek bir yapılandırma dosyası kullanarak yeni parametrelerle basitleştirilmiştir. Şimdi, AAD & ADFS kimlik sistemi kullanarak Azure Stack dağıtımları destekler.

Eklenti 4 x ve XI Nagios ile çalışır. İndirebilirsiniz [burada](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). İndirme sitesinde ayrıca yükleme ve yapılandırma ayrıntılarını içerir.

### <a name="requirements-for-nagios"></a>Nagios gereksinimleri

1.  En düşük Nagios sürümü 4.x.

2.  Microsoft Azure Active Directory Python kitaplığı. Bu, Python PIP kullanılarak yüklenebilir.

```bash  
sudo pip install adal pyyaml six
```

### <a name="install-plugin"></a>Eklentisini yükleme

Bu bölümde, Nagios'ın varsayılan yüklemede varsayılarak Azure Stack eklentisini yüklemek açıklar.

Eklenti paketi aşağıdaki dosyaları içerir:

```
  azurestack_plugin.py
  azurestack_handler.sh
  samples/etc/azurestack.cfg
  samples/etc/azurestack_commands.cfg
  samples/etc/azurestack_contacts.cfg
  samples/etc/azurestack_hosts.cfg
  samples/etc/azurestack_services.cfg
```

1.  Eklentiyi Kopyala `azurestack_plugin.py` aşağıdaki dizine `/usr/local/nagios/libexec`.

2.  İşleyici kopyalama `azurestack_handler.sh` aşağıdaki dizine `/usr/local/nagios/libexec/eventhandlers`.

3.  Eklenti dosyası yürütülebilir olarak ayarlı olun

    ```bash
      sudo cp azurestack_plugin.py <PLUGINS_DIR>
      sudo chmod +x <PLUGINS_DIR>/azurestack_plugin.py
    ```

### <a name="configure-plugin"></a>Eklentiyi yapılandırmak

Aşağıdaki parametreleri azurestack.cfg dosyasında yapılandırılması kullanılabilir. Kalın yazı tipli parametre yapılandırılması seçtiğiniz kimlik doğrulaması modelinden bağımsız gerekir.

Ayrıntılı bir SPN oluşturmak üzere nasıl belgelenen bilgiler [burada](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals).

| Parametre | Açıklama | Kimlik Doğrulaması |
| --- | --- | --- |
| **External_domain_fqdn ** | Dış etki alanı FQDN'si |    |
| ** Bölge: ** | Bölge Adı |    |
| ** Kiracı: ** | Kiracı kimliği\* |    |
| client_id: | İstemci Kimliği | Gizli dizi ile SPN |
| client_secret: | İstemci parolası | Gizli dizi ile SPN |
| client_cert\*\*: | Sertifika yolu | Sertifika ile SPN |
| client_cert_thumbprint\*\*: | Sertifika parmak izi | Sertifika ile SPN |

\*Kiracı kimliği, AD FS ile Azure Stack dağıtımları için gerekli değildir.

\*\* İstemci gizli anahtarı ve istemci sertifikası birbirini dışlar.

Bunlar da Nagios içinde yapılandırılabilir gibi diğer yapılandırma dosyalarını isteğe bağlı yapılandırma ayarlarını içerir.

> [!Note]  
> Azurestack_hosts.cfg ve azurestack_services.cfg hedef konumu kontrol edin.

| Yapılandırma | Açıklama |
| --- | --- |
| azurestack_commands.cfg | İşleyici yapılandırması değişiklikleri gereksinimi yoktur |
| azurestack_contacts.cfg | Bildirim ayarları |
| azurestack_hosts.cfg | Azure Stack dağıtım adlandırma |
| azurestack_services.cfg | Hizmeti |

### <a name="setup-steps"></a>Kurulum adımları

1.  Yapılandırma dosyasını değiştirme

2.  Değiştirilmiş yapılandırma dosyasını aşağıdaki klasöre kopyalayın `/usr/local/nagios/etc/objects`.

### <a name="update-nagios-configuration"></a>Nagios yapılandırmasını güncelleştirme

Nagios yapılandırma, Azure Stack emin olmak için güncelleştirilmesi gerekiyor: Nagios eklentisi yüklenir.

1.  Şu dosyayı açın

```bash  
/usr/local/nagios/etc/nagios.cfg
```

1.  Şu girişi ekleyin

```bash  
  #load the Azure Stack Plugin Configuration
  cfg_file=/usr/local/Nagios/etc/objects/azurestack_contacts.cfg
  cfg_file=/usr/local/Nagios/etc/objects/azurestack_commands.cfg
  cfg_file=/usr/local/Nagios/etc/objects/azurestack_hosts.cfg
  cfg_file=/usr/local/Nagios/etc/objects/azurestack_services.cfg
```

1.  Nagios yeniden yükleyin

```bash  
sudo service nagios reload
```

### <a name="manually-close-active-alerts"></a>El ile etkin uyarıları Kapat

Etkin uyarıları özel bir bildirim işlevini kullanarak Nagios içinde kapatılabilir. Özel bir bildirim olması gerekir:

```
  /close-alert <ALERT_GUID>
```

Bir uyarı, bir terminalde aşağıdaki komutla kullanarak da kapatılabilir:

```bash
  /usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Close --alert-id <ALERT_GUID>
```

### <a name="troubleshooting"></a>Sorun giderme

Eklenti sorun giderme olabilir eklentisini el ile bir terminal içinde arama yapılır. Aşağıdaki yöntemi kullanın:

```bash
  /usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Monitor
```

## <a name="use-powershell-to-monitor-health-and-alerts"></a>İzleyici sistem durumu ve uyarıları için PowerShell kullanma

Operations Manager, Nagios ve Nagios tabanlı bir çözümü kullanmıyorsanız, Azure Stack ile tümleştirme çözümlerini izleme geniş etkinleştirmek için PowerShell kullanabilirsiniz.

1. PowerShell kullanmak için bilgisayarınızda yüklü olduğundan emin olun [PowerShell yüklenmiş ve yapılandırılmış](azure-stack-powershell-install.md) Azure Stack operatörü ortam için. PowerShell'i Resource Manager (Yönetici) uç noktası ulaşan yerel bir bilgisayara yükleme (https://adminmanagement. [ Bölge]. [External_FQDN]).

2. Azure Stack operatör olarak Azure Stack ortamınıza bağlanmak için aşağıdaki komutları çalıştırın:

   ```powershell
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https:\//adminmanagement.[Region].[External_FQDN] `
      -AzureKeyVaultDnsSuffix adminvault.[Region].[External_FQDN] `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Aşağıdaki örnekler gibi uyarılarla çalışma için komutları kullanın:
   ```powershell
    #Retrieve all alerts
    $Alerts = Get-AzsAlert
    $Alerts

    #Filter for active alerts
    $Active = $Alerts | Where-Object { $_.State -eq "active" }
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    $RPHealth = Get-AzsRPHealth
    $RPHealth

    #Retrieve infrastructure role instance health
    $FRPID = $RPHealth | Where-Object { $_.DisplayName -eq "Capacity" }
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId
    ```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Yerleşik sistem durumu izleme hakkında daha fazla bilgi için bkz: [izleme sistem durumu ve Uyarıları Azure Stack'te](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik tümleştirmesi](azure-stack-integrate-security.md)
