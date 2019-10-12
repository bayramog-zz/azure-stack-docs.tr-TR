---
title: Azure Stack için Kubernetes panosuna erişin | Microsoft Docs
description: Azure Stack 'deki Kubernetes panosuna erişmeyi öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 06/18/2019
ms.openlocfilehash: 2c1a762f002e5058e11857117b4210ad0b59e564
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277554"
---
# <a name="access-the-kubernetes-dashboard-in-azure-stack"></a>Azure Stack Kubernetes panosuna erişin 

*Uygulama hedefi: Azure Stack tümleşik sistemler ve Azure Stack Geliştirme Seti* 
> [!Note]   
> Azure Stack Kubernetes önizleme aşamasındadır. Azure Stack bağlantısı kesik bir senaryo şu anda önizleme tarafından desteklenmiyor. Yalnızca geliştirme ve test senaryoları için Market öğesini kullanın.

Kubernetes, temel yönetim işlemleri için kullanabileceğiniz bir Web panosu içerir. Bu pano uygulamalarınızın temel sistem durumunu ve ölçümlerini görüntülemenize, hizmetler oluşturmanıza ve dağıtmanıza ve mevcut uygulamaları düzenlemenize olanak tanır. Bu makalede, Azure Stack üzerinde Kubernetes panosunu ayarlama gösterilmektedir.

## <a name="prerequisites-for-kubernetes-dashboard"></a>Kubernetes panosu önkoşulları

* Kubernetes kümesi Azure Stack

    Azure Stack için bir Kubernetes kümesi dağıtmış olmanız gerekir. Daha fazla bilgi için bkz. [Kubernetes 'ı dağıtma](azure-stack-solution-template-kubernetes-deploy.md).

* SSH istemcisi

    Kümedeki ana düğümünüz için güvenlik bağlantısı sağlamak üzere bir SSH istemcisine ihtiyacınız olacak. Windows kullanıyorsanız, [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm)kullanabilirsiniz. Kubernetes kümenizi dağıtırken kullanılan özel anahtara ihtiyacınız olacak.

* FTP (PSCP)

    Ayrıca, sertifikaları ana düğümden Azure Stack yönetim makinenize aktarmak için SSH ve SSH Dosya Aktarım Protokolü destekleyen bir FTP istemcisi de gerekebilir. [FileZilla](https://filezilla-project.org/download.php?type=client)kullanabilirsiniz. Kubernetes kümenizi dağıtırken kullanılan özel anahtara ihtiyacınız olacak.

## <a name="overview-of-steps-to-enable-dashboard"></a>Panoyu etkinleştirme adımlarına genel bakış

1.  Kubernetes sertifikalarını kümedeki ana düğümden dışarı aktarın. 
2.  Sertifikaları Azure Stack yönetimi makinenize aktarın.
2.  Kubernetes web panosunu açın. 

## <a name="export-certificate-from-the-master"></a>Sertifikayı ana bilgisayardan dışarı aktar 

Panonun URL 'sini kümenizdeki ana düğümden alabilirsiniz.

1. Azure Stack panosundan küme ana yöneticinize ait genel IP adresini ve Kullanıcı adını alın. Bu bilgileri almak için:

    - [Azure Stack portalında](https://portal.local.azurestack.external/) oturum açın
    - Tüm **hizmetler** > **tüm kaynaklar**' ı seçin. Küme kaynak grubunuzda ana öğeyi bulun. Ana öğe `k8s-master-<sequence-of-numbers>` olarak adlandırılmıştır. 

2. Portalda ana düğümü açın. **Genel IP** adresini kopyalayın. **VM yerel hesap kutusunu kullanarak Kullanıcı adınızı oturum açma** bölümünde almak için **Bağlan** ' a tıklayın. Bu, kümenizi oluştururken ayarladığınız kullanıcı adıdır. Bağlantı dikey penceresinde listelenen özel IP adresi yerine genel IP adresini kullanın.

3.  Ana sunucuya bağlanmak için bir SSH istemcisi açın. Windows üzerinde çalışıyorsanız, bağlantıyı oluşturmak için [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm) kullanabilirsiniz. Ana düğüm için genel IP adresini, Kullanıcı adını kullanacaksınız ve kümeyi oluştururken kullandığınız özel anahtarı eklersiniz.

4.  Terminal bağlandığı zaman, Kubernetes komut satırı istemcisini açmak için `kubectl` yazın.

5. Şu komutu çalıştırın:

    ```Bash   
    kubectl cluster-info 
    ``` 
    Panonun URL 'sini bulun. Örneğin: `https://k8-1258.local.cloudapp.azurestack.external/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy`

6.  Otomatik olarak imzalanan sertifikayı ayıklayın ve PFX biçimine dönüştürün. Şu komutu çalıştırın:

    ```Bash  
    sudo su 
    openssl pkcs12 -export -out /etc/kubernetes/certs/client.pfx -inkey /etc/kubernetes/certs/client.key  -in /etc/kubernetes/certs/client.crt -certfile /etc/kubernetes/certs/ca.crt 
    ```

7.  **Kuto-System** ad alanındaki gizli dizi listesini alın. Şu komutu çalıştırın:

    ```Bash  
    kubectl -n kube-system get secrets
    ```

    Kubernetes-Dashboard-Token-\<XXXXX > değerini unutmayın. 

8.  Belirteci alın ve kaydedin. Önceki adımdaki gizli değer ile `kubernetes-dashboard-token-<####>` ' yı güncelleştirin.

    ```Bash  
    kubectl -n kube-system describe secret kubernetes-dashboard-token-<####>| awk '$1=="token:"{print $2}' 
    ```

## <a name="import-the-certificate"></a>Sertifikayı içeri aktar

1. FileZilla açın ve ana düğüme bağlanın. Şunları yapmanız gerekir:

    - ana düğüm genel IP 'si
    - Kullanıcı adı
    - özel gizli dizi
    - **SFTP-SSH dosya aktarım protokolü** kullanma

2. @No__t-0 ve `/etc/kubernetes/certs/ca.crt` Azure Stack yönetim makinenize kopyalayın.

3. Dosya konumlarını unutmayın. Betiği konumlarla güncelleştirin ve ardından PowerShell 'i yükseltilmiş bir istem ile açın. Güncelleştirilmiş betiği çalıştırın:  

    ```powershell   
    Import-Certificate -Filepath "ca.crt" -CertStoreLocation cert:\LocalMachine\Root 
    $pfxpwd = Get-Credential -UserName 'Enter password below' -Message 'Enter password below' 
    Import-PfxCertificate -Filepath "client.pfx" -CertStoreLocation cert:\CurrentUser\My -Password $pfxpwd.Password 
    ``` 

## <a name="open-the-kubernetes-dashboard"></a>Kubernetes panosunu açma 

1. Web tarayıcınızda açılır pencere engelleyicisini devre dışı bırakın.

2. Tarayıcınızı `kubectl cluster-info` komutunu çalıştırdığınızda belirtilen URL 'ye işaret edin. Örneğin: https: \//azurestackdomainnamefork8sdashboard/api/v1/namespaces/kuin-System/Services/https: Kubernetes-Dashboard:/proxy 
3. İstemci sertifikasını seçin.
4. Belirteci girin. 
5. Ana düğümdeki Bash komut satırına yeniden bağlanın ve `kubernetes-dashboard` ' a izin verin. Şu komutu çalıştırın:

    ```Bash  
    kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard 
    ``` 

    Betik, `kubernetes-dashboard` bulut Yöneticisi ayrıcalıkları verir. Daha fazla bilgi için bkz. [RBAC özellikli kümeler için](https://docs.microsoft.com/azure/aks/kubernetes-dashboard).

Panoyu kullanabilirsiniz. Kubernetes panosu hakkında daha fazla bilgi için bkz. [Kubernetes Web UI panosu](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) 

![Kubernetes panosunu Azure Stack](media/azure-stack-solution-template-kubernetes-dashboard/azure-stack-kub-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar 

[Kubernetes 'i Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)  

[Market 'e bir Kubernetes kümesi ekleme (Azure Stack işleci için)](../operator/azure-stack-solution-template-kubernetes-cluster-add.md)  

[Azure 'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)  
