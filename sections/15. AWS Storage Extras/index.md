# Introduction

## AWS Snow Family

- Highly-secure, portable devices to **collect and process data at the edge, and migrate data into and out of AWS**
- Data migration:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cf0746d-73de-40e2-8c93-9415cda2d8a7/Untitled.png)

- Edge computing:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a627aacb-2ff3-46d8-ae0c-28c2d022a009/Untitled.png)

- Challenges:
    - Limited connectivity
    - Limited bandwidth
    - High network cost
    - Shared bandwidth (can’t maximize the line)
    - Connection stability

    > **AWS Snow Family: offline devices to perform data migrations**
    If it takes more than a week to transfer over the network, use Snowball devices !
    >

### Diagrams

- Direct upload to S3:

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5a75293-1f5c-4c54-af96-95f7f4ad7b95/Untitled.png)

- With Snow Family:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/148e4861-2e27-423b-9c4c-f4c506368e27/Untitled.png)

### Snowball Edge (for data transfer)

- Physical data transport solution: move TBs or PBs of data in or out of AWS
- Alternative to moving data over the network (and paying network fees)
- Pay per data transfer job
- Provide block storage and Amazon S3-compatible object storage
- **Snowball Edge Storage Optimized**
    - 80TB of HDD capacity for block volume and S3 compatible object storage
- **Snowball Edge Compute Optimized**
    - 42TB of HDD or 28TB NVMe capacity for block volume and S3 compatible object storage
- Use cases: large data cloud migrations, DC decommission, disaster recovery

### AWS Snowcone & Snowcone SSD

- **Small, portable computing, anywhere, rugged & secure, withstands harsh environments**
- Light (4,5 pounds, 2.1kg)
- Device used for edge computing, storage and data transfer
- **Snowcone** - 8TB of HDD storage
- **Snowcone SSD** - 14TB of SSD storage
- Use Snowcone where Snowball does not fit (space-constrained environment)
- Must provide your own battery / cables
- Can be sent back to AWS offline, or connect it to internet and use **AWS DataSync** to send data

### AWS Snowmobile

- Transfer exabytes of data ( 1EB = 1,000PB = 1,000,000 TBs)
- Each Snowmobile has 100PB of capacity (use multiple in parallel)
- High security: temperature controlled, GPS, 24/7 video surveillance
- Better than Snowball if you transfer more than 10 PB

### AWS Snow Family for Data Migrations

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9dfcc4c2-0179-4b0b-a7ca-6bf72dd87db8/Untitled.png)

### Snow Family - Usage Process

1. Request Snowball devices from the AWS console for delivery
2. Install the snowball client / AWS OpsHub on your servers
3. Connect the snowball to your servers and copy files using client
4. Ship back the device when you’re done (goes to the right AWS facility)
5. Data will be loaded into an S3 bucket
6. Snowball is completely wiped

### What is Edge Computing?

- Process data while it’s being created on an edge location
    - A truck on the road, a ship on the sea, a mining station underground…
- These locations may have
    - Limited / no internet access
    - Limited / no easy access to computing power
- We setup a Snowball Edge / Snowcone device to do edge computing
- Use cases of Edge Computing
    - Preprocess data
    - Machine learning at the edge
    - Transcoding media streams
- Eventually (if need be) we can ship back the device to AWS (for transferring data for example)

### Snow Family - Edge Computing

- **Snowcone & Snowcone SSD (smaller)**
    - 2CPUs, 4GB of memory, wired or wireless access
    - USB-C power using a cord or the optional battery
- **Snowball Edge - Compute Optimized**
    - 104 vCPUs, 416 GiB of RAM
    - Optional GPU (useful for video processing or machine learning)
    - 28TB NVMe or 42TB HDD usable storage
- **Snowball Edge - Storage Optimized**
    - Up to 40 vCPUs, 80 GiB of RAM, 80 TB storage
    - Object storage clustering available
- All: Can run EC2 instances & AWS Lambda functions (using AWS IoT Greengrass)
- Long-term deployment options: 1 and 3 year discounted pricing

### AWS OpsHub

- Historically, to use Snow Family devices, you needed a CLI
- Today, you can use **AWS OpsHub** (a software you install on your computer / laptop) to manage your Snow Family Device
    - Unlocking and configuring single or clustered devices
    - Transferring files
    - Launching and managing instances running on Snow Family Devices
    - Monitor device metrics (storage capacity, active instances on your device)
    - Launch compatible AWS services on your devices (ex: Amazon Ec2 instances, AWS DataSync, Network File System (NFS))