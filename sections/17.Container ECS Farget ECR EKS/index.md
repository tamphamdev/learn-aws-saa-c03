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