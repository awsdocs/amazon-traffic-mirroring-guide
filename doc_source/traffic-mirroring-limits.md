# Traffic Mirroring quotas and limitations<a name="traffic-mirroring-limits"></a>

Traffic Mirroring has the following quotas and limitations\.

**Topics**
+ [Quotas](#traffic-mirroring-quotas)
+ [Limitations](#traffic-mirroring-limitations)
+ [MTU](#traffic-mirroring-mtu)
+ [Traffic bandwidth and prioritization](#traffic-mirroring-bandwidth)
+ [Checksum offloading](#traffic-checksum-offloading)

## Quotas<a name="traffic-mirroring-quotas"></a>

The following are the quotas for Traffic Mirroring for your AWS account\.

### Sessions<a name="traffic-mirroring-limits-sessions"></a>

The following table lists the Traffic Mirroring session limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of sessions per account  |  10,000  | No | 
| Maximum number of sessions per source network interface |  3  | No | 
| Maximum number of sessions for a single Gateway Load Balancer endpoint | Unlimited | Not applicable | 

### Targets<a name="traffic-mirroring-limits-targets"></a>

The following table lists the Traffic Mirroring target limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of targets per account  |  10,000  | No | 

### Filters<a name="traffic-mirroring-limits-filters"></a>

The following table lists the Traffic Mirroring filter limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of filters per account  |  10,000  | No | 
| Maximum number of sessions per source network interface |  3  | No | 
| Maximum number of filter rules per filter | 10 | No | 

### Throughput<a name="traffic-mirroring-limits-throughput"></a>

The following table lists the Traffic Mirroring throughput limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum throughput through a single Gateway Load Balancer endpoint  | 100 Gbps | No | 

### Packets<a name="traffic-mirroring-limits-packets"></a>

The following table lists the Traffic Mirroring packet sizes\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of MTUs for a Gateway Load Balancer endpoint  | 8,500 | No | 

### Sources<a name="traffic-mirroring-limits-sources"></a>

The following table lists the Traffic Mirroring source limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of sources per Network Load Balancer  |  No limit  | No | 
| Maximum number of sources per Gateway Load Balancer endpoint | No limit | No | 
| Maximum number of sessions per target \(smaller sizes\) |  10  | No | 
| Maximum number of sources per target \(largest size\) | 100 \* | No | 

 \*  This applies only to the largest instance size\. For example, for M5 instances, the maximum is 100 for `m5.24xlarge` and `10` for all other M5 instance sizes\. For more information about instance sizes, see [Available instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes) in the *Amazon EC2 User Guide*\. 

## Limitations<a name="traffic-mirroring-limitations"></a>

**Instance types**
+ Traffic Mirroring is not available on the following virtualized Nitro instance types:
  + General purpose: M6a, M6i, M6in, M7g
  + Compute optimized: C6a, C6gn, C6i, C6id, C6in, C7g, Hpc6a
  + Memory optimized: R6a, R6i, R6id, R6idn, R6in, R7g, X2idn, X2iedn, X2iezn
  + Storage optimized: I4i, Im4gn, Is4gen
  + Accelerated computing: Trn1
+ Traffic Mirroring is not available on bare metal instances\.
+ Traffic Mirroring is available only on the following non\-Nitro instance types: C4, D2, G3, G3s, H1, I3, M4, P2, P3, R4, X1, and X1e\.

**Traffic types**

Traffic Mirroring can't mirror the following traffic types:
+ ARP
+ DHCP
+ Instance metadata service
+ NTP
+ Windows activation

**VPC Flow Logs**  
VPC Flow Logs do not capture mirrored traffic\.

## MTU<a name="traffic-mirroring-mtu"></a>

We truncate the packet to the MTU value when both of the following are true:
+ The traffic mirror target is a standalone instance\.
+ The mirrored traffic packet size is greater than the traffic mirror target MTU value\.

For example, if an 8996 byte packet is mirrored, and the traffic mirror target MTU value is 9001 bytes, the mirror encapsulation results in the mirrored packet being greater than the MTU value\. In this case, the mirror packet is truncated\. To prevent mirror packets from being truncated, set the traffic mirror source interface MTU value to 54 bytes less than the traffic mirror target MTU value for IPv4 and 74 bytes less than the traffic mirror target MTU value when you use IPv6\. Therefore, the maximum MTU value supported by Traffic Mirroring with no packet truncation is 8947 bytes\.

For more information about configuring the network MTU value, see [Network maximum transmission unit \(MTU\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Traffic bandwidth and prioritization<a name="traffic-mirroring-bandwidth"></a>

Mirrored traffic counts toward instance bandwidth\. For example, if you mirror a network interface that has 1 Gbps of inbound traffic and 1 Gbps of outbound traffic, the instance must handle 4 Gbps of traffic \(1 Gbps inbound, 1 Gbps mirrored inbound, 1 Gbps outbound, and 1 Gbps mirrored outbound\)\.

Production traffic has a higher priority than mirrored traffic when there is traffic congestion\. As a result, mirrored traffic is dropped when there is congestion\.

By default, each Gateway Load Balancer endpoint can support a bandwidth of up to 10 Gbps per Availability Zone and automatically scales up to 100 Gbps\. For more information, see [AWS PrivateLink quotas](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-limits-endpoints.html) in the *AWS PrivateLink Guide*\.

## Checksum offloading<a name="traffic-checksum-offloading"></a>

The Elastic Network Adapter \(ENA\) provides checksum offloading capabilities\. If a packet is truncated, this might result in the packet checksum not being calculated for the mirrored packet\. The following checksums are not calculated when the mirrored packet is truncated:
+ If the mirror packet is truncated, the mirror packet L4 checksum is not calculated\.
+ If any part of the L3 header is truncated, the L3 checksum is not calculated\.

If this causes issues, you can disable ENA checksum offloading on the ENA for the source\. For example, use the following commands on Amazon Linux 2:

```
[ec2-user ~]$ sudo ethtool --offload eth0 tx off 
[ec2-user ~]$ sudo ethtool --show-offload eth0
Features for eth0:
rx-checksumming: on
tx-checksumming: off
     tx-checksum-ipv4: off
     tx-checksum-ip-generic: off [fixed]
     tx-checksum-ipv6: off [fixed]
     tx-checksum-fcoe-crc: off [fixed]
     tx-checksum-sctp: off [fixed]
```