# EC2 Instance Storage

## EBS Volume

- **EBS (Elastic Block Store)** **Volume** là 1 **mạng lưới**  (network) **** ổ cứng có thể được attach tới instance khi nó chạy
- Cho phép instance giữ data kể cả khi nó bị terminate
    - **Nó chỉ được mounted tới 1 EC2 instance trong 1 lúc** (ở CCP level - Certificate Cloud Pratice) level  SA, Developer, SysOps: “multi-attach” feature for some EBS