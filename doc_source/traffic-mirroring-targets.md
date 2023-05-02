# Traffic mirror target concepts<a name="traffic-mirroring-targets"></a>

A *traffic mirror target* is the destination for mirrored traffic\. After you create a traffic mirror target, you add it to a traffic mirror session\. You can use a traffic mirror target in more than one traffic mirror session\. For more information, see [Traffic mirror session concepts](traffic-mirroring-sessions.md)\.

You can use the following resources as traffic mirror targets:
+ [Network interfaces](#traffic-mirroring-targets-eni) of type `interface`
+ [Network Load Balancers](#traffic-mirroring-targets-nlb)
+ [Gateway Load Balancer endpoints](#traffic-mirroring-targets-gwlb)

For high availability, we recommend that you use a Network Load Balancer or a Gateway Load Balancer endpoint as a mirror target\.

You might experience out\-of\-order delivery of mirrored packets when you use a Network Load Balancer or Gateway Load Balancer endpoint as your traffic mirror target\. If your monitoring appliance can't handle out\-of\-order packets, we recommend using a network interface as your traffic mirror target\.

A traffic mirror target can be owned by an AWS account that is different from the traffic mirror source\. For more information, see [Cross\-account targets](cross-account-traffic-mirroring-targets.md)\.

## Network interfaces<a name="traffic-mirroring-targets-eni"></a>

The following diagram shows a traffic mirror session where the traffic mirror target is a network interface for an EC2 instance\. Traffic Mirroring filters the traffic from the mirror source and sends the accepted mirrored traffic to the mirror target\.

![\[A traffic mirror session where the mirror target is an EC2 instance.\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/get-started.png)

## Network Load Balancer<a name="traffic-mirroring-targets-nlb"></a>

The following diagram shows a traffic mirror session where the traffic mirror target is a Network Load Balancer\. You install the monitoring software on the target instances, and then register them with the load balancer\. Traffic Mirroring filters the traffic from the mirror source and sends the accepted mirrored traffic to the load balancer\. The load balancer sends the mirrored traffic to the target instances\.

![\[A traffic mirror session where the mirror target is a Network Load Balancer.\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/nlb-target.png)

**Considerations**
+ Traffic mirroring can't occur unless the load balancer has UDP listeners on port 4789\. If you remove the UDP listeners, Traffic Mirroring fails without an error indication\.
+ To improve availability and fault tolerance, we recommend that you select at least two Availability Zones when you create the Network Load Balancer, and that you register target instances in each selected Availability Zone\.
+ We recommend that you enable cross\-zone load balancing for your Network Load Balancer\. If all registered target instances in an Availability Zone become unhealthy and cross\-zone load balancing is disabled, the load balancer can't send the mirrored traffic to target instances in another Availability Zone\. If you enable cross\-zone load balancing, the load balancer can send the mirrored traffic to healthy target instances in another Availability Zone\.
+ If you select an additional Availability Zone for your Network Load Balancer after you create it, Traffic Mirroring does not send mirrored traffic to target instances in the new Availability Zone unless you enable cross\-zone load balancing\.
+ When the Network Load Balancer removes a load balancer node from the DNS table, Traffic Mirroring continues to send the mirrored traffic to that node\.

## Gateway Load Balancer endpoints<a name="traffic-mirroring-targets-gwlb"></a>

The following diagram shows a traffic mirror session where the traffic mirror target is a Gateway Load Balancer endpoint\. The mirror source is in the service consumer VPC and the Gateway Load Balancer is in the service provider VPC\. You install the monitoring software on the target appliances, and then register them with the load balancer\. Traffic Mirroring filters the traffic from the mirror source and sends the accepted mirrored traffic to the Gateway Load Balancer endpoint\. The load balancer sends the mirrored traffic to the target appliances\.

![\[A traffic mirror session where the mirror target is a Gateway Load Balancer endpoint.\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/gwlbe-target.png)

**Considerations**
+ A listener for Gateway Load Balancers listens for all IP packets across all ports, and then forwards traffic to the target group\.
+ To improve availability and fault tolerance, we recommend that you select at least two Availability Zones when you create the Gateway Load Balancer, and that you register target appliances in each selected Availability Zone\.
+ If all registered target appliances in an Availability Zone become unhealthy and cross\-zone load balancing is disabled, the load balancer can't send the mirrored traffic to target appliances in another Availability Zone\. If you enable cross\-zone load balancing, the load balancer can send the mirrored traffic to healthy target appliances in another Availability Zone\.
+ The maximum MTU supported by the Gateway Load Balancer is 8500\. Traffic Mirroring adds 54 bytes of additional headers to the original packet payload when using IPv4, and 74 bytes when using IPv6\. Therefore, the maximum packet size that can be delivered to an appliance without truncation is `8500 – 54 = 8446` when using IPv4, or `8500 – 74 = 8426` when using IPv6\.
+ You can use the `BytesProcessed` and `PacketsDropped` CloudWatch metrics for VPC endpoints to monitor the volume of traffic being sent over the Gateway Load Balancer endpoint\. You can also use CloudWatch metrics for Traffic Mirroring\. For more information, see [Monitor mirrored traffic using Amazon CloudWatch](traffic-mirror-cloudwatch.md)\.

## VXLAN encapsulation<a name="traffic-mirroring-vxlan-encapsulation"></a>

Mirrored traffic is encapsulated in VXLAN packets and then routed to the mirror target\. The security groups for a traffic mirror target must allow VXLAN traffic \(UDP port 4789\) from the traffic mirror source\. The route table for the subnet with the traffic mirror source must have a route that sends the mirrored traffic to the traffic mirror target\. The monitoring software that you run on the mirror target must be able to process encapsulated VXLAN packets\.

## Routing and security groups<a name="traffic-mirroring-routing"></a>

Encapsulated mirror traffic is routed to the traffic mirror target by using the VPC route table\. Make sure that your route table is configured to send the mirrored traffic to the correct traffic mirror target\.

Mirrored outbound traffic from is not subject to security group evaluation\.

Packets that are dropped at the traffic mirror source by security group rules or by network ACL rules are not mirrored\.

## Processing mirrored traffic<a name="traffic-mirroring-targets-options"></a>

You can use open\-source tools or choose a monitoring solution available on [AWS Marketplace](http://aws.amazon.com/marketplace/)\. You can stream mirrored traffic to any network packet collector or analytics tool, without having to install vendor\-specific agents\.