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

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60a9bbe3-50f9-4eed-806f-26c3bab0efde/Untitled.png)

        - Tất cả EC2 đặt chung vào 1 hardware, chung AZ
        - Pros: Băng thông tốt (10Gbps bandwidth giữa các instances), độ trễ thấp
        - Cons: nếu hardware hư hỏng ⇒ tất cả EC2 đều hỏng cùng lúc
        - Use case: công việc Big data cần hoàn thành nhanh, application cần network nhanh và độ trễ cực thấp ⇒ ***high risk***
    2. **Spread**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a4cc4c7-847d-4130-98d0-2736bf174636/Untitled.png)

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