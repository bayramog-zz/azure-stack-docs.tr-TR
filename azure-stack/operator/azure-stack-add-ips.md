---
title: Azure Stack genel IP adresleri ekleme | Microsoft Docs
description: Azure Stack genel IP adresleri eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: justinha
ms.reviewer: scottnap
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: 3978ac3339a2bf6a9dd4df770736bca1a21cc5a3
ms.sourcegitcommit: 38f21e0bcf7b593242ad615c9d8ef8a1ac19c734
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70902658"
---
# <a name="add-public-ip-addresses"></a>Genel IP adresleri ekleme
*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*  

Bu makalede, genel IP adresleri olarak dış adreslere başvuruyoruz. Azure Stack bağlamında, genel IP adresi, Azure Stack dışından erişilebilen bir IP adresidir. Bu dış ağın genel İnternet yönlendirilebilir olup olmadığı veya bir intranette olup olmadığı ve özel adres alanını kullandığı, bu makalenin amaçlarına bakılmaksızın, adımlar aynıdır.

> [!IMPORTANT]
> Bu makaledeki adımlar yalnızca iş ortağı araç seti 1809 veya sonraki bir sürümü kullanılarak dağıtılan sistemler için geçerlidir. Sürüm 1809 ' den önce dağıtılmış sistemler, yeni genel VIP havuz aralığına ızın vermek için raf üstü (TOR) anahtar erişim denetim listelerinin (ACL 'Ler) güncelleştirilmesini gerektirir. Eski anahtar yapılandırması çalıştırıyorsanız, yeni genel IP adreslerinin engellenmesini engellemek için yeni genel IP havuzu için uygun ızın ACL 'Lerini eklemek veya anahtarı en son iş ortağı araç seti 'ni kullanarak yeniden yapılandırmak için OEM 'lerinizle birlikte çalışın.

## <a name="add-a-public-ip-address-pool"></a>Genel IP adresi havuzu ekleme
Azure Stack sisteminin ilk dağıtımından sonra dilediğiniz zaman Azure Stack sisteminize genel IP adresleri ekleyebilirsiniz. Geçerli kullanımın ve genel IP adresi kullanılabilirliğinin Azure Stack nasıl olduğunu görmek için [genel IP adresi tüketimini görüntüleme](azure-stack-viewing-public-ip-address-consumption.md) bölümüne bakın.

Yüksek düzeyde, Azure Stack yeni bir genel IP adresi bloğu ekleme işlemi şuna benzer:

 ![IP akışı Ekle](media/azure-stack-add-ips/flow.PNG)

## <a name="obtain-the-address-block-from-your-provider"></a>Sağlayıcınızdan adres bloğunu alın
Yapmanız gereken ilk şey, Azure Stack eklemek istediğiniz adres bloğunu elde etmek için gereklidir. Adres bloğunu nereden elde ettiğinize bağlı olarak, sağlama süresinin ne olduğunu değerlendirin ve bunu Azure Stack genel IP adreslerini tükettiğinizde ücretle karşılaştırarak bunu yönetin.

> [!IMPORTANT]
> Azure Stack, geçerli bir adres bloğu ise sağladığınız tüm adres bloğunu kabul eder ve Azure Stack var olan bir adres aralığı ile çakışmaz. Lütfen Azure Stack bağlı olduğu dış ağla yönlendirilebilir ve çakışmayan geçerli bir adres bloğu aldığınızdan emin olun. Aralığı Azure Stack ekledikten sonra kaldıramazsınız.

## <a name="add-the-ip-address-range-to-azure-stack"></a>IP adresi aralığını Azure Stack ekleyin

1. Bir tarayıcıda, yönetim portalı panonuza gidin. Bu örnekte, https://adminportal.local.azurestack.external kullanacağız.
2. Azure Stack yönetici portalında Bulut işletmeni olarak oturum açın.
3. Varsayılan panoda, bölge yönetimi listesini bulun ve yönetmek istediğiniz bölgeyi seçin. Bu örnekte, yerel kullanırız.
4. Kaynak sağlayıcıları kutucuğunu bulun ve ağ kaynak sağlayıcısına tıklayın.
5. Genel IP havuzları kullanım kutucuğuna tıklayın.
6. IP Havuzu Ekle düğmesine tıklayın.
7. IP havuzu için bir ad girin. Seçtiğiniz ad, IP havuzunu kolayca belirlemenize yardımcı olur. Adı adres aralığıyla aynı hale getirmek iyi bir uygulamadır, ancak bu gerekli değildir.
8. CıDR gösteriminde eklemek istediğiniz adres bloğunu girin. Örneğin: 192.168.203.0/24
9. Adres aralığı (CıDR bloğu) alanında geçerli bir CıDR aralığı sağladığınızda başlangıç IP adresi, bitiş IP adresi ve kullanılabilir IP adresleri alanları otomatik olarak doldurulur. Bunlar salt okunurdur ve otomatik olarak oluşturulur, böylece adres aralığı alanındaki değeri değiştirmeden bu alanları değiştiremezsiniz.
10. Dikey penceredeki bilgileri gözden geçirdikten ve her şeyin doğru göründüğünü doğruladıktan sonra, değişikliği yürütmek için **Tamam** ' ı seçin ve adres aralığını Azure Stack ekleyin.


## <a name="next-steps"></a>Sonraki adımlar 
[Ölçek birimi düğüm eylemlerini gözden geçirin](azure-stack-node-actions.md).
