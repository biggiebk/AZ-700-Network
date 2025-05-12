# ExpressRoute

Can you make recommendations for adding missing detailed information related to Azure ExpressRoute for the AZ-700 exam including step to create resources and make those updates. Include source links. Add any SKU information related to resources in a detailed table comparing each SKU. Include any information related limits. Add a table of contents and a links section that includes FAQ.

Can you make recommendations for adding missing detailed information related to Azure Network Security for the AZ-700 exam including step to create resources and make those updates. Include source links. Add any SKU information related to resources in a detailed table comparing each SKU. Include any information related limits to resources related to subscription, service, quotas, and constraints. Add a table of contents and a links section that includes FAQ.

## Links

ExpressRoute locations: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations
Providers: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-locations#partners
AzureCT (connectivity toolkit) - https://github.com/Azure/NetworkMonitoring/blob/main/AzureCT/AvailabilityTesting.md

## Pros

	- Private Connection
	- Layer 3
	- Dual channel
	- High Bandwidth up to 100Gbps
	- 3rd party or Microsoft (Direct)
	- Global Reach & Fast path
	- Metered/Unlimited
	- Can do VPN over ExpressRoute
	- Can coexist with a VPN
	- SLA over the entire connections 99.95%
	- Bidirectional Forwarding Detection (BFD) with ExpressRoute Peering
		- BFD is configured by default on MS edge
		- You only need to configure BFD on both your primary and secondary devices (on-prem)
		- You configure the BFD on the interface adn then link it to the BGP session.

## Cons

	- Management
	- Cost!

## Capabilities

- Layer 3 connectivity and redundancy
- Connectivity to all regions within a geography
- Global connectivity with ExpressRoute premium add-on
- Across on-premises connectivity with ExpressRoute Global Reach
- Bandwidth options - 50Mbps to 100Gbps
- Billing Modules - Unlimited or metered

## Benefits

- Predictable, reliable, and high-throughput connections
- Up to 100 Gbps bandwidth - supports dynamic scaling of bandwidth and direct access to national clouds
- built in redundant circuits
- Integrates with existing Multiprotocol Label Switching (MPLS)
- Up to 99.95 availability SLA across the entire connection

## Challenges

- Working with a third-party connectivity provider
- Requires high-bandwidth routers on-premises

## Connectivity Models

### Service providers

- CloudExchange Colocation Providers: These providers allow you to connect to the Microsoft cloud through virtual cross-connections at colocation facilities. They offer Layer 2 or managed Layer 3 cross-connections.
- Point-to-Point Ethernet Providers: These providers establish direct Ethernet links between your on-premises data centers or offices and the Microsoft cloud. They typically offer Layer 2 connections.
- Any-to-Any (IPVPN) Providers: These providers integrate your Wide Area Network (WAN) with the Microsoft cloud, making it appear as an extension of your network. They usually offer managed Layer 3 connectivity, often through MPLS VPN.
- ExpressRoute Direct: This option allows you to connect directly to Microsoft's global network at peering locations, offering high-capacity connections (10 Gbps or 100 Gbps) for large-scale deployments.

#### Design

- Users service providers to enable fast onboarding and connectivity into existing infra
- Integrates with hundreds of providers
- Circuits SKUs form 50Mbps to 10 Gbps
- 

### Direct module

- Direct Module

#### Design

- 

## How to Configure Encryption

- Establish ExpressRoute connectivity with an ExpressRoute circuit and private peering
- Establish the VPN connectivity over ExpressRoute
- Routing between the on-prem networks and AZ over both the ExpressRoute and VPN Paths

## Coexisting S2S and ExpressRoute

- Use S2S VPN as a secure failover path for ExpressRoute
- Use S2S VPNs to connect to sites that are not connected with ExpressRoute
- Needs two VNet gateways for the same VNet
	- ExpressRoute Gateway and VPN Gateway

## SKUs

- Local (if avail) - provides free egress data transfer and gives you access ton only 1-2 AZ regions in the same area as your circuit
- Standard - Give you access to all AZ regions in a geopolitical area
- Premium - provides support for more than 4k routes, ability to connect to to more than 10 VNets, and global connectivity

## ExpressRoute Circuit Type

- Choose Metered or unlimited data plan
- Choose bandwidth
- You can increase the gateway size but not the decrease without service outage
- Pricing varies by region and zone

## Billing Model
- Unlimited data. BIlling is based on a fixed monthly fee, all inbound and outbound data transfer is included free of charge.
- Metered data. BIlling is based on monthly usage; all inbound data transfer is free of charge. Outbound data transfer is charged per GB of data transfer. Data transfer rates vary by region
- ExpressRoute premium add-on. ExpressRoute premium is an add-on to the ExpressRoute circuit

## Steps to Create ExpressRoute

1. Virtual network gateway (GW Type ExpressRoute)
	- To add or remove High Availability Zone support you have to recreate
2. Create ExpressRoute Circuit
	- Resiliency type
	- Need to initiate the provisioning process with your service provider or will be in Not provisioned in provider status.
		- Service key
	- Can change
		- increase/decrease bandwidth
		- SKU
		- Billing model
3. Create connection to VNet Gateway and ExpressRoute (requires provisioning)
	- Resiliency type
4. Set Peering
	- Azure Private - Pair of subnets that are not part of any addr space reserved for VNet. One subnet will be used for the primary link, while the other is secondary. (IaaS)
	- Azure Public - 
	- Microsoft - 365 products and public services (PaaS)
5. Route filters (optional, but required if both private/public and Microsoft)
	- Must be in the same location as the ER it is associated with
	- Service communities (regions, services, etc.)
6. Connect VNets
	- ERGW? Peer type

## Cross-region connectivity to link multiple ER

- USer Cross-region connectivity to link multiple ER
- Connectivity to all regions within a geopolitical region
- Global connectivity with ER Premium
- Local connectivity with ER Local
- Across on-prem connectivity with ER Global Reach to (Connect your DC's via ER)
- ER Direct

## FastPath

- Designed to improve the data path performance between your on-prem network and your VNet
- When enabled, FastPath sends network traffic directly to VMs in the VNet, bypassing the gateway
- Improves data path performance such as packets per second and connections per second between your on-prem network and your VNet
- Can enable ER FastPath if your VNet gateway is Ultra Performance or **ERGw3AZ**

## Troubleshoot

- Verify Circuit provisioning and state
	- Circuit State (enabled) and Provider Status (provisioned in portal or succeeded in powershell/cli)
- Check peering status
- Reset a failed circuit
- Validate ARP
	- Getting ARP tables in the Resource Manager deployment model – https://learn.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager
	- How to use this information – https://learn.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager#how-to-use-this-information
- MOst network issues can be analyzed an isoloated with Azure NetworkWatcher or Powershell/CLI
- The Azure Connectivity TOolkit (AzureCT) was developed to put tools in one package
