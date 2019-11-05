---
title: Azure Stack bir Windows sanal makinesi çalıştırın | Microsoft Docs
description: Azure Stack bir Windows sanal makinesini çalıştırmayı öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/01/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 11/01/2019
ms.openlocfilehash: f3c16e202b43f9d672d9f3e385c3f14cf30935e7
ms.sourcegitcommit: 8a74a5572e24bfc42f71e18e181318c82c8b4f24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569133"
---
# <a name="run-a-windows-virtual-machine-on-azure-stack"></a>Azure Stack bir Windows sanal makinesi çalıştırma

Azure Stack içinde bir sanal makine (VM) sağlamak, ağ ve depolama kaynakları dahil olmak üzere VM 'nin yanı sıra bazı ek bileşenler gerektirir. Bu makalede, Azure 'da Windows VM çalıştırmaya yönelik en iyi yöntemler gösterilmektedir.

![Azure Stack 'de Windows VM mimarisi](./media/iaas-architecture-vm-windows/image1.png)

## <a name="resource-group"></a>Kaynak grubu

[Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , ilgili Azure Stack kaynaklarını tutan bir mantıksal kapsayıcıdır. Genel olarak, kaynakları yaşam süresine göre ve onları yönetecek şekilde gruplandırın.

Aynı yaşam döngüsünü aynı [kaynak grubuyla](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)paylaşan yakından ilişkili kaynakları yerleştirin. Kaynak grupları, kaynakları bir grup halinde dağıtıp izlemenize ve faturalandırma maliyetlerini kaynak grubuna göre izlemenize olanak tanır. Kaynakları, test dağıtımları için yararlı olan bir küme olarak da silebilirsiniz. Belirli bir kaynağın bulunmasını ve kaynağın rolünün anlaşılmasını basitleştirmek için anlamlı kaynak adları atayın. Daha fazla bilgi için bkz. [Azure kaynakları Için önerilen adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="virtual-machine"></a>Sanal makine

Yayımlanmış görüntüler listesinden veya Azure Stack blob depolamaya yüklenmiş özel olarak yönetilen bir görüntüden veya sanal sabit diskten (VHD) bir VM sağlayabilirsiniz.

Azure Stack, Azure 'dan farklı sanal makine boyutları sunar. Daha fazla bilgi için bkz. [Azure Stack sanal makineler Için boyutlar](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes). Mevcut bir iş yükünü Azure Stack taşıyorsanız, şirket içi sunucularınız/Azure ile en yakın eşleşen VM boyutuyla başlayın. Daha sonra gerçek iş yükünüzün performansını CPU, bellek ve saniye başına disk giriş/çıkış işlemi (ıOPS) bakımından ölçün ve gereken şekilde ayarlayın.

## <a name="disks"></a>Diskler

Maliyet, sağlanan diskin kapasitesine bağlıdır. IOPS ve aktarım hızı (diğer bir deyişle, veri aktarım hızı) VM boyutuna bağlıdır, bu nedenle bir disk sağladığınızda, üç faktöre (kapasite, ıOPS ve aktarım hızı) göz önünde bulundurun.

Azure Stack üzerinde disk ıOPS (saniye başına giriş/çıkış Işlemi), disk türü yerine [VM boyutunun](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) bir işlevidir. Bu, bir Standard_Fs serisi VM için, SSD veya HDD 'yi disk türü için seçip seçmeksizin bağımsız olarak, tek bir ek veri diski için ıOPS sınırının 2300 ıOPS olduğunu gösterir. Uygulanan ıOPS sınırı, gürültülü komşuları engellemek için bir sınır (en fazla olası). Bu, belirli bir VM boyutuna alacağınız ıOPS 'nin bir güvencesi değildir.

Ayrıca, [yönetilen diskler](https://docs.microsoft.com/azure-stack/user/azure-stack-managed-disk-considerations)kullanmanızı öneririz. Yönetilen diskler, depolama alanını sizin için işleyerek disk yönetimini basitleştirir. Yönetilen diskler depolama hesabı gerektirmez. Yalnızca diskin boyutu ile türünü belirtirsiniz ve disk yüksek oranda kullanılabilir bir kaynak olarak dağıtılır.

İşletim sistemi diski, Azure Stack BLOB depolama alanında depolanan bir VHD 'dir, bu nedenle konak makinesi çalışmıyor olsa bile devam eder. Ayrıca, uygulama verileri için kullanılan kalıcı VHD 'ler olan bir veya daha fazla [veri diski](https://docs.microsoft.com/azure-stack/user/azure-stack-manage-vm-disks)oluşturmanızı öneririz. Mümkünse uygulamaları, işletim sistemi diski yerine bir veri diskine yükleyin. Bazı eski uygulamaların C: sürücüsüne bileşenleri yüklemesi gerekebilir. Bu durumda, PowerShell kullanarak [işletim sistemi diskini yeniden boyutlandırabilirsiniz](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-expand-os-disk) .

VM, geçici bir disk (Windows üzerinde D: sürücüsü) ile de oluşturulur. Bu disk, Azure Stack depolama altyapısındaki geçici bir birimde depolanır. Yeniden başlatmalar ve diğer VM yaşam döngüsü olayları sırasında silinebilir. Bu diski yalnızca sayfa veya takas dosyaları gibi geçici veriler için kullanın.

## <a name="network"></a>Ağ

Ağ bileşenleri aşağıdaki kaynakları içerir:

-   **Sanal ağ**. Her VM, birden çok alt ağa kesimlenebilir bir sanal ağa dağıtılır.

-   **Ağ arabirimi (NIC)** . NIC, sanal makinenin sanal ağla iletişim kurmasına imkan tanır. VM 'niz için birden çok NIC gerekiyorsa, her [VM boyutu](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes)için en fazla sayıda NIC tanımlandığının farkında olun.

-   **Genel IP adresi/VIP**. VM ile iletişim kurmak için genel bir IP adresi gerekir — örneğin, Uzak Masaüstü (RDP) ile. Genel IP adresi dinamik veya statik olabilir. Varsayılan seçenek dinamiktir.

-   Değişmeyen bir IP adresine ihtiyacınız varsa (örneğin, bir DNS ' A ' kaydı oluşturmanız veya IP adresini güvenli bir listeye eklemeniz gerekiyorsa) [statik BIR IP](https://docs.microsoft.com/azure/virtual-network/virtual-networks-reserved-public-ip) adresi ayırın.

-   IP adresi için bir tam etki alanı adı da (FQDN) oluşturabilirsiniz. Daha sonra, DNS 'de FQDN 'ye işaret eden bir [CNAME kaydını](https://en.wikipedia.org/wiki/CNAME_record) kaydedebilirsiniz. Daha fazla bilgi için, [Azure Portal bir tam etki alanı adı oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-portal-create-fqdn)bölümüne bakın.

-   **Ağ güvenlik grubu (NSG)** . NSG 'Ler, VM 'lere ağ trafiğine izin vermek veya bu trafiği reddetmek için kullanılır. NSG 'ler, alt ağlarla veya tek tek sanal makine örnekleriyle ilişkilendirilebilir.

Tüm NSG 'ler, tüm gelen Internet trafiğini engelleyen bir kural dahil olmak üzere [varsayılan kurallar](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)kümesi içerir. Varsayılan kurallar silinemez, ancak diğer kurallar tarafından geçersiz kılınabilir. Internet trafiğini etkinleştirmek için, belirli bağlantı noktalarına gelen trafiğe izin veren kurallar oluşturun — örneğin, HTTP için bağlantı noktası 80. RDP’yi etkinleştirmek için TCP bağlantı noktası 3389’a gelen trafiğe izin veren bir NSG kuralı ekleyin.

## <a name="operations"></a>İşlemler

**Tanılama**. Temel sistem durumu ölçümleri, tanılama altyapı günlükleri ve [önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)dahil olmak üzere izleme ve tanılamayı etkinleştirin. Önyükleme tanılaması, VM’nizin önyükleme gerçekleştiremeyecek bir duruma girmesi durumunda önyükleme hatasını tanılamanıza yardımcı olabilir. Günlükleri depolamak için bir Azure depolama hesabı oluşturun. Tanılama günlükleri için standart bir yerel olarak yedekli depolama (LRS) yeterlidir. Daha fazla bilgi için bkz. [izleme ve tanılamayı etkinleştirme](https://docs.microsoft.com/azure-stack/user/azure-stack-metrics-azure-data).

**Kullanılabilirlik**. Azure Stack işleci tarafından zamanlanan planlı bakım nedeniyle VM 'niz bir yeniden başlatmaya tabi olabilir. Daha yüksek kullanılabilirlik için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure-stack/operator/azure-stack-overview#providing-high-availability)birden çok VM dağıtın.

**Yedeklemeler** Azure Stack IaaS sanal makinelerinizi korumaya yönelik öneriler için, bu makaleye başvurun.

**VM durduruluyor**. Azure’da “durduruldu” ile “serbest bırakıldı” durumları birbirinden farklıdır. Sanal makine durdurulmuş durumdayken ücret ödersiniz, ancak serbest bırakılmış durumdayken ödemezsiniz. Azure Stack portalında **Durdur** düğmesi VM 'yi kaldırır. VM’yi oturumunuz açıkken işletim sistemi aracılığıyla kapatırsanız sanal makine durdurulmasına rağmen **serbest bırakılmaz** ve bu nedenle ücretlendirilmeye devam edersiniz.

**Bir VM 'Yi silme**. Bir VM 'yi silerseniz, VM diskleri silinmez. Bu, sanal makineyi verileri kaybetmeden güvenli bir şekilde silebileceğiniz anlamına gelir. Ancak, depolama ücretlendirilmeye devam edersiniz. VM diskini silmek için yönetilen disk nesnesini silin. Yanlışlıkla silinmeye engel olmak için kaynak *kilidini* kullanarak kaynak grubunun tamamını kilitleyin veya bir VM gibi tek tek kaynakları kilitleyin.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Azure kaynaklarınızın güvenlik durumuna ilişkin merkezi bir görünüm sağlamak için sanal makinelerinizi [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/quick-onboard-azure-stack) 'ne ekleyin. Güvenlik Merkezi olası güvenlik sorunlarını izler ve dağıtımınızın güvenlik durumunun geniş kapsamlı bir resmini sunar. Güvenlik Merkezi, Azure aboneliğine göre yapılandırılır. [Azure aboneliğinizi Güvenlik Merkezi standardına](https://docs.microsoft.com/azure/security-center/security-center-get-started)ekleme bölümünde açıklandığı gibi güvenlik verileri toplamayı etkinleştirin. Veri toplama etkinleştirilirse Güvenlik Merkezi, söz konusu abonelik altında oluşturulmuş tüm VM’leri otomatik olarak tarar.

**Düzeltme Eki Yönetimi**. SANAL makinenizde yama yönetimini yapılandırmak için [Bu](https://docs.microsoft.com/azure-stack/user/vm-update-management) makaleye başvurun. Etkinleştirilirse Güvenlik Merkezi, herhangi bir güvenlik güncelleştirmesinin ve kritik güncelleştirmenin eksik olup olmadığını denetler. Otomatik sistem güncelleştirmelerini etkinleştirmek için VM 'deki [Grup İlkesi ayarlarını](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates) kullanın.

**Kötü amaçlı yazılımdan koruma**. Etkinleştirilirse Güvenlik Merkezi, kötü amaçlı yazılımdan koruma yazılımının yüklü olup olmadığını denetler. Ayrıca Azure portalının içinden kötü amaçlı yazılımdan koruma yazılımlarını yüklemek için de Güvenlik Merkezi’ni kullanabilirsiniz.

**Erişim denetimi**. Azure kaynaklarına erişimi denetlemek için [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) kullanın. RBAC, DevOps takımınızın üyelerine yetkilendirme rolleri atamanıza imkan tanır. Örneğin, Okuyucu rolü Azure kaynaklarını görüntüleyebilir ancak oluşturamaz, yönetemez ve silemez. Bazı izinler bir Azure Kaynak türüne özeldir. Örneğin, Sanal Makine Katılımcısı rolü bir VM’yi yeniden başlatıp serbest bırakabilir, yönetici parolasını sıfırlayabilir, yeni bir VM oluşturabilir ve başka işlemler gerçekleştirebilir. Bu mimari için kullanışlı olabilecek diğer [YERLEŞIK RBAC rolleri](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) [DevTest Labs kullanıcısı](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#devtest-labs-user) ve [ağ katılımcısı](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#network-contributor)içerir.

> [!Note]  
> RBAC, bir sanal makinede oturum açmış kullanıcıların gerçekleştirebileceği eylemleri kısıtlamaz. Bu izinler konuk işletim sistemindeki hesap türüne göre belirlenir.

**Denetim günlükleri**. Sağlama eylemlerini ve diğer VM olaylarını görmek için [etkinlik günlüklerini](https://docs.microsoft.com/azure-stack/user/azure-stack-metrics-azure-data?#activity-log) kullanın.

**Veri şifreleme**. Azure Stack, bekleyen şifreleme kullanarak depolama alt sistemi düzeyinde Kullanıcı ve altyapı verilerini korur. Azure Stack depolama alt sistemi, 128 bit AES şifrelemesi ile BitLocker kullanılarak şifrelenir. Daha fazla bilgi için [Bu](https://docs.microsoft.com/azure-stack/operator/azure-stack-security-bitlocker) makaleye bakın.


## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack VM 'Ler hakkında daha fazla bilgi için bkz. [Azure Stack VM özellikleri](azure-stack-vm-considerations.md).  
- Azure bulut desenleri hakkında daha fazla bilgi edinmek için bkz. [bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns).
