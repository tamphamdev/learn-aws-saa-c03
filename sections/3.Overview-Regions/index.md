## 3. Regions

## Global Infrastructure

- AWS Regions
- Availability zones (AZ)
- Local zones (Data centers)
- Edge Locations/Points of Presence

## AWS Regions

- AWS có các vùng trên toàn thế giới
- Được đặt tên như sau **us-east-1**, **eu-west-3**…
- Mỗi **vùng là một cụm data centers**
- Hầu hết dịch vụ của AWS là **phạm vi khu vực**

Chọn vùng như thế nào?

- Tuân thủ luật pháp của chính phủ, data không được rời khỏi phạm vi nước đó nếu ko có sự kiểm soát…
- Phạm vi của khách hàng ⇒ **giảm latency**
- Các dịch vụ có sẵn trong khu vực ⇒ 1 số khu vực sẽ có 1 vài feature của khu vực đó mà không phải ở đâu cũng có
- Pricing ⇒ khác biệt giữa các vùng cùng ảnh hưởng tới giá

## Availability zones (AZ)

- Mỗi khu vực (region) sẽ có nhiều AZ (thường là 3, min 3, max 6)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e340fd4-be29-4660-a28f-56ce28be4e4f/Untitled.png)

- Ví dụ: regions **Sysney có : ap-southeast-2**
    - ap-southeast-2a
    - ap-southeast-2b
    - ap-southeast-2c
        - Mỗi AZ như này sẽ có 1 hoặc nhiều data center rời rạc
        - Với năng lượng, network, kết nối độc lập với nhau khỏi thảm hoạ/thiên tai

        - Chúng kết nối với băng thông cao, và độ trễ cực thấp
- Mỗi AZ lại có nhiều data center rời rạc

## AWS Edge locations

- AWS có 216 Point of Presence ( 205 Edge locations  và 11 Regional caches) trong 84 thành phố qa 42 đất nước