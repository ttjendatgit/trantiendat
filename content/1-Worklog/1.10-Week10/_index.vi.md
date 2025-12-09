---
title: "Worklog Tuần 10"
date: "2025-11-16"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Mục tiêu tuần 10:

* Tập trung vào việc cải thiện và tối ưu hiệu suất của API thời tiết.
* Tham dự sự kiện AWS Cloud Mastery Series #1.
* Triển khai các cải tiến để làm cho API trở nên mạnh mẽ và hiệu quả hơn.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | **Triển khai Caching cho API:** <br> - Nghiên cứu và triển khai chiến lược caching sử dụng Amazon ElastiCache (Redis) hoặc caching của API Gateway. <br> - Áp dụng caching cho dữ liệu thời tiết được yêu cầu thường xuyên (ví dụ: theo thành phố) để giảm lệnh gọi API bên ngoài và độ trễ. | 10/11/2025 | 10/11/2025 | 
| 3 | **Xử lý Lỗi & Logic Thử lại:** <br> - Cải thiện xử lý lỗi trong các hàm Lambda khi API bên ngoài gặp sự cố. <br> - Triển khai logic thử lại với exponential backoff cho các lỗi tạm thời. <br> - Xác định định dạng phản hồi lỗi rõ ràng cho API. | 11/11/2025 | 11/11/2025 | 
| 4 | **Tối ưu Hiệu suất Lambda:** <br> - Xem xét và điều chỉnh cấu hình hàm Lambda (bộ nhớ, timeout). <br> - Triển khai Lambda layers cho các phụ thuộc được chia sẻ (ví dụ: client API thời tiết). <br> - Tối ưu mã để giảm thời gian khởi động lạnh (cold start). | 12/11/2025 | 12/11/2025 | 
| 5 | **Giám sát & Ghi Log API:** <br> - Thiết lập Amazon CloudWatch Logs để ghi log cho hàm Lambda. <br> - Tạo CloudWatch Alarms cho tỷ lệ lỗi và độ trễ cao. <br> - Triển khai structured logging để dễ dàng gỡ lỗi. | 13/11/2025 | 13/11/2025 | CloudWatch |
| 6 | **Bảo mật & Giới hạn Tốc độ:** <br> - Cấu hình usage plans và API keys trong API Gateway để giới hạn tốc độ cơ bản. <br> - Xem xét và siết chặt các vai trò và chính sách IAM cho hàm Lambda. <br> - Đảm bảo xử lý an toàn khóa API bên ngoài bằng AWS Secrets Manager. | 14/11/2025 | 14/11/2025 | API Gateway, IAM, Secrets Manager |
| 7 | **Tham dự AWS Cloud Mastery Series #1:** <br> - Tham gia sự kiện để tìm hiểu về các dịch vụ và kiến trúc AWS nâng cao. <br> - Ghi chép lại những điểm chính liên quan đến dự án. | 15/11/2025 | 15/11/2025 | 

### Thành tựu tuần 10:
* Tăng cường độ mạnh mẽ của API với việc xử lý lỗi và cơ chế thử lại được cải thiện.
* Tối ưu hóa hàm Lambda để có hiệu suất và khả năng quản lý tốt hơn.
* Thiết lập giám sát và ghi log để có tầm nhìn hoạt động.
* Cải thiện bảo mật API với giới hạn tốc độ và quản lý thông tin xác thực an toàn.
* Thu được những hiểu biết mới từ sự kiện AWS Cloud Mastery Series.
