---
title: Azure Stack VaaS portalındaki testleri izleme ve yönetme | Microsoft Docs
description: Azure Stack VaaS portalındaki testleri izleyin ve yönetin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 5d3d32df25aeba90ef065fc4363f0887dcbc36e5
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418362"
---
# <a name="monitor-and-manage-tests-in-the-vaas-portal"></a>VaaS portalındaki testleri izleme ve yönetme

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack çözümünüze karşı testleri zamanladıktan sonra, hizmet olarak doğrulama (VaaS), test yürütme durumunu raporlamaya başlayacaktır. Bu bilgiler, VaaS portalında, planlama ve Testleri iptal etme gibi eylemlerle birlikte kullanılabilir.

## <a name="navigate-to-the-workflow-tests-summary-page"></a>İş akışı testleri özet sayfasına gidin

1. Çözümler panosunda, en az bir iş akışına sahip var olan bir çözümü seçin.

    ![İş akışı kutucukları](media/tile_all-workflows.png)

1. İş akışı kutucuğunda **Yönet** ' i seçin. Sonraki sayfada seçili çözüm için oluşturulan iş akışları listelenir.

1. Test özetini açmak için iş akışı adını seçin.

## <a name="change-workflow-parameters"></a>İş akışı parametrelerini değiştir

Herhangi bir iş akışı türü için iş akışı oluşturma sırasında belirtilen [test parametrelerini](azure-stack-vaas-parameters.md#test-parameters) düzenleyebilirsiniz.

1. Testler Özeti sayfasında **Düzenle** düğmesini seçin.

1. [Hizmet olarak Azure Stack doğrulaması Için Iş akışı ortak parametrelerine](azure-stack-vaas-parameters.md)göre yeni değerler sağlayın.

1. Değerleri kaydetmek için **Gönder** ' i seçin.

> [!NOTE]
> **Test geçişi** iş akışında, yeni parametre değerlerini kaydedebilmek için test seçimini ve gözden geçirme sayfasına gitmeniz gerekir.

### <a name="add-tests-test-pass-only"></a>Testler ekle (yalnızca test geçişi)

**Test geçişi** iş akışlarında, hem **testler ekleme** hem de **düzenleme** düğmeleri iş akışında yeni testler zamanlamanıza izin verir.

> [!TIP]
> Yalnızca yeni testler zamanlamak istiyorsanız ve **test geçiş** iş akışı için parametreleri düzenleme gereksinimi yoksa, test **Ekle** ' yi seçin.

## <a name="managing-test-instances"></a>Test örneklerini yönetme

Resmi olmayan çalıştırmalar (yani, **test geçiş** iş akışı) için, testler özet sayfası Azure Stack çözümüne göre zamanlanan testleri listeler.

Resmi çalıştırmalar (yani, **doğrulama** iş akışları) için, testler özet sayfası Azure Stack çözümünün doğrulanmasını tamamlamak için gereken testleri listeler. Doğrulama testleri bu sayfadan zamanlandı.

Her zamanlanmış test örneği aşağıdaki bilgileri gösterir:

| Sütun | Açıklama |
| --- | --- |
| Test adı | Testin adı ve sürümü. |
| Category | Testin amacı. |
| Oluşturuldu | Testin zamanlandığı zaman. |
| Başlatıldı | Testin yürütmeye başladığı zaman. |
| Duration | Testin çalıştırıldığı sürenin uzunluğu. |
| Durum | Testin durumu veya sonucu. Yürütme öncesi veya sürmekte olan durumlar şunlardır: `Pending`,. `Running` Terminal durumları: `Cancelled`, `Failed`, `Aborted`,. `Succeeded` |
| Aracı adı | Testi çalıştıran aracının adı. |
| Toplam işlem | Test sırasında denenen işlemlerin toplam sayısı. |
| Geçilen işlemler | Test sırasında başarılı olan işlem sayısı. |
|  Başarısız İşlemler | Test sırasında başarısız olan işlem sayısı. |

### <a name="actions"></a>Eylemler

Her test örneği, test örnekleri tablosunda **[...]** bağlam menüsünü seçtiğinizde gerçekleştirebileceğiniz kullanılabilir eylemleri listeler.

#### <a name="view-information-about-the-test-definition"></a>Test tanımıyla ilgili bilgileri görüntüle

Test tanımıyla ilgili genel bilgileri görüntülemek için bağlam menüsünde **bilgileri görüntüle** ' yi seçin. Bu, aynı ada ve sürüme sahip her test örneği tarafından paylaşılır.

| Test özelliği | Açıklama |
| -- | -- |
| Test adı | Testin adı. |
| Test sürümü | Testin sürümü. |
| Yayımcı | Testin yayımcısı. |
| Category |  Testin amacı. |
| Hedef hizmetler | Test edilmekte olan Azure Stack Hizmetleri. |
| Açıklama | Testin açıklaması. |
| Tahmini süre (dakika) | Testin beklenen çalışma zamanı. |
| Bağlantılar | Test veya iletişim noktaları hakkındaki ilgili bilgiler. |

#### <a name="view-test-instance-parameters"></a>Test örneği parametrelerini görüntüle

Zamanlama zamanında test örneğine sunulan parametreleri görüntülemek için bağlam menüsünden **parametreleri görüntüle** ' yi seçin. Parolalar gibi hassas dizeler görüntülenmez. Bu eylem yalnızca zamanlanan testler için kullanılabilir.

Bu pencere, tüm test örnekleri için aşağıdaki meta verileri içerir:

| Test örneği özelliği | Açıklama |
| -- | -- |
| Test adı | Testin adı. |
| Test sürümü | Testin sürümü. |
| Test örneği KIMLIĞI | Testin belirli örneğini tanımlayan GUID. |

#### <a name="view-test-instance-operations"></a>Test örneği işlemlerini görüntüleme

Test sırasında gerçekleştirilen işlemlerin ayrıntılı durumunu görüntülemek için bağlam menüsünde **Işlemleri görüntüle** ' yi seçin. Bu eylem yalnızca zamanlanan testler için kullanılabilir.

![İşlemleri görüntüleme](media/manage-test_context-menu-operations.png)

#### <a name="download-logs-for-a-completed-test-instance"></a>Tamamlanmış bir test örneği için günlükleri indirin

Test yürütmesi sırasında günlük çıkışının bir `.zip` dosyasını indirmek için bağlam menüsünde **günlükleri indir** ' i seçin. Bu `Cancelled`eylem yalnızca `Failed` `Succeeded`,,, veya durumunda olan bir test olan, yalnızca tamamlanan testler için kullanılabilir. `Aborted`

#### <a name="reschedule-a-test-instance-or-schedule-a-test"></a>Test örneğini yeniden zamanlama veya test zamanlama

Yönetim sayfasından testlerin zamanlanması, testin çalıştırıldığı iş akışı türüne bağlıdır.

##### <a name="test-pass-workflow"></a>Test geçiş iş akışı

Test geçiş iş akışında, bir test örneğini yeniden **planlama** , özgün test örneği olarak aynı parametre kümesini yeniden kullanır ve kendi günlükleri dahil olmak üzere özgün sonucu *değiştirir* . Yeniden zamanaktardığınızda parolalar gibi hassas dizeleri yeniden girmeniz gerekir.

1. Test örneğini yeniden çizelgelemek üzere bir istem açmak için bağlam menüsünden **yeniden zamanla** ' yı seçin.

1. Geçerli parametreleri girin.

1. Test örneğini yeniden çizelgelemek ve var olan örneği değiştirmek için **Gönder** ' i seçin.

##### <a name="validation-workflows"></a>Doğrulama iş akışları

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

#### <a name="cancel-a-test-instance"></a>Test örneğini iptal et

Zamanlanan bir test, durumu veya `Pending` `Running`ise iptal edilebilir.  

1. Test örneğini iptal etmek üzere bir istem açmak için bağlam menüsünden **iptal** ' i seçin.

1. Test örneğini iptal etmek için **Gönder** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
