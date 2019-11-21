---
title: Azure Stack networking differences | Microsoft Docs
description: Learn about differences and considerations when working with networking in Azure Stack.
services: azure-stack
keywords: ''
author: mattbriggs
manager: femila
ms.date: 10/10/2019
ms.topic: article
ms.service: azure-stack
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 07/10/2019
ms.openlocfilehash: 2ba1fc76b8b0ead1da543abb467d7a1613a5304c
ms.sourcegitcommit: ac7d98a2b58442e82798022d69ebfae6616a225f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239334"
---
# <a name="differences-and-considerations-for-azure-stack-networking"></a>Differences and considerations for Azure Stack networking

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

Azure Stack networking has many of the features provided by Azure networking. However, there are some key differences that you should understand before deploying an Azure Stack network.

This article provides an overview of the unique considerations for Azure Stack networking and its features. To learn about high-level differences between Azure Stack and Azure, see the [Key considerations](azure-stack-considerations.md) article.

## <a name="cheat-sheet-networking-differences"></a>Cheat sheet: Networking differences

| Hizmet | Özellik | Azure (global) | Azure Stack |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS | Multi-tenant DNS | Desteklenen | Not yet supported |
|  | DNS AAAA records | Desteklenen | Desteklenmiyor |
|  | DNS zones per subscription | 100 (default)<br>Can be increased on request. | 100 |
|  | DNS record sets per zone | 5000 (default)<br>Can be increased on request. | 5000 |
|  | Name servers for zone delegation | Azure provides four name servers for each user (tenant) zone that is created. | Azure Stack provides two name servers for each user (tenant) zone that is created. |
| Azure Güvenlik Duvarı | Network security service | Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. | Not yet supported. |
| Sanal Ağ | Sanal ağ eşlemesi | Connect two virtual networks in the same region through the Azure backbone network. | Not yet supported |
|  | IPv6 addresses | You can assign an IPv6 address as part of the [Network Interface Configuration](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions). | Yalnızca IPv4 desteklenir. |
|  | DDoS Protection Plan | Desteklenen | Not yet supported. |
|  | Scale Set IP Configurations | Desteklenen | Not yet supported. |
|  | Private Access Services (Subnet) | Desteklenen | Not yet supported. |
|  | Hizmet Uç Noktaları | Supported for internal (non-Internet) connection to Azure Services. | Not yet supported. |
|  | Hizmet Uç Noktası İlkeleri | Desteklenen | Not yet supported. |
|  | Service Tunnels | Desteklenen | Not yet supported.  |
| Ağ Güvenlik Grupları | Augmented Security Rules | Desteklenen | Not yet supported. |
|  | Effective Security Rules | Desteklenen | Not yet supported. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Not yet supported. |
| Sanal Ağ Geçitleri | Point-to-Site VPN Gateway | Desteklenen | Not yet supported. |
|  | Vnet-to-Vnet Gateway | Desteklenen | Not yet supported. |
|  | Virtual Network Gateway Type | Azure Supports VPN<br> Express Route <br> Hyper Net. | Azure Stack currently supports only VPN type. |
|  | VPN Gateway SKUs | Support for Basic, GW1, GW2, GW3, Standard High Performance, Ultra-High Performance. | Support for Basic, Standard, and High-Performance SKUs. |
|  | VPN Type | Azure supports both policy-based and route-based. | Azure Stack supports route-based only. |
|  | BGP Settings | Azure supports configuration of BGP Peering Address and Peer Weight. | BGP Peering Address and Peer Weight are auto-configured in Azure Stack. There's no way for the user to configure these settings with their own values. |
|  | Default Gateway Site | Azure supports configuration of a default site for forced tunneling. | Not yet supported. |
|  | Gateway Resizing | Azure supports resizing the gateway after deployment. | Resizing not supported. |
|  | Availability Configuration | Active/Active | Active/Passive |
|  | UsePolicyBasedTrafficSelectors | Azure supports using policy-based traffic selectors with route-based gateway connections. | Not yet supported. |
| Yük dengeleyici | SKU | Basic and Standard Load Balancers are supported | Only the Basic Load Balancer is supported.<br>The SKU property is not supported.<br>The Basic SKU load balancer /path/ cannot have more than 5 front-end IP configurations.  |
|  | Bölgeler | Availability Zones are Supported. | Not yet supported |
|  | Inbound NAT Rules support for Service Endpoints | Azure supports specifying Service Endpoints for Inbound NAT rules. | Azure Stack doesn't yet support Service Endpoints, so these can't be specified. |
|  | Protokol | Azure Supports specifying GRE or ESP. | Protocol Class isn't supported in Azure Stack. |
| Genel IP Adresi | Public IP Address Version | Azure supports both IPv6 and IPv4. | Yalnızca IPv4 desteklenir. |
| | SKU | Azure supports Basic and Standard. | Only Basic is supported. |
| Ağ Arabirimi | Get Effective Route Table | Desteklenen | Not yet supported. |
|  | Get Effective ACLs | Desteklenen | Not yet supported. |
|  | Enable Accelerated Networking | Desteklenen | Not yet supported. |
|  | IP İletimi | Disabled by default.  Can be enabled. | Toggling this setting isn't supported.  On by default. |
|  | Uygulama Güvenlik Grupları | Desteklenen | Not yet supported. |
|  | Internal DNS Name Label | Desteklenen | Not yet supported. |
|  | Private IP Address Version | Both IPv6 and IPv4 are supported. | Yalnızca IPv4 desteklenir. |
|  | Static MAC Address | Desteklenmiyor | Desteklenmiyor. Each Azure Stack system uses the same MAC address pool. |
| Ağ İzleyicisi | Network Watcher tenant network monitoring capabilities | Desteklenen | Not yet supported. |
| CDN | Content Delivery Network profiles | Desteklenen | Not yet supported. |
| Uygulama ağ geçidi | Layer-7 load balancing | Desteklenen | Not yet supported. |
| Traffic Manager | Route incoming traffic for optimal application performance and reliability. | Desteklenen | Not yet supported. |
| Express Route | Set up a fast, private connection to Microsoft cloud services from your on-premises infrastructure or colocation facility. | Desteklenen | Support for connecting Azure Stack to an Express Route circuit. |

## <a name="api-versions"></a>API sürümleri 

Azure Stack Networking supports the following API versions: 

- 2018-11-01
- 2018-10-01
- 2018-08-01
- 2018-07-01
- 2018-06-01
- 2018-05-01
- 2018-04-01
- 2018-03-01
- 2018-02-01
- 2018-01-01
- 2017-11-01
- 2017-10-01

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’te DNS](azure-stack-dns.md)
