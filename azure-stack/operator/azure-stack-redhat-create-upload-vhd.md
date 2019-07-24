---
title: Azure Stack kullanım için Red Hat Enterprise Linux VHD oluşturun ve yükleyin | Microsoft Docs
description: Red Hat Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: jeffgo
ms.lastreviewed: 08/15/2018
ms.openlocfilehash: 52ca03b01e762f980cbabcbe63d718bfd4c2c152
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418187"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure-stack"></a>Azure Stack için Red Hat tabanlı bir sanal makine hazırlama

Bu makalede, Azure Stack kullanım için bir Red Hat Enterprise Linux (RHEL) sanal makinesinin nasıl hazırlanacağını öğreneceksiniz. Bu makalede ele alınan RHEL 'nin sürümleri 7.1 + ' dir. Bu makalede ele alınan hazırlıklar için hiper yönetici, Hyper-V, çekirdek tabanlı sanal makine (KVM) ve VMware ' dir.

Red Hat Enterprise Linux destek bilgileri için [Red Hat ve Azure Stack bakın: Sık sorulan sorular](https://access.redhat.com/articles/3413531).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi 'nden Red Hat tabanlı bir sanal makine hazırlama

Bu bölümde, Red Hat Web sitesinden bir ISO dosyanızın olduğunu ve RHEL görüntüsünü bir sanal sabit diske (VHD) yüklediğinizi varsaymış olursunuz. Hyper-V Yöneticisi 'nin bir işletim sistemi görüntüsünü yüklemek için nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Hyper-v rolünü yüklemek ve bir sanal makineyi yapılandırmak](https://technet.microsoft.com/library/hh846766.aspx).

### <a name="rhel-installation-notes"></a>RHEL yükleme notları

* Azure Stack VHDX biçimini desteklemiyor. Azure yalnızca sabit VHD 'YI destekler. Hyper-V Yöneticisi 'Ni kullanarak diski VHD biçimine dönüştürebilir veya Convert-VHD cmdlet 'ini kullanabilirsiniz. VirtualBox kullanırsanız, diski oluştururken varsayılan dinamik olarak ayrılan seçeneğe karşılık olarak **sabit boyut** ' u seçin.
* Azure Stack yalnızca 1. nesil sanal makineleri destekler. 1\. nesil bir sanal makineyi VHDX 'ten VHD dosya biçimine dönüştürebilir ve dinamik olarak genişleterek sabit boyutlu bir diske taşıyabilirsiniz. Bir sanal makinenin neslini değiştiremezsiniz. Daha fazla bilgi için bkz. [Hyper-V ' d a 1. nesil veya 2 sanal makine oluşturmalıyım?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* VHD için izin verilen en büyük boyut 1.023 GB 'dir.
* Linux işletim sistemini yüklediğinizde, çoğu yükleme için genellikle varsayılan olan mantıksal birim Yöneticisi (LVM) yerine standart bölümler kullanmanızı öneririz. Bu yöntem, özellikle de sorun giderme için bir işletim sistemi diskini başka bir özdeş sanal makineye iliştirmeniz gerekiyorsa, bu, LVM adının kopyalanmış sanal makinelerle çakışmasını önler.
* Evrensel Disk Biçimi (UDF) dosya sistemlerini bağlamak için çekirdek desteği gereklidir. İlk önyüklemede, konuğa bağlı olan UDF biçimli medya, sağlama yapılandırmasını Linux sanal makinesine geçirir. Azure Linux Aracısı, yapılandırmasını okumak ve sanal makineyi sağlamak için UDF dosya sistemini takmalıdır.
* İşletim sistemi diskinde bir takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diskinde bir takas dosyası oluşturmak için yapılandırılabilir. Aşağıdaki adımlarda hakkında daha fazla bilgi bulabilirsiniz.
* Azure 'daki tüm VHD 'Lerin 1 MB 'a hizalanmış bir sanal boyutu olmalıdır. Bir ham diskten VHD 'ye dönüştürme sırasında, dönüştürmeden önce ham disk boyutunun 1 MB 'ın katı olduğundan emin olmanız gerekir. Aşağıdaki adımlarda daha fazla ayrıntı bulabilirsiniz.
* Azure Stack, Cloud-init ' i desteklemez. SANAL makinenizin Windows Azure Linux aracısının (WALA) desteklenen bir sürümüyle yapılandırılması gerekir.

### <a name="prepare-an-rhel-7-virtual-machine-from-hyper-v-manager"></a>Hyper-V Yöneticisi 'nden bir RHEL 7 sanal makinesi hazırlama

1. Hyper-V Yöneticisi 'nde sanal makineyi seçin.

1. **Bağlan** ' a tıklayarak sanal makine için bir konsol penceresi açın.

1. `/etc/sysconfig/network` Dosyayı oluşturun veya düzenleyin ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. `/etc/sysconfig/network-scripts/ifcfg-eth0` Dosyayı oluşturun veya düzenleyin ve gerekirse aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Aşağıdaki komutu çalıştırarak ağ hizmetinin önyükleme zamanında başlamasını sağlayın:

    ```bash
    sudo systemctl enable network
    ```

1. Aşağıdaki komutu çalıştırarak, RHEL deposundan paketlerin yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Grub yapılandırmanızda çekirdek önyükleme satırını, Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu değişikliği yapmak için bir metin `/etc/default/grub` düzenleyicisinde açın ve `GRUB_CMDLINE_LINUX` parametresini değiştirin. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini sağlar ve bu da hata ayıklama sorunlarını gidermek için Azure desteğine yardımcı olabilir. Bu yapılandırma, NIC 'ler için yeni RHEL 7 adlandırma kurallarını da kapatır.

   Grafik ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istiyoruz bir bulut ortamında yararlı değildir. İsterseniz, `crashkernel` seçeneğini yapılandırılmış olarak bırakabilirsiniz. Bu parametrenin, sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bu, daha küçük sanal makine boyutlarında sorunlu olabileceğini unutmayın. Aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Düzenlemesini `/etc/default/grub`tamamladıktan sonra, grub yapılandırmasını yeniden derlemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Bulutu Durdur ve Kaldır-init:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusunun yüklü olduğundan ve önyükleme zamanında başlayacak şekilde yapılandırıldığından emin olun, genellikle varsayılan değer olan. Aşağıdaki `/etc/ssh/sshd_config` satırı içerecek şekilde değiştirin:

    ```sh
    ClientAliveInterval 180
    ```

1. Walınuxagent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna gönderildi. Aşağıdaki komutu çalıştırarak ek özellikler deposunu etkinleştirin:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Aşağıdaki komutu çalıştırarak Azure Linux aracısını yükler:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure üzerinde sağlandıktan sonra sanal makineye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak disk geçici bir disktir ve sanal makine sağlanmamışsa boşaltılır. Önceki adımda Azure Linux aracısını yükledikten sonra, aşağıdaki parametreleri `/etc/waagent.conf` uygun şekilde değiştirin:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Aboneliğin kaydını silmek istiyorsanız aşağıdaki komutu çalıştırın:

    ```bash
    sudo subscription-manager unregister
    ```

1. Kuruluş sertifika yetkilisi kullanılarak dağıtılan bir sistem kullanıyorsanız, RHEL sanal makinesi Azure Stack kök sertifikaya güvenmeyecektir. Bunu güvenilir kök depoya yerleştirmeniz gerekir. Bkz. [sunucuya güvenilen kök sertifikalar ekleme](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırmak ve Azure 'da sağlamak üzere hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Hyper-V Yöneticisi 'nde **eylem** > **Kapat** ' a tıklayın.

1. Hyper-V Yöneticisi "disk düzenleme" özelliğini veya Convert-VHD PowerShell komutunu kullanarak VHD 'yi sabit boyutlu bir VHD 'ye dönüştürün. Linux VHD 'niz artık Azure 'a yüklenmeye hazırdır.

## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>KVM 'den Red Hat tabanlı bir sanal makine hazırlama

1. Red Hat Web sitesinden RHEL 7 ' nin KVM görüntüsünü indirin. Bu yordam, örnek olarak RHEL 7 kullanır.

1. Bir kök parolası ayarlayın.

    Şifrelenmiş bir parola oluşturun ve komutun çıkışını kopyalayın:

    ```bash
    openssl passwd -1 changeme
    ```

   Guestbalık ile bir kök parola ayarlayın:

    ```sh
    guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Kök kullanıcının ikinci alanını "!!" olarak değiştirin şifrelenmiş parolaya.

1. QCOW2 görüntüsünden KVM 'de bir sanal makine oluşturun. Disk türünü **QCOW2**olarak ayarlayın ve sanal ağ arabirimi cihaz modelini **Virtio**olarak ayarlayın. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. `/etc/sysconfig/network` Dosyayı oluşturun veya düzenleyin ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. `/etc/sysconfig/network-scripts/ifcfg-eth0` Dosyayı oluşturun veya düzenleyin ve aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Aşağıdaki komutu çalıştırarak ağ hizmetinin önyükleme zamanında başlamasını sağlayın:

    ```bash
    sudo systemctl enable network
    ```

1. Aşağıdaki komutu çalıştırarak, RHEL deposundan paketlerin yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Grub yapılandırmanızda çekirdek önyükleme satırını, Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu yapılandırmayı yapmak için bir metin `/etc/default/grub` düzenleyicisinde açın ve `GRUB_CMDLINE_LINUX` parametresini değiştirin. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu komut, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarını gidermek için Azure desteğine yardımcı olabilir. Bu komut, NIC 'ler için yeni RHEL 7 adlandırma kurallarını da kapatır

   Grafik ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderildiği bir bulut ortamında yararlı değildir. İsterseniz, `crashkernel` seçeneğini yapılandırılmış olarak bırakabilirsiniz. Bu parametre, sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azaltır ve bu da daha küçük sanal makine boyutlarında sorunlu olabilir. Aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

1. Düzenlemesini `/etc/default/grub`tamamladıktan sonra, grub yapılandırmasını yeniden derlemek için aşağıdaki komutu çalıştırın:

    ```bash
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Hyper-V modüllerini initramfs içine ekleyin.

    İçeriği `/etc/dracut.conf` düzenleyin ve ekleyin:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Initramfs yeniden derle:

    ```bash
    dracut -f -v
    ```

1. Bulutu Durdur ve Kaldır-init:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusunun önyükleme sırasında başlatılacak şekilde yüklendiğinden ve yapılandırıldığından emin olun:

    ```bash
    systemctl enable sshd
    ```

    /Etc/ssh/sshd_config öğesini aşağıdaki satırları içerecek şekilde değiştirin:

    ```sh
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. Azure Stack için özel bir VHD oluştururken, 2.2.20 ve 2.2.35 arasında (her ikisi de hariç) Walınuxagent sürümünün Azure Stack ortamlarında çalışmadığını aklınızda bulundurun. Görüntünüzü hazırlamak için 2.2.20/2.2.35 Versions sürümlerini kullanabilirsiniz. 2\.2.35 üzerindeki sürümleri kullanarak özel görüntünüzü hazırlayın, Azure Stack 1903 sürümüne güncelleştirin veya 1901/1902 düzeltmesini uygulayın. 

     Walınuxagent 'ı indirmek için şu yönergeleri izleyin:
    
   a.   Setuptools 'ı indir
    ```bash
    wget https://pypi.python.org/packages/source/s/setuptools/setuptools-7.0.tar.gz --no-check-certificate
    tar xzf setuptools-7.0.tar.gz
    cd setuptools-7.0
    ```
   b. Bu, GitHub deposundan "2.2.20" sürümünü indirdiğimiz bir örnektir. GitHub 'umuza aracının 2.2.20 sürümünü indirip sıkıştırmasını açın. 
    ```bash
    wget https://github.com/Azure/WALinuxAgent/archive/v2.2.20.zip
    unzip v2.2.20.zip
    cd WALinuxAgent-2.2.20
    ```
    c. Setup.py 'i yükler
    ```bash
    sudo python setup.py install
    ```
    d. Waagent 'ı yeniden Başlat
    ```bash
    sudo systemctl restart waagent
    ```
    e. Aracı sürümü indirmiş bir sürümle eşleşiyorsa test edin. Bu örnekte, 2.2.20 olmalıdır.
    
    ```bash
    waagent -version
    ```

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure üzerinde sağlandıktan sonra sanal makineye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak disk geçici bir disktir ve sanal makine sağlanmamışsa boşaltılır. Önceki adımda Azure Linux aracısını yükledikten sonra, aşağıdaki parametreleri `/etc/waagent.conf` uygun şekilde değiştirin:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    #NOTE: set this to whatever you need it to be.
    ```

1. Aşağıdaki komutu çalıştırarak aboneliğin kaydını silin (gerekiyorsa):

    ```bash
    subscription-manager unregister
    ```

1. Kuruluş sertifika yetkilisi kullanılarak dağıtılan bir sistem kullanıyorsanız, RHEL sanal makinesi Azure Stack kök sertifikaya güvenmeyecektir. Bunu güvenilir kök depoya yerleştirmeniz gerekir. Bkz. [sunucuya güvenilen kök sertifikalar ekleme](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırmak ve Azure 'da sağlamak üzere hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Sanal makineyi KVM 'de kapatın.

1. QCOW2 görüntüsünü VHD biçimine dönüştürün.

    > [!NOTE]
    > QEMU-img sürümlerindeki bilinen bir hata vardır > = 2.2.1, hatalı biçimli bir VHD ile sonuçlanır. Bu sorun QEMU 2,6 ' de düzeltildi. QEMU-img 2.2.0 veya Lower ya da 2,6 veya üzeri bir sürümü kullanmanız önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.

    Önce görüntüyü ham biçime dönüştürün:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Ham görüntünün boyutunun 1 MB ile hizalandığından emin olun. Aksi takdirde, 1 MB ile hizalamak için boyutu yuvarlayın:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Ham diski sabit boyutlu bir VHD 'ye Dönüştür:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Ya da QEMU sürüm **2.6 +** ile `force_size` seçeneği vardır:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>VMware 'den Red Hat tabanlı bir sanal makine hazırlama

Bu bölümde, VMware 'de zaten bir RHEL sanal makinesini yüklemiş olduğunuz varsayılmaktadır. VMware 'de bir işletim sisteminin nasıl yükleneceğine ilişkin ayrıntılar için bkz. [VMware Konuk Işletim sistemi yükleme kılavuzu](https://partnerweb.vmware.com/GOSIG/home.html).

* Linux işletim sistemini yüklediğinizde, genellikle çoğu yükleme için varsayılan olan LVM yerine standart bölümler kullanmanızı öneririz. Bu, özellikle de bir işletim sistemi diskinin sorun gidermeye yönelik başka bir sanal makineye eklenmesi gerekiyorsa, bu, LVM adının klonlanmış sanal makineyle çakışıp çakışmadığını önler. Tercih edilen durumlarda LVM veya RAID, veri disklerinde kullanılabilir.
* İşletim sistemi diskinde bir takas bölümü yapılandırmayın. Linux aracısını, geçici kaynak diskinde bir takas dosyası oluşturacak şekilde yapılandırabilirsiniz. Bunu izleyen adımlarda hakkında daha fazla bilgi edinebilirsiniz.
* Sanal sabit diski oluştururken, **sanal diski tek bir dosya olarak depola**' yı seçin.

### <a name="prepare-an-rhel-7-virtual-machine-from-vmware"></a>VMware 'den bir RHEL 7 sanal makinesi hazırlama

1. `/etc/sysconfig/network` Dosyayı oluşturun veya düzenleyin ve aşağıdaki metni ekleyin:

    ```sh
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. `/etc/sysconfig/network-scripts/ifcfg-eth0` Dosyayı oluşturun veya düzenleyin ve aşağıdaki metni ekleyin:

    ```sh
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

1. Aşağıdaki komutu çalıştırarak ağ hizmetinin önyükleme zamanında başlamasını sağlayın:

    ```bash
    sudo chkconfig network on
    ```

1. Aşağıdaki komutu çalıştırarak, RHEL deposundan paketlerin yüklenmesini etkinleştirmek için Red Hat aboneliğinizi kaydedin:

    ```bash
    sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Grub yapılandırmanızda çekirdek önyükleme satırını, Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu değişikliği yapmak için bir metin `/etc/default/grub` düzenleyicisinde açın ve `GRUB_CMDLINE_LINUX` parametresini değiştirin. Örneğin:

    ```sh
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

    Bu yapılandırma aynı zamanda tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarını gidermek için Azure desteğine yardımcı olabilir. Ayrıca, NIC 'ler için yeni RHEL 7 adlandırma kurallarını kapatır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:

    ```sh
    rhgb quiet crashkernel=auto
    ```

    Grafik ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istiyoruz bir bulut ortamında yararlı değildir. İsterseniz, `crashkernel` seçeneğini yapılandırılmış olarak bırakabilirsiniz. Bu parametrenin, sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bu, daha küçük sanal makine boyutlarında sorunlu olabileceğini unutmayın.

1. Düzenlemesini `/etc/default/grub`tamamladıktan sonra, grub yapılandırmasını yeniden derlemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Initramfs 'ye Hyper-V modülleri ekleyin.

    Düzenleme `/etc/dracut.conf`, içerik ekleme:

    ```sh
    add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
    ```

    Initramfs yeniden derle:

    ```bash
    dracut -f -v
    ```

1. Bulutu Durdur ve Kaldır-init:

    ```bash
    systemctl stop cloud-init
    yum remove cloud-init
    ```

1. SSH sunucusunun, önyükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun. Bu ayar genellikle varsayılandır. Aşağıdaki `/etc/ssh/sshd_config` satırı içerecek şekilde değiştirin:

    ```sh
    ClientAliveInterval 180
    ```

1. Walınuxagent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna gönderildi. Aşağıdaki komutu çalıştırarak ek özellikler deposunu etkinleştirin:

    ```bash
    subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Aşağıdaki komutu çalıştırarak Azure Linux aracısını yükler:

    ```bash
    sudo yum install WALinuxAgent
    sudo systemctl enable waagent.service
    ```

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure üzerinde sağlandıktan sonra sanal makineye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine sağlanmamışsa boşaltılıp boşaltıyacağını unutmayın. Önceki adımda Azure Linux aracısını yükledikten sonra, aşağıdaki parametreleri `/etc/waagent.conf` uygun şekilde değiştirin:

    ```sh
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    NOTE: set this to whatever you need it to be.
    ```

1. Aboneliğin kaydını silmek istiyorsanız aşağıdaki komutu çalıştırın:

    ```bash
    sudo subscription-manager unregister
    ```

1. Kuruluş sertifika yetkilisi kullanılarak dağıtılan bir sistem kullanıyorsanız, RHEL sanal makinesi Azure Stack kök sertifikaya güvenmeyecektir. Bunu güvenilir kök depoya yerleştirmeniz gerekir. Bkz. [sunucuya güvenilen kök sertifikalar ekleme](https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html).

1. Sanal makinenin sağlamasını kaldırmak ve Azure 'da sağlamak üzere hazırlamak için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

1. Sanal makineyi kapatın ve VMDK dosyasını VHD biçimine dönüştürün.

    > [!NOTE]
    > QEMU-img sürümlerindeki bilinen bir hata vardır > = 2.2.1, hatalı biçimli bir VHD ile sonuçlanır. Bu sorun QEMU 2,6 ' de düzeltildi. QEMU-img 2.2.0 veya Lower ya da 2,6 veya üzeri bir sürümü kullanmanız önerilir. Başvuru: <https://bugs.launchpad.net/qemu/+bug/1490611>.

    Önce görüntüyü ham biçime dönüştürün:

    ```bash
    qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Ham görüntünün boyutunun 1 MB ile hizalandığından emin olun. Aksi takdirde, 1 MB ile hizalamak için boyutu yuvarlayın:

    ```bash
    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
    rounded_size=$((($size/$MB + 1)*$MB))
    qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Ham diski sabit boyutlu bir VHD 'ye Dönüştür:

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    Ya da QEMU sürüm **2.6 +** ile `force_size` seçeneği vardır:

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Bir kickstart dosyasını otomatik olarak kullanarak bir ISO 'dan Red Hat tabanlı bir sanal makine hazırlama

1. Aşağıdaki içeriği içeren bir kickstart dosyası oluşturun ve dosyayı kaydedin. Kickstart yüklemesi hakkında daha fazla bilgi için, bkz. [kickstart Yükleme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

    ```sh
    Kickstart for provisioning a RHEL 7 Azure VM

    System authorization information
    auth --enableshadow --passalgo=sha512

    Use graphical install
    text

    Do not run the Setup Agent on first boot
    firstboot --disable

    Keyboard layouts
    keyboard --vckeymap=us --xlayouts='us'

    System language
    lang en_US.UTF-8

    Network information
    network  --bootproto=dhcp

    Root password
    rootpw --plaintext "to_be_disabled"

    System services
    services --enabled="sshd,waagent,NetworkManager"

    System timezone
    timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

    Partition clearing information
    clearpart --all --initlabel

    Clear the MBR
    zerombr

    Disk partitioning information
    part /boot --fstype="xfs" --size=500
    part / --fstyp="xfs" --size=1 --grow --asprimary

    System bootloader configuration
    bootloader --location=mbr

    Firewall configuration
    firewall --disabled

    Enable SELinux
    selinux --enforcing

    Don't configure X
    skipx

    Power down the machine after install
    poweroff

    %packages
    @base
    @console-internet
    chrony
    sudo
    parted
    -dracut-config-rescue

    %end

    %post --log=/var/log/anaconda/post-install.log

    #!/bin/bash

    Register Red Hat Subscription
    subscription-manager register --username=XXX --password=XXX --auto-attach --force

    Install latest repo update
    yum update -y

    Stop and Uninstall cloud-init
    systemctl stop cloud-init
    yum remove cloud-init
    
    Enable extras repo
    subscription-manager repos --enable=rhel-7-server-extras-rpms

    Install WALinuxAgent
    yum install -y WALinuxAgent

    Unregister Red Hat subscription
    subscription-manager unregister

    Enable waaagent at boot-up
    systemctl enable waagent

    Disable the root account
    usermod root -p '!!'

    Configure swap in WALinuxAgent
    sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
    sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

    Set the cmdline
    sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

    Enable SSH keepalive
    sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

    Build the grub cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg

    Configure network
    cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    EOF

    Deprovision and prepare for Azure
    waagent -force -deprovision

    %end
    ```

1. Yükleme sisteminin erişebileceği kickstart dosyasını yerleştirin.

1. Hyper-V Yöneticisi 'nde yeni bir sanal makine oluşturun. **Sanal sabit diski bağla** sayfasında, **daha sonra bir sanal sabit disk Ekle**' yi seçin ve yeni sanal makine Sihirbazı ' nı doldurun.

1. Sanal makine ayarlarını açın:

    a. Sanal makineye yeni bir sanal sabit disk ekleyin. **VHD biçimini** ve **sabit boyutu**seçtiğinizden emin olun.

    b. Yükleme ISO dosyasını DVD sürücüsüne ekleyin.

    c. BIOS 'u CD 'den önyükleme olacak şekilde ayarlayın.

1. Sanal makineyi başlatın. Yükleme Kılavuzu göründüğünde, önyükleme seçeneklerini yapılandırmak için **Tab** tuşuna basın.

1. Önyükleme `inst.ks=<the location of the kickstart file>` seçeneklerinin sonuna yazın ve **ENTER**tuşuna basın.

1. Yüklemenin bitmesini bekleyin. İşiniz bittiğinde, sanal makine otomatik olarak kapatılır. Linux VHD 'niz artık Azure 'a yüklenmeye hazırdır.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V sürücüsü, Hyper-V olmayan bir hiper yönetici kullanılırken ilk RAM diskine dahil edilemedi

Bazı durumlarda Linux yükleyicileri, Linux 'un Hyper-V ortamında çalıştığını algılayamadığı takdirde, ilk RAM diskindeki (ınitrd veya ınitramfs) Hyper-V sürücülerini içermeyebilir.

Linux görüntünüzü hazırlamak için farklı bir sanallaştırma sistemi (yani, Oracle VM VirtualBox, Xen projesi vb.) kullanırken, ilk RAM üzerinde en az hv_vmbus ve hv_storvsc çekirdek modüllerinin kullanılabilir olmasını sağlamak için ınitrd 'i yeniden oluşturmanız gerekebilir dis. Bu, en azından yukarı akış Red Hat dağıtımına dayalı olan sistemlerde bilinen bir sorundur.

Bu sorunu çözmek için, ınitramfs 'ye Hyper-V modülleri ekleyin ve yeniden oluşturun:

Aşağıdaki `/etc/dracut.conf`içeriği düzenleyin ve ekleyin:

```sh
add_drivers+="hv_vmbus hv_netvsc hv_storvsc"
```

Initramfs yeniden derle:

```bash
dracut -f -v
```

Daha fazla bilgi için bkz. [ınitramfs](https://access.redhat.com/solutions/1958)'yi yeniden oluşturma.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Stack yeni sanal makineler oluşturmak için Red Hat Enterprise Linux sanal sabit diskinizi kullanmaya hazırsınız. VHD dosyasını ilk kez Azure Stack karşıya yüklüyorsanız, bkz. [Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md).

Red Hat Enterprise Linux çalıştırılmak üzere sertifikalı hiper yöneticiler hakkında daha fazla bilgi için [Red Hat Web sitesine](https://access.redhat.com/certified-hypervisors)bakın.
