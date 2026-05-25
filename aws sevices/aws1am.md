 What is IAM in AWS?
AWS IAM (Identity and Access Management) is a service that helps you securely control access to AWS services and resources. You can create users, groups, roles, and policies to manage permissions.

| IAM Concept | Description                                                          |
| ----------- | -------------------------------------------------------------------- |
| **User**    | Represents a person or service that uses AWS                         |
| **Group**   | A collection of users with shared permissions                        |
| **Role**    | A temporary identity used by services or users to assume permissions |

3. Difference between IAM Role and IAM User?

| Feature  | IAM User                  | IAM Role                          |
| -------- | ------------------------- | --------------------------------- |
| Identity | Persistent identity       | Temporary identity                |
| Login    | Has username/password     | Cannot login directly             |
| Use case | Human or long-term access | EC2, Lambda, Cross-account access |

4. What is a policy in IAM?
A policy is a JSON document that defines permissions (allow or deny actions on AWS resources). Policies can be:

Managed (AWS or customer-created)
Inline (directly attached to a user/group/role)

5. What is the principle of least privilege?
Grant users only the minimum permissions they need to perform their tasks — helps reduce security risks.

6. What is an IAM role used for in EC2?
An IAM role can be attached to EC2 so it can access AWS services (like S3 or DynamoDB) without storing credentials in the instance.

7. What is an assume role?
The sts:AssumeRole API allows a user/service to temporarily assume another role’s permissions, often used in:

Cross-account access
Lambda, ECS, EC2 roles
Federated identity providers

8. How do you secure IAM best practices?
Enable MFA (Multi-Factor Authentication)

Use roles for services, not long-term access keys
Rotate access keys regularly
Follow least privilege
Monitor IAM activity via CloudTrail

| Type               | Description                              |
| ------------------ | ---------------------------------------- |
| **Identity-based** | Attached to users, groups, roles         |
| **Resource-based** | Attached to resources (e.g., S3, Lambda) |


🔐 What is sts:AssumeRole?
sts:AssumeRole is an AWS Security Token Service (STS) API action that allows an IAM principal (user, role, or service) to temporarily take on the permissions of another IAM role.


2. What happens if a policy has both Allow and Deny?
Explicit Deny always overrides Allow. If a permission is both allowed and explicitly denied, the action will be denied.

3. What is a service-linked role?
A service-linked role is an IAM role that’s pre-defined by AWS for a service to perform actions on your behalf.

Example: AWSServiceRoleForEC2Spot


15. How do IAM permissions work in Lambda?
Lambda functions use an IAM execution role to access other services like S3, CloudWatch, or DynamoDB. This role must have the right permissions attached.

