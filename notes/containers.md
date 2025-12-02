# Containers

---

## Container-related services on AWS

**Amazon Elastic Container Service (Amazon ECS)**
- Amazons own container platform.

**Amazon Elastic Kubernetes Service**
- Amazons managed Kubernetes (open source).

**AWS Fargate**
- Amazons own serverless container platform.
- Works with ECS and EKS.

**Amazon ECR**
- Store container images.

---

## Amazon ECS
- Service that lets you run and manage containers on AWS.
- Acts as manager that handles container availability and scaling.
- *Requirement:* Choosing a launch type when creating a cluster.

## Amazon ECS EC2 Launch Type
- *You are responsible* for the infrastructure.
- Must provision, manage, and maintain EC2 instances.
- Containers run on these EC2 instances.
- **ECS Agent** is required for each EC2 instance.
- *Best for* custom performance / security needs.

## Amazon ECS Fargate Launch Type
- Serverless container; No infrastructure to manage.
- AWS handles everything behind the scenes.
- For better **scalability**, increase the number of tasks (containers) you want to run.
- AWS automatically handles adding containers. Runs ECS tasks for you bases on CPU / RAM you need.
- *Best for* focus on application development.

---

## Amazon ECS IAM Roles

**EC2 Instance Profile (For Launch Type Only)**
- Gives the *EC2 Agent* permissions to interact with AWS services.
- Required for the ECS Agent running on EC2 instances to do its job.
- Enables API Calls for ECS, Send logs to CloudWatch, Pull Docker Images from ECR, Access Secrets Manager or SSM Parameter Store.

**ECS Task Role**
- Defines what permissions each individual task/container has while running.
- Allows each task to have a specific role.
- Use different roles for different ECS services you run.
- Task role is defined in the task definition.

## Security Best Practices

**Principle of Least Privilege:**
- Give each container only the permissions it needs.
- Don't give unrestricted access to everything.
- Use separate roles for different tasks to lock down access.

**Benefits:**
- Secure interaction with AWS services.
- Prevents containers from having excessive permissions.
- Reduces security risks from compromised containers.
- Better compliance and audit trail.

---

## ECS Service Auto Scaling
- Automatically adjusts ECS tasks based on demand to handle spikes and save costs

**Scaling Strategies:**
- **Target Tracking** - Maintain a metric automatically
- **Step Scaling** - Scalte up/down based on thresholds
- **Schedules Scaling** - Scale at specific times based on predicted traffic.

---

## Amazon ECR (Elastic Container Registry)
- Cloud storage for Docker Images, fully inegrated with ECS.
- **Repositories:**
    - *Private* - Internal/sensitive images
    - *Public* - Open-source or shared images
- ECS tasks can pull images automatically.
- Access is controlled through IAM.
- Supports image vulnerability scanning, versioning image tags, image lifecycle.
- Backed by Amazon S3 for secure storage.

---

## Amazon EKS *Elastic Kubernetes Service
- Run Kubernetes on AWS without managing the control plane.
- Alternative to ECS, similar goal but different API.
- Best use for migrating existing Kubernetes workloads to AWS
- Collects logs and metrics using CloudWatch Container Insights.
- *Summary:* EKS lets you use Kubernetes in AWS with AWS handling the management task.

---

## Amazon EKS - Node Types

**Managed Node Groups**
- Creates and manages nodes (EC2 instances) for you.
- No need to provision or manually maintain nodes.
- Supports on demand or spot instances.

**Self-Managed Nodes**
- Nodes created by you and registered to the EKS cluster and manager by an ASG.
- Can use prebuilt AMI.
- Supports on demand or spot instances.
- Create your own custom AMIs for more control over environment.

**AWS Fargate**
- No maintenence required; no nodes managed.
- AWS handles all infrascructure under the hood.
- Define the CPU and memory requirments for containers.


| Feature | Managed Node Groups | Self-Managed Nodes | AWS Fargate |
| --- | --- | --- | --- |
| **Infrastructure Management** | AWS manages nodes | You manage nodes | AWS manages everything |
| **Customization** | Limited | High (custom AMIs) | Limited |
| **Auto-scaling** | EKS-managed ASG | You configure | Automatic |
| **Operational Overhead** | Low | High | None |
| **Instance Types** | On-demand, Spot | On-demand, Spot | N/A (serverless) |
| **Best For** | Convenience | Control & customization | Simplicity |
| **Responsibility** | Low | High | None |

**Decision Guide**
- Choose *Managed Node Groups* if:
    - You want convenience and automatic management
    - Don't need excessive customization
    - Want AWS to handle scaling and maintenence

- Choose *Self-Managed Nodes* if:
    - You need full control over node config
    - Require custom AMIs or specific setups
    - Have team expertise to manage infrastructure

- Choose *AWS Fargate* if:
    - You want zero infrastructure management
    - Prefer serverless approach
    - Want to focus purely on applications
    - Willing to pay slightly more for convenience

    ---