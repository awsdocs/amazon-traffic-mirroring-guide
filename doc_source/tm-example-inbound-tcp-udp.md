# Example: Mirror Inbound TCP and UDP Traffic to Two Different Appliances<a name="tm-example-inbound-tcp-udp"></a>

Consider the scenario where you want to mirror inbound TCP and UDP traffic on an instance, and send the TCP traffic to one appliance \(Appliance A\) and UDP traffic to a second appliance \(Appliance B\)\. You need the following Traffic Mirror entities for this example:
+ A Traffic Mirror target for Appliance A \(Target A\)
+ A Traffic Mirror target for Appliance B \(Target B\)
+ A Traffic Mirror filter with a Traffic Mirror rule for the TCP inbound traffic \(Filter 1\)
+ A Traffic Mirror filter with a Traffic Mirror rule for the UDP inbound traffic \(Filter 2\)
+ A Traffic Mirror session that has the following:
  + A Traffic Mirror source
  + A Traffic Mirror target \(Target A\) for Appliance A
  + A Traffic Mirror filter \(Filter 1\) with a Traffic Mirror rule for the TCP inbound traffic
+ A Traffic Mirror session that has the following:
  + A Traffic Mirror source
  + A Traffic Mirror target \(Target B\) for Appliance B
  + A Traffic Mirror filter \(Filter 2\) with a Traffic Mirror rule for the UDP inbound traffic

## Step 1: Create a Traffic Mirror Target for Appliance A<a name="step-create-targetA"></a>

Create a Traffic Mirror target for Appliance A \(Target A\)\. Depending on your configuration, the target is one of the following types:
+ The network interface of the monitoring appliance
+ The Network Load Balancer when the appliance is deployed behind one

For more information, see [Create a Traffic Mirror Target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

## Step 2: Create a Traffic Mirror Target for Appliance B<a name="step-create-targetB"></a>

Create a Traffic Mirror target \(Target B\) for Appliance B\. Depending on your configuration, the target is one of the following types:
+ The network interface of the monitoring appliance
+ The Network Load Balancer when the appliance is deployed behind one

For more information, see [Create a Traffic Mirror Target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

## Step 3: Create a Traffic Mirror Filter with a Rule for TCP Traffic<a name="step-create-filter-tcp"></a>

Create a Traffic Mirror filter \(Filter 1\) with the following inbound rule for TCP traffic\. For more information, see [Create a Traffic Mirror Filter](traffic-mirroring-filter.md#create-traffic-mirroring-filter)


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

## Step 4: Create a Traffic Mirror Filter with a Rule for UDP Traffic<a name="step-create-filter-tcp"></a>

Create a Traffic Mirror filter \(Filter 2\) with the following inbound rule for UDP traffic\. For more information, see [Create a Traffic Mirror Filter](traffic-mirroring-filter.md#create-traffic-mirroring-filter)


**Traffic Mirror Filter Rule for Inbound UDP Traffic**  

| Option | Value | 
| --- | --- | 
| Rule action | Accept | 
| Protocol | UDP | 
| Source port range |  | 
| Destination port range |  | 
| Source CIDR block | 0\.0\.0\.0/0 | 
| Destination CIDR block | 0\.0\.0\.0/0 | 
| Description | UDP Rule | 

## Step 5: Create a Traffic Mirror Session for the TCP Traffic<a name="step-create-session-tcp"></a>

Create and configure a Traffic Mirror session with the following options\. For more information, see [Create a Traffic Mirror Session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.


**Traffic Mirror Session to Monitor Inbound TCP Traffic**  

| Option | Value | 
| --- | --- | 
| Mirror source | The network interface of the instance that you want to monitor\. | 
| Mirror target | Target A | 
| Filter | Filter 1 | 
| Session number | 1 | 

## Step 6: Create a Traffic Mirror Session for the UDP Traffic<a name="step-create-session-udp"></a>

Create and configure a Traffic Mirror session with the following options\. For more information, see [Create a Traffic Mirror Session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.


**Traffic Mirror Session to Monitor Inbound UDP Traffic**  

| Option | Value | 
| --- | --- | 
| Mirror source | The network interface of the instance that you want to monitor\. | 
| Mirror target | Target B | 
| Filter | Filter 2 | 
| Session number | 2 | 