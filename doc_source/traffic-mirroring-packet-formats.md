# Traffic Mirroring packet format<a name="traffic-mirroring-packet-formats"></a>

Mirrored traffic is encapsulated with a VXLAN header\. All appliances that receive traffic directly with this feature should be able parse a VXLAN\-encapsulated packet, as shown in the following example:

![\[Traffic Mirroring packet.\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring-packets.png)

For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

The following fields apply to Traffic Mirroring:
+ **VXLAN ID** — The virtual network ID that you can assign to a traffic mirror session\. If you do not assign a value, we assign a random value that is unique to all sessions in the account\.
+ **Source IP address** — The primary IP address of the source network interface\. 
+ **Destination IP address** — The primary IP address of the appliance, or the Network Load Balancer when the appliance is deployed behind one\. 

Appliances that received mirrored traffic through a Gateway Load Balancer should be able to parse both outer GENEVE encapsulation \(from Gateway Load Balancer\) and an inner VXLAN encapsulation \(from VPC Traffic Mirroring\) to retrieve the original L3 packet\. The following shows an example:

![\[Traffic Mirroring packets include Gateway Load Balancer\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring-gwlb-packets.png)