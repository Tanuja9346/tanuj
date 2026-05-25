Questions
🔹 1. What is AWS Lambda?
Answer:
AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. You only pay for the compute time your code consumes.

 2. What are key features of AWS Lambda?
Event-driven execution
Automatic scaling
Supports multiple runtimes (Python, Node.js, Java, Go, etc.)
Built-in integrations with AWS services (S3, DynamoDB, API Gateway, etc.)
Supports environment variables
Concurrency control

Concurrency control in AWS Lambda means controlling how many Lambda instances can run at the same time.

Each time a Lambda function is invoked, AWS may create a new instance to handle that request. If 100 requests arrive simultaneously, up to 100 Lambda instances may run in parallel (depending on your concurrency settings).

3. What triggers can invoke a Lambda function?


| Trigger Source    | Example                               |
| ----------------- | ------------------------------------- |
| S3                | File uploads (e.g., image processing) |
| API Gateway       | REST APIs and WebSockets              |
| DynamoDB Streams  | DB change events                      |
| CloudWatch Events | Scheduled (cron) jobs                 |
| SNS/SQS           | Message processing                    |
| EventBridge       | Event-driven applications             |
| ALB               | HTTP-based invocation                 |

4. 
Client (Browser/App)
       ↓
  API Gateway (REST API)
       ↓
   AWS Lambda (Backend Code)
       ↓
DynamoDB / RDS / S3 (Data Storage)

. How does Lambda pricing work?
Answer:
Charged by:

Number of invocations
Duration (rounded to nearest millisecond)
Memory allocated (128MB–10GB)
1 million free requests/month + 400,000 GB-seconds free

5. What is the Lambda execution role?
Answer:
The IAM role that Lambda assumes at runtime to:

Read/write from AWS services (e.g., S3, DynamoDB)
Log to CloudWatch
Access secrets

6. What are Lambda layers?
Answer:
Layers allow you to package shared libraries, dependencies, or custom runtimes and attach them to multiple Lambda functions.

Use cases:
Reuse common Python packages
Custom runtime environments
Security tools

 7. What is the maximum timeout for Lambda?
Answer:
Maximum timeout is 15 minutes (900 seconds) per invocation.

8. Can Lambda functions run in a VPC?
Answer:
Yes. You can configure a Lambda to access private subnets and resources (like RDS) by attaching it to a VPC.

Important to also set:

Subnet IDs
Security groups
NAT Gateway (if accessing internet)

9.  9. What are cold starts in Lambda?
Answer:
Cold starts occur when a new Lambda execution environment is initialized (e.g., after idle period or first invocation). It adds latency due to container setup.

To reduce cold start:

Use Provisioned Concurrency{Pre-warmed environments for no cold starts i.e create containers before
}
provisied concurrency = 10 i.e it creates 10 comntainers no cold start
Optimize function size/startup time

11. What is Provisioned Concurrency?
Answer:
It pre-warms a number of Lambda instances so that they are ready to respond immediately, eliminating cold starts.


12. Can Lambda have environment variables?
Answer:
Yes. Environment variables can store:

Configurations
Secrets (encrypted with KMS)
Region settings, flags

 13. How do you secure sensitive data in Lambda?
Answer:

Use KMS-encrypted environment variables
Access secrets from AWS Secrets Manager or SSM Parameter Store
Grant least-privilege IAM execution roles

14. How does Lambda handle retries on failure?
Depends on the trigger:

| Trigger           | Retry Behavior                         |
| ----------------- | -------------------------------------- |
| S3                | Retry up to 3 times                    |
| CloudWatch Events | No retry                               |
| SQS               | Retries with dead-letter queue support |
| API Gateway       | No retry, returns error to client      |
15. What is the difference between synchronous and asynchronous invocation in Lambda?
Mode	Behavior
Synchronous	Waits for response (e.g., API Gateway)
Asynchronous	Event is queued, Lambda runs later (e.g., S3, SNS)


What’s the difference between Lambda and Fargate?

| Feature  | Lambda                         | Fargate                 |
| -------- | ------------------------------ | ----------------------- |
| Use case | Short-lived, event-driven      | Long-running containers |
| Duration | Max 15 mins                    | No time limit           |
| Startup  | Fast, but cold starts possible | Slower than Lambda      |

 17. Can Lambda call another Lambda?
Answer:
Yes. You can:

Directly invoke using the AWS SDK

Use SNS, SQS, EventBridge as intermediaries

Orchestrate with Step Functions

🔹 18. What is the Lambda execution context?
Answer:
It’s the temporary runtime environment where your function runs. Includes:

Memory and disk (/tmp up to 10GB)

Execution role permissions

Reused for subsequent invocations (warm start)

🔹 19. What’s inside the Lambda event and context objects?
Answer:

event: The trigger payload (e.g., S3 object info, API request)

context: Metadata about the invocation (function name, memory limit, remaining time)

🔹 20. How do you manage versions and aliases in Lambda?
Answer:

Versions: Immutable snapshots of your Lambda code

Aliases: Pointers to versions (e.g., prod, dev) for blue/green deployments

API Gateway Concept in AWS
Amazon API Gateway is a fully managed AWS service that helps you create, publish, maintain, monitor, and secure APIs at any scale. It's the "front door" for applications to access backend services like:

AWS Lambda

EC2

AWS Fargate

DynamoDB / RDS

Any HTTP/HTTPS endpoint


 @@@ What is an Endpoint in API Gateway (or in general APIs)?
An endpoint is a URL (Uniform Resource Locator) that allows clients (like browsers, apps, or tools like Postman) to access a specific function or resource exposed by an API.

rough notes::::
----------------
no maintaience
scalibility --> 1k users at a time can hit 
pay as u use87
ha
monitoring
logging bydefaullt store in cloudwatch.
faas
event based triggers: 

@@@create lamdafunction,write code which langauage u want,role,create.test and deploy

***deploy the python packages and deployment.
upload file in zip formate.and s3 zip formate.

* any dependencie library required install in work environment by creating venv.
* now take this libary to aws environment.

create python folder pick libaray related in venv folder i.e work env folder and move to pyhton folder and zip the python package folder.{i.e directly all files are ziped clicked on that folder}

* configurations and settings:
triggers: upload a file in s3 it will trigger lamda function so it is event driven

destination: send/store output of function into a other.

Layers: used for keep libraries for reduce code package size in python.so creste layers and add the layers to main code.

@@ s3 event notification : create in lamda or s3 console based on event type i.e put,post etc.suffix and prefix. whenever u pushed or done action on s3 it will automatically trigger based on action and u can see cloudwatch logs what happend in trigger.


A REST API (Representational State Transfer Application Programming Interface) is a way for two computer systems to communicate over the internet using HTTP protocols in a simple, standardized manner.

🔹 Key Concepts of REST API:
Stateless:
Each request from a client to the server must contain all the information needed. The server does not remember past requests.

Uses HTTP Methods:
REST APIs rely on standard HTTP methods:

GET → Retrieve data

POST → Create data

PUT → Update data

DELETE → Remove data

Resource-Based:
Everything is treated as a resource, and is identified by a URL.
Example:

GET /users → Get all users

GET /users/123 → Get user with ID 123

POST /users → Create a new user

Uses JSON or XML:
Most modern REST APIs use JSON (JavaScript Object Notation) to send and receive data.

Stateless and Cacheable:
Since each request is independent, responses can be cached to improve performance.

-----alias and versions:

u can make it diff version under single function. and u want call  version not with function name but with alias u can use.


snapstart: enable it and version it  and aws move this code to fire cracker vm here excuted thing create snapshot of memory and diskstate and stored in cache env when user hit vwersion it takes from restore and invoke and shutdown.
give 10* performance.only for java.