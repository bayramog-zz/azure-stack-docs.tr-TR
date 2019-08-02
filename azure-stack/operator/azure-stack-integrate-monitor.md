---
title: Dış izleme çözümünü Azure Stack ile tümleştirin | Microsoft Docs
description: Azure Stack, veri merkezinizdeki bir dış izleme çözümüyle nasıl tümleştirileceğini öğrenin.
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
ms.openlocfilehash: 7b5bfb39c3ec14c23b1df54c13f2733724fcfe05
ms.sourcegitcommit: ddb625bb01de11bfb75d9f7a1cc61d5814b3bc31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68712911"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Dış izleme çözümünü Azure Stack ile tümleştirin

Azure Stack altyapısının dış izlemesi için Azure Stack yazılımını, fiziksel bilgisayarları ve fiziksel ağ anahtarlarını izlemeniz gerekir. Bu alanların her biri, sistem durumu ve uyarı bilgilerini almak için bir yöntem sunar:

- Azure Stack yazılım, sistem durumunu ve uyarıları almak için REST tabanlı bir API sunar. Depolama Alanları Doğrudan, depolama durumu ve uyarılar gibi yazılım tanımlı teknolojilerin kullanımı, yazılım izlemenin bir parçasıdır.
- Fiziksel bilgisayarlar, temel kart yönetim denetleyicileri (BMC 'ler) aracılığıyla sistem durumu ve uyarı bilgilerini kullanılabilir hale getirir.
- Fiziksel ağ cihazları, SNMP protokolü aracılığıyla sistem durumu ve uyarı bilgilerini kullanabilir hale getirir.

Her bir Azure Stack çözümü bir donanım yaşam döngüsü konağından gelir. Bu konak, fiziksel sunucular ve ağ cihazları için özgün ekipman üreticisi (OEM) donanım satıcısının izleme yazılımını çalıştırır. İzleme çözümleri, veri merkezinizdeki mevcut izleme çözümleriyle tümleştirilebilen lütfen OEM sağlayıcınıza danışın.

> [!IMPORTANT]
> Kullandığınız dış izleme çözümü aracısız olmalıdır. Üçüncü taraf aracıları Azure Stack bileşenleri içine yükleyemezsiniz.

Aşağıdaki diyagramda Azure Stack tümleşik bir sistem, donanım yaşam döngüsü Konağı, bir dış izleme çözümü ve bir dış bilet/veri toplama sistemi arasındaki trafik akışı gösterilmektedir.

![Azure Stack, izleme ve bilet oluşturma çözümü arasındaki trafiği gösteren diyagram.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

> [!NOTE]
> Fiziksel sunucularla doğrudan dış Izleme tümleştirmesine izin verilmez ve Access Control listeleri (ACL 'Ler) tarafından etkin bir şekilde engellenir.  Fiziksel ağ cihazlarıyla doğrudan dış Izleme tümleştirmesi desteklenir, lütfen bu özelliğin nasıl etkinleştirileceği ile ilgili olarak OEM sağlayıcınıza danışın.

Bu makalede, Azure Stack System Center Operations Manager ve Nagios gibi dış izleme çözümleriyle nasıl tümleştirileceği açıklanmaktadır. Ayrıca, PowerShell kullanarak veya REST API çağrıları aracılığıyla uyarılarla programlama yoluyla nasıl çalışacağınızı da içerir.

## <a name="integrate-with-operations-manager"></a>Operations Manager ile tümleştirin

Azure Stack dış izleme için Operations Manager kullanabilirsiniz. Microsoft Azure Stack için System Center yönetim paketi, birden çok Azure Stack dağıtımını tek bir Operations Manager örneğiyle izlemenize olanak sağlar. Yönetim Paketi, Azure Stack iletişim kurmak için sistem durumu kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı REST API 'Lerini kullanır. Donanım yaşam döngüsü ana bilgisayarında çalışan OEM izleme yazılımını atlamayı planlıyorsanız, fiziksel sunucuları izlemek için satıcı yönetim paketleri yükleyebilirsiniz. Ağ anahtarlarını izlemek için ağ aygıtı bulma Operations Manager de kullanabilirsiniz.

Azure Stack için yönetim paketi aşağıdaki özellikleri sağlar:

- Birden çok Azure Stack dağıtımı yönetebilirsiniz
- Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) için destek
- Uyarıları alabilir ve kapatabilirsiniz
- Bir sistem durumu ve kapasite panosu var
- Düzeltme Eki ve güncelleştirme (P & U) devam ederken otomatik bakım modu algılamayı içerir
- Dağıtım ve bölge için güncelleştirme görevlerini zorla içerir
- Bir bölgeye özel bilgi ekleyebilirsiniz
- Bildirim ve raporlamayı destekler

Microsoft Azure Stack ve ilişkili [Kullanıcı Kılavuzu](https://www.microsoft.com/en-us/download/details.aspx?id=55184)Için System Center yönetim paketini indirebilir veya doğrudan Operations Manager.

Anahtar oluşturma çözümü için Operations Manager System Center Service Manager tümleştirebilirsiniz. Tümleşik ürün bağlayıcısı, Service Manager bir hizmet isteğini çözdükten sonra Azure Stack ve Operations Manager bir uyarıyı kapatmanıza olanak tanıyan iki yönlü iletişim sağlar.

Aşağıdaki diyagramda, mevcut bir System Center dağıtımıyla Azure Stack tümleştirmesi gösterilmektedir. Azure Stack işlemleri çalıştırmak için System Center Orchestrator veya Service Management Automation (SMA) ile Service Manager daha da otomatikleştirebilirsiniz.

![OM, Service Manager ve SMA ile tümleştirmeyi gösteren diyagram.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios ile tümleştirme

Microsoft Azure Stack için Nagios eklentisini ayarlayabilir ve yapılandırabilirsiniz.

Nagios izleme eklentisi, iş ortağı Cloudbase çözümleriyle birlikte geliştirilmiştir ve bu, izin veren ücretsiz yazılım lisansı-MıT (Massachusetts Technology of Technology) altında mevcuttur.

Eklenti Python 'da yazılmıştır ve REST API sistem durumu kaynak sağlayıcısı 'ndan yararlanır. Azure Stack uyarıları almak ve kapatmak için temel işlevler sunar. System Center yönetim paketi gibi, birden çok Azure Stack dağıtımı eklemenize ve bildirim gönderebilmenizi sağlar.

Sürüm 1,2 ile Azure Stack – Nagios eklentisi Microsoft ADAL kitaplığını kullanır ve gizli veya sertifikayla hizmet sorumlusu kullanarak kimlik doğrulamasını destekler. Ayrıca, yapılandırma yeni parametrelerle tek bir yapılandırma dosyası kullanılarak basitleştirilmiştir. Artık kimlik sistemi olarak ADFS & ADFS kullanarak Azure Stack dağıtımlarını desteklemektedir.

Eklenti Nagios 4X ve XI ile birlikte kullanılabilir. [Buradan](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)indirebilirsiniz. İndirme sitesi ayrıca yükleme ve yapılandırma ayrıntılarını içerir.

### <a name="requirements-for-nagios"></a>Nagios gereksinimleri

1.  En düşük Nagios sürümü 4. x

2.  Python Kitaplığı Microsoft Azure Active Directory. Bu, Python PıP kullanılarak yüklenebilir.

    ```bash  
    sudo pip install adal pyyaml six
    ```

### <a name="install-plugin"></a>Eklentiyi yükler

Bu bölümde, Nagios 'ın varsayılan yüklemesinde olduğu gibi Azure Stack eklentisinin nasıl yükleneceği açıklanmaktadır.

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

1.  Eklentiyi `azurestack_plugin.py` aşağıdaki dizine `/usr/local/nagios/libexec`kopyalayın.

2.  İşleyiciyi `azurestack_handler.sh` aşağıdaki dizine `/usr/local/nagios/libexec/eventhandlers`kopyalayın.

3.  Eklenti dosyasını yürütülebilir olarak ayarlanmış hale getirme

    ```bash
    sudo cp azurestack_plugin.py <PLUGINS_DIR>
    sudo chmod +x <PLUGINS_DIR>/azurestack_plugin.py
    ```

### <a name="configure-plugin"></a>Eklentiyi Yapılandır

Aşağıdaki parametreler azurestack. cfg dosyasında yapılandırılabilir. Kalın ' daki parametrelerin, seçtiğiniz kimlik doğrulama modelinden bağımsız olarak yapılandırılması gerekir.

SPN oluşturma hakkında ayrıntılı bilgi [burada](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals)belgelenmiştir.

| Parametre | Açıklama | Authentication |
| --- | --- | --- |
| **External_domain_fqdn ** | Dış etki alanı FQDN 'SI |    |
| \* * bölge: * * | Bölge Adı |    |
| **tenant_id: ** | Kiracı KIMLIĞI\* |    |
| client_id: | İstemci Kimliği | Gizli anahtar içeren SPN |
| client_secret: | İstemci parolası | Gizli anahtar içeren SPN |
| client_cert\*:\* | Sertifika yolu | Sertifika ile SPN |
| client_cert_thumbprint\*:\* | Sertifika Parmak İzi | Sertifika ile SPN |

\*ADFS ile Azure Stack dağıtımları için kiracı KIMLIĞI gerekli değil.

\*\*İstemci parolası ve istemci sertifikası birbirini dışlıyor.

Diğer yapılandırma dosyaları, Nagios 'da yapılandırılabilecek şekilde isteğe bağlı yapılandırma ayarları içerir.

> [!Note]  
> Azurestack_hosts. cfg ve azurestack_services. cfg içindeki konum hedefini denetleyin.

| Yapılandırma | Açıklama |
| --- | --- |
| azurestack_commands.cfg | İşleyici yapılandırmasında değişiklik yok gereksinimi |
| azurestack_contacts. cfg | Bildirim Ayarları |
| azurestack_hosts. cfg | Azure Stack dağıtım adlandırma |
| azurestack_services. cfg | Hizmetin yapılandırması |

### <a name="setup-steps"></a>Kurulum adımları

1.  Yapılandırma dosyasını değiştirme

2.  Değiştirilen yapılandırma dosyalarını aşağıdaki klasöre `/usr/local/nagios/etc/objects`kopyalayın.

### <a name="update-nagios-configuration"></a>Nagios yapılandırmasını güncelleştirme

Azure Stack – Nagios eklentisinin yüklü olduğundan emin olmak için Nagios yapılandırmasının güncelleştirilmesi gerekir.

1.  Aşağıdaki dosyayı açın

```bash  
/usr/local/nagios/etc/nagios.cfg
```

2.  Aşağıdaki girişi ekleyin

```bash  
# Load the Azure Stack Plugin Configuration
cfg_file=/usr/local/Nagios/etc/objects/azurestack_contacts.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_commands.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_hosts.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_services.cfg
```

3.  Nagios 'ı yeniden yükleme

```bash  
sudo service nagios reload
```

### <a name="manually-close-active-alerts"></a>Etkin uyarıları el ile kapat

Etkin uyarılar, özel bildirim işlevselliği kullanılarak Nagios içinde kapatılabilir. Özel bildirim şu olmalıdır:

```
/close-alert <ALERT_GUID>
```

Bir uyarı, aşağıdaki komutla bir Terminal kullanılarak da kapatılabilir:

```bash
/usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Close --alert-id <ALERT_GUID>
```

### <a name="troubleshooting"></a>Sorun giderme

Eklentinin sorunlarını giderme, bir terminalde eklentiyi el ile çağırma işlemi gerçekleştirebilir. Aşağıdaki yöntemi kullanın:

```bash
/usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Monitor
```

## <a name="use-powershell-to-monitor-health-and-alerts"></a>Sistem durumunu ve Uyarıları izlemek için PowerShell 'i kullanma

Operations Manager, Nagios veya Nagios tabanlı bir çözüm kullanmıyorsanız, Azure Stack ile tümleştirilecek çok çeşitli izleme çözümlerini etkinleştirmek için PowerShell kullanabilirsiniz.

1. PowerShell 'i kullanmak için [PowerShell 'in yüklü olduğundan ve](azure-stack-powershell-install.md) bir Azure Stack operatör ortamı için yapılandırıldığından emin olun. PowerShell 'i Kaynak Yöneticisi (yönetici) uç noktasına (https://adminmanagement. [ Bölge]. [External_FQDN]).

2. Azure Stack ortamına Azure Stack operatörü olarak bağlanmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN] `
      -AzureKeyVaultDnsSuffix adminvault.[Region].[External_FQDN] `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.[Region].[External_FQDN]

   Connect-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Uyarılarla çalışmak için aşağıdaki örnekler gibi komutları kullanın:
   ```powershell
    # Retrieve all alerts
    $Alerts = Get-AzsAlert
    $Alerts

    # Filter for active alerts
    $Active = $Alerts | Where-Object { $_.State -eq "active" }
    $Active

    # Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    $RPHealth = Get-AzsRPHealth
    $RPHealth

    # Retrieve infrastructure role instance health
    $FRPID = $RPHealth | Where-Object { $_.DisplayName -eq "Capacity" }
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId
    ```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Yerleşik sistem durumu izleme hakkında bilgi için bkz. [Azure Stack durumu ve Uyarıları izleme](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik Tümleştirmesi](azure-stack-integrate-security.md)
