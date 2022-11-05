# Traffic Mirroring limitations and quotas<a name="traffic-mirroring-limits"></a>

The following sections describe the limitations, quotas, and checksum offloading for Traffic Mirroring\.

## Limitations<a name="traffic-mirroring-limitations"></a>

Traffic Mirroring is available on a majority of the current generation Nitro\-based instances and some select non\-Nitro instance types\. 

The following non\-Nitro instance types are currently supported:
+ C4, D2, G3, G3s, H1, I3, M4, P2, P3, R4, X1, X1e

Traffic Mirroring is not available on the following instance types:
+ These current generation instances: C6a, C6gn, C6i, C6id, Hpc6a, I4i, Im4gn, Is4gen, M6a, M6i, R6i, R6id, T2, Trn1, X2idn, X2iedn, X2iezn
+ Bare metal instances
+ [Previous generation instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes)

The following traffic types cannot be mirrored:
+ ARP
+ DHCP
+ Instance metadata service
+ NTP
+ Windows activation

## Quotas<a name="traffic-mirroring-quotas"></a>

The following are the quotas for Traffic Mirroring for your AWS account\.

### Sessions<a name="traffic-mirroring-sessions"></a>

The following table lists the Traffic Mirroring session limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of sessions per account  |  10,000  | No | 
| Maximum number of sessions per source network interface |  3  | No | 
| Maximum number of sessions for a single Gateway Load Balancer endpoint | Unlimited | Not applicable | 

### Targets<a name="traffic-mirroring--limits-targets"></a>

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

### Throughput<a name="traffic-mirroring-throughput"></a>

The following table lists the Traffic Mirroring throughput limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum throughput through a single Gateway Load Balancer endpoint  | 100 Gbps | No | 

### Packets<a name="traffic-mirroring-packets"></a>

The following table lists the Traffic Mirroring packet sizes\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of MTUs for a Gateway Load Balancer endpoint  | 8,500 | No | 

### Sources<a name="traffic-mirroring-sources"></a>

The following table lists the Traffic Mirroring source limits\.


| Quota | Default | Adjustable | 
| --- | --- | --- | 
|  Maximum number of sources per Network Load Balancer  |  No limit  | No | 
| Maximum number of sources per Gateway Load Balancer endpoint | No limit | No | 
| Maximum number of sessions per target \(smaller sizes\) |  10  | No | 
| Maximum number of sources per target \(largest size\) | 100 \* | No | 

 \*  This applies only to the largest instance size\. For example, for M5 instances, the maximum is 100 for `m5.24xlarge` and `10` for all other M5 instance sizes\. For more information about instance sizes, see [Available instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes) in the *Amazon EC2 User Guide*\. 

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