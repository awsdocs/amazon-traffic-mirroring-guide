# Traffic mirror targets<a name="traffic-mirroring-target"></a>

A traffic mirror target is the destination for mirrored traffic\. For more information, see [Traffic mirror target concepts](traffic-mirroring-targets.md)\.

After you create a target, assign it to a traffic mirror session\. For more information, see [Create a traffic mirror session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.

You must configure a security group for the traffic mirror target that allows VXLAN traffic \(UDP port 4789\) from the traffic mirror source\.

You can share a traffic mirror target across accounts\. For more information, see [Cross\-account targets](cross-account-traffic-mirroring-targets.md)\.

**Topics**
+ [Create a traffic mirror target](#create-traffic-mirroring-target)
+ [View traffic mirror target details](#view-traffic-mirroring-targets)
+ [Modify traffic mirror target tags](#modify-traffic-mirroring-targets)
+ [Delete a traffic mirror target](#delete-traffic-mirroring-target)

## Create a traffic mirror target<a name="create-traffic-mirroring-target"></a>

**To create a traffic mirror target using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the **Region** selector, choose the AWS Region that you used when you created the mirror target\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror targets**\.

1. Choose **Create traffic mirror target**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror target\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror target\.

1. For **Target type**, choose the type of the traffic mirror target:
   + **Network interface**
   + **Network Load Balancer**
   + **Gateway Load Balancer endpoint**

1. For **Target**, choose the traffic mirror target\. We display targets based on the target type that you selected in the previous step\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror target using the AWS CLI**  
Use the [create\-traffic\-mirror\-target](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-target.html) command\.

## View traffic mirror target details<a name="view-traffic-mirroring-targets"></a>

**To view your traffic mirror targets using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror targets**\.

1. Select the ID of the traffic mirror target to open its details page\.

**To view your traffic mirror targets using the AWS CLI**  
Use the [describe\-traffic\-mirror\-targets](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-targets.html) command\.

## Modify traffic mirror target tags<a name="modify-traffic-mirroring-targets"></a>

**To modify your traffic mirror target tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror targets**\.

1. Select the ID of the traffic mirror target to open its details page\.

1. On the **Tags** tab, choose **Manage tags**\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\. For each tag to remove, choose **Remove**\.

1. Choose **Save**\.

**To modify your traffic mirror target tags using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command to add a tag\. Use the [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) command to remove a tag\.

## Delete a traffic mirror target<a name="delete-traffic-mirroring-target"></a>

Before you can delete a traffic mirror target, you must remove it from any traffic mirror sessions\.

**To delete your traffic mirror target using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror targets**\.

1. Select the traffic mirror target\.

1. Choose **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror target using the AWS CLI**  
Use the [delete\-traffic\-mirror\-target](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-target.html) command\.