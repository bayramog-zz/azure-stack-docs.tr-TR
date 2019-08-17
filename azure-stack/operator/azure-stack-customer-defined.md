---
title: Azure Stack anahtar yapılandırmanızda belirli ayarları değiştirme | Microsoft Docs
description: Azure Stack anahtar yapılandırmanızda neleri özelleştirebileceğinizi öğrenin. Özgün donanım üreticisi (OEM) yapılandırmayı oluşturduktan sonra, bunu OEM veya Microsoft Azure Stack mühendislik ekibinden onay olmadan değiştirmeyin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: Femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 08/09/2019
ms.openlocfilehash: 8ed7b3c71c1a9a2098fa125caeb03c949a332f50
ms.sourcegitcommit: 54448d4518c7d9145b8755f51b18d554b3190810
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559347"
---
#  <a name="modify-specific-settings-on-your-azure-stack-switch-configuration"></a>Azure Stack anahtar yapılandırmanızda belirli ayarları değiştirme

Azure Stack anahtar yapılandırmanız için birkaç ortam ayarını değiştirebilirsiniz. Özgün donatım üreticiniz (OEM) tarafından oluşturulan şablonda değişiklik yaptığınız ayarları belirleyebilirsiniz. Bu makalede, bu özelleştirilebilir ayarların her biri ve değişikliklerin Azure Stack nasıl etkilediği açıklanmaktadır. Bu ayarlar parola güncelleştirme, Syslog sunucusu, SNMP izleme, kimlik doğrulama ve erişim denetim listesi içerir. 

Azure Stack çözümünün dağıtımı sırasında, özgün ekipman üreticisi (OEM), hem Yöneticiler hem de BMC için anahtar yapılandırması oluşturur ve uygular. OEM, gerekli yapılandırmaların bu cihazlarda doğru şekilde ayarlandığını doğrulamak için Azure Stack Otomasyon aracını kullanır. Yapılandırma Azure Stack [dağıtım çalışma sayfanızdaki](azure-stack-deployment-worksheet.md)bilgileri temel alarak. OEM, yapılandırmayı oluşturduktan sonra, OEM veya Microsoft Azure Stack mühendislik ekibinin onayını olmadan yapılandırmayı değiştirmeyin. Ağ aygıtı yapılandırmasında yapılan bir değişiklik, Azure Stack örneğindeki ağ sorunlarının çalışmasını veya sorun gidermeyi önemli ölçüde etkileyebilir.

Ancak, ağ anahtarlarının yapılandırmasında eklenebilen, kaldırılacak veya değiştirilebilen bazı değerler vardır.

>[!Warning]  
> Yapılandırmayı OEM veya Microsoft Azure Stack mühendislik ekibinden onay olmadan değiştirmeyin. Ağ aygıtı yapılandırmasında yapılan bir değişiklik, Azure Stack örneğindeki ağ sorunlarının çalışmasını veya sorun gidermeyi önemli ölçüde etkileyebilir.
>
> Ağ cihazınızda bu işlevler hakkında daha fazla bilgi için, bu değişiklikleri nasıl yapacağınız hakkında daha fazla bilgi için lütfen OEM donanım sağlayıcınıza veya Microsoft destek 'e başvurun. OEM 'niz, Azure Stack dağıtım çalışma sayfanıza göre Otomasyon Aracı tarafından oluşturulan yapılandırma dosyasına sahiptir. 

## <a name="password-update"></a>Parola güncelleştirme

İşleci, ağ anahtarları üzerindeki herhangi bir kullanıcının parolasını dilediğiniz zaman güncelleştirebilir. Azure Stack sisteminde herhangi bir bilgiyi değiştirme veya [Azure Stack gizli dizileri döndürme](azure-stack-rotate-secrets.md)adımlarını kullanma gereksinimi yoktur.

## <a name="syslog-server"></a>Syslog sunucusu

İşleçler, veri merkezindeki bir Syslog sunucusuna anahtar günlüklerini yönlendirebilir. Zaman içinde belirli bir noktadaki günlüklerin sorun giderme için kullanılabilir olduğundan emin olmak için bu yapılandırmayı kullanın. Varsayılan olarak, Günlükler anahtarlar üzerinde depolanır; günlükleri depolamanın kapasitesi sınırlıdır. Anahtar yönetimi erişimi için izinleri yapılandırmaya genel bir bakış için [erişim denetim listesi güncelleştirmeleri](#access-control-list-updates) bölümüne bakın.

## <a name="snmp-monitoring"></a>SNMP izleme

İşleci, basit ağ Yönetim Protokolü (SNMP) v2 veya v3 'i ağ cihazlarını izleyip veri merkezindeki bir ağ izleme uygulamasına tuzak gönderecek şekilde yapılandırabilir. Güvenlik nedenleriyle, v2 'den daha güvenli olduğundan SNMPv3 'yi kullanın. Mıbs ve gereken yapılandırma için OEM donanım sağlayıcınıza danışın. Anahtar yönetimi erişimi için izinleri yapılandırmaya genel bir bakış için [erişim denetim listesi güncelleştirmeleri](#access-control-list-updates) bölümüne bakın.

## <a name="authentication"></a>Authentication

İşleci, ağ cihazlarında kimlik doğrulamasını yönetmek için RADIUS veya TACACS yapılandırabilir. Desteklenen yöntemler ve yapılandırma için OEM donanım sağlayıcınıza danışın.  Anahtar yönetimi erişimi için izinleri yapılandırmaya genel bir bakış için [erişim denetim listesi güncelleştirmeleri](#access-control-list-updates) bölümüne bakın.

## <a name="access-control-list-updates"></a>Erişim denetim listesi güncelleştirmeleri

İşleci, ağ cihazı yönetim arabirimlerine ve donanım yaşam döngüsü konağına (HLH) güvenilen bir veri merkezi ağ aralığından erişime izin vermek için bazı erişim denetim listelerini (ACL 'ler) değiştirebilir. İşleci hangi bileşenin ulaşılabildiğini ve nereden hangilerinin olacağını seçebilir. Erişim denetim listesi ile operatör, belirli bir ağ aralığı içindeki yönetim sıçrama kutusu VM 'lerinin, anahtar yönetimi arabirimine ve HLH işletim sisteminin yanı sıra HLH BMC 'ye erişmesine izin verebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack veri merkezi tümleştirmesi-DNS](azure-stack-integrate-dns.md)