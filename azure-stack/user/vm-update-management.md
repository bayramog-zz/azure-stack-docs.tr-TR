---
title: Azure Stack 'de VM güncelleştirme ve yönetim Otomasyonu | Microsoft Docs
description: Azure Stack ' de dağıtılan Windows ve Linux VM 'lerini yönetmek için Azure Otomasyonu 'nda VM'ler için Azure İzleyici, Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümlerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: rtiberiu
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: 3fa6d124722d45d727525820b6a99d408f0d2350
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975161"
---
# <a name="vm-update-and-management-automation-in-azure-stack"></a>Azure Stack 'de VM güncelleştirme ve yönetim Otomasyonu
Azure Stack kullanılarak dağıtılan Windows ve Linux sanal makinelerini (VM 'Ler) yönetmek için aşağıdaki Azure Otomasyonu çözüm özelliklerini kullanın:

- **[Güncelleştirme yönetimi](https://docs.microsoft.com/azure/automation/automation-update-management)** : Güncelleştirme Yönetimi çözümü sayesinde, tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızlıca değerlendirebilir ve Windows ve Linux VM 'Leri için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

- **[Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking)** : İzlenen sunuculardaki yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosyalar ve Linux Daemon 'ları üzerinde yapılan değişiklikler, işlenmek üzere buluttaki Azure Izleyici hizmetine gönderilir. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder. Değişiklik İzleme panosundaki bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

- **[Envanter](https://docs.microsoft.com/azure/automation/automation-vm-inventory)** . Bir Azure Stack VM için envanter izleme, envanter toplamayı ayarlamak ve yapılandırmak için tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

- **[VM'ler için Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)** : VM'ler için Azure İzleyici Azure ve Azure Stack VM 'lerinizi ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. Windows ve Linux sanal makinelerinizin performansını ve sistem durumunu analiz eder ve ayrıca, diğer kaynaklardaki ve dış süreçlerdeki işlemleri ve bağımlılıklarını izler.

> [!IMPORTANT]
> Bu çözümler, Azure VM 'Leri yönetmek için kullanılanlar ile aynıdır. Aynı araçları kullanarak hem Azure hem de Azure Stack VM 'Leri aynı arabirimden aynı şekilde yönetilir. Azure Stack VM 'Ler Ayrıca, Azure Stack Güncelleştirme Yönetimi, Değişiklik İzleme, envanter ve VM'ler için Azure İzleyici çözümlerini kullanırken Azure VM 'Lerle aynı şekilde fiyatlandırılır.

## <a name="prerequisites"></a>Önkoşullar
Azure Stack VM 'Leri güncelleştirmek ve yönetmek için bu özellikler kullanılmadan önce çeşitli önkoşulların karşılanması gerekir. Bunlar, Azure portal alınması gereken adımları ve ayrıca Azure Stack yönetim portalını içerir.

### <a name="in-the-azure-portal"></a>Azure portal
Azure Stack VM 'Ler için VM'ler için Azure İzleyici, envanter, Değişiklik İzleme ve Güncelleştirme Yönetimi Azure Otomasyonu özelliklerini kullanmak için, önce bu çözümleri Azure 'da etkinleştirmeniz gerekir.

> [!TIP]
> Bu özellikleri Azure VM 'Ler için zaten etkinleştirdiyseniz, önceden var olan LogAnalytics çalışma alanı kimlik bilgilerinizi kullanabilirsiniz. Kullanmak istediğiniz bir LogAnalytics çalışma alanı kimliği ve birincil anahtarınız zaten varsa, [sonraki bölüme](./vm-update-management.md#in-the-azure-stack-administrator-portal)atlayın. Aksi takdirde, yeni bir LogAnalytics çalışma alanı ve Otomasyon hesabı oluşturmak için bu bölümde devam edin.

Bu çözümleri etkinleştirmenin ilk adımı, Azure aboneliğinizde [bir LogAnalytics çalışma alanı oluşturmaktır](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) . Log Analytics çalışma alanı, kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Azure Izleyici günlükleri ortamıdır. Bir çalışma alanı oluşturduktan sonra, çalışma alanı kimliği ve anahtarı ' nı aklınızda bulabilirsiniz. Bu bilgileri görüntülemek için çalışma alanı Dikey penceresine gidin, **Gelişmiş ayarlar**' a tıklayın ve **çalışma alanı kimliğini** ve **birincil anahtar** değerlerini gözden geçirin. 

Daha sonra [bir Otomasyon hesabı oluşturmanız](https://docs.microsoft.com/azure/automation/automation-create-standalone-account)gerekir. Otomasyon hesabı, Azure Otomasyonu kaynaklarınız için bir kapsayıcıdır. Ortamlarınızı ayırmak veya Otomasyon iş akışlarınızı ve kaynaklarınızı daha fazla düzenlemek için bir yol sağlar. Otomasyon hesabı oluşturulduktan sonra, envanter, Değişiklik İzleme ve Güncelleştirme Yönetimi özelliklerini etkinleştirmeniz gerekir. Her özelliği etkinleştirmek için şu adımları izleyin:

1. Azure portal, kullanmak istediğiniz Otomasyon hesabına gidin.

2. Etkinleştirilecek çözümü seçin ( **Envanter**, **değişiklik izleme**veya **güncelleştirme yönetimi**).

3. Kullanılacak Log Analytics çalışma alanını seçmek için **çalışma alanı seç...** açılan listesini kullanın.

4. Kalan tüm bilgilerin doğru olduğundan emin olun ve ardından çözümü etkinleştirmek için **Etkinleştir** ' e tıklayın.

5. Üç çözümü de etkinleştirmek için 2-4 arasındaki adımları yineleyin. 

   [![](media/vm-update-management/1-sm.PNG "Azure Otomasyonu hesabı özelliklerini etkinleştirme")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici etkinleştir

VM'ler için Azure İzleyici, Azure VM 'lerinizi ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. Windows ve Linux sanal makinelerinizin performansını ve sistem durumunu analiz eder ve ayrıca, diğer kaynaklardaki ve dış süreçlerdeki işlemleri ve bağımlılıklarını izler.

Bir çözüm olarak VM'ler için Azure İzleyici, şirket içinde veya başka bir bulut sağlayıcısında barındırılan VM 'Ler için performans ve uygulama bağımlılıklarını izleme desteği içerir. Üç anahtar özellik kapsamlı Öngörüler sunun:

1. Önceden yapılandırılmış durum ölçütlerine göre ölçülen Windows ve Linux çalıştıran Azure VM 'lerinin mantıksal bileşenleri, değerlendirilen koşul karşılandığında sizi uyarır.

2. Konuk VM işletim sisteminden çekirdek performans ölçümlerini görüntüleyen önceden tanımlanmış popüler performans grafikleri.

3. Çeşitli kaynak gruplarından ve aboneliklerden VM ile bağlantılı bileşenleri görüntüleyen bağımlılık eşlemesi.

Log Analytics çalışma alanı oluşturulduktan sonra, Linux ve Windows VM 'lerinde koleksiyon için çalışma alanındaki performans sayaçlarını etkinleştirin. Ardından, ServiceMap ve InfrastructureInsights çözümünü çalışma alanınıza yükleyip etkinleştirin. İşlem, [dağıtım VM'ler için Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#how-to-enable-azure-monitor-for-vms-preview) kılavuzunda açıklanmaktadır.

### <a name="in-the-azure-stack-administrator-portal"></a>Azure Stack yönetici portalında
Azure portal Azure Otomasyonu çözümlerini etkinleştirdikten sonra, bir sonraki adımda Azure Stack yönetici portalında bir bulut Yöneticisi olarak oturum açmanız ve **Azure izleyici, güncelleştirme ve yapılandırma yönetimi** Ile **Azure izleyici, güncelleştirme ve güncelleştirmeleri indirmeniz gerekir Azure Stack marketi 'nde Linux uzantısı için yapılandırma yönetimi** .

   ![Azure Izleyici, güncelleştirme ve yapılandırma yönetimi uzantısı Market öğesi](media/vm-update-management/2.PNG) 

VM'ler için Azure İzleyici Map çözümünü etkinleştirmek ve ağ bağımlılıklarıyla ilgili Öngörüler elde etmek için **Azure izleyici Dependency Agent**indirin:

   ![Azure Izleyici Dependency Agent](media/vm-update-management/2-dependency.PNG) 

## <a name="enable-update-management-for-azure-stack-vms"></a>Azure Stack VM 'Ler için Güncelleştirme Yönetimi etkinleştirme
Azure Stack VM 'Ler için güncelleştirme yönetimini etkinleştirmek üzere aşağıdaki adımları izleyin.

1. Azure Stack Kullanıcı portalında oturum açın.

2. Azure Stack Kullanıcı-Portalı ' nda, bu çözümleri etkinleştirmek istediğiniz VM 'lerin uzantılar dikey penceresine gidin, **+ Ekle**' ye tıklayın, **Azure Update ve Configuration Management** uzantısını seçin ve ardından **Oluştur**' a tıklayın:

   [![](media/vm-update-management/3-sm.PNG "VM Uzantısı dikey penceresi")](media/vm-update-management/3-lg.PNG#lightbox)

3. Aracıyı LogAnalytics çalışma alanıyla bağlamak için önceden oluşturulan çalışma alanı kimliği ve birincil anahtar sağlayın. Sonra uzantıyı dağıtmak için **Tamam** ' ı tıklatın.

   [![](media/vm-update-management/4-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlama")](media/vm-update-management/4-lg.PNG#lightbox) 

4. [Güncelleştirme yönetimi belgelerinde](https://docs.microsoft.com/azure/automation/automation-update-management)açıklandığı gibi, yönetmek istediğiniz her sanal makine için güncelleştirme yönetimi çözümünü etkinleştirmeniz gerekir. Çalışma alanına rapor veren tüm VM 'Ler için çözümü etkinleştirmek üzere **güncelleştirme yönetimi**' ni seçin, **makineleri Yönet**' i tıklatın ve ardından **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir** seçeneğini belirleyin.

   [![](media/vm-update-management/5-sm.PNG "Tüm makinelerde Güncelleştirme Yönetimi Çözümü Etkinleştir")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Çalışma alanına rapor veren Azure Stack sanal makinelere yönelik her çözümü etkinleştirmek için bu adımı tekrarlayın. 
  
Azure güncelleştirme ve yapılandırma yönetimi uzantısı etkinleştirildikten sonra, yönetilen her VM için günde iki kez tarama gerçekleştirilir. API, durumun değişip değişmediğini belirlemekte son güncelleştirme zamanını sorgulamak için her 15 dakikada bir çağrılır. Durum değiştiyse, bir uyumluluk taraması başlatılır.

VM 'Ler tarandıktan sonra, Güncelleştirme Yönetimi çözümünde Azure Otomasyonu hesabında görünürler: 

   [![](media/vm-update-management/6-sm.PNG "Güncelleştirme Yönetimi Azure Otomasyonu hesabı")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> Panonun yönetilen bilgisayarlardan güncelleştirilmiş verileri görüntülemesi 30 dakika ile 6 saat arasında sürebilir.

Azure Stack VM 'Ler artık Azure VM 'leriyle birlikte zamanlanmış güncelleştirme dağıtımlarında bulunabilir.

## <a name="enable-azure-monitor-for-vms-running-on-azure-stack"></a>Azure Stack üzerinde çalışan VM'ler için Azure İzleyici etkinleştir
VM 'nin **Azure İzleyicisi, güncelleştirme ve yapılandırma yönetimi**ve **Azure izleyici Dependency Agent** uzantıları yüklü olduğunda, [VM'ler için Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) çözümünde raporlama verileri başlatılır. 

> [!TIP]
> **Azure izleyici Dependency Agent** uzantısı herhangi bir parametre gerektirmez. Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](https://docs.microsoft.com/azure/azure-monitor/platform/gateway) BT güvenlik ilkeleriniz bilgisayarların internet'e bağlanmak için ağ üzerinde izin verme durumunda.

VM'ler için Azure İzleyici, bir VM 'nin ne kadar iyi performans gösterdiğini belirlemenize yardımcı olmak üzere birkaç ana performans göstergelerini (KPI) hedefleyen bir performans grafikleri kümesi içerir. Grafikler, performans sorunlarını ve anormallikleri belirleyebilmeniz için bir süre boyunca kaynak kullanımını gösterir. Ayrıca, seçilen ölçüm temelinde kaynak kullanımını görüntülemek için her makinenin bir perspektifine geçebilirsiniz. Performansla ilgilenirken göz önünde bulundurmanız gereken birçok öğe olsa da, VM'ler için Azure İzleyici işlemci, bellek, ağ bağdaştırıcısı ve disk kullanımı ile ilgili anahtar işletim sistemi performans göstergelerini izler. Performans grafikleri sistem durumu izleme özelliğini tamamlar ve olası sistem bileşeni başarısızlığını gösteren sorunları açığa çıkarır. VM'ler için Azure İzleyici verimlilik sağlamak için Kapasite planlamasını ve ayarlamayı ve iyileştirmeyi da destekler.

   ![Azure Izleyici VM Performans sekmesi](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-performance/vminsights-performance-aggview-01.png)

Azure Stack ' de çalışan Windows ve Linux VM 'lerinde bulunan uygulama bileşenlerinin görüntülenmesi VM'ler için Azure İzleyici iki şekilde gözlemlenebilir. İlki bir VM 'den doğrudan, ikincisi ise Azure Izleyici 'deki VM grupları arasında yer verilir.
[Uygulama bileşenlerini anlamak için VM'ler için Azure izleyici eşleme kullanma](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-maps) makalesi, iki perspektifle ve harita özelliğinin nasıl kullanılacağı hakkında deneyimi anlamanıza yardımcı olur.

   ![Azure Izleyici VM eşleme sekmesi](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-maps/map-multivm-azure-monitor-01.png)

[VM'ler için Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) herhangi bir performans verisi gösterilmiyorsa, [Loganalytics çalışma alanı](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters) gelişmiş ayarlarınızda Windows ve Linux için performans verileri koleksiyonunu etkinleştirmeniz gerekir.

## <a name="enable-update-management-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Güncelleştirme Yönetimi etkinleştirme
Çok sayıda Azure Stack sanal makinesi varsa, [bu Azure Resource Manager şablonunu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) kullanarak çözümü VM 'lerde daha kolay dağıtabilirsiniz. Şablon, Microsoft Monitoring Agent uzantısını mevcut bir Azure Stack sanal makinesine dağıtır ve var olan bir Azure LogAnalytics çalışma alanına ekler.
 
## <a name="next-steps"></a>Sonraki adımlar
[SQL Server VM performansını iyileştirin](azure-stack-sql-server-vm-considerations.md)
