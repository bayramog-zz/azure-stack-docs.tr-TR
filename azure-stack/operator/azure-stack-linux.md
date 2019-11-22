---
title: Azure Stack Market 'e Linux görüntüleri ekleme | Microsoft Docs
description: Azure Stack Market 'e Linux görüntüleri eklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 208e632634c59be0338c70020e7fc0fdae846797
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74299011"
---
# <a name="add-linux-images-to-azure-stack-marketplace"></a>Azure Stack Market 'e Linux görüntüleri ekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Market 'e Linux tabanlı bir görüntü ekleyerek Azure Stack Linux sanal makinelerini (VM) dağıtabilirsiniz. Azure Stack'e Linux görüntüsü eklemenin en kolay yolu, Market Yönetimi sayfasıdır. Bu görüntüler, Azure Stack ile uyumlu olacak şekilde hazırlanmış ve test edilmiştir.

## <a name="marketplace-management"></a>Market yönetimi

Linux görüntülerini Azure Marketi 'nden indirmek için bkz. [Azure 'dan Market öğelerini indirme Azure Stack](azure-stack-download-azure-marketplace-item.md). Azure Stack kullanıcıları sunmak istediğiniz Linux görüntülerini seçin.

Bu görüntülere sık sık güncelleştirmeler vardır. bu nedenle, güncel tutmak için Market yönetimini sık sık denetleyin.

## <a name="prepare-your-own-image"></a>Kendi görüntünüzü hazırlama

Mümkün olduğunda Market Yönetimi sayfasından görüntüleri indirin. Bu görüntüler, Azure Stack için hazırlanmış ve test edilmiştir.

### <a name="azure-linux-agent"></a>Azure Linux Aracısı

Azure Linux Aracısı (genellikle **walınuxagent** veya **walınuxagent**olarak adlandırılır) gereklidir ve aracının tüm sürümleri Azure Stack üzerinde çalışır. 2\.2.21 ve 2.2.34 (dahil) arasındaki sürümler Azure Stack desteklenmez. En son aracı sürümlerini 2.2.35 üzerinde kullanmak için, 1901 düzeltmesini/1902 düzeltmesini uygulayın veya Azure Stack 1903 sürümüne (veya üzeri) güncelleştirin. [Cloud-init](https://cloud-init.io/) 'nin 1910 ' den fazla Azure Stack yayımlandığında desteklendiğini unutmayın.

| Azure Stack derlemesi | Azure Linux Aracısı derlemesi |
| ------------- | ------------- |
| 1.1901.0.99 veya önceki sürümler | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 veya daha yeni |
| 1.1902.2.73  | 2.2.35 veya daha yeni |
| 1.1903.0.35  | 2.2.35 veya daha yeni |
| 1903 sonrasında derleme | 2.2.35 veya daha yeni |
| Desteklenmiyor | 2.2.21-2.2.34 |
| 1910 sonrasında derleme | Tüm Azure WALA Aracısı sürümleri|

Aşağıdaki yönergeleri kullanarak kendi Linux görüntünüzü hazırlayabilirsiniz:

* [CentOS Tabanlı Dağıtımlar](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES ve openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="cloud-init"></a>Cloud-init

[Cloud-init](https://cloud-init.io/) 1910 ' den fazla Azure Stack yayımlandığında desteklenir. Linux sanal Bilgisayarınızı özelleştirmek için Cloud-init kullanmak için aşağıdaki PowerShell yönergelerini kullanabilirsiniz: 

### <a name="step-1-create-a-cloud-inittxt-file-with-your-cloud-config"></a>1\. Adım: Cloud-config ile kabuğunuzda Cloud-init. txt dosyası oluşturma

Kabuğunuzda Cloud-init. txt adlı bir dosya oluşturun ve aşağıdaki bulut yapılandırmasını yapıştırın:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
    path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
    path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
  ```
  
### <a name="step-2-reference-the-cloud-inittxt-during-the-linux-vm-deployment"></a>2\. Adım: Linux VM dağıtımı sırasında kabuğunuzda Cloud-init. txt dosyasına başvurma

Dosyayı bir Azure depolama hesabına, Azure Stack depolama hesabına veya Azure Stack Linux sanal makinenizin erişilebilir GitHub deposuna yükleyin.
Şu anda, VM dağıtımı için Cloud-init kullanılması yalnızca REST, PowerShell ve CLı üzerinde desteklenir ve Azure Stack ilişkili bir Portal Kullanıcı arabirimine sahip değildir.

PowerShell kullanarak Linux VM oluşturmak için [Bu](../user/azure-stack-quick-create-vm-linux-powershell.md) yönergeleri takip edebilirsiniz, ancak `-CustomData` bayrağının bir parçası olarak kabuğunuzda Cloud-init. txt dosyasına başvurduğunuzdan emin olun:

```powershell
$VirtualMachine =Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred -CustomData "#include https://cloudinitstrg.blob.core.windows.net/strg/cloud-init.txt"
```

## <a name="add-your-image-to-marketplace"></a>Görüntünüzü Market 'e ekleme

[Görüntüyü Market 'e ekleyin](azure-stack-add-vm-image.md). `OSType` parametresinin `Linux`olarak ayarlandığından emin olun.

Görüntüyü Market 'e ekledikten sonra bir Market öğesi oluşturulur ve kullanıcılar bir Linux sanal makinesi dağıtabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Market öğelerini Azure Stack'e indirme](azure-stack-download-azure-marketplace-item.md)
* [Azure Stack Market 'e genel bakış](azure-stack-marketplace.md)
