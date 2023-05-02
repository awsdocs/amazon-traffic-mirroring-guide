# Traffic mirror filter concepts<a name="traffic-mirroring-filters"></a>

A *traffic mirror filter* is a set of inbound and outbound traffic rules that determine the traffic that is copied from the traffic mirror source and sent to the traffic mirror target\. The combination of rules that you add define what traffic is mirrored\. You can also choose to mirror certain network services traffic, including Amazon DNS\. When you add network services traffic, all traffic \(inbound and outbound\) related to that network service is mirrored\.

*Traffic mirror filter rules* control what traffic is mirrored\. You add rules to apply to the inbound and outbound traffic for the traffic mirror source\. We evaluate rules from the lowest value to the highest value\. The first rule that matches the traffic determines whether the traffic is mirrored\. If you don't add any rules, then no traffic is mirrored\.

For example, in the following set of filter rules, rule 10 ensures that SSH traffic from my network to my VPC is not mirrored and rule 20 mirrors all other IPv4 TCP traffic\.


**Inbound**  

| Number | Rule action | Protocol | Source port range | Destination port range | Source CIDR block | Destination CIDR block | 
| --- | --- | --- | --- | --- | --- | --- | 
| 10 | reject | TCP \(6\) |  | 22 | my\-network | vpc\-cidr | 
| 20 | accept | TCP \(6\) |  |  | 0\.0\.0\.0/0 | 0\.0\.0\.0/0 | 

In the following set of filter rules, rule 10 mirrors HTTPS traffic from all IPv4 addresses and rule 20 mirrors HTTPS traffic from all IPv6 addresses\.


**Inbound**  

| Number | Rule action | Protocol | Source port range | Destination port range | Source CIDR block | Destination CIDR block | 
| --- | --- | --- | --- | --- | --- | --- | 
| 10 | accept | TCP \(6\) |  | 443 | 0\.0\.0\.0/0 | 0\.0\.0\.0/0 | 
| 20 | accept | TCP \(6\) |  | 443 | ::/0 | ::/0 | 

Note that if you don't add outbound rules, then no outbound traffic is mirrored\.