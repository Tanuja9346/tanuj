1. What is an Application Load Balancer (ALB)?
Answer:
An Application Load Balancer is a Layer 7 (HTTP/HTTPS) load balancer in AWS that routes traffic based on content, such as:

Host-based routing (example.com)

Path-based routing (/api, /images)

Header-based or query string routing

 2. ALB vs NLB vs CLB – Key Differences?

| Feature         | ALB                     | NLB                    | CLB (Classic LB)  |
| --------------- | ----------------------- | ---------------------- | ----------------- |
| Layer           | 7 (HTTP/HTTPS)          | 4 (TCP/UDP)            | 4 & 7             |
| Routing         | Content-based           | IP/Port-based          | Basic round robin |
| Target Support  | EC2, IPs, Lambda        | EC2, IPs, ALBs         | EC2 only          |
| Sticky Sessions | Supported (via cookies) | Not natively supported | Supported         |
| SSL Termination | Yes                     | Yes                    | Yes               |

3. 3. What are Target Groups in ALB?
Answer:
Target groups define the backends for an ALB. Each target group can:

Contain EC2 instances, IP addresses, or Lambda functions

Be registered by port

Have its own health checks

Be used in rules to route traffic conditionally
Lower number = higher priority.



5. Can one ALB serve multiple domains or applications?
Answer:
Yes, using host-based rules, you can serve multiple apps/domains from a single ALB:

app1.example.com → Target group A

app2.example.com → Target group B

This is common in multi-tenant or microservices setups.

6. What protocols does ALB support?
Answer:

HTTP

HTTPS

WebSockets (over HTTP/S)

WebSockets are a full-duplex communication protocol over a single, long-lived TCP connection that allows real-time data flow between a client (usually a browser) and a server.

Unlike HTTP (which is request-response based), WebSockets allow:

Persistent connection

Low-latency, real-time communication

Bidirectional data transfer
use case: chat applicationsd,online gaming, live dashboards,collabarative apps, notificatiob system.

 7. How does SSL/TLS termination work in ALB?
Answer:
ALB offloads SSL at the load balancer. You:

Attach an SSL certificate (from ACM or uploaded) to the ALB listener

ALB handles encryption/decryption

Backend sees unencrypted HTTP traffic (unless re-encryption is used)

8. What are ALB listener rules?
Answer:
Rules define how to match and route requests using:

Conditions: Host, path, headers, query strings,sourceip,http header,http method

Actions: Forward to target group, redirect, return fixed response


🔹 9. What is connection draining / deregistration delay?
Answer:
When a target is removed, ALB waits for in-flight requests to complete (default is 300 seconds). This is known as deregistration delay.

.

🔹 10. What types of health checks does ALB support?
Answer:

Protocol: HTTP or HTTPS
Path: Customizable (e.g., /health, /status)
Frequency, timeout, and thresholds are configurable
Only healthy targets receive traffic

.

🔹 10. What types of health checks does ALB support?
Answer:

Protocol: HTTP or HTTPS
Path: Customizable (e.g., /health, /status)
Frequency, timeout, and thresholds are configurable
Only healthy targets receive traffic

configurable

🔹 12. Can you restrict access to ALB?
Answer:
Yes:

Use Security Groups (inbound rules)

Use WAF (Web Application Firewall)

Use Listener rules (return 403, redirect)

Use Shield Advanced for DDoS protection

14. What is the idle timeout in ALB?
Answer:
The default is 60 seconds. It defines how long a connection is kept open when idle before being closed.

 13. How do ALB access logs work?
Answer:

Logs are stored in S3.

Can be enabled per load balancer.

Includes request time, latency, backend response code, etc.

Used for analytics and debugging.

15. How is Lambda integrated with ALB?
Answer:

ALB can directly invoke Lambda functions as targets.

You must define a target group of type "Lambda".

Good for serverless web apps, lightweight APIs.

🔹 16. How does ALB support HTTP/2 and WebSockets?
Answer:

HTTP/2: Automatically supported over HTTPS.

WebSockets: Fully supported for real-time apps.

17. Can an ALB span multiple Availability Zones?
Answer:
Yes, and it’s recommended. You must enable at least 2 AZs for high availability.

19. How do you secure an ALB endpoint?
Answer:

Use HTTPS with SSL certificates

Enable WAF

Restrict Security Groups

Use private ALBs (internal only)

Enforce TLS policies


 20. What is ALB’s pricing model?
Answer:

Charged by:

Hours the ALB is running
Number of new connections
Data processed (GB)
Separate charges for WAF, Shield, and logging


