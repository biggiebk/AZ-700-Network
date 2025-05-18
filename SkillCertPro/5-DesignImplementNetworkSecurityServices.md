# Design and Implement Azure Network Security Services

## Table of Contents

1. [Implement and Manage Network Security Groups](#implement-and-manage-network-security-groups)
   - [Network Security Groups (NSGs) in Azure](#network-security-groups-nsgs-in-azure)
   - [Creating a Network Security Group (NSG)](#creating-a-network-security-group-nsg)
   - [Associating an NSG to a Resource](#associating-an-nsg-to-a-resource)
   - [Alternative Approach with NSGs](#alternative-approach-with-nsgs)
   - [Security Rules NSGs](#security-rules-nsgs)
   - [Best Practices for NSGs](#best-practices-for-nsgs)
   - [Create and Configure NSG Rules](#create-and-configure-nsg-rules)
   - [Interpret NSG Flow Logs](#interpret-nsg-flow-logs)
   - [Validate NSG Flow Rules](#validate-nsg-flow-rules)
   - [Verify IP Flow](#verify-ip-flow)
   - [Configure an NSG for Remote Server Administration](#configure-an-nsg-for-remote-server-administration)

2. [Design and Implement Azure Firewall and Azure Firewall Manager](#design-and-implement-azure-firewall-and-azure-firewall-manager)
   - [Mapping Requirements to Azure Firewall Features](#mapping-requirements-to-azure-firewall-features)
   - [Selecting an Azure Firewall SKU](#selecting-an-azure-firewall-sku)
   - [Consider these factors when choosing a SKU](#consider-these-factors-when-choosing-a-sku)
   - [Designing an Azure Firewall Deployment](#designing-an-azure-firewall-deployment)
   - [Creation and Implementing an Azure Firewall Deployment (Hands-on Labs)](#creation-and-implementing-an-azure-firewall-deployment-hands-on-labs)
   - [Configure Azure Firewall Rules](#configure-azure-firewall-rules)
   - [Creating and Implementing Azure Firewall Manager Policies](#creating-and-implementing-azure-firewall-manager-policies)
   - [Creating a Secure Hub with Azure Firewall](#creating-a-secure-hub-with-azure-firewall)

3. [Design and Implement a Web Application Firewall (WAF) Deployment](#design-and-implement-a-web-application-firewall-waf-deployment)
   - [Understanding WAF Features and Capabilities](#understanding-waf-features-and-capabilities)
   - [Designing a WAF Deployment](#designing-a-waf-deployment)
   - [Detection vs. Prevention Mode](#detection-vs-prevention-mode)
   - [Configuring Rule Sets for WAF on Azure Front Door](#configuring-rule-sets-for-waf-on-azure-front-door)
   - [Configuring Rule Sets for WAF on Application Gateway](#configuring-rule-sets-for-waf-on-application-gateway)
   - [Implementing a WAF Policy](#implementing-a-waf-policy)
   - [Associating a WAF Policy](#associating-a-waf-policy)

---

## Implement and Manage Network Security Groups

### Network Security Groups (NSGs) in Azure

NSGs are stateful firewalls that control inbound and outbound network traffic to Azure resources (virtual machines, web apps, etc.) within a virtual network (VNet). They function like security filters, allowing or denying traffic based on pre-defined rules.

### Creating a Network Security Group (NSG)

1. Access the Azure portal and navigate to your resource group
2. Click on "Network Security Groups" or search for it in the services blade.
3. Select **Add** to crate a new NSG.
4. Prove a name and location for your NSG.
5. (Optional) Add tags for organization purposes.
6. Click **create**.

### Associating an NSG to a Resource

There are tw primary methods for associating an NSG with a resource:

* **During Resource Creation:**

1. When creating a new resource (e.g., virtual machine), locate the **Networking** section.
2. Under **Security Group**, select the NSG you want to associate.
3. Proceed with creating the resource.

* **After Resource Creation:**

1. Go to the existing resource (e.g., virtual machine) in the Azure Portal
2. Navigate to the **Networking** section.
3. Under **Security Group** select the desired NSG.
4. Click **Save** to apply the association.

### Alternative Approach with NSGs

Since ASGs are no longer recommended, here's how to achieve similar functionality with NSGs:

1. Create an NSG with appropriate security rules for your application's specific needs.
2. Associate this NSG with the network interface cards (NICs) attached to the resources requiring application-level security.

### Security Rules NSGs

NSGs control traffic flow using security rules. Each rule specifies:

* **Direction:** Inbound (traffic entering the resource) or Outbound (traffic leaving the resource)
* **Priority:** Lower numbers execute first (important for allowing rules to take precedence over blocking rules)
* **Source:** IP address/ragne, Azure service tag (e.g., Internet) or another NSG
* **Destination:** Port or port range, IP address/range, or another NSG
* **Protocol:** TCP, UDP, ICMP, or any
* **Action:** Allow or Deny traffic matching the rule's criteria

### Best Practices for NSGs

* **Start with Deny-All Default Rule:** Create an initial rule with lowest priority (highest number) that denies all traffic. Subsequent rules can then allow specific traffic based onyour applications needs.
* **Least Privilege:** Grant only the minimum access required for your resources to function correctly.
* **Separate NSGs for Different Security Needs:** Consider crating distinct NSGs for various resource groups or tiers (e.g., web servers, databases) to enhance organization and control.
* **Document Your Rules:** Maintain clear documentation of yur NSG rules to simplify troubleshooting and future modifications.

### Create and Configure NSG Rules

* **Rules:** An NSG consists of security rules that define what traffic is allowed (permit) or denied (deny) to reach your resources. Each rule specifies:

  * **Direction:** Inbound (to) or outbound (from) the resource.
	* **Priority:** Lower numbered rules are evaluated first (important for allowing specific traffic before a deny-all rule)
	* **Protocol:** TCP, UDP,ICMP, or any
	* **Source:** IP address/range, Azure service tag (e.g, Internet) or virtual network,
	* **Destination Port:** Specific port (e.g., 80 for HTTP) or a range.
  * **Destination:** IP address/range or virtual network.

* **Creating Rules:** You can crate NSG rules through the Azure portal, Azure CLI, or Powershell. The specifics might differ slightly depending ont he chosen method.

### Interpret NSG Flow Logs

* **Flow Logs:** NSGs can generate flow logs that provide detailed information about network traffic flow. These logs include:

  * Rule that matched the traffic flow.
	* Source and destination IP addresses.
	* Ports used.
	* Bytes transferred.
	* FLow direction (inbound/outbound).

* **Interpreting Logs:** Flow logs help you troubleshoot connectivity issues, identify suspicious activity, and validate your NSG rules' effectiveness. You can analyze logs in Azure Monitor or export them for further analysis.

### Validate NSG Flow Rules

* **Testing Rules:** After creating NSG rules, it's essential to test them to ensure they function as intended. Here are some methods:

  * **NSG Simulator:** Azure provides an NSG simulator tool to test rules without impacting live traffic.
	* **Test VMs:** Create temporary VMs with specific configurations to test if traffic flows as expected.
	* **Flow Logs:** ANalyze flow logs to see if allowed traffic is flowing and denied traffic is blocked.

### Verify IP Flow

* **Verifying Connectivity:** Once you've configured NSG rules, verify that communication between resources is working as expected. You can use tools like ping or remote desktop to test connectivity.
* **Troubleshooting Issues:** If communication fails, analyze NSG rules, flow logs, and resource configuration to identify the cause.

### Configure an NSG for Remote Server Administration

* **Remote Access Scenarios:** There might be situations where you need remote access (RDP/SSH) to VMs secured by NSGs. Here are two options:

  * **Temporary Rule:** Crate a temporary NSG rule allowing RDP/SSH traffic from your public IP for a limited duration. This approach is less secure and should be used cautiously.

	* **Azure Bastion:** A more secure approach is to use Azure Bastion. It's a managed service that provides secure RDP/SSH access to VMs without exposing them to the public internet. Bastion uses jump boxes within the virtual network, eliminating the need for public IP addresses on VMs.

## Design and Implement Azure Firewall and Azure Firewall Manager

### Mapping Requirements to Azure Firewall Features

* **Identify Security Needs:** Before diving into Azure Firewall, clearly define your security requirements. Are you looking to control inbound/outbound traffic, filter specific protocols/ports, or integrate with advanced threat protection.
* **Matching Features:** Once you have your needs, map the m to Azure Firewall features. Here are some key capabilities:

  * **Network Traffic Filtering:** Allow or deny traffic based on source/destination IP addresses, ports, protocols, and application security groups.
	* **Next Generation Firewall (NGFW):** Inspect traffic for deep packet inspection (DPI) to identify malware and other threats.
	* **Stateful Inspection:** Track connections to ensure authorized communication.
	* **Dynamic ROuting:** Integrate with routing protocols (BGP) for secure communication between on-premises and Azure environments.
	* **High Availability:** Deploy firewalls in an active/passive configuration fore redundancy.

### Selecting an Azure Firewall SKU:

  * **WAF (Web Application Firewall):** Protects web applications from common attacks like SQL injection and cross-site scripting (XSS). Choose this if web application security is a primary concern.
	* **Standard:** Offers all core firewall functionalities like traffic filtering, stateful inspection, and basic threat protection. This is a good choice for general-purpose network security.
	* **Basic:** Provide basic traffic filtering and stateful inspection. Suitable for simpler deployments with limited security requirements.

### Consider these factors when choosing a SKU

* **Security Needs:** The level of threat protection required (WAF vs Standard vs Basic).
* **Performance:** Throughput and connection capacity needed for your network traffic.
* **Cost:** Pricing varies between SKUs

### Designing an Azure Firewall Deployment

Here's what to consider when designing your deployment

* **Network Topology:** Decide whether to use a hub-and-spoke architecture with Azure VIrtual WAN or individual virtual Networks.
* **Deployment Location:** Place firewalls strategically near your resources for optimal performance.
* **Hight Availability:** Configure firewalls in an active/passive configuration for redundancy.
* **Network Security Groups (NSGs):** Use NSGs to define granular security policies at the subnet or individual resource level.
* **Azure Firewall Manager (Optional):** For centralized management of multiple firewalls across subscriptions and regions.

### Creation and Implementing an Azure Firewall Deployment (Hands-on Labs)

**Resources:**

* Microsoft provides hands-on labs to practice deploying Azure Firewall. These labs walk you
through the creation and configuration process in the Azure portal:
"https://learn.microsoft.com/en-us/azure/firewall-manager/"
* Exam prep resources like "https://m.youtube.com/watch?v=WAkOkuaonBI" can also be
helpful for understanding the configuration steps.

### Configure Azure Firewall Rules

Azure Firewall acts as a stateful firewall, inspecting incoming and outgoing traffic based on a defined rule set. Here's how to configure these rules:

* **Service Rules:** Define allowed or denied traffic based on Azure services (e.g., allowing access to Storage but denying access to SQL Database).
* **Application Rules:** Granular control by specifying protocols, ports, and source/destination IP addresses or ranges.
* **NAT Rules:** Configure Network Address Translation (NAT) to manage outbound traffic visibility and source IP address presentation.

### Creating and Implementing Azure Firewall Manager Policies

Azure Firewall Manager simplifies managing multiple Azure Firewalls across subscriptions and regions. Here's how to create and implement policies:

* **Centralized Policy Management:** Define firewall policies with allowed/denied rules once and apply them to multiple firewalls for consistency.
* **Hierarchical Policies:** Create global policies for organization-wide security baselines and local policies for specific needs of individual teams or workloads.
* **Policy Inheritance:** Local policies inherit rules from global policies, allowing for customization while maintaining a core security posture.

### Creating a Secure Hub with Azure Firewall

A secure hub centralizes security policies for a Virtual WAN environment. Here's how to deploy Azure Firewall with a secure hub:

* **Deploy Azure FIrewall in a Secured Virtual Hub:** Create a secured virtual hub by deploying Azure Firewall within an Azure Virtual WAN hub. This simplifies traffic routing and policy application.
* **Route Automation:** Firewall Manager automates route configuration for secured virtual hubs, ensuring traffic flows through the firewall for inspection.
* **Centralized Security Management:** Manage security policies and settings for Azure Firewall within the secure hub from a central location using Firewall Manager.

**Additional Resources:**

* Microsoft Documentation:
  * [What is Azure Firewall?](https://learn.microsoft.com/en-us/azure/firewall/overview)
* Microsoft Azure Pricing:
  * [Azure Firewall Manager Pricing](https://azure.microsoft.com/en-in/pricing/details/firewall-manager/)

## Design and Implement a Web Application Firewall (WAF) Deployment

### Understanding WAF Features and Capabilities

Before deploying a WAF, it's crucial to understand its features and how the map to your security requirements. HEre's a breakdown of key aspects:

* **Rule sets:** These pre-configured rules identify and bloc common web application attacks like SQL injection, cross-site scripting (XSS), and common web vulnerabilities. Microsoft provides managed rule sets and allows customization for specific needs.
* **Pattern matching:** WAF can identify malicious patterns within HTTP requests and headers, helping to block attacks that exploit specific vulnerabilities.
* **IP reputation blocking:** Based on known malicious IP addresses, WAF can automatically block traffic from these sources.
* **Anomaly detection:** This feature analyzes traffic patterns and identifies deviations from normal behavior, potentially signaling attacks.
* **Rate limiting;** WAF can limit the number of requests from a single IP address to prevent denial-of-service (DoS) attacks.
* **Logging and alerting:** WAF logs all activity and can be configured to generate alerts for suspicious behavior, allowing for investigation and response.

### Designing a WAF Deployment

Here's what to consider when designing your WAF deployment:

* **Integration point:** CHoose where to integrate WAF. Azure offers options like Azure Application Gateway (AG) or Azure Front Door (AFD). AG provides advanced routing capabilities, while AFD focuses on global performance optimization.
* **High availability:** Ensure your WAF solution is highly available by deploying it across multiple regions or availability zones.
* **Scalability:** Design for future growth. WAF should scale to handle increasing traffic volumes.
* **Logging and monitoring:** Plan how you'll collect, analyze, an store WAF logs. Integrate with Security Information and Event Management (SIEM) tools for centralize visibility.

### Detection vs. Prevention Mode

WAF Offers two primary modes:

* **Detection Mode:** Logs suspicious activity but doesn't block traffic. Ths allows for analysis before taking action. Useful for initial deployment or for monitoring established rules.
* **Prevention Mode:** Actively blocks traffic that violates WAF rules. This is the recommended mode for production environments, but requires careful configuration to avoid unintended blocking of legitimate traffic.

### Configuring Rule Sets for WAF on Azure Front Door

Azure Front Door (AFD) integrates natively with WAF. Here's ow to configure rule sets:

1. Access your AFD profile in the Azure portal.
2. Navigate to the **WebApplicationFirewall** blade.
3. Select **Managed Rules** and choose the desired rule set (e.g., OSWAP 3.xCRS).
4. Optionally, configure custom rules for specific scenarios.
5. Define the detection or prevention mode for the rules.
6. Save your configuration.

**Additional Resources:**


* Microsoft documentation: https://learn.microsoft.com/en-us/azure/web-application-firewall/
* Design and Implement a Web Application Firewall WAF deployment - [Part 1 YouTube](https://www.youtube.com/watch?v=8YYHN_C1iEo)

### Configuring Rule Sets for WAF on Application Gateway

* **Rule Sets:** These are pre-defined collections of rules that identify and block malicoius traffic targeting web applications. Microsoft offers built-in rule sets like OWASP ModSEcurity Core Rule Set 3.3.0
* **Custom Rules:** YOu can create custom rules to address specific threats relevant to your application. These rules can be based on patterns in request headers, body content, or cookies.
* **Configuring Rules:**

  * **Match Conditions:** Define what triggers the rule (e.g., specific HTTP  methods, URL paths, or header values).
	* **Action:** Specify the action to take when a rule matches (e.g, Block the request, Log the request, or Redirect).

### Implementing a WAF Policy

* A WAF policy is a container of your configured rule sets. It defines how these rules are applied to your web applications.
* You can create different policies with varying rule sets for different applications depending on their security needs.
* Within a policy, you can define the order in which rule sets are evaluated. This helps prioritize certain check and ensure effective protection.

### Associating a WAF Policy

* Once you have a WAF policy with your desired rule sets, you need to associate it with your Application Gateway.
* This association tells the Application Gateway to enforce the rules defined in the policy on incoming traffic directed to your web applications.

**Additional Considerations:**

* **Logging:** Enable logging for your WAF policy to analy ze attacks and identify potential security breaches.
* **Monitoring:** Monitor the effectiveness of your WAF by reviewing logs and application health metrics.
* **Testing:** Regularly test your WAF rules to ensure they don't block legitimate traffic.

**Resources for further learning:**

* Microsoft documentation on Azure WAF: https://learn.microsoft.com/en-us/azure/web-application-firewall/
* Microsoft Cloud Academy course on AZ-700: [Microsoft Cloud Academy](https://azuremarketplace.microsoft.com/en/marketplace/apps/aad.cloudacademysso?tab=overview) (Subscription required)
* Video tutorial on WAF deployment for AZ-700: [YouTube](https://www.youtube.com/watch?v=B3O6bXu-NbM)
