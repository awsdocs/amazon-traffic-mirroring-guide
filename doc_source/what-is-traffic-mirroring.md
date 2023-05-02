# What is Traffic Mirroring?<a name="what-is-traffic-mirroring"></a>

Traffic Mirroring is an Amazon VPC feature that you can use to copy network traffic from an elastic network interface of type `interface`\. You can then send the traffic to out\-of\-band security and monitoring appliances for:
+ Content inspection
+ Threat monitoring
+ Troubleshooting

The security and monitoring appliances can be deployed as individual instances, or as a fleet of instances behind either a Network Load Balancer or a Gateway Load Balancer with a UDP listener\. Traffic Mirroring supports filters and packet truncation, so that you can extract only the traffic of interest, using the monitoring tools of your choice\.

## Traffic Mirroring concepts<a name="concepts"></a>

The following are the key concepts for Traffic Mirroring:
+ **Source** — The network interface to monitor\.
+ **Filter** — A set of rules that defines the traffic that is mirrored\.
+ **Target** — The destination for mirrored traffic\.
+ **Session** — Establishes a relationship between a source, a filter, and a target\.

## Work with Traffic Mirroring<a name="access-traffic-mirroring"></a>

You can create, access, and manage your traffic mirror resources using any of the following:
+ **AWS Management Console**— Provides a web interface that you can use to access your traffic mirror resources\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Amazon VPC\. The AWS CLI is supported on Windows, macOS, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS SDKs** — Provide language\-specific APIs\. The AWS SDKs take care of many of the connection details, such as calculating signatures, handling request retries, and handling errors\. For more information, see [AWS SDKs](https://aws.amazon.com/tools/#SDKs)\.
+ **Query API**— Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Amazon VPC\. However, it requires that your application handle low\-level details such as generating the hash to sign the request and handling errors\. For more information, see [Amazon VPC actions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/OperationList-query-vpc.html) in the *Amazon EC2 API Reference*\.

## Traffic Mirroring benefits<a name="traffic-mirroring-benefits"></a>

Traffic Mirroring offers the following benefits:
+ **Simplified operation** — Mirror any range of your VPC traffic without having to manage packet forwarding agents on your EC2 instances\.
+ **Enhanced security** — Capture packets at the elastic network interface, which cannot be disabled or tampered with from a user space\.
+ **Increased monitoring options** — Send your mirrored traffic to any security device\.

## Pricing<a name="pricing"></a>

You are charged on an hourly basis for each active traffic mirror session\. You'll continue to be charged for Traffic Mirroring until you delete all active traffic mirror sessions\. For example, you'll still be charged in the following scenarios:
+ You detached the network interface from the mirror source
+ You stopped or terminated the mirror source
+ You changed the instance type of the mirror source to an unsupported instance type

For the steps to delete a traffic mirror session, see [Delete a traffic mirror session](traffic-mirroring-session.md#delete-traffic-mirroring-session)\.

For information about pricing for Traffic Mirroring, see **Network Analysis** on the [Amazon VPC pricing](http://aws.amazon.com/vpc/pricing/) page\.