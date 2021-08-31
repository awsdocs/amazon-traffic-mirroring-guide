# Traffic Mirroring quotas and considerations<a name="traffic-mirroring-considerations"></a>

Take the following information into consideration when you use Traffic Mirroring:
+ Encapsulated mirror traffic is routed by using the VPC route table\. Make sure that your route table is configured to send the mirrored traffic to the correct traffic mirror target\. 
+ You can only create a traffic mirror session if you are the owner of the source network interface or its subnet\.
+ We truncate the packet to the MTU value when both of the following are true:
  + The traffic mirror target is a standalone instance\.
  + The mirrored traffic packet size is greater than the traffic mirror target MTU value\.

For example, if an 8996 byte packet is mirrored, and the traffic mirror target MTU value is 9001 bytes, the mirror encapsulation results in the mirrored packet being greater than the MTU value\. In this case, the mirror packet is truncated\. To prevent mirror packets from being truncated, set the traffic mirror source interface MTU value to 54 bytes less than the traffic mirror target MTU value for IPv4 and 74 bytes less than the traffic mirror target MTU value when you use IPv6\. Therefore, the maximum MTU value supported by Traffic Mirroring with no packet truncation is 8947 bytes\. For more information about configuring the network MTU value, see[ Network Maximum Transmission Unit \(MTU\) for Your EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\. 
+ We recommend using a Network Load Balancer as a target for high availability\. 
+ Mirrored traffic counts toward instance bandwidth\. The impact depends on the amount of traffic and the traffic type\. Consider a scenario where you mirror a network interface that has 1 Gbps of inbound traffic and 1 Gbps of outbound traffic\. In this case, the instance needs to handle 1 Gbps of inbound traffic and 3 Gbps of outbound traffic \(1 Gbps for the outbound traffic, 1 Gbps for the mirrored inbound traffic and 1 Gbps for the mirrored outbound traffic\)\.
+ Production traffic has a higher priority than mirrored traffic when there is traffic congestion\. As a result, mirrored traffic is dropped when there is congestion\.
+ Mirrored traffic on an instance is not subject to outgoing security group evaluation\.
+ Flow logs do not capture mirrored traffic\.
+ If you do not have UDP listeners on the Network Load Balancer, you can still use the Network Load Balancer as a target\. However, Traffic Mirroring cannot occur because there are no UDP listeners\.
+ When you delete a network interface that is a traffic mirror source, the traffic mirror sessions that are associated with the source are automatically deleted\.
+ If you remove the UDP listeners from a Network Load Balancer that is a traffic mirror target, Traffic Mirroring fails without an error indication\.
+ When the Network Load Balancer removes the node in an Availability Zone from the DNS table, Traffic Mirroring continues to send the mirrored packets to that node\. 
+  When you have an existing Network Load Balancer which is a traffic mirror target and you add additional subnets to it, there is no effect\. For example, mirrored traffic in the Availability Zone of the new subnet is not routed in the same Availability Zone unless you enable cross\-zone load balancing\.
+ We recommend that you use cross\-zone load balancing to ensure that the packets continue to be mirrored when all targets in an Availability Zone are not healthy\.
+ Packets that are dropped at the traffic mirror source by security group rules or by network ACL rules do not get mirrored\.
+ An elastic network interface cannot be a traffic mirror target and a traffic mirror session source at the same time\.
+ Traffic Mirroring is available only on virtualized Nitro\-based instances, and the following Xen instances: 
  + C4
  + D2
  + G3
  + G3s
  + H1
  + I3
  + M4
  + P2
  + P3 
  + R4
  + X1
  + X1e
+ Traffic Mirroring is not supported on the T2, R3 and I2 instance types and previous generation instances\.

## Traffic types<a name="traffic-mirroring-network-services"></a>

Not all traffic can be mirrored\. The following traffic types cannot be mirrored:
+ ARP
+ DHCP
+ Instance metadata service
+ NTP
+ Windows activation

## Traffic Mirroring service quotas<a name="traffic-mirroring-limits"></a>

The following are the Traffic Mirroring service quotas for your AWS account\. 

### <a name="traffic-mirroring-session-limits"></a>

The following quotas apply to traffic mirror sessions:
+ Maximum number of sessions per account: 10,000
+ Maximum number of sessions on a traffic mirror source network interface: 3

### <a name="traffic-mirroring-target-limits"></a>

The following quotas apply to traffic mirror targets:
+ Maximum number of traffic mirror targets per account: 10,000

The following quotas apply to traffic mirror filters:
+ Maximum number of traffic mirror filters per account: 10,000

The following quotas apply to a traffic mirror source to traffic mirror target ratio:
+ Maximum number of mirror sources per Network Load Balancer: No limit
+ Maximum number of mirror sources per a Dedicated instance type as target: 100
  + C3\.8XL
  + C4\.8XL
  + C5\.18XL
  + C5N\.18XL
  + C5D\.18XL
  + p2\.16XL
  + p3\.16xl
  + G2\.8XL
  + g3\.16xl
  + m4\.10XL
  + m5\.24XL
  + m5d\.24XL
  + R5\.24XL
  + R5d\.24XL
  + X1\.32XL
  + X1e\.32XL

   Maximum number of mirror sources per interface as a target attached to a non\-dedicated instance: 10

  This quota cannot be increased\.

The following quotas apply to traffic mirror filters:
+ Maximum number of rules per filter: 10

  This quota cannot be increased\.

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
