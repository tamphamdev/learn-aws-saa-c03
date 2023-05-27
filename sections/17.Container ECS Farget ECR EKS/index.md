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