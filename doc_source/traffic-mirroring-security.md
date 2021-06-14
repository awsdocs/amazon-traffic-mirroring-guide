# Identity and access management for Traffic Mirroring<a name="traffic-mirroring-security"></a>

## <a name="traffic-mirror-session-policy"></a>

AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use traffic mirror resources\.

To allow access to traffic mirror resources, you create and attach an IAM policy either to:
+ The IAM user or
+ The group to which the IAM user belongs\.

The IAM user must be given permission to use the specific traffic mirror resources and API actions they need\. When you attach a policy to a user or group of users, it allows or denies permission to perform specified tasks on specified resources\. 

You can also use resource\-level permissions to restrict what resources users can use when they invoke APIs\.

**Example: CreateTrafficMirrorSession policy**  
The following IAM policy allows users to use the `CreateTrafficMirrorSession` API, but restricts the action to a specific traffic mirror target \(`tmt-12345645678`\)\. In order to create a traffic mirror session, users must also have permission to use the traffic mirror filter and network interface resources\. Therefore, you must include these resources in your IAM policy\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:CreateTrafficMirrorSession",
            "Resource": [
                "arn:aws:ec2:*:*:traffic-mirror-target/tmt-12345645678",
                "arn:aws:ec2:*:*:traffic-mirror-filter/*",
                "arn:aws:ec2:*:*:network-interface/*"
            ]
        }
     ]
}
```

For more information about supported traffic mirror actions, resources, and condition keys, see [Actions, Resources, and Condition Keys for Amazon EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonec2.html) in the *IAM User Guide*\.