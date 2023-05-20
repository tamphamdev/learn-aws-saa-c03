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

  ## Amazon FSx - Overview (???)

- Launch 3rd party high-performance file systems on AWS
- Fully managed service

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ecdd557e-00d2-4623-9a78-e59aabd247ad/Untitled.png)

### Amazon FSx for Windows (File Server)

- **FSx for Windows** is a fully managed Windows files system share drive
- Supports SMB protocol & Windows NTFS
- Microsoft Active Directory integration, ACLs ,users quotes
- **Can be mounted on Linux EC2 instances**
- Supports **Microsoft’s Distributed Files System (DFS) Namespaces** ( groups files across multiple FS

- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- Storage Options:
    - **SSD** - latency sensitive workloads (databases, media processing, data analytics…)
    - **HDD** - broad spectrum of workloads (home directory, CMS…)
    - Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
    - Can be configured to be Multi-AZ (high availability)
    - Data is backed-up daily to S3

### Amazon FSx for Lustre (Linux + Cluster)

- Lustre is a type of parallel distributed file system, for large-scale computing
- The name Lustre is derived from “Linux” and “cluster”

- Machine Learning, **High Performance Computing (HPC)**
- Video Processing, Financial Modeling, Electronic Design Automation
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Storage Options:
    - **SSD** - low-latency, IOPS intensive workloads, small & random file operations
    - **HDD** - throughput-intensive workloads, large & sequential file operations
- **Seamless integration with S3**
    - Can “read S3” as file system (through FSx)
    - Can write the output of the computations back to S3 (through FSx)
- Can be used from on-premises servers (VPN or Direct Connect)

### FSx File System Deployment Options

- Scratch File System
    - Temporary storage
    - Data is not replicated (doesn’t persist if file server fails)
    - High burst (6X faster, 200MBps per TiB)
    - Usage: short-term processing, optimize costs
- Persistent File System
    - Long-term storage
    - Data is replicated within same AZ
    - Replace failed files within minutes
    - Usage: long-term processing, sensitive data

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5fb74ee-d01d-475f-853f-f3519269cd6b/Untitled.png)


### Amazon FSx for NetApp ONTAP

- Managed NetApp ONTAP on AWS
- **File System compatible with NFS, SMB, iSCSI protocol**
- Move workloads running on ONTAP or NAS to AWS
- Works with:
    - Linux
    - Windows
    - MacOS
    - VMware Cloud on AWS
    - Amazon Workspaces & AppStream 2.0
    - Amazon EC2, ECS and EKS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression and data de-duplication
- **Point-in-time instantaneous cloning (helpful for testing new workloads)**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33ec7a09-b500-4494-b083-3e24a2101368/Untitled.png)

### Amazon FSx for OpenZFS

- Managed OpenZFS file system on AWS
- File System compatible with NFS (v3, v4, v4.1, v4.2)
- Move workloads running on ZFS to AWS
- Works with:
    - Linux
    - Windows
    - MacOS
    - VMware Cloud on AWS
    - Amazon Workspaces & AppStream 2.0
    - Amazon EC2, ECS and EKS
- UP to 1,000,000 IOPS with < 0.5ms latency
- Snapshots, compression and low-cost
- **Point-in-time instantaneous cloning (helpful for testing new workloads)**
## Hybrid Cloud for Storage

- AWS is pushing for “hybrid cloud”
    - Part of your infrastructure is on the cloud
    - Part of your infrastructure is on-premises
- This can be due to
    - Long cloud migrations
    - Security requirements
    - Compliance requirements
    - IT strategy
- S3 is proprietary storage technology (unlike EFS / NFS) , so how do you expose the S3 data on-premises?
- AWS Storage Gateway

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d78e23bc-94fa-4d2b-bd79-3b3c348731e7/Untitled.png)

### AWS Storage Gateway

- Bridge between on-premises data and cloud data
- Use cases:
    - disaster recovery
    - backup & restore
    - tiered storage
    - on-premises cache & low-latency files access
- Types of Storage Gateway:
    - **S3 File Gateway**
    - **FSx File Gateway**
    - **Volume Gateway**
    - **Tape Gateway**

### Amazon S3 File Gateway

- Configured S3 buckets are accessible using the NFS and SMB protocol
- **Most recently used data is cached in the file gateway**
- Supports S3 Standard, S3 Standard IA, S3 One Zone, S3 Intelligent Tiering
- **Transition to S3 Glacier using a Lifecycle Policy**
- Bucket access using IAM roles for each File Gateway
- SMB Protocol has integration with Active Directory (AD) for user authentication

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94e51c81-ac33-49af-8786-2e63c009d0fa/Untitled.png)

### Amazon FSx File Gateway

- Native access to Amazon FSx for Windows File Server
- **Local cache for frequently accessed data**
- Windows native compatibility (SMB, NTFS, Active Directory…)
- Useful for group file shares and home directories

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/713b15fa-22d6-4409-ac11-ac4334853f03/Untitled.png)

### Volume Gateway

- Block storage using iSCSI protocol backed by S#
- Backed by EBS snapshots which can help restore on-premises volumes
- **Cached volumes**: low latency access to most recent data
- **Stored volumes**: entire dataset is on premise, scheduled backups to S3

### Tape Gateway

- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but, in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/951db9bf-8b86-4186-8d04-6f3c379aecf9/Untitled.png)

### Storage Gateway - Hardware appliance

- Using Storage Gateway means you need on-premises virtualization
- Otherwise, you can use a Storage Gateway Hardware Appliance
- You can buy it on amazon.com

- Works with File Gateway, Volume Gateway, Tape Gateway
- Has the required CPU, memory, network, SSD cache resources

### AWS Storage Gateway

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c718045-b0fb-4996-b13c-2c39af24ac36/Untitled.png)

## AWS Transfer Family

- A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol
- Supported Protocols
    - **AWS Transfer for FTP** (File Transfer Protocol (FTP))
    - **AWS Transfer for FTPS** (File Transfer Protocol over SSL (FTPS))
    - **AWS Transfer for SFTP** (Secure File Transfer Protocol (SFTP))
- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
- Pay per provisioned endpoint per hour + data transfers in GB
- Store and manage users’ credential within the service
- Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)
- Usage: sharing files, public datasets, CRM, ERP…

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/53524293-4067-4b9c-bd23-cc8ffcf05694/Untitled.png)

## AWS DataSync

- Move large amount of data to and from
    - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) - **needs agent**
    - AWS to AWS (different storage services) - no agent needed
- Can synchronize to:
    - Amazon S3 (any storage classes - including Glacier)
    - Amazon EFS
    - Amazon FSx (Windows, Lustre, NetApp, OpenZFS…)
- Replication tasks can be scheduled hourly, daily, weekly.
- **File permissions and metadata are preserved (NFS POSIX, SMB…)**
- One agent task can use 10Gbps, can setup a bandwidth limit

### AWS DataSync - NFS / SMB to AWS (S3, EFS, FSx…)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c697c52-d073-4a80-8511-9e812e7bcadf/Untitled.png)

### AWS DataSync - Transfer between AWS storage services