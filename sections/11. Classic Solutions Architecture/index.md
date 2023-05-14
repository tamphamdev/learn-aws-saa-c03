# Classic Solutions Architecture
# Overview

## Instantiating Applications quickly

- EC2 Instances:
    - **Use a Golden AMI:** Install your applications, OS dependencies etc…beforehand and launch your EC2 instance from the Golden AMI
    - **Bootstrap using User Data**: For dynamic configuration, use User Data scripts
    - **Hybrid**: mix Golden AMI and User Data( Elastic Beanstalk)
- RDS Databases:
    - Restore from a snapshot: the database will have schemas and data ready
- EBS Volumes:
    - Restore from a snapshot: the disk will already be formatted and have data
## Elastic Beanstalk

- Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS….
- Managed service
    - Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration…
    - Jus the application code is the responsibility of the developer

- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances
## Elastic Beanstalk - Components

- Application: collection of Elastic Beanstalk components (environments, versions, configurations,…)
- Application Version: an iteration of your application code
- Environment
    - Collection of AWS resources running an application version (only one application version at a time)
    - Tiers: Web Server Environment Tier & Work Environment Tier
    - You can create multiple environments (dev, test, prod….)