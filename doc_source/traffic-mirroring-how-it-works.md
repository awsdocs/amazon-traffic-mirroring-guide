# How Traffic Mirroring Works<a name="traffic-mirroring-how-it-works"></a>

Traffic Mirroring copies inbound and outbound traffic from the network interfaces that are attached to your Amazon EC2 instances\. You can send the mirrored traffic to the network interface of another EC2 instance, or a Network Load Balancer that has a UDP listener\. The traffic mirror source and the traffic mirror target \(monitoring appliance\) can be in the same VPC\. Or they can be in a different VPC connected via intra\-Region VPC peering or a transit gateway\.

Consider the following scenario, where you want to mirror traffic from two sources \(Source A and Source B\) to a single traffic mirror target \(Target D\)\. The following procedures are required:
+ Identify the traffic mirror source \(Source A\)
+ Identify the traffic mirror source \(Source B\)
+ Configure the traffic mirror target \(Target D\)
+ Configure the traffic mirror filter \(Filter A\)
+ Configure the traffic mirror session for Source A, Filter A, and Target D
+ Configure the traffic mirror session for Source B, Filter A, and Target D

 After you create the traffic mirror session, any traffic that matches the filter rules is encapsulated in a VXLAN header\. It is then sent to the target\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring.png)

## Traffic Mirror Targets<a name="traffic-mirroring-targets"></a>

A *traffic mirror target* is the destination for mirrored traffic\. The traffic mirror target can be owned by an AWS account that is different from the traffic mirror source\.

Use any of the following resources for a traffic mirror target:
+ A network interface
+ A Network Load Balancer

A traffic mirror target can be used in more than one traffic mirror session\. Make sure to allow VXLAN traffic \(UDP port 4789\) from the traffic mirror source in the security groups that are associated with the traffic mirror target\.

### Network Load Balancer Considerations<a name="traffic-mirroring-targets-nlb"></a>

When the traffic mirror target is a Network Load Balancer, the following rules apply:
+ There must be UDP listeners on port 4789\.
+ If all of the Network Load Balancer traffic mirror targets in an Availability Zone become unhealthy, the mirrored traffic can still be sent to traffic mirror targets in other zones\. In this case, enable cross\-zone load balancing to allow the Network Load Balancer to forward the mirrored traffic to a healthy target in another zone\.

## Traffic Mirror Filters and Filter Rules<a name="traffic-mirroring-filters"></a>

A *traffic mirror filter* is a set of inbound and outbound traffic rules that define the traffic that is copied from the traffic mirror source, and sent to the traffic mirror destination\. By default, no traffic is mirrored\. To mirror traffic, add traffic mirror rules to the filter\. The rules that you add define what traffic gets mirrored\. You can also choose to mirror certain network services traffic, including Amazon DNS\. When you add network services traffic, all traffic \(inbound and outbound\) related to that network service is mirrored\. 

### Traffic Mirror Filter Rules<a name="traffic-mirroring-rules"></a>

Traffic mirror filter rules define what traffic gets mirrored\. You can define a set of parameters to apply to the traffic mirror source traffic to determine the traffic to mirror\. The following traffic mirror filter rule parameters are available:
+ Traffic direction: Inbound or outbound
+ Action: The action to take, either to accept or reject the packet
+ Protocol: The L4 protocol
+ Source port range
+ Destination port range
+ Source CIDR block
+ Destination CIDR block

## Traffic Mirror Sessions<a name="traffic-mirroring-sessions"></a>

A *traffic mirror* session establishes a relationship between a traffic mirror source and a traffic mirror target\.

A traffic mirror session contains the following resources:
+ A traffic mirror source
+ A traffic mirror target 
+ A traffic mirror filter

A given packet is only mirrored one time\. However, you can use multiple traffic mirror sessions on the same source\. This is useful if you want to send a subset of the mirrored traffic from a traffic mirror source to different tools\. For example, you can filter HTTP traffic in a higher priority traffic mirror session and send it to a specific monitoring appliance\. At the same time, you can filter all other TCP traffic in a lower priority traffic mirror session and send it to another monitoring appliance\.

Traffic mirror sessions are evaluated based on the ascending session number that you define when you create the session\. 

## Traffic Mirroring and VPC Flow Logs<a name="flow-log"></a>

You can use Traffic Mirroring and VPC Flow Logs to monitor your VPC traffic\. 

VPC Flow Logs allow customers to collect, store, and analyze network flow logs\. The Flow Logs capture information about the following:
+ Allowed and denied traffic
+ Source and destination IP addresses
+ Ports
+ Protocol number
+ Packet and byte counts
+ Action taken \(accept or reject\)

You can use VPC Flow Logs to troubleshoot connectivity and security issues, and to make sure that the network access rules are working as expected\.

Traffic Mirroring provides deeper insight into the network traffic by allowing you to analyze actual traffic content, including payload\. Traffic Mirroring is targeted for the following types of cases:
+ Analyzing the actual packets to perform a root\-cause analysis on a performance issue
+ Reverse\-engineering a sophisticated network attack
+ Detecting and stopping insider abuse or compromised workloads

## Traffic Mirror Source and Target Connectivity Options<a name="traffic-mirroring-connection"></a>

The traffic mirror source and the traffic mirror target \(monitoring appliance\) can be in:
+ The same VPC, or
+ A different VPC connected by using an intra\-Region VPC peering connection or a transit gateway\.

The traffic mirror target can be owned by an AWS account that is different from the traffic mirror source\. 

The mirrored traffic is sent to the traffic mirror target using the source VPC route table\. Before you configure Traffic Mirroring, make sure that the traffic mirror source can route to the traffic mirror target\. 

The following table describes the available resource configurations\. 


**Available Traffic Mirroring Configurations**  

| Source Owner | Source VPC | Source Type | Target Owner | Target VPC | Connectivity Option | 
| --- | --- | --- | --- | --- | --- | 
| Account A | VPC 1 | Network interface | Account A | VPC1 | No additional configuration | 
| Account A | VPC 1 | Network interface | Account A | VPC 2 | Intra\-Region peering or a transit gateway | 
| Account A | VPC 1 | Network interface | Account B | VPC 2 | Cross\-account Intra\-Region peering or a transit gateway | 
| Account A | VPC 1 | Network interface | Account B | VPC 1 | VPC sharing | 