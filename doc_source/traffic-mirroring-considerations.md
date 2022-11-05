# Traffic Mirroring considerations<a name="traffic-mirroring-considerations"></a>

## General<a name="traffic-mirroring-considerations-general"></a>
+ You can only create a traffic mirror session if you are the owner of the source network interface or its subnet\. 
+ We recommend using either a Network Load Balancer or a Gateway Load Balancer endpoint as a target for high availability\. 
+ An elastic network interface cannot be a traffic mirror target and a traffic mirror session source at the same time\. 
+ When you delete a network interface that is a traffic mirror source, the traffic mirror sessions that are associated with the source are automatically deleted\. 
+ Flow logs do not capture mirrored traffic\. 
+ You might experience out\-of\-order delivery of mirrored packets when you use a Network Load Balancer or Gateway Load Balancer endpoint as your traffic mirror target\. If your monitoring appliance cannot handle out\-of\-order packets, we recommend using an elastic network interface as your traffic mirror target\. 

## Routing and security group rules evaluation<a name="traffic-mirroring-considerations-routing"></a>
+ Encapsulated mirror traffic is routed by using the VPC route table\. Make sure that your route table is configured to send the mirrored traffic to the correct traffic mirror target\. 
+ Mirrored outbound traffic from a source instance is not subject to security group evaluation\.
+ Packets that are dropped at the traffic mirror source by security group rules or by network ACL rules are not mirrored\.

## MTU<a name="traffic-mirroring-considerations-mtu"></a>
+ We truncate the packet to the MTU value when both of the following are true:
  + The traffic mirror target is a standalone instance\.
  + The mirrored traffic packet size is greater than the traffic mirror target MTU value\.

    For example, if an 8996 byte packet is mirrored, and the traffic mirror target MTU value is 9001 bytes, the mirror encapsulation results in the mirrored packet being greater than the MTU value\. In this case, the mirror packet is truncated\. To prevent mirror packets from being truncated, set the traffic mirror source interface MTU value to 54 bytes less than the traffic mirror target MTU value for IPv4 and 74 bytes less than the traffic mirror target MTU value when you use IPv6\. Therefore, the maximum MTU value supported by Traffic Mirroring with no packet truncation is 8947 bytes\. For more information about configuring the network MTU value, see [ Network Maximum Transmission Unit \(MTU\) for Your EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

## Traffic bandwidth and prioritization<a name="traffic-mirroring-bandwidth-routing"></a>
+ Mirrored traffic counts toward instance bandwidth\. For example, if you mirror a network interface that has 1 Gbps of inbound traffic and 1 Gbps of outbound traffic, the instance must handle 4 Gbps of traffic \(1 Gbps inbound, 1 Gbps mirrored inbound, 1 Gbps outbound, and 1 Gbps mirrored outbound\)\.
+ Production traffic has a higher priority than mirrored traffic when there is traffic congestion\. As a result, mirrored traffic is dropped when there is congestion\.
+ By default, each Gateway Load Balancer endpoint can support a bandwidth of up to 10 Gbps per Availability Zone and automatically scales up to 100 Gbps\. For more information, [AWS PrivateLink quotas](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-limits-endpoints.html) in the *AWS PrivateLink Guide*\.

## Network Load Balancer<a name="traffic-mirroring-considerations-nlb"></a>

The following rules apply when the Traffic Mirroring is a Network Load Balancer
+ There must be UDP listeners on port 4789\. 
+ If you do not have UDP listeners on the Network Load Balancer, you can still use the Network Load Balancer as a target\. However, Traffic Mirroring cannot occur because there are no UDP listeners\.
+ If you remove the UDP listeners from a Network Load Balancer that is a traffic mirror target, Traffic Mirroring fails without an error indication\. 
+ When the Network Load Balancer removes the node in an Availability Zone from the DNS table, Traffic Mirroring continues to send the mirrored packets to that node\.
+ When you have an existing Network Load Balancer which is a traffic mirror target and you add additional subnets to it, there is no effect\. For example, mirrored traffic in the Availability Zone of the new subnet is not routed in the same Availability Zone unless you enable cross\-zone load balancing\. 
+ We recommend that you use cross\-zone load balancing with your Network Load Balancer to ensure that the packets continue to be mirrored when all targets in an Availability Zone are not healthy\. For more information, see [Availability Zones and load balancer nodes](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#availability-zones) in the *Elastic Load Balancing User Guide*\.

## Gateway Load Balancer<a name="traffic-mirroring-considerations-glb"></a>

The following rules apply when the Traffic Mirroring is a Gateway Load Balancer
+ A listener for Gateway Load Balancers listens for all IP packets across all ports, and forwards traffic to the target group that you select\. 
+ The maximum MTU supported by the Gateway Load Balancer is 8500\. VPC Traffic Mirroring adds 54 bytes of additional headers to the original packet payload when using IPv4, and 74 bytes when using IPv6\. Therefore, the maximum packet size that can be delivered to the appliance without truncation is `8500 – 54 = 8446` when using IPv4, or `8500 – 74 = 8426` when using IPv6\. 
+ You can use the `BytesProcessed` and `PacketsDropped` CloudWatch metrics for Amazon VPC endpoints to monitor the volume of traffic being sent over the Gateway Load Balancer endpoint\. You can also use existing Amazon VPC Traffic Mirroring CloudWatch metrics for traffic volumes for each of the traffic mirroring sessions\. For more information, see [Monitor mirrored traffic using Amazon CloudWatch](traffic-mirror-cloudwatch.md)\. 
+ If all of the Gateway Load Balancer targets in an Availability Zone becomes unhealthy, the mirrored traffic can still be sent to traffic mirror targets in other zones\. In this case, cross\-zone load balancing to allow the Gateway Load Balancer to forward the mirrored traffic to a healthy target in another zone\. For more information, see [Gateway Load Balancer target failure scenarios](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/health-checks.html#failure-scenarios) in the *User Guide for Gateway Load Balancers*\.