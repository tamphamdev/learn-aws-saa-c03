**Note: luôn setup billing alert trước khi sử dụng dịch vụ**

1. **Amazon EC2**
    - EC2 là 1 trong những dịch vụ phổ biến nhất của Amazon
    - EC2 = Elastic Compute Cloud = Infrastructure as Service
    - Các khả năng chính bao gồm có:
        - Thuê virtual machines(EC2)
        - Chứa dữ liệu trên virtual drive (EBS)
        - Phân tán và cân bằng tải (ELB aka Load balancer)
        - Auto scaling (ASG)
2. **EC2 Sizing & configuration options**
    - Lựa chọn hệ điều hành(**OS**): Linux, Windows, MacOS
    - **CPU** core & power
    - **Ram**
    - Storage space:
        - Network-attached (**EBS** & **EFS**)
        - Hardware (**EC2 instance store**)
    - Network card: tốc độ của card mạng, Public IP Address
    - Firewall rules: **security group**
    - Bootstrap script (configure cho lần đầu): EC2 User data
3. **EC2 User data**
    - Có thể dùng EC2 User data script để bootstrap instance
    - Bootstraping nghĩa là khởi chạy command khi EC2 start
    - Script chỉ thực thi duy nhất 1 lần khi instance chạy lần đầu
    - EC2 user data thường dùng để tự động boot các task như:
        - Installing updates
        - Installing software
        - Downloading common files từ internet
        - Hoăc bất cứ thứ gì bạn có thể nghĩ ra
    - EC2 User Data Script chạy với Root user
    - **Note**:
        - PublicIP có thể bị thay đổi khi start ⇒ stop state instance. Private thì không
        - Khi stop instance thì ko bị charge tiền.
        - Khi terminate sẽ mất volume và data
4. **EC2 Instance Types**
    - m5.2xlarge
    - m: Instance class
    - 5: thế hệ (AWS liên tục cải tiến)
    - 2xlarge: size trong instance class

    Các kiểu EC2:

    **Mục đích chung - General:**

    - Đa dạng mục đích sử dụng cho web server và code repositories
    - Cân bằng giữa:
        - Tính toán
        - Bộ nhớ
        - Băng thông

    **Tối ưu hoá tính toán - Computed Optimized**

    - Sử dụng tốt cho những yêu cầu được tính toán với vi xử lí năng suất cao:
        - Thực thi một nhóm công việc
        - Media transcoding
        - Web server hiệu năng cao
        - Tính toán hiệu năng cao (HPC-High performance computing)
        - Machine learning
        - Game servers

    **Tối ưu bộ nhớ - Memory Optimized**

    - Hiệu suất xử lí nhanh với lượng dữ liệu lớn trong bộ nhớ
    - Dùng để:
        - Hiệu xuất cao, quan hệ/không quan hệ database
        - Web scale cache stores
        - In-memory database optimized cho BI
        - Applications performing real-time processing of big unstructured data

    **Tối ưu bộ nhớ - Storage Optimized**

    - Dùng để lưu lượng với data với tần suất xử dụng thường xuyên
    - Dùng để:
        - Relational & NoSQL DB
        - Cache in memory database (Redis)
        - Data warehousing
        - Distributed file systems
5. **Security Groups**
    - Security Groups là nền tảng của an minh mạng trong AWS
    - Có thể được sử dụng bởi nhiều instances
    - Nên tạo 1 group riêng để có thể truy cập SSH
    - Khoá bởi region/VPC (nếu đổi region phải tạo lại)
    - Kiểm soát được traffic ra vào của EC2 instances.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ead753bf-567f-42f5-84ee-0da27f30f533/Untitled.png)

    - Security groups chỉ chứa **allow** rules
    - Security groups rules có thể reference bởi IP hoặc bởi security group??
    - Security groups đc coi như là “firewall” bên ngoài EC2 instances
    - Port :
        - 22 = SSH (Secure Shell) - log into a Linux instance
        - 21 = FTP (File Transfer Protocol) - upload files into a file share
        - 22 = SFTP (Secure File Transfer Protocol) - upload files using SSH
        - 80 = HTTP - access unsecured websites
        - 443 = HTTPS - access secured websites
        - 3389 = RDP (Remote Desktop Protocol) - log into a Windows instance
    - Chúng qui định:
        - Truy cập port
        - Authorised IP range - IPv4 & IPv6
        - Kiểm soát mạng ra vào ( từ bên ngoài vào instance và từ instance ra ngoài)
    - Nếu một group cho phép truy cập inbound của group khác thì instance của group đó mặc định được truy cập

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f2bb5e9-e078-4f61-999e-c2814feec589/Untitled.png)

    - Note: nếu SSH hoặc truy cập qua HTTP mà timeout ⇒ 100% lỗi security group