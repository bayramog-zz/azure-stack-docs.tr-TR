---
title: Azure Stack güvenlik denetimleri yapılandırın
description: Azure Stack'te güvenlik denetimlerini yapılandırma hakkında bilgi edinin
services: azure-stack
author: PatAltimore
ms.service: azure-stack
ms.topic: article
ms.date: 06/17/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 06/17/2019
ms.openlocfilehash: c82ba5ca0de27ece5d3bf0ef260efa8d182ad9bf
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419774"
---
# <a name="configure-azure-stack-security-controls"></a>Azure Stack güvenlik denetimleri yapılandırın

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede, Azure Stack'te değiştirilebilir ve uygunsa ödün vurgular güvenlik denetimleri açıklanmaktadır.

Azure Stack mimarisinin iki güvenlik ilkesine yapı taşları yerleşik: varsayılan olarak sağlamlaştırılmış ve ihlal varsayımı. Azure Stack güvenlik daha fazla bilgi için [Azure Stack altyapısını güvenlik duruşunu](azure-stack-security-foundations.md). Azure Stack varsayılan güvenlik duruşunu üretime hazır olsa da, ek sağlamlaştırma gerektiren bazı dağıtım senaryolarında vardır.

## <a name="tls-version-policy"></a>TLS sürüm İlkesi

Ağ üzerinden şifrelenmiş bir iletişim kurmak için yaygın olarak benimsenen bir şifreleme Protokolü Aktarım Katmanı Güvenliği (TLS) protokolüdür. TLS zaman içinde gelişir ve birden çok sürümü yayımlandı. Azure Stack altyapısının, yalnızca kendi iletişim için TLS 1.2 kullanır. Dış arabirimler için Azure Stack, TLS 1.2 kullanmak için şu anda varsayılandır. Ancak, geriye dönük uyumluluk, ayrıca TLS 1.1 aşağı anlaşmasını destekler. ve 1.0. TLS 1.1 ve TLS 1.0 iletişim kurmak için TLS istemci istekleri, Azure Stack için daha düşük TLS sürümü anlaşması tarafından istek geliştirir. İstemcide TLS 1.2 isterse, Azure Stack TLS 1.2 kullanarak TLS bağlantı kurar.

TLS 1.0 ve 1.1 artımlı olarak kaldırılmaktadır veya kuruluşlar ile uyumluluk standartları yasaklanmış 1906 güncelleştirmesiyle başlayarak artık TLS ilke Azure Stack'te yapılandırabilirsiniz. Burada bir sürüm 1.2 düşük TLS oturum kurma girişimleri izin reddedildi ve dağıtılmamış bir TLS 1.2 yalnızca ilke uygulayabilir.

> [!IMPORTANT]
> TLS 1.2 kullanarak yalnızca ilke üretim ortamları için hazırlanmıştır.

## <a name="get-tls-policy"></a>TLS ilkesini Al

Kullanım [ayrıcalıklı uç noktası (CESARETLENDİRİCİ)](azure-stack-privileged-endpoint.md) tüm Azure Stack uç noktalar için TLS İlkesi görüntülemek için:

```powershell
Get-TLSPolicy
```

Örnek çıktı:

    TLS_1.2

## <a name="set-tls-policy"></a>TLS İlkesi ayarlama

Kullanım [ayrıcalıklı uç noktası (CESARETLENDİRİCİ)](azure-stack-privileged-endpoint.md) tüm Azure Stack uç noktalar için TLS ilkesini ayarlamak için:

```powershell
Set-TLSPolicy -Version <String>
```

Parametreler için *kümesi TLSPolicy* cmdlet:

| Parametre | Açıklama | Tür | Gerekli |
|---------|---------|---------|---------|
| *Sürüm* | Azure Stack'te TLS sürümlerini izin | String | evet|

Tüm Azure Stack uç noktaları için izin verilen TLS sürümlerini yapılandırmak için aşağıdaki değerlerden birini kullanın:

| Sürüm değeri | Açıklama |
|---------|---------|
| *TLS_All* | Azure Stack TLS uç noktalar için TLS 1.2 destekler, ancak bu aşağı anlaşma TLS 1.1 ve TLS 1.0 izin verilir. |
| *TLS_1.2* | Azure Stack TLS uç noktaları yalnızca TLS 1.2 destekler. | 

TLS İlkesi güncelleştiriliyor tamamlanması birkaç dakika sürer.

### <a name="enforce-tls-12-configuration-example"></a>TLS 1.2 yapılandırma örneği zorla

Bu örnek yalnızca TLS 1.2 zorlamak için TLS ilkesini ayarlar.

```powershell
Set-TLSPolicy -Version TLS_1.2
```

Örnek çıktı:

    VERBOSE: Successfully setting enforce TLS 1.2 to True
    VERBOSE: Invoking action plan to update GPOs
    VERBOSE: Create Client for execution of action plan
    VERBOSE: Start action plan
    <...>
    VERBOSE: Verifying TLS policy
    VERBOSE: Get GPO TLS protocols registry 'enabled' values
    VERBOSE: GPO TLS applied with the following preferences:
    VERBOSE:     TLS protocol SSL 2.0 enabled value: 0
    VERBOSE:     TLS protocol SSL 3.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.1 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.2 enabled value: 1
    VERBOSE: TLS 1.2 is enforced

### <a name="allow-all-versions-of-tls-12-11-and-10-configuration-example"></a>Tüm sürümleri (1.2, 1.1 ve 1.0) TLS yapılandırma örneği izin ver

Bu örnek, TLS ilkenizi TLS (1.2, 1.1 ve 1.0) tüm sürümlerini izin verecek şekilde ayarlar.

```powershell
Set-TLSPolicy -Version TLS_All
```

Örnek çıktı:

    VERBOSE: Successfully setting enforce TLS 1.2 to False
    VERBOSE: Invoking action plan to update GPOs
    VERBOSE: Create Client for execution of action plan
    VERBOSE: Start action plan
    <...>
    VERBOSE: Verifying TLS policy
    VERBOSE: Get GPO TLS protocols registry 'enabled' values
    VERBOSE: GPO TLS applied with the following preferences:
    VERBOSE:     TLS protocol SSL 2.0 enabled value: 0
    VERBOSE:     TLS protocol SSL 3.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.0 enabled value: 1
    VERBOSE:     TLS protocol TLS 1.1 enabled value: 1
    VERBOSE:     TLS protocol TLS 1.2 enabled value: 1
    VERBOSE: TLS 1.2 is not enforced

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack altyapısını güvenlik durumu hakkında bilgi edinin](azure-stack-security-foundations.md)
- [Azure stack'teki dizilerinizin döndürme hakkında bilgi edinin](azure-stack-rotate-secrets.md)
- [Azure Stack'te güncelleştirme Windows Defender virüsten koruma](azure-stack-security-av.md)