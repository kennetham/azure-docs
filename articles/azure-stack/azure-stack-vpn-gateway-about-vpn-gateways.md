---
title: About VPN gateway for Azure Stack | Microsoft Docs
description: Learn about and configure VPN gateways you use with Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''

ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
---

# About VPN gateway for Azure Stack
*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*


Before you can send network traffic between your Azure virtual network and your on-premises site, you must create a virtual network gateway for your virtual network.

A VPN gateway is a type of virtual network gateway that sends encrypted traffic across a public connection. You can use VPN gateways to send traffic securely between a virtual network in Azure Stack and a virtual network in Azure. YOu can also send traffic securely between a virtual network and another network that is connected to a VPN device.

When you create a virtual network gateway, you specify the gateway type that you want to create. Azure Stack supports one type of virtual network gateway: the 'Vpn' type.

Each virtual network can have two virtual network gateways, but only one of each type. Depending on the settings that you choose, you can create multiple connections to a single VPN gateway. An example is a Multi-Site connection configuration.

> [!NOTE]
> In Azure, the bandwidth throughput for VPN Gateway SKU you choose must be divided across all the Connections that are connected to it.  In Azure Stack, the bandwidth value for the VPN Gateway SKU is applied to each Connection resource that is connected to it.     

> For example, In Azure, the Basic VPN Gateway SKU can accommodate approximately 100 Mbps of aggregate throughput.  If you create two Connections to that VPN Gateway, and one connection is using 50 Mbps of bandwidth, then 50 Mbps is available to the other Connection.   

> In Azure Stack, *each* Connection to the Basic VPN Gateway SKU gets allocated 100 Mbps of throughput.

## Configuring a VPN Gateway
A VPN gateway connection relies on multiple resources that are configured with specific settings. Most of the resources can be configured separately, although they must be configured in a certain order in some cases.

### Settings
The settings that you chose for each resource are critical to creating a successful connection. For information about individual resources and settings for VPN Gateway, see [About VPN gateway settings for Azure Stack](azure-stack-vpn-gateway-settings.md). You can find information to help you understand gateway types, VPN types, connection types, gateway subnets, local network gateways, and various other resource settings that you may want to consider.

### Deployment tools
You can create and configure resources using one configuration tool, such as the Azure portal. Later you might switch to another tool like PowerShell to configure additional resources or modify existing resources when applicable. Currently, you can't configure every resource and resource setting in the Azure portal. The instructions in the articles for each connection topology specify when a specific configuration tool is needed.

## Connection topology diagrams
It's important to know that there are different configurations available for VPN gateway connections. Determine which configuration best fits your needs. In the sections below, you can view information and topology diagrams about the following VPN gateway connections: The following sections contain tables which list:

- Available deployment model
- Available configuration tools
- Links that take you directly to an article, if available

The diagrams and descriptions in the following sections can help you select a connection topology to match your requirements. The diagrams show the main baseline topologies, but it's possible to build more complex configurations using the diagrams as a guideline.

## Site-to-Site and Multi-Site (IPsec/IKE VPN tunnel)
### Site-to-Site
A Site-to-Site (S2S) VPN gateway connection is a connection over IPsec/IKE (IKEv1 or IKEv2) VPN tunnel. This type of connection requires a VPN device located on-premises that has a public IP address assigned to it and is not located behind a NAT. S2S connections can be used for cross-premises and hybrid configurations.    
![Site-to-site VPN connection configuration example](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### Multi-Site
This type of connection is a variation of the Site-to-Site connection. You create more than one VPN connection from your virtual network gateway, typically connecting to multiple on-premises sites. When working with multiple connections, you must use a RouteBased VPN type (known as a dynamic gateway when working with classic VNets). Because each virtual network can only have one VPN gateway, all connections through the gateway share the available bandwidth. This is often called a "multi-site" connection.   
![Azure VPN Gateway Multi-Site connection example](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## Gateway SKUs
When you create a virtual network gateway for Azure Stack, you specify the gateway SKU that you want to use. The following VPN gateway SKUs are supported:
- Basic
- Standard
- HighPerformance

When you select a higher gateway SKU, like Standard over Basic, or HighPerformance over Standard or Basic, more CPUs and network bandwidth are allocated to the gateway. As a result, the gateway can support higher network throughput to the virtual network.

Azure Stack does not support the UltraPerformance gateway SKU, which is used exclusively with Express Route.

Consider the following when you select SKU:
- Azure Stack does not support Policy-based gateways.
- Border Gateway Protocol (BGP) is not supported on the Basic SKU.
- ExpressRoute-VPN Gateway coexist configurations are not supported in Azure Stack
- Active-active S2S VPN Gateway connections can be configured on the HighPerformance SKU only.

## Estimated aggregate throughput by SKU
The following table shows the gateway types and the estimated aggregate throughput by gateway SKU.

|	| VPN Gateway throughput *(1)* | VPN Gateway max IPsec tunnels *(2)* |
|-------|-------|-------|
|**Basic SKU** ***(3)*** 	| 100 Mbps	| 10	|
|**Standard SKU** 		| 100 Mbps 	| 10	|
|**High Performance SKU** | 200 Mbps	| 5	|
***(1)*** The VPN throughput is not a guaranteed throughput for cross-premises connections across the Internet. It is the maximum possible throughput measurement.  
***(2)*** Max tunnels number is the total per Azure Stack deployment for ALL subscriptions.  
***(3)*** BGP is not supported for the Basic SKU.

## Next steps
Learn about [settings for VPN gateways](azure-stack-vpn-gateway-settings.md) for Azure Stack.
