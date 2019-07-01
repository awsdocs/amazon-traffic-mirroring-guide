# Example: Mirror Inbound TCP Traffic to a Single Monitoring Appliance<a name="tm-example-inbound-tcp"></a>

Consider the scenario where you want to mirror inbound TCP traffic on an instance, and send it to a single monitoring appliance\. You need the following Traffic Mirror resources for this example:
+ A Traffic Mirror target for the appliance \(Target A\)
+ A Traffic Mirror filter with a Traffic Mirror rule for the TCP inbound traffic \(Filter 1\)
+ A Traffic Mirror session that has the following:
  + A Traffic Mirror source
  + A Traffic Mirror target for the appliance
  + A Traffic Mirror filter with a Traffic Mirror rule for the TCP inbound traffic

## Step 1: Create a Traffic Mirror Target<a name="step-create-target"></a>

Create a Traffic Mirror target \(Target A\) for the monitoring appliance\. Depending on your configuration, the target is one of the following types:
+ The network interface of the monitoring appliance
+ The Network Load Balancer when the appliance is deployed behind one

For more information, see [Create a Traffic Mirror Target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

## Step 2: Create a Traffic Mirror Filter<a name="step-create-filter"></a>

Create a Traffic Mirror filter \(Filter 1\) that has the following inbound rule\. For more information, see [Create a Traffic Mirror Filter](traffic-mirroring-filter.md#create-traffic-mirroring-filter)\.


**Traffic Mirror Filter Rule for Inbound TCP Traffic**  

| Option | Value | 
| --- | --- | 
| Rule action | Accept | 
| Protocol | TCP | 
| Source port range |  | 
| Destination port range |  | 
| Source CIDR block | 0\.0\.0\.0/0 | 
| Destination CIDR block | 0\.0\.0\.0/0 | 
| Description | TCP Rule | 

## Step 3: Create a Traffic Mirror Session<a name="step-create-session"></a>

Create and configure a Traffic Mirror session with the following options\. For more information, see [Create a Traffic Mirror Session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.


**Traffic Mirror Session to Monitor Inbound TCP Traffic**  

| Option | Value | 
| --- | --- | 
| Mirror source | The network interface of the instance that you want to monitor\. | 
| Mirror target | Target A | 
| Filter | Filter 1 | 