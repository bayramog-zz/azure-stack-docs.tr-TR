---
title: Azure Stack abonelik çoğaltıcısından yararlanarak kaynakları yedekleme | Microsoft Docs
description: Azure Stack abonelik çoğaltıcısından yararlanarak kaynakları nasıl yedekleyeceğinizi öğrenin.
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/29/2019
ms.author: mabrigg
ms.reviewer: rtiberiu
ms.lastreviewed: 10/29/2019
ms.openlocfilehash: 5ef02dbe7683b4c7364811452af59013476687fd
ms.sourcegitcommit: cc5c965b13bc3dae9a4f46a899e602f41dc66f78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73236252"
---
# <a name="how-to-back-up-resources-using-the-azure-stack-subscription-replicator"></a>Azure Stack abonelik çoğaltıcısından yararlanarak kaynakları yedekleme

Kaynakları Azure Stack abonelikleri arasında kopyalamak için Azure Stack abonelik çoğaltıcı PowerShell betiğini kullanabilirsiniz. Replicator betiği, farklı Azure ve Azure Stack aboneliklerinden Azure Resource Manager kaynaklarını okur ve yeniden oluşturur. Bu makalede betiğin nasıl çalıştığı, komut dosyasını nasıl kullanabileceğiniz ve betikteki işlemler için bir başvuru sunan gösterilmektedir.

## <a name="subscription-replicator-overview"></a>Abonelik çoğaltıcısından genel bakış

Azure abonelik Çoğaltıcısı (v3) modüler olacak şekilde tasarlandı. Bu araç, kaynak çoğaltmasını düzenleyen bir çekirdek işlemci kullanır. Ayrıca araç, farklı türlerde kaynakları kopyalamak için şablon görevi gören özelleştirilebilir işlemcileri destekler. 

Çekirdek işlemci aşağıdaki üç komut dosyasından oluşur:

- **resource_retriever. ps1**

    - Çıktı dosyalarını depolamak için klasörler oluşturur.

    - Kaynak aboneliğin bağlamını ayarlar.

    - Kaynakları alır ve **resource_processor. ps1**' ye geçirir.

- **resource_processor. ps1**

    - **Resource_retriever. ps1**tarafından geçirilen kaynağı işler.

    - Hangi özelleştirilmiş işlemciyi kullanacağınızı ve kaynakları geçireceğini belirler.

- **post_process. ps1**

    - Son işlem, özelleştirilmiş işlemcinin oluşturduğu çıktıyı hedef abonelikte dağıtılması için hazırlar.

    - Hedef abonelikte kaynakları dağıtmak için dağıtım kodu oluşturur.

Üç komut dosyası bilgi akışını, daha fazla esneklik sağlamak için standart bir şekilde denetler. Örneğin, ek kaynaklar için destek eklemek, çekirdek işlemcide herhangi bir kodu değiştirmenize gerek yoktur.

Yukarıda bahsedilen özelleştirilmiş işlemciler, belirli bir kaynak türünün nasıl işlenmesi gerektiğini belirten `ps1` dosyalardır. Özelleştirilmiş bir işlemcinin adı her zaman bir kaynaktaki tür verileri kullanılarak adlandırılır. Örneğin, `$vm` bir sanal makine nesnesini tutan varsayıldığında, `$vm`çalışır. Tür `Microsoft.Compute/virtualMachines`yield. Diğer bir deyişle, bir sanal makine için bir işlemci `virtualMachines_processor.ps1`adlandırılırdı, çekirdek işlemcinin hangi özelleştirilmiş işlemciyi kullanacağınızı belirleyen, kaynak meta verilerinde göründüğünden, adın tam olarak aynı olması gerekir.

Özelleştirilmiş bir işlemci, hangi bilgilerin önemli olduğunu belirleyerek bir kaynağın nasıl çoğaltılacağı belirler ve bu bilgilerin kaynak meta verilerden nasıl çekildiğini dikte eder. Daha sonra özelleştirilmiş işlemci ayıklanan tüm verileri alır ve kaynağı hedef abonelikte dağıtmak üzere bir Azure Resource Manager şablonuyla birlikte kullanılacak bir parametre dosyası oluşturmak için kullanır. Bu parametreler dosyası, post_process. ps1 tarafından işlendikten sonra **Parameter_Files** içinde depolanır.

Replicator dosya yapısında **Standardized_ARM_Templates**adlı bir klasör vardır. Kaynak ortama bağlı olarak, dağıtımlar bu standartlaştırılmış Azure Resource Manager şablonlardan birini kullanır veya özelleştirilmiş bir Azure Resource Manager şablonunun oluşturulması gerekir. Bu durumda, özelleştirilmiş bir işlemcinin bir Azure Resource Manager şablonu Oluşturucusu çağırması gerekir. Daha önce başlatılan örnekte, sanal makineler için Azure Resource Manager şablonu oluşturucusunun adı **virtualMachines_ARM_Template_Generator. ps1**olarak adlandırılır. Azure Resource Manager şablonu Oluşturucusu, bir kaynağın meta verilerinde hangi bilgilerin olduğunu temel alan özelleştirilmiş bir Azure Resource Manager şablonu oluşturmaktan sorumludur. Örneğin, sanal makine kaynağı bir kullanılabilirlik kümesinin üyesi olduğunu belirten meta veriler içeriyorsa, Azure Resource Manager şablon Oluşturucusu, bu, bir kullanılabilirlik kümesinin KIMLIĞINI belirten kod içeren bir Azure Resource Manager şablonu oluşturur. sanal makine bir parçasıdır. Bu şekilde, sanal makine yeni aboneliğe dağıtıldığında, dağıtım sırasında kullanılabilirlik kümesine otomatik olarak eklenir. Bu özelleştirilmiş Azure Resource Manager şablonları, **Standardized_ARM_Templates** klasörünün Içinde bulunan **Custom_ARM_Templates** klasöründe depolanır. Post_processor. ps1, bir dağıtımın standartlaştırılmış bir Azure Resource Manager şablonu mı yoksa özelleştirilmiş bir tane mi kullanılacağını belirlemekten sorumludur ve ilgili dağıtım kodunu üretmelidir.

**Post-Process. ps1** betiği, parametre dosyalarını temizletmekten ve kullanıcının yeni kaynakları dağıtmak için kullanacağı betikleri oluşturmaktan sorumludur. Temizleme aşamasında betik, kaynak abonelik KIMLIĞI, kiracı KIMLIĞI ve konuma karşılık gelen hedef değerlerle tüm başvuruları değiştirir. Ardından parametreler dosyası **Parameter_Files** klasörüne çıktı. Ardından, işlenen kaynağın özelleştirilmiş bir Azure Resource Manager şablonu kullanıp kullanmadığını belirler ve **New-AzureRmResourceGroupDeployment** cmdlet 'ini kullanan karşılık gelen dağıtım kodunu üretir. Dağıtım kodu daha sonra **Deployment_Files** klasöründe saklanan **deployresources. ps1** adlı dosyaya eklenir. Son olarak, komut dosyası kaynağın ait olduğu kaynak grubunu belirler ve bu kaynak grubunun dağıtımına yönelik dağıtım kodunun zaten mevcut olup olmadığını görmek için **Deployresourcegroups. ps1** betiğini denetler. Yoksa, kaynak grubunu dağıtmak üzere bu komut dosyasına kod ekler, daha sonra hiçbir şey yapmaz.

### <a name="dynamic-api-retrieval"></a>Dinamik API alımı

Bu aracın, kaynak abonelikte bulunan en yeni kaynak sağlayıcısı API sürümünün hedef abonelikte kaynakları dağıtmak için kullanılabilmesi için yerleşik olarak dinamik API alımı vardır:

![Şekil API 'SI alımı](./media/azure-stack-network-howto-backup-replicator/image1.png)

Şekil API 'SI alımı **resource_processor. ps1**.

Ancak, hedef aboneliğin kaynak sağlayıcısı API sürümünün kaynak aboneliğin daha eski olduğu ve kaynak abonelikten sağlanmakta olan sürümü desteklemediği bir şansınız vardır. Bu durumda, dağıtım çalıştırıldığında bir hata oluşur. Bu sorunu çözmek için, hedef abonelikteki kaynak sağlayıcılarını kaynak aboneliğindekilerle eşleşecek şekilde güncelleştirin.

### <a name="parallel-deployments"></a>Paralel dağıtımlar

Araç **paralel**adlı bir parametre gerektiriyor. Bu parametre, alınan kaynakların paralel olarak dağıtılıp dağıtılmayacağını belirten bir Boole değeri alır. Değer true olarak ayarlanırsa **,** **New-AzureRmResourceGroupDeployment** öğesine yapılan her çağrı için, **-AsJob** bayrağı ve kod blokları, kaynağa göre kaynak dağıtımı kümeleri arasına eklenir türü. Bir sonraki kaynak türü dağıtılmadan önce, bir türdeki tüm kaynakların dağıtılmasından emin olmanızı sağlar. **Paralel** parametre değeri **false**olarak ayarlandıysa, kaynakların hepsi seri olarak dağıtılır.

## <a name="adding-additional-resource-types"></a>Ek kaynak türleri ekleme

Yeni kaynak türleri eklemek basittir. Geliştiricinin özelleştirilmiş bir işlemci ve bir Azure Resource Manager şablonu ya da bir Azure Resource Manager şablon Oluşturucusu oluşturması gerekir. Bu tamamlandıktan sonra geliştirici, kaynak türünü **$ResourceType** parametresi Için validateset 'e ve resource_retriever. ps1 içindeki **$resourceTypes** dizisine eklemesi gerekir. Kaynak türü * * $resourceTypes * * dizisine eklenirken, doğru sırada eklenmelidir. Dizinin sırası, kaynakların dağıtılacağı sırayı belirler, bu nedenle bağımlılıkları göz önünde bulundurun. Son olarak, özelleştirilmiş işlemci bir Azure Resource Manager şablon Oluşturucu kullanıyorsa, **post_process. ps1**içindeki **$customTypes** dizisine kaynak türü adı eklemesi gerekir.

## <a name="running-azure-subscription-replicator"></a>Azure abonelik çoğaltıcısını çalıştırma

Azure abonelik çoğaltıcı (v3) aracını çalıştırmak için, tüm parametreleri sağlamak için resource_retriever. ps1 öğesini açmanız gerekir. **ResourceType** parametresi, bir kaynak türü yerine **Tümünü** seçmek için bir seçenek vardır. **All** seçilirse, resource_retriever. ps1 tüm kaynakları bir sırada işler, böylece dağıtım çalıştırıldığında bağımlı kaynaklar önce dağıtılır. Örneğin sanal makineler sanal makineler tarafından, sanal makinelerin düzgün dağıtılabilmesi için bir VNet 'in yerinde olmasını gerektirdiğinden sanal makinelerden önce dağıtılır.

Betik yürütmeyi bitirdiğinde üç yeni klasör olacaktır, **Deployment_Files**, **Parameter_Files**ve **Custom_ARM_Templates**.

 > [!Note]  
 > Oluşturulan betiklerin hiçbirini çalıştırmadan önce, doğru ortamı ayarlamanız ve hedef abonelikte oturum açmanız (örn. yeni Azure Stack) ve çalışma dizinini **Deployment_Files** klasörüne ayarlamanız gerekir.

Deployment_Files, **Deployresourcegroups. ps1** ve **deployresources. ps1**iki dosya tutacak. DeployResourceGroups. ps1 yürütülmesi kaynak gruplarını dağıtır. DeployResources. ps1 yürütmek, işlenen tüm kaynakları dağıtır. Aracın, kaynak türü olarak **All** veya **Microsoft. COMPUTE/virtualmachines** Ile yürütülmesi durumunda, deployresources. ps1 kullanıcıdan tüm sanal makineleri oluşturmak için kullanılacak bir sanal makine yönetici parolası girmesini ister .

### <a name="example"></a>Örnek

1.  Betiği çalıştırın.

    ![](./media/azure-stack-network-howto-backup-replicator/image2.png)

1.  Betiğin çalıştırılmasını bekleyin.

    ![](./media/azure-stack-network-howto-backup-replicator/image3.png)

1.  Yeni oluşturulan klasörleri gözden geçirin:

    ![](./media/azure-stack-network-howto-backup-replicator/image4.png)

    ![](./media/azure-stack-network-howto-backup-replicator/image5.png)

1.  Bağlamı hedef abonelik olarak ayarlayın.

    ![](./media/azure-stack-network-howto-backup-replicator/image6.png)

1.  **Deployment_Files** klasörüne geçmek için `cd` yazın.

    ![](./media/azure-stack-network-howto-backup-replicator/image7.png)

1.  Kaynak gruplarını dağıtmak için `DeployResourceGroups.ps1` çalıştırın.

    ![](./media/azure-stack-network-howto-backup-replicator/image8.png)

1.  Kaynakları dağıtmak için `DeployResources.ps1` çalıştırın.

    ![](./media/azure-stack-network-howto-backup-replicator/image9.png)

1.  Durumu denetlemek için `Get-Job` çalıştırın. Işi al | Receive-Job sonuçları döndürür.

    ![](./media/azure-stack-network-howto-backup-replicator/image10.png)

## <a name="clean-up"></a>Temizle

ReplicatorV3 klasörünün içinde, **cleanup_generated_items. ps1** adlı bir dosya vardır-bu, **Deployment_Files**, **Parameter_Files**ve **Custom_ARM_Templates** klasörlerini ve tüm içeriğini kaldırır.

## <a name="subscription-replicator-operations"></a>Abonelik çoğaltıcı işlemleri

Azure abonelik çoğaltıcı (v3) Şu anda aşağıdaki kaynak türlerini çoğaltabilir:

- Microsoft. COMPUTE/kullanılabilirliği Bilitysets

- Microsoft.Compute/virtualMachines

- Microsoft.Network/loadBalancers

- Microsoft.Network/networkSecurityGroups

- Microsoft.Network/publicIPAddresses

- Microsoft. Network/routeTables

- Microsoft.Network/virtualNetworks

- Microsoft. Network/Virtualnetworkgateway 'ler

- Microsoft.Storage/storageAccounts

Aracı, kaynak türü olarak **Tümü** ile çalıştırılırken, çoğaltma ve dağıtım sırasında aşağıdaki sıra izlenir (aşağıda, tüm kaynakların yapılandırması çoğaltılır, yani SKU, teklif vb.):

- Microsoft.Network/virtualNetworks

    - Çoğaltır:-tüm adres alanları-tüm alt ağlar

- Microsoft. Network/Virtualnetworkgateway 'ler

    - Çoğaltır:-genel IP yapılandırması-alt ağ yapılandırması-VPN türü-ağ geçidi türü

- Microsoft. Network/routeTables

- Microsoft.Network/networkSecurityGroups

    - Çoğaltır:-tüm güvenlik kuralları gelen ve giden

- Microsoft.Network/publicIPAddresses

- Microsoft.Network/loadBalancers

    - Çoğaltır:-özel IP adresleri-genel IP adresi yapılandırması-alt ağ yapılandırması
    
- Microsoft. COMPUTE/kullanılabilirliği Bilitysets

    - Çoğaltır:-hata etki alanı sayısı-güncelleştirme etki alanı sayısı

- Microsoft.Storage/storageAccounts

- Microsoft.Compute/virtualMachines
    - Tığı  
            -Veri diskleri (veriler olmadan)  
            -Sanal makine boyutu  
            -İşletim sistemi  
            -Tanılama depolama hesabı yapılandırması  
            -Genel IP yapılandırması  
            -Ağ arabirimi  
            -Ağ arabirimi özel IP adresi  
            -Ağ güvenlik grubu yapılandırması  
            -Kullanılabilirlik kümesi yapılandırması  
 
> [!Note]  
> Yalnızca işletim sistemi diski ve veri diskleri için yönetilen diskler oluşturur. Şu anda depolama hesaplarının kullanılmasına yönelik destek yoktur. 

### <a name="limitations"></a>Sınırlamalar

Araç, hedef aboneliğin kaynak sağlayıcıları kaynak abonelikten çoğaltılan tüm kaynakları ve seçenekleri destekledikleri sürece kaynakları bir abonelikten diğerine çoğaltabilir.

Çoğaltmanın başarılı olmasını sağlamak için hedef aboneliğin kaynak sağlayıcısı sürümlerinin kaynak abonelikle eşleştiğinden emin olun.

Ticari Azure 'dan ticari Azure 'a veya Azure Stack içindeki bir abonelikten aynı Azure Stack farklı bir aboneliğe çoğaltma yaparken, depolama hesapları çoğaltılırken sorunlar olur. Bunun nedeni, tüm depolama hesabı adlarının tüm ticari Azure genelinde veya bir Azure Stack bölgesindeki/örnekteki tüm aboneliklerde benzersiz olması nedeniyle depolama hesabı adlandırma gereksinimidir. Depolama hesaplarının farklı Azure Stack örneklerine çoğaltılması, yığınlar ayrı bölgeler/örnekler olduğu için başarılı olur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ağ oluşturma farklılıkları ve konuları](azure-stack-network-differences.md)  