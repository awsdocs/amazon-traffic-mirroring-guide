# Traffic mirror source and target connectivity options<a name="traffic-mirroring-connection"></a>

The traffic mirror source and the traffic mirror target \(monitoring appliance\) can be in the same VPC, or different VPCs, connected using an intra\-Region VPC peering connection or with a transit gateway using a Gateway Load Balancer endpoint to connect to a Gateway Load Balancer in a different VPC\.

The traffic mirror target can be owned by an AWS account that is different from the traffic mirror source\.

The mirrored traffic is sent to the traffic mirror target using the source VPC route table\. Before you configure Traffic Mirroring, make sure that the traffic mirror source can route to the traffic mirror target\.

The following table describes the available resource configurations\.


| Source owner | Source VPC | Target owner | Target VPC | Connectivity option | 
| --- | --- | --- | --- | --- | 
| Account A | VPC 1 | Account A | VPC1 | No additional configuration | 
| Account A | VPC 1 | Account A | VPC 2 | Intra\-Region peering or a transit gateway or Gateway Load Balancer endpoint | 
| Account A | VPC 1 | Account B | VPC 2 | Cross\-account intra\-Region peering connection, a transit gateway, or a Gateway Load Balancer endpoint | 
| Account A | VPC 1 | Account B | VPC 1 | VPC sharing | 