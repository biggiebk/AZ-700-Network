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
