# Security

https://learn.microsoft.com/security/benchmark/azure/mcsb-network-security

SEIM - Security Event Management System  
UBA - User Behavior Analysis  

## Defense in Depth

1. Physical  
2. Identity & Access  
3. Perimeter  
4. Network  
5. Compute  
6. Application  
7. Data  

## Defender for Cloud

https://learn.microsoft.com/security/benchmark/azure/mcsb-network-security  

## DDoS Protection

### SKUs

- DDOS Network Protection (paid for per 100 IPs)  
- DDOS IP Protection (Individual IP)  

### Types of Attacks

- Volumetric attacks  
- Protocol attacks  
- Resource (application) layer attacks  

## Network Security Groups

Service Tags  
Application Security Group  

## Design and Implement Azure Firewall

- Stateful  
- High availability  
- Unrestricted cloud scalability (Auto scales)  
- create, enforce & log  
	- application and network connectivity policies (L3 - L7 filtering)  
	- threat intelligence  
- Hybrid connectivity (on-prem and cloud)  
- Monitor integration

### Azure Firewall: Secured vHub vs Hub vNet Comparison

| Feature                          | Secured vHub                                      | Hub Virtual Network (Hub vNet)                   |
|----------------------------------|--------------------------------------------------|--------------------------------------------------|
| **Deployment Model**             | Integrated with Azure Virtual WAN.               | Standalone deployment in a Virtual Network.      |
| **Routing**                      | Automatically integrated with Virtual WAN routing. | Requires manual configuration of User-Defined Routes (UDRs). |
| **Centralized Management**       | Managed via Azure Firewall Manager.              | Managed via Azure Firewall Manager or standalone. |
| **Scalability**                  | Auto-scaling with Virtual WAN.                   | Auto-scaling supported.                          |
| **High Availability**            | Built-in with Virtual WAN.                       | Built-in.                                        |
| **Global Connectivity**          | Supports global connectivity across regions via Virtual WAN. | Limited to the region of the Virtual Network.    |
| **Integration with Branches**    | Seamless integration with branch offices via Virtual WAN. | Requires VPN or ExpressRoute gateways for branch connectivity. |
| **Policy Management**            | Centralized policy management for multiple hubs. | Centralized or standalone policy management.     |
| **Cost**                         | Additional cost for Virtual WAN and Secured vHub. | Cost limited to Azure Firewall and associated resources. |
| **Use Case**                     | Ideal for global, distributed network architectures. | Ideal for hub-and-spoke architectures within a single region. |
| **Third-Party Security Providers** | Supported via Azure Firewall Manager.            | Not natively supported.                          |

For more details, refer to the official documentation:  
- [Azure Firewall in Secured Virtual Hub](https://learn.microsoft.com/en-us/azure/firewall-manager/secured-virtual-hub)  
- [Azure Firewall in Hub Virtual Network](https://learn.microsoft.com/en-us/azure/firewall/overview)

### Rule processing

- NAT Rules Configure DNAT rules to allow incoming connections  
- Network Rules Configure rules that contain source address, protocols, destination ports, and destination addresses.  
- Application Rules Configure FQDN names that can be accessed from a subnet  

### NSG vs Azure Firewall: Feature Comparison

| Feature                          | Network Security Groups (NSGs)                          | Azure Firewall                                      |
|----------------------------------|---------------------------------------------------------|----------------------------------------------------|
| **Purpose**                      | Filters traffic at the network layer (L3/L4).           | Provides stateful traffic filtering at L3-L7.      |
| **Scope**                        | Subnets and NICs within a Virtual Network.              | Entire Virtual Network or multiple VNets.          |
| **Stateful/Stateless**           | Stateful.                                               | Stateful.                                          |
| **Protocol Support**             | TCP, UDP, ICMP.                                         | TCP, UDP, ICMP, HTTP/S, and more.                  |
| **Application Layer Filtering**  | Not supported.                                          | Supported (L7 filtering for HTTP/S traffic).       |
| **Threat Intelligence**          | Not supported.                                          | Supported (blocks traffic from known malicious IPs). |
| **Logging and Monitoring**       | NSG Flow Logs via Azure Monitor.                       | Fully integrated with Azure Monitor and Log Analytics. |
| **Rule Types**                   | Inbound and outbound security rules.                   | NAT, Network, and Application rules.              |
| **SNAT Support**                 | Not supported.                                          | Supported (Source Network Address Translation).    |
| **DNAT Support**                 | Not supported.                                          | Supported (Destination Network Address Translation). |
| **Integration with DDoS**        | Works with DDoS Protection Standard.                   | Works with DDoS Protection Standard.              |
| **Cost**                         | Free (included with Azure).                            | Paid service with Basic, Standard, and Premium SKUs. |
| **High Availability**            | Built-in.                                               | Built-in with unrestricted scalability.            |
| **Centralized Management**       | Not supported (rules managed individually).            | Centralized management via Azure Firewall Manager. |
| **Use Case**                     | Basic network-level traffic filtering.                 | Advanced threat protection and application-level filtering. |

For more details, refer to the official documentation:  
- [Azure Network Security Groups](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)  
- [Azure Firewall](https://learn.microsoft.com/en-us/azure/firewall/overview)  

### Rule Comparison: Azure NSGs vs Azure Firewall

| Feature                          | Network Security Groups (NSGs)                          | Azure Firewall                                      |
|----------------------------------|---------------------------------------------------------|----------------------------------------------------|
| **Rule Types**                   | Inbound and outbound security rules.                   | NAT, Network, and Application rules.              |
| **NAT Rules**                    | Not supported.                                          | Supported (DNAT rules to allow incoming connections). |
| **Network Rules**                | Supported (based on source/destination IP, port, and protocol). | Supported (source/destination IP, port, and protocol). |
| **Application Rules**            | Not supported.                                          | Supported (FQDN-based filtering for HTTP/S traffic). |
| **Priority**                     | Rules are evaluated in priority order (lower number = higher priority). | Rules are evaluated in priority order (lower number = higher priority). |
| **Default Rules**                | Includes default rules to allow/deny traffic (e.g., deny all inbound). | Includes default rules for system traffic and management. |
| **Custom Rules**                 | Fully customizable inbound and outbound rules.         | Fully customizable NAT, Network, and Application rules. |
| **Logging**                      | NSG Flow Logs via Azure Monitor.                       | Fully integrated logging via Azure Monitor and Log Analytics. |
| **Protocol Support**             | TCP, UDP, ICMP.                                         | TCP, UDP, ICMP, HTTP/S, and more.                  |
| **Rule Scope**                   | Applied to subnets or individual NICs.                 | Applied to the entire Virtual Network or multiple VNets. |

For more details, refer to the official documentation:  
- [Azure Network Security Groups](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)  
- [Azure Firewall Rules](https://learn.microsoft.com/en-us/azure/firewall/rule-processing)  

### Azure Firewall SKU Comparison

| Feature                          | Basic                          | Standard                       | Premium                        |
|----------------------------------|--------------------------------|--------------------------------|--------------------------------|
| **Stateful Firewall**            | Yes                            | Yes                            | Yes                            |
| **Threat Intelligence**          | Not supported                  | Supported                      | Supported                      |
| **TLS Inspection**               | Not supported                  | Not supported                  | Supported                      |
| **IDPS (Intrusion Detection and Prevention System)** | Not supported                  | Not supported                  | Supported                      |
| **Application Rules**            | Supported                      | Supported                      | Supported                      |
| **Network Rules**                | Supported                      | Supported                      | Supported                      |
| **NAT Rules**                    | Supported                      | Supported                      | Supported                      |
| **Protocol Support**             | TCP, UDP, ICMP                 | TCP, UDP, ICMP, HTTP/S         | TCP, UDP, ICMP, HTTP/S, TLS    |
| **Logging and Monitoring**       | Basic logging                  | Full logging via Azure Monitor | Full logging via Azure Monitor |
| **High Availability**            | Built-in                       | Built-in                       | Built-in                       |
| **Scalability**                  | Limited                        | Unlimited                      | Unlimited                      |
| **Use Case**                     | Small-scale workloads          | General-purpose workloads      | Advanced security workloads    |

For more details, refer to the official documentation:  
- [Azure Firewall SKUs](https://learn.microsoft.com/en-us/azure/firewall/overview#sku-comparison)  

## Secure your networks with Azure Firewall Manager  

- Centrally deploy & manage
- centrally manage routes
- Hierarchy of policies (global and local)
- Security as a Service -> advanced security -> selected 3rd party providers
- available in all regions

## Web Application Firewall

- Specialized/focus on the application that is a centralized protection service for our web applications.
- Supports on or more regions.
- Protects against most common exploits and vulnerabilities (OWASP top 10 auto policies)
- Only layer 7 (HTTP)
- Can be set to Prevention or Detection(Default)
- Can create custom rules

- Types:

	- Application Gateway
		- Create multiple polices to individual listeners or to path-based routing
		- Customize and seperate polies for each site
		- Monitor attacks
	- Front Door
		- Global and centralized solution
		- WAF enabled web applications inspect every incoming request delivered by Front Door at the network edge
		- WAF policy can be associated to one or more Front Door front-ends for protection
	- CDN (Preview)

