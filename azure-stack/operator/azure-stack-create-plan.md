---
title: Azure Stack bir plan oluşturun | Microsoft Docs
description: Abonelerin sanal makineler sağlamasına izin veren Azure Stack bir plan oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 6cf55f668c36508f59d0d4549c81f3d9056d7749
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159767"
---
# <a name="create-a-plan-in-azure-stack"></a>Azure Stack'te plan oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

[Azure Stack planlar](azure-stack-overview.md) bir veya daha fazla hizmetin ve kotaların gruplanmalarıdır. Bir sağlayıcı olarak kullanıcılarınıza sunmak için planlar oluşturabilirsiniz. Ayrıca, kullanıcılarınızın dahil ettikleri planları, hizmetleri ve kotaları kullanmak için tekliflerinize abone olur. Bu örnek, bilgi işlem, ağ ve depolama kaynak sağlayıcılarını içeren bir plan oluşturmayı gösterir. Bu plan, abonelere sanal makineler sağlama olanağı sunar.

::: moniker range=">=azs-1902"
## <a name="create-a-plan-1902-and-later"></a>Plan oluşturma (1902 ve üzeri)

1. [Azure Stack yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.

2. Kullanıcıların abone olabileceği bir plan ve teklif oluşturmak için **+ kaynak oluştur**' u seçin, sonra da **+ planlar**ve ardından **planlama**yapın.
  
   ![Azure Stack yönetici portalında bir plan seçin](media/azure-stack-create-plan/select-plan.png)

3. Seçilen hizmetlerden her biri için plan adını belirtmenizi, hizmetler eklemenizi ve kotaları tanımlamanızı sağlayan sekmeli bir kullanıcı arabirimi görüntülenir. En önemlisi, oluşturma kararı vermeden önce oluşturduğunuz teklifin ayrıntılarını gözden geçirebilirsiniz.

   **Yeni plan** penceresinin **temel bilgiler** sekmesinde bir **görünen ad** ve bir **kaynak adı**girin. Görünen ad, işleçlerin tarafından görüntülenebilecek kolay addır. Yönetici portalında, plan ayrıntıları yalnızca işleçlere görünür.

   ![Azure Stack yeni plana ilişkin ayrıntıları belirtin](media/azure-stack-create-plan/plan-name.png)

4. Yeni bir kaynak grubu oluşturun veya plan için kapsayıcı olarak var olan bir **kaynak grubunu**seçin.

   ![Azure Stack yeni plan için kaynak grubunu belirtin](media/azure-stack-create-plan/resource-group.png)

5. **Hizmetler** sekmesini seçin veya **ileri ' ye tıklayın: Hizmetler >** düğmesi ve ardından **Microsoft. COMPUTE**, **Microsoft. Network**ve **Microsoft. Storage**onay kutusunu seçin.
  
   ![Azure Stack yeni plan için hizmetleri seçin](media/azure-stack-create-plan/services.png)

6. **Kotalar** sekmesini seçin veya **ileri ' ye tıklayın: Kotalar >** düğmesi. **Microsoft. Storage**' ın yanındaki açılan kutudan varsayılan kotayı seçin ya da **Yeni oluştur** ' u seçerek özelleştirilmiş bir kota oluşturun.
  
   ![Azure Stack yeni plana yönelik kotaları belirtin](media/azure-stack-create-plan/quotas.png)

7. Yeni bir kota oluşturuyorsanız, kota için bir **ad** girin ve ardından kota değerlerini belirtin. Kotayı oluşturmak için **Tamam ' ı** seçin.

   ![Azure Stack yeni plan için yeni kota oluştur](media/azure-stack-create-plan/new-quota.png)

8. **Microsoft. Network** ve **Microsoft. COMPUTE**için kotalar oluşturup atamak üzere 6 ve 7. adımları yineleyin. Her üç hizmetin kotası atandığında, bir sonraki örneğe benzer şekilde görünür.

   ![Azure Stack yeni plan için kota atamalarını doldurun](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Planı gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Doğru olduklarından emin olmak için tüm değerleri ve kotaları gözden geçirin. Arabirim, bir plandaki her kotanın ayrıntılarını görüntülemek için seçili planlardaki kotaları tek seferde genişletmenizi sağlar. Ayrıca, gerekli düzenlemeleri yapmak için geri dönebilirsiniz.

   ![Planı Azure Stack oluşturma](media/azure-stack-create-plan/create.png)

10. Hazırsanız, planı oluşturmak için **Oluştur** ' u seçin.

11. Yeni planı görmek için, sol taraftaki **tüm hizmetler**' e tıklayın, **planlar**' ı seçin ve ardından planı bulun ve adını seçin. Kaynak listeniz uzunsa, planınızı adına göre bulmak için **arama** ' yı kullanın.
::: moniker-end

::: moniker range="<=azs-1901"
## <a name="create-a-plan-1901-and-earlier"></a>Plan oluşturma (1901 ve önceki sürümler)

1. [Azure Stack yönetici portalında](https://adminportal.local.azurestack.external)oturum açın.

2. Kullanıcıların abone olabileceği bir plan ve teklif oluşturmak için **+ Yeni**' yi seçin, ardından **+ planlar**ve ardından **planlama**yapın.
  
   ![Azure Stack yönetici portalında bir plan seçin](media/azure-stack-create-plan/select-plan1901.png)

3. **Yeni plan**altına bir **görünen ad** ve bir **kaynak adı**girin. Görünen ad, planın, kullanıcıların görebileceği kolay adıdır. Yalnızca yönetici, yöneticilerin planla birlikte Azure Resource Manager kaynağı olarak çalışmak için kullanacağı kaynak adını görebilir.

   ![Azure Stack yeni plana ilişkin ayrıntıları belirtin](media/azure-stack-create-plan/plan-name1901.png)

4. Yeni bir kaynak grubu oluşturun veya plan için kapsayıcı olarak var olan bir **kaynak grubunu**seçin.

   ![Azure Stack yeni plan için kaynak grubunu belirtin](media/azure-stack-create-plan/resource-group1901.png)

5. **Hizmetler** ' i seçin ve **Microsoft. COMPUTE**, **Microsoft. Network**ve **Microsoft. Storage**onay kutusunu seçin. Ardından, yapılandırmayı kaydetmek için **Seç** ' i seçin. Fare her bir seçeneğin üzerine geldiğinde onay kutuları belirir.
  
   ![Azure Stack yeni plan için hizmetleri seçin](media/azure-stack-create-plan/services1901.png)

6. **Kotalar**, **Microsoft. Storage (yerel)** ' i seçin ve ardından varsayılan kotayı seçin ya da **yeni kota oluştur** ' u seçerek özelleştirilmiş bir kota oluşturun.
  
   ![Azure Stack yeni plana yönelik kotaları belirtin](media/azure-stack-create-plan/quotas1901.png)

7. Yeni bir kota oluşturuyorsanız kota için bir **ad** girin > kota değerlerini belirtin > **Tamam ' ı**seçin. **Kota oluştur** iletişim kutusu kapanır.

   ![Azure Stack yeni plan için yeni kota oluştur](media/azure-stack-create-plan/new-quota1901.png)

   Ardından oluşturduğunuz yeni kotayı seçersiniz. Kota seçildiğinde bu kota atanır ve seçim iletişim kutusu kapanır.
  
   ![Azure Stack yeni plana yönelik kotayı ata](media/azure-stack-create-plan/assign-quota1901.png)

8. **Microsoft. Network (local)** ve **Microsoft. COMPUTE (local)** için kotalar oluşturup atamak için 6 ve 7. adımları yineleyin. Her üç hizmetin kotası atandığında, bir sonraki örneğe benzer şekilde görünür.

   ![Azure Stack yeni plan için kota atamalarını doldurun](media/azure-stack-create-plan/all-quotas-assigned1901.png)

9. **Kotalar**altında **Tamam**' ı seçin ve ardından **Yeni plan**altında **Oluştur** ' u seçerek planı oluşturun.

    ![Planı Azure Stack oluşturma](media/azure-stack-create-plan/create1901.png)

10. Yeni planınızı görmek için **tüm kaynaklar**' ı seçin ve planı arayın ve adını seçin. Kaynak listeniz uzunsa, planınızı adına göre bulmak için **arama** ' yı kullanın.

    ![Azure Stack yeni planı gözden geçirin](media/azure-stack-create-plan/plan-overview1901.png)
::: moniker-end

## <a name="next-steps"></a>Sonraki adımlar

* [Teklif oluşturma](azure-stack-create-offer.md)
