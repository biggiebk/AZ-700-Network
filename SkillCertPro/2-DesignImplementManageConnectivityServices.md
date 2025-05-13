# Design, Implement, and Manage Connectivity Services (20–25%)

## Table of Contents

1. [Design, Implement, and Manage a Site-to-Site VPN Connection](#design-implement-and-manage-a-site-to-site-vpn-connection)
   - [Designing a Site-to-Site VPN Connection](#designing-a-site-to-site-vpn-connection)
   - [Selecting VNet Gateway SKU](#selecting-vnet-gateway-sku)
   - [Implementing a Site-to-Site VPN Connection](#implementing-a-site-to-site-vpn-connection)
   - [Policy-Based vs. Route-Based VPN Connections](#policy-based-vs-route-based-vpn-connections)
   - [Create and Configure a Local Network Gateway (LNG)](#create-and-configure-a-local-network-gateway-lng)
   - [Create and Configure IPsec/IKE Policy](#create-and-configure-ipsecike-policy)
   - [Create and Configure a Virtual Network Gateway (VNet Gateway)](#create-and-configure-a-virtual-network-gateway-vnet-gateway)
   - [Diagnose and Resolve Virtual Network Gateway Connectivity Issues](#diagnose-and-resolve-virtual-network-gateway-connectivity-issues)

2. [Design, Implement, and Manage a Point-to-Site VPN Connection](#design-implement-and-manage-a-point-to-site-vpn-connection)
   - [Selecting a Virtual Network Gateway SKU for Point-to-Site VPN](#selecting-a-virtual-network-gateway-sku-for-point-to-site-vpn)
   - [Tunnel Type Selection and Configuration](#tunnel-type-selection-and-configuration)
   - [Authentication Method Selection](#authentication-method-selection)
   - [RADIUS Authentication Configuration](#radius-authentication-configuration)
   - [Azure AD Authentication with Microsoft Entra Configuration](#azure-ad-authentication-with-microsoft-entra-configuration)
   - [Implement a VPN Client Configuration File](#implement-a-vpn-client-configuration-file)
   - [Diagnose and Resolve Client-Side and Authentication Issues](#diagnose-and-resolve-client-side-and-authentication-issues)
   - [Azure Requirements for Always On VPN](#azure-requirements-for-always-on-vpn)
   - [Azure Requirements for Azure Network Adapter](#azure-requirements-for-azure-network-adapter)

3. [Design, Implement, and Manage Azure ExpressRoute](#design-implement-and-manage-azure-expressroute)
   - [Selecting an ExpressRoute Connectivity Model](#selecting-an-expressroute-connectivity-model)
   - [Choosing the Model](#choosing-the-model)
   - [Selecting an Appropriate ExpressRoute SKU and Tier](#selecting-an-appropriate-expressroute-sku-and-tier)
   - [Designing ExpressRoute for Specific Requirements](#designing-expressroute-for-specific-requirements)
   - [ExpressRoute Options](#expressroute-options)
   - [Configuring Azure Private Peering](#configuring-azure-private-peering)
   - [Configure Microsoft Peering](#configure-microsoft-peering)
   - [Create and Configure an ExpressRoute Gateway](#create-and-configure-an-expressroute-gateway)
   - [Connect a Virtual Network to an ExpressRoute Circuit](#connect-a-virtual-network-to-an-expressroute-circuit)
   - [Recommend a Route Advertisement Configuration](#recommend-a-route-advertisement-configuration)
   - [Configure Encryption over ExpressRoute](#configure-encryption-over-expressroute)
   - [Implementing Bidirectional Forwarding Detection (BFD)](#implementing-bidirectional-forwarding-detection-bfd)
   - [Diagnosing and Resolving ExpressRoute Connection Issues](#diagnosing-and-resolving-expressroute-connection-issues)

4. [Design and Implement an Azure Virtual WAN Architecture](#design-and-implement-an-azure-virtual-wan-architecture)
   - [Selecting a Virtual WAN SKU](#selecting-a-virtual-wan-sku)
   - [Choosing the Right SKU](#choosing-the-right-sku)
   - [Designing a Virtual WAN Architecture](#designing-a-virtual-wan-architecture)
   - [Creating a Hub in a Virtual WAN](#creating-a-hub-in-a-virtual-wan)
   - [Choosing Appropriate Scale Units for Gateways](#choosing-appropriate-scale-units-for-gateways)
   - [Deploying a Gateway into a Virtual WAN Hub](#deploying-a-gateway-into-a-virtual-wan-hub)
   - [Configuring Virtual Hub Routing](#configuring-virtual-hub-routing)
   - [Custom Routing with User-Defined Routes (UDRs)](#custom-routing-with-user-defined-routes-udrs)
   - [Integrating a Third-Party NVA for Cloud Connectivity](#integrating-a-third-party-nva-for-cloud-connectivity)

---

## Design, Implement, and Manage a Site-to-Site VPN Connection

### Designing a Site-to-Site VPN Connection

* **Connectivity Requirements:**

  * Identify traffic volume and bandwidth needs between your on-premises network and Azure.
  * Determine if you need high availability (HA) for connection (explained later)

* **IP Addressing:**

  * Plan non-overlapping address spaces for your on-premises network and Azure VNets.

* **Security Considerations:**

  * Choose an appropriate encryption algorithm (e.g., AES-GCM) and key strength.
  * Define firewall rules to control traffic flow.

* **High Availability (HA):**

  * For redundancy, configure Active-Active mode with two virtual network gateways (VNet gateways) in your Azure VNet, each paired with your on-premises VPN device. Traffic is balanced across both tunnels.
  * Alternatively, use Active-Standby mode with a primary and Secondary VNet Gateway. The secondary takes over if the primary fails.

### Selecting VNet Gateway SKU:

VNet gateway SKUs offer different Throughput (Mbps) capacities. Choose the one that aligns with your expected traffic volume:

* **Basic:** Suitable for low-bandwidth scenarios (up to 100 Mbps).
* **Standard:** Good for typical workloads (up to 1 Gbps).
* **High Performance:** Ideal for high-bandwidth needs (up to 10 Gbps).
* **Ultra Performance:** Supports extremely high bandwidth requirements (up to 80 Gbps).

### Implementing a Site-to-Site VPN Connection:

  * **Create a Resource Group:** Organize your Azure resources in a logical group.
  * **Configure a Virtual Network(VNet):** Define the address space for your Azure resources.
  * **Create a GatewaySubnet:** A dedicated subnet within your VNet for deploying the VNet gateway.
  * **Deploy a Virtual Network Gateway (VNet Gateway):** Choose the desired SKU based on your needs.
  * **Configure a Local Network Gateway:** This represents your on-premises VPN device.
  * **Create a VPN Connection:** Specify the VNet gateway, local network gateway, connection type (IPSec), and authentication method (e.g., pre-shared key).
  * **Configure your On-Premises VPN Device:** Establish the VPN tunnel according to your device's specific instructions.
  * **Verify the Connection:** Use Azure portal tools or network monitoring tools to confirm successful connectivity.

**Additional Resources:**

* Microsoft documentation on S2S VPNs: https://learn.microsoft.com/en-us/azure/vpn-gateway/
* Exam AZ-700: Design and implement core networking infrastructure: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-700

### Policy-Based vs. Route-Based VPN Connections

* **Policy-Based VPN (Default):** This is the most common type and is easier to configure. It uses pre-defined security policies to determine which traffic is allowed through the VPN tunnel. Policies define source and destination address prefixes, protocols, and ports.

* **Route-Based VPN:** Offers more granular control by relying on routing protocols (like BGP) to determine which traffic goes through the VPN. This is typically used for complex network configurations with specific routing needs.

### Choosing the Right Option:

* Use a policy-based VPN for most scenarios, especially for simple connectivity between your on-premises network and Azure VNet.

* Consider a route-based VPN if you need fine-grained control over traffic routing or have complex network configurations.

### Create and Configure a Local Network Gateway (LNG)

An LNG represents your on-premises network in Azure. It defines the address space of your on-premises network and the public IP address of your VPN device. Here's how to create and configure it:

1. Access Azure Portal and navigate to "Virtual Wans" or "Virtual Network Gateways".
2. Click "Create" and choose "Local network gateway".
3. Provide a name, resource group, and location for the LNG.
4. Define the "address space" that represents your on-premises network IP range.
5. Specify the "Public IP address" of your on-premises VPN device.
6. Review and create the LNG.

### Create and Configure IPsec/IKE Policy

IPSec (Internet Protocol Security) and IKE (Internet Key Exchange) are protocols used to establish a secure VPN tunnel. The IPSec/IKE policy defines the encryption algorithms, key exchange methods, and other security parameters for the connection. Here's the configuration process:

1. Go to your Virtual Network Gateway in Azure Portal.
2. Click on "Connections" and then "Add".
3. Choose "Site-to-Site (IPSec)" as the connection type.
4. Select the previously created Local Network Gateway.
5. Define the "Shared key" (PSK) used for authentication between Azure and your on-premises VPN device (refer to your device's documentation for configuration).
6. Choose the appropriate "IKE version" (typically IKEv2).
7. Optionally, configure advanced settings like encryption algorithms, DH group, and lifetime cycles.
8. Review and create the VPN connection. Azure will provide a configuration script that you can download and import into your on-premises VPN device to complete the setup.

### Create and Configure a Virtual Network Gateway (VNet Gateway)

A VNet Gateway acts as a secure tunnel endpoint that connects your on-premises network to your Azure virtual network (VNet). Here's how to create and configure one:

* **Design:**

  * Choose a gateway type: VNet Gateway supports various options like VPN (IPSec), ExpressRoute, and internet-facing. Select "VPN" for site-to-site connectivity.
  * Decide on a SKU: Different SKUs offer varying throughput and pricing options. Consider your bandwidth needs.
  * Plan the gateway subnet: Create a dedicated subnet within your VNet to deploy the VNet Gateway. It should have enough IP addresses for future scaling.

* **Configuration:**

  * Use the Azure portal, Azure PowerShell, or Azure CLI to create the VNet Gateway resource.
  * Specify the resource group, location, gateway type (VPN), SKU, and the pre-created gateway subnet.
  * Configure public IP address allocation (static or dynamic) for the VNet Gateway.

### Diagnose and Resolve Virtual Network Gateway Connectivity Issues

Troubleshooting connectivity issues with your VNet Gateway involves identifying the root cause. Here are some steps:

* **Verification:**

  * Check the VNet Gateway health using the Azure portal. Look for errors or warnings.
  * Verify the VPN connection status. Ensure it's connected and operational.

* **Connectivity Tools:**

  * Use Azure Monitor logs to diagnose issues related to routing, IKE negotiation, or IPSec tunnels.
  * Leverage tools like Azure Network Watcher to perform connectivity tests and identify bottlenecks.

* **Common Issues:**

  * Incorrect configuration: Double-check settings like subnet assignments, public IP addresses, and firewall rules.
  * Routing problems: Ensure proper routing is configured on both the on-premises network and the Azure VNet.
  * Security group restrictions: Verify that security groups don't block necessary ports (IKE and IPSec).

## Design, Implement, and Manage a Point-to-Site VPN Connection

### Selecting a Virtual Network Gateway SKU for Point-to-Site VPN

P2S VPN connections rely on virtual network gateways (VNet Gateways) in Azure. When choosing a VNet gateway SKU, consider these factors:

* **Number of concurrent connections:** Select a SKU that supports the expected number of users simultaneously connecting via P2S VPN. Basic SKU is suitable for low connection counts, while Standard SKU caters to more users.
* **Throughput needs:** Standard SKUs offer higher bandwidth compared to Basic SKUs for data transfer over the VPN tunnel.
* **Scalability requirements:** If you anticipate growth in P2S users, choose a standard SKU for easier scaling.

### Tunnel Type Selection and Configuration

There are two main tunnel types for P2S VPN:

* **Policy-based Routing (IKEv2):** This is the most common and recommended option. It offers dynamic routing capabilities and negotiation between the VPN client and Azure VNet gateway.
* **Route-based VPN (OpenVPN):** This offers more granular control over routing but requires manual configuration on both the client and Azure side.

### Authentication Method Selection

P2S VPN supports various authentication methods to secure connections:

* **Pre-shared Key (PSK):** A simpler method using a shared secret for authentication. However, it's less secure for large deployments.
* **RADIUS Authentication:** Provides centralized user authentication using a Remote Authentication Dial-In Service (RADIUS) server. More secure for managing multiple users.
* **Azure Active Directory (Azure AD) with Microsoft Entra:** Integrates with Azure AD for user authentication using existing identities. Offers strong security and centralized management.

### RADIUS Authentication Configuration

To configure RADIUS authentication:

* **Deploy a RADIUS server:** This can be an on-premises server or a cloud-based solution like Azure AD RADIUS.
* **Configure RADIUS server:** Set up user accounts, authentication protocols (e.g., PAP, MSCHAP v2), and shared secrets.
* **Configure the Azure VNet Gateway:** In the Azure portal, specify the RADIUS server details like address, shared secret, and authentication methods.

### Azure AD Authentication with Microsoft Entra Configuration

Microsoft Entra (Formerly Azure AD Multi-Factor Authentication) integrates with Azure AD for P2S VPN authentication. Here's how to configure it:

* **Enable Azure AD authentication on the VNet gateway:** In the Azure portal, choose this option during VNet gateway creation.
* **Configure user groups:** Assign specific Azure AD user groups access to the P2S VPN connection.
* **Configure Microsoft Entra for multi-factor authentication (MFA):** Set up MFA policies for additional security on top of Azure AD credentials.

**Additional Resources:**
* Microsoft documentation on P2S VPN: https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about
* Selecting a VNet gateway SKU: https://learn.microsoft.com/en-us/azure/vpn-gateway/

### Implement a VPN Client Configuration File

Azure Point-to-Site (P2S) VPN connections allow individual devices to securely connect to your Azure Virtual Network (VNet). The client configuration file holds the information needed for a device to establish the VPN tunnel. You cannot directly edit this file, but Azure generates it based on your P2S configuration.

Here's how to obtain the VPN client configuration file:

* **Azure Portal:**

  1. Go to your virtual network gateway resource.
  2. Select "Point-to-Site configuration."
  3. Click "Download VPN client." This generates a zip file containing the configuration files.

* **PowerShell:** Use the New-AZVpnClientConfiguration cmdlet to generate the configuration for a specific resource group and gateway name.

The configuration file typically includes:

* **Connection details:** Server address, VPN type (IKEv2 or OpenVPN).
* **Authentication settings:** Based on your chosen method (certificate, Azure AD, RADIUS).
* **Encryption settings:** Cipher suites and algorithms used for secure communication.

### Diagnose and Resolve Client-Side and Authentication Issues

Troubleshooting P2S VPN connectivity issues involves checking both client-side configuration and authentication methods. Here are some common issues and solutions:

* **Client configuration errors:** Ensure the downloaded configuration file is imported correctly on the client device. Verify settings like connection name, server address, and authentication certificates (if used).
* **Connectivity issues:** Check firewall rules on the client device that might block VPN traffic. Test internet connectivity and ensure the client device can reach the Azure VPN Gateway endpoint.
* **Authentication failures:** For certificate-based authentication, verify the client certificate is valid, not expired, and trusted by the Azure VPN Gateway. For Azure AD or RADIUS authentication, check if the user has the necessary permissions and multi-factor authentication (MFA) is configured correctly (if used).

### Azure Requirements for Always On VPN

Always On VPN is a Windows 10 feature that allows a device to automatically establish and maintain a VPN connection. While Azure supports P2S VPN with Always On VPN, there are specific requirements:

* **Supported Operating Systems:** Windows 10 or later version with Always On VPN feature enabled.
* **Configuration method:** Pushing the VPN profile using Group Policy or Mobile Device Management (MDM) is recommended for centralized control.
* **Authentication method:** Certificate authentication is the most secure option for Always On VPN with P2S.

### Azure Requirements for Azure Network Adapter

Azure Network Adapter is a feature that allows a Windows or Linux VM to directly connect to an Azure VNet without needing a separate VPN connection. Here are the key points for Azure Network Adapter:

* **Supported Platforms:** Windows Server 2016 or later, most Linux distributions.
* **Deployment Model:** Only available for VMs deployed in the Resource Manager deployment model.
* **Connectivity:** Provides direct, private connectivity between the VM and the VNet resources.
* **Security:** Requires proper Network Security Group (NSG) configuration to control inbound and outbound traffic.

**Additional Resources:**

* Microsoft documentation on Point-to-Site VPN: https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about
* Download Azure VPN Client: https://www.microsoft.com/p/azure-vpn-client/9np355qt2sqb
* Always On VPN documentation: https://learn.microsoft.com/en-us/windows-server/remote/remote-access/tutorial-aovpn-deploy-setup
* Azure Network Adapter documentation: https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/use-azure-network-adapter

## Design, Implement, and Manage Azure ExpressRoute

### Selecting an ExpressRoute Connectivity Model

ExpressRoute offers two connectivity models:

1. **Private Peering:** This establishes a private connection between your on-premises network and Microsoft Azure through a connectivity provider. Traffic stays off the public internet, improving security and performance.

2. **Microsoft Peering:** This model connects to Microsoft public cloud servers (like Office 365 and Dynamics 365) through the Microsoft global network.

### Choosing the Model

* Select **Private Peering** for secure, private connectivity to Azure resources and Microsoft services within the same region.
* Select **Microsoft Peering** if you need to connect to Microsoft public cloud services in addition to Azure resources.

### Selecting an Appropriate ExpressRoute SKU and Tier

ExpressRoute comes in Different SKUs (Standard and Basic) and tiers (Local, Metro, and Global Reach). Here's a breakdown:

* **SKUs:**

  * **Standard:** Offers higher bandwidth options (Up to 100Gbps) and features like Global Reach (explained later)
  * **Basic:** Lower bandwidth options (up to 2Gbps) suitable for smaller deployments.

* **Tiers:**

  * **Local:** Connects your on-premises network to Azure within the same metro area.
  * **Metro:** Connects your network to Azure across nearby metro regions.
  * **Global Reach:** Extends private connectivity across geographically distant Azure regions (Available only with Standard SKU)

### Choosing the SKU and Tier

* Consider your bandwidth needs. Standard SKU offers higher options for demanding workloads.
* Evaluate geographic reach requirements. Local is for same-metro connections, Metro for nearby regions, and Global Reach for geographically dispersed regions (with standard SKU)

### Designing ExpressRoute for Specific Requirements

Now, let's explore how to design ExpressRoute for key functionalities:

* **Cross-Region Connectivity:** Utilize ExpressRoute Global Reach (Standard SKU only) to establish private connections between Azure resources in different regions.
* **Redundancy:** Configure redundant ExpressRoute circuits from different connectivity providers to ensure uptime if one circuit fails.
* **Disaster Recovery:** Designate a secondary Azure region for disaster recovery and establish ExpressRoute connectivity to both primary and secondary regions for seamless failover.

**Additional Resources:**

* Microsoft offers a comprehensive learning module on Design and implement Azure ExpressRoute: https://learn.microsoft.com/en-us/training/modules/design-implement-azure-expressroute/
* This module covers configuration exercises using Azure portal and Azure PowerShell.

## ExpressRoute Options

Azure ExpressRoute offers several options to tailor your private connection to Microsoft Azure, each catering to specific use cases:

* **Global Reach:** Connects your on-premises network to Azure across different geographical regions. This is ideal for organizations with geographically dispersed resources or users who need low latency access to Azure services in multiple regions.

  * Implementation: You provision a single ExpressRoute circuit and configure Global Reach within it. This establishes private connectivity between your on-premises network and multiple Azure Regions.

* **FastPath:** Optimizes the data path for specific Microsoft cloud services like Azure Storage, Azure Databricks, and Azure SQL Database. It bypasses the standard internet routing for these services, potentially reducing latency and improving performance.

  * Implementation: You enable FastPath on your ExpressRoute Circuit during provisioning or later through Azure PowerShell or the Azure CLI. This activates the optimized data path for designated services.

**Choosing Peering Configuration**

ExpressRoute supports two primary peering configurations:

* **Azure Private Peering:** Enables private connectivity between your on-premises network and all Azure services (including virtual networks, PaaS services, and IaaS resources). This is the most common choice for private, secure access to all Azure offerings.
* **Microsoft Peering:** Allows private communication with Microsoft cloud services (like Office 365, Dynamics 365, and Azure Active Directory) that are not natively part of the Azure public cloud. This peering is helpful for hybrid deployments that integrate Azure with other Microsoft services.

You can also configure **both Azure private and Microsoft peering** on the same ExpressRoute circuit for scenarios where you need access to both types of resources.

### Configuring Azure Private Peering

Here's a breakdown of configuring Azure private peering for our ExpressRoute circuit:

1. **Provision an ExpressRoute circuit:** Use the Azure portal, Azure PowerShell, or the Azure CLI to create a new circuit. Specify the connectivity provider (e.g., Equinix, Verizon) and location.
2. **Enable private peering:** During circuit creation or later, select "Azure private peering" in the peering configuration options.
3. **Configure BGP:** Establish Border Gateway Protocol (BGP) routing between your on-premises network and the Microsoft cloud. This involves exchanging routing information to ensure proper traffic flow. BGP configuration details will vary depending on your specific network equipment.
4. **Connect your virtual networks:** Once the ExpressRoute circuit and peering are set up, link your Azure virtual networks to the private peering connection. This enables private communication between your on-premises resources and your virtual networks in Azure.

**Additional Considerations:**

* **High Availability:** Design your ExpressRoute circuit with redundancy to avoid a single point of failure. Consider provisioning circuits with different connectivity providers or location.
* **Cost Optimization:** Evaluate your traffic patterns and requirements to determine the most cost-effective ExpressRoute option. Global Reach and FastPath may incur additional charges.

### Configure Microsoft Peering

Microsoft peering allows direct private connectivity between your on-premises network and various Microsoft Cloud services like Azure AD, Office 365, and Dynamics 365. Here's how to configure it:

1. **Provision an ExpressRoute circuit:** Work with your connectivity provider to establish a circuit.
2. **Enable Microsoft peering in the circuit:** Within the Azure portal or Azure PowerShell, navigate to your ExpressRoute circuit and enable Microsoft peering.
3. **Configure Routing:** Advertise your on-premises network prefixes to Azure via Border Gateway Protocol (BGP). This informs Azure about the traffic routes for your network.

### Create and Configure an ExpressRoute Gateway

An ExpressRoute gateway manages the connection between your virtual network and the ExpressRoute circuit. Here's the process:

1. **Create an ExpressRoute gateway:** In the Azure portal or PowerShell, define a new ExpressRoute Gateway resource within your desired virtual network resource group.
2. **Link the ExpressRoute circuit:** Associate the previously created ExpressRoute circuit with the ExpressRoute gateway.
3. **Configure routing:** Similar to Microsoft peering, advertise your virtual network prefixes to the ExpressRoute circuit using BGP.

### Connect a Virtual Network to an ExpressRoute Circuit

Connecting a virtual network to the ExpressRoute circuit establishes the private communication path. Here's how to achieve it:

1. **Locate the virtual network:** Within the Azure portal or PowerShell, identify the virtual network you want to connect.
2. **Subnet association:** Choose the specific subnet(s) within the virtual network that require ExpressRoute connectivity.
3. **Link the ExpressRoute gateway:** Associate the ExpressRoute gateway you created earlier with the chosen subnet(s). This creates the connection.

### Recommend a Route Advertisement Configuration

BGP route advertisement dictates how traffic is routed between your on-premises network and Azure resources. Here's how to recommend a configuration:

* **Identify traffic types:** Analyze the traffic flow between your on-premises network and Azure services (Microsoft vs. internet-bound).
* **Specific vs. aggregate prefixes:** Decide whether to advertise specific prefixes for each subnet or use a more concise aggregate advertisement.
* **Redundancy:** Consider advertising prefixes to both primary and secondary connections within the ExpressRoute circuit for high availability.

**Additional Resources:**

* Microsoft official documentation offers in-depth explanations and step-by-step guides for each of these functionalities: https://learn.microsoft.com/en-us/training/modules/design-implement-azure-expressroute/
* You can find practice labs for these configurations to gain hands-on experience: https://github.com/microsoft/Deploy-and-Optimize-Azure-ExpressRoute-Private-Peering (search for "AZ-700 Labs")

### Configure Encryption over ExpressRoute

While ExpressRoute itself establishes a private connection, you can add an extra layer of security by encrypting the data traversing the connection. Here are two main options:

* **VPN within ExpressRoute:** You can configure a Point-to-Point (P2P) VPN tunnel within the ExpressRoute circuit. This encrypts data traffic between your on-premises network and Azure using protocols like IPSEC.
* **Azure Virtual WAN with ExpressRoute:** Azure Virtual WAN allows centralizing and managing network connectivity across various branches and cloud environments. You can integrate ExpressRoute circuits with Virtual WAN and leverage its built-in encryption capabilities.

### Implementing Bidirectional Forwarding Detection (BFD)

BFD is a fast convergence protocol that helps detect and recover from path failures within the ExpressRoute circuit. It continuously sends probes between your network and Microsoft's edge and quickly identifies any outages. By implementing BFD:

* You achieve faster failover times to secondary connections in case of path disruptions.
* BFD helps maintain high availability and resiliency for your Azure deployments.

### Diagnosing and Resolving ExpressRoute Connection Issues

Troubleshooting ExpressRoute connectivity issues requires a systematic approach. Here's a breakdown:

* **Verification:** First, verify basic configurations like circuit provisioning, peering setup, and routing policies. Tools like Azure portal or Azure Resource Manager can help with this.
* **Connectivity Checks:** Use tools like ping and traceroute to test connectivity between your on-premises network and Azure resources. Identify any bottlenecks or connectivity drops.
* **Azure ExpressRoute Diagnostics:** Leverage built-in diagnostics tools within Azure to analyze connectivity issues. These tools provide insights into BGP route propagation, peering status, and potential configuration problems.
* **Service Provider Involvement:** If the issue persists, it might be related to the connectivity provided by your service provider. Collaborate with your provider to isolate and address the problem within their network.

**Additional Resources:**

* Microsoft offers a comprehensive learning module on designing, implementing, and managing Azure ExpressRoute: https://learn.microsoft.com/en-us/training/modules/design-implement-azure-expressroute/

## Design and Implement an Azure Virtual WAN Architecture

### Selecting a Virtual WAN SKU

Azure Virtual WAN offers two SKUs:

* **Standard:** Provides full mesh connectivity between virtual WAN Hubs within a region. Ideal for scenarios with frequent communication between geographically distributed locations.
* **Basic:** Offers limited connectivity options. Best suited for simple, hub-and-spoke architectures with no need for inter-hub communication.

### Choosing the Right SKU depends on Your Needs

* **Number of connection locations:** Standard offers full mesh for better scalability across regions.
* **Inter-hub communication:** Standard allows communication between hubs, while Basic doesn't.
* **Cost:** Standard has a higher cost than Basic.

### Designing a Virtual WAN Architecture

Once you've chosen the SKU, define your architecture by considering these elements:

* **Connectivity types:**

  * **Branch connectivity:** Connect branch offices using Site-to-Site VPN (S2S VPN) or ExpressRoute for private connectivity.
  * **Remote user access:** Allow remote users to connect securely with Point-to-Site VPN (P2S VPN).
  * **Cloud connectivity:** Integrate Azure Virtual Networks (VNets) using VNet connections.

* **Security Services:** Leverage Azure Firewall within the Virtual WAN hub for centralized network security policies.

* **Routing:** Define routing policies to control traffic flow within the Virtual WAN.

### Creating a Hub in a Virtual WAN

Here's how to create a virtual WAN hub:

1. Login to the Azure portal and navigate to the Virtual WAN service.
2. Click "Create" and choose a name and resource group for your Virtual WAN.
3. Select the desired location for the hub (consider factors like latency and compliance).
4. Choose the SKU (Standard or Basic) based on your design requirements.
5. Review and create the virtual WAN hub.

### Choosing Appropriate Scale Units for Gateways

* **Site-to-Site Gateway:**

  * **Basic:** Suitable for low-bandwidth scenarios (up to 1 Gbps) with limited connections.
  * **Standard:** Ideal for most deployments, offering up to 10 Gbps throughput and supporting more connections.
  * **High Performance:** Best for high-bandwidth requirements (up to 20Gbps) and critical connections.

* **ExpressRoute Gateway:**

  * **Standard:** Supports up to 2 Gbps throughput for private connectivity to Azure.
  * **ExpressRoute Global Reach:** Extends connectivity across different Azure regions for geographically dispersed resources.

### Deploying a Gateway into a Virtual WAN Hub

1. Access the Azure portal and navigate to Virtual WANs.
2. Select your Virtual WAN and then go to **Gateways**.
3. Choose the desired gateway type (Site-to-Site VPN or ExpressRoute) and size (scale unit).
4. Configure the gateway settings based on your specific needs (e.g., VPN device IP address for Site-to-Site).
5. Click **Create** to deploy the gateway into your Virtual WAN Hub.

### Configuring Virtual Hub Routing

Azure Virtual WAN offers two routing methods:

* **Basic:** Automatic routing for basic scenarios with limited spokes.
* **Custom:** Manual configuration for more granular control over traffic flow.

### Custom Routing with User-Defined Routes (UDRs)

1. Navigate to your Virtual WAN hub.
2. Go to **Routing** and select **User-defined routes**.
3. Click **Add** to create a new route.
4. Specify the destination prefix (traffic target), next hop type (internet, VNet, etc.) and next hop address (Where to send traffic).
5. Define the priority for this route (higher values take precedence).
6. Click **Save** to create the UDR.

### Integrating a Third-Party NVA for Cloud Connectivity

Azure Virtual WAN allows integrating a third-party Network Virtual Appliance (NVA) for advanced security or functionality within the Virtual Hub. Here's a general process:

1. Deploy a VNet in your Virtual WAN hub where you want the NVA.
2. Deploy your desired third-party NVA solution in the VNet.
3. Configure peering between the VNet with the NVA and the VNets requiring NVA services.
4. Set up routing to direct traffic through the NVA for specific security policies or functionalities.

**Additional Resources:**

* Microsoft Documentation on Azure Virtual WAN: https://learn.microsoft.com/en-us/training/modules/introduction-azure-virtual-wan/
* Hub-Spoke Network Topology with Azure Virtual WAN: https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/hub-spoke-vwan-architecture
* SD-WAN connectivity architecture with Azure Virtual WAN: https://learn.microsoft.com/en-us/azure/virtual-wan/sd-wan-connectivity-architecture
