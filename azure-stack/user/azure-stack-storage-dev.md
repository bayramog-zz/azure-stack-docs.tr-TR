---
title: Azure Stack depolama geliştirme araçları ile çalışmaya başlama | Microsoft Docs
description: Azure Stack depolama Geliştirme Araçları'nı kullanmaya başlama Kılavuzu
services: azure-stack
author: mattbriggs
ms.author: mabrigg
ms.date: 02/27/2019
ms.topic: conceptual
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: 28fc182404107b100c9b69ecd73d961d83bd3039
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64985608"
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Azure Stack depolama geliştirme araçları ile çalışmaya başlama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Microsoft Azure Stack blob, tablo ve kuyruk depolama içeren depolama hizmetleri kümesi sağlar.

Bu makalede, Azure Stack depolama geliştirme araçları ile çalışmaya başlamak için bir kılavuz olarak kullanın. Daha ayrıntılı bilgi ve örnek kod, karşılık gelen Azure depolama eğitimler bulabilirsiniz.

> [!NOTE]  
> Azure Stack depolama ve her platform için belirli gereksinimleri dahil olmak üzere Azure depolama arasındaki farklar bilinen vardır. Örneğin, belirli istemci kitaplıkları ve Azure Stack için belirli bir uç nokta son eki gereksinimleri vardır. Daha fazla bilgi için [Azure Stack Depolama: Farklılıklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Azure istemci kütüphaneleri

İçin depolama istemci kitaplıkları, REST API ile uyumlu bir sürümünün unutmayın. Azure Stack uç nokta da kodunuzda belirtmeniz gerekir.

### <a name="1811-update-or-newer-versions"></a>1811 güncelleştirmesi veya daha yeni sürümleri

| İstemci kitaplığı | Azure Stack desteklenen sürüm | Bağlantı | Uç nokta belirtimi |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 9.2.0 | Nuget paketi:<br>https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0 | app.config dosyası |
| Java | 7.0.0 | Maven paketi:<br>https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/7.0.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0 | Bağlantı dizesi kurulumu |
| Node.js | 2.8.3 | NPM bağlantısı:<br>https://www.npmjs.com/package/azure-storage<br>(Çalıştırın: `npm install azure-storage@2.8.3`)<br> <br>Github sürüm:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3 | Hizmet örneği bildirimi |
| C++ | 5.2.0 | Nuget paketi:<br>https://www.nuget.org/packages/Microsoft.Azure.Storage.CPP.v140/5.2.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0 | Bağlantı dizesi kurulumu |
| PHP | 1.2.0 | GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common<br>Blob: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob<br>Kuyruk:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.1.1-queue<br>Tablo: https://github.com/Azure/azure-storage-php/releases/tag/v1.1.0-table<br> <br>Oluşturucusu yükleyin (daha fazla bilgi edinmek için [aşağıdaki ayrıntılara göz atın](#install-php-client-via-composer---current).) | Bağlantı dizesi kurulumu |
| Python | 1.1.0 | GitHub sürüm:<br>Ortak:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-common<br>Blob:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob<br>Kuyruk:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-queue | Hizmet örneği bildirimi |
| Ruby | 1.0.1 | RubyGems paketi:<br>Ortak:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Kuyruk: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tablo: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>Blob: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Kuyruk: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tablo: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Bağlantı dizesi kurulumu |

#### <a name="install-php-client-via-composer---current"></a>Composer - geçerli PHP istemcisi yükleme

Oluşturucusu yüklemek için: (örnek olarak blob alın).

1. Adlı bir dosya oluşturun **composer.json** aşağıdaki kodla proje kökündeki:

    ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.2.0"
      }
    }
    ```

2. İndirme [composer.phar](https://getcomposer.org/composer.phar) proje kök dizini.
3. Çalıştır: `php composer.phar install`.

### <a name="previous-versions-1802-to-1809-update"></a>Önceki sürümler (1802 için 1809 güncelleştirme)

| İstemci kitaplığı | Azure Stack desteklenen sürüm | Bağlantı | Uç nokta belirtimi |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | Nuget paketi:<br>https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0 | app.config dosyası |
| Java | 6.1.0 | Maven paketi:<br>https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0 | Bağlantı dizesi kurulumu |
| Node.js | 2.7.0 | NPM bağlantısı:<br>https://www.npmjs.com/package/azure-storage<br>(Çalıştırın: `npm install azure-storage@2.7.0`)<br> <br>Github sürüm:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0 | Hizmet örneği bildirimi |
| C++ | 3.1.0 | Nuget paketi:<br>https://www.nuget.org/packages/wastorage.v140/3.1.0<br> <br>GitHub sürüm:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0 | Bağlantı dizesi kurulumu |
| PHP | 1.0.0 | GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common<br>Blob: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob<br>Kuyruk:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue<br>Tablo: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table<br> <br>Oluşturucusu aracılığıyla yükleyin (aşağıdaki ayrıntılarına bakın).) | Bağlantı dizesi kurulumu |
| Python | 1.0.0 | GitHub sürüm:<br>Ortak:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common<br>Blob:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob<br>Kuyruk:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue | Hizmet örneği bildirimi |
| Ruby | 1.0.1 | RubyGems paketi:<br>Ortak:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>Blob: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Kuyruk: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>Tablo: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>GitHub sürüm:<br>Ortak: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>Blob: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Kuyruk: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>Tablo: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Bağlantı dizesi kurulumu |

#### <a name="install-php-client-via-composer---previous"></a>PHP istemci Oluşturucusu - önceki aracılığıyla yükleme

Oluşturucusu yüklemek için: (örnek olarak blob Al).

1. Adlı bir dosya oluşturun **composer.json** aşağıdaki kodla proje kökündeki:

   ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
   ```

2. İndirme [composer.phar](https://getcomposer.org/composer.phar) proje kök dizini.
3. Çalıştır: `php composer.phar install`.

## <a name="endpoint-declaration"></a>Uç nokta bildirimini

Bir Azure Stack uç nokta iki bölümleri içerir: bir bölge ve Azure Stack etki alanı adı.
Azure Stack geliştirme Seti'ni varsayılan uç noktadır **local.azurestack.external**.
Uç noktanız hakkında emin değilseniz, bulut yöneticinize başvurun.

## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Azure Stack için uç nokta soneki app.config dosyasında belirtilir:

```xml
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```

### <a name="java"></a>Java

Azure Stack için uç nokta soneki bağlantı dizesinin ayarında belirtilen:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Azure Stack için uç nokta son eki bildirimi örneğinde belirtilir:

```nodejs
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```

### <a name="c"></a>C++

Azure Stack için uç nokta soneki bağlantı dizesinin ayarında belirtilen:

```cpp
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Azure Stack için uç nokta soneki bağlantı dizesinin ayarında belirtilen:

```php
$connectionString = 'BlobEndpoint=https://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=https:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=https:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Azure Stack için uç nokta son eki bildirimi örneğinde belirtilir:

```python
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```

### <a name="ruby"></a>Ruby

Azure Stack için uç nokta soneki bağlantı dizesinin ayarında belirtilen:

```ruby
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki Azure Blob Depolama öğreticilerde, Azure Stack için geçerlidir. Önceki açıklanan Azure Stack için belirli bir uç nokta son eki gereksinimi Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs)
* [Java'da Blob Depolama'yı kullanma](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Node.js'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-nodejs-how-to-use-blob-storage)
* [BLOB depolama alanından C++ kullanma](/azure/storage/blobs/storage-c-plus-plus-how-to-use-blobs)
* [PHP'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-php-how-to-use-blobs)
* [Python'dan Azure Blob Depolama kullanma](/azure/storage/blobs/storage-python-how-to-use-blob-storage)
* [Ruby'de Blob Depolama'yı kullanma](/azure/storage/blobs/storage-ruby-how-to-use-blob-storage)

## <a name="queue-storage"></a>Kuyruk depolama

Aşağıdaki Azure kuyruk depolama öğreticilerde, Azure Stack için geçerlidir. Önceki açıklanan Azure Stack için belirli bir uç nokta son eki gereksinimi Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](/azure/storage/queues/storage-dotnet-how-to-use-queues)
* [Java'da Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Node.js'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Kuyruk Depolama'yı C++ kullanma](/azure/storage/queues/storage-c-plus-plus-how-to-use-queues)
* [PHP'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-php-how-to-use-queues)
* [Python'da Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-python-how-to-use-queue-storage)
* [Ruby'de Kuyruk Depolama'yı kullanma](/azure/storage/queues/storage-ruby-how-to-use-queue-storage)

## <a name="table-storage"></a>Table Storage

Aşağıdaki Azure tablo depolama öğreticilerde, Azure Stack için geçerlidir. Önceki açıklanan Azure Stack için belirli bir uç nokta son eki gereksinimi Not [örnekler](#examples) bölümü.

* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](/azure/cosmos-db/table-storage-how-to-use-dotnet)
* [Java'da Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-java)
* [Node.js'den Azure Tablo depolamayı kullanma](/azure/cosmos-db/table-storage-how-to-use-nodejs)
* [Tablo depolama'yı C++ kullanma](/azure/cosmos-db/table-storage-how-to-use-c-plus)
* [PHP'de Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-php)
* [Python'da tablo depolama kullanma](/azure/cosmos-db/table-storage-how-to-use-python)
* [Ruby'de Tablo Depolama'yı kullanma](/azure/cosmos-db/table-storage-how-to-use-ruby)

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama'ya giriş](/azure/storage/common/storage-introduction)
