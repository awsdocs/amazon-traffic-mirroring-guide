# Traffic mirror targets<a name="traffic-mirroring-target"></a>

A target is the destination for a traffic mirror session\.

The traffic mirror target can be an elastic network interface, a Network Load Balancer, or a Gateway Load Balancer endpoint\. After you create a target, assign it to a traffic mirror session\. For more information, see [Create a traffic mirror session](traffic-mirroring-session.md#create-traffic-mirroring-session)\.

You must configure a security group for the traffic mirror target that allows VXLAN traffic from the source to the target\.

You can share a traffic mirror target across accounts\. To share a traffic mirror target, create the target, and then share the target\. For more information, see [Share a traffic mirror target](cross-account-traffic-mirroring-targets.md#tm-sharing)\.

## Create a traffic mirror target<a name="create-traffic-mirroring-target"></a>

Create a destination for mirrored traffic\.

**To create a traffic mirror target using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the **Region** selector, choose the AWS Region that you used when you created the VPCs\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Targets**\.

1. Choose **Create traffic mirror target**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror target\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror target\.

1. For **Target type**, choose the traffic mirror target type\.

1. For **Target**, choose the traffic mirror target\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

**To create a traffic mirror target using the AWS CLI**  
Use the [create\-traffic\-mirror\-target](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-traffic-mirror-target.html) command\.

## View traffic mirror target details<a name="view-traffic-mirroring-targets"></a>

View the traffic mirror target details\.

**To view your traffic mirror targets using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Targets**\.

1. Select the ID of the traffic mirror target to open its details page\.

**To view your traffic mirror targets using the AWS CLI**  
Use the [describe\-traffic\-mirror\-targets](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-traffic-mirror-targets.html) command\.

## Modify traffic mirror target tags<a name="modify-traffic-mirroring-targets"></a>

Add a tag to the traffic mirror target, or remove a tag from the traffic mirror target\.

**To modify your traffic mirror target tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Targets**\.

1. Select the ID of the traffic mirror target to open its details page\.

1. On the **Tags** tab, choose **Manage tags**\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\. For each tag to remove, choose **Remove**\.

1. Choose **Save**\.

**To modify your traffic mirror target tags using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command to add a tag\. Use the [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) command to remove a tag\.

## Delete a traffic mirror target<a name="delete-traffic-mirroring-target"></a>

Before you delete a traffic mirror target, pause all traffic mirror sessions that use the traffic mirror target\.

**To delete your traffic mirror target using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Targets**\.

1. Select the traffic mirror target\.

1. Choose **Delete**\.

1. When prompted for confirmation, enter **delete**, and then choose **Delete**\.

**To delete a traffic mirror target using the AWS CLI**  
Use the [delete\-traffic\-mirror\-target](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-traffic-mirror-target.html) command\.