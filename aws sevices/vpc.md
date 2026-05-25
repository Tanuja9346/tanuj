Q: What is VPC in AWS?
Ans:
A VPC (Virtual Private Cloud) is a logically isolated section of the AWS cloud where we can launch AWS resources in a virtual network that we define. It gives us complete control over network configuration, including IP ranges, subnets, route tables, gateways, and security.
It's used to isolate workloads, implement strict security boundaries, and architect multi-tier applications with full control over networking.

Q2. What’s the difference between a public and a private subnet?
Answer:

A public subnet has a route to an Internet Gateway (IGW), allowing instances to connect to the internet directly.

A private subnet has no direct route to the internet. To access the internet, instances typically use a NAT Gateway/Instance in a public subnet.

Q3. What is a NAT Gateway and how is it different from a NAT Instance?
Answer:

NAT Gateway is a managed service by AWS that allows instances in private subnets to access the internet for updates, etc., without exposing them.

NAT Instance is a manually configured EC2 instance used similarly, but it needs maintenance, scaling, and patching.
NAT Gateway is preferred for high availability and lower maintenance.


Q: How many subnets can a VPC have? OR How many subnets can we create per VPC?
Ans:
Currently, user can create 200 subnets per VPC. If want to create more, need to submit a case at the support center.

Q: What are the components of VPC?
Ans:
AWS VPC is made up of multiple networking components, some of which are as follows:
CIDR block
Subnets (public/private)
Route tables
Internet Gateway (IGW)
NAT Gateway/Instance
VPC Peering / Transit Gateway
Security Groups
Network ACLs
DHCP Options Set

2. Explain the difference between Security Groups and Network ACLs.

| Feature         | Security Group       | NACL                    |
| --------------- | -------------------- | ----------------------- |
| Scope           | Instance-level       | Subnet-level            |
| Stateful        | Yes                  | No                      |
| Rule direction  | Inbound & Outbound   | Inbound & Outbound      |
| Rule evaluation | Allow only           | Allow & Deny            |
| Use case        | Fine-grained control | Broad subnet protection |


3. How do you design a secure VPC for a 3-tier web application?
Expected design:
 VPC with 3 AZs for high availability.

Public Subnet for ALB or NGINX (Web Tier).

Private Subnet (App Tier) for app servers.

Private Subnet (DB Tier) with no internet access, with RDS deployed.

Use NAT Gateway in public subnet for app servers to access the internet.

Apply Security Groups and NACLs for network control.

Use CloudWatch + VPC Flow Logs for monitoring.



4. How do you troubleshoot if EC2 in private subnet can't access internet?
Checklist:

Check subnet’s route table – should point to NAT Gateway.

Ensure NAT Gateway is in public subnet with an IGW and EIP.

Verify Security Group allows outbound traffic.

Check NACLs – allow ephemeral ports.

Confirm DNS resolution is enabled in VPC settings.


5. How do you secure resources in a VPC?
Answer:

Security Groups for instance-level traffic control.
NACLs for subnet-level stateless control.
Use private subnets for sensitive resources (DB, backend).
Enable VPC Flow Logs for monitoring.
Implement IAM policies + endpoint policies for services like S3/DynamoDB.
Use Bastion Host or SSM Session Manager to access private EC2s.

6.  What is VPC Peering, and how is it different from Transit Gateway?
Answer:

VPC Peering allows direct connection between two VPCs. It's point-to-point and does not support transitive routing.

Transit Gateway is a scalable hub-and-spoke model to connect multiple VPCs and on-prem. It supports transitive routing and is more scalable.

Use Transit Gateway for complex architectures involving many VPCs or hybrid setups.

📘 Example of Transitive Routing:
Imagine you have three VPCs:

VPC A

VPC B

VPC C

❌ Without Transitive Routing (VPC Peering):
You create:

A peering connection between A ↔ B

A peering connection between B ↔ C

You cannot send traffic directly from A → C through B.
VPC Peering is non-transitive – traffic must be routed directly between connected VPCs.

✅ With Transitive Routing (Transit Gateway):
If all 3 VPCs are attached to an AWS Transit Gateway (TGW):

A ↔ TGW ↔ B

C ↔ TGW

Now, A can talk to C through the TGW, even though A and C are not directly connected.
This is transitive routing: A → TGW → C.
7. Can you explain a real-time VPC issue you faced and how you resolved it?
Answer:
Once, an EC2 instance in a private subnet couldn't reach the internet.
I checked:

Route table: had no route to NAT.

NAT Gateway: was in a different AZ and had no Elastic IP.

I fixed the route table and associated the correct NAT Gateway.

Verified that the NAT Gateway had an EIP and was in a public subnet.

The issue was resolved after updating the route and EIP assignment.


rules of peering:
'
1. cidr should not overlap
2. no transitive routing
3. i:i connection
4. cross account peering connection supported
5. cross region supported
6. routetable update required
7. sg and nacl should configured.
8. dns resolution should apply
Peered VPCs cannot route traffic through each other’s IGW, NAT, or VPN.
Each VPC needs its own IGW or NAT.
9. 10. Tagging and IAM Policies



