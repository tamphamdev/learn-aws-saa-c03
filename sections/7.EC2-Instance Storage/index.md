# EC2 Instance Storage

## EBS Volume

- **EBS (Elastic Block Store)** **Volume** là 1 **network drive** (không phải physical drive) có thể được attach tới instance khi nó chạy
- Bị ràng buộc ở **1 AZ cụ thể**
- Cho phép instance giữ data kể cả khi nó bị terminate
- **Nó chỉ được mounted tới 1 EC2 instance trong 1 lúc** (ở CCP level - Certificate Cloud Practice) level  SA, Developer, SysOps: “multi-attach” feature for some EBS
- Giống nhau: nó giống như “network USB”
- Free tier: 30GB free EBS storage loại Gerneral Purpose (SSD) hoặc Magnetic mỗi tháng
- Là network drive (không phải physical drive)
    - Dùng network để giao tiếp giữa các instances, có nghĩa là sẽ có 1 chút độ trễ
    - Có thể attach và detach từ 1 EC2 instance 1 cách dễ dàng và nhanh chóng
- Bị khoá bởi AZ
    - EBS Volume ở us-east-1a không thể attach tới us-east-1b
    - Để di chuyển volume xung quanh, bạn cần snapshot nó trước
- Phải cung cấp dung lượng của EBS (size - GBs và IOPS - **I/O operations per second**)
- Bạn sẽ đc tính tiền bởi tất cả dung lượng bạn cũng cấp
- Có thể tăng dung lượng liên tục
- EBS - Delete on Termination
    - Kiểm soát EBS khi EC2 instance bị terminate
        - Mặc định, root EBS sẽ bị delete (enabled)
        - Mặc định, bất cứ EBS volume khác không bị delete (disabled)
    - Có thể thay đổi bởi AWS console / AWS CLI
    - **Use case: bảo toàn root volume khi instance bị terminated**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/751ecdab-8e4e-423b-b9c5-0d7781892c2c/Untitled.png)
## EBS Snapshot

- Backup EBS volume tại 1 thời điểm
- Không cần thiết detach volume để snapshot, nhưng khuyến khích
- Có thể copy snapshot xung quanh AZ hoặc region

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ada3fe88-c76f-4a31-8aa7-93f57794f73f/Untitled.png)

- Features:
    - EBS Snapshot Archive
        - Di chuyển 1 snapshot tới “archive tier” đó rẻ hơn 75%
        - Cần khoảng 24 - 72 giờ để restore archive đó
    - Recycle Bin
        - Cài đặt rule để giữ lạp snapshot đã delete, sau đó có thể recover lại sau khi lỡ xoá nhầm
        - Rules giữa lại (1 ngày - 1 năm)
    - Fast Snapshot Restore (FSR)
        - Bắt buộc khởi tạo đầy đủ snapshot ko có độ trễ cho lần đầu sử dụng ($$$)
## AMI - Amazone Machine Image

- AMI là sự **tuỳ biến** của EC2 instance
    - Có thể tự thêm software, configuration, OS, monitoring…
    - Khởi động nhanh hơn/thời gian config bởi vì tất cả đều được chuẩn bị trước
- AMI có thể được build cho **1 số vùng nhất định** ( và copy sang vùng khác)
- Có thể lauch EC2 từ:
    - **Public AMI**: AWS cung cấp
    - **AMI của bản thân**: tự tạo và bảo trì nó
    - **Hoặc trên AWS Marketplace AMI**: ai đó tạo và bán
### AMI Process (From an EC2 instance)

- Start EC2 instance và customize i
- Stop the instance (để bảo toàn data)
- Build AMI - nó cũng sẽ tạo EBS snapshot
- Launch instance từ AMI
### EC2 Instance store

- EBS volume là network drive (giống ổ đĩa USB) nhưng giới hạn performance
- **Nếu bạn cần high-performance hardware disk, sử dụng EC2 Instance Store**
- Better I/O performance
- EC2 Instance Store sẽ mất nếu chúng bị stop (ephemeral - ngắn hạn, không lâu dài) **(không tốt cho lưu trữ dài hạn)**
- Good ⇒ buffer / cache / scratch data/ temporary content
- Nếu hardware sự cố ⇒ có nguy cơ bị mất data
- Backup và copy là điều nên làm
### EBS Volume Types

- Bao gồm 6 loại:
    - **gp2 / gp3 (SSD)**: Dùng cho mục đích sử dụng chung, cân bằng giữa giá tiền và hiệu năng ⇒ đa dạng công việc
    - **io1 / io2 (SSD**): Performance cao nhất cho các nhiệm vụ quan trọng, độ trễ tháp và lưu lượng cao
    - **st1 (HDD)**: HDD giá rẻ, được thiết kế cho sự truy cập thường xuyên và lưu lượng căng(?)
    - **sc1 (HDD)**: HDD giá rẻ nhất, được thiết kế cho công việc ít truy cập thường xuyên
- **Chỉ có gp2/gp3 và io1/io2 có thể được sử dụng như boot volume**
- EBS volume được kí hiệu cho Size | Throughput | IOPS (I/O Ops Per Sec)
- Type use cases:
    - **General SSD:**
        - Giá thành hiệu quả, độ trễ thấp
        - System boot, Virtual desktop, Development, Testing enviroments
        - 1Gb - 16Tb
        - **gp2**:
            - 1 Gps nhỏ có thể burst IOPS tới 3000
            - Size của volume và IOPS liên kết với nhau, IOPS tối đa là 16,000
            - 3 IOPS mỗi GB, có nghĩa là tại 5334GB chúng ta max IOPS (vì bị liên kết)
        - **gp3**:
            - Cơ bản 3000 IOPS và lưu lượng 125Mb
            - Có thể tăng IOPS lên tới 16,000 and có lưu lượng có thể lên tới 1000MiB/s độc lập

    - **Provisioned IOPS (PIOPS) SDD:**
        - Các app có business khẩn cấp với độ duy trì IOPS cao
        - Hoặc app cần > 16,000 IOPS
        - Tốt cho các **công việc databases (perf lưu trữ data và liên tục)**
        - **io2 Block Express** (4GiB- 64TiB):
            - Sub milisenconds latency
            - PIOPS: 256,000 với IOPS:GiB tỉ lệ 1000:1
        - Hỗ trợ attach nhiều EBS
        - io1/io2 (4 GiB - 16TiB):
            - Max PIOPS: 64,000 cho Nitro EC2 instance & 32,000 cho các instance khác
            - Có thể tăng PIOPS **độc lập** từ storage size
            - io2 có độ ổn định và IOPS mỗi GiB hơn (**cùng giá với io1**)
    - **Hard Disk Drive (HDD):**
        - Không thể là boot volume
        - 125GiB tới 16TiB
        - Lưu lượng optimized (st1)
            - Big data, Data warehouses, Log processing
            - **Max lưu lượng** 500MiB/s - max IOPS 500
        - Cold HDD (sc1):
            - Cho data được truy cập thường xuyên
            - Trong bối cảnh chi phí thấp là quan trọng
            - **Max lưu lượng** 250MiB/s - max IOPS 250
### EBS Multi Attach - io1/io2 family

- Attach cùng EBS volume tới nhiều EC2 instance trong cùng 1 AZ
- Mỗi instance có full quyền đọc/viết tới high-perfomance volume cùng lúc
- Use case:
    - Có được 1 application với khả năng cao hơn trong cluster Linux applications(ex: Teradata)
    - Application cần được quyền write đồng thời
- **Tối đa 16 EC2 instance cùng lúc**
- Phải sử dụng file systems là cluster-aware ( không phải XFS, EXT4…)
### EBS Encryption

- Khi bạn tạo 1 EBS volume mã hoá, phải tuân theo:
    - Data ở phần còn lại được mã hoá bên trong ổ đĩa
    - Tất cả data di chuyển giữa các instance và ổ đĩa là đã đc mã hoá
    - Tất cả snapshot đc mã hoá
    - Tất cả ổ đĩa được tạo từ snapshot

- Việc mã hoá và giải mã được xử lí minh bạch (ko cần phải làm gì cả)
- Việc mã hoá có ít ảnh hưởng tới độ trễ
- EBS Encryption leverages key form KMS (AES-256)
- Snapshot của ổ đĩa bị mã hoá cũng mã hoá
- Copy 1 snapshot không mã hoá cho phép mã hoá
- Mã hoá: mã hoá 1 ổ đĩa EBS không được mã hoá
    - Tạo 1 EBS snapshot của ổ đĩa
    - Mã hoá EBS snapshot (sử dụng copy)
    - Tạo mới EBS ổ điã từ snapshot (ổ đĩa sẽ bị mã hoá)
    - Bây giờ bạn có thể attach ổ đĩa mã hoá vào instance
## EFS - Elastic File System

- Quản lí NFS (network file system) có thể gắn vào nhiều EC2
- EFS hoạt động với nhiều EC2 instance trong nhiều AZ
- Khả năng sử dụng cao, scalable, đắt đỏ (3 lần gp2), trả mỗi lần sử dụng

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3abe2bd4-279c-4fe0-bfec-c5a2c6fe7adf/Untitled.png)

- Use case: content management, web serving, data sharing, Wordpress
    - Sử dụng NFSv4.1 protocol
    - Sử dụng **security group** để kiểm soát truy cập EFS
    - **Tương thích với Linux based AMI (not Windows)**
    - Mã hoá khi nghỉ sử dụng KMS
    - POSIX file system (~Linux) có chuẩn file API
    - File system scale tự động, trả mỗi khi sử dụng, không cần định mức sử dụng

## EFS - Performance & Storage classes

- **EFS Scale**
    - 1000 NFS client đồng thời, 10GB+/s lưu lượng
    - Scale lên tới Petabype network file system, tự động
- **Performance Mode (set lúc EFS creation time)**
    - Mục đích sử dụng chung (default) - latency - các trường hợp nhạy cảm (webserver, CMS…)
    - Max I/O - higher latency, throughput, highly parallel (big data, media processing)
- **Throughput Mode**
    - **Bursting** - 1TB = 50MiB/s + burst of up to 100MiB/s
    - **Provisioned** - set your throughput regardless of storage size, ex 1GiB/s for 1 TB storage
    - **Elastic** - automatically scales throughput up or down based on your workloads
        - Up yo 3GiB/ for reads and 1GiB/s for writes
        - Used for unpredictable workloads
- **Storage Classes**
    - **Storage tiers (lifecyle management feature - move file after N days)**
        - Standard: for frequently accessed files
        - Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable EFS-IA with a Lifecycle Policy

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/30ea6f3d-e897-4131-afad-8e8c17fd8637/Untitled.png)

    - **Availability and durability**:
        - Standard: Multi-AZ, great for prod
        - One Zone: One AZ, greate for dv, backup enabled by default, compatible with IA (EFS One Zone-IA)
    - Over 90% in cost savings

    **Summary**: [hand on Udemy](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/learn/lecture/13528130#announcements)


## EBS vs EFS - Elastic Block Storage

- **EBS volumes…**
    - one intance (except multi-attach io1/io2)
    - are locked at the AZ level
    - gp2: IO increase IP independently
- **To migrate an EBS volume across AZ**
    - Take a snapshot
    - Restore the snapshot to another AZ
    - EBS backups use UI and you shouldn’t run them while your application is handling a lot of traffice
- **Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated (you can disable that)**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f50c823-603a-4ee8-8863-e4df8c1c0fbf/Untitled.png)

- **EFS - Elastic File System**
    - Mounting 100s of instances arcoss AZ
    - EFS share website files (WordPress)
    - Only for Linux instances (POSIX)
    - EFS has a higher price point than EBS
    - Can leverage EFS-IA for cost savings
    - Remember: EFS vs EBS vs Instance store (physically attach to EC2 instance, lose instance lose store)
    
**Quiz**:
- EC2 Instance store highest performance, >250k IOPS, lose data when EC2 stop, cache
- When launch EC2 instance with 2 EBS volumes, Root volume delete when EC2 terminated by default, other EBS will not
- EFS ⇒ can access same data at the same time ⇒ point into file systems