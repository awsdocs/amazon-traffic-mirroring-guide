# Traffic mirror filters<a name="traffic-mirroring-filter"></a>

Use a traffic mirror filter and its rules to determine the traffic that is mirrored\. A traffic mirror filter contains one or more traffic mirror rules\. For more information, see [Traffic mirror filter concepts](traffic-mirroring-filters.md)\.

Rules are evaluated from the lowest value to the highest value\. The first rule that matches the traffic determines the action to take\.

**Topics**
+ [Create a traffic mirror filter](#create-traffic-mirroring-filter)
+ [View your traffic mirror filters](#view-traffic-mirroring-filter)
+ [Modify your traffic mirror filter rules](#modify-traffic-mirroring-filter-rules)
+ [Modify traffic mirror filter tags](#modify-traffic-mirroring-filter-tags)
+ [Modify traffic mirror filter network services](#modify-traffic-mirroring-filter-network-services)
+ [Delete a traffic mirror filter](#delete-traffic-mirroring-filter)

## Create a traffic mirror filter<a name="create-traffic-mirroring-filter"></a>

**To create a traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

1. Choose **Create traffic mirror filter**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror filter\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror filter\.

1. \(Optional\) If you need to mirror Amazon DNS traffic, select **amazon\-dns**\.

1. For each rule, inbound or outbound, choose **Add rule**, and then specify the following information:
   + **Number**: The rule priority\.
   + **Rule action**: Indicates whether to accept or reject the packets\.
   + **Protocol**: The protocol\.
   + \(Optional\) **Source port range**: The source port range\.
   + \(Optional\) **Destination port range**: The destination port range\.
   + **Source CIDR block**: The source CIDR block\. The source and destination CIDR blocks must both be either IPv4 ranges or IPv6 ranges\.
   + **Destination CIDR block**: The destination CIDR block\. The source and destination CIDR blocks must both be either IPv4 ranges or IPv6 ranges\.
   + **Description**: A description for the rule\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror filter using the AWS CLI**  
Use the [create\-traffic\-mirror\-filter](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-filter.html) command\.

## View your traffic mirror filters<a name="view-traffic-mirroring-filter"></a>

**To view your traffic mirror filters using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

1. Select the ID of the traffic mirror filter to open its details page\.

**To view your traffic mirror filters using the AWS CLI**  
Use the [describe\-traffic\-mirror\-filters](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-filters.html) command\.

## Modify your traffic mirror filter rules<a name="modify-traffic-mirroring-filter-rules"></a>

**To modify your traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

1. Select the ID of the traffic mirror filter to open its details page\.

1. For each rule to add, choose either **Inbound rules **, **Add inbound rule** or **Outbound rules**, **Add outbound rule**\. Specify the following information, and then choose **Add rule**:
   + **Rule number**: The rule priority\.
   + \(Optional\) **Description**: A description for the rule\.
   + **Rule action**: Indicates whether to accept or reject the packets\.
   + **Protocol**: The protocol\.
   + \(Optional\) **Source port range**: The source port range\.
   + \(Optional\) **Destination port range**: The destination port range\.
   + **Source CIDR block**: The source CIDR block\. The source and destination CIDR blocks must both be either IPv4 ranges or IPv6 ranges\.
   + **Destination CIDR block**: The destination CIDR block\. The source and destination CIDR blocks must both be either IPv4 ranges or IPv6 ranges\.

1. For each inbound rule to modify, select the rule and choose **Modify outbound rule**\. Update the rule as needed, and then choose **Modify rule**\.

1. For each rule to delete, select the rule and choose **Delete**\. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

## Modify traffic mirror filter tags<a name="modify-traffic-mirroring-filter-tags"></a>

**To modify your traffic mirror filters using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

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

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

1. Select the radio button for the traffic mirror filter\.

1. Choose **Actions**, **Modify Network Services**\.

1. If you need to mirror Amazon DNS traffic, select **amazon\-dns**\. Otherwise, clear **amazon\-dns**\.

1. Choose **Modify**\.

**To modify the network services traffic mirror filters using the AWS CLI**  
Use the [modify\-traffic\-mirror\-filter\-network\-services](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-traffic-mirror-filter-network-services.html) command\.

## Delete a traffic mirror filter<a name="delete-traffic-mirroring-filter"></a>

Before you can delete a traffic mirror filter, you must remove it from any traffic mirror sessions\.

**To delete a traffic mirror filter using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror filters**\.

1. Select the traffic mirror filter, and then choose **Actions**, **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror filter using the AWS CLI**  
Use the [delete\-traffic\-mirror\-filter](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-filter.html) command\.