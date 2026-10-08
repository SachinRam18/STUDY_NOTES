# AWS INTERVIEW NOTES — FRESHER / ENTRY-LEVEL
**Resume says:** AWS (Basics) · For: Software Engineer / Full-Stack Developer

> **Goal:** Understand each service, explain it simply, answer basic questions confidently.

---

# TABLE OF CONTENTS
1. [EC2](#1-ec2--elastic-compute-cloud)
2. [S3](#2-s3--simple-storage-service)
3. [IAM](#3-iam--identity-and-access-management)
4. [RDS](#4-rds--relational-database-service)
5. [VPC](#5-vpc--virtual-private-cloud)
6. [Lambda](#6-lambda--serverless-functions)
7. [CloudWatch](#7-cloudwatch--monitoring--logging)
8. [Other Services (Brief)](#8-other-services-brief)
9. [How AWS Services Work Together](#9-how-aws-services-work-together)
10. [How Would You...? Questions](#10-how-would-you-questions)
11. [Key Comparisons](#11-key-comparisons)
12. [Interview Questions](#12-interview-questions)
13. [How to Answer AWS Questions](#13-how-to-answer-aws-questions)
14. [Resume-Based Questions](#14-resume-based-questions)
15. [Last-Minute Revision](#15-last-minute-revision)

---

# 1. EC2 — Elastic Compute Cloud

## What is it?
EC2 is a **virtual server in the cloud**. Instead of buying a physical machine, you rent one from AWS. You choose the OS, hardware, and networking — and in minutes you have a Linux/Windows server ready to use. You can run any application on it: Spring Boot, FastAPI, Node.js, etc.

## Why use it?
- Host your backend application 24/7
- Full control — install software, open ports, run any process
- Pay only for what you use (by the hour/second)
- No hardware management

## Important Concepts

| Concept | Meaning |
|---|---|
| **Instance** | One virtual machine running in EC2 |
| **AMI** | OS template (e.g., Ubuntu 22.04). Choose this when launching |
| **Instance Type** | Hardware specs: CPU + RAM. `t2.micro` = 1 CPU, 1 GB RAM (free tier) |
| **Key Pair** | SSH authentication. AWS keeps public key; you download `.pem` file |
| **Security Group** | Virtual firewall. Controls which ports are open. Default = all blocked |
| **Public IP** | Temporary IP — changes on restart. Use **Elastic IP** for a static one |
| **EBS** | The hard disk attached to EC2. Persists when instance is stopped |
| **Stop vs Terminate** | Stop = paused, data kept. Terminate = permanently deleted |

## Basic Flow

```
Developer
   ↓
AWS Console → Launch EC2 (choose AMI + instance type + key pair + SG)
   ↓
EC2 boots (Ubuntu server in AWS data center)
   ↓
SSH in → install Java/Python → run your app
   ↓
Security Group opens port 8080
   ↓
Users access: http://<ec2-public-ip>:8080
```

## Simple Practical Example

> "I build a Spring Boot backend. I deploy it to EC2 by:
> 1. Building the JAR (`mvn package`)
> 2. Copying it to EC2 (`scp`)
> 3. SSHing into EC2 and running `nohup java -jar app.jar &`
> 4. Opening port 8080 in the security group"

```bash
# SSH into EC2
ssh -i my-key.pem ubuntu@<public-ip>

# Run app in background (keeps running after SSH closes)
nohup java -jar app.jar > app.log 2>&1 &
```

## Interview Questions

### Basic
**Q: What is EC2?**
A: A virtual server in AWS. You choose the OS and hardware, SSH in, and run your application on it.

**Q: What is an AMI?**
A: Amazon Machine Image — the OS template used to create an EC2 instance (e.g., Ubuntu 22.04).

**Q: What is a security group?**
A: A virtual firewall that controls inbound/outbound traffic. All inbound traffic is blocked by default. You add rules to allow specific ports.

**Q: What is the difference between a public IP and an Elastic IP?**
A: Public IP is temporary — it changes every time the instance restarts. Elastic IP is static and stays the same even after restart.

**Q: What is the difference between stopping and terminating an EC2 instance?**
A: Stop = instance shuts down, EBS data is saved, you can restart it. Terminate = instance is permanently deleted.

### Practical
**Q: How do you deploy a Spring Boot app to EC2?**
A: Build the JAR → `scp` it to EC2 → SSH in → install JDK → run `nohup java -jar app.jar &` → open the port in security group.

**Q: How do you keep an app running after closing SSH?**
A: Use `nohup java -jar app.jar > app.log 2>&1 &`. The `nohup` command detaches the process from the SSH session.

**Q: How do you pass DB credentials to an app on EC2 securely?**
A: Use environment variables (`export DB_PASSWORD=xxx`). Never hardcode credentials in code or config files.

### Scenario
**Q: After EC2 restarts, your React frontend can't connect to the backend. Why?**
A: The public IP changed on restart. Fix: assign an Elastic IP so the IP stays the same.

**Q: Users can't reach your app on port 8080. What do you check?**
A: Check the security group inbound rules — port 8080 must be open. Also verify the app is actually running.

**Q: You deployed an update. Old app is still running. What do you do?**
A: Find the old process (`ps aux | grep java`), kill it (`kill <pid>`), then start the new JAR.

---

# 2. S3 — Simple Storage Service

## What is it?
S3 is AWS's **file storage service**. You store files (called objects) inside containers called buckets. Think of it as Google Drive for your application — store images, documents, videos, backups, or even host a static website.

## Why use it?
- Unlimited storage, pay per GB used
- Every file gets a URL — easy to share or embed
- Much cheaper and more durable than storing files on EC2's disk
- Files survive even if the EC2 instance is deleted

## Important Concepts

| Concept | Meaning |
|---|---|
| **Bucket** | Container for storing files. Name must be globally unique |
| **Object** | A file stored in S3 |
| **Key** | The file's path/name inside the bucket. e.g., `uploads/user1/photo.jpg` |
| **Private by default** | All objects are private. You must explicitly allow public access |
| **Bucket Policy** | JSON rules that control who can access the bucket |
| **Pre-signed URL** | Temporary URL to access a private file for a limited time (e.g., 1 hour) |
| **Static Website Hosting** | S3 can serve your React build files as a website — no server needed |

## Basic Flow

```
User picks a file in React
   ↓
React sends file → POST /upload → Backend (EC2)
   ↓
Backend uploads file to S3 using AWS SDK
   ↓
S3 stores file, backend saves the URL in database (RDS)
   ↓
React shows the image using the S3 URL
```

## Simple Practical Example

> "In my project, users upload profile pictures. The React frontend sends the image to the FastAPI backend. The backend uploads it to an S3 bucket and saves the URL in the database. To display the image, React just uses the URL stored in the DB."

```python
# Upload a file to S3 (Python/boto3) — minimal example
import boto3
s3 = boto3.client('s3')  # uses IAM role automatically on EC2
s3.upload_fileobj(file.file, 'my-bucket', 'uploads/photo.jpg')
```

## Interview Questions

### Basic
**Q: What is S3?**
A: S3 is AWS's object storage service. You store files in buckets. Each file gets a unique key and a URL. Used for images, documents, backups, and static website hosting.

**Q: What is a bucket?**
A: A container for storing objects. Bucket names must be globally unique across all AWS accounts.

**Q: Are S3 objects public or private by default?**
A: Private by default. You must explicitly configure public access via a bucket policy.

**Q: What is a pre-signed URL?**
A: A temporary URL that gives time-limited access to a private S3 object. The backend generates it and sends it to the client. After the expiry, the URL stops working.

**Q: Why store uploaded files in S3 instead of on EC2?**
A: EC2's disk is limited, expensive per GB, and data can be lost if the instance is terminated. S3 is unlimited, cheap, highly durable, and every file gets a URL.

### Practical
**Q: How does a backend upload a file to S3?**
A: Use the AWS SDK (boto3 for Python, AWS SDK for Java). Initialize an S3 client — if on EC2 with an IAM role attached, no credentials needed. Call `upload_fileobj()` with the bucket name and key.

**Q: How do you allow a user to download a private S3 file?**
A: Generate a pre-signed URL in the backend using the SDK. Return it to the frontend. The user's browser uses the URL directly to download from S3. Set an expiry (e.g., 15 minutes).

**Q: How do you host a React app on S3?**
A: `npm run build` → upload `/build` folder to an S3 bucket → enable Static Website Hosting → set index document to `index.html`.

### Scenario
**Q: Users upload profile images. Walk me through the flow.**
A: React sends image to backend → backend uploads to S3 with a unique key (e.g., `users/123/profile.jpg`) → backend saves the S3 URL to the database → frontend loads the image from that URL.

**Q: A user wants to download their private contract PDF. How do you handle it?**
A: Backend authenticates the user, checks ownership, generates a pre-signed URL (valid 15 min), returns it to frontend. Frontend redirects user to the URL. S3 serves the file directly.

**Q: Two users upload files with the same name. What happens?**
A: If the key is the same, one overwrites the other. Fix: use unique keys like `uploads/{user_id}/{uuid}_{filename}`.

---

# 3. IAM — Identity and Access Management

## What is it?
IAM controls **who can access what** in your AWS account. You create users (for people), groups (for teams), and roles (for AWS services) — and attach policies that define what they're allowed to do.

## Why use it?
- Control permissions for every person and service
- Prevent unauthorized access
- Give applications access to other AWS services safely (without hardcoding keys)
- Follow the principle of least privilege

## Important Concepts

| Concept | Meaning |
|---|---|
| **IAM User** | Identity for a person. Has username + password or access keys |
| **IAM Group** | Collection of users. Assign permissions to the group |
| **IAM Role** | Identity for an AWS service (e.g., EC2). No permanent credentials — uses temporary tokens |
| **IAM Policy** | JSON that defines what actions are allowed/denied on which resources |
| **Access Keys** | Key ID + Secret for programmatic access. **Never hardcode in code** |
| **Least Privilege** | Give only the minimum permissions needed — nothing more |
| **Root Account** | The master AWS account. Never use it for daily work |

## Basic Flow

```
EC2 needs to access S3
   ↓
Create IAM Role → attach S3 access policy
   ↓
Attach role to EC2 instance
   ↓
AWS SDK on EC2 auto-fetches temporary credentials from the role
   ↓
S3 API call succeeds — no hardcoded keys needed
```

## Simple Practical Example

> "In my project, the EC2 backend uploads files to S3. Instead of hardcoding AWS access keys in the code (which is dangerous), I attach an IAM role to the EC2 instance that has permission to write to S3. The SDK picks up credentials automatically."

```python
# WRONG — never do this
s3 = boto3.client('s3', aws_access_key_id='AKIA...', aws_secret_access_key='xxx')

# RIGHT — with IAM role on EC2, no credentials needed
s3 = boto3.client('s3')
```

## Interview Questions

### Basic
**Q: What is IAM?**
A: IAM (Identity and Access Management) is AWS's service for managing access to AWS resources. It lets you control who can do what using users, groups, roles, and policies.

**Q: Difference between IAM User and IAM Role?**
A: User is for humans — has permanent credentials (password/keys). Role is for AWS services (like EC2) — uses temporary credentials, auto-renewed, no permanent keys stored.

**Q: What is an IAM Policy?**
A: A JSON document defining what actions are allowed or denied on which resources. Attached to users, groups, or roles.

**Q: Why should you NOT hardcode AWS access keys in your code?**
A: If the code is pushed to GitHub (even accidentally), the keys are exposed publicly. Anyone can access your AWS account. Use IAM roles instead — they're automatic and don't need to be stored anywhere.

**Q: What is the principle of least privilege?**
A: Give each user or service only the minimum permissions needed for their task. Don't give admin access when read-only is enough.

### Practical
**Q: How do you give EC2 permission to upload to S3?**
A: Create an IAM role with an S3 write policy. Attach the role to the EC2 instance. The AWS SDK automatically uses the role's temporary credentials — no manual configuration needed in code.

**Q: A developer left the company. How do you revoke their AWS access?**
A: Go to IAM → Users → select the user → disable or delete the user. All their access keys and console access are immediately revoked.

### Scenario
**Q: Your AWS access key was accidentally pushed to GitHub. What do you do?**
A: Immediately go to IAM → delete that access key. Create a new one. Check CloudTrail for any unauthorized activity. Use IAM roles in future to avoid this completely.

**Q: A Lambda function needs to write to S3. How do you give it permission?**
A: Create an IAM role with S3 write permission. Assign it as the Lambda function's execution role. Lambda uses it automatically.

---

# 4. RDS — Relational Database Service

## What is it?
RDS is AWS's **managed relational database service**. Instead of installing MySQL or PostgreSQL on a server yourself, AWS gives you a fully managed database instance. You connect to it exactly like a regular database — same connection string, same queries.

## Why use it?
- AWS handles backups, patches, and maintenance automatically
- Easy to connect from any backend (Spring Boot, FastAPI)
- More reliable than self-managing a DB on EC2
- Supports MySQL, PostgreSQL, MariaDB, SQL Server, Oracle

## Important Concepts

| Concept | Meaning |
|---|---|
| **DB Instance** | One managed database server running in AWS |
| **Engine** | The DB type: MySQL, PostgreSQL, etc. |
| **Endpoint** | The hostname your app uses to connect (e.g., `mydb.xxx.rds.amazonaws.com`) |
| **Port** | MySQL = 3306, PostgreSQL = 5432 |
| **Publicly Accessible** | Should be **NO** in production. DB should not be reachable from internet |
| **Security Group** | Controls who can connect. Set to allow only EC2's security group |
| **Automated Backups** | AWS takes daily backups automatically |
| **db.t3.micro** | Free-tier eligible instance type |

## Basic Flow

```
Spring Boot / FastAPI (on EC2)
   ↓
Connects to: mydb.xxx.rds.amazonaws.com:3306
   ↓
RDS Security Group: allows port 3306 from EC2's security group only
   ↓
RDS (MySQL) processes query → returns result
   ↓
Backend sends response to user
```

## Simple Practical Example

> "My Spring Boot app connects to RDS MySQL. The database endpoint and credentials are set as environment variables on EC2. The app uses the standard JDBC connection — it works exactly like a local MySQL database."

```properties
# application.properties
spring.datasource.url=jdbc:mysql://${RDS_ENDPOINT}:3306/${DB_NAME}
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
```

## Interview Questions

### Basic
**Q: What is RDS?**
A: RDS is AWS's managed relational database service. You get a fully managed MySQL/PostgreSQL instance — AWS handles backups, patches, and scaling. You connect to it like any regular database.

**Q: Should RDS be publicly accessible?**
A: No. In production, RDS should have no public IP. Only the backend (EC2) should reach it, via the internal VPC network, through security group rules.

**Q: Why use RDS instead of installing MySQL on EC2?**
A: RDS is managed — AWS handles backups, patches, and recovery automatically. With EC2, you manage everything yourself. RDS is more reliable and easier to maintain for production use.

**Q: How does a backend connect to RDS?**
A: Using the standard database driver (JDBC for Java, SQLAlchemy for Python) with the RDS endpoint, port, database name, and credentials stored as environment variables.

### Practical
**Q: How do you secure RDS?**
A: Set Publicly Accessible = No. Place it in a private subnet. Configure its security group to allow inbound only from the EC2 security group. Store credentials in environment variables or AWS Secrets Manager.

**Q: Where should DB credentials go?**
A: Environment variables on EC2, or better — AWS Secrets Manager. Never hardcode in application code or config files.

### Scenario
**Q: EC2 backend can't connect to RDS. What do you check?**
A: 1) Is RDS running? 2) Is the endpoint correct in env vars? 3) Does RDS security group allow port 3306 from EC2's security group? 4) Test connection: `mysql -h <endpoint> -u admin -p`

**Q: RDS vs MySQL on EC2 — which would you choose?**
A: RDS for production — it's managed, so I don't worry about backups, patches, or crashes. MySQL on EC2 is an option for quick testing but not ideal for real applications.

---

# 5. VPC — Virtual Private Cloud

## What is it?
VPC is your **private, isolated network in AWS**. All your AWS resources (EC2, RDS, Lambda) live inside a VPC. Think of it as your own virtual data center section in AWS — separated from other customers.

## Why use it?
- Isolate your resources from the public internet
- Put public resources (EC2 backend) in public subnets
- Put private resources (RDS database) in private subnets
- Control traffic flow between services

## Important Concepts

| Concept | Meaning |
|---|---|
| **VPC** | Your isolated private network in AWS |
| **Public Subnet** | Has internet access via Internet Gateway. For EC2 (backend) |
| **Private Subnet** | No internet access. For RDS (database) |
| **Internet Gateway** | Connects public subnet to the internet |
| **Security Group** | Firewall at the instance level — controls port access |
| **Default VPC** | AWS creates one automatically per region. Good enough for most beginner use |

## Basic Flow

```
Internet
   ↓
Internet Gateway
   ↓
Public Subnet → EC2 (Backend) [has public IP]
   ↓
Private Subnet → RDS (Database) [no public IP, only EC2 can reach it]
```

## Simple Practical Example

> "My EC2 backend is in a public subnet so users can reach it. My RDS database is in a private subnet — it has no public IP. The RDS security group only allows connections from the EC2 security group. So only my backend can talk to the database."

## Interview Questions

### Basic
**Q: What is a VPC?**
A: VPC (Virtual Private Cloud) is an isolated private network in AWS where your resources live. You control what's public (internet-facing) and what's private (internal only).

**Q: Difference between public subnet and private subnet?**
A: Public subnet has a route to the internet via an Internet Gateway — resources here can have public IPs. Private subnet has no internet route — resources are internal only.

**Q: What is a security group?**
A: A virtual firewall at the instance level that controls inbound/outbound traffic. You define rules like "allow port 3306 only from EC2's security group."

**Q: Why put RDS in a private subnet?**
A: Databases hold sensitive data and should never be directly reachable from the internet. Private subnet means no public IP, so the internet can't connect to it — only your backend can.

### Scenario
**Q: EC2 is running but users can't access port 8080. What's wrong?**
A: The security group inbound rule for port 8080 is probably missing. Add: Type = Custom TCP, Port = 8080, Source = 0.0.0.0/0.

**Q: RDS security group — should you allow 0.0.0.0/0 on port 3306?**
A: Never. That exposes the database to the internet. Allow only from the EC2 security group ID.

---

# 6. Lambda — Serverless Functions

## What is it?
Lambda lets you **run code without managing a server**. You write a function, upload it, define a trigger (what causes it to run), and AWS runs it automatically. You pay only for the time your code actually runs — down to the millisecond.

## Why use it?
- No server to manage or pay for when idle
- Runs in response to events (file uploaded, API call, schedule, message)
- Auto-scales automatically
- Great for short, event-driven tasks

## Important Concepts

| Concept | Meaning |
|---|---|
| **Function** | Your code with a handler entry point |
| **Trigger** | What causes Lambda to run: S3 upload, API call, schedule, SQS, etc. |
| **Event** | Data passed to your function (e.g., which file was uploaded) |
| **Runtime** | The language Lambda uses: Python, Java, Node.js, etc. |
| **Cold Start** | First invocation after idle adds slight delay (container startup) |
| **15-minute limit** | Maximum execution time per invocation |

## Basic Flow

```
Event (e.g., file uploaded to S3)
   ↓
Lambda triggered automatically
   ↓
Your function runs (resize image, process data, etc.)
   ↓
Function completes → AWS cleans up
```

## Simple Practical Example

> "When a user uploads a profile picture to S3, a Lambda function is triggered automatically. It resizes the image and saves the thumbnail back to S3. No server needed — it only runs when an upload happens."

```python
# Lambda triggered by S3 upload
def lambda_handler(event, context):
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    print(f"New file: {key} in bucket: {bucket}")
    # process the file here...
```

## Interview Questions

### Basic
**Q: What is Lambda?**
A: Lambda is AWS's serverless compute service. You write a function, define a trigger, and AWS runs it automatically. You don't manage any server.

**Q: What does "serverless" mean?**
A: It doesn't mean no servers exist — it means you don't manage any servers. AWS handles all infrastructure. You just write code.

**Q: What is a cold start?**
A: When Lambda hasn't been invoked recently, AWS needs to initialize a new container. This adds a small delay (milliseconds to seconds) on the first call. Subsequent calls are faster.

**Q: EC2 vs Lambda — when would you use each?**
A: EC2 for always-running applications (like a Spring Boot API server). Lambda for short, event-triggered tasks (resize image on upload, send email, run scheduled cleanup).

### Scenario
**Q: You want to resize an image every time a user uploads one. How?**
A: Upload goes to S3 → S3 event triggers Lambda → Lambda downloads, resizes, re-uploads thumbnail. No persistent server needed.

**Q: You want to send a welcome email when a user registers. How would Lambda help?**
A: After registration, publish an event to SNS or SQS. Lambda subscribes to this and sends the email using SES. The API responds immediately without waiting for the email.

---

# 7. CloudWatch — Monitoring & Logging

## What is it?
CloudWatch is AWS's **monitoring and logging service**. It collects metrics (CPU, memory, requests) and logs (application output) from your AWS resources. You set alarms to get notified when something goes wrong.

## Why use it?
- See if your EC2 CPU is too high
- Search your application logs for errors
- Get alerted before your app crashes
- Debug production issues without SSHing into the server

## Important Concepts

| Concept | Meaning |
|---|---|
| **Metrics** | Numbers over time: CPU %, request count, error rate |
| **Logs** | Application output (your `print()` / `log.error()` statements) |
| **Log Group** | Container for logs from one application/service |
| **Alarm** | Triggers a notification when a metric crosses a threshold |
| **CloudWatch Agent** | Software installed on EC2 to send app logs + memory metrics |

## Basic Flow

```
EC2 (your app writes logs to app.log)
   ↓
CloudWatch Agent collects logs
   ↓
CloudWatch Logs (searchable in AWS Console)
   ↓
CloudWatch Alarm: "If ERROR count > 10 in 5 min → email me"
   ↓
SNS → Email notification sent
```

## Simple Practical Example

> "When my application on EC2 was throwing errors, I checked CloudWatch Logs to find the exact error message and stack trace. I also set up a CloudWatch Alarm to notify me by email if CPU exceeds 80% for more than 5 minutes."

## Interview Questions

### Basic
**Q: What is CloudWatch?**
A: AWS's monitoring service. It collects metrics (CPU, memory, request count) and logs from services. You can set alarms to get notified when something goes wrong.

**Q: What is the difference between metrics and logs?**
A: Metrics are numbers over time (e.g., CPU = 75%). Logs are text records of events (e.g., "ERROR: DB connection failed"). Metrics for dashboards/alarms; logs for debugging.

**Q: How do you monitor an app running on EC2?**
A: Install the CloudWatch Agent. Configure it to collect the application log file. View logs in CloudWatch Log Groups. Set alarms on CPU/error metrics.

### Scenario
**Q: Your Spring Boot app on EC2 started failing. How do you investigate?**
A: Check CloudWatch Logs for ERROR/Exception entries. Check EC2 CPU/memory metrics. SSH into EC2 to verify the process is still running.

**Q: How would you get alerted if EC2 CPU goes above 90%?**
A: Create a CloudWatch Alarm: metric = CPUUtilization, condition = > 90% for 5 minutes, action = send email via SNS.

---

# 8. Other Services (Brief)

Know these at one-line level. No deep explanation needed.

| Service | What it is | When you'd mention it |
|---|---|---|
| **API Gateway** | Creates HTTP endpoints that trigger Lambda or proxy to EC2 | "For a serverless API: API Gateway + Lambda" |
| **CloudFront** | CDN — caches content near users globally | "Put CloudFront in front of S3 to make React load faster worldwide" |
| **Route 53** | AWS DNS — maps domain names to AWS resources | "To use a custom domain (myapp.com) instead of an EC2 IP" |
| **Elastic Beanstalk** | PaaS — upload your app, AWS sets up EC2/load balancer automatically | "Easier deployment without manual EC2 setup" |
| **Secrets Manager** | Stores secrets (DB passwords) securely, better than env vars | "Recommended for production credential storage" |
| **SNS** | Sends notifications to emails, Lambda, SQS | "CloudWatch alarm → SNS → email alert" |
| **SQS** | Message queue — decouples services | "Put tasks in a queue; a worker processes them asynchronously" |
| **ECR + ECS** | Docker image registry + container orchestration | "Push Docker image to ECR; ECS runs it" |

---

# 9. How AWS Services Work Together

## Typical Full-Stack Architecture

```
         USER
          ↓
   React Frontend (S3 static hosting)
          ↓  (HTTP API calls)
   EC2 Backend (Spring Boot / FastAPI)
     ↓           ↓
   RDS          S3
(Database)   (File Storage)
          ↓
    CloudWatch (Logs + Monitoring)

IAM — controls EC2's access to S3 + Secrets Manager
```

## Each Connection Explained

**React → EC2**
The React app (running in the user's browser) sends API requests to the backend hosted on EC2. Example: `fetch('http://<ec2-ip>:8080/api/users')`.

**EC2 → RDS**
The backend connects to RDS using the database driver (JDBC/SQLAlchemy). Uses the RDS endpoint as the host. Credentials come from environment variables. They talk over the private VPC network — not the internet.

**EC2 → S3**
The backend uses the AWS SDK to upload/download files. The EC2 has an IAM role attached — no hardcoded credentials. The SDK gets temporary credentials from the IAM role automatically.

**IAM (everywhere)**
IAM role on EC2 defines what it can access. Without this, EC2 can't touch S3, Secrets Manager, or anything else. Never hardcode AWS keys — use roles.

**CloudWatch**
Collects EC2 application logs and metrics. Used for debugging errors and setting up CPU/error alerts.

## Complete Request Flow (File Upload)

```
1. User picks a file in React
2. React → POST /upload → EC2 backend
3. EC2 backend → boto3/SDK → S3 (uploads file)
   (EC2 uses IAM role to authenticate — no hardcoded keys)
4. Backend saves S3 URL → RDS database
5. Backend returns URL → React
6. React displays image using S3 URL
7. CloudWatch logs the request
```

---

# 10. How Would You...? Questions

**Q1: How would you deploy a Spring Boot application on AWS?**
> Launch an EC2 instance (Ubuntu, t2.micro). SSH in, install JDK. Build the JAR locally (`mvn package`), copy it to EC2 (`scp`). Run `nohup java -jar app.jar &`. Open the app port in the security group.

**Q2: How would you deploy a FastAPI application on AWS?**
> Launch EC2. SSH in, install Python. Copy project files via `scp`. Install dependencies (`pip install -r requirements.txt`). Run `nohup uvicorn main:app --host 0.0.0.0 --port 8000 &`. Open port 8000 in security group.

**Q3: Where would you store user-uploaded images?**
> S3. The backend receives the file and uploads it to S3 using the SDK. S3 gives each file a URL. The URL is saved in the database. EC2's disk is not suitable — it's limited, expensive, and lost on instance termination.

**Q4: How would your backend connect to a database in AWS?**
> Use RDS. Configure the database URL, port, name, and credentials as environment variables. Use the standard database driver (JDBC/SQLAlchemy). RDS is in a private subnet — only EC2 can reach it via the VPC internal network.

**Q5: How would you secure your AWS resources?**
> Security groups to control port access. RDS in private subnet (not public). IAM roles instead of hardcoded credentials. Least-privilege permissions. Credentials in environment variables or Secrets Manager.

**Q6: How would you monitor your application?**
> CloudWatch. Install the CloudWatch Agent on EC2 to ship logs. Set alarms on CPU and error metrics. When errors spike, I get notified via email (through SNS). Check logs in CloudWatch console to debug issues.

**Q7: How would you give an EC2 application permission to access S3?**
> Create an IAM role with an S3 policy (allow put/get on the bucket). Attach the role to EC2. The AWS SDK on EC2 automatically uses the role's credentials — no keys needed in the code.

**Q8: How would you make a database inaccessible from the internet?**
> Set RDS "Publicly Accessible = No". Place it in a private subnet. Configure its security group to allow inbound on port 3306 only from the EC2 security group. No public IP means internet cannot reach it.

**Q9: Why use RDS instead of MySQL installed on EC2?**
> RDS is managed — AWS handles backups, patches, and failover. If MySQL is on EC2, I manage everything myself. RDS is more reliable for production. MySQL on EC2 is fine for quick testing but not recommended for real projects.

**Q10: How would you make sure credentials are not exposed?**
> Use environment variables on EC2 (`export DB_PASSWORD=xxx`). For production, use AWS Secrets Manager. Use IAM roles for service-to-service authentication. Never commit credentials to Git.

---

# 11. Key Comparisons

## EC2 vs Lambda

| Feature | EC2 | Lambda |
|---|---|---|
| Server management | You manage it | AWS manages it |
| Running model | Always on | Runs only when triggered |
| Best for | Long-running apps (Spring Boot API) | Short event-driven tasks (resize image) |
| Pricing | Per hour (even when idle) | Per millisecond of execution |
| Timeout | No limit | Max 15 minutes |

## S3 vs EC2 Storage (EBS)

| Feature | S3 | EBS (EC2 disk) |
|---|---|---|
| Purpose | Store files/objects | App files and OS disk |
| Access | Via URL / API | Via file system (only from EC2) |
| Cost | Cheaper per GB | More expensive |
| Survives termination | Yes | No (by default) |
| Best for | Images, docs, backups | Application code, databases |

## IAM User vs IAM Role

| Feature | IAM User | IAM Role |
|---|---|---|
| Used by | Humans | AWS services (EC2, Lambda) |
| Credentials | Permanent (access keys) | Temporary (auto-renewed) |
| Risk | Keys can be leaked | No permanent keys to leak |
| Best practice | For people / CI-CD | For applications on AWS |

## RDS vs MySQL on EC2

| Feature | RDS | MySQL on EC2 |
|---|---|---|
| Management | Fully managed | You manage everything |
| Backups | Automatic | Manual |
| Updates/patches | AWS handles | You handle |
| Reliability | High | Depends on your setup |
| Recommended for | Production | Quick testing only |

## Authentication vs Authorization (IAM Context)

- **Authentication** = "Who are you?" — verifying identity (login, access keys)
- **Authorization** = "What can you do?" — checking permissions (IAM policies)

---

# 12. Interview Questions

## Basic (20 Questions)

| # | Q | A |
|---|---|---|
| 1 | What is EC2? | Virtual server in the cloud. Run any application on it. |
| 2 | What is an AMI? | OS template used to create an EC2 instance (e.g., Ubuntu 22.04) |
| 3 | What is a security group? | Virtual firewall. Controls which ports allow inbound/outbound traffic |
| 4 | What is a key pair? | SSH authentication for EC2. You download `.pem`, AWS stores public key |
| 5 | Stop vs terminate EC2? | Stop = paused, data kept. Terminate = permanently deleted |
| 6 | What is S3? | File storage service. Store objects in buckets. Each file gets a URL |
| 7 | What is a bucket? | Container for S3 objects. Must have globally unique name |
| 8 | Are S3 objects public by default? | No — private by default. Must explicitly allow public access |
| 9 | What is a pre-signed URL? | Temporary URL for accessing a private S3 object for limited time |
| 10 | What is IAM? | Controls who can access what in AWS — via users, roles, policies |
| 11 | IAM User vs IAM Role? | User = for humans, permanent keys. Role = for services, temporary credentials |
| 12 | Why not hardcode AWS keys? | Leaked keys = full account access. Use IAM roles instead |
| 13 | What is RDS? | Managed relational DB service. Supports MySQL, PostgreSQL, etc. |
| 14 | Should RDS be public? | No. Keep it in private subnet, only reachable from the backend |
| 15 | What is VPC? | Isolated private network in AWS where all resources live |
| 16 | Public vs private subnet? | Public = internet access (EC2). Private = no internet (RDS) |
| 17 | What is Lambda? | Serverless — runs code in response to events, no server management |
| 18 | What is a cold start? | First Lambda invocation after idle period — slight startup delay |
| 19 | What is CloudWatch? | AWS monitoring. Collects metrics and logs, lets you set alarms |
| 20 | EC2 vs Lambda? | EC2 = always-running. Lambda = event-driven, runs only when triggered |

## Practical (10 Questions)

**Q: How do you deploy a Spring Boot app to EC2?**
A: Build JAR → scp to EC2 → SSH in → install JDK → `nohup java -jar app.jar &` → open port in security group.

**Q: How does EC2 access S3 without hardcoded keys?**
A: Attach an IAM role with S3 permission to EC2. AWS SDK reads temporary credentials from the role automatically.

**Q: How does Spring Boot connect to RDS?**
A: Set datasource URL, username, password as environment variables. Use JDBC driver. Spring Boot connects using these on startup.

**Q: How do you access a private S3 file from a frontend?**
A: Backend generates a pre-signed URL with an expiry. Frontend uses that URL to download directly from S3.

**Q: How do you keep an app running after closing SSH?**
A: `nohup java -jar app.jar > app.log 2>&1 &`

**Q: How do you pass DB credentials to an app on EC2?**
A: Set them as environment variables. App reads them via `System.getenv()` or `${VAR}` in `application.properties`.

**Q: How do you check if your EC2 app is running?**
A: `ps aux | grep java` or `curl http://localhost:8080/health`

**Q: How do you open a port on EC2?**
A: Go to security group → Edit Inbound Rules → Add rule with the port and source IP.

**Q: How do you store uploaded files if not S3?**
A: You could store on EC2's disk (EBS), but it's not recommended — limited space, data lost on termination, no URL. S3 is always the right answer.

**Q: How do you monitor logs from a production EC2 app?**
A: Install CloudWatch Agent, configure it to send app log file to CloudWatch. Search logs in the console.

## Scenario-Based (10 Questions)

**Q: After EC2 restarts, React can't connect to the backend. Why and fix?**
A: Public IP changed on restart. Fix: assign an Elastic IP to EC2 so the IP is permanent.

**Q: Users can't reach your app on port 8080. What do you check?**
A: Security group — is port 8080 open? Also check if the app is actually running (`ps aux`).

**Q: EC2 backend can't connect to RDS. Checklist?**
A: 1) RDS running? 2) Correct endpoint? 3) RDS security group allows port 3306 from EC2 SG? 4) Test manually: `mysql -h <endpoint> -u admin -p`

**Q: User uploads profile picture. Walk through the full flow.**
A: React → POST /upload → EC2 backend → uploads to S3 (with IAM role, no keys) → saves URL in RDS → returns URL → React shows image.

**Q: You pushed an AWS access key to GitHub. What do you do?**
A: Delete the key in IAM immediately. Create a new key. Check CloudTrail for unauthorized usage. Use IAM roles in future.

**Q: Where do you run a Spring Boot API on AWS?**
A: EC2. It's a long-running process — Lambda's timeout (15 min) and stateless nature make it unsuitable.

**Q: How do you let users download their private documents?**
A: Backend generates a pre-signed URL (15 min expiry) after verifying ownership. User's browser downloads directly from S3 using that URL.

**Q: Your application on EC2 suddenly slows down. What do you check?**
A: CloudWatch metrics — is CPU at 100%? Check logs for errors. Check RDS — is DB slow or connection pool full?

**Q: You need a scheduled task to delete old temp files daily. How?**
A: Lambda function triggered by a CloudWatch scheduled event (EventBridge). No server needed.

**Q: Your app needs file storage and a relational database. Which services?**
A: S3 for file storage, RDS for the relational database.

## Resume-Based (5 Questions)

**Q: You've written AWS (Basics) on your resume. What do you know?**
*(See section 14 below for full answer)*

**Q: Which AWS services have you worked with?**
A: "I understand and have worked with EC2, S3, IAM, and RDS. I know the concepts of VPC, Lambda, and CloudWatch."

**Q: Have you deployed anything on AWS?**
*(Be honest — see section 14)*

**Q: Why would you use EC2 over hosting locally?**
A: EC2 is accessible from anywhere, runs 24/7, doesn't depend on my local machine, and scales easily.

**Q: What is the most important thing to know about AWS security?**
A: Never hardcode AWS credentials. Use IAM roles for services. Use security groups to control access. Keep databases in private subnets.

---

# 13. How to Answer AWS Questions

**Structure every answer as:**
1. Definition (what it is)
2. Why it's used
3. Simple example

**Example — "What is S3?"**

> *"S3 is AWS's object storage service — used to store files like images, documents, and videos. We store files in containers called buckets, and each file gets a URL. For example, in my project, when a user uploads a profile picture, the backend uploads it to S3 and saves the URL in the database. The frontend then loads the image directly from that URL."*

---

**Example — "How would your backend access S3?"**

> *"I would attach an IAM role to the EC2 instance with permission to read and write to the S3 bucket. The AWS SDK automatically reads the role's temporary credentials — so no access keys need to be in the code. This is the recommended approach because hardcoded keys are a security risk."*

---

**When you don't know something:**

> *"I understand the concept at a basic level — [explain what you know]. I haven't used it in depth yet, but I know it's used for [correct use case]."*

**Never:**
- Claim hands-on experience you don't have
- Say "I don't know" and stop — always say what you do know

---

# 14. Resume-Based Questions

> **Honest framing for a fresher with AWS (Basics) on their resume:**
> - "I **understand** how EC2 works and know the deployment process."
> - "I **know** how to configure security groups and IAM roles."
> - "I **have practiced** concepts using the AWS free tier / documentation."
> - Avoid claiming "I deployed a production system" unless you actually did.

---

**Q: "You have AWS Basics on your resume. Tell me what you know."**

> *"I have studied and practiced core AWS services that are relevant for full-stack development. I understand EC2 — it's a virtual server where you deploy your backend application. I know S3 for file storage — when users upload images, the backend stores them in S3 and saves the URL in the database. I understand IAM — especially how to use IAM roles instead of hardcoded credentials so that EC2 can securely access S3. I know RDS for managed relational databases — my Spring Boot/FastAPI app connects to it using JDBC or SQLAlchemy with the endpoint configured as an environment variable. I also have a basic understanding of VPC, Lambda, and CloudWatch."*

---

**Q: "How would you deploy your Spring Boot application to AWS?"**
> *"I would launch an EC2 instance with Ubuntu, SSH into it, install JDK, build the JAR locally and transfer it with scp, then run it with `nohup java -jar app.jar &` so it keeps running. I'd open the application port in the security group and configure RDS credentials as environment variables."*

---

**Q: "Where would you store the database?"**
> *"I would use RDS — AWS's managed database service. It supports MySQL and PostgreSQL. I'd keep it in a private subnet so it's not reachable from the internet, and configure the security group to only allow connections from the backend EC2 instance."*

---

**Q: "Where would you store uploaded files?"**
> *"S3. The backend uploads files using the AWS SDK. Each file gets a URL that I store in the database. S3 is the right choice because it's unlimited, durable, and much cheaper than using EC2's disk."*

---

**Q: "Why would you use IAM roles over access keys?"**
> *"Access keys are permanent credentials that can be accidentally committed to GitHub or exposed in logs. IAM roles provide temporary credentials that are automatically rotated and never stored in code. If I attach an IAM role to EC2, the SDK picks up credentials automatically — no keys needed anywhere in the code."*

---

# 15. Last-Minute Revision

## Services at a Glance

### EC2 — Virtual Server
- Rent a Linux/Windows server in the cloud
- AMI = OS template · Instance type = hardware specs
- Security group = firewall · Key pair = SSH access
- Public IP changes on restart → use Elastic IP
- Stop = paused · Terminate = deleted permanently

### S3 — File Storage
- Store objects (files) in buckets
- Private by default
- Each file has a key (path) and a URL
- Pre-signed URL = temporary access to private file
- Use for: images, documents, React static site

### IAM — Permissions
- Users = humans · Roles = AWS services · Policies = rules
- NEVER hardcode AWS access keys in code
- Use IAM roles on EC2 to access other services
- Least privilege = give only what's needed

### RDS — Managed Database
- MySQL, PostgreSQL, etc. — fully managed
- Connect using endpoint + port + credentials from env vars
- Keep in private subnet — not publicly accessible
- Security group allows only backend (EC2) to connect

### VPC — Private Network
- All resources live in a VPC
- Public subnet → EC2 (internet-facing)
- Private subnet → RDS (not internet-facing)
- Security groups control port-level access

### Lambda — Serverless
- Run code without managing a server
- Triggered by: S3 upload, API Gateway, schedule, etc.
- Pay per millisecond · Max 15 min timeout
- Good for: event-driven tasks, not long-running servers

### CloudWatch — Monitoring
- Collects metrics (CPU, memory) and logs
- Set alarms: "CPU > 80% → email me"
- Debug production issues by searching logs

---

## Most Important AWS Flow

```
         USER
          ↓
    React (S3 static site or EC2)
          ↓
    EC2 Backend (Spring Boot / FastAPI)
        ↓         ↓
      RDS         S3
   (Database)  (Files)

IAM Role on EC2 → allows EC2 to access S3
CloudWatch → monitors EC2 logs and metrics
Security Group → controls who can reach EC2 and RDS
```

---

## Top 15 Questions to Revise Before Interview

1. What is EC2? How do you deploy a backend on it?
2. What is a security group and what does it do?
3. What is an AMI?
4. Stop vs terminate — what's the difference?
5. What is S3? Why use it instead of storing files on EC2?
6. What is a pre-signed URL and when is it used?
7. What is IAM? User vs Role — what's the difference?
8. Why should you never hardcode AWS credentials?
9. How does EC2 access S3 without access keys?
10. What is RDS? Why use it over MySQL on EC2?
11. Should RDS be publicly accessible? Why not?
12. How does a backend connect to RDS?
13. What is VPC? Public vs private subnet?
14. What is Lambda? When would you use it over EC2?
15. What is CloudWatch? How do you use it to debug an app?

---

*Prepared for: Entry-Level Software Engineer / Full-Stack Developer Interviews*
*Focus: AWS Basics — practical understanding, not certification depth*
