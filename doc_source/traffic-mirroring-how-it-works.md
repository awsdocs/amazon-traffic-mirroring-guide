# How Traffic Mirroring works<a name="traffic-mirroring-how-it-works"></a>

Traffic Mirroring copies inbound and outbound traffic from the network interfaces that are attached to your instances\. You can send the mirrored traffic to the network interface of another instance, a Network Load Balancer that has a UDP listener, or a Gateway Load Balancer that has a UDP listener\. The traffic mirror source and the traffic mirror target \(monitoring appliance\) can be in the same VPC\. Or they can be in a different VPCs that are connected through intra\-Region VPC peering, a transit gateway, or by a Gateway Load Balancer endpoint to connect to a Gateway Load Balancer in a different VPC\.

Consider the following scenario, where you mirror traffic from two sources \(Source A and Source B\) to a single traffic mirror target \(Target D\)\. After you create the traffic mirror session, any traffic that matches the filter rules is encapsulated in a VXLAN header\. It is then sent to the target\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring.png)

The following procedures are required:
+ Identify the traffic mirror source \(Source A\)
+ Identify the traffic mirror source \(Source B\)
+ Configure the traffic mirror target \(Target D\)
+ Configure the traffic mirror filter \(Filter A\)
+ Configure the traffic mirror session for Source A, Filter A, and Target D
+ Configure the traffic mirror session for Source B, Filter A, and Target D

**Topics**
+ [Targets](traffic-mirroring-targets.md)
+ [Filters](traffic-mirroring-filters.md)
+ [Sessions](traffic-mirroring-sessions.md)
+ [Connectivity options](traffic-mirroring-connection.md)
+ [Packet format](traffic-mirroring-packet-formats.md)