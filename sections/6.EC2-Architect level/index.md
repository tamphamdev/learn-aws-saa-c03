# EC2 - Architect Solutions Level

## Private vs Public IP (IPv4)

- Networking có 2 loại IP: IPv4 và IPv6:
    - IPv4: 1.160.10.240
    - IPv6:3ffe: 1900:4545:3:200:f8ff:fe21:67cf
- IPv4 là dạng phổ thông nhất được sử dụng online
- IPv6 mới hơn giải quyết vấn đề của IoT
- IPv4 cho phép **3.7 tỉ** địa chỉ public khác nhay
- IPv4 [0-255].[0-255].[0-255].[0-255].


- **Public IP**:
    - Public IP có nghĩa máy chủ đc định danh bởi internet (www)
    - Phải là duy nhất trong toàn bộ web (2 máy chủ không thể có cung public IP)
    - Có thể dễ dàng bị đánh dấu vị trí địa lí

- **Private IP**:
    - Chỉ có thể thể được định danh trong private network
    - IP phải là duy nhất trong private network
    - Nhưng 2 private network có t hể có cùng IPs
    - Máy chủ kết nối với internet (www) thông qua internet gateway (proxy)

- **Elastic IPs**:
    - Khi stop và start 1 EC2 instance, nó có thể sẽ thay đổi public IP
    - Khi bạn cần public IP cố định (fixed public IP), bạn cần Elastic IP
    - Ngắn gọn Elastic IP là public IPv4 thuộc về bạn miễn là bạn không xoá nó
    - Chỉ có thể gắn 1 `EIP` (Elastic IP) vào 1 instance 1 lúc
    - Chỉ có thể có 5 `EIP` trong cùng 1 account (có thể xin AWS tăng thêm)
    - Túm lại **tránh xài EIP**:
        - Poor architectural
        - Thay vào đó sử dụng random public IP và đăng kí DNS cho nó
        - Cuối cùng có thể sử dụng Load Balancer và không sử dụng public IP


  **Note**: EIP sẽ charge tiền nếu không thoả mãn điều kiện sau:
      - Elastic IP không liên kết với 1 EC2 instance bất kì nào
      - EC2 instance được kết nối với Elastic IP address đang chạy
      - The instance has only one Elastic IP address attached to it.
      - Instance chỉ có 1 Elastic IP kết nối vào
      - Elastic IP liên kết với network interface
      - Nếu tạo ra để đó sẽ bị charge
## Placement Groups

- Muốn kiểm soát vị trị chiến lược EC2 Instance
- Có thể sử dụng placement group định nghĩa việc đó
- Khi tạo placement group, nên chỉ định cụ thể như sau:
    - *Cluster* - cluster instance vào trong 1 low-latency group trong 1 AZ đơn lẻ (nhóm lại)
    - *Spread* - spreads instance  across underlying hardware ( tối đa 7 instances mỗi group mỗi AZ) - critical applications (dàn trải)
    - *Partition* - spreads instances across many different partitions ( which rely on different sets of racks) with in an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
    1. ***Cluster***
    - Tất cả EC2 đặt chung vào 1 hardware, chung AZ
    - Pros: Băng thông tốt (10Gbps bandwidth giữa các instances), độ trễ thấp
    - Cons: nếu hardware hư hỏng ⇒ tất cả EC2 đều hỏng cùng lúc
    - Use case: công việc Big data cần hoàn thành nhanh, application cần network nhanh và độ trễ cực thấp ⇒ ***high risk***
    2. **Spread**


    - Đặt ở các hardware khác nhau ⇒ ***minimize failure risk***
    - Pros:
        - có thể đặt ở nhiều nơi trong AZ
        - Giảm thiểu rủi ro
        - EC2 instances đặt khác nhau trên các máy chủ vật lí
    - Cons:
        - Tối đa 7 instance cho mỗi AZ mỗi placement group
    - Use case:
        - Application cần sự tồn tại tối đa (lúc nào cũng online)
        - Những app fail nên được cô lập khỏi các app khác (ko chết đồng loạt)
    3. ***Partition***

    - Lên tới 7 partition mỗi AZ
    - Có thể phân bổ nhiều AZ trong cùng region
    - Lên tới 100 EC2 instances
    - Instance trong partition không share hardware với partition khác (EC2 ở part 1 ko share với EC2 ở part 2)
    - Một partition chết có thể ảnh hưởng nhiều EC2 nhưng không ảnh hưởng partition khác
    - EC2 instance có thể truy cập tới thông tin partition thông qa metadata
    - Use cases: Big data applications, HDFS, Hbase, Cassandra, Aplache Kafka

**Elastic Network Interfaces (ENI)**
- Logical components trong VPC (Virtual Private Cloud) đại diện cho **virtual network card**
- ENI có các thuộc tính như sau:
    - Primary private IPv4, 1 hoặc nhiều IPv4 phụ (secondary)
    - 1 Elastic IP (IPv4) mỗi private IPv4
    - 1 Elastic IP (IPv4) mỗi public IPv4
    - Một hoặc nhiều security groups
    - MAC Address
- Có thể tạo 1 ENI độc lập và attach chúng vào EC2 instances cho chuyển đổi dự phòng
- Bound to 1 AZ cụ thể
## **EC2 Hibernate**

- **Stop** - data trên ổ đĩa (EBS) sẽ giữ nguyên vẹn cho lần start tiếp thep
- **Terminate** - bất cứ EBS ổ đĩa sẽ bị mất data
- **Start**:
    - Chạy lần đầu: OS khởi động & EC2 User data script chạy
    - Bước tiếp theo: OS khởi động lên
    - Sau đó app start, cache khởi động, và có thể mất thời gian
- **Hibernate**:
    - Trạng thái của RAM được bảo toàn
    - Instance khởi động lẹ hơn (OS ko dừng/khởi động lại)
    - EBS volume gốc nên đc encrypted
    - ⇒ Khi hibernated RAM sẽ được lưu trữ tạm vào trong EBS volume gốc, khi Run sẽ lấy RAM từ trong EBS volume ra
    - Use cases:
        - Các tiến trình chạy dài
        - Lưu trữ trạng thái của RAM
        - Các service cần thời gian để khởi tạo