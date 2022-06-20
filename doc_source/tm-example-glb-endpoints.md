# Example: Mirror traffic to appliances behind a Gateway Load Balancer via Gateway Load Balancer endpoints<a name="tm-example-glb-endpoints"></a>

You can deploy a Gateway Load Balancer \(GWLB\) and Gateway Load Balancer endpoint \(GWLBe\) to securely send mirror traffic across VPC and accounts\. The GWLBe is a VPC endpoint that provides private connectivity between VPC with the mirror sources and the monitoring appliances deployed behind the GWLB\. 

The following diagram shows a deployment of a GWLB for traffic mirroring utilizing GWLBe interfaces\. The GWLB is deployed in a centralized Service VPC with multiple appliances as targets\. The GWLB is set up for each Availability Zone that the customer wants to monitor traffic, and it can configure their GWLB with cross\-zone load balancing as an option to protect against single Availability Zone failures\. In the spoke VPCs, GWLBe interfaces are deployed in each spoke VPC\. These endpoints are connected to the GWLB to send traffic from the spoke VPC to the Service VPC\. 

![\[Traffic Mirroring packet.\]](http://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring-example-gwlb.png)

Consider the scenario where you want to mirror inbound TCP traffic on an instance and then send it to a Gateway Load Balancer via a Gateway Load Balancer endpoint\. You need the following Traffic Mirroring entities for this example: 
+ A Traffic Mirroring target for the Gateway Load Balancer endpoint \(Target A\) in Spoke VPC1
+ A Traffic Mirroring target for the Gateway Load Balancer endpoint \(Target B\) in Spoke VPC2
+ A Traffic Mirroring filter with a Traffic Mirroring rule for the TCP inbound traffic \(Filter 1\) for the Gateway Load Balancer endpoint
+ A Traffic Mirroring session for Spoke VPC1 that has the following:
  + A Traffic Mirroring source
  + A Traffic Mirroring target \(Target A\) for the Gateway Load Balancer endpoint
  + A Traffic Mirroring filter \(Filter 1\) with a Traffic Mirroring rule for the TCP inbound traffic
+ A Traffic Mirroring session for Spoke VPC2 that has the following:
  + A Traffic Mirroring source
  + A Traffic Mirroring target \(Target B\) for the Gateway Load Balancer endpoint
  + A Traffic Mirroring filter \(Filter 1\) with a Traffic Mirroring rule for the TCP inbound traffic

## Step 1: Create a traffic mirror target in Spoke VPC1<a name="step-create-glb-target-vpc1"></a>

Create a traffic mirror target \(Target A\) for the Gateway Load Balancer endpoint in Spoke VPC1\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\. 

The Gateway Load Balancer endpoint will be the target when the monitoring appliances are deployed behind a Gateway Load Balancer\.

## Step 2: Create a traffic mirror target in Spoke VPC2<a name="step-create-glb-target-vpc2"></a>

Create a traffic mirror target \(Target B\) for the Gateway Load Balancer endpoint in Spoke VPC1\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\. 

The Gateway Load Balancer endpoint will be the target when the monitoring appliances are deployed behind a Gateway Load Balancer\.

## Step 3: Create a traffic mirror filter rule<a name="step-create-glb-target-filter"></a>

Create a traffic mirror filter \(Filter 1\) that has the following inbound rule\. For more information on creating a filter, see [Create a traffic mirror filter](traffic-mirroring-filter.md#create-traffic-mirroring-filter)\. 


**Traffic mirror filter rule for inbound TCP traffic**  

| Option | Value | 
| --- | --- | 
| Rule action | Accept | 
| Protocol | TCP | 
| Source port range |  | 
| Destination port range |  | 
| Source CIDR block | 0\.0\.0\.0/0 | 
| Destination CIDR block | 0\.0\.0\.0/0 | 
| Description | TCP Rule | 

## Step 4: Create a traffic mirror session in Spoke VPC1<a name="step-create-session-vpc1"></a>

Create and configure a traffic mirror session with the following options\. For more information, see [Create a traffic mirror session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.


**Traffic mirror session to monitor inbound TCP traffic for Spoke VPC1**  

| Option | Value | 
| --- | --- | 
| Mirror source | The network interface of the instance that you want to monitor\. | 
| Mirror target | Target A | 
| Filter | Filter 1 | 

## Step 5: Create a traffic mirror session in Spoke VPC2<a name="step-create-session-vpc2"></a>

Create and configure a traffic mirror session with the following options\. For more information, see [Create a traffic mirror session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.


**Traffic mirror session to monitor inbound TCP traffic for Spoke VPC2**  

| Option | Value | 
| --- | --- | 
| Mirror source | The network interface of the instance that you want to monitor\. | 
| Mirror target | Target B | 
| Filter | Filter 1 | 