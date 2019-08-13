---
title: Azure Stack 'de Python ile API sürüm profillerini kullanma | Microsoft Docs
description: Azure Stack 'de Python ile API sürüm profillerini nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 05/16/2019
<!-- dev: viananth -->
ms.openlocfilehash: 35ce331c29e89af3a81396a9658cf8a0f29018d3
ms.sourcegitcommit: 58c28c0c4086b4d769e9d8c5a8249a76c0f09e57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959428"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Azure Stack 'de Python ile API sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

## <a name="python-and-api-version-profiles"></a>Python ve API sürüm profilleri

Python SDK, Azure Stack ve küresel Azure gibi farklı bulut platformlarını hedeflemek için API sürüm profillerini destekler. Karma bulut için çözümler oluştururken API profillerini kullanın. Python SDK aşağıdaki API profillerini destekler:

- **sürümü**  
    Bu profil, Azure platformunda tüm hizmet sağlayıcıları için en son API sürümlerini hedefler.
- **2019-03-01-karma**  
    Bu profil, 1904 veya üzeri damga sürümleri için Azure Stack platformunda tüm kaynak sağlayıcılarının en son API sürümlerini hedefler.
- **2018-03-01-karma**  
    Bu profil, Azure Stack platformunda tüm kaynak sağlayıcıları için en uyumlu API sürümlerini hedefler.
- **2017-03-09-profil**  
    Bu profil, Azure Stack tarafından desteklenen kaynak sağlayıcılarının en uyumlu API sürümlerini hedefler.

   API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [Azure Stack API sürüm profillerini yönetme](azure-stack-version-profiles.md).

## <a name="install-the-azure-python-sdk"></a>Azure Python SDK 'sını yükler

1. [Resmi siteden](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)git 'i yükler.
2. Python SDK 'nın nasıl yükleneceğine ilişkin yönergeler için bkz. [Python geliştiricileri Için Azure](/python/azure/python-sdk-azure-install?view=azure-python).
3. Kullanılabilir değilse, bir abonelik oluşturun ve abonelik KIMLIĞINI daha sonra kullanmak üzere kaydedin. Abonelik oluşturma yönergeleri için, bkz. [Azure Stack tekliflere abonelik oluşturma](../operator/azure-stack-subscribe-plan-provision-vm.md).
4. Bir hizmet sorumlusu oluşturun ve KIMLIĞINI ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../operator/azure-stack-create-service-principals.md).
5. Hizmet sorumlunuzu aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusuna rol atama hakkında yönergeler için bkz. [Azure Stack uygulamalarına erişim sağlama](../operator/azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Önkoşullar

Python Azure SDK 'sını Azure Stack ile kullanmak için, aşağıdaki değerleri sağlamanız ve sonra değerleri ortam değişkenleriyle ayarlamanız gerekir. Ortam değişkenlerini ayarlamak için, belirli işletim sisteminize ait tabloyu izleyen yönergelere bakın.

| Value | Ortam değişkenleri | Açıklama |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kimliği | `AZURE_TENANT_ID` | Azure Stack [KIRACı kimliğinizin](../operator/azure-stack-identity-overview.md)değeri. |
| İstemci Kimliği | `AZURE_CLIENT_ID` | Hizmet sorumlusu bu makalenin önceki bölümünde oluşturulduğunda kaydedilen hizmet sorumlusu uygulama KIMLIĞI. |
| Abonelik Kimliği | `AZURE_SUBSCRIPTION_ID` | [ABONELIK kimliği](../operator/azure-stack-plan-offer-quota-overview.md#subscriptions) , Azure Stack tekliflere nasıl erişirsiniz. |
| İstemci Gizli Anahtarı | `AZURE_CLIENT_SECRET` | Hizmet sorumlusu oluşturulduğunda kaydedilen hizmet sorumlusu uygulama gizli anahtarı. |
| Resource Manager uç noktası | `ARM_ENDPOINT` | [Azure Stack Kaynak Yöneticisi uç noktasına](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint)bakın. |
| Kaynak Konumu | `AZURE_RESOURCE_LOCATION` | Azure Stack ortamınızın kaynak konumu.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

ASDK kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Tümleşik sistemlerle CA kök sertifikasına güvenmeniz gerekmez.

#### <a name="windows"></a>Windows

1. Makinenizde Python sertifika deposu konumunu bulun. Konum, Python 'u yüklediğiniz yere göre farklılık gösterebilir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```PowerShell  
      python -c "import certifi; print(certifi.where())"
    ```

    Sertifika deposu konumunu bir yere getirin. Örneğin, *~/lib/python3.5/site-packages/certifi/CAcert.pem*. Belirli yolunuz, işletim sistemine ve yüklediğiniz Python sürümüne bağlıdır.

2. Azure Stack CA kök sertifikasına, var olan Python sertifikasına ekleyerek güvenin.

    ```powershell
    $pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

    $root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $root.Import($pemFile)

    Write-Host "Extracting required information from the cert file"
    $md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
    $sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
    $sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

    $issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
    $subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
    $labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
    $serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
    $md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
    $sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
    $sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
    $certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

    $rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
    $serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

    Write-Host "Adding the certificate content to Python Cert store"
    Add-Content "${env:ProgramFiles(x86)}\Python35\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"

    ```

> [!NOTE]  
> Aşağıda belirtildiği gibi Python SDK ile geliştirme için virtualenv kullanıyorsanız, yukarıdaki sertifikayı sanal ortamınızın sertifika deposuna eklemeniz gerekir. Yol şuna benzer görünebilir: ".. \Mytestenv\lib\site-packages\certifi\cacert.exe "



## <a name="python-samples-for-azure-stack"></a>Azure Stack için Python örnekleri

Python SDK kullanılarak Azure Stack için kullanılabilir kod örneklerinden bazıları şunlardır:

- [Kaynakları ve kaynak gruplarını yönetme](https://azure.microsoft.com/resources/samples/hybrid-resourcemanager-python-manage-resources/)
- [Depolama hesabını Yönet](https://azure.microsoft.com/resources/samples/hybrid-storage-python-manage-storage-account/)
- [Sanal makineleri yönetme](https://azure.microsoft.com/resources/samples/hybrid-compute-python-manage-vm/) (Bu örnek, Azure Stack tarafından desteklenen en son API sürümlerini hedefleyen 2019-03-01-karma kullanır)

## <a name="python-manage-virtual-machine-sample"></a>Python sanal makine örneğini Yönet

Azure Stack sanal makineler (VM) için ortak yönetim görevlerini gerçekleştirmek üzere aşağıdaki kod örneğini kullanın. Kod örneği şunları gösterir:

- VM oluşturma:
  - Linux VM'i oluşturma
  - Bir Windows Sanal Makinesi oluşturun
- VM 'yi güncelleştirme:
  - Sürücüyü Genişlet
  - Bir VM’yi etiketleme
  - Veri disklerini iliştirme
  - Veri diskini çıkarma
- VM çalıştırma:
  - VM başlatma
  - VM durdurma
  - Bir VM’yi yeniden başlatma
- VM'leri listele
- VM silme

Bu işlemleri gerçekleştiren kodu gözden geçirmek için GitHub deposu **run_example ()** işlevine GitHub deposu [karma-COMPUTE-Python-Manage-VM](https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM)' de bakın.

Her işlem, bir açıklama ve bir yazdırma işleviyle açıkça etiketlenir. Örneklerin bu listede gösterilen sırada olması gerekmez.

## <a name="run-the-python-sample"></a>Python örneğini çalıştırma

1. Henüz yüklenmemişse [Python 'ı yükleme](https://www.python.org/downloads/) . Bu örnek (ve SDK), Python 2,7, 3,4, 3,5 ve 3,6 ile uyumludur.

2. Python geliştirmenin genel önerisi sanal bir ortam kullanmaktır. Daha fazla bilgi için bkz. [Python belgeleri](https://docs.python.org/3/tutorial/venv.html).

3. Python 3 ' te "venv" modülü ile sanal ortamı yükleyip başlatın (Python 2,7 için [virtualalenv](https://pypi.python.org/pypi/virtualenv) ' yi yüklemelisiniz):

    ```bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ```

4. Depoyu kopyalayın:

    ```bash
    git clone https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM.git
    ```

5. PIP kullanarak bağımlılıkları yükler:

    ```bash
    cd Hybrid-Compute-Python-Manage-VM
    pip install -r requirements.txt
    ```

6. Azure Stack çalışmak için bir [hizmet sorumlusu](../operator/azure-stack-create-service-principals.md) oluşturun. Hizmet sorumlunun aboneliğinizde katkıda bulunan [/Owner rolüne](../operator/azure-stack-create-service-principals.md#assign-a-role) sahip olduğundan emin olun.

7. Aşağıdaki değişkenleri ayarlayın ve bu ortam değişkenlerini geçerli kabuğınızdan dışarı aktarın:

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    export AZURE_RESOURCE_LOCATION={your AzureStack Resource location}
    ```

8. Bu örneği çalıştırmak için, Ubuntu 16,04-LTS ve WindowsServer 2012-R2-DataCenter görüntülerinin Azure Stack marketi 'nde mevcut olması gerekir. Bu görüntüler [Azure 'dan indirilebilir](../operator/azure-stack-download-azure-marketplace-item.md)ya da [Platform görüntü deposuna](../operator/azure-stack-add-vm-image.md)eklenebilir.

9. Örneği çalıştırın:

    ```python
    python example.py
    ```


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Python geliştirme merkezi](https://azure.microsoft.com/develop/python/)
- [Azure sanal makineler belgeleri](https://azure.microsoft.com/services/virtual-machines/)
- [Sanal makineler için öğrenme yolu](/learn/paths/deploy-a-website-with-azure-virtual-machines/)

Microsoft Azure aboneliğiniz yoksa [buradan](https://go.microsoft.com/fwlink/?LinkId=330212)ücretsiz bir deneme hesabı edinebilirsiniz.
