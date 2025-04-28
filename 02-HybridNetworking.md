## Azure Gateway SKUs Comparison

| SKU               | Max S2S Tunnels | Max P2S Connections | Aggregate Throughput |
|--------------------|-----------------|---------------------|-----------------------|
| **Basic**         | 10              | 128                 | 100 Mbps             |
| **Standard**      | 10              | 128                 | 100 Mbps             |
| **HighPerformance**| 30             | 128                 | 1 Gbps               |
| **UltraPerformance**| 30            | Not Supported       | 10 Gbps              |
| **VpnGw1**        | 128             | 250                 | Up to 650 Mbps       |
| **VpnGw2**        | 128             | 500                 | Up to 1 Gbps         |
| **VpnGw3**        | 128             | 1000                | Up to 1.25 Gbps      |
| **VpnGw4**        | 128             | 5000                | Up to 5 Gbps         |
| **VpnGw5**        | 128             | 10000               | Up to 10 Gbps        |

For more details, refer to the official Azure documentation:  
[Azure VPN Gateway SKUs](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways#benchmark)

# Virtual WAN

## Pros

- Managed service automates connectivity to simplify management
- Go bigger
	- VPN x 10 connection per VPN Service
- Additional optional security features via Microsoft or 3rd Party (NVA) integration

## Cons

- Cost
- Planning to prevents unwanted connections
- Less control

## Why you still need VNets
- No DNS. Still need Azure DNS or other solution
- No DDoS protection must enable on IP or Vnet

## Creating a Virtual WAN

1. Create Virtual WAN
2. Create Virtual Hubs
3. Create VNets and peer to Virtual Hub(s)
4. Connect VPN (Basic and standard) or ExpressRoute (Req)
	- Basic: S2S VPN only
	- Standard: All VPN and ER
5. Peer with Remote Tenant (Cross tenant peering)
