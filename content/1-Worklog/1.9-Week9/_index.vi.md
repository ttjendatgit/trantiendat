---
title: "Worklog Tuần 9"
date: "2025-09-09"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9:

* Nghiên cứu và xác định các dịch vụ AWS phù hợp để xây dựng API thời tiết.
* Thiết kế và hoàn thiện cấu trúc API hoàn chỉnh sử dụng đặc tả Swagger/OpenAPI.
* Cộng tác với nhóm Frontend để thống nhất thiết kế API.
* Quản lý phiên bản cho đặc tả API bằng Git.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2 | **Nghiên cứu Dịch vụ AWS:** <br> - Nghiên cứu AWS API Gateway để quản lý và tạo API. <br> - Xác định AWS Lambda làm tầng điện toán serverless. <br> - Xem xét các nhà cung cấp dữ liệu thời tiết bên ngoài và phương pháp tích hợp API. | 03/11/2025 | 03/11/2025 | Tài liệu AWS |
| 3 | **Xác định Endpoint API & Luồng Dữ liệu:** <br> - Xác định 4 endpoint API cốt lõi. <br> - Thiết kế lược đồ request/response cho mỗi endpoint. | 04/11/2025 | 04/11/2025 | Kế hoạch dự án |
| 4 | **Mô hình Dữ liệu & Tích hợp Bên ngoài:** <br> - Xác định mô hình dữ liệu cho phản hồi thời tiết (nhiệt độ, độ ẩm, điều kiện, v.v.). <br> - Xác định mô hình dữ liệu cho phản hồi dự báo 5 ngày. <br> - Nghiên cứu và chọn một API thời tiết miễn phí (ví dụ: OpenWeatherMap). <br> - Lập kế hoạch logic tích hợp trong hàm Lambda. | 05/11/2025 | 05/11/2025 | 
| 5 | **Bản nháp Đặc tả Swagger/OpenAPI:** <br> - Bắt đầu viết file YAML Swagger (OpenAPI 3.0). <br> - Xác định đối tượng `paths` với tất cả 4 endpoint. <br> - Chỉ định các tham số (path, query) và ví dụ request. <br> - Xác định lược đồ và ví dụ phản hồi cho các trường hợp thành công/lỗi. | 06/11/2025 | 06/11/2025 |
| 6 | **Cộng tác & Kiểm soát Phiên bản:** <br> - Cộng tác với nhóm Frontend để xem xét và hoàn thiện các endpoint API và mô hình dữ liệu. <br> - Điều chỉnh đặc tả Swagger dựa trên phản hồi. | 07/11/2025 | 07/11/2025 | 

### Thành tựu tuần 9:
* Đã nghiên cứu và chọn AWS API Gateway và Lambda làm nền tảng cho API thời tiết.
* Đã thiết kế và lập tài liệu cho bốn endpoint API cốt lõi để truy xuất dữ liệu thời tiết.
* Đã cộng tác thành công với nhóm Frontend để thống nhất thiết kế API.
* Đã hoàn thiện và kiểm soát phiên bản đặc tả API bằng Git.