# Traffic mirror sessions<a name="traffic-mirroring-session"></a>

A traffic mirror session establishes a relationship between a traffic mirror source and a traffic mirror target\. For more information, see [Traffic mirror session concepts](traffic-mirroring-sessions.md)\.

A traffic mirror session contains the following resources:
+ A traffic mirror [source](traffic-mirroring-sessions.md#traffic-mirroring-sources)
+ A traffic mirror [target](traffic-mirroring-target.md)
+ A traffic mirror [filter](traffic-mirroring-filter.md)

You can create a traffic mirror session only if you are the owner of the network interface or the subnet for the traffic mirror source\.

**Topics**
+ [Create a traffic mirror session](#create-traffic-mirroring-session)
+ [View your traffic mirror sessions](#view-traffic-mirroring-session)
+ [Modify your traffic mirror session](#modify-traffic-mirroring-session)
+ [Modify traffic mirror session tags](#tag-traffic-mirroring-session)
+ [Delete a traffic mirror session](#delete-traffic-mirroring-session)

## Create a traffic mirror session<a name="create-traffic-mirroring-session"></a>

**To create a traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the **Region** selector, choose the AWS Region that you used when you created the VPCs\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror sessions**\.

1. Choose **Create traffic mirror session**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror session\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror session\.

1. For **Mirror source**, choose the network interface of the mirror source\.

1. For **Mirror target**, choose an existing traffic mirror target or choose **Create target** to create one\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

   If the mirror target is owned by another account and shared with you, you must first [accept the resource share](cross-account-traffic-mirroring-targets.md#tm-share-accept)\.

1. For **Additional settings**, do the following:

   1. For **Session number**, enter the session number\. The valid values are 1 to 32,766, where 1 is the highest priority\. Sessions are evaluated based on the priority indicated by this session number\.

   1. \(Optional\) For **VNI**, enter the VXLAN ID to use for the traffic mirror session\. For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

      If you do not enter a value, we assign a random number\.

   1. \(Optional\) For **Packet Length**, enter the number of bytes in each packet to mirror\.

      To mirror the entire packet, do not enter a value\. To mirror only a portion of each packet, set this value to the number of bytes to mirror\. For example, if you set this value to 100, the first 100 bytes after the VXLAN header that meet the filter criteria are copied to the target\.

   1. For **Filter**, choose an existing traffic mirror filter\. Alternatively, choose **Create filter**\. For more information, see [Step 2: Create the traffic mirror filter](traffic-mirroring-getting-started.md#step-create-traffic-mirroring-filters)\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror session using the AWS CLI**  
Use the [create\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-session.html) command\.

## View your traffic mirror sessions<a name="view-traffic-mirroring-session"></a>

**To view your traffic mirror sessions using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror sessions**\.

1. Select the ID of the traffic mirror session to open its details page\.

**To view your traffic mirror session using the AWS CLI**  
Use the [describe\-traffic\-mirror\-sessions](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-sessions.html) command\.

## Modify your traffic mirror session<a name="modify-traffic-mirroring-session"></a>

**To modify your traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror sessions**\.

1. Select the radio button for the traffic mirror session\.

1. Choose **Actions**, **Modify session**\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror session\.

1. For **Mirror target**, choose an existing traffic mirror target or choose **Create target** to create one\. For more information, see [Create a traffic mirror target](traffic-mirroring-target.md#create-traffic-mirroring-target)\.

1. For **Additional settings**, do the following:

   1. For **Session number**, enter the session number\. The valid values are 1 to 32,766, where 1 is the highest priority\.

   1. \(Optional\) For **VNI**, enter the VXLAN ID to use for the traffic mirror session\. For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

      If you do not enter a value, we assign a random unused number\.

   1. \(Optional\) For **Packet Length**, enter the number of bytes in each packet to mirror\.

      To mirror the entire packet, do not enter a value\. To mirror only a portion of each packet, set this value to the number of bytes to mirror\. For example, if you set this value to 100, the first 100 bytes after the VXLAN header that meet the filter criteria are copied to the target\.

   1. For **Filter**, choose the traffic mirror filter that determines what traffic gets mirrored\.

1. Choose **Modify**\.

**To modify your traffic mirror session using the AWS CLI**  
Use the [modify\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-traffic-mirror-session.html) command\.

## Modify traffic mirror session tags<a name="tag-traffic-mirroring-session"></a>

**To modify your traffic mirror session tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror sessions**\.

1. Select the ID of the traffic mirror session to open its details page\.

1. On the **Tags** tab, choose **Manage tags**\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\. For each tag to remove, choose **Remove**\.

1. Choose **Modify**\.

**To modify your traffic mirror session using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command to add a tag\. Use the [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) command to remove a tag\.

## Delete a traffic mirror session<a name="delete-traffic-mirroring-session"></a>

You are charged on an hourly basis for each active traffic mirror session\. To stop all Traffic Mirroring charges, you must delete all active traffic mirror sessions\. If you delete the network interface for the traffic mirror source, the traffic mirror sessions for the source are deleted automatically\.

**To delete your traffic mirror session using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror sessions**\.

1. Select the traffic mirror session, and then choose **Actions**, **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror session using the AWS CLI**  
Use the [delete\-traffic\-mirror\-session](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-session.html) command\.