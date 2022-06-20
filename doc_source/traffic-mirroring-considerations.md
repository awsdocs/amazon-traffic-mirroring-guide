# Traffic Mirroring considerations<a name="traffic-mirroring-considerations"></a>

## General<a name="traffic-mirroring-considerations-general"></a>
+ You can only create a traffic mirror session if you are the owner of the source network interface or its subnet\. 
+ We recommend using either a Network Load Balancer or a Gateway Load Balancer endpoint as a target for high availability\. 
+ An elastic network interface cannot be a traffic mirror target and a traffic mirror session source at the same time\. 
+ When you delete a network interface that is a traffic mirror source, the traffic mirror sessions that are associated with the source are automatically deleted\. 
+ Flow logs do not capture mirrored traffic\. 

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
+ Each Gateway Load Balancer endpoint can support a bandwidth of up to 40 Gbps\. For more information about Gateway Load Balancer endpoint quotas, see [Availability Zones and load balancer nodes](https://docs.aws.amazon.com/latest/privatelink/create-gateway-load-balancer-endpoint-service.html#considerations-gateway-load-balancer-endpoint-service) in the *AWS PrivateLink Guide*\.

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
+ You can use the `BytesProcessed` and `PacketsDropped` CloudWatch metrics for Amazon VPC endpoints to monitor the volume of traffic being sent over the Gateway Load Balancer endpoint\. You can also use existing Amazon VPC Traffic Mirroring CloudWatch metrics for traffic volumes for each of the traffic mirroring sessions\. For more information, see [ Monitor mirrored traffic using Amazon CloudWatchMonitor mirrored traffic Monitor your mirrored traffic with Amazon CloudWatch metrics\.  You can monitor your mirrored traffic using Amazon CloudWatch, which collects information from your network interface that is part of a traffic mirror session, and creates readable, near real\-time metrics\. You can use this information to monitor and troubleshoot Traffic Mirroring\.  For more information about Amazon CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\. For more information, see [List the available CloudWatch metrics for your instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html) in *Amazon EC2 User Guide for Linux Instances*\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.  Traffic Mirroring metrics and dimensions The following metrics are available for your mirrored traffic\. 


| Metric | Description | 
| --- | --- | 
|  `NetworkMirrorIn`  |  The number of bytes received on all network interfaces by the instance that are mirrored\.  The number reported is the number of bytes received during the period\. If you are using basic \(five\-minute\) monitoring, you can divide this number by 300 to find Bytes/second\. If you have detailed \(one\-minute\) monitoring, divide it by 60\.  Units: Bytes  | 
|  `NetworkMirrorOut`  |  The number of bytes sent out on all network interfaces by the instance that are mirrored\.  The number reported is the number of bytes sent during the period\. If you are using basic \(five\-minute\) monitoring, you can divide this number by 300 to find Bytes/second\. If you have detailed \(one\-minute\) monitoring, divide it by 60\.  Units: Bytes  | 
|  `NetworkPacketsMirrorIn`  |  The number of packets received on all network interfaces by the instance that are mirrored\. This metric is available for basic monitoring only\.  Units: Count  | 
| NetworkPacketsMirrorOut |  The number of packets sent out on all network interfaces by the instance that are mirrored\. This metric is available for basic monitoring only\.  Units: Count  | 
|  `NetworkSkipMirrorIn`  |  The number of bytes received, that meet the traffic mirror filter rules, that did not get mirrored because of production traffic taking priority\.  Units: Bytes  | 
|  `NetworkSkipMirrorOut`  |  The number of bytes sent out, that meet the traffic mirror filter rules, that did not get mirrored because of production traffic taking priority\. Units: Bytes  | 
|  `NetworkPacketsSkipMirrorIn`  |  The number of packets received, that meet the traffic mirror filter rules, that did not get mirrored because of production traffic taking priority\. This metric is available for basic monitoring only\.  Units: Count  | 
| NetworkPacketsSkipMirrorOut |  The number of packets sent out, that meet the traffic mirror filter rules, that did not get mirrored because of production traffic taking priority\. This metric is available for basic monitoring only\.  Units: Count  |  To filter the metric data, use the following dimensions\. 


| Dimension | Description | 
| --- | --- | 
| AutoScalingGroupName | This dimension filters the data you request for all instances in a specified capacity group\. An Auto Scaling group is a collection of instances you define if you're using Auto Scaling\. This dimension is available only for Amazon EC2 metrics when the instances are in such an Auto Scaling group\. Available for instances with Detailed or Basic Monitoring enabled\.  | 
| ImageId | This dimension filters the data you request for all instances running this Amazon EC2 Amazon Machine Image \(AMI\)\. Available for instances with Detailed Monitoring enabled\.  | 
| InstanceId | This dimension filters the data you request for the identified instance only\. This helps you pinpoint an exact instance from which to monitor data\. Available for instances with Detailed or Basic Monitoring enabled\.  | 
| InstanceType | This dimension filters the data you request for all instances running with this specified instance type\. This helps you categorize your data by the type of instance running\. For example, you might compare data from an m1\.small instance and an m1\.large instance to determine which has the better business value for your application\. Available for instances with Detailed Monitoring enabled\.  |    View Traffic Mirroring CloudWatch metrics You can view the metrics for Traffic Mirroring as follows\. To view metrics using the CloudWatch console Metrics are grouped first by the service namespace, and then by the various dimension combinations within each namespace\. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.  In the navigation pane, choose **Metrics**\.   Under **All metrics**, choose the **EC2** metric namespace\.   To view the metrics, select the metric dimension\.    To view metrics using the AWS CLI At a command prompt, use the following command to list the metrics that are available for Traffic Mirroring:  

```
aws cloudwatch list-metrics --namespace "AWS/EC2"
``` The Traffic Mirroring metrics are included with the metrics for Amazon EC2\.  ](traffic-mirror-cloudwatch.md)\. 
+ If all of the Gateway Load Balancer targets in an Availability Zone becomes unhealthy, the mirrored traffic can still be sent to traffic mirror targets in other zones\. In this case, cross\-zone load balancing to allow the Gateway Load Balancer to forward the mirrored traffic to a healthy target in another zone\. For more information, see [Gateway Load Balancer target failure scenarios](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/health-checks.html#failure-scenarios) in the *User Guide for Gateway Load Balancers*\.