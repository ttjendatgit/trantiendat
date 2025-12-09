---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Four ways to grant cross-account access in AWS

**Authors:** Anshu Bathla and Jay Goradia on February 24, 2025 in AWS Identity and Access Management (IAM), Best Practices, Intermediate (200), Security, Identity, & Compliance, Technical Guide Permalink

As your Amazon Web Services (AWS) environment grows, you may need to grant resource access to multiple accounts. This can stem from various reasons, such as enabling centralized operations across multiple AWS accounts, sharing resources between teams or projects in an organization, or integrating with third-party services. However, granting access to multiple accounts requires careful consideration of security, availability, and manageability requirements.

In this blog post, we'll explore four different ways to grant cross-account access using resource-based policies. Each method has its own trade-offs, and the best choice depends on your specific requirements and use case.

**Evaluating different techniques to grant cross-account access**

Cross-account access is granted through identity-based policies and resource-based policies in AWS Identity and Access Management (IAM). Identity-based policies are attached to IAM roles, while resource-based policies are attached to resources like Amazon Simple Storage Service (Amazon S3) buckets and AWS Key Management Service (AWS KMS) keys. Resource-based policies require you to specify one or more principals (users or IAM roles) that are allowed to access the resource.

Your choice of how to define the principal in the resource-based policy will affect several aspects of both the security and availability of the solution. This article focuses on understanding this impact and selecting appropriate trade-offs for your use case.

**An example scenario**

Imagine you have an S3 bucket in an AWS account (Account A) that needs to be accessed by various principals in another AWS account (Account B). In this case, we assume that the principals in Account B have the necessary S3 access in their identity-based policies, and we will focus on creating resource-based policies in Account A. Although the methods explained here use Amazon S3, the concepts discussed apply to all AWS services that support resource-based policies. In the following sections, we'll guide you through four different ways to grant cross-account access in this case and discuss the trade-offs of each method.

**Method 1: Grant access to a specific IAM role using the Principal element of the resource-based policy**

In this example, you use a bucket policy to grant access to a specific IAM role (`RoleFromAccountB`) in Account B by specifying the Amazon Resource Name (ARN) of the IAM role in the `Principal` element of the policy in Account A.


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRoleInThePrincipalElement",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/RoleFromAccountB"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}

Using this bucket policy, if someone in Account B deletes or recreates the role (RoleFromAccountB), that role will no longer be able to access the amzn-s3-demo-bucket-account-a bucket, even if the role is recreated with the same name. The reason is that when you save this policy, the role's ARN is mapped to the role's unique ID, which looks something like this: AROADBQP57FF2AEXAMPLE. You'll see the role identifier in the Principal element of resource-based policies if you view them after deleting the role they referenced.

This behavior is intentional. Resource-based policies only allow the specific instance of the role that you set as the principal at the time of policy creation. This helps prevent unintended access to your resource if you delete a role but forget to update the resource-based policy to remove it. This behavior can also pose an availability risk because the role (RoleFromAccountB) will have a new unique ID when recreated and will no longer have access to the bucket. The role might be recreated for several reasons, including accidentally when you use tools like infrastructure as code.

You might consider choosing this method if:

You own the roles in both Account A and Account B and can control the creation and deletion of these roles.

You want the resource-based policy in Account A to stop granting access when the specified role (RoleFromAccountB) is deleted.

You prioritize granular access control over potential availability concerns if the role (RoleFromAccountB) is deleted.

**Method 2: Grant access to an account using the Principal element of the resource-based policy**

In this example, you grant access to a specific account in the Principal element of the resource-based policy. This resource-based policy from Account A allows any user or role from Account B that has an identity-based policy granting them read object permissions.

Note: You can use either "Principal": {"AWS": "111122223333"} or "Principal": {"AWS": "arn:aws:iam::111122223333:root"} in the Principal element. They are equivalent, and the long-form ARN does not represent the root user.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInThePrincipalElement",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}


This resource-based policy helps avoid the potential availability issue discussed in Method 1. If a role in Account B that needs access to the bucket is recreated, it will still have access after the role is recreated. This is because you don't specify the role in the Principal element—instead, you specify an account. If using Method 2, you must be comfortable delegating access control decisions to the owner of that account.

This method explicitly delegates access control decisions to IAM in the other account (Account B). Principals in Account B have access to this bucket if they are allowed by their identity-based policies.

You might consider choosing this method if:

You need to grant access to multiple principals in Account B.

You want to delegate access decisions to the account where the owner exists (Account B).

You prioritize ease of management and availability over granular access control.

**Method 3: Grant access to a specific IAM role using the aws:PrincipalArn condition**

This method builds upon Method 2 and adds a condition that grants access only to a specific IAM role. Similar to Method 2, you use the account number as the value of the Principal element, but also use the aws:PrincipalArn condition key to restrict access to a specific principal in Account B.

The aws:PrincipalArn condition key is a global condition key that compares the ARN of the principal making the request with the ARN you specify in the policy. For IAM roles, the request context returns the role's ARN, not the ARN of the user who assumed the role.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInPrincipalAndRoleInPrincipalArn",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "ArnEquals": {
          "aws:PrincipalArn": "arn:aws:iam::111122223333:role/RoleFromAccountB"
        }
      }
    }
  ]
}

This policy comes with similar availability benefits as the policy in Method 2: access to this resource will persist after role recreation. This is because a role is only translated to a unique identifier when used in the Principal element. It is not translated to a unique identifier when used in a condition. If the role (RoleFromAccountB) in Account B is recreated, whether accidentally or intentionally, the policy will continue to grant access because the role matches the role ARN specified in the condition key of the resource-based policy in Account A. Therefore, Method 3 provides a balanced approach between availability and security.

You might consider choosing this method if:

You are comfortable that this policy will continue to grant access to the role specified in the aws:PrincipalArn condition key if that role (RoleFromAccountB) is recreated.

You don't own Account B that you're granting access to and have no control over when the role might be recreated.

You want to balance availability with security.

**Method 4: Grant access to an entire AWS Organizations organization**

This method focuses on a different use case and is not a replacement for the methods listed above. Use this method if you have a resource (in this example, an S3 bucket) that you want to share with an entire organization, but not with anyone outside.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessToAnEntireOrganization",
      "Principal": {
        "AWS": "*"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgId": "o-12345"
        },
        "StringNotEquals": {
          "aws:PrincipalAccount": "${aws:ResourceAccount}"
        }
      }
    }
  ]
}

There is no way to specify an organization using the Principal element of a resource-based policy, so you must use the aws:PrincipalOrgId condition key to restrict access to a specific organization. In this policy, you specify a wildcard in the Principal element, indicating that anyone can access the bucket. Then, the condition reduces "anyone" to only AWS account principals that belong to the specified organization and have an identity-based policy allowing them access.

You then add an additional condition block to compare the aws:PrincipalAccount condition key with the aws:ResourceAccount condition key using a policy variable. This additional condition block is optional and excludes the bucket-owning account (Account A) from the allow statement. The reason to use this additional condition block is so that principals in Account A still need an allow statement in their identity-based policy to access this bucket. If you choose to exclude this aws:PrincipalAccount comparison, principals in Account A will be granted access to the bucket without an explicit allow statement in their identity-based policies. Policy evaluation logic only requires either an identity-based policy or a resource-based policy (but not both) to allow a request when the principal and resource are in the same account.

You might consider choosing this method if:

You have a shared resource that your entire organization should be able to access.

# Conclusion
Choosing a method for granting cross-account access requires careful consideration of your requirements and use cases. Each of the four methods discussed in this blog post has its own advantages and disadvantages. By understanding these methods and their implications, you can decide on the most appropriate approach to grant cross-account access to your AWS resources. Remember to regularly review and audit your resource-based policies to ensure they align with your security and access requirements.