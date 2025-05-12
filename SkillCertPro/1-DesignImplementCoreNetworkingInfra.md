# Design and implement core networking infrastructure

## Design and implement IP addressing for Azure resources

### Plan and Implement Network Segmentation and Address Spaces

Network segmentation is a crucial security practice that divides your Azure virtual network (VNet) into smaller subnets. Each subnet acts as a separate network segment, isolating resources based on function or security requirements. This approach offers several benefits:

* **Enhanced Security:** By isolating workloads, a security breach in one subnet is less likely to affect others.
* **Improved Traffic Management:** Segmenting network allows for optimized traffic flow by directing communication within specific subnets.
* **Granular Access Control:** Security groups can be applied to subnets, defining which resources can communicate with others.

Planning your address space involves selecting appropriate private and public IP address ranges for your VNets and subnets. Here are some key considerations:

* **Private Address Space:** Choos a private address range from RFC 1918 space (e.g., 10.0.0.0/16, 172.16.0.0/12, 192.168.0.0/16) to accommodate your resources within the VNet.
* **Public IP Address Prefixes(Optional):** If your resources require internet accessibility, plan a public IP address prefix from Azure. This allows assigning public IP addresses to resources within the subnet for inbound connections.

### Create a Virtual Network (VNet)

A VNet acts as the foundation for private IP addressing in Azure. It represents a logically isolated network within the azure cloud dedicated to your resources. When creating a VNet, you define:

* **Resource Group:** A logical contain to organize Azure resources.
* **Name:** a unique identifier for your VNet.
* **Location:** The Azure region where the VNet resides.
* **Address Space:** The private IP address range for your VNet.
* Subnet Configuration (Optional):** You can define subnets during VNet creation or add them later.

### Plan and Configure Subnetting for Services

Subnetting involves dividing your VNet's address space into smaller, more manageable networks called subnets. Each subnet can house specific resources requiring IP address for communication within the VNet. Here's how subnetting is crucial for different Azure services:

* **VNet Gateways:** Subnets for internet connectivity (public gateway) and on-premises connectivity (VPN gateway) may be required.
* **Private Endpoints:** Subnets for private access to Azure services without traversing the internet can be configured.
* **Service Endpoints:** Subnets can be linked to specific Azure services for secure, direct communication.
* **Firewalls, Application Gateways:** Dedicated subnets may be needed to deploy these network security and traffic management solutions.
* **VNet-Integrated Platform Service:** Subnets can be designated for Azure services like Azure App Service or Azure SQL Database deployed with the VNet.
* **Azure Bastion:** A dedicated subnet is required for the Azure Bastion service, which provides secure RDP/SSH access to VMs in a private VNet.

#### Additional Considerations:

* **Subnet Delegation:** This feature simplifies resource deployment within a subnet by automatically assigning IP addresses from a designated range to resources upon creation.
* **Shared vs. Dedicated Subnets:** Decide if a subnet will host multiple service types or be dedicated to a specific function.

### Plan and Configure Subnet Delegation

* **Subnet delegation:** This feature allows you to delegate Azure resource providers the authority to manage IP addresses within a specific subnet. This simplifies network management by letting services like Azure App Service or Azure Kubernetes Service (AKS) automatically assign IP addresses to their resources within the subnet.

* **Planing considerations:**

  * **Resource providers:** Identify which Azure resource providers need delegation (e.g., App Service, AKS, etc.).
	* **Security groups:** Ensure appropriate security groups are applied to control inbound and outbound traffic for delegated resources.
	* **Monitoring:** Monitor resource utilization within the subnet to avoid potential IP address exhaustion.

### Plan and Configure Shared or Dedicated Subnets

* **Shared subnets:** A single subnet can be shared by multiple types of Azure resources. This approach is efficient for workloads with similar security requirements.
* **Dedicated subnets:** Each type of resource or workload can have its own dedicated subnet. This enhances security by isolating traffic between different workloads.
* **Planing considerations:**

  * **Security:** Evaluate security needs for your workloads. Dedicated subnets offer greater isolation.
	* **Scalability:** Consider future growth and choose a subnet size that can accommodate additional resources.
	* **Management:** Dedicated subnets require more management overhead compared to shared subnets.

### Create a Prefix for Public IP Addresses

* **Public IP address prefix:** A range of public IP addresses used to assign public IPs to Azure resources like VMs or web apps. This allows internet access to these resources.
* **Crating a prefix:** You can define a public IP address prefix during virtual network creation or allocate one separately.
* **Planning considerations:**

  * **Size:** Determine the number of public IP addresses you need based on your resource requirements.
  * **Static vs. Dynamic allocation:** Choose static allocation for specific resources or dynamic allocation for flexible deployments with Azure Resource Manager templates.

### Choose When to use a Public IP Address Prefix

* **Public IP addresses are needed for:**

  * Resources requiring internet access (e.g., web servers, VPN gateways).
	* Accessing resources remotely via RDP or SSH.

* **Alternatives to public IP addresses:**

  * Azure Load Balancer can distribute traffic across VMs with private IP addresses.
	* Azure Private Link allows secure access to Azure services without exposing them to the internet.

* **Planning considerations:**

  * **Security:** Evaluate if public exposure is necessary for your resources. Consider alternatives for increased security.
	* **Cost:** Public IP addresses incur additional charges. Choose the most cost-effective access method.

### Plan and implement a custom public IP address prefix (bring your own IP - BYOI)

* **BYOI Overview:** In Azure, you can use your own public IP address ranges for resources. This is beneficial if you have existing IP allocation you want to leverage or need specific IP ranges for compliance reasons.
* **Planning Considerations:**

  * **IP Ownership:** Ensure you have legal rights to the IP range you want to bring.
	* **Region Availability:** BYOI IPs are not available in all Azure regions. Check for compatibility before planning.
	* **Prefix Size:** Request a prefix large enough for your future needs.
	* **Validation:** Azure validates the ownership and routing information for your BYOI prefix.

* **Implementation Steps:**

1. Contact Microsoft support to initiate the BYOI process.
2. Provide ownership and routing information for your IP prefix.
3. Once approved, create a virtual network (VNet) and associate the BYOI prefix during creation.

### Create a Public IP Address

* **Public IP Overview:** A public IP address allows internet access for Azure resoruces like virtual machines or web apps.
* **Creation Methods:**

  * **Azure Portal:** Select the desired resource and configure a public IP address during creation or allocation.
	* **Azure Resource Manger (ARM) Templates:** Define public IP resources within your ARM template for infrastructure as code deployments.
	* **Azure CLI or PowerShell:** Use commands to create public IP resources programmatically.

* **Allocation Types:**

  * **Basic:** Ephemeral public IP, dynamically assigned by Azure upon resource creation. Useful for temporary deployments.
	* **Standard:** Static public IP with fixed address. Ideal for production environments or long-running resources.

### Associate public IP Addresses to Resources

* **Associate Process:** After creating a public IP address, you can associate it with various resources.
* **Supported Resources:**

  * Virtual Machines
	* CLoud Services (classic model)
	* App Service environments
	* Azure Kubernetes Service (AS) nodes (with limitations)
	* Bastion hosts

* **Association Methods:**

  * **Azure Portal:** During resource creation or through the resource's configuration blade.
	* **ARM templates:** Define the public IP association within your template for infrastructure as code.
	* **Azure CLI or PowerShell:** Use commands to associate public IPs with resources.

* **Additional Tips:**

  * Public IP addresses incur charges. Choose the allocation type (basic or standard) based on your resource's needs.
	* Consider using Azure Load Balancer to distribute traffic across multiple resources with a single public IP for scalability and redundancy.
	* For enhanced security, use Network Security Groups (NSGs) to control inbound and outbound traffic for resources with public IPs.

## Design and implement name resolution

### Design Name Resolution Inside a VNet

By default, Azure provides a built-in DNS service for VNets. This service, known as the azure-provided DNS, resolves names ending with the suffix .internal.cloudapp.net for resources within the VNet. However, you can also:

* **User Custom DNS Servers:** You can configure your VNet to use external DNS servers hosted on-premises or by another provider. This offers more control over name resolution policies.
* **Link a Private DNS Zone:** Integrate a private DNS zone (explained later) with your VNet for more granular control over internal domain names.

### Configure DNS Settings for a VNet

When creating a VNet in Azure, you can specify the DNS server settings. This includes:

* **Name Servers:** Define the IP addresses of the DNS servers your VNet should use for name resolution. This can be teh Azure-provided DNS, custom DNS servers, or a combination.
* **DNS Search Suffix:** This optional setting specifies the domain suffix that should be appended to unqualified names (those without a dot-separated suffix) during resolution attempts.

### Design Public DNS Zones

Public DNS zones are hosted in Azure DNS, a managed DNS service. They map public domain names (like contoso.com) to public IP addresses of internet-facing resources like websites or applications. Designing a public DNS zone involves:

* **Planing the Zone Name:** Choose a unique and relevant domain name for your organization or application.
* **Record Types:** Define various record types like A records (maps domain names to IP addresses), CNAME records (aliases for other domains), and MX records (for mail servers).
* **Security Considerations:** Implement security measures like DNSSEC (Domain Name System SEcurity Extensions) to protect your zone from spoofing attacks.

### Design Private DNS Zones

Private DNS zones, also hosted in Azure DNS, are used for internal name resolution within your organization's VNets. Unlike public zones, they are not accessible from the internet. Here's how to design a private zone:

* **Zone Naming:** Choose a descriptive name that reflects its purpose within your VNet.
* **Record Types:** Similar to public zones, define record types to map internal domain names to resources within the VNET (e.g., web servers, databases).
* **Integration with VNet:** Link the private zone with your VNet to enable resources within the VNet to resolve names hosted in the private zone.

### Configure Public and PRivate DNS Zones:**

* **Public DNS Zones:** These zones reside in Azure DNS and are accessible from the public internet. They allow you to manage DNS records for your custom domain names (e.g., contoso.com) and point them to Azure resources (websites, VMS) with public IP addresses.
* **Private DNS ZOnes:** These zones exist within your Virtual Network (VNet) and are not accessible from the internet. They provide internal name resolution for resources within the VNet, allowing VMs to communicate using FQDNs instead of IP addresses. You can either use:

  * **Azure-provided DNS:** This is the default option where Azure manages a hidden zone (.internal.cloudapp.net) for your VNet.
  * **Custom DNS Servers:** You can integrate your own DNS server solution within the VNet for more granular control over internal name resolution.

### Link a Private DNS Zone to a VNet

To leverage a custom private DNS zone for internal name resolution in your VNet, you need to link them. This allows VMs within the VNet to automatically register their hostnames and IP addresses in the linked zone.

### Design and Implement Azure DNS Private Resolver:

Azure DNS Private Resolver acts as a managed DNS service for private DNS zones. It provides a single endpoint within your VNet for VMs to perform DNS queries. This simplifies configuration and eliminates the need to manage individual DNS servers for each VNet.

**Here are some additional points to consider:**

  * **Record Types:** Both public and private zones allow you to configure various DNS record types like A records (map hostname to IP), CNAME records (alias for another hostname), and MX records (for email delivery).
	* **Security:** Public DNS zones require proper access control to prevent unauthorized modifications. Private zones offer inherent security as they are not accessible from the internet.
	* **Integration:** Azure DNS integrates with other azure services like Azure Active Directory for secure zone management and Traffic Manager for load balancing across geographically distributed resources.

**Additional Resources:**

Microsoft documentation on Azure DNS: https://learn.microsoft.com/en-us/azure/dns/
Study guide for Exam AZ-700: https://learn.microsoft.com/en-us/training/courses/az-700t00

## Design and implement VNet connectivity and routing

### Service Chaining with Gateway Transit

* **Concept:** Service chaining allows traffic to flow through a sequence of Azure network security services (firewalls, application gateways) before reaching its destination. Gateway transit simplifies this process by enable a single VNet to act as a transit center for traffic flowing between VNets and network security services.
* **Design Considerations:**

  * Identify the sequence of network security services the traffic needs to traverse.
	* Plan the location fo the transit VNet for optimal performance.
	* Configure routing to direct traffic to the transit VNet and then specific security services.

* **Benefits:**

  * Centralize security enforcement for multiple VNets.
	* Simplified management of network security policies.

### VNet Peering

* **Concept:** VNet peering directly connects VNets within the same region or across regions. It allows resources in peered VNets to communicate with each other using private IP addresses.

* **Implementation:**

  * Choose between region or global peering based on VNet locations.
	* Configure peering connections in each VNet specifying the peer VNet's resource ID.
	* Optionally, configure route tables to control traffic flow between peered VNets.

* **Benefits:**

  * Low-latency, secure communication between VNets.
  * Simplifies network architecture for interconnected resources.

### Azure Virtual Network Manager

* **Concept:** Virtual Network Manager is a centralized tool fro managing and deploying VNets at scale. It allows for defining network configurations as code and deploying them consistently across environments.
* **Implementation:**

  * Define VNet configurations using Azure Resource Manager (ARM) templates.
	* Utilize Virtual Network Manager to manage deployments, policies, and access control fro VNets.

* **Benefits:**

  * Consistent and repeatable VNet Deployments
	* Simplified management of complex network configurations.
	* Improved governance and control over network resources.

### User-Defiend Routes (UDRs)

* **Concept:** UDRs provide granular control over traffic routing within a VNet. They define how packets are forwarded to specific subnets or next hops (gateways) based on destination IP addresses and prefixes.
* **Design Considerations:**

  * Identify the traffic flow patterns within the VNet.
	* Define UDRs to route traffic to the appropriate subnets or gateways.
	* Prioritize UDrs to ensure the most specific route takes precedence.

* **Benefits:**

  * Customized traffic flow management within a VNet.
	* Optimization of network performance for specific networks.

### Associating Route Table with Subnet

* **Concept:** A route table defines the routing behavior for a subnet. A subnet can only be associated with one route table at a time. The route table dictates how traffic destined for addresses outside the subnet is forwarded.
* **Implementation:**

  * Create a route table with UDRs defining the desired traffic flow.
	* Associate the route table with desired subnet in the VNet configuration.

* **Benefits:**

  * Control traffic flow at the subnet level for specified workloads.
	* Allows for differentiated routing policies within a VNet.

### Forced Tunneling

* **Concept:** Forced tunneling ensures all traffic destined for the internet or on-premises networks traverses the Azure virtual network gateway (VPN gateway or ExpressRoute gateway). This mechanism prevents traffic from bypassing security controls within the VNet.
* **Configuration:**

  1. Create a route table with a default route pointing to the internet or on-premises network address space via the virtual network gateways's IP address.
	2. Associate the route table wi tht desired subnets in your VNet.

* **Benefits:**

  * Enhanced security: traffic is routed through firewalls and network security groups (NSGs) for inspection and control.
	* Centralized management: Policies are applied at the gateway level, simplifying administration.

* **Considerations:**

  * Increased latency: Traffic might take a longer path compared to direct egress.
	* Potential costs implications: Egress charges may apply depending on your pricing tier.

### Diagnosing and Resolving Routing Issues

* **Troubleshooting Techniques:**

  1. **Verification:**

	  * Confirm route table associations with subnets.
		* Validate route table entries (destination, next hop).
		* Check for overlapping address spaces.
	
	2. **Network Watcher:** Use Azure Network Watcher's tools like "Next hop" and "Effective route" to trace the path packets take and identify bottlenecks.
	3. **Logs:** Analyze diagnostic logs from virtual network gateways, VMs, and NSGs for routing-related events or errors.

* **Common Issues:**

  * Incorrect route table associations or entries.
	* Overlapping IP addresses causing conflicts.
	* Security group rules blocking traffic unexpectedly.
	* Connectivity problems with the virtual network gateway.

### Azure Route Sever

* **Purpose:** A highly available, scalable routing solution for complex networking topologies with multiple VNets, on-premises networks, and internet connectivity. It centralizes route management, simplifies configuration, and enhances routing performance.
* **Use Cases:**

  * Large-scale deployments with intricate routing requirements.
	* Scenarios requiring dynamic routing protocols like BGP for inter-network communication.
	* Need for centralized route advertisement and propagation across multiple VNets.

* **Benefits:**

  * Scalability: handles high volumes of routes and traffic efficiently.
	* Centralized Management; Simplifies route configuration and updates across VNets.
	* Dynamic Routing: Supports protocols like BGP for optimal path selection.
	* High Availability: redundancy ensures reliable route advertisement and traffic flow.

* **Implementation Considerations:**

  * Deployment complexity: Requires careful planning and configuration.
	* Licensing costs: Azure Route Server incurs additional charges.

### Network Address Translation (NAT) Gateway

* **Function:** Enables outbound internet connectivity for resources within a private VNet that don't have public IP addresses assigned. The NAT gateway translates private IP addresses to a public IP address, allowing outbound communication without exposing individual VM addresses to the internet.

* **Use Cases:**

  * VNets with with resources that need internet access without public IP addresses (e.g., internal databases).
	* Maintaining security by minimizing public exposure of resources.
	* Scenarios where public IP addresses are not cost-effective for all resources.

* **Implementation:**

  1. Create a NAT gateway resource in your desired resource group and region.
	2. Allocate a public IP address for the NAT gateway.
	3. Associate teh NAT Gateway's subnet with a public IP address prefix.
	4. Configure outbound security rules in the NAT gateway's NSG to allow required traffic.

* **Benefits:**

  * Secure outbound internet access: resources remain private while having internet connectivity.
	* Centralized outbound control: Manage outbound traffic through the NAT gateway's security policies.
	* Cost optimization: REduce costs associated with public IP addresses for all resources.

**Additional Considerations:**

* **UDRs (User-Defined Routes):** PRive granular control over traffic flow within a VNEt. Use them to override default routes and direct traffic to specific destinations.
* **Service Chaining with Gateway Transit:** COnfigure a VNet gateway (VPN or ExpressRoute) to route traffic through another gateway (typically a NAT gateway) for security or network address translation purposes.
* **VNet Peering:** Directly connect VNets within the same region or across regions for private communication between resources.

## Monitor networks

### Configure Monitoring, Network Diagnostics, Next Hop, and Path Visualization in Azure Network Watcher

* **Network Watcher:** This service provides tools to monitor and diagnose your Azure virtual networks.
* **Monitoring:** You can configure Network Watcher to collect data on various network metrics like bandwidth utilization, packet latency, and connection status.
* **Network diagnostics::** This functionality allows you to troubleshoot network issues by performing tasks like:

  * **Next hop:** Identify the next hop for a specific destination IP address, helping you understand traffic routing.
	* **Path visualization:** Visually trace the network path taken by packets between resources, providing insights into potential bottlenecks.

* **Logs:** Network Watcher can also collect logs for network security groups (NSGs) ad application security groups (ASGs), aiding in security analysis.

### Monitor and troubleshoot network health by using Azure Network Watcher

Network Watcher offers various tools to proactively monitor and troubleshoot network health:

* **Connectivity checks:** Verify connectivity between resources within a virtual network or between your Azure environment and on-premises locations.
* **IP flow analysis:** Analyze network traffic patterns to identify anomalies and potential security threats.
 **VM connection monitor:** Monitor network connectivity and performance for Azure.

### Monitor and troubleshoot networks by using Azure Monitor Network Insights

* **Azure Monitor:** This service provides a central location for collecting, analyzing, and visualizing telemetry data from various Azure resources, including virtual networks.
* **Network Insights:** This is a feature within Azure Monitor that focuses specifically on network traffic data. It offers advanced capabilities for:

  * **Traffic analysis:** Analyze traffic patterns by source, destination, protocol, and application.
	* **Performance monitoring:** Monitor network performance metrics like latency, throughput, and packet loss.
	* **Troubleshooting tools:** Leverage tools like next hop analysis and path visualization similar to Network Watcher, but with potentially deeper insights from traffic data.

### Activating and Monitoring DDoS Protection

Distributed Denial-of-Service (DDoS) attacks overwhelm your resources with traffic, making applications inaccessible. Azure offers DDoS Protection Standard and Premium tiers to safeguard your virtual networks. Here's how to activate and monitor them:

* **Activate DDoS Protection:**

  1. Navigate ot your virtual network in the Azure portal.
	2. Go to the "Security" section and select "DDOS protection."
	3. Choose the appropriate tier (Standard or Premium) based on your needs.
	4. Configure DDoS protection settings like attack mitigation thresholds.

* **Monitor DDoS Protection:**

	1. Access the DDoS Protection overview page for your virtual network.
	2. View metrics like incoming traffic volume, attack types detected, and mitigation actions taken.
	3. Utilize Azure Monitor logs to analyze detailed DDoS activity and identify potential threats.
	4. Configure alerts to receive notifications when DDoS attacks occur or mitigation thresholds are reached.

	### Evaluating Network SEcurity Recommendations from Microsoft Defender for CLoud Secure Score

	Microsoft Defender for Cloud provides "Secure Score" that assesses your Azure enviornment's security posture. It includes recommendations for improving network security. Here's how to evaluate them:

* **Understanding Secure Score:**

  1. Access teh Microsoft Defender for Cloud Portal
  2. Locate the "Secure Score" section to view your overall score and breakdown by security areas.

* Evaluating Network Security Recommendations

  1. Navigate to the "Recommendations" section under "Secure Score."
	2. Filter by security control to focus on network security recommendations.
	3. Each recommendation details the potential security risk and recommended mitigation steps.
	4. Analyze the impact of each recommendation and prioritize them based on severity and risk.

By implementing these recommendations, you can strengthen your network security posture and reduce vulnerabilities.

Here are some additional resources that you might find helpful:

* **Azure DDoS Protection documentation:** https://learn.microsoft.com/en-us/azure/ddos-
protection/
* **Microsoft Defender for Cloud Secure Score:** https://learn.microsoft.com/en-
us/training/modules/examine-microsoft-secure-score/

### Evaluating Network Security Recommendations from Attack Path Analysis

* **Microsoft Defender for Cloud Attack Path Analysis:** This is a feature that analyzes your Azure environment and identifies potential attack paths an attacker could exploit. It considers vulnerabilities, misconfigurations, and resource access controls to build a security graph.
* **Evaluating Recommendations:** After running Attack Path Analysis, you'll see a list of identified attack paths. Each path details the resources involved and potential weaknesses. Defender for Cloud also provides recommendations to mitigate these risks. These might include:

  * Patching vulnerable resources
	* Hardening security configurations (e.g., restricting access controls)
	* Implementing additional security measures (e.g, firewalls)

**How to Us this in AZ-700:**

* You'll need to understand how to interpret the attack paths displayed by Defend for Cloud.
* You should be ble to identify network security weaknesses based on the information provided.
* The exame might ask you to evaluate specific recommendations and determine their effectiveness in mitigating network security risks.


### Identifying Network Resources with Security Explorer

* **Microsoft Defender for Cloud Security Explorer:** This is another tool within Defender for Cloud that allows you to investigate security posture across your Azure resources. It provides a unified view of security alerts, recommendations, and resource configurations.
* **Identifying Network Resources:** Security Explorer allows you to filter and search for specific resources based on various criteria, including:

  * Resource type (e.g., virtual networks, load balancers)
	* Location
	* Security state (e.g., compliant, misconfigured)

**How to Use This in AZ-700:**

* You should be familiar with the different types of Azure network resources and their security implications.
* The exam might ask you to use Security Explorer to identify specific network resources with security vulnerabilities or misconfigurations.
* Practice using Microsoft Defender for CLoud in a lab environment to get hands-on experience with Attack Path Analysis and Security Explorer.
