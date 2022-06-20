# Traffic mirror sessions<a name="traffic-mirroring-session"></a>

A traffic mirror session establishes a relationship between a traffic mirror source and a traffic mirror target\. A traffic mirror session contains the following resources:
+ A traffic mirror source
+ A traffic mirror [target](traffic-mirroring-target.md)
+ A traffic mirror [filter](traffic-mirroring-filter.md)

The source is the network interface of type `interface` \(for example, the network interface for an EC2 instance or an RDS instance\)\. For more information, see [Traffic Mirroring limitations and quotas](traffic-mirroring-limits.md)\.

Each packet is mirrored once\. However, you can use multiple traffic mirror sessions on the same source\. This is useful if you want to send a subset of the mirrored traffic from a traffic mirror source to multiple tools\. For example, you can filter HTTP traffic in a higher priority traffic mirror session and send it to a specific monitoring appliance\. At the same time, you can filter all other TCP traffic in a lower priority traffic mirror session and send it to another monitoring appliance\.

Traffic mirror sessions are evaluated based on the ascending priority that you define when you create the session\.

## Create a traffic mirror session<a name="create-traffic-mirroring-session"></a>

Create a traffic mirror session\.

Before you create a traffic mirror session, make sure that you have the following information:
+ The network interface for the source\. The network interface type must be `instance`\.
+ The traffic mirror target
  + To use a target in your account, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.
  + To use a target that is owned by another account and shared with you, accept the shared resource before you create the traffic mirror session\. For more information, see [Accept a resource share](cross-account-traffic-mirroring-targets.md#tm-share-accept)\.
+ The traffic mirror filter \(for more information, see [Create a traffic mirror filter](traffic-mirroring-filter.md#create-traffic-mirroring-filter)\)

**To create a traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the **Region** selector, choose the AWS Region that you used when you created the VPCs\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Choose **Create traffic mirror session**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror session\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror session\.

1. For **Mirror source**, choose the network interface of the instance that you want to monitor\.

1. For **Mirror target**, choose the traffic mirror target or create one\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

1. For **Additional settings**, do the following:

   1. For **Session number**, enter the session number\. The valid values are 1 to 32,766, where 1 is the highest priority\.

      The session number determines the order that traffic mirror sessions are evaluated when an interface is used by multiple sessions that have the same interface, but have different traffic mirror targets and traffic mirror filters\. Traffic is only mirrored one time\.

   1. \(Optional\) For **VNI**, enter the VXLAN ID to use for the traffic mirror session\. For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

      If you do not enter a value, we assign a random number\.

   1. \(Optional\) For **Packet Length**, enter the number of bytes in each packet to mirror\.

      If you do not want to mirror the entire packet, set **Packet Length** to the number of bytes in each packet to mirror\. For example, if you set this value to 100, the first 100 bytes after the VXLAN header that meet the filter criteria are copied to the target\.

      To mirror the entire packet, do not enter a value in this field\.

   1. For **Filter**, choose the traffic mirror filter that determines what traffic gets mirrored\.

      To create a filter, choose **Create filter**\. For more information, see [Step 2: Create the traffic mirror filter](traffic-mirroring-getting-started.md#step-create-traffic-mirroring-filters)\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror session using the AWS CLI**  
Use the [create\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-session.html) command\.

## View your traffic mirror sessions<a name="view-traffic-mirroring-session"></a>

**To view your traffic mirror sessions using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Select the ID of the traffic mirror session to open its details page\.

**To view your traffic mirror session using the AWS CLI**  
Use the [describe\-traffic\-mirror\-sessions](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-sessions.html) command\.

## Modify your traffic mirror session<a name="modify-traffic-mirroring-session"></a>

**To modify your traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Select the traffic mirror session\.

1. Choose **Actions**, **Modify session**\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror session\.

1. For **Mirror target**, choose the traffic mirror target\.

   To create a target, choose **Create target**\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

1. For **Additional settings**, do the following:

   1. For **Session number**, enter the session number\. The session number determines the order that traffic mirror sessions are evaluated\. The valid values are 1 to 32,766, where 1 is the highest priority\.

   1. \(Optional\) For **VNI**, enter the VXLAN ID to use for the traffic mirror session\. For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

      If you do not enter a value, we assign a random unused number\.

   1. \(Optional\) For **Packet Length**, enter the number of bytes in each packet to mirror\.

      If you do not want to mirror the entire packet, set **Packet Length** to the number of bytes in each packet to mirror\. For example, if you set this value to 100, the first 100 bytes after the VXLAN header that meet the filter criteria are copied to the target\.

      To mirror the entire packet, do not enter a value in this field\.

   1. For **Filter**, choose the traffic mirror filter that determines what traffic gets mirrored\.

1. Choose **Modify**\.

**To modify your traffic mirror session using the AWS CLI**  
Use the [modify\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-traffic-mirror-session.html) command\.

## Modify traffic mirror session tags<a name="tag-traffic-mirroring-session"></a>

**To modify your traffic mirror session tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Select the ID of the traffic mirror session to open its details page\.

1. On the **Tags** tab, choose **Manage tags**\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\. For each tag to remove, choose **Remove**\.

1. Choose **Modify**\.

**To modify your traffic mirror session using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command to add a tag\. Use the [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) command to remove a tag\.

## Delete a traffic mirror session<a name="delete-traffic-mirroring-session"></a>

**To delete your traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Select the traffic mirror session, and then choose **Actions**, **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror session using the AWS CLI**  
Use the [delete\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-session.html) command\.