---
title: "Worklog Tuần 7"
date: "2025-10-24"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tập trung ôn tập 4 trụ cột chính của AWS Well-Architected Framework
* Nắm vững các dịch vụ AWS trọng tâm cho kỳ thi
* Thực hành các bài lab về bảo mật và hiệu năng

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2 | **Thiết kế kiến trúc bảo mật (Secure Architectures)**<br>- IAM: Users, Groups, Roles, Policies<br>- MFA và các phương thức xác thực<br>- Service Control Policies (SCPs) cho Organizations<br>- Encryption: KMS (CMKs, Data Keys), TLS/ACM<br>- Review Security Groups vs NACLs<br>- AWS Shield & WAF | 20/10/2025 | 20/10/2025 |  |
| 3 | **Tiếp tục Secure Architectures**<br>- Amazon GuardDuty: Threat detection<br>- AWS Secrets Manager<br>- VPC Security: Flow Logs, Endpoints<br>- Data encryption at rest & in transit<br>- **Thực hành:**<br>&emsp; + Tạo IAM Policies phức tạp<br>&emsp; + Cấu hình KMS với CMK<br>&emsp; + Thiết lập WAF rules | 21/10/2025 | 21/10/2025 |  |
| 4 | **Thiết kế kiến trúc linh hoạt và bền vững (Resilient Architectures)**<br>- Multi-AZ deployments: RDS, EC2, ELB<br>- Multi-Region strategies<br>- Disaster Recovery: Backup & Restore, Pilot Light, Warm Standby<br>- Auto Scaling: Policies, Lifecycle Hooks<br>- Route 53: Routing Policies, Health Checks | 22/10/2025 | 22/10/2025 |  |
| 5 | **Tiếp tục Resilient Architectures**<br>- Load Balancing: ALB, NLB, GLB<br>- Database resilience: RDS Multi-AZ, Aurora Global Database<br>- Storage resilience: S3 Cross-Region Replication<br>- **Thực hành:**<br>&emsp; + Cấu hình Auto Scaling group<br>&emsp; + Thiết lập Multi-AZ RDS<br>&emsp; + Tạo Route 53 failover | 23/10/2025 | 23/10/2025 |  |
| 6 | **Thiết kế hệ thống hiệu năng cao (High-Performing Architectures)**<br>- Compute scaling: EC2 Auto Scaling, Lambda, Fargate<br>- Storage: S3 performance, EFS, EBS types<br>- Caching: ElastiCache (Redis/Memcached)<br>- Network Optimization: CloudFront, Global Accelerator | 24/10/2025 | 24/10/2025 |  |


### Kết quả đạt được tuần 7:

* **Nắm vững Secure Architectures:**
  * Hiểu sâu IAM: Policies, Roles, Permission Boundaries
  * Thành thạo KMS: CMK, Data Keys, Key Policies
  * Phân biệt Security Groups vs NACLs
  * Biết cách triển khai WAF và Shield

* **Thành thạo Resilient Architectures:**
  * Thiết kế Multi-AZ và Multi-Region
  * Hiểu các chiến lược Disaster Recovery
  * Cấu hình Auto Scaling và Load Balancing
  * Sử dụng Route 53 cho failover

* **Thực hành thành công:**
  * Tạo IAM policies phức tạp
  * Cấu hình KMS encryption
  * Thiết lập Auto Scaling groups
  * Triển khai Multi-AZ databases

