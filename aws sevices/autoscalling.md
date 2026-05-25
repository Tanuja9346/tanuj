🔄 Auto Scaling Interview Questions

1. What is Auto Scaling in AWS?
Auto Scaling is a service that automatically adjusts the number of EC2 instances in a group based on load or defined conditions (e.g., CPU > 70%).

2. What are the components of Auto Scaling?
Launch Template or Launch Configuration

Auto Scaling Group (ASG)

Scaling Policies

Health Checks

3. Difference between Launch Configuration and Launch Template?
Feature	Launch Configuration	Launch Template
Flexibility	Less flexible	Supports multiple versions
Newer APIs	Not supported	Fully supported
Features like T2 Unlimited, Multiple NICs	Not supported	Supported

✅ Best Practice: Use Launch Templates for new setups.

4. What is an Auto Scaling Group (ASG)?
An ASG is a logical grouping of EC2 instances. It ensures that a minimum, maximum, and desired number of instances are running and can scale up/down based on policies.

5. How does Auto Scaling integrate with CloudWatch?
CloudWatch alarms can trigger scaling actions. Example:

CPU > 80% for 5 mins → Add 2 instances

CPU < 30% for 10 mins → Remove 1 instance

6. What are different types of scaling policies?
Target Tracking Scaling: Keep a metric (e.g., CPU) at a target value

Step Scaling: Scale in/out by steps based on thresholds

Simple Scaling: Add/remove fixed instances based on alarm

Scheduled Scaling: Scale at a specific time (e.g., weekends)

7. What happens if an instance in an ASG fails?
Auto Scaling will detect the failed instance using health checks and automatically replace it.

🎯 Target Group Interview Questions
8. What is a Target Group in AWS?
A Target Group routes traffic to EC2 instances, IPs, Lambda functions, or containers behind a Load Balancer.

9. How does Auto Scaling use Target Groups?
The ASG registers its EC2 instances with the Target Group automatically so that the Load Balancer can route traffic to healthy instances.

10. What health checks are used in Target Groups?
HTTP/HTTPS checks on specific ports and paths

Used by Load Balancer to determine healthy targets

11. Can a Target Group support multiple ports?
No. A single target group supports one port per target. For multiple ports, use multiple target groups.

12. Types of Target Groups:
Instance Target: Routes traffic to EC2 by instance ID

IP Target: Routes to specific private IPs

Lambda Target: Direct invocation of Lambda functions

🚀 Launch Template Interview Questions
13. What is a Launch Template?
A Launch Template defines the configuration for launching EC2 instances, including:

AMI

Instance type

Key pair

Security groups

IAM roles

User data (bootstrap scripts)

14. Benefits of Launch Templates over Launch Configurations?
Support for versioning

Compatible with EC2 Spot Fleet, EC2 Auto Scaling, and Mixed Instance policies

Supports advanced networking (e.g., ENIs)

15. How do Launch Template versions work?
You can create multiple versions (v1, v2, etc.). When used in an ASG, you can specify the default or a specific version.

16. How do you automate Auto Scaling deployments with Launch Templates?
Use CloudFormation, Terraform, or Boto3

Define the Launch Template

Create an ASG that references the template

Attach a Target Group and scaling policies

17. What happens if your AMI in a launch template is deleted?
The ASG will fail to launch new instances. Always version-control and maintain your base AMIs using tools like EC2 Image Builder.

