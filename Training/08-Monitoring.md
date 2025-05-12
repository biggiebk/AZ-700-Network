# Monitoring

Metrics - numerical values that describe some aspect of a system at a point in time. They are lightweight and capable of supporting near real-time scenarios

Logs - contain different kinds of data organized into records with different sets of properties for ech type. Telemetry (events, traces) and performance data can be combined for analysis

Data sources

- Resources
- Subscriptions
- Tenant
- Application
- Operating systems
- Custom sources

## Azure Monitor

- Insights
	- app
	- container
	- VM
	Monitoring solutions
- Integrate
	- Logic apps
	-
- Visualize
- Analyze
- Respond

## Network Metrics

- Each service has individual metrics
- More metrics as you add/enable services

### Azure Monitor Network Insights

- Network health and metrics
- Connectivity
- Traffic
- Diagnostic Toolkit
	- Network Watcher

#### Network Watcher

- Regional service
- IP Flow Verify diagnoses connectivity issues
- Next Hop determines if traffic is being correctly routed
- VPN Diag troubleshoots gateways and connections
- NSG Flow Logs maps IP traffic through a network security group
- Connection troubleshooting shows connectivity between source VM and destination
- Topology generates a visual diagram of resources
	- Provides a visual representation of your networking elements
	- View all the resources in a virtual network, resource to resource associations, and relationships between the resources
	- The NetworkWatcher instance is in the same region as the virtual network

#### Connection Monitor

- Check network conn between 2 VMs
- Compare cross-region network latencies
- COmpare the latencies of the on prem site to the latencies of the azure app
- 

#### IP Flow Verify

- Checks if a packet is allowed or denied to or from a virtual machine

#### NSG Diagnostics

- Used to understand which traffic flows will be allowed or denied in your VNet
- TOol outputs whether traffic was allowed or denied
- Outputs NSG rules that were evaluated for the specified flow
- Detail information for debugging

#### Next Hop

- Determines whether traffic is being directed to the intended destination by showing the next hop

#### Effective security routes

- Details teh effective security Rules (inbound and outbound) of the network interface card of a Virtual machine

#### VPN Troubleshoot

- Troubleshoots gateways and connections
- Provides summary information and detailed information
- Can troubleshoot multiple gateways or connections simultaneously

#### Packet Capture

- Captures inbound and outbound traffic from a Virtual Machine
- Saves data to a storage account, file, or both

https://learn.microsoft.com/azure/network-watcher/packet-capture-overview

#### Connection Troubleshoot

- Check connectivity between source VM and destination
- Identify configuration issues that are impacting reachability
- Provide all possible hop by hop paths from the source destination
- Review hop by hop latency - min, max, and avg between source and destination
- View a graphical topology from your source to destination

#### Flow Logs

- View information about ingress and egress IP traffic through NSG
- FLow logs are written JSON format and show outbound and inbound flows on a per rule basis
- Json format can be visually displayed in Power BI or third party tools like Kibana

#### Traffic Analytics

- Networks security Groups
- Network security group flow logs
- log analytics
- log analytics workspace
- Network Watcher

