# Design and Implement Application Delivery Services

## Design and i=Implement Azure Load Balancer and Azure Traffic Manager

### Mapping Requirements to Azure Load Balancer Features

* **Hight Availability:** Load Balancer distributes traffic across healthy VMs in a pool, ensuring service remains available even if one VM fails.
* **Scalability:** It automatically scales resources up or down on traffic volume.
* **Health Monitoring:** Continuously monitors VM health and removes unhealthy ones from the pool.
* **Load Distribution:** Balances traffic across VMs using various methods (round robin, weight, etc.).
* **Security:** Supports inbound and outbound traffic rules for granular control.

### Identifying Use Cases for Azure Load Balancer

* **Web applications:** Distributes traffic across web servers for hight availability and performance.
* **Multi-tier applications:** Balances traffic between application tiers (web, database) for scalability.
* **Stateful applications:** Manages sessions for applications requiring session persistence.
* **Backend workloads:** Balances traffic to services like VMs, container instances, or Azure App Service.

### Choosing an Azure Load Balancer SKU and Tier

* **Basic SKU:** Cost-effective option for simple scenarios with limited throughput needs.
* **Standard SKU:** Offers higher throughput, availability zones for redundancy, and outbound rules.
* **Global Standard SKU (Public Preview):** Extends Standard SKU's capabilities across global regions for geographically distributed applications.

### Tier Selection

* **Basic Tier:** Ideal for development and testing environment.
* **General Purpose Tier:** Suitable for most production deployments.

### Public vs. Internal Load Balancers

* **Public Load Balancer:** Exposes your application to the public internet via a public IP address.
* **Internal Load Balancer:** Only accessible within the Azure virtual network, not exposed publicly.

Choose a public load balancer for applications users access from the internet. Use internal load balancers for applications within your virtual network that don't require external access.

### Regional vs. Global Load Balancers

* **Regional Load Balancer:** Distributes traffic within a single Azure region.
* **Global Load Balancer (Public Preview)**: Distributes traffic across multiple Azure regions based on routing methods (geographic, performance).

Use a regional load balancer for applications primarily used within a specific region. Consider a global load balancer for geographical distributed deployments or to improve user experience by routing requests to the closest healthy endpoint.

**Additional Resources:**

* Microsoft Azure documentation: https://learn.microsoft.com/en-us/azure/load-balancer/
* Microsoft Azure Traffic Manager: https://learn.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview
* Load-balancing options in Azure: https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview

### Azure Load Balancer

* **What it is:** A layer 4 load balancer that distributes traffic across healthy backend servers (VMs, App Services, etc.) based on pre-defined rules.
* **Benefits:**

  * High availability: Ensure application uptime even if a server fails.
  * Scalability: Distributes traffic for increased capacity.
  * Performance: Optimizes traffic flow for faster response times.

* **Configuration:**

  * Define a load balancing pool containing backend servers.
  * Choose a health probe to monitor server health (e.g., ping, HTTP).
  * Select a load distribution method (e.g., round robin, least connections).
  * Create inbound NAT rules to map public ports to backend ports.

### Azure Traffic Manager (ATM)

* **What it is:** A DNS-based traffic routing service that directs users to the most optimal backend service based on your chosen policy.
* **Benefits:**

  * Global load balancing: Routes traffic to geographically closest endpoints.
	* High availability: Fails over to healthy endpoints if a region becomes unavailable.
	* Performance optimization: Improves user experience by minimizing latency.

* **Routing methods:**

  * Priority: Directs traffic to the highest priority endpoint.
	* Weighted round robin: Distributes traffic with weight assigned to each endpoint.
	* Geographic: Routes users to the closest endpoint based on their location.
	* Performance: Routes users to the endpoint with the fastest response times.
	* Multi-value: Return multiple healthy endpoints for further client-side selection.

### Gateway Load Balancer (Azure Front Door)

**WHile not explicitly mentioned in teh AZ-700 objectives, it's important to understand the difference between Application Gateway and Azure Front Door (AFD):**

* **Application Gateway:** A layer 7load balancer that operates at the application layer (HTTP/HTTPS). Offers advanced features like content routing, SSL offloading, and web application firewall (WAF) capabilities.
* **Azure Front Door (AFD):** A global content delivery network (CDN) that provides layer 7 load balancing, web application acceleration, and security features. Ideal for highly scalable and geographically distributed web applications.

### Azure Load Balancer

* **Layer 4 Load Balancing:** Operates at the transport layer, distributing traffic based on IP addresses and ports.
* **Implementation:**

  * **Load Balancing Rule:** Defines how traffic is distributed to backend resources (VMs, App Services). You can choose distribution methods like round robin or based on health probes.
	* **Inbound NAT Rules:** Allow external traffic to reach backend resources with private IP addresses using public ports. You map a public port on the load balancer to a specific port ont he backend VM.
	* **Outbound NAT Rules:** Configure outbound connections initiated from backend resources. You can use:

	    * **Dynamic Outbound NAT (DNAT):** Assigns a public IP address from a pool to backend VMs for outbound connections.
			* **Static Outbound NAT (SNAT):** Maps a public IP address to a specific backend VM from consistent outbound communication.

### Azure Traffic Manager

* **DNS-Based Load Balancing:** Operates at the DNS layer, directing traffic to geographically distributed endpoints based on user location or other routing methods.
* **Implementation:**

  * Create a Traffic Manager profile and specify the endpoints (web apps, VMs, etc.) you want to route traffic to.
	* Choose a traffic routing method:

	  * **Priority:** Route traffic to the highest priority endpoint.
		* **Performance:** Route traffic to the endpoint with the fastest response time.
		* **Geographic:** Route traffic to the endpoint closest to the user's location.
    * **Weighted Round Robin:** Distribute traffic across endpoints with weight assignments.
		* **Multi-value:** Return multiple healthy endpoints for applications with availability needs.

**Resources for further learning:**

* Microsoft Documentation:
  * Azure Load Balancer: https://learn.microsoft.com/en-us/azure/load-balancer/
  * Azure Traffic Manager: https://learn.microsoft.com/en-us/azure/traffic-manager/

## Design and implement Azure Application Gateway

### Mapping Requirements to Features

**Azure Application Gateway (AG)** is a Layer 7 load balancer that manages traffic for your web applications in Azure. Here's how to map your requirements to its features:

  * **High Availability & Scalability:** AG distributes traffic across multiple backend servers (pools) for redundancy and handles high traffic volumes.
	* **Application Routing:** Route traffic based on URL paths, host headers, or other HTTP(S) request attributes for intelligent traffic management.
	* **Web Application Firewall (WAF):** (Available in specific SKUs) Protects your applications from common web attacks.
	* **SSL/TLS Termination:** Offloads SSL processing from backend servers, improving performance and security.
	* **Performance Optimization:** Features like caching and compression can improve application responsiveness.

**Identify your needs:**

* Do you need Layer 7 routing for your web application?
* Do you require high availability and scalability?
* Is web application security a concern?
* Would SSL termination benefit your application architecture?

**Match needs with features:** If your requirements align with these features, AG is a good choice.

### Use Cases for Azure Application Gateway

Here are some common use cases for AG:

* **Load Balancing:** Distributing traffic across multiple web servers for high availability and scalability.
* **Web Application Traffic Management:** Routing traffic based on complex rules for different application versions or functionalities.
* **Web Application Firewall (WAF):** Protecting your web applications from common attacks (with specific SKUs)
* **SSL Offloading:** Improving performance and security by terminating SSL/TLS connections at the gateway.
* **Microservices Architecture:** Managing traffic flow between microservices based on specific routing rules.

Consider AG if you need any of these functionalities in your Azure environment

### Manual vs. Autoscale

AG offers two backend pool capacity options:

* **Manual Scaling:** You define the number of backend servers in the pool and manage scaling manually.
* **Autoscaling:** AG automatically scales the backend pool based on predefined metrics like CPU or memory usage.

Choose manual scaling for predictable traffic patterns.

Use autoscaling for dynamic workloads with fluctuating traffic to optimize resource utilization and cost.

### Creating a Backend Pool

A backend pool is a group of backend servers (VMs, App SErvices, etc.) that AG distributes traffic across. Here's how to create one.

1. Navigate to your Application Gateway resource in the Azure portal.
2. Go to Backend pools section.
3. Click Add.
4. Provide a name for the pool.
5. Choose the Add a target option and select the type of backend resource (VM, App Service, etc.).
6. Specify the target resources (IP addresses or URIs) you want to add to the pool.
7. Click ok

You can add/remove backend servers from the pool as needed.

### Configuring Health Probes

Health probes monitor the health of backend servers in a pool. Unhealthy servers are removed from traffic distribution. Here's how to configure probes:

1. Navigate to your Application Gateway in Azure portl.
2. GO to Health probes section.
3. Click Add.
4. Choose the probe type (e.g., HTTP, TCP).
5. Define the probe target (path, port) and interval.
6. Set the unhealthy threshold (consecutive failures before marking unhealthy)
7. Clock OK to create the probe.

### Listeners

* Listeners are virtual entities that accept incoming traffic on specific ports and protocols (HTTP/HTTPS)
* You define a listener with a name, frontend IP configuration (public/private), port, and protocol.
* An Application Gateway can have multiple listeners to handle different types of traffic.

### Routing Rules

* Routing rules determine how incoming requests are forwarded to backend resources based on conditions.
* You define conditions using a combination of factors like:

  * Path (URL): Route traffic based on specific paths within the URL.
	* Host Header: Route traffic based on the hostname in the request header.
	* Headers: Route based on custom headers present int he request.

* Routing rules point to a specific backend pool containing the target resources.

### HTTP Settings

* HTTP settings define how Application Gateway handles the HTTP aspects of communication.
* You can configure:

  * **Pick-for-sticky sessions:** Maintain user sessions on a specific backend server for a consistent experience.
	* **Caching:** Enable caching of static content to improve performance.
	* **URL rewrite:** Modify request or response URLs on the fly (more on rewrite sets later).
	* **Timeouts:** Set timeouts for backend server responses to prevent hanging requests.

### Transport Layer Security (TLS)

* TLS encrypts communication between the internet and your Application Gateway for secure access.
* You can configure:

  * **TLS/SSL certificates:** Upload your public SSL certificate to terminate TLS at the Gateway.
	* **Client certificates:** Require client certificates for mutual authentication (optional).
	* **Cipher suites:** Choose the supported TLS cipher suites for secure connections.

### Rewrite Sets

* Rewrite sets allow you to modify request or response URLs based onp redefined rules.
* You can define rewrite sets with:

  * Search string: The part of the URL you want to modify.
	* Replace string: The value to replace the search string with.
	* Regular expressions: Use advanced patterns for complex URL manipulations.

* Rewrite sets are referenced in HTTP settings to achieve URL modifications.

**Resources for further learning:**

Microsoft Documentation: https://learn.microsoft.com/en-us/training/modules/configure-azure-application-gateway/
Tutorial: Create application gateway with multiple websites: https://learn.microsoft.com/en-us/azure/application-gateway/create-multiple-sites-portal

## Design and Implement Azure Front Door

### Mapping Requirements to Features

* **Performance:** AFD utilizes Microsoft's global edge network, bringing content closer to users for faster load times. Features like:

  * **Anycast Routing:** Directs traffic to the nearest edge location.
	* **Health Probing:** Monitors backend health and automatically fails over to healthy origin.

* **Security:** AFD bolsters your application's security with:

  * **Web Application Firewall (WAF):** Filters out malicious traffic.
	* **HTTPS Offloading:** Improves performance by handling SSL encryption/decryption at the edge.

* **Scalability:** AFD seamlessly scales to handle traffic spikes.
* **Global Reach:** Delivers content globally with numerous Points of Presence (PoPs).

### Identify Use Cases

AFD shines in scenarios like:

* **Globally distributed web applications:** Ensures optimal user experience across regions.
* **Static content delivery:** Delivers static content like images and scripts efficiently.
* **API Gateway:** Provides a single entry point for APIs and enforces access control.

### Choosing an Appropriate Tier

AFD Offers two tiers:

* **Standard:** Cost-effective option for moderate traffic and basic features.
* **Premium:** Ideal for high-traffic scenarios and advanced features like custom routing rules.

### Configuring Azure Front Door

Here's a breakdown of the configuration process:

* **Front Door Profile:** Create a profile specifying the resource group and location.
* **Frontend Hosts:** Define custom domain names for users to access your application.
* **Routing Rules:** Specify how AFD routes traffic to backend origins. Options include:

  * **Weighted Round Robing:** Distributes traffic evenly across healthy origins.
	* **Priority Routing:** Directs traffic to higher-priority origins first.
	* **Path-Based Routing:** Routes traffic based on URL paths.

* **Origins:** Add your backend servers (App Services, Azure Functions,etc.) as origins.
* **Health Probes:** Configure probes to monitor origin health and trigger failover.

### Configuring SSL Termination and Encryption

AFD can handle SSL termination, decrypting HTTPS traffic at the edge and improving performance. You can also configure end-to-end encryption by installing certificates on your backend servers and AFD.

**Additional Resources:**

* Microsoft Documentation: https://learn.microsoft.com/en-us/azure/frontdoor/
* Quickstart: Create an Azure Front Door profile: https://learn.microsoft.com/en-us/azure/frontdoor/create-front-door-portal

### Azure Front Door

Azure Front Door (AFD) is a cloud-based content delivery network (CDN) and global load balancer service that accelerates content delivery and distribution for your web applications and APIs. It improves performance, scalability, and security by strategically caching content at geographically distributed edge locations (Points of PResence or PoPs) closer to your users.

### Configuring Caching

AFD officers various caching options to optimize content delivery:

* **Caching Rules:** Define which content types (e.g., HTML, images, JavaScript) and URLs should be cached for a specific duration.
* **TTL (Time-to-live):** Set the expiration time for cached content, controlling how often it's refreshed from the origin server.
* **Cache Behavior:** Specify cache behavior for specific scenarios (e.g., query strings, dynamic content).

### Benefits fo Caching

* **Reduced Latency:** Users receive content from the nearest PoP, minimizing travel time.
* **Reduced Origin Load:** ORigins are relieved of serving frequently accessed content.
* **Improved Scalability:** AFD handles increased traffic efficiently.

### Configuring Traffic Acceleration

* **HTTP/2:** Users faster protocol compared to HTTP/1.1 for efficient communication.
* **TCP Offloading:** Handles TCP connections on the edge servers, freeing up origin resources.
* **TLS Offloading:** Performs encryption/decryption at the edge, further optimizing origin performance.
* **Origin COnnection Multiplexing:** Establishes multiple connections to origins for faster data transfer.

### Benefits of Traffic Acceleration

* **Faster Page Load Times:** USers experience quicker content delivery.
* **Improved User Experience:** Enhances website responsiveness and engagement.

### Implementing Rules, URL Rewrite, and URL Redirect

AFD' Routing engine allows you to define rules for managing traffic flow and content delivery:

* **Routing Rules:** Based on factors like HTTP method, URL path, or hostname, determine which backend to route a request to.
* **URL Rewrite:** Modify the request URL path before forwarding it to the origin server (e.g, SEO-friendly URLs).
* **URL Redirect:** Permanently (301) or temporarily (302) redirect users to a different URL based on conditions.

### Benefits of RUles, URL Rewrite, and URL Redirect

* **Traffic Management:** Granular control over how traffic is directed to backends.
* **SEO Optimization:** Enhance search engine ranking with URL rewrites.
* **User Experience:** Provide seamless redirects for content updates or maintenance.

### Securing an Origin with Azure Private Link

Azure Private Link enables private, secure connections between your web app or service in a virtual network (vNet) and AFD without trafersing the public internet. This isolation protects against unauthorized access.

### Benefits of Azure Private Link:

  * **Enhanced Security:** Traffic flows over a secure, dedicated Azure network connection.
	* **Improved Control:** Limits exposure of application endpoints to only authorized resources.
	* **Compliance:** Adheres to stricter data security regulations.

### Additional Considerations

* **Health Probes:** AFD monitors backend health using health probes (e.g., HTTP/TCP) to ensure
traffic is directed to healthy origins.
* **WAF Integration:** Integrate Azure Web Application Firewall (WAF) with AFD for additional
protection against malware and common attacks.
* **Monitoring and Logging:** Monitor AFD performance metrics and logs for troubleshooting
and optimization.
