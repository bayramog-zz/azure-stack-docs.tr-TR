---
title: Azure Stack depolama için Araçlar | Microsoft Docs
description: Aktarım araçları Azure Stack depolama verileri hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 27e70df453678bf2f6d3a9427a5a692b3cc62d8d
ms.sourcegitcommit: d1fdecdfa843dfc0629bfc226f1baf14f3ea621d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67387780"
---
# <a name="use-data-transfer-tools-for-azure-stack-storage"></a>Azure Stack depolama için veri aktarım araçları kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Microsoft Azure Stack diskler, bloblar, tablolar, kuyruklar ve hesap yönetimi işlevleri için depolama hizmetleri sunmaktadır. Yönetmek veya için veya Azure Stack depolama alanından verileri taşımak istiyorsanız, bir dizi Azure storage araçları kullanabilirsiniz. Bu makalede, kullanılabilen araçlara genel bakış sağlar.

Aşağıdaki araçlar hangisinin sizin için en uygun gereksinimlerinizi belirleyin:

* [AzCopy](#azcopy)

    Bir nesneyi başka bir nesne depolama hesabınızda veya depolama hesapları arasında veri kopyalamak için indirebileceğiniz bir depolama özel, komut satırı yardımcı.

* [Azure PowerShell](#azure-powershell)

    Bir görev tabanlı, komut satırı kabuğu ve betik dilidir özellikle sistem yönetimi için tasarlanmıştır.

* [Azure CLI](#azure-cli)

    Azure'da ve Azure Stack'te platformları ile çalışmak için bir komut kümesi sağlar. açık kaynaklı, platformlar arası aracı.

* [Microsoft Depolama Gezgini](#microsoft-azure-storage-explorer)

    Kullanımı kolay tek başına uygulama kullanıcı arabirimi ile.

* [Blobfuse](#blobfuse)

    Sanal dosya sistemi sürücüsü için mevcut blok blobu verileriniz depolama hesabınızda Linux dosya sistemi üzerinden erişmenize olanak sağlayan Azure Blob Depolama. 

Azure ve Azure Stack arasında depolama hizmetleri farklar nedeniyle aşağıdaki bölümlerde açıklanan her aracı için bazı belirli gereksinimleri olabilir. Azure Stack depolama ve Azure depolama arasında bir karşılaştırma için bkz. [Azure Stack Depolama: Farklılıklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md).

## <a name="azcopy"></a>AzCopy

AzCopy, en iyi performansı sunan basit komutlar kullanılarak Microsoft Azure blob ve tablo depolama ve veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. Verileri bir nesneden diğerine depolama hesabınızda veya depolama hesapları arasında kopyalayabilirsiniz.

### <a name="download-and-install-azcopy"></a>AzCopy yükleyip

* 1811 güncelleştirme veya daha yeni sürümlerini [AzCopy indirme](/azure/storage/common/storage-use-azcopy-v10#download-azcopy).
* Önceki sürümleri (1802 için 1809 güncelleştirme) [AzCopy 7.1.0 indirme](https://aka.ms/azcopyforazurestack20170417).

### <a name="accopy-101-configuration-and-limits"></a>AcCopy 10.1 yapılandırmayı ve sınırlarını

AzCopy 10.1 önceki API sürümlerini kullanacak şekilde yapılandırılması için sunulmuştur. Bu, Azure Stack (sınırlı) desteği sağlar.
Azure Stack desteklemek AzCopy için API sürümü yapılandırmak için `AZCOPY_DEFAULT_SERVICE_API_VERSION` ortam değişkenine `2017-11-09`.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | İçinde bir komut istemi kullanın: `set AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`<br> PowerShell kullanımda: `$env:AZCOPY_DEFAULT_SERVICE_API_VERSION="2017-11-09"`|
| **Linux** | `export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09` |
| **MacOS** | `export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09` |

AzCopy 10.1 Azure Stack için aşağıdaki özellikleri destekler:

| Özellik | Desteklenen eylemler |
| --- | --- |
|Kapsayıcı yönetme|Bir kapsayıcı oluşturma<br>Kapsayıcıları listeleme
|İşi Yönet|İşleri görüntüle<br>Bir işi sürdürme
|BLOB Kaldır|Tek bir blob Kaldır<br>Tüm veya kısmi sanal dizini Kaldır
|Dosya yükleme|Dosyayı karşıya yükleme<br>Bir dizin karşıya yükleme<br>Bir dizinin içeriklerini karşıya yükleme
|Dosyayı indirin|Dosya indirme<br>Bir dizine indirin<br>Bir dizinin içeriğini indirin
|Dosya eşitleme|Yerel dosya sisteminde bir kapsayıcıya Eşitle<br>Bir kapsayıcıya yerel dosya sistemi Eşitle

   > [!NOTE]
   > * Azure Stack, Azure Active Directory (AD) kullanarak, AzCopy için sağlayan yetkilendirme kimlik bilgilerini desteklemez. Depolama nesneleri bir paylaşılan erişim imzası (SAS) belirteci kullanarak Azure Stack'te erişmeniz gerekir.
   > * Azure Stack, iki Azure Stack blob konumlar arasında ve Azure depolama ve Azure Stack arasında zaman uyumsuz veri aktarımı desteklemiyor. "Azcopy cp" kullanamazsınız veri Azure yığını Azure depolama (veya bulmadýðýný şekilde) doğrudan AzCopy 10.1 ile taşıyabilirsiniz.

### <a name="azcopy-command-examples-for-data-transfer"></a>Veri aktarımı için AzCopy komut örnekleri

Aşağıdaki örnekler ve Azure Stack bloblarından veri kopyalamak için tipik senaryoları izleyin. Daha fazla bilgi için bkz. [Azcopy'i kullanmaya başlama](/azure/storage/common/storage-use-azcopy-v10).

### <a name="download-all-blobs-to-a-local-disk"></a>Tüm blobları yerel diskinize indirin

```
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true
```

### <a name="upload-single-file-to-virtual-directory"></a>Tek Dosyalı sanal dizine yükleyin

```
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

### <a name="azcopy-known-issues"></a>Azcopy bilinen sorunlar

 - Dosya depolama henüz Azure Stack'te kullanılabilir olmadığından herhangi bir dosya deposu AzCopy işlemi kullanılabilir değil.
 - AzCopy 10.1 kullanarak veya Azure Stack ve Azure storage iki Azure Stack blob konumlar arasında veri aktarmak istiyorsanız, verileri yerel konuma indirmeniz ve hedef dizin Azure Stack veya Azure depolama için yeniden yüklemeniz gerekir. AzCopy 7.1 kullanın ve aktarımı belirtin **/SyncCopy** veri kopyalamak için seçenek.  
 - Azcopy Linux sürümünü yalnızca 1802 güncelleştirmesi veya sonraki sürümleri destekler. Ve tablo hizmetini desteklemiyor.
 
## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell hem Azure hem de Azure Stack hizmetlerini yönetmek için cmdlet'ler sağlayan bir modüldür. Bu, bir görev tabanlı, komut satırı kabuğu ve betik dilidir özellikle sistem yönetimi için tasarlanmış olan.

### <a name="install-and-configure-powershell-for-azure-stack"></a>PowerShell'i yükleme ve Azure Stack için yapılandırma

Azure Stack uyumlu Azure PowerShell modülleri, Azure Stack ile çalışmak için gereklidir. Daha fazla bilgi için [Azure Stack için PowerShell yükleme](../operator/azure-stack-powershell-install.md) ve [Azure Stack kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md) daha fazla bilgi için.

### <a name="powershell-sample-script-for-azure-stack"></a>Azure Stack için PowerShell örnek betiği 

Bu örnek, başarıyla sahip olduğunuzu varsayar [Azure Stack için PowerShell yüklü](../operator/azure-stack-powershell-install.md). Bu betik, yapılandırmayı tamamlamak ve yerel bir PowerShell ortamı için hesabınızı eklemek için kimlik bilgileri, Azure Stack kiracısı isteyin yardımcı olur. Ardından, betiği varsayılan Azure aboneliği, Azure'da yeni bir depolama hesabı oluşturma, bu yeni bir depolama hesabında yeni bir kapsayıcı oluşturur ve var olan bir görüntü dosyası (blob), kapsayıcıya yüklemek. Betik bu kapsayıcıdaki tüm blobları listeler sonra yerel bilgisayarınızda yeni bir hedef dizin oluşturma ve görüntü dosyasını indirin.

1. Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
2. İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).
3. Açık **Windows PowerShell ISE** ve **yönetici olarak çalıştır**, tıklayın **dosya** > **yeni** yeni betik dosyası oluşturma.
4. Aşağıdaki betiği kopyalayın ve yeni komut dosyasına yapıştırın.
5. Yapılandırma ayarlarınızı temel alan betik değişkenleri güncelleştirin.
   > [!NOTE]
   > Bu betik için kök dizininde çalıştırılması gereken **AzureStack_Tools**.

```powershell  
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environment name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudience = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase.
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\download" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudience

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Add-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder

# end
```

### <a name="powershell-known-issues"></a>PowerShell bilinen sorunlar

Azure Stack için geçerli uyumlu Azure PowerShell modülü sürüm için kullanıcı işlemlerini 1.2.11 ' dir. Azure PowerShell'in en son sürümünden farklıdır. Bu fark, depolama hizmetleri işlemi etkiler:

Dönüş değeri biçimi `Get-AzureRmStorageAccountKey` sürümünde 1.2.11 iki özelliğe sahiptir: `Key1` ve `Key2`, geçerli Azure sürümü tüm hesap anahtarları içeren bir dizi döndürür.

```powershell
# This command gets a specific key for a storage account, 
# and works for Azure PowerShell version 1.4, and later versions.
(Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
-AccountName "MyStorageAccount").Value[0]

# This command gets a specific key for a storage account, 
# and works for Azure PowerShell version 1.3.2, and previous versions.
(Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
-AccountName "MyStorageAccount").Key1
```

Daha fazla bilgi için [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey).

## <a name="azure-cli"></a>Azure CLI

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. MacOS, Linux ve Windows üzerinde yükleyin ve komut satırından çalıştırabilirsiniz.

Azure CLI, Azure kaynaklarını komut satırından yönetmek ve ve Azure Resource Manager'da çalışacak Otomasyon betikleri oluşturmak için optimize edilmiştir. Zengin verilere erişim de dahil olmak üzere Azure Stack portalında bulunan aynı işlevlerin birçoğunu sunar.

Azure Stack, Azure CLI 2.0 veya sonraki bir sürümünü gerektirir. Yükleme ve Azure CLI, Azure Stack ile yapılandırma hakkında daha fazla bilgi için bkz. [yüklemek ve Azure Stack CLI yapılandırma](azure-stack-version-profiles-azurecli2.md). Azure Stack depolama hesabınızdaki kaynaklarla çalışma çeşitli görevleri gerçekleştirmek için Azure CLI kullanma hakkında daha fazla bilgi için bkz. [Azure depolama ile Azure CLI kullanma](/azure/storage/storage-azure-cli)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure Stack için Azure CLI örnek betiği

CLI yükleme ve yapılandırmasını tamamladıktan sonra Azure Stack ile depolama kaynaklarını etkileşim kurmak için bir küçük Kabuk örnek betiği çalışmak için aşağıdaki adımları deneyebilirsiniz. Komut aşağıdaki eylemleri gerçekleştirir:

* Depolama hesabınızda yeni bir kapsayıcı oluşturur.
* (Blob) olarak mevcut bir dosyayı kapsayıcıya yükler.
* Tüm blobları kapsayıcıda listeler.
* Yerel bilgisayarınızda, belirttiğiniz bir hedefe dosyasını indirir.

Bu betiği çalıştırmadan önce başarıyla için bağlanabilir ve hedef Azure Stack için oturum açın, emin olun.

1. Sık kullandığınız metin düzenleyicisinde açın, sonra kopyalayın ve yukarıdaki betik düzenleyiciye yapıştırın.
2. Betiğin değişkenleri yapılandırma ayarlarınızı yansıtacak şekilde güncelleştirin.
3. Gerekli değişkenleri güncelleştirdikten sonra komut dosyasını kaydedin ve düzenleyicinizi çıkın. Sonraki adımlarda betiğinizi adlı varsayılır **my_storage_sample.sh**.
4. Gerekirse, betiği şu yürütülebilir, şekilde işaretle: `chmod +x my_storage_sample.sh`
5. Bu betiği yürütün. Örneğin, Bash hizmetinde: `./my_storage_sample.sh`

```azurecli
#!/bin/bash
# A simple Azure Stack storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
```

## <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

Microsoft Azure Depolama Gezgini, Microsoft'tan bir tek başına uygulamadır. Bu, kolayca Azure depolama ve Azure Stack depolama ile Windows, macOS ve Linux bilgisayarlara çalışmanızı sağlar. Azure Stack depolama verilerinizi yönetmek için kolay bir yol isterseniz, Microsoft Azure Depolama Gezgini'ni kullanarak düşünün.

* Azure Depolama Gezgini'ni Azure Stack ile çalışacak şekilde yapılandırma hakkında daha fazla bilgi edinmek için [Connect Depolama Gezgini'ni Azure Stack aboneliğine](azure-stack-storage-connect-se.md).
* Microsoft Azure Depolama Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini ile çalışmaya başlama](/azure/vs-azure-tools-storage-manage-with-storage-explorer)

## <a name="blobfuse"></a>Blobfuse 

[Blobfuse](https://github.com/Azure/azure-storage-fuse) Linux dosya sistemi üzerinden mevcut blok blobu verileriniz depolama hesabınızda erişmenize olanak sağlayan Azure Blob Depolama, sanal dosya sistemi sürücüsü içindir. Azure Blob Depolama, bir nesne depolama hizmetidir ve bu nedenle bir hiyerarşik ad alanı yok. Sanal dizin şeması ile ileri eğik çizgi kullanımını kullanarak bu ad alanı Blobfuse sağlar `/` sınırlayıcı olarak. Blobfuse, hem Azure hem de Azure Stack üzerinde çalışır. 

Blob Depolama Blobfuse ile bir dosya sistemi olarak Linux üzerinde bağlama hakkında daha fazla bilgi için bkz: [Blobfuse bağlama Blob depolamaya bir dosya sistemi olarak nasıl](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux). 

Azure Stack için **blobEndpoint** accountName, accountKey/sasToken, kapsayıcı adı, yanı sıra, bağlama için hazırlama adımında, depolama hesabı kimlik bilgileri yapılandırılırken belirtilmesi gerekiyor. 

Azure Stack Geliştirme Seti, blobEndpoint olmalıdır `myaccount.blob.local.azurestack.external`. Uç noktanız hakkında emin değilseniz, Azure Stack tümleşik sisteminde bulut yöneticinize başvurun. 

AccountKey ve sasToken yalnızca yapılandırılmış birer birer olabileceğini lütfen unutmayın. Depolama hesabı anahtarı verildiğinde, kimlik bilgilerini yapılandırma dosyasını aşağıdaki biçimdedir: 

```
accountName myaccount 
accountKey myaccesskey== 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

Paylaşılan erişim belirteci verildiğinde, kimlik bilgilerini yapılandırma dosyasını aşağıdaki biçimdedir:

```  
accountName myaccount 
sasToken ?mysastoken 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini, bir Azure Stack aboneliğine bağlanma](azure-stack-storage-connect-se.md)
* [Depolama Gezgini ile çalışmaya başlama](/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Azure ile tutarlı Depolama: farklılıklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md)
* [Microsoft Azure Depolama'ya giriş](/azure/storage/common/storage-introduction)
