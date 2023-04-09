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
  6. **SSH**
      - Generate file PEM
      - `ssh -i fileName.pem ec2-user@public-ip-address`
          - Trong đó ec2-user vì EC2 AIM có user setup sẵn và user đó tên là **ec2-user**
      - Nếu bị lỗi `permission 0644`
          - Run command ⇒ `chmod fileName.ped`
  7. **EC2 Instance options**
      - **On-demand**  ⇒ ngắn hạn, dự đoán được giá, trả theo giây
      - **Reserved**( 1 & 3 years):
          - Reserved instances - dài hạn
          - Convertible Reserved instances - dài hạn nhưng linh hoạt giữa các instances
      - **Savings plans** ( 1 & 3 years) ⇒ cam kết sử dụng dài hạn
      - **Spot Instances** ⇒ sử dụng ngắn hạn, rẻ, có thể mất instances (ít độ tin cậy)
      - **Dedicated Host** ⇒ book toàn bộ server vật lí, kiểm soát nơi đặt instance
      - **Dedicated Instances** ⇒ ko có users nào xài chung hardware với bạn
      - **Capacity Reservations** ⇒ đặt trước dung lượng cho bất cứ khoản thời gian nào

      1. **On-demand**
          1. Trả bất cho những gì sử dụng
          2. Phí cao nhất nhưng không cần trả trước
          3. Không cần cam kết dài hạn
          4. Đề nghị cho workload ngắn hạn hoặc ko liên tục, khi mà bạn ko dự đoán được ứng dụng sẽ như thế nào
              1. Linux hoặc Win ⇒ tính trên từng giây, sau khi phút đầu tiên
              2. Tất cả hệ điều hành ⇒ tính trên từng giờ
      2. **Reserved Instances**
          1. Giảm giá **~72%** so với On-demand
          2. Bạn sẽ đặt trước 1 thuộc tính instances cụ thể **(Instance Type, Region, Tenancy, OS)**
          3. **Đặt trước định kì** - **1 năm** (+discount) hoặc **3 năm** (+++discount)
          4. **Lựa chọn thanh toán** - **Không trả trước** (+), **Trả trước 1 phần** (++), **Trả trước tất cả** (+++)
          5. **Đặt trước phạm vi Instance** - **Vùng** (đặt trước dung lượng của AZ - availability zone)
          6. Đề nghị sử dụng khi ứng dụng vào trạng thái hoạt động ổn định (database)
          7. Có thể giao dịch được
          8. Có thể đổi qua Instance khác
              1. Có thể đổi instance type, instance family(?), OS, phạm vi, tenancy
              2. Giảm giá **66%**
      3. **Saving plans**
          1. Được giảm giá khi sử dụng dài hạn ~72% giống với RI
          2. Cam kết 1 kiểu nhất định khi sử dụng ($10/giờ cho 1 hoặc 3 năm)
          3. Nếu xài vượt quá EC2 Saving Plan sẽ tính giá của On-demand
          4. Lock instance family nhất định & AWS region (M5 in us-east-1)
          5. Linh hoạt giữa:
              1. Instance size (eg: m5.xlarg, m5.2xlarge)
              2. OS (eg Linux, Window)
              3. Tenancy (Host, Dedicated, Default)
      4. **Spot Instances**
          1. Giảm giá tới 90% so với On-demand
          2. Có thể mất instance bất cứ lúc nào
          3. Instance **hiệu quả nhất về giá** trong AWS
          4. **Phù hợp cho các công việc có khả năng chịu được sự cố**
              1. Batch jobs
              2. Data analysis
              3. Image processing
              4. Bất cứ **distributed** workloads
              5. Công việc với thời gian linh hoạt bắt đầu và kết thúc
          5. **Không phù hợp với công việc khẩn cấp hoặc database**
      5. **Dedicated Hosts**
          1. Server vật lí với EC2 Instance cho bạn thoải mái sử dụng
          2. Cho phép bạn đáp ứng yêu cầu tuân thủ và sử dụng các giấy phép phần mềm hiện có được liên kết với máy chủ của bạn (giấy phép phần mềm trên mỗi socket, trên mỗi lõi, trên mỗi máy ảo)
          3. Options:
              1. **On-demand** - trả theo giây cho Dedicated Host kích hoạt
              2. **Reserved** - 1 hoặc 3 năm (Không trả trước, trả trước 1 phần, trả trước toàn bộ)
          4. Option mắc nhất
          5. Hiệu quả cho phần mềm có bản quyền model phức tạp (BYOL - Bring Your Own License)
      6. **Dedicated Instances**
          1. Instances chạy trên phần cứng được chỉ định
          2. Có thể share phần cứng với instance khá trong cùng account
          3. Không thể kiểm soát nơi đặt instance (có thể di chuyển phần cứng sau khi Stop/Start)
          4. Khác biệt với **Dedicated Host:**

          ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/060f838f-f4e7-4dab-9a24-af7a05cdfa9e/Untitled.png)

      7. **Capacity Reservations**
          1. Đặt trước dung lượng **On-demand** instances với AZ cụ thể cho bất cứ thời lượng
          2. Bạn luôn có quyền try cập tới dung lượng EC2 khi muốn
          3. **Không cam kết thời gian** (create/cancel bất cứ khi nào), **không giảm giá**
          4. Kết hợp Regional Reserved Instances và Savings Plan để được lợi ích giảm giá
          5. Bạn sẽ bị tính phí On-demand khi bạn chạy instance hoặc không
          6. Phù hợp cho ngắn hạn, không bị gián đoạn công việc và cần 1 AZ cụ thể

      ### **Summary**:

      - **On demand**: đến và ở nghỉ dưỡng bất cứ khi nào chúng ta muốn, chúng ta trả đầy đủ tiền.
      - **Reserved**: giống như lên kế hoạch trước và nếu chúng ta lên kế hoạch ở lại trong thời gian dài, chúng ta có thể nhận được mức giảm giá tốt.
      - **Savings Plans**: trả một số tiền cố định cho mỗi giờ trong một khoảng thời gian nhất định và lưu trú ở bất kỳ loại phòng nào (ví dụ: phòng King, phòng Suite, phòng hướng biển, ...)
      - **Spot instances**: khách sạn cho phép người ta đấu giá các phòng trống và người đặt giá cao nhất giữ lại các phòng đó. Bạn có thể bị đuổi ra bất cứ lúc nào.
      - **Dedicated Hosts**: chúng ta đặt toàn bộ tòa nhà của khu nghỉ dưỡng.
      Capacity Reservations: bạn đặt phòng trong một khoảng thời gian và trả đầy đủ tiền, ngay cả khi bạn không ở trong đó.
  8. **EC2 Spot Instance Request (giống đấu giá)**
      1. Có thể giảm giá tới 90% khi so sánh với On-demand
      2. Định nghĩa **max spot price** và có đc instance khi **current spot price < max spot price**
          1. Giá mỗi giờ dựa trên offer và capacity
          2. Nếu current spot price > max price ⇒ bạn có thể chọn **stop** hoặc **terminate** instance trong 2 phút
      3. Spot Block (ngừng support sau 31-12-2022)
          1. Chiếm 1 chổ instance trong 1 khoảng thời gian cụ thể ( 1-6 giờ) không bị gián đoạn
          2. Trong trường hợp hiếm, instances có thể bị thu hồi
      4. **Dùng cho batch jobs, data analysis**, **các công việc có khả năng chịu được sự cố**
      5. **Không phù hợp với công việc khẩn cấp hoặc database**
      6. Làm sao để terminate Spot Instances?
          1. Spot Instance có 2 loại request: **one-time** | **persistent**
              1. One-time ⇒ launch instance ⇒ done ⇒ chỉ chạy 1 lần duy nhất
              2. Persistent ⇒ launch instance ⇒ done/stop/interrupt ⇒ auto chạy lại từ đầu
          2. Chỉ có thể cancel Spot Instance request khi đang **open**, **active**, **disabled**
          3. **Cancel Spot Request Instance không làm terminate instances**
          4. Cancel Spot Request trước sau đó mới terminate Spot Instances
  9. **Spot Fleet**
      1. Spot Fleets = Spot Instances + (optional) On-demand Instances
      2. Spot Fleets sẽ cố sử dụng instances target với giá mà được đặt trước
          1. Định nghĩa các pools có thể triển khai: instances type (m5.large), OS, AZ
          2. Có thể có nhiều pools để Fleet có thể chọn
          3. Spot Fleet ngừng triển khai instance khi đạt đến max cost hoặc capacity
      3. Chiến thuật khi triển trai Spot Instances:
          1. **lowestPrice**: từ pool có giá thấp nhất (tối ưu hoá cost, công việc ngắn hạn)
          2. **diversified**: phân tán giữa các pools (tối cho sự khả dụng, công việc dài hạn)
          3. **capacityOptimized**: pool với sự tối ưu khả năng của các instances
      4. **Spot Fleet cho phép chúng ta tự động request Spot Instances với giá thấp nhất**