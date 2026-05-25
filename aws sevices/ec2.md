2. What are the different EC2 pricing models, and when would you use each?
Expected knowledge:

On-Demand – test/dev, unpredictable workloads.
In case of On-Demand, there are no long-term contracts. They bill you per hour for the compute capacity you use. Companies may scale up or down the capacity of their application based on demand, and they only pay for the hourly rate of the instance they select.



Reserved Instances – steady-state, predictable usage.

Savings Plans – flexible commitments.

Spot Instances – batch jobs, fault-tolerant workloads,stateless applications.

Dedicated Hosts – compliance-heavy use cases.

3. How do you secure access to EC2 instances?
Cover points like:

IAM roles for instance-level permissions.

Security Groups (stateful firewall).

NACLs (stateless).

Key Pairs for SSH access.

Session Manager via AWS Systems Manager as a better alternative to SSH.

. How can you implement high availability and fault tolerance with EC2?
Should include:

Using Auto Scaling across multiple AZs.

Load balancing via ELB (Application or Network Load Balancer).

Data replication using EBS snapshots, S3, or EFS across AZs/Regions.

Launch Templates for consistent and scalable deployments.

. What is the difference between EBS and Instance Store volumes?
Compare:

EBS – persistent, survives instance stop/start, supports snapshots.

Instance Store – ephemeral, physically attached to host, data loss on stop/terminate.

