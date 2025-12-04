# Serverless
- *No server management required* - focus shifts from infrastructure to writing code.
- AWS runs it for you; You just deploy your code.
- *Function-based deployment* - Deploy individual functions that run on demand.
- Function get triggered by events.
- Billed only when functions actually run.
- Does *NOT* mean servers don't exist; You don't manage or see them.

---

## Serverless in AWS

**Faas (Function-as-a-service)**
- Write individual functions that run on demand.
- Billed only when they execute.
- *AWS Lambda* is an example.

---

## Core AWS Serverless Services

**AWS Lambda**
- The core of AWS serverless offerings
- Write code in a function and upload it
- AWS runs it automatically - no server management
- Event-driven execution

**DynamoDB**
- Fully-managed serverless NoSQL database
- Scales automatically
- No provisioning or managing database servers

**Amazon Cognito**
- Manages user authentication
- Handles logins and signups
- Scales without manual infrastructure management

**API Gateway**
- Bridge between users and Lambda functions
- Create and monitor APIs
- Interacts with backend services

**Amazon S3**
- Serverless object storage
- Stores files and static content
- Scales automatically based on data stored

**SNS (Simple Notification Service)**
- Handles notifications
- Communication between system components
- Pub/sub messaging

**SQS (Simple Queue Service)**
- Message queuing between services
- Decouples system components
- Reliable message delivery

**Kinesis Data Firehose**
- Loads streaming data into AWS
- Real-time analytics
- No server management for data streaming

**Aurora Serverless**
- Fully-managed serverless relational database
- Auto-scales based on demand
- Serverless version of Amazon Aurora

**AWS Step Functions**
- Manages workflows involving multiple Lambda functions or services
- Orchestrates complex processes
- Visual workflow monitoring

**AWS Fargate**
- Serverless compute for containers
- No EC2 instance management
- Focus purely on containers, AWS handles infrastructure

---

## Example Serverless Architecture
1. Users authenticate via Cognito
2. Static content stored in S3 bucket
3. Users interact with API Gateway
4. API Gateway triggers Lambda functions
5. Lambda performs operations (e.g., writes to DynamoDB)
6. SNS/SQS handles inter-service communication

---

## AWS Lambda
- Serverless compute - no servers to manage.
- *Short executions* - max runtime limit of 15 minutes.
- Perfect for *event-driven* code (triggered by events).
- *Runs on demand* - only executes when needed.
- Only pay for *actual compute used*.

**Scaling**
- Automatic scaling handled by AWS.
- Adjusts from 1 user to 1 million users automatically.
- No configuration needed.

**Best for**
- Quick tasks and short executions.
- Variable/unpredictable traffic.
- Cost optimization.

**Key Lambda benefits**
1. Cost efficiency 
    - Easy pricing with pay-per-use model
    - Charged per request and for compute time
    - No idle time charges (unlike EC2)

2. AWS Integration
    - Works perfectly with AWS ecosystem
    - Build REST APIs with API Gateway
    - Run code with S3 when file uploaded
    - React to table changes with DynamoDB
    - Many more AWS services

3. Multi-language support
    - Python, Go, Node.js, Java, Ruby, and more...

4. Easy monitoring
    - Built-in monitoring for all Lambda functions
    - Track execution frequency
    - View error and logs in one place

5. Resource Scaling
    - Can allocate up to 10gb of RAM
    - When you increase RAM, you get more CPU power and better network performance

---

## EC2 vs Lambda

| Use Case | Choose EC2 | Choose Lambda |
| --- | --- | --- |
| **Duration** | Long-running (hours/days) | Short tasks (<15 min) |
| **Traffic Pattern** | Consistent/predictable | Variable/unpredictable |
| **Management** | Need server control | Want zero management |
| **Scaling** | Manual or configured | Automatic |
| **Billing** | Pay for uptime | Pay per execution |
| **Idle Time** | Still paying | No cost |
| **Event-Driven** | Can be configured | Perfect fit |

---

## AWS Lambda Language support
- **Built-in supported languages:**
    - **Node.js** – great for event-driven apps
    - **Python** – popular for data processing, automation, web apps
    - **Java** – enterprise workloads
    - **C#, PowerShell, .NET Core** – good for Microsoft stack developers
    - **Ruby** – useful for Rails-style web apps

- **Custom Runtime API:**
    - Lets you use **non-native languages**
    - You define how Lambda runs your code
    - Useful for languages like **Golang** or anything AWS doesn’t support yet

- **Lambda Container Images:**
    - Package your app + dependencies in a Docker image
    - Must implement the Lambda Runtime API
    - Good for complex setups

- **When to use ECS/Fargate instead:**
    - When you need **full Docker flexibility** beyond Lambda's constraints

---