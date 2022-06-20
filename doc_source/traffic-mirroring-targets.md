# Traffic mirror targets<a name="traffic-mirroring-targets"></a>

A *traffic mirror target* is the destination for mirrored traffic\. A traffic mirror target can be owned by an AWS account that is different from the traffic mirror source\.

Use any of the following resources for a traffic mirror target:
+ A network interface
+ A Network Load Balancer
+ Gateway Load Balancer endpoint

You can use a traffic mirror target in more than one traffic mirror session\. Make sure to allow VXLAN traffic \(UDP port 4789\) from the traffic mirror source in the security groups that are associated with the traffic mirror target\.

## Traffic mirror target options<a name="traffic-mirroring-targets-options"></a>

You can either use open\-source tools or choose a monitoring solution available on [AWS Marketplace](http://aws.amazon.com/marketplace/)\. You can stream replicated traffic to any network packet collector or analytics tool, without having to install vendor\-specific agents\. 

## Network Load Balancer considerations<a name="traffic-mirroring-targets-nlb"></a>

When the traffic mirror target is a Network Load Balancer, the following rules apply:
+ There must be UDP listeners on port 4789\.
+ If all of the Network Load Balancer targets in an Availability Zone become unhealthy, the mirrored traffic can still be sent to traffic mirror targets in other zones\. In this case, enable cross\-zone load balancing to allow the Network Load Balancer to forward the mirrored traffic to a healthy target in another zone\. For more information, see [Availability Zones and load balancer nodes](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#availability-zones) in the *Elastic Load Balancing User Guide*\.
+ For more information on Traffic Mirroring considerations, see [Traffic Mirroring considerations](traffic-mirroring-considerations.md)\.

## Gateway Load Balancer considerations<a name="traffic-mirroring-targets-gwlb"></a>

When the traffic mirror target is a Gateway Load Balancer endpoint the following rules apply:
+ A listener for Gateway Load Balancers listens for all IP packets across all ports, and then forwards traffic to the target group you've chosen\.
+ If all of the Gateway Load Balancers in an Availability Zone become unhealthy, the mirrored traffic can still be sent to traffic mirror targets in other zones\. In this case, cross\-zone load balancing is used to allow the Gateway Load Balancer to forward the mirrored traffic to a healthy target in another zone\. For more information, see [Gateway Load Balancer target failure scenarios](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/health-checks.html#failure-scenarios) in the *User Guide for Gateway Load Balancers*\. 
+ For more information on Traffic Mirroring considerations, see [Traffic Mirroring considerations](traffic-mirroring-considerations.md)\.