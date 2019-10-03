---
title: Azure CLı ile Azure Stack yönetme | Microsoft Docs
description: Azure Stack, kaynakları yönetmek ve dağıtmak için platformlar arası komut satırı arabirimini (CLı) nasıl kullanacağınızı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: a0218652e2dace72356a32fe99ac5f6ac450cc94
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71824785"
---
# <a name="manage-and-deploy-resources-to-azure-stack-with-azure-cli"></a>Azure CLı ile Azure Stack kaynakları yönetme ve dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Linux, Mac ve Windows istemci platformlarındaki Azure Stack Geliştirme Seti (ASDK) kaynaklarını yönetmek üzere Azure komut satırı arabirimi 'ni (CLı) ayarlamak için bu makaledeki adımları izleyin.

## <a name="prepare-for-azure-cli"></a>Azure CLı için hazırlanma

ASDK kullanıyorsanız, geliştirme makinenizde Azure CLı kullanmak için Azure Stack CA kök sertifikasına ihtiyacınız vardır. Bu sertifikayı CLı aracılığıyla kaynakları yönetmek için kullanırsınız.

 - CLı 'yi ASDK dışında bir iş istasyonundan kullanıyorsanız, **Azure Stack CA kök sertifikası** gerekir.  

 - **Sanal makine diğer adları uç noktası** , "UbuntuLTS" veya "Win2012Datacenter" gibi bir diğer ad sağlar. Bu diğer ad, VM 'Leri dağıtımında tek bir parametre olarak bir görüntü yayımcısı, teklif, SKU ve sürüme başvurur.  

Aşağıdaki bölümlerde bu değerlerin nasıl alınacağı açıklanır.

### <a name="export-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını dışarı aktarma

Tümleşik bir sistem kullanıyorsanız, CA kök sertifikasını dışarı aktarmanız gerekmez. ASDK kullanıyorsanız, CA kök sertifikasını bir ASDK 'de dışarı aktarın.

ASDK kök sertifikasını pek biçiminde dışarı aktarmak için:

1. Azure Stack kök sertifikanın adını alın:
    - Azure Stack Kullanıcı veya yönetici portalında oturum açın.
    - Adres çubuğunun yakınında **güvenli** ' e tıklayın.
    - Açılır pencerede **geçerli**' e tıklayın.
    - Sertifika penceresinde, **sertifika yolu** sekmesi ' ne tıklayın.
    - Azure Stack kök CERT adının bir kısmını aklınızda edin.

    ![Azure Stack kök sertifika](media/azure-stack-version-profiles-azurecli2/root-cert-name.png)

2. [Azure Stack üzerinde bir Windows sanal makinesi oluşturun](azure-stack-quick-windows-portal.md).

3. VM 'de oturum açın, yükseltilmiş bir PowerShell istemi açın ve ardından aşağıdaki betiği çalıştırın:

    ```powershell  
      $label = "<the name of your azure stack root cert from Step 1>"
      Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
      $root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
      if (-not $root)
      {
          Write-Error "Certificate with subject CN=$label not found"
          return
      }

    Write-Host "Exporting certificate"
    Export-Certificate -Type CERT -FilePath root.cer -Cert $root

    Write-Host "Converting certificate to PEM format"
    certutil -encode root.cer root.pem
    ```

4. Sertifikayı yerel makinenize kopyalayın.


### <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer adları uç noktasını ayarlama

VM diğer adı dosyasını barındıran, genel olarak erişilebilen bir uç nokta ayarlayabilirsiniz. VM diğer adı dosyası, bir görüntü için ortak ad sağlayan bir JSON dosyasıdır. Bir sanal makineyi Azure CLı parametresi olarak dağıtırken bu adı kullanırsınız.

1. Özel bir görüntü yayımlarsanız, yayımlama sırasında belirttiğiniz Yayımcı, teklif, SKU ve sürüm bilgilerini unutmayın. Market 'ten bir görüntüdür, bu bilgileri ```Get-AzureVMImage``` cmdlet 'ini kullanarak görüntüleyebilirsiniz.  

2. [Örnek dosyayı](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) GitHub 'dan indirin.

3. Azure Stack ' de bir depolama hesabı oluşturun. Bu tamamlandığında bir blob kapsayıcısı oluşturun. Erişim ilkesini "genel" olarak ayarlayın.  

4. JSON dosyasını yeni kapsayıcıya yükleyin. Bu tamamlandığında, Blobun URL 'sini görüntüleyebilirsiniz. Blob adı ' nı ve ardından blob özelliklerinden URL 'YI seçin.

### <a name="install-or-upgrade-cli"></a>CLı 'yı yükler veya yükseltin

Geliştirme iş istasyonunuzda oturum açın ve CLı 'yı yükledikten sonra. Azure Stack, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. API profillerinin en son sürümü, CLı 'nin güncel bir sürümünü gerektirir. CLı 'yi, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 'yı kullanma makalesinde açıklanan adımları kullanarak yüklersiniz. 

1. Yüklemenin başarılı olup olmadığını doğrulamak için bir Terminal veya komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

    ```shell
    az --version
    ```

    Bilgisayarınızda yüklü olan Azure CLı ve diğer bağımlı kitaplıkların sürümünü görmeniz gerekir.

    ![Azure Stack Python konumunda Azure CLı](media/azure-stack-version-profiles-azurecli2/cli-python-location.png)

2. CLı 'nın Python konumunu bir yere unutmayın. ASDK çalıştırıyorsanız, sertifikanızı eklemek için bu konumu kullanmanız gerekir.


## <a name="windows-azure-ad"></a>Windows (Azure AD)

Bu bölüm, Azure AD 'yi kimlik yönetimi hizmetiniz olarak kullanıyorsanız ve CLı 'yi bir Windows makinesinde kullanıyorsanız, CLı ayarlama işleminde size kılavuzluk eder.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

ASDK kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Bu adım tümleşik sistemler için gerekli değildir.

Azure Stack CA kök sertifikasına güvenmek için, Azure CLı ile yüklenen Python sürümü için mevcut Python sertifika deposuna ekleyin. Kendi Python örneğinizi çalıştırıyor olabilirsiniz. Azure CLı, Python 'un kendi sürümünü içerir.

1. Makinenizde sertifika depolama konumunu bulun.  Komutunu `az --version`çalıştırarak konumu bulabilirsiniz.

2. CLı Python uygulamanızı içeren klasöre gidin. Python 'un bu sürümünü çalıştırmak istiyorsunuz. Sistem YOLUNUZDA Python ayarladıysanız Python çalıştırmak kendi Python sürümünüzü yürütür. Bunun yerine, CLı tarafından kullanılan sürümü çalıştırmak ve sertifikanızı bu sürüme eklemek istersiniz. Örneğin, CLı Python 'niz şu konumda olabilir: `C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\`.

    Aşağıdaki komutları kullanın:

    ```powershell  
    cd "c:\pathtoyourcliversionofpython"
    .\python -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu bir yere getirin. Örneğin, `C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\certifi\cacert.pem`. Belirli yolunuz, işletim sistemi ve CLı yüklemenize bağlıdır.

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
    Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. `az cloud register` Komutunu çalıştırarak Azure Stack ortamınızı kaydettirin.

    Bazı senaryolarda, doğrudan giden internet bağlantısı, SSL yakalaşmayı zorlayan bir ara sunucu veya güvenlik duvarı aracılığıyla yönlendirilir. Bu durumlarda, `az cloud register` komut "buluttan uç noktalar alınamıyor" gibi bir hata vererek başarısız olabilir. Bu hatayı geçici olarak çözmek için aşağıdaki ortam değişkenlerini ayarlayın:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Ortamınızı kaydedin. Çalıştırılırken `az cloud register`aşağıdaki parametreleri kullanın:

    | Value | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanıcı `AzureStackUser` ortamı için kullanın. Operatörünüz varsa, öğesini belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | ASDK 'deki **Resourcemanagerurl** : `https://management.local.azurestack.external/`Tümleşik sistemlerdeki **Resourcemanagerurl 'si** : `https://management.<region>.<fqdn>/` tümleşik sistem uç noktasıyla ilgili bir sorunuz varsa, bulut işleçle iletişim kurun. |
    | Depolama uç noktası | Local. azurestack. External | `local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | Keykasası son eki | .vault.local.azurestack.external | `.vault.local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | VM görüntüsü diğer adı belge uç noktası- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | VM görüntüsü diğer adlarını içeren belge URI 'SI. Daha fazla bilgi için bkz. [VM diğer adları uç noktasını ayarlama](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains VM image aliases>
    ```

1. Aşağıdaki komutları kullanarak etkin ortamı ayarlayın.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Ortam yapılandırmanızı Azure Stack belirli API sürümü profilini kullanacak şekilde güncelleştirin. Yapılandırmayı güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az cloud update --profile 2019-03-01-hybrid
   ```

    >[!NOTE]  
    >1808 derlemeden önce bir Azure Stack sürümünü çalıştırıyorsanız, API sürüm profili **2019-03-01-karma**yerine API sürüm profili **2017-03-09-profile** ' yı kullanmanız gerekir. Ayrıca, Azure CLı 'nin yeni bir sürümünü kullanmanız gerekir.
 
1. `az login` Komutunu kullanarak Azure Stack ortamınızda oturum açın. Azure Stack ortamında Kullanıcı veya [hizmet sorumlusu](/azure/active-directory/develop/app-objects-and-service-principals)olarak oturum açın. 

   - *Kullanıcı*olarak oturum açın: 

     Kullanıcı adını ve parolayı doğrudan `az login` komut içinden belirtebilir veya bir tarayıcı kullanarak kimlik doğrulaması yapabilirsiniz. Hesabınız Multi-Factor Authentication etkinleştirildiyse, ikincisini yapmanız gerekir:

     ```azurecli
     az login -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Kullanıcı hesabınızda Multi-Factor Authentication etkinleştirilmişse, `az login` `-u` parametresini belirtmeden komutunu kullanın. Bu komutun çalıştırılması, kimlik doğrulaması için kullanmanız gereken bir URL ve kod sağlar.

   - *Hizmet sorumlusu*olarak oturum açın: 
    
     Oturum açmadan önce, Azure portal veya CLı [aracılığıyla bir hizmet sorumlusu oluşturun](azure-stack-create-service-principals.md) ve bir rol atayın. Şimdi aşağıdaki komutu kullanarak oturum açın:

     ```azurecli  
     az login --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> --service-principal -u <Application Id of the Service Principal> -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şey ayarlandığında, Azure Stack içinde kaynak oluşturmak için CLı kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve VM ekleyebilirsiniz. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komut yeni oluşturulan kaynağın aşağıdaki özelliklerini verir:

![Kaynak grubu çıkış oluştur](media/azure-stack-connect-cli/image1.png)

## <a name="windows-ad-fs"></a>Windows (AD FS)

Bu bölüm, kimlik yönetimi hizmetiniz olarak Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız ve CLı 'yi bir Windows makinesinde kullanıyorsanız, CLı ayarlama işleminde size yol gösterir.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

ASDK kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Bu adım tümleşik sistemler için gerekli değildir.

1. Makinenizde sertifika konumunu bulun. Konum, Python 'u yüklediğiniz yere göre farklılık gösterebilir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```powershell  
      python -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu bir yere getirin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuz, işletim sistemine ve yüklediğiniz Python sürümüne bağlıdır.

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
    Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. `az cloud register` Komutunu çalıştırarak Azure Stack ortamınızı kaydettirin.

    Bazı senaryolarda, doğrudan giden internet bağlantısı, SSL yakalaşmayı zorlayan bir ara sunucu veya güvenlik duvarı aracılığıyla yönlendirilir. Bu durumlarda, `az cloud register` komut "buluttan uç noktalar alınamıyor" gibi bir hata vererek başarısız olabilir. Bu hatayı geçici olarak çözmek için aşağıdaki ortam değişkenlerini ayarlayın:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Ortamınızı kaydedin. Çalıştırılırken `az cloud register`aşağıdaki parametreleri kullanın:

    | Value | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanıcı `AzureStackUser` ortamı için kullanın. Operatörünüz varsa, öğesini belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | ASDK 'deki **Resourcemanagerurl** : `https://management.local.azurestack.external/`Tümleşik sistemlerdeki **Resourcemanagerurl 'si** : `https://management.<region>.<fqdn>/` tümleşik sistem uç noktasıyla ilgili bir sorunuz varsa, bulut işleçle iletişim kurun. |
    | Depolama uç noktası | Local. azurestack. External | `local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | Keykasası son eki | .vault.local.azurestack.external | `.vault.local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | VM görüntüsü diğer adı belge uç noktası- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | VM görüntüsü diğer adlarını içeren belge URI 'SI. Daha fazla bilgi için bkz. [VM diğer adları uç noktasını ayarlama](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains VM image aliases>
    ```

1. Aşağıdaki komutları kullanarak etkin ortamı ayarlayın.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Ortam yapılandırmanızı Azure Stack belirli API sürümü profilini kullanacak şekilde güncelleştirin. Yapılandırmayı güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az cloud update --profile 2019-03-01-hybrid
   ```

    >[!NOTE]  
    >1808 derlemeden önce bir Azure Stack sürümünü çalıştırıyorsanız, API sürüm profili **2019-03-01-karma**yerine API sürüm profili **2017-03-09-profile** ' yı kullanmanız gerekir. Ayrıca, Azure CLı 'nin yeni bir sürümünü kullanmanız gerekir.

1. `az login` Komutunu kullanarak Azure Stack ortamınızda oturum açın. Azure Stack ortamında Kullanıcı veya [hizmet sorumlusu](/azure/active-directory/develop/app-objects-and-service-principals)olarak oturum açabilirsiniz. 

   - *Kullanıcı*olarak oturum açın:

     Kullanıcı adını ve parolayı doğrudan `az login` komut içinden belirtebilir veya bir tarayıcı kullanarak kimlik doğrulaması yapabilirsiniz. Hesabınız Multi-Factor Authentication etkinleştirildiyse, ikincisini yapmanız gerekir:

     ```azurecli
     az cloud register  -n <environmentname>   --endpoint-resource-manager "https://management.local.azurestack.external"  --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains VM image aliases>   --profile "2019-03-01-hybrid"
     ```

     > [!NOTE]
     > Kullanıcı hesabınızda Multi-Factor Authentication etkinleştirilmişse, `az login` `-u` parametresini belirtmeden komutunu kullanın. Bu komutun çalıştırılması, kimlik doğrulaması için kullanmanız gereken bir URL ve kod sağlar.

   - *Hizmet sorumlusu*olarak oturum açın: 
    
     Hizmet sorumlusu oturum açması için kullanılacak. ped dosyasını hazırlayın.

     Sorumlunun oluşturulduğu istemci makinede, hizmet sorumlusu sertifikasını konumunda `cert:\CurrentUser\My`bulunan özel anahtarla PFX olarak dışarı aktarın. Sertifika adının sorumlu ile aynı adı vardır.

     Pfx 'i ped 'ye dönüştürün (OpenSSL yardımcı programını kullanın).

     CLı 'da oturum açın:
  
     ```azurecli  
     az login --service-principal \
      -u <Client ID from the Service Principal details> \
      -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
      --tenant <Tenant ID> \
      --debug 
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şey ayarlandığında, Azure Stack içinde kaynak oluşturmak için CLı kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve VM ekleyebilirsiniz. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komut yeni oluşturulan kaynağın aşağıdaki özelliklerini verir:

![Kaynak grubu çıkış oluştur](media/azure-stack-connect-cli/image1.png)


## <a name="linux-azure-ad"></a>Linux (Azure AD)

Bu bölümde, Azure AD 'yi kimlik yönetimi hizmetiniz olarak kullanıyorsanız ve bir Linux makinesinde CLı kullanıyorsanız, CLı 'yi ayarlama işlemi adım adım açıklanmaktadır.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

ASDK kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Bu adım tümleşik sistemler için gerekli değildir.

Azure Stack CA kök sertifikasına, var olan Python sertifikasına ekleyerek güvenin.

1. Makinenizde sertifika konumunu bulun. Konum, Python 'u yüklediğiniz yere göre farklılık gösterebilir. PIP ve certifi modülünün yüklü olması gerekir. Bash isteminde aşağıdaki Python komutunu kullanın:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu bir yere getirin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuz, işletim sisteminize ve yüklediğiniz Python sürümüne bağlıdır.

2. Aşağıdaki Bash komutunu sertifikanızın yoluyla çalıştırın.

   - Bir uzak Linux makinesi için:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Azure Stack ortamı içindeki bir Linux makinesi için:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack bağlanmak için aşağıdaki adımları kullanın:

1. `az cloud register` Komutunu çalıştırarak Azure Stack ortamınızı kaydettirin. Bazı senaryolarda, doğrudan giden internet bağlantısı, SSL yakalaşmayı zorlayan bir ara sunucu veya güvenlik duvarı aracılığıyla yönlendirilir. Bu durumlarda, `az cloud register` komut "buluttan uç noktalar alınamıyor" gibi bir hata vererek başarısız olabilir. Bu hatayı geçici olarak çözmek için aşağıdaki ortam değişkenlerini ayarlayın:

   ```shell
   export AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1
   export ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Ortamınızı kaydedin. Çalıştırılırken `az cloud register`aşağıdaki parametreleri kullanın:

    | Value | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanıcı `AzureStackUser` ortamı için kullanın. Operatörünüz varsa, öğesini belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | ASDK 'deki **Resourcemanagerurl** : `https://management.local.azurestack.external/`Tümleşik sistemlerdeki **Resourcemanagerurl 'si** : `https://management.<region>.<fqdn>/` tümleşik sistem uç noktasıyla ilgili bir sorunuz varsa, bulut işleçle iletişim kurun. |
    | Depolama uç noktası | Local. azurestack. External | `local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | Keykasası son eki | .vault.local.azurestack.external | `.vault.local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | VM görüntüsü diğer adı belge uç noktası- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | VM görüntüsü diğer adlarını içeren belge URI 'SI. Daha fazla bilgi için bkz. [VM diğer adları uç noktasını ayarlama](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains VM image aliases>
    ```

3. Etkin ortamı ayarlayın. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Ortam yapılandırmanızı Azure Stack belirli API sürümü profilini kullanacak şekilde güncelleştirin. Yapılandırmayı güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
      az cloud update --profile 2019-03-01-hybrid
   ```

    >[!NOTE]  
    >1808 derlemeden önce bir Azure Stack sürümünü çalıştırıyorsanız, API sürüm profili **2019-03-01-karma**yerine API sürüm profili **2017-03-09-profile** ' yı kullanmanız gerekir. Ayrıca, Azure CLı 'nin yeni bir sürümünü kullanmanız gerekir.

5. `az login` Komutunu kullanarak Azure Stack ortamınızda oturum açın. Azure Stack ortamında Kullanıcı veya [hizmet sorumlusu](/azure/active-directory/develop/app-objects-and-service-principals)olarak oturum açabilirsiniz. 

   * *Kullanıcı*olarak oturum açın:

     Kullanıcı adını ve parolayı doğrudan `az login` komut içinden belirtebilir veya bir tarayıcı kullanarak kimlik doğrulaması yapabilirsiniz. Hesabınız Multi-Factor Authentication etkinleştirildiyse, ikincisini yapmanız gerekir:

     ```azurecli
     az login \
       -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Kullanıcı hesabınızda Multi-Factor Authentication etkinleştirildiyse, `az login` `-u` parametresini sağlamadan komutunu kullanabilirsiniz. Bu komutun çalıştırılması, kimlik doğrulaması için kullanmanız gereken bir URL ve kod sağlar.
   
   * *Hizmet sorumlusu* olarak oturum açın
    
     Oturum açmadan önce, Azure portal veya CLı [aracılığıyla bir hizmet sorumlusu oluşturun](azure-stack-create-service-principals.md) ve bir rol atayın. Şimdi aşağıdaki komutu kullanarak oturum açın:

     ```azurecli  
     az login \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
       --service-principal \
       -u <Application Id of the Service Principal> \
       -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şey ayarlandığında, Azure Stack içinde kaynak oluşturmak için CLı kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve VM ekleyebilirsiniz. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
    az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komut yeni oluşturulan kaynağın aşağıdaki özelliklerini verir:

![Kaynak grubu çıkış oluştur](media/azure-stack-connect-cli/image1.png)

## <a name="linux-ad-fs"></a>Linux (AD FS)

Bu bölümde, yönetim hizmetiniz olarak Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız ve bir Linux makinesinde CLı kullanıyorsanız, CLı 'yi ayarlama işlemi adım adım açıklanmaktadır.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasına güven

ASDK kullanıyorsanız, uzak makinenizde CA kök sertifikasına güvenmeniz gerekir. Bu adım tümleşik sistemler için gerekli değildir.

Azure Stack CA kök sertifikasına, var olan Python sertifikasına ekleyerek güvenin.

1. Makinenizde sertifika konumunu bulun. Konum, Python 'u yüklediğiniz yere göre farklılık gösterebilir. PIP ve certifi modülünün yüklü olması gerekir. Bash isteminde aşağıdaki Python komutunu kullanın:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu bir yere getirin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuz, işletim sisteminize ve yüklediğiniz Python sürümüne bağlıdır.

2. Aşağıdaki Bash komutunu sertifikanızın yoluyla çalıştırın.

   - Bir uzak Linux makinesi için:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Azure Stack ortamı içindeki bir Linux makinesi için:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack bağlanmak için aşağıdaki adımları kullanın:

1. `az cloud register` Komutunu çalıştırarak Azure Stack ortamınızı kaydettirin. Bazı senaryolarda, doğrudan giden internet bağlantısı, SSL yakalaşmayı zorlayan bir ara sunucu veya güvenlik duvarı aracılığıyla yönlendirilir. Bu durumlarda, `az cloud register` komut "buluttan uç noktalar alınamıyor" gibi bir hata vererek başarısız olabilir. Bu hatayı geçici olarak çözmek için aşağıdaki ortam değişkenlerini ayarlayın:

   ```shell
   export AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1
   export ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Ortamınızı kaydedin. Çalıştırırken `az cloud register`aşağıdaki parametreleri kullanın.

    | Value | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanıcı `AzureStackUser` ortamı için kullanın. Operatörünüz varsa, öğesini belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | ASDK 'deki **Resourcemanagerurl** : `https://management.local.azurestack.external/`Tümleşik sistemlerdeki **Resourcemanagerurl 'si** : `https://management.<region>.<fqdn>/` tümleşik sistem uç noktasıyla ilgili bir sorunuz varsa, bulut işleçle iletişim kurun. |
    | Depolama uç noktası | Local. azurestack. External | `local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | Keykasası son eki | .vault.local.azurestack.external | `.vault.local.azurestack.external`, ASDK içindir. Tümleşik bir sistem için, sisteminiz için bir uç nokta kullanın.  |
    | VM görüntüsü diğer adı belge uç noktası- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | VM görüntüsü diğer adlarını içeren belge URI 'SI. Daha fazla bilgi için bkz. [VM diğer adları uç noktasını ayarlama](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains VM image aliases>
    ```

3. Etkin ortamı ayarlayın. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Ortam yapılandırmanızı Azure Stack belirli API sürümü profilini kullanacak şekilde güncelleştirin. Yapılandırmayı güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
      az cloud update --profile 2019-03-01-hybrid
   ```

    >[!NOTE]  
    >1808 derlemeden önce bir Azure Stack sürümünü çalıştırıyorsanız, API sürüm profili **2019-03-01-karma**yerine API sürüm profili **2017-03-09-profile** ' yı kullanmanız gerekir. Ayrıca, Azure CLı 'nin yeni bir sürümünü kullanmanız gerekir.

5. `az login` Komutunu kullanarak Azure Stack ortamınızda oturum açın. Azure Stack ortamında Kullanıcı veya [hizmet sorumlusu](/azure/active-directory/develop/app-objects-and-service-principals)olarak oturum açabilirsiniz. 

6. Oturum Aç: 

   *  Bir cihaz koduyla bir Web tarayıcısı kullanan **Kullanıcı** olarak:  

   ```azurecli  
    az login --use-device-code
   ```

   > [!NOTE]  
   >Komutun çalıştırılması, kimlik doğrulaması için kullanmanız gereken bir URL ve kod sağlar.

   * Hizmet sorumlusu olarak:
        
     Hizmet sorumlusu oturum açması için kullanılacak. ped dosyasını hazırlayın.

      * Sorumlunun oluşturulduğu istemci makinede, hizmet sorumlusu sertifikasını konumunda `cert:\CurrentUser\My`bulunan özel anahtarla PFX olarak dışarı aktarın. Sertifika adının sorumlu ile aynı adı vardır.
  
      * Pfx 'i ped 'ye dönüştürün (OpenSSL yardımcı programını kullanın).

     CLı 'da oturum açın:

      ```azurecli  
      az login --service-principal \
        -u <Client ID from the Service Principal details> \
        -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
        --tenant <Tenant ID> \
        --debug 
      ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şey ayarlandığında, Azure Stack içinde kaynak oluşturmak için CLı kullanın. Örneğin, bir uygulama için bir kaynak grubu oluşturabilir ve VM ekleyebilirsiniz. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
  az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komut yeni oluşturulan kaynağın aşağıdaki özelliklerini verir:

![Kaynak grubu çıkış oluştur](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bilinen sorunlar

Azure Stack 'de CLı kullanırken bilinen sorunlar vardır:

 - CLı etkileşimli modu. Örneğin `az interactive` , komutu henüz Azure Stack desteklenmez.
 - Azure Stack ' de kullanılabilen VM görüntülerinin listesini almak için komutu yerine `az vm image list --all` `az vm image list` komutunu kullanın. `--all` Seçeneğinin belirtilmesi, yanıtın yalnızca Azure Stack ortamınızda bulunan görüntüleri döndürdüğünden emin olmanızı sağlar.
 - Azure 'da kullanılabilen VM görüntüsü diğer adları Azure Stack için geçerli olmayabilir. VM görüntülerini kullanırken, görüntü diğer adı yerine tüm URN parametresini (kurallı: UbuntuServer: 14.04.3-LTS: 1.0.0) kullanmanız gerekir. Bu urn, `az vm images list` komuttan türetilen görüntü belirtimleriyle eşleşmelidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLı ile şablonları dağıtma](azure-stack-deploy-template-command-line.md)
- [Azure Stack kullanıcıları için Azure CLı 'yi etkinleştirme (operatör)](../operator/azure-stack-cli-admin.md)
- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md) 
