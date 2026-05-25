2. How does a CDN work?
A user requests content (like a video or image).
The request is routed to the nearest edge server.
If cached, it's served from there.
If not, it’s fetched from the origin server, cached, and delivered to the user.

. What is a CDN?
A Content Delivery Network (CDN) is a globally distributed network of servers designed to deliver web content (static or dynamic) to users based on their geographic location, improving speed, reliability, and availability.

What are edge servers in a CDN?
Edge servers are geographically distributed servers that cache and serve content close to the end user to reduce latency.

 What content can a CDN cache?

Static files: HTML, CSS, JS, images, fonts, videos
Dynamic content (with customization)
APIs and JSON responses (with adjusted cache policies)

5. TTL defines how long a cached object stays in an edge server before it’s refreshed from the origin. Lower TTL = fresher content; higher TTL = better performance.

6. 7. What is cache invalidation?
Cache invalidation forces the CDN to remove outdated cached content, forcing it to fetch fresh content from the origin.

8. What is origin server in CDN?
An origin is the source of truth for content (e.g., S3, EC2, or a web server). The CDN pulls content from here when it's not cached.

13. What is geo-blocking in CDN?
Geo-blocking allows you to restrict access to content based on the user’s geographic location (e.g., block traffic from specific countries).

14. What is Signed URL or Signed Cookie in CDN?
They allow authenticated access to private content by generating a time-limited, signed token. Common in premium video streaming or file delivery.


| Component             | Purpose                                |
| --------------------- | -------------------------------------- |
| S3 Bucket             | Stores static content                  |
| CloudFront            | Delivers content globally with caching |
| Origin Access Control | Secure access to S3                    |
| ACM/Route 53          | Enable HTTPS + custom domain           |
