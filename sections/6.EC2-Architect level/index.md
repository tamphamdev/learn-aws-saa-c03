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