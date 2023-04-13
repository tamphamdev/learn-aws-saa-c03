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