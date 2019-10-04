---
title: Azure Stack Storage geliştirme araçları 'nı kullanmaya başlama | Microsoft Docs
description: Azure Stack Storage geliştirme araçları 'nı kullanmaya başlama kılavuzu
services: azure-stack
author: mattbriggs
ms.author: mabrigg
ms.date: 10/02/2019
ms.topic: conceptual
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: 407d8167fcc359531496397dda00e51d59f49d7c
ms.sourcegitcommit: a7207f4a4c40d4917b63e729fd6872b3dba72968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71909602"
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Azure Stack Storage geliştirme araçları 'nı kullanmaya başlama

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti*

Microsoft Azure Stack blob, tablo ve kuyruk depolama alanı içeren bir depolama hizmetleri kümesi sağlar.

Azure Stack depolama geliştirme araçları 'nı kullanmaya başlamak için bu makaleyi kılavuz olarak kullanın. İlgili Azure depolama öğreticilerinde daha ayrıntılı bilgi ve örnek kod bulabilirsiniz.

> [!NOTE]  
> Azure Stack depolama ile Azure depolama arasında her platform için belirli gereksinimler de dahil olmak üzere farklar vardır. Örneğin, Azure Stack için belirli istemci kitaplıkları ve uç nokta soneki gereksinimleri vardır. Daha fazla bilgi için bkz. [Azure Stack Storage: Farklar ve önemli](azure-stack-acs-differences.md)noktalar.

## <a name="azure-client-libraries"></a>Azure istemci kitaplıkları

Depolama istemci kitaplıkları için, REST API uyumlu olan sürümden haberdar olun. Kodunuzda Azure Stack uç noktasını da belirtmeniz gerekir.

::: moniker range=">=azs-1811"
### <a name="1811-update-or-newer-versions"></a>1811 güncelleştirme veya daha yeni sürümler

| İstemci kitaplığı | Desteklenen Azure Stack sürümü | Bağlantı | Uç nokta belirtimi |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 9.2.0 | NuGet paketi:<br><https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0> | App. config dosyası |
| Java | 7.0.0 | Maven paketi:<br><https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/7.0.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0> | Bağlantı dizesi kurulumu |
| Node.js | 2.8.3 | NPM bağlantısı:<br><https://www.npmjs.com/package/azure-storage><br>(Çalıştır: `npm install azure-storage@2.8.3`)<br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3> | Hizmet örneği bildirimi |
| C++ | 5.2.0 | NuGet paketi:<br><https://www.nuget.org/packages/Microsoft.Azure.Storage.CPP.v140/5.2.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0> | Bağlantı dizesi kurulumu |
| PHP | 1.2.0 | GitHub sürümü:<br>Ortak: <https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common><br>Blob: <https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob><br>Sıradaki<br><https://github.com/Azure/azure-storage-php/releases/tag/v1.1.1-queue><br>Tablo: <https://github.com/Azure/azure-storage-php/releases/tag/v1.1.0-table><br> <br>Besteci aracılığıyla Install (daha fazla bilgi Için [aşağıdaki ayrıntılara bakın](#install-php-client-via-composer---current).) | Bağlantı dizesi kurulumu |
| Python | 1.1.0 | GitHub sürümü:<br>Birçok<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-common><br>Bun<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob><br>Sıradaki<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-queue> | Hizmet örneği bildirimi |
| Ruby | 1.0.1 | RubyGems paketi:<br>Birçok<br><https://rubygems.org/gems/azure-storage-common/versions/1.0.1><br>Blob: <https://rubygems.org/gems/azure-storage-blob/versions/1.0.1><br>Sıra: <https://rubygems.org/gems/azure-storage-queue/versions/1.0.1><br>Tablo: <https://rubygems.org/gems/azure-storage-table/versions/1.0.1><br> <br>GitHub sürümü:<br>Ortak: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common><br>Blob: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob><br>Sıra: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue><br>Tablo: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table> | Bağlantı dizesi kurulumu |

#### <a name="install-php-client-via-composer---current"></a>PHP istemcisini besteci aracılığıyla Install-Current

Besteci aracılığıyla yüklemek için: (blobu örnek olarak alın).

1. Projenin kökünde aşağıdaki kodla birlikte **besteci. JSON** adlı bir dosya oluşturun:

    ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.2.0"
      }
    }
    ```

2. [Besteci. phar](https://getcomposer.org/composer.phar) 'yi proje köküne indirin.
3. Şunu çalıştırın: `php composer.phar install`.
::: moniker-end

::: moniker range=">=azs-1802 <=azs-1809"
### <a name="previous-versions-1802-to-1809-update"></a>Önceki sürümler (1802 ile 1809 güncelleştirme)

| İstemci kitaplığı | Desteklenen Azure Stack sürümü | Bağlantı | Uç nokta belirtimi |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | NuGet paketi:<br><https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0> | App. config dosyası |
| Java | 6.1.0 | Maven paketi:<br><https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0> | Bağlantı dizesi kurulumu |
| Node.js | 2.7.0 | NPM bağlantısı:<br><https://www.npmjs.com/package/azure-storage><br>(Çalıştır: `npm install azure-storage@2.7.0`)<br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0> | Hizmet örneği bildirimi |
| C++ | 3.1.0 | NuGet paketi:<br><https://www.nuget.org/packages/wastorage.v140/3.1.0><br> <br>GitHub sürümü:<br><https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0> | Bağlantı dizesi kurulumu |
| PHP | 1.0.0 | GitHub sürümü:<br>Ortak: <https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common><br>Blob: <https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob><br>Sıradaki<br><https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue><br>Tablo: <https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table><br> <br>Besteci aracılığıyla Install (aşağıdaki ayrıntılara bakın).) | Bağlantı dizesi kurulumu |
| Python | 1.0.0 | GitHub sürümü:<br>Birçok<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common><br>Bun<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob><br>Sıradaki<br><https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue> | Hizmet örneği bildirimi |
| Ruby | 1.0.1 | RubyGems paketi:<br>Birçok<br><https://rubygems.org/gems/azure-storage-common/versions/1.0.1><br>Blob: <https://rubygems.org/gems/azure-storage-blob/versions/1.0.1><br>Sıra: <https://rubygems.org/gems/azure-storage-queue/versions/1.0.1><br>Tablo: <https://rubygems.org/gems/azure-storage-table/versions/1.0.1><br> <br>GitHub sürümü:<br>Ortak: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common><br>Blob: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob><br>Sıra: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue><br>Tablo: <https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table> | Bağlantı dizesi kurulumu |

#### <a name="install-php-client-via-composer---previous"></a>PHP istemcisini besteci aracılığıyla Install-Previous

Besteci aracılığıyla yüklemek için: (blobu örnek olarak al).

1. Projenin kökünde aşağıdaki kodla birlikte **besteci. JSON** adlı bir dosya oluşturun:

   ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
   ```

2. [Besteci. phar](https://getcomposer.org/composer.phar) 'yi proje köküne indirin.
3. Şunu çalıştırın: `php composer.phar install`.
:::moniker-end

## <a name="endpoint-declaration"></a>Uç nokta bildirimi

Azure Stack uç noktası iki bölümden oluşur: bir bölgenin adı ve Azure Stack etki alanı.
Azure Stack Geliştirme Seti, varsayılan uç nokta **Local. azurestack. external**' dır.
Uç noktanız hakkında emin değilseniz bulut yöneticinize başvurun.

## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Azure Stack için uç nokta soneki App. config dosyasında belirtilir:

```xml
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```

### <a name="java"></a>Java

Azure Stack için uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Azure Stack için uç nokta soneki bildirim örneğinde belirtilir:

```nodejs
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```

### <a name="c"></a>C++

Azure Stack için uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```cpp
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Azure Stack için uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```php
$connectionString = 'BlobEndpoint=https://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=https:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=https:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Azure Stack için uç nokta soneki bildirim örneğinde belirtilir:

```python
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```

### <a name="ruby"></a>Ruby

Azure Stack için uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```ruby
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki Azure Blob depolama öğreticileri Azure Stack için geçerlidir. Önceki [örnekler](#examples) bölümünde açıklanan Azure Stack için belirli uç nokta soneki gereksinimini aklınızda edin.

* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)
* [Java'da Blob Depolama'yı kullanma](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Node.js'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-nodejs-how-to-use-blob-storage)
* [Öğesinden blob depolamayı kullanmaC++](/azure/storage/blobs/storage-c-plus-plus-how-to-use-blobs)
* [PHP'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-php-how-to-use-blobs)
* [Python 'dan Azure Blob depolamayı kullanma](/azure/storage/blobs/storage-python-how-to-use-blob-storage)
* [Ruby'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-ruby-how-to-use-blob-storage)

## <a name="queue-storage"></a>Kuyruk depolama

Aşağıdaki Azure kuyruk depolama öğreticileri Azure Stack için geçerlidir. Önceki [örnekler](#examples) bölümünde açıklanan Azure Stack için belirli uç nokta soneki gereksinimini aklınızda edin.

* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](/azure/storage/queues/storage-dotnet-how-to-use-queues)
* [Java'da Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Node.js'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Kuyruk depolamayı kullanmaC++](/azure/storage/queues/storage-c-plus-plus-how-to-use-queues)
* [PHP'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-php-how-to-use-queues)
* [Python'da Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-python-how-to-use-queue-storage)
* [Ruby'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-ruby-how-to-use-queue-storage)

## <a name="table-storage"></a>Tablo depolama alanı

Aşağıdaki Azure Tablo depolama öğreticileri Azure Stack için geçerlidir. Önceki [örnekler](#examples) bölümünde açıklanan Azure Stack için belirli uç nokta soneki gereksinimini aklınızda edin.

* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](/azure/cosmos-db/table-storage-how-to-use-dotnet)
* [Java'da Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-java)
* [Node.js'den Azure Tablo depolamayı kullanma](/azure/cosmos-db/table-storage-how-to-use-nodejs)
* [Öğesinden Tablo Depolamayı kullanmaC++](/azure/cosmos-db/table-storage-how-to-use-c-plus)
* [PHP'de Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-php)
* [Python 'da Tablo Depolamayı kullanma](/azure/cosmos-db/table-storage-how-to-use-python)
* [Ruby'de Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-ruby)

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure depolama 'ya giriş](/azure/storage/common/storage-introduction)
