# Traffic Mirroring Authentication and Access Control<a name="traffic-mirroring-security"></a>

## <a name="traffic-mirror-session-policy"></a>

To allow access to Traffic Mirror resources, you must create and attach an IAM policy to the IAM user or the group to which the IAM user belongs\. The IAM user must be given permission to use the specific Traffic Mirror resources and API actions they need\. When you attach a policy to a user or group of users, it allows or denies permission to perform the specified tasks on the specified resources\. 

You can also use resource\-level permissions to restrict what resources users can use when they invoke APIs\. For example, the following IAM policy restricts the Traffic Mirror target tmt\-12345645678 that can be used in a `CreateTrafficMirrorSession` API call by a user:

**Example Example: CreateTrafficMirrorSession Policy**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:CreateTrafficMirrorSession",
            "Resource": [
                "arn:aws:ec2:*:*:traffic-mirror-target/tmt-12345678",
                "arn:aws:ec2:*:*:traffic-mirror-filter/*",
                "arn:aws:ec2:*:*:network-interface/*"
            ]
        }
     ]
}
```

All Traffic Mirror APIs support tag\-based authentication\. For information about the Amazon EC2 resource\-level permissions, see [Supported Resource\-Level Permissions for Amazon EC2 API Actions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-iam-actions-resources.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
The Network Load Balancer in the `CreateTrafficMirrorTarget` API does not support tag\-based authorization\.