# Get started with Traffic Mirroring<a name="traffic-mirroring-getting-started"></a>

The following tasks help you to become familiar with traffic mirror targets, filters, and sessions\. Follow the instructions to create a traffic mirror target and filter, and then use those resources to create a session\.

**Topics**
+ [Prerequisites](#traffic-mirroring-prerequisites)
+ [Step 1: Create the traffic mirror target](#step-create-traffic-mirroring-target)
+ [Step 2: Create the traffic mirror filter](#step-create-traffic-mirroring-filters)
+ [Step 3: Create the traffic mirror session](#step-create-traffic-mirroring-sessions)
+ [Step 4: Analyze the data](#analyze-data)

## Prerequisites<a name="traffic-mirroring-prerequisites"></a>

Review the Traffic Mirroring considerations\. For more information, see [Traffic Mirroring considerations](traffic-mirroring-considerations.md)\. Also verify the following\.
+ Make sure that the traffic mirror source and traffic mirror target are either: 
  + In the same VPC, or
  + In different VPCs that are connected via VPC peering, a transit gateway, or a Gateway Load Balancer endpoint\.
+ The traffic mirror target instance must allow traffic to UDP port 4789\.
+ The traffic mirror source must have a route table entry for the traffic mirror target\.
+ Security group rules and network ACL rules on the traffic mirror target cannot drop the mirrored traffic from the traffic mirror source\.

## Step 1: Create the traffic mirror target<a name="step-create-traffic-mirroring-target"></a>

Create a destination for mirrored traffic\.

**Create a traffic mirror target**

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

## Step 2: Create the traffic mirror filter<a name="step-create-traffic-mirroring-filters"></a>

A traffic mirror filter contains one or more traffic mirror rules, and a set of network services\. The filters and rules that you add define the traffic that is mirrored\. 

**To create a traffic mirror filter**

1. On the navigation pane, choose **Traffic Mirroring**, **Mirror Filters**\.

1. Choose **Create traffic mirror filter**\.

1. \(Optional For **Name tag**, enter a name for the traffic mirror filter\.

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

1. \(Optional\) For each outbound rule, choose **Outbound rules**, **Add rule**, and then specify the following information:
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

## Step 3: Create the traffic mirror session<a name="step-create-traffic-mirroring-sessions"></a>

Create a traffic mirror session that sends mirrored packets from the source to a target so that you can monitor and analyze traffic\.

**To create a traffic mirror session**

1. In the navigation pane, choose **Traffic Mirroring**, **Mirror Sessions**\.

1. Choose **Create traffic mirror session**\.

1. \(Optional\) For **Name tag**, enter a name for the traffic mirror session\.

1. \(Optional\) For **Description**, enter a description for the traffic mirror session\.

1. For **Mirror source**, choose the network interface of the instance that you want to monitor\. 

1. For **Mirror target**, choose the traffic mirror target\.

1. For **Additional settings**, do the following:

   1. For **Session number**, enter the session number\. The valid values are 1 to 32,766, where 1 is the highest priority\.

      The session number determines the order that traffic mirror sessions are evaluated in both of the following situations:
      + When an interface is used by multiple sessions\.
      + When an interface is used by different traffic mirror targets and traffic mirror filters\.

      Traffic is only mirrored one time\.

   1. \(Optional\) For **VNI**, enter the VXLAN ID to use for the traffic mirror session\. For more information about the VXLAN protocol, see [RFC 7348](https://tools.ietf.org/html/rfc7348)\.

      If you do not enter a value, we assign a random unused number\.

   1. \(Optional\) For **Packet Length**, enter the number of bytes in each packet to mirror\.

      If you do not want to mirror the entire packet, set **Packet Length** to the number of bytes in each packet to mirror\. For example, if you set this value to 100, the first 100 bytes after the VXLAN header that meet the filter criteria are copied to the target\.

      To mirror the entire packet, do not enter a value in this field\.

   1. For **Filter**, choose the traffic mirror filter that determines what traffic gets mirrored\.

1. \(Optional\) For each tag to add, choose **Add new tag** and enter the tag key and tag value\.

1. Choose **Create**\.

## Step 4: Analyze the data<a name="analyze-data"></a>

After the mirrored traffic is on the traffic mirror target, you can use a tool from the [AWS Partner Network](https://partners.amazonaws.com/search/partners/?keyword=traffic%20mirroring) to analyze the data\.