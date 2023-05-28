# Content

## Docker

### What is Docker?

- Docker is software development platform to deploy apps
- Apps care packaged in containers that can be run on any OS
- Apps run the same, regardless of where they’re run
    - Any machine
    - No compatibility issues
    - Predicable behavior
    - Less work
    - Easier to maintain and deploy
    - Works with any language, any OS, any technology
- **Use cases**: microservices, architecture, left-and-shift apps from on-premises to the AWS cloud….

### Docker on an OS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31b7a55e-101b-467d-aaa5-64bb6e38fb30/Untitled.png)

### Where are Docker images stored?

- Docker images are stored in Docker Repositories
- **Docker Hub** (https://hub.docker.com)
    - Public Repository
    - Find base images for many technologies or OS(eg: Ubuntu, MySQL…)
- **Amazon ECR (Amazon Elastic Container Registry)**
    - Private repository
    - Public repository (Amazon ECR Public Gallery https://gallert.ecr.aws)

### Docker vs Virtual Machines

- Docker is “sort if” a virtualization technology, but not exactly
- Resources are shared with the host ⇒ many containers on one server

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/649116c4-36c0-48da-ba1e-c17a6f6f07f7/Untitled.png)

### Getting Started with Docker

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8051e4c2-d1ca-45c3-8194-3b9ee4133115/Untitled.png)

### Docker Containers Management on AWS

- **Amazon Elastic Container Service (Amazon ECS)**
    - Amazon’s own container platform
- **Amazon Elastic Kubernetes Service (Amazon EKS)**
    - Amazon’s managed Kubernetes (open source)
- **AWS Farget**
    - Amazon’s own Serverless container platform
    - Works with ECS and with EKS
- **Amazon ECR:**
    - Store container images

**Note**:

- Docker filter sau khi build sẽ trở thành Docker Image
- Push/pull docker image lên gọi là Docker Repository
- Docker Image khi chạy sẽ trở thành Docker Container

### Amazon ECS - EC2 Launch Type

- ECS = Elastic Container Service
- **Launch Docker containers on AWS = Launch ECS Task on ECS Cluster**
- **EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances)**
- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster
- AWS takes care of starting / stopping containers

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9453a94d-d0e1-43fd-aef4-bad0203df11a/Untitled.png)

### Amazon ECS - Fargate Launch Type

- Launch Docker container on AWS
- **You don’t provision the infrastructure (no EC2 instances to manage)**
- **It’s all Serverless!**
- You just create task defininitions
- AWS just runs ECS Task for you based on the CPU / RAM you need
- To scale, just increase the number of tasks. Simple - no more EC2 instances

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f072035d-0a7a-4f52-8a5e-c2922a4687c9/Untitled.png)

### Amazon ECS - IAM Roles for ECS

- **EC2 Instance Profile (EC2 Launch Type only):**
    - Used by the ECS agent
    - Makes API calls to ECS service
    - Send container logs to CloudWatch Logs
    - Pull Docker image from ECR
    - Reference sensitive data in Secrets Manager or SSM Parameter Store
- **ECS Task Role:**
    - Allow each task to have a specific role
    - Use different roles for the different ECS Services you run
    - Task Role is defined in the **task definition**

### Amazon ECS - Load Balancer Integrations

- **Application Load Balancer** supported and works for most use cases
- **Network Load Balancer** recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link
- **Elastic Load Balancer** supported but not recommended (no advanced features - no Fargate)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d2669c21-786b-408c-a33f-4d73ad5bc9b6/Untitled.png)

### Amazon ECS - Data Volumes (EFS)

- Mount EFS file systems onto ECS tasks
- Works for both **EC2** and **Fargate** launch types
- Tasks running in any AZ will share the same data in the EFS file system
- **Fargate + EFS = Serverless**
- Use cases: persistent multi-AZ shared storage for your containers
- Note:
    - Amazon S3 cannot be mounted as a file system

    ### ECS Service Auto Scaling

- Automatically increase / decrease the desired number of ECS tasks
- Amazon ECS Auto Scaling uses **AWS Application Auto Scaling**
    - ECS Service Average CPU Utilization
    - ECS Service Average Memory Utilization - Scale on RAM
    - ALB Request Count Per Target - metric coming from the ALB
- **Target Tracking** - scale based on target value for a specific CloudWatch metric
- **Step Scaling** - scale based on a specified CloudWatch Alarm
- **Scheduled Scaling** - scale based on a specified data/time (predicable changes)
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Farget Auto Scaling is much easier to setup (because **Serverless**)

### EC2 Launch Type - Auto Scaling EC2 Instances

- Accommodate ECS Service Scaling by adding underlying EC2 instances
- **Auto Scaling Group Scaling**
    - Scale your ASG based on CPU Utilization
    - Add EC2 instances over time
- **EC2 Cluster Capacity Provider**
    - Used to automatically provision and scale the infrastructure for you ECS Tasks
    - Capacity Provider paired with an Auto Scaling Group
    - Add EC2 Instances when you’re missing capacity (CPU, RAM…)

### ECS Scaling - Service CPU Usage Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1cf96269-da42-4502-9254-2337c589511e/Untitled.png)

### ECS tasks invoked by Event Bridge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71e9b2bc-3e65-4389-ae20-1097424166a5/Untitled.png)

### ECS tasks invoked by Event Bridge Schedule

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f728dbca-64c8-4b34-9b9b-6c793d9734e5/Untitled.png)

### ECS - SQS Queue Example

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c8e8870-98fd-43ae-92d4-10fbb456d61d/Untitled.png)

### ECS - Intercept Stopped Tasks using EventBridge

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8bc7eab-0a75-4e0c-95c7-ada461187f1d/Untitled.png)

## Amazon ECR

- ECR = Elastic Container Registry
- Store and manage Docker Images on AWS
- **Private** and **Public** repository (**Amazon ECR Public Gallery** https://gallery.ecr.aws)
- Fully integrated with ECS, backed by Amazon S3
- Access is controlled through IAM (permission errors ⇒ policy)
- Supports images vulnerability scanning, versioning, image tags, image lifecycle…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/014216cf-939d-4f5e-afd9-9f0345ebc6ac/Untitled.png)

## Amazon EKS Overview

- Amazon EKS = Amazon Elastic Kubernetes Service
- It is a way to launch managed Kubernetes cluster on AWS
- Kubernetes is an open-source system for automatic deployment, scaling and management of containerized (usually Docker) application
- It’s an alternative to ECS, similar goal by different API
- EKS supports **EC2** if you want to deploy worker nodes or **Fargate** to deploy serverless containers
- **Use case**: if your company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes
- **Kubernetes is cloud-agnostic** (Can be used in any cloud - Azure, GCP…)

### Diagram

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fdb2a97d-177e-43c7-b217-d487158473c4/Untitled.png)

### Node Types

- **Managed Node Groups**
    - Creates and manages Nodes (EC2 instances) for you
    - Nodes are part of an ASG managed by EKS
    - Supports On-demand or Spot Instances
- **Self-Managed Nodes**
    - Nodes created by you and registered to the EKS cluster and managed by an ASG
    - You can use prebuilt AMI - Amazon EKS Optimized AMI
    - Supports On-Demand or Spot Instances
- **AWS Fargate**
    - No maintenance required, no nodes managed

### Data Volumes

- Need to specify StorageClass manifest on your EKS cluster
- Leverages a Container Storage Interface (CSI) compliant driver
- Support for :
    - Amazon EBS
    - Amazon EFS (work with Fargate)
    - Amazon FSx for Lustre
    - Amazon FSx for NetApp ONTAP

## AWS App Runner

- Fully managed service that makes it easy to deploy web applications and APIs at scale
- No infrastructure experience required
- Start with your source code or container image
- Automatically builds and deploy the web app
- Automatic scaling, highly available, load balancer, encryption
- VPC access support
- Connect to database, cache, and message queue services
- Use case: web apps, APIs, microservices, rapid production, deployment