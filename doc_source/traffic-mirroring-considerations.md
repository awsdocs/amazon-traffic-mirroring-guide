# Traffic Mirroring quotas and considerations<a name="traffic-mirroring-considerations"></a>

## Considerations<a name="traffic-mirroring-basics"></a>
+ Encapsulated mirror traffic is routed by using the VPC route table\. Make sure that your route table is configured to send the mirrored traffic to the correct traffic mirror target\. 
+ You can only create a traffic mirror session if you are the owner of the source network interface or its subnet\.
+ We truncate the packet to the MTU value when both of the following are true:
  + The traffic mirror target is a standalone instance\.
  + The mirrored traffic packet size is greater than the traffic mirror target MTU value\.

  For example, if an 8996 byte packet is mirrored, and the traffic mirror target MTU value is 9001 bytes, the mirror encapsulation results in the mirrored packet being greater than the MTU value\. In this case, the mirror packet is truncated\. To prevent mirror packets from being truncated, set the traffic mirror source interface MTU value to 54 bytes less than the traffic mirror target MTU value for IPv4 and 74 bytes less than the traffic mirror target MTU value when you use IPv6\. Therefore, the maximum MTU value supported by Traffic Mirroring with no packet truncation is 8947 bytes\. For more information about configuring the network MTU value, see[ Network Maximum Transmission Unit \(MTU\) for Your EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\. 
+ Mirrored traffic counts toward instance bandwidth\. For example, if you mirror a network interface that has 1 Gbps of inbound traffic and 1 Gbps of outbound traffic, the instance must handle 4 Gbps of traffic \(1 Gbps inbound, 1 Gbps mirrored inbound, 1 Gbps outbound, and 1 Gbps mirrored outbound\)\.
+ Production traffic has a higher priority than mirrored traffic when there is traffic congestion\. As a result, mirrored traffic is dropped when there is congestion\.
+ Mirrored outbound traffic from a source instance is not subject to security group evaluation\.
+ Flow logs do not capture mirrored traffic\.
+ When you delete a network interface that is a traffic mirror source, the traffic mirror sessions that are associated with the source are automatically deleted\.
+ Packets that are dropped at the traffic mirror source by security group rules or by network ACL rules are not mirrored\.
+ An elastic network interface cannot be a traffic mirror target and a traffic mirror session source at the same time\.
+ We recommend using a Network Load Balancer as a target for high availability\.
+ If you do not have UDP listeners on the Network Load Balancer, you can still use the Network Load Balancer as a target\. However, Traffic Mirroring cannot occur because there are no UDP listeners\.
+ If you remove the UDP listeners from a Network Load Balancer that is a traffic mirror target, Traffic Mirroring fails without an error indication\.
+ When the Network Load Balancer removes the node in an Availability Zone from the DNS table, Traffic Mirroring continues to send the mirrored packets to that node\. 
+  When you have an existing Network Load Balancer which is a traffic mirror target and you add additional subnets to it, there is no effect\. For example, mirrored traffic in the Availability Zone of the new subnet is not routed in the same Availability Zone unless you enable cross\-zone load balancing\.
+ We recommend that you use cross\-zone load balancing with your Network Load Balancer to ensure that the packets continue to be mirrored when all targets in an Availability Zone are not healthy\.

## Limitations<a name="traffic-mirroring-network-services"></a>

Traffic Mirroring is not available on the following EC2 instance types:
+ These current generation instances: C6gn, C6i, Im4gn, Is4gen, M6a, M6i, R6i, T2
+ Any bare metal instances
+ Any [previous generation instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes)

The following traffic types cannot be mirrored:
+ ARP
+ DHCP
+ Instance metadata service
+ NTP
+ Windows activation

## Quotas<a name="traffic-mirroring-limits"></a>

The following are the quotas for Traffic Mirroring for your AWS account\.

**Sessions**
+ Maximum number of sessions per account: 10,000
+ Maximum number of sessions per source network interface: 3

**Targets**
+ Maximum number of targets per account: 10,000

**Filters**
+ Maximum number of filters per account: 10,000
+ Maximum number of rules per filter: 10 †

**Sources**
+ Maximum number of sources per Network Load Balancer: No limit
+ Maximum number of sources per target \(smaller sizes\): 10 †
+ Maximum number of sources per target \(largest size\): 100 \*

† This quota cannot be increased\.

\* Applies only to the [largest instance size](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes) for an instance type\. For example, for M5 instances, the maximum is 100 for `m5.24xlarge` and 10 for all other M5 instance sizes\.

## Checksum offloading<a name="traffic-checksum-offloading"></a>

The Elastic Network Adapter \(ENA\) provides checksum offloading capabilities\. If a packet is truncated, this might result in the packet checksum not being calculated for the mirrored packet\. The following checksums are not calculated when the mirrored packet is truncated:
+ If the mirror packet is truncated, the mirror packet L4 checksum is not calculated\.
+ If any part of the L3 header is truncated, the L3 checksum is not calculated\.

Use the following commands to disable ENA checksum offloading on Amazon Linux 2 AMI:

```
[ec2-user@ip-11-0-0-166 ~]$ sudo ethtool --offload eth0 tx off 
[ec2-user@ip-11-0-0-166 ~]$ sudo ethtool --show-offload eth0
Features for eth0:
rx-checksumming: on
tx-checksumming: off
     tx-checksum-ipv4: off
     tx-checksum-ip-generic: off [fixed]
     tx-checksum-ipv6: off [fixed]
     tx-checksum-fcoe-crc: off [fixed]
     tx-checksum-sctp: off [fixed]
```