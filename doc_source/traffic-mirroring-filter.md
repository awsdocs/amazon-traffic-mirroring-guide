# Traffic mirror filters<a name="traffic-mirroring-filter"></a>

Use a traffic mirror filter and its rules to determine the traffic that is mirrored\. A traffic mirror filter contains one or more traffic mirror rules\. You can also mirror certain network services\.

You can define a set of parameters to apply to the traffic mirror source traffic to determine the traffic to mirror\. The following traffic mirror filter rule parameters are available:
+ Traffic direction: Inbound or outbound
+ Action: The action to take, either to accept or reject the packet
+ Protocol: The L4 protocol
+ Source port range
+ Destination port range
+ Source CIDR block
+ Destination CIDR block

Rules are evaluated from the lowest value to the highest value\. The first rule that matches the traffic determines the action to take\.

## Create a traffic mirror filter<a name="create-traffic-mirroring-filter"></a>

Create a traffic mirror filter\.

Create a traffic mirror filter and add rules to the filter to define the traffic that is mirrored\. A traffic mirror filter contains one or more traffic mirror rules, and a set of network services\. 

The **Source CIDR block** and **Destination CIDR block** values must both be either an IPv4 range or an IPv6 range\.

**To create a traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the **Region** selector, choose the AWS Region that you used when you created the VPCs\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Choose **Create traffic mirror filter**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror filter\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror filter\.

1. \(Optional\) Mirror network services\.

   \[Mirror Amazon DNS traffic\] Select **amazon\-dns**\.

1. \(Optional\) For each inbound rule, choose **Inbound rules**, **Add rule**, and then specify the following information:
   + **Number**: Enter a priority to assign to the rule\.
   + **Rule action**: Choose the action to take for the packet\.
   + **Protocol**: Choose the L4 protocol to assign to the rule\.
   + \(Optional\) **Source port range**: Enter the source port range\.
   + \(Optional\) **Destination port range**: Enter the destination port range\.
   + **Source CIDR block**: Enter a source CIDR block\.
   + **Destination CIDR block**: Enter a destination CIDR block\.
   + **Description**: Enter a description for the rule\.

1. \(Optional\) Add outbound rules\. Choose **Outbound rules**, **Add, rule**, and then specify the following information about the traffic mirror source outbound traffic:
   + **Number**: Enter a priority to assign to the rule\.
   + **Rule action**: Choose the action to take for the packet\.
   + **Protocol**: Choose the IP protocol to assign to the rule\.
   +  \(Optional\) **Source port range**: Enter the source port range\.
   + \(Optional\) **Destination port range**: Enter the destination port range\.
   +  **Source CIDR block**: Enter a source CIDR block\.
   + **Destination CIDR block**: Enter a destination CIDR block\.
   + **Description**: Enter a description for the rule\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror filter using the AWS CLI**  
Use the [create\-traffic\-mirror\-filter](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-filter.html) command\.

## View your traffic mirror filters<a name="view-traffic-mirroring-filter"></a>

**To view your traffic mirror filters using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Select the ID of the traffic mirror filter to open its details page\.

**To view your traffic mirror filters using the AWS CLI**  
Use the [describe\-traffic\-mirror\-filters](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-filters.html) command\.

## Modify your traffic mirror filter rules<a name="modify-traffic-mirroring-filter-rules"></a>

Add or remove inbound and outbound traffic mirror filter rules\.

The **Source CIDR block** and **Destination CIDR block** values must both be either an IPv4 range or an IPv6 range\.

**To modify your traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Select the ID of the traffic mirror filter to open its details page\.

1. To add inbound rules, choose **Inbound rules **, **Add inbound rule**\. Specify the following information, and then choose **Add rule**:
   + **Rule number**: Enter a priority to assign to the rule\.
   + \(Optional **Description**: Enter a description for the rule\.
   + **Rule action**: Choose the action to take for the packet\.
   + **Protocol**: Choose the L4 protocol to assign to the rule\.
   + \(Optional\) **Source port range**: Enter the source port range\.
   + \(Optional\) **Destination port range**: Enter the destination port range\.
   + **Source CIDR block**: Enter a source CIDR block\.
   + **Destination CIDR block**: Enter a destination CIDR block\.

1. To add outbound rules, choose **Outbound rules **, **Add outbound rule**\. Specify the following information, and then choose **Add rule**:
   + **Rule number**: Enter a priority to assign to the rule\.
   + \(Optional\) **Description**: Enter a description for the rule\.
   + **Rule action**: Choose the action to take for the packet\.
   + **Protocol**: Choose the IP protocol to assign to the rule\.
   +  \(Optional\) **Source port range**: Enter the source port range\.
   + \(Optional\) **Destination port range**: Enter the destination port range\.
   +  **Source CIDR block**: Enter a source CIDR block\.
   + **Destination CIDR block**: Enter a destination CIDR block\.

1. To modify a rule, choose **Inbound rules** or **Outbound rules**\. Select the rule and choose **Modify inbound rule** or **Modify outbound rule**\. Update the rule as needed, and then choose **Modify rule**\.

1. To delete a rule, choose **Inbound rules** or **Outbound rules**\. Select the rule and choose **Delete**\. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

## Modify traffic mirror filter tags<a name="modify-traffic-mirroring-filter-tags"></a>

**To modify your traffic mirror filters using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Select the ID of the traffic mirror filter to open its details page\.

1. From the **Tags** tab, choose**Manage tags**\.

1. For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. For each tag to remove, choose **Remove**\.

1. Choose **Save**\.

**To modify the traffic mirror filter tags using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command to add a tag\. Use the [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) command to remove a tag\.

## Modify traffic mirror filter network services<a name="modify-traffic-mirroring-filter-network-services"></a>

**To modify your traffic mirror filter network services using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Select the traffic mirror filter\.

1. Choose **Actions**, **Modify Network Services**\.

1. \[Mirror Amazon DNS traffic\] Select **amazon dns**\.

1. Choose **Modify**\.

**To modify the network services traffic mirror filters using the AWS CLI**  
Use the [modify\-traffic\-mirror\-filter\-network\-services](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-traffic-mirror-filter-network-services.html) command\.

## Delete a traffic mirror filter<a name="delete-traffic-mirroring-filter"></a>

**To delete a traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Select the traffic mirror filter, and then choose **Actions**, **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror filter using the AWS CLI**  
Use the [delete\-traffic\-mirror\-filter](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-filter.html) command\.