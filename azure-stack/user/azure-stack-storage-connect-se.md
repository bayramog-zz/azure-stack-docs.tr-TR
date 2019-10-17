---
title: Depolama Gezgini bir Azure Stack aboneliğine veya depolama hesabına bağlanın | Microsoft Docs
description: Depolama Gezgini 'ni bir Azure Stack aboneliğine bağlamayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/14/2019
ms.openlocfilehash: 454fe5b07dc5576cecdb11b59e5424e3c5ccbb72
ms.sourcegitcommit: df20662e77a6ed0a7eba03f79eb53e8cd4471206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445385"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Depolama Gezginini bir Azure Stack aboneliğine veya depolama hesabına bağlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Bu makalede, [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)kullanarak Azure Stack aboneliklerinize ve depolama hesaplarınıza nasıl bağlanacağınızı öğreneceksiniz. Depolama Gezgini, Windows, macOS ve Linux 'ta Azure Stack Depolama verileriyle kolayca çalışabilmenizi sağlayan tek başına bir uygulamadır.

> [!NOTE]  
> Azure Stack depolamadan veri taşımak için kullanabileceğiniz çeşitli araçlar vardır. Daha fazla bilgi için bkz. [Azure Stack depolama Için veri aktarımı araçları](azure-stack-storage-transfer.md).

Henüz yüklenmemişse [Depolama Gezgini indirin](https://www.storageexplorer.com/) ve yükleyin.

Bir Azure Stack aboneliğine veya depolama hesabına bağlandıktan sonra, Azure Stack verilerinize çalışmak için [Azure Depolama Gezgini makalelerini](/azure/vs-azure-tools-storage-manage-with-storage-explorer) kullanabilirsiniz. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Azure Stack bağlanmaya hazırlanma

Azure Stack aboneliğine erişebilmek Depolama Gezgini için Azure Stack veya VPN bağlantısına doğrudan erişmeniz gerekir. Azure Stack ile VPN bağlantısı kurma hakkında bilgi edinmek için bkz. [VPN ile Azure Stack’e Bağlanma](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn).

> [!Note]  
> ASDK için, VPN aracılığıyla ASDK 'ye bağlanıyorsanız, VPN kurulum işlemi sırasında oluşturulan kök sertifikayı (CA. cer) kullanmayın.  Bu, DER ile kodlanmış bir sertifikadır ve Depolama Gezgini Azure Stack aboneliklerinizi almasına izin vermez. Depolama Gezgini ile kullanmak üzere Base-64 kodlu bir sertifikayı dışarı aktarmak için aşağıdaki adımları kullanın.

Bağlantısı kesilen tümleşik sistemler ve ASDK için, kök sertifikayı temel 64 biçiminde dışarı aktarmak ve ardından Azure Depolama Gezgini içine aktarmak için dahili bir kurumsal sertifika yetkilisi kullanılması önerilir.  

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Azure Stack sertifikayı dışa ve ardından içeri aktarma

Bağlantısı kesilmiş tümleşik sistemler ve ASDK için Azure Stack sertifikayı dışa aktarın ve içeri aktarın. Bağlı tümleşik sistemler için, sertifika genel olarak imzalanır ve bu adım gerekli değildir.

1. @No__t-0 ' ı bir Azure Stack konak makinesinde veya Azure Stack VPN bağlantısı olan yerel bir makineye açın. 

2. **Dosya**' da, **ek bileşen Ekle/Kaldır**' ı seçin. Kullanılabilir ek bileşenlerde **Sertifikalar** ' ı seçin. 

3. **Bilgisayar hesabı**' nı seçin ve ardından **İleri**' yi seçin. **Yerel bilgisayar**' ı seçin ve ardından **son**' u seçin.

4.  **Konsol root\sertifikası (yerel bilgisayar) \ güvenilen kök sertifika yetkilisi** ' nin altında, **Azurestackselfsignedrootcert**bulun.

    ![mmc.exe dosyası ile Azure Stack kök sertifikasını yükleme](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

5. Sertifikaya sağ tıklayın, **Tüm görevler** > **dışarı aktar**' ı seçin ve ardından sertifikayı, **Base-64 kodlamalı X. 509.440 (. CER)** .

    Dışarı aktarılan sertifika sonraki adımda kullanılır.

6. Depolama Gezgini başlatın. **Azure depolama 'Ya Bağlan** iletişim kutusunu görürseniz, iptal edin.

7. **Düzenle** menüsünde **SSL sertifikaları**' nı seçin ve ardından **sertifikaları içeri aktar**' ı seçin. Dosya seçici iletişim kutusunu kullanarak, önceki adımda dışarı aktardığınız sertifikayı açın.

    Sertifikayı aldıktan sonra Depolama Gezgini yeniden başlatmanız istenir.

    ![Sertifikayı Depolama Gezgini 'ne aktarma](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

8. Depolama Gezgini yeniden başlatıldıktan sonra **Düzenle** menüsünü seçin ve **hedef Azure Stack API 'lerinin** seçili olup olmadığını denetleyin. Değilse, **hedef Azure Stack**' yi seçin ve değişikliğin etkili olması için Depolama Gezgini yeniden başlatın. Bu yapılandırma, Azure Stack ortamınıza uyum için gereklidir.

    ![Hedef Azure Stack’in seçili olduğundan emin olun](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription-with-azure-ad"></a>Azure AD ile Azure Stack aboneliğine bağlanma

Depolama Gezgini Azure Active Directory (Azure AD) hesabına ait olan bir Azure Stack aboneliğine bağlamak için aşağıdaki adımları kullanın.

1. Depolama Gezgini sol bölmesinde, **hesapları Yönet**' i seçin.  
    Oturum açtığınız tüm Microsoft abonelikleri görüntülenir.

2. Azure Stack aboneliğine bağlanmak için **Hesap Ekle**' yi seçin.

    ![Azure Stack hesabı ekleme](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. Azure depolama 'ya Bağlan iletişim kutusundaki Azure **ortamı**altında **Azure**, **Azure Çin 21Vianet**, Azure **Almanya**, **Azure ABD kamu**veya **Yeni ortam ekleme**' yi seçin. Bu, kullanılan Azure Stack hesabına bağlıdır. En az bir etkin Azure Stack aboneliğiyle ilişkili Azure Stack hesabıyla oturum açmak için **oturum aç '** ı seçin.

    ![Azure depolamaya bağlanma](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Bir Azure Stack hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure Stack abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini belirleyin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler** onay kutusunun işaretlenmesi veya temizlenmesi, listelenen Azure Stack aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Özel Bulut Ortamı iletişim kutusunu doldurduktan sonra Azure Stack aboneliklerini seçin](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Azure Stack abonelik hesaplarını içeren depolama hesaplarının listesi](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-subscription-with-ad-fs-account"></a>AD FS hesabıyla Azure Stack aboneliğine bağlanma

> [!Note]  
> Azure Federasyon Hizmeti (AD FS) oturum açma deneyimi, Azure Stack 1804 veya daha yeni güncelleştirme ile Depolama Gezgini 1.2.0 veya daha yeni sürümleri destekler.
Depolama Gezgini bir AD FS hesabına ait olan bir Azure Stack aboneliğine bağlanmak için aşağıdaki adımları kullanın.

1. **Hesapları Yönet**' i seçin. Gezgin, oturum açtığınız Microsoft aboneliklerini listeler.
2. Azure Stack aboneliğine bağlanmak için **Hesap Ekle** ' yi seçin.

    ![Hesap ekleme-Depolama Gezgini](media/azure-stack-storage-connect-se/add-an-account.png)

3. **İleri**’yi seçin. Azure depolama 'ya Bağlan iletişim kutusundaki **Azure ortamı**altında **özel ortam kullan**' ı seçin ve ardından **İleri**' ye tıklayın.

    ![Azure depolama 'ya bağlanma](media/azure-stack-storage-connect-se/connect-to-azure-storage.png)

4. Azure Stack özel ortam için gereken bilgileri girin. 

    | Alan | Notlar |
    | ---   | ---   |
    | Ortam adı | Alan Kullanıcı tarafından özelleştirilebilir. |
    | Azure Resource Manager uç noktası | Azure Stack Geliştirme Seti kaynak uç noktaları Azure Resource Manager örnekleri.<br>İşleçler için: https://adminmanagement.local.azurestack.external <br> Kullanıcılar için: https://management.local.azurestack.external |

    Azure Stack tümleşik sistem üzerinde çalışıyorsanız ve yönetim uç noktanızı bilmiyorsanız, işleçle iletişim kurun.

    ![Hesap ekleme-özel ortamlar](./media/azure-stack-storage-connect-se/custom-environments.png)

5. En az bir etkin Azure Stack aboneliğiyle ilişkili Azure Stack hesabına bağlanmak için **oturum aç '** ı seçin.



6. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini seçin ve ardından **Uygula**' yı seçin.

    ![Hesap yönetimi](./media/azure-stack-storage-connect-se/account-management.png)

    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![İlişkili aboneliklerin listesi](./media/azure-stack-storage-connect-se/list-of-associated-subscriptions.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Azure Stack depolama hesabına bağlanma

Depolama hesabı adını ve anahtar çiftini kullanarak bir Azure Stack depolama hesabına de bağlanabilirsiniz.

1. Depolama Gezgini sol bölmesinde, hesapları Yönet ' i seçin. Oturum açtığınız tüm Microsoft hesapları görüntülenir.

    ![Hesap ekleme-Depolama Gezgini](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Azure Stack aboneliğine bağlanmak için **Hesap Ekle**' yi seçin.

    ![Hesap ekleme-Azure depolama 'ya bağlanma](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. Azure depolama 'ya Bağlan iletişim kutusunda **depolama hesabı adı ve anahtarı kullan**' ı seçin.

4. Hesap adını hesap **adına** girin ve hesap anahtarını **hesap anahtarı** metin kutusuna yapıştırın. Daha sonra, **depolama uç noktaları etki** alanında **diğer (aşağıya girin)** öğesini seçin ve Azure Stack uç noktasını girin.

    Azure Stack uç noktası iki bölümden oluşur: bir bölgenin adı ve Azure Stack etki alanı. Azure Stack Geliştirme Seti, varsayılan uç nokta **Local. azurestack. external**' dır. Uç noktanız hakkında emin değilseniz bulut yöneticinize başvurun.

    ![Ad ve anahtar Ekle](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. **Bağlan**’ı seçin.
6. Depolama hesabı başarıyla eklendikten sonra, depolama hesabı adının sonuna (**dış, diğer**) eklenmiş olarak görüntülenir.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini 'ni kullanmaya başlama](/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Azure Stack depolama: farklar ve konular](azure-stack-acs-differences.md)
* Azure depolama hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure depolama 'Ya giriş](/azure/storage/common/storage-introduction)
