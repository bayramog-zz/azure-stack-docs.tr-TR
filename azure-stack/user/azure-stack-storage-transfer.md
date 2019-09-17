---
title: Azure Stack Storage 'da veri aktarımı araçlarını kullanma | Microsoft Docs
description: Azure Stack depolama veri aktarımı araçları hakkında bilgi edinin.
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
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 87e9021c6d2b085c95e186e410b009ae89753311
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71010803"
---
# <a name="use-data-transfer-tools-in-azure-stack-storage"></a>Azure Stack depolamada veri aktarımı araçlarını kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Azure Stack, diskler, Bloblar, tablolar, kuyruklar ve hesap yönetimi işlevleri için bir depolama hizmetleri kümesi sağlar. Bazı Azure depolama araçları, verileri veya Azure Stack depolama alanından yönetmek veya taşımak istiyorsanız kullanılabilir. Bu makalede, kullanılabilir araçlara genel bir bakış sunulmaktadır.

Gereksinimleriniz aşağıdaki araçlardan hangisinin sizin için en iyi şekilde çalışacağını belirlemektir:

* [AzCopy](#azcopy)

    Bir nesneden verileri depolama hesaplarınız içinde veya arasında başka bir nesneye kopyalamak için indirebileceğiniz, depolamaya özgü bir komut satırı yardımcı programı.

* [Azure PowerShell](#azure-powershell)

    Özellikle sistem yönetimi için tasarlanan görev tabanlı, komut satırı kabuğu ve betik dili.

* [Azure CLI](#azure-cli)

    Azure ve Azure Stack platformlarıyla çalışmaya yönelik bir dizi komut sağlayan açık kaynaklı, platformlar arası bir araç.

* [Microsoft Depolama Gezgini](#microsoft-azure-storage-explorer)

    Kullanıcı arabirimiyle kullanımı kolay bir tek başına uygulama.

* [Blobsigortası](#blobfuse)

    Azure Blob depolama için bir sanal dosya sistemi sürücüsü, Linux dosya sistemi aracılığıyla Depolama hesabınızdaki mevcut Blok Blobu verilerinize erişmenizi sağlar.

Azure ile Azure Stack arasındaki depolama hizmetleri farklılıkları nedeniyle, aşağıdaki bölümlerde açıklanan her bir araç için bazı özel gereksinimler olabilir. Azure Stack depolama ile Azure depolama arasında bir karşılaştırma için bkz [. Azure Stack depolama: Farklar ve önemli](azure-stack-acs-differences.md)noktalar.

## <a name="azcopy"></a>AzCopy

AzCopy, en iyi performansa sahip basit komutları kullanarak Microsoft Azure Blob ve tablo depolamadan veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. Verileri, depolama hesaplarınız içinde veya arasında bir nesneden diğerine kopyalayabilirsiniz.

### <a name="download-and-install-azcopy"></a>AzCopy indirin ve yükleyin

::: moniker range=">=azs-1811"
* 1811 güncelleştirmesi veya daha yeni sürümler için [AzCopy ile v10 arasındaki + indirin](/azure/storage/common/storage-use-azcopy-v10#download-azcopy).
::: moniker-end

::: moniker range="<azs-1811"
* Önceki sürümler için (1802 1809 güncelleştirme), [AzCopy 7.1.0 indirin](https://aka.ms/azcopyforazurestack20170417).
::: moniker-end

### <a name="azcopy-101-configuration-and-limits"></a>AzCopy 10,1 yapılandırma ve sınırları

AzCopy 10,1 artık eski API sürümlerini kullanacak şekilde yapılandırılabilir. Bu, Azure Stack için destek sunar (sınırlı).
AzCopy için API sürümünü Azure Stack destekleyecek şekilde yapılandırmak için, `AZCOPY_DEFAULT_SERVICE_API_VERSION` ortam değişkenini olarak `2017-11-09`ayarlayın.

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | Komut isteminde şunu kullanın:`set AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`<br> PowerShell kullanımı:`$env:AZCOPY_DEFAULT_SERVICE_API_VERSION="2017-11-09"`|
| **Linux** | `export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09` |
| **MacOS** | `export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09` |

AzCopy 10,1 ' de, aşağıdaki özellikler Azure Stack desteklenir:

| Özellik | Desteklenen eylemler |
| --- | --- |
|Kapsayıcıyı Yönet|Bir kapsayıcı oluşturma<br>Kapsayıcıların içeriğini listeleyin
|İşi Yönet|İşleri görüntüle<br>İşi sürdürür
|Blobu kaldır|Tek bir blobu kaldırma<br>Tüm veya kısmi sanal dizini Kaldır
|Dosya yükle|Karşıya dosya yükle<br>Bir dizini karşıya yükle<br>Bir dizinin içeriğini karşıya yükleme
|Dosyayı indir|Dosya indir<br>Bir dizin indirin<br>Bir dizinin içeriğini indirin
|Dosya eşitlemesini|Kapsayıcıyı yerel dosya sistemiyle eşitler<br>Yerel bir dosya sistemini bir kapsayıcıya eşitler

   > [!NOTE]
   > * Azure Stack, AzCopy için Azure Active Directory (AD) kullanarak yetkilendirme kimlik bilgileri sağlamayı desteklemez. Paylaşılan erişim Imzası (SAS) belirtecini kullanarak Azure Stack depolama nesnelerine erişmeniz gerekir.
   > * Azure Stack iki Azure Stack blob konumu arasında ve Azure depolama ile Azure Stack arasında zaman uyumlu veri aktarımını desteklemez. Azure Stack verileri Azure depolama 'ya (veya başka bir yolla) doğrudan AzCopy 10,1 ile taşımak için "AzCopy CP" kullanamazsınız.

### <a name="azcopy-command-examples-for-data-transfer"></a>Veri aktarımı için AzCopy komutu örnekleri

Aşağıdaki örnekler, Azure Stack bloblarına ve bunlara veri kopyalamak için tipik senaryoları izler. Daha fazla bilgi için bkz. [AzCopy ile çalışmaya başlama](/azure/storage/common/storage-use-azcopy-v10).

### <a name="download-all-blobs-to-a-local-disk"></a>Tüm Blobları yerel bir diske indir

```
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true
```

### <a name="upload-single-file-to-virtual-directory"></a>Sanal dizine tek bir dosya yükle

```
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

### <a name="azcopy-known-issues"></a>AzCopy bilinen sorunlar

 - Dosya depolama alanı Azure Stack ' de henüz kullanılamadığından, dosya depolaması üzerinde herhangi bir AzCopy işlemi kullanılamaz.
 - İki Azure Stack blob konumu arasında veya AzCopy 10.1 kullanarak Azure Stack ile Azure depolama arasında veri aktarmak istiyorsanız, önce verileri önce yerel bir konuma indirmeniz ve sonra Azure Stack veya Azure depolama alanında hedef dizine yeniden yüklemeniz gerekir. Veya AzCopy 7,1 ' i kullanabilir ve verileri kopyalamak için **/Synccopy** seçeneğiyle birlikte aktarmayı belirtebilirsiniz.  
 - AzCopy 'in Linux sürümü yalnızca 1802 güncelleştirme veya sonraki sürümlerini destekler ve tablo hizmetini desteklemez.
 - Azure Tablo depolama hizmetinize ve bu kaynaktan veri kopyalamak istiyorsanız, [AzCopy sürüm 7.3.0 'i yükleyebilirsiniz](https://aka.ms/azcopyforazurestack20171109)
 
## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, hem Azure hem de Azure Stack hizmetleri yönetmek için cmdlet 'ler sağlayan bir modüldür. Bu, özellikle sistem yönetimi için tasarlanan görev tabanlı, komut satırı kabuğu ve betik dilidir.

### <a name="install-and-configure-powershell-for-azure-stack"></a>Azure Stack için PowerShell 'i yükleyip yapılandırma

Azure Stack uyumlu Azure PowerShell modüllerinin Azure Stack ile çalışması gerekir. Daha fazla bilgi için bkz. [Azure Stack Için PowerShell 'ı yükleyip](../operator/azure-stack-powershell-install.md) [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md).

### <a name="powershell-sample-script-for-azure-stack"></a>Azure Stack için PowerShell örnek betiği 

Bu örnek, [Azure Stack Için PowerShell](../operator/azure-stack-powershell-install.md)'ı başarıyla yüklediğinizi varsayar. Bu betik, yapılandırmayı tamamlamanıza ve Azure Stack Kiracı kimlik bilgilerinizin hesabınızı yerel PowerShell ortamına eklemesini istemek için size yardımcı olur. Betik daha sonra varsayılan Azure aboneliğini ayarlar, Azure 'da yeni bir depolama hesabı oluşturur, bu yeni depolama hesabında yeni bir kapsayıcı oluşturur ve var olan bir görüntü dosyasını (blob) bu kapsayıcıya yükler. Komut dosyası, bu kapsayıcıdaki tüm Blobları listeledikten sonra, yerel bilgisayarınızda yeni bir hedef dizin oluşturur ve görüntü dosyasını indirirler.

1. Yükleme [Azure Stack ile uyumlu Azure PowerShell modüllerini](../operator/azure-stack-powershell-install.md).
2. İndirme [Azure Stack ile çalışması için gereken araçları](../operator/azure-stack-powershell-download.md).
3. **Windows PowerShell ISE** açın ve **yönetici olarak çalıştır**' a tıklayın ve ardından yeni **Dosya** > ' ya tıklayarak yeni bir komut dosyası oluşturun.
4. Aşağıdaki betiği kopyalayın ve yeni betik dosyasına yapıştırın.
5. Yapılandırma ayarlarınıza bağlı olarak komut dosyası değişkenlerini güncelleştirin.
   > [!NOTE]
   > Bu betik, **AzureStack_Tools**için kök dizinde çalıştırılmalıdır.

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

### <a name="powershell-known-issues"></a>Bilinen PowerShell sorunları

Azure Stack için geçerli uyumlu Azure PowerShell modülü sürümü Kullanıcı işlemleri için 1.2.11. En son Azure PowerShell sürümünden farklıdır. Bu fark, depolama hizmetleri işlemini aşağıdaki şekilde etkiler:

Version 1.2.11 `Get-AzureRmStorageAccountKey` içindeki dönüş değeri biçimi iki özelliğe sahiptir: `Key1` ve `Key2`geçerli Azure sürümü, tüm hesap anahtarlarını içeren bir dizi döndürür.

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

Daha fazla bilgi için bkz. [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey).

## <a name="azure-cli"></a>Azure CLI

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. Bunu macOS, Linux ve Windows 'a yükleyebilir ve komut satırından çalıştırabilirsiniz.

Azure CLı, Azure kaynaklarını komut satırından yönetmek ve yönetmek için ve Azure Resource Manager karşı çalışan Otomasyon betikleri oluşturmak için en iyi duruma getirilmiştir. Azure Stack portalında bulunan ve zengin veri erişimi de dahil olmak üzere aynı işlevlerin birçoğunu sağlar.

Azure Stack için Azure CLı 2,0 veya üzeri bir sürüm gerekir. Azure Stack ile Azure CLı 'yi yükleme ve yapılandırma hakkında daha fazla bilgi için bkz. [Azure Stack CLI 'Yi yükleme ve yapılandırma](azure-stack-version-profiles-azurecli2.md). Azure CLı 'yı kullanarak Azure Stack depolama hesabınızda kaynaklarla çalışan çeşitli görevleri gerçekleştirme hakkında daha fazla bilgi için, bkz. Azure [CLI ile Azure depolama Ile kullanma](/azure/storage/storage-azure-cli).

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure Stack için Azure CLı örnek betiği

CLı yükleme ve yapılandırmasını tamamladıktan sonra, Azure Stack depolama kaynaklarıyla etkileşim kurmak için küçük bir Shell örnek betiği ile çalışmak üzere aşağıdaki adımları deneyebilirsiniz. Betik aşağıdaki eylemleri tamamlar:

* Depolama hesabınızda yeni bir kapsayıcı oluşturur.
* Var olan bir dosyayı (blob olarak) kapsayıcıya yükler.
* Kapsayıcıdaki tüm Blobları listeler.
* Dosyayı, belirttiğiniz yerel bilgisayarınızdaki bir hedefe indirir.

Bu betiği çalıştırmadan önce, hedef Azure Stack başarıyla bağlanıp oturum açarak emin olun.

1. En sevdiğiniz metin düzenleyicinizi açın, ardından önceki betiği kopyalayıp düzenleyiciye yapıştırın.
2. Betiklerin değişkenlerini yapılandırma ayarlarınızı yansıtacak şekilde güncelleştirin.
3. Gerekli değişkenleri güncelleştirdikten sonra, betiği kaydedin ve düzenleyiciden çıkın. Sonraki adımlarda, **my_storage_sample. sh**betiğinizi adlandırdığınızı varsayalım.
4. Gerekirse betiği çalıştırılabilir olarak işaretleyin:`chmod +x my_storage_sample.sh`
5. Betiği yürütün. Örneğin, bash:`./my_storage_sample.sh`

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

Azure Depolama Gezgini, Microsoft 'un tek başına uygulamasıdır. Hem Azure Storage hem de Windows, macOS ve Linux bilgisayarlarda Azure Stack Depolama verileriyle kolayca çalışmanıza olanak sağlar. Azure Stack depolama verilerinizi yönetmenin kolay bir yolunu istiyorsanız Microsoft Azure Depolama Gezgini kullanmayı göz önünde bulundurun.

* Azure Depolama Gezgini Azure Stack ile çalışacak şekilde yapılandırma hakkında daha fazla bilgi edinmek için bkz [Depolama Gezgini Azure Stack aboneliğine bağlanma](azure-stack-storage-connect-se.md).
* Microsoft Azure Depolama Gezgini hakkında daha fazla bilgi edinmek için bkz. [Depolama Gezgini ile çalışmaya başlama](/azure/vs-azure-tools-storage-manage-with-storage-explorer)

## <a name="blobfuse"></a>Blobsigortası 

[Blobsigortası](https://github.com/Azure/azure-storage-fuse) , Azure Blob depolama için bir sanal dosya sistemi sürücüsüdür ve bu sayede, Linux dosya sistemi aracılığıyla Depolama hesabınızdaki mevcut Blok Blobu verilerinize erişebilirsiniz. Azure Blob Storage bir nesne depolama hizmetidir ve bu nedenle hiyerarşik bir ad alanı yoktur. Blobsigortası, bir sınırlayıcı olarak eğik çizgi `/` kullanımıyla sanal dizin düzenini kullanarak bu ad alanını sağlar. Blobsigortası hem Azure hem de Azure Stack üzerinde çalışmaktadır. 

Linux üzerinde Blobsigortası içeren bir dosya sistemi olarak BLOB depolama takma hakkında daha fazla bilgi edinmek için bkz. [BLOB depolamayı bir dosya sistemi olarak Blobsigortası ile bağlama](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux). 

Azure Stack için, depolama hesabı kimlik bilgilerinizi accountName, accountKey/sasToken ve containerName ile birlikte yapılandırırken *Blobendpoint* belirtilmelidir.

Azure Stack Geliştirme Seti (ASDK) ' de *Blobendpoint* olmalıdır `myaccount.blob.local.azurestack.external`. Azure Stack tümleşik sistemde, uç noktanız hakkında emin değilseniz bulut yöneticinize başvurun.

*Accountkey* ve *sastoken* , tek seferde yalnızca bir tane yapılandırılabilir. Bir depolama hesabı anahtarı verildiğinde, kimlik bilgileri yapılandırma dosyası aşağıdaki biçimdedir:

```
accountName myaccount 
accountKey myaccesskey== 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

Paylaşılan erişim belirteci verildiğinde, kimlik bilgileri yapılandırma dosyası aşağıdaki biçimdedir:

```  
accountName myaccount 
sasToken ?mysastoken 
containerName mycontainer 
blobEndpoint myaccount.blob.local.azurestack.external
```

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezginini bir Azure Stack aboneliğine bağlama](azure-stack-storage-connect-se.md)
* [Depolama Gezgini 'ni kullanmaya başlama](/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Azure ile tutarlı depolama: farklar ve konular](azure-stack-acs-differences.md)
* [Microsoft Azure depolama 'ya giriş](/azure/storage/common/storage-introduction)
