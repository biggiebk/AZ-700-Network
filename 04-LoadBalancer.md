# Load Balancer

https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview

- High availability
- Load sharing
- Performance
- Scalability

- Can use public or private frontend IP

## What LB Solution?

- Type of traffic Non-HTTP(S) or HTTPS
- Location of resources (regions) regional or global
- SLA
- Features and Functionality
- Limitations
- Cost

## Non-HTTP(s)

### Regional

Called a Public or Internal Load Balancer

- Transport Layer (OSI Layer 4)
- Public or Internal (Private IP)
- TCP/UDP
- IaaS
- Backend Pool: VMs and VM Scale Sets (VMSS)
- SKUs:
	- Basic Retires 09/30/2025
		- Users basic IP
		- No Zone Redundancy
		- Open by default
		- Dynamic and static IP
		- VM or VMSS
		- Up to 300 instances for backend pool
		- Health probes HTTP(?) and TCP
		- Inbound only
		- Cheap
		- No SLA
	- Standard
		- SLA 99.99% for the LB only
			- Service depends on your backend configuration
		- Standard IP
		- Static IP
		- Standard by default with NSG
		- Inbound and outbound connectivity
		- VM and VMSS (mixed)
		- Up to 5,000 instance per backend pool
		- Health probes HTTP(S) and TCP
		- Increase cost
		- Zone redundancy - availability
			- None: same as basic or azure does it?
			- Zone redundant: deployed across availability zones (azure managed)
				- less cost
			- Zonal: you pick which zones it is deployed in (IP per zone)
				- costs more than zone redundant
	- Gateway Load Balancer: Load balances NVA
		- Frontend IP
		- Load-balancing rules
		- Tunnel interfaces
		- Chain
		- SKU is the size of the VM used for the NVA

Traffic rules map the frontend ip to backend pool and health probe. Along with session persistence (Check up on this)

#### Outbound

- Can use Specific IP's or IP Prefix
- Masks the backend pool
- SNAT
- Public IP's only
- Replaces the NAT Gateway

### Global

Named Traffic Manager

- SLA 100% (99.99%)
- High performance or low latency
- compliance (data sovereignty)
- Hybrid applications
- Complex deployments
- DNS based
- Makes decisions based on rules to determine which endpoint to direct the calling actor
	- Endpoints are around the world
		- Azure endpoints
		- External endpoints
			- Can be an FQDN
	- 6 Routing methods
		- Priority: Lowest available priority number wins
		- Weighted: Determine endpoint based on a weight score
		- Performance: Lowest latency based on the DNS query source IP
		- Geographic: Routing based on the location of the DNS query source IP
		- Multi-value:
		- Subnet: 
- Uses Health Probes TCP and HTTP(S)
- Uses trafficmanager.net domain

## HTTP(S)

### Regional

Application Gateway

- Public or private
- External or Internal
- Health probes
- Layer 7 
- Can read HTTP headers to make decisions
- HTTP(s)
- HTTP2
- Web Sockets
- Endpoints
	- VM
	- VMSS
	- App Services
	- IP Addresses
	- Hostnames
	- On-prem/external app servers
- Endpoint Routing
	- Path based (URL)
	- Multi-site (FQDN)
- TLS Termination
- Rewrite policies
- Optionally enable WAF
- SKUs
	- Version 1: scales manually (small, medium, & large)
	- Version 2
		- Auto scales
		- Fixed size
	- New Version 2
		- Basic (Preview)
			- SLA 99.9
			- Max conn 200
			- Does not include advance traffic management features (?)
		- Standard version 2
			- SLA 99.95
			- 62,500 conn
			- Auto scales
	- HTTP/HTTPS listeners
		- Port
		- Certificate
		- Custom error pages
	- Rules
		- Health Probe
		- Backend pool
		- HTTP Setting
	- Redirection ?
	- Rule Types ?
		- Basic
		- Path
	-	Rewrite polices
		- condition
		- type
			- Request header
			- Response header
			- Re-eval path map
	

### Global

Front Door

- Entry point that uses the Microsoft global edge network
- Layer 7 or Split TCP based anycast
- Optional WAF
- External or Internal (premium SKU -> integration with private link)
- Health problem monitoring
- URL-path based routing for requests
- Enables hosting of multiple websites
- Scalable
- CDN integration
- URL path based routing
- Endpoint
	- Internet facing (in or outside Azure)
- SSL/TLS offload
	- Can re-encrypt
- Domain & cert management
- Additional security features
- SKU
	- Standard
		- ?
	- Premium
		- ?
- SLA 99.99% (with some regional requirements)
