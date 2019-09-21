---
title: Hizmet olarak Azure Stack doğrulamasında iş akışı ortak parametreleri | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 091dc657153309298a1eda161b633a42050a0f57
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159352"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Hizmet olarak Azure Stack doğrulaması için iş akışı ortak parametreleri

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Ortak Parametreler, hizmet olarak doğrulama (VaaS) içindeki tüm testlerin gerektirdiği ortam değişkenleri ve Kullanıcı kimlik bilgileri gibi değerleri içerir. Bu değerler iş akışını oluşturduğunuzda veya değiştirirken iş akışı düzeyinde tanımlanır. Testler planlanırken, bu değerler iş akışı altındaki her teste parametre olarak geçirilir.

> [!NOTE]
> Her test kendi parametre kümesini tanımlar. Zamanlama zamanında, bir test ortak parametrelerden bağımsız olarak bir değer girmenizi gerektirebilir veya ortak parametre değerini geçersiz kılmanıza olanak sağlayabilir.

## <a name="environment-parameters"></a>Ortam parametreleri

Ortam parametreleri test altındaki Azure Stack ortamını anlatmaktadır. Bu değerler, test ettiğiniz belirli bir örnek için bir Azure Stack damga bilgi dosyası oluşturarak ve karşıya yükleyerek sağlanmalıdır.

> [!NOTE]
> Resmi doğrulama iş akışlarında, ortam parametreleri iş akışı oluşturulduktan sonra değiştirilemez.

### <a name="generate-the-stamp-information-file"></a>Damga bilgi dosyasını oluşturma

1. DVD veya Azure Stack ortamına erişimi olan herhangi bir makinede oturum açın.
2. Yükseltilmiş bir PowerShell penceresinde aşağıdaki komutları çalıştırın:

    ```powershell  
    $CloudAdminUser = "<cloud admin username>"
    $CloudAdminPassword = ConvertTo-SecureString "<cloud admin password>" -AsPlainText -Force
    $stampInfoCreds = New-Object System.Management.Automation.PSCredential($CloudAdminUser, $CloudAdminPassword)
    $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation' -Credential $stampInfoCreds
    ConvertTo-Json $params > stampinfoproperties.json
    ```

### <a name="locate-values-in-the-ece-configuration-file"></a>ECE yapılandırma dosyasındaki değerleri bulma

Ortam parametresi değerleri de DVD `C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787` üzerinde bulunan **ece yapılandırma dosyasında** el ile bulunabilir.

## <a name="test-parameters"></a>Test parametreleri

Ortak test parametreleri, yapılandırma dosyalarında depolanabilecek hassas bilgileri içerir. Bunların el ile sağlanması gerekir.

Parametre    | Açıklama
-------------|-----------------
Kiracı Yöneticisi Kullanıcı                            | AAD dizininde hizmet yöneticisi tarafından sağlanan Kiracı Yöneticisi Azure Active Directory. Bu Kullanıcı, kaynakları (VM 'Ler, depolama hesapları vb.) ayarlamak ve iş yüklerini yürütmek için şablon dağıtma gibi kiracı düzeyinde eylemler gerçekleştirir. Kiracı hesabını sağlama hakkında ayrıntılı bilgi için bkz. [yeni Azure Stack kiracı ekleme](../operator/azure-stack-add-new-user-aad.md).
Hizmet Yöneticisi kullanıcısı             | Azure Stack dağıtımı sırasında belirtilen Azure AD dizin kiracının Yöneticisi Azure Active Directory. ECE yapılandırma dosyasında arama yapın ve `UniqueName` öğesindeki değeri seçin. `AADTenant`
Bulut Yöneticisi kullanıcısı               | Azure Stack etki alanı yöneticisi hesabı (örneğin, `contoso\cloudadmin`). ECE yapılandırma dosyasında arama yapın ve `UserName` öğesindeki değeri seçin. `User Role="CloudAdmin"`
Tanılama bağlantı dizesi          | Test yürütmesi sırasında tanılama günlüklerinin kopyalanacağı bir Azure depolama hesabına yönelik SAS URL 'SI. SAS URL 'SI oluşturma yönergeleri için bkz. [Tanılama bağlantı dizesi oluşturma](#generate-the-diagnostics-connection-string). |

> [!IMPORTANT]
> Devam etmeden önce **Tanılama bağlantı dizesi** geçerli olmalıdır.

### <a name="generate-the-diagnostics-connection-string"></a>Tanılama bağlantı dizesi oluştur

Tanılama bağlantı dizesi, test yürütmesi sırasında tanılama günlüklerini depolamak için gereklidir. Depolama hesabınıza karşıya yükleme günlüklerini sağlamak üzere bir paylaşılan erişim imzası (SAS) URL 'SI oluşturmak için kurulum sırasında oluşturulan Azure Storage hesabını kullanın (bkz. [doğrulamadan hizmet kaynakları olarak ayarlama](azure-stack-vaas-set-up-resources.md)).

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. **Izin verilen hizmetler seçeneklerinden** **BLOB** ' u seçin. Kalan seçeneklerin seçimini kaldırın.

1. **Izin verilen kaynak türlerindeki** **hizmeti**, **kapsayıcıyı**ve **nesneyi** seçin.

1. **Okuma**, **yazma**, **Listeleme**, **ekleme**, **izin verilen izinlerden** **Oluştur** ' u seçin. Kalan seçeneklerin seçimini kaldırın.

1. **Başlangıç saatini** geçerli saate ve **son saati** , geçerli zamandan üç aya kadar ayarlayın.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]

> [!NOTE]  
> SAS URL 'si, URL oluşturulduğunda belirtilen bitiş saatinde sona erer.  
Testleri zamanlarken, URL 'nin en az 30 gün ve test yürütmesi için gereken süre için geçerli olduğundan emin olun (üç ay önerilir).

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet anahtar kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md) hakkında bilgi edinin