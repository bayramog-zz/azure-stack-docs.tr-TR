---
title: Azure 'da ve Azure Stack hub 'ında şirket içi verileri kullanarak çapraz bulutu ölçeklendiryen bir uygulama oluşturma düzeni.
description: Şirket içi verileri kullanan ölçeklenebilir bir çoklu bulut uygulaması oluşturmak için Azure ve Azure Stack hub 'ı kullanmayı öğrenin.
author: BryanLa
ms.service: azure-stack
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 75a72deac409013b3fcd77693c3a7ceb2d2497cc
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638689"
---
# <a name="cross-cloud-scaling-on-premises-data-pattern"></a>Platformlar arası ölçeklendirme (Şirket içi veriler) deseninin

Azure ve Azure Stack hub 'ına yayılan bir karma uygulama oluşturmayı öğrenin. Bu model ayrıca uyumluluk için tek bir şirket içi veri kaynağını nasıl kullanacağınızı gösterir.

## <a name="context-and-problem"></a>Bağlam ve sorun

Birçok kuruluş, büyük miktarlarda müşterilere duyarlı verileri toplar ve depolar. Genellikle, kurumsal yönetmelikler veya kamu ilkesi nedeniyle gizli verileri genel bulutta depolamaktan engellenirler. Bu kuruluşlar, genel bulutun ölçeklenebilirliğinden de faydalanmak istiyor. Genel bulut trafikte her zaman en yüksek noktaları işleyebilir ve müşterilerin ihtiyaç duydukları donanım için ihtiyaç duyduklarında bu şekilde ödeme yapmasına olanak tanır.

## <a name="solution"></a>Çözüm

Çözüm, özel bulutun uyumluluk avantajlarından yararlanır ve bunları genel bulutun ölçeklenebilirliği ile birleştirerek gerçekleştirir. Azure ve Azure Stack Merkez karma bulutu, geliştiriciler için tutarlı bir deneyim sağlar. Bu tutarlılık, kendilerine hem genel buluta hem de şirket içi ortamlara yönelik yeteneklerini uygulama olanağı sağlar.

Çözüm dağıtım kılavuzu, aynı Web uygulamasını ortak ve özel bir buluta dağıtmanıza olanak tanır. Özel bulutta barındırılan internet 'e ait olmayan bir ağa da erişebilirsiniz. Web uygulamaları yükleme için izlenir. Trafikte önemli bir artış olduğunda, bir program trafiği genel buluta yeniden yönlendirmek için DNS kayıtlarını yönetir. Trafik artık önemli olmadığında DNS kayıtları, trafiği özel buluta geri yönlendirmek üzere güncelleştirilir.

[Şirket içi veri düzeniyle platformlar arası ölçeklendirmeyi ![](media/pattern-cross-cloud-scale-onprem-data/solution-architecture.png)](media/pattern-cross-cloud-scale-onprem-data/solution-architecture.png)

## <a name="components"></a>Bileşenler

Bu çözüm aşağıdaki bileşenleri kullanır:

| Katmanı | Bileşen | Açıklama |
|----------|-----------|-------------|
| Azure | Azure App Service | [Azure App Service](/azure/app-service/) Web uygulamaları, yeniden derde API uygulamaları ve Azure işlevleri oluşturmanıza ve barındırmanıza olanak tanır. Altyapıyı yönetmeksizin tercih ettiğiniz programlama dilinde tümü. |
| | Azure Sanal Ağı| [Azure sanal ağı (VNet)](/azure/virtual-network/virtual-networks-overview) , Azure 'daki özel ağlar için temel yapı taşdır. VNet, sanal makineler (VM) gibi birden çok Azure Kaynak türünün birbirleriyle, internet ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar. Çözüm ek ağ bileşenlerinin kullanımını da gösterir:<br>-uygulama ve ağ geçidi alt ağları<br>-Yerel bir şirket içi ağ geçidi<br>-siteden siteye VPN Ağ Geçidi bağlantısı görevi gören bir sanal ağ geçidi<br>-Genel IP adresi<br>-Noktadan siteye VPN bağlantısı<br>-DNS etki alanlarını barındırmak ve ad çözümlemesi sağlamak için Azure DNS |
| | Azure Traffic Manager | [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) , DNS tabanlı bir trafik yük dengeleyicidir. Farklı veri merkezlerindeki hizmet uç noktaları için Kullanıcı trafiğinin dağıtımını denetlemenize olanak tanır. |
| | Azure Application Insights | [Application Insights](/azure/azure-monitor/app/app-insights-overview) , Web geliştiricileri için uygulamaları birden çok platformda oluşturup yöneten genişletilebilir bir uygulama performansı yönetim hizmetidir.|
| | Azure İşlevleri | [Azure işlevleri](/azure/azure-functions/) , öncelıkle bir VM oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda yürütmeniz için izin verir. |
| | Azure Otomatik Ölçeklendirme | [Otomatik ölçeklendirme](/azure/azure-monitor/platform/autoscale-overview) Cloud Services, sanal makineler ve Web uygulamalarının yerleşik bir özelliğidir. Özelliği, uygulamaların talep değiştiğinde en iyi şekilde gerçekleştirmesini sağlar. Uygulamalar, trafik artışlarını ayarlar, ölçüm değiştiğinde size bildirimde bulunmak ve gerektiğinde ölçeklendirirsiniz. |
| Azure Stack hub 'ı | IaaS Işlem | Azure Stack hub, Azure tarafından etkinleştirilen aynı uygulama modelini, Self Servis Portalını ve API 'Leri kullanmanıza olanak sağlar. Azure Stack hub IaaS, tutarlı karma bulut dağıtımları için çok çeşitli açık kaynaklı teknolojiler sağlar. Çözüm örneği, örneğin SQL Server için bir Windows Server VM kullanır.|
| | Azure App Service | Aynı Azure Web uygulaması gibi, çözüm, Web uygulamasını barındırmak için [Azure Stack hub 'ında Azure App Service](/azure-stack/operator/azure-stack-app-service-overview) kullanır. |
| | Networking (Ağ İletişimi) | Azure Stack hub sanal ağı, tam olarak Azure sanal ağı gibi çalışmaktadır. Özel ana bilgisayar adları da dahil olmak üzere aynı ağ bileşenlerinden birçoğunu kullanır. 
| Azure DevOps Services | Kaydol | Derleme, test ve dağıtım için hızlı bir şekilde sürekli tümleştirme ayarlayın. Daha fazla bilgi için bkz. [kaydolma, Azure DevOps 'da oturum açma](/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops). |
| | Azure Pipelines | Sürekli tümleştirme/sürekli teslim için [Azure Pipelines](/azure/devops/pipelines/agents/agents?view=azure-devops) kullanın. Azure Pipelines barındırılan derleme ve sürüm aracılarını ve tanımlarını yönetmenizi sağlar. |
| | Kod deposu | Geliştirme işlem hattınızı kolaylaştırmak için birden çok kod depolarından yararlanın. GitHub, Bitbucket, Dropbox, OneDrive ve Azure Repos içindeki mevcut kod depolarını kullanın. |

## <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler

Bu çözümü nasıl uygulayacağınıza karar verirken aşağıdaki noktaları göz önünde bulundurun:

### <a name="scalability"></a>Ölçeklenebilirlik 

Azure ve Azure Stack hub, günümüzün küresel olarak dağıtılan iş ihtiyaçlarını desteklemeye yönelik olarak tasarlanmıştır.

**Sorun olmadan karma bulut**

Microsoft, tek bir çözüm içinde Azure Stack hub ve Azure ile şirket içi varlıkların hiç bir tümleştirmesini sunmaktadır. Bu tümleştirme, birden çok nokta çözümünü ve bulut sağlayıcılarının bir karışımını yönetmeyi ortadan kaldırır. Azure 'un gücü, platformlar arası ölçeklendirmeyle yalnızca birkaç tıklamayla oluşur. Bulut patlaması ile Azure Stack hub 'ınızı Azure 'a bağlayın, verileriniz ve uygulamalarınız gerektiğinde Azure 'da kullanılabilir.

- İkincil bir DR sitesi oluşturma ve sürdürme gereksinimini ortadan kaldırın.
- Azure 'da bant yedeklemesini ve 99 yıla kadar yedekleme verilerini ortadan kaldırarak zamandan ve paradan tasarruf edin.
- Çalışan Hyper-V, fiziksel (önizlemede) ve VMware (önizlemede) iş yüklerinden yararlanarak bulutun ekonomisi ve esnekliğinden yararlanın.
- Üretim iş yüklerini etkilemeden, Azure 'da şirket içi varlığınızın çoğaltılan bir kopyasında işlem yoğunluğu raporlarını veya analizlerini çalıştırın.
- Gerektiğinde daha büyük işlem şablonlarıyla, buluta veri bloğu yapın ve şirket içi iş yüklerini Azure 'da çalıştırın. Karma, ihtiyacınız olduğunda ihtiyacınız olan gücü size sağlar.
- Birkaç tıklamayla Azure 'da çok katmanlı geliştirme ortamları oluşturun; hatta canlı üretim verilerini neredeyse gerçek zamanlı eşitlemeye devam etmek için geliştirme ve test ortamınıza çoğaltın.

**Azure Stack hub ile platformlar arası ölçeklendirme ekonomisi**

Bulut patlaması 'nın temel avantajı ekonomik tasarruflar. Yalnızca bu kaynaklar için bir talep olduğunda ek kaynaklar için ödeme yaparsınız. Gereksiz ek kapasiteye göre daha fazla harcama yoktur ya da isteğe bağlı tepe noktaları ve dalgalanmalar tahmin edilmeye çalışılıyor.

**Yüksek talep yüklerini buluta azaltma**

Platformlar arası ölçeklendirme, burfe 'leri işlemek için kullanılabilir. Yük, temel uygulamaları ortak buluta taşıyarak, iş açısından kritik uygulamalar için yerel kaynakları boşaltarak dağıtılır. Bir uygulama özel buluta uygulanabilir, daha sonra yalnızca talepleri karşılamak için gerektiğinde genel buluta veri bloğu oluşturabilir.


### <a name="availability"></a>Erişilebilirlik

Genel dağıtımın, değişken bağlantısı ve bölgeye göre farklı kamu düzenlemeleri gibi kendi zorlukları vardır. Geliştiriciler yalnızca bir uygulama geliştirebilir ve bunu farklı gereksinimlerle farklı nedenlerle dağıtabilir.  Uygulamanızı Azure genel bulutuna dağıtın, ardından ek örnekleri veya bileşenleri yerel olarak dağıtın. Azure kullanarak tüm örnekler arasındaki trafiği yönetebilirsiniz.

### <a name="manageability"></a>Yönetilebilirlik

**Tek, tutarlı bir geliştirme yaklaşımı**

Azure ve Azure Stack hub, kuruluş genelinde tutarlı bir geliştirme araçları kümesi kullanmanıza olanak sağlar. Bu tutarlılık, sürekli tümleştirme ve sürekli geliştirme (CI/CD) için bir uygulama uygulamayı kolaylaştırır. Azure 'da veya Azure Stack hub 'da dağıtılan birçok uygulama ve hizmet birbirinin yerini alabilir ve herhangi bir konumda sorunsuz bir şekilde çalışabilir.

Karma CI/CD işlem hattı size yardımcı olabilir:
- Kod deponuzda kod yürütmelerine dayalı olarak yeni bir derleme başlatın.
- Yeni oluşturulan kodunuzu kullanıcı kabul testi için Azure 'a otomatik olarak dağıtın.
- Kodunuzun testi başarılı olduktan sonra Azure Stack hub 'ına otomatik olarak dağıtın.

**Tek, tutarlı bir kimlik yönetimi çözümü**

Azure Stack hub Azure Active Directory ve Active Directory Federasyon Hizmetleri (AD FS) (ADFS) ile birlikte kullanılabilir. Azure Stack hub, bağlantılı senaryolarda Azure Active Directory ile birlikte kullanılabilir. Bağlantısı olmayan ortamlar için, ADFS 'yi bağlantısı kesik bir çözüm olarak kullanabilirsiniz. Hizmet sorumluları, uygulamalara erişim vermek için kullanılır ve bunlara Azure Resource Manager aracılığıyla kaynak dağıtıp yapılandırabilir. 

### <a name="security"></a>Güvenlik

**Uyumluluk ve veri egemenliği sağlayın**

Azure Stack hub, bir genel bulut kullanırken olduğu gibi birden çok ülkede aynı hizmeti çalıştırmanızı sağlar. Her ülkede veri merkezlerinde aynı uygulamayı dağıtmak, veri egemenlik gereksinimlerinin karşılanmasını sağlar. Bu özellik, kişisel verilerin her bir ülkenin kenarlıkları içinde tutulmasını sağlar.

**Azure Stack hub-güvenlik sonrası**

Kesintisiz ve sürekli bir bakım süreci olmadan güvenlik duruşu yoktur. Bu nedenle, Microsoft, tüm altyapıda sorunsuz bir şekilde yama ve güncelleştirme uygulayan bir düzenleme altyapısına yatırım yapmış olur.

Microsoft, Azure Stack hub OEM iş ortakları sayesinde aynı güvenlik duruşunu OEM 'ye özgü bileşenlere genişletir. Donanım yaşam döngüsü Konağı ve bunun üzerine çalışan yazılım gibi. Bu iş ortaklığı, Azure Stack hub 'ının tüm altyapıda Tekdüzen, katı bir güvenlik duruşunu olmasını sağlar. Bu durumda, müşteriler uygulama iş yüklerini derleyip güvenli hale getirebilirsiniz.

**PowerShell, CLı ve Azure portal ile hizmet sorumlularını kullanma**

Bir betik veya uygulamaya kaynak erişimi vermek için, uygulamanız için bir kimlik ayarlayın ve uygulamanın kimliğini kendi kimlik bilgileriyle doğrulayın. Bu kimlik bir hizmet sorumlusu olarak bilinir ve şunları yapmanızı sağlar:

- Uygulama kimliğine kendi izinlerinize göre izinler atayın ve uygulamanın ihtiyaçlarını tam olarak kısıtlanmıştır.
- Katılımsız bir betik yürütürken kimlik doğrulaması için sertifika kullanabilirsiniz. 

Hizmet sorumlusu oluşturma ve kimlik bilgileri için bir sertifika kullanma hakkında daha fazla bilgi için bkz. [kaynaklara erişmek için uygulama kimliği kullanma](/azure-stack/operator/azure-stack-create-service-principals).

## <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar 

- Kuruluşum DevOps yaklaşımını kullanıyor ya da yakın bir tarihte planlanmış bir tane var.
- Azure Stack Hub uygulaması ve genel bulut arasında CI/CD uygulamaları uygulamak istiyorum.
- CI/CD işlem hattını bulutta ve şirket içi ortamlarda birleştirmek istiyorum.
- Bulut veya şirket içi hizmetleri kullanarak uygulamaları sorunsuz bir şekilde geliştirmeye olanak istiyorum.
- Bulut ve şirket içi uygulamalarda tutarlı geliştirici becerilerinden yararlanmak istiyorum.
- Azure kullanıyorum, ancak şirket içi Azure Stack hub bulutu üzerinde çalışan geliştiricilerim var.
- Şirket içi Uygulamalarım, dönemsel, döngüsel veya öngörülemeyen dalgalanmalar sırasında isteğe bağlı olarak ani artışlar yaşar.
- Şirket içi bileşenim var ve bunları sorunsuz bir şekilde ölçeklendirmek için bulutu kullanmak istiyorum.
- Bulut ölçeklenebilirliği istiyorum, ancak uygulamamın şirket içinde mümkün olduğunca çalışmasını istiyorum.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tanıtılan konular hakkında daha fazla bilgi edinmek için:
- Bu düzenin nasıl kullanıldığına genel bir bakış için [veri merkezleri ve genel bulut arasındaki uygulamaları dinamik olarak ölçeklendirin](https://www.youtube.com/watch?v=2lw8zOpJTn0) .
- En iyi uygulamalar hakkında daha fazla bilgi edinmek ve ek soruları yanıtlamak için bkz. [karma uygulama tasarımı konuları](overview-app-design-considerations.md) .
- Bu model Azure Stack hub dahil olmak üzere Azure Stack ürün ailesini kullanır. Ürünlerin ve çözümlerin tamamı hakkında daha fazla bilgi edinmek için [ürün ve çözümlerin Azure Stack ailesine](/azure-stack)bakın.

Çözüm örneğini test etmeye hazırsanız, [platformlar arası ölçeklendirme (Şirket içi veriler) Çözüm dağıtım kılavuzu](solution-deployment-guide-cross-cloud-scaling-onprem-data.md)ile devam edin. Dağıtım Kılavuzu, bileşenlerinin dağıtılması ve test edilmesi için adım adım yönergeler sağlar.