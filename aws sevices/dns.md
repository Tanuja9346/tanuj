1. Routing Policy in Route 53

Routing policy decides how DNS traffic is routed to your resources.

Simple Routing:
One record → one resource
Used for single server or single application
Example: one EC2 hosting website

Weighted Routing:
Traffic distributed based on percentage
Example:
Server A → 80%
Server B → 20%
Used in:
load testing
blue-green deployment
canary deployment

Latency-based Routing:
Routes user to lowest latency region
Example:
India users → Mumbai region
US users → Virginia region
Improves performance

Failover Routing:
Primary and secondary server setup
If primary fails, traffic moves to standby
Requires health checks
Used for Disaster Recovery (DR)

Geolocation Routing:
Routes traffic based on user location
Example:
India users → Indian website
US users → US website
Used for regional content restriction

Geoproximity Routing:
Routes based on user and resource location
Can shift traffic using bias values
Mostly used in global applications

Multi-value Answer Routing:
Returns multiple healthy IPs
Provides simple load balancing
Improves availability

2. TTL (Time To Live)

TTL means how long DNS resolver caches the DNS record.

Example
TTL = 300 seconds
DNS cache stores record for 5 minutes
After 5 mins, it queries Route53 again
Low TTL

Example: 60 sec

Faster DNS updates
Good for failover
More DNS queries
High TTL

Example: 86400 sec

Better performance
Less DNS queries
DNS changes take longer
Real-time DevOps Usage
Production stable app → High TTL
Migration/failover → Low TTL

3. Alias Record

Alias record is an AWS-specific feature.
It maps domain directly to AWS resources without using IP address.

Supported AWS Services
EC2 Load Balancer
CloudFront
S3 Static Website
API Gateway
Elastic Beanstalk

Benefits:
No charges for Alias queries to AWS resources
Automatically tracks IP changes
Works at root domain

Example:
example.com → ALB

Without Alias:
CNAME not allowed at root domain

With Alias:
Root domain supported

4. Record Types in Route53

A Record:
Maps domain → IPv4 address

Example:
example.com → 192.168.1.10

AAAA Record:
Maps domain → IPv6 address

Example:
example.com → 2404:6800:4009::200e

CNAME Record:
Maps one domain name → another domain name

Example:
app.example.com → server.example.com

Cannot be used for root domain.
reason:
example.com → loadbalancer.amazonaws.com

this is invalid:
because root domain already requires:

NS records
SOA records
DNS standards do not allow CNAME there.

MX Record:
Used for mail servers.

Example:
example.com → mail.google.com

TXT Record:
Stores text information.

Used for:
SPF
DKIM
domain verification

Example:
v=spf1 include:_spf.google.com ~all

NS Record:
Specifies name servers for hosted zone.

Example:
ns-123.awsdns.com

SOA Record:

Contains zone administration details:
serial number
refresh timer
retry timer

PTR Record:

Used for reverse DNS lookup.
IP → Domain mapping

