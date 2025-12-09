---
title: "Worklog Tuần 4"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Mục tiêu tuần 4:

* Bắt đầu học các kiến thức cơ bản về Amazon Virtual Private Cloud (VPC) và các thành phần chính của nó.
* Hiểu cách thiết kế kiến trúc mạng bảo mật và hiệu quả trên AWS.
* Chuẩn bị và tích cực tham gia Workshop GenAI Builder.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | **Bắt đầu với Amazon Virtual Private Cloud (VPC):** <br> - Hiểu khái niệm và mục đích của VPC như một mạng riêng của bạn trên đám mây AWS. <br> - Tìm hiểu về các khối CIDR và cách lập kế hoạch dải địa chỉ IP. <br> - Định nghĩa và phân biệt giữa public subnet và private subnet.                                                                  | 29/09/2025   | 29/09/2025      | <https://000003.awsstudygroup.com/vi/> |   
| 3   | **Tiếp tục với các thành phần mạng của VPC:** <br> - Nghiên cứu Route Tables và cách chúng điều khiển luồng traffic giữa các subnet và ra mạng bên ngoài. <br> - Tìm hiểu về Internet Gateway (IGW) để truy cập internet công cộng. <br> - Hiểu về Network Address Translation (NAT) Gateway để truy cập internet ra ngoài từ các private subnet.                                   | 30/09/2025 | 30/09/2025      | <https://000003.awsstudygroup.com/vi/> |
| 4   | **Tìm hiểu về bảo mật và kết nối VPC:** <br> - Hiểu Security Groups như một tường lửa có trạng thái ở cấp độ instance. <br> - Tìm hiểu về Network ACLs như một tường lửa không trạng thái ở cấp độ subnet. <br> - Giới thiệu về VPC Peering để kết nối các VPC với nhau. <br> - Bắt đầu khám phá các khái niệm về AWS Site-to-Site VPN cho kết nối hybrid cloud.                                                                      | 01/10/2025 | 01/10/2025      | <https://000003.awsstudygroup.com/vi/> |
| 5   | - Giới thiệu về VPC Peering để kết nối các VPC với nhau. <br> - Bắt đầu khám phá các khái niệm về AWS Site-to-Site VPN cho kết nối hybrid cloud.             | 02/10/2025   | 02/10/2025      | <https://000003.awsstudygroup.com/> |
| 6   |  **Tham dự và hoàn thành Workshop GenAI Builder:**  <br> - Ghi chép lại những điểm chính và kỹ năng mới thu được liên quan đến các dịch vụ AI của AWS.                                                                                                           | 03/10/2025 | 03/10/2025  |


### Kết quả đạt được tuần 4:

* **Xây dựng được hiểu biết nền tảng về Amazon VPC**, bao gồm vai trò của nó như một mạng ảo biệt lập và mục đích của các khối CIDR để lập kế hoạch IP.
* **Nắm vững các thành phần VPC chính**: Học cách tạo và cấu hình public subnet (có route đến IGW) và private subnet (có route đến NAT Gateway), và hiểu cách Route Tables điều hướng traffic.
* **Tiếp thu kiến thức về bảo mật VPC**: Phân biệt được Security Groups (bảo vệ ở cấp instance, có trạng thái) và Network ACLs (bộ quy tắc ở cấp subnet, không trạng thái).
* **Khám phá khả năng kết nối nâng cao**: Được giới thiệu về VPC Peering cho kết nối nội bộ AWS và bắt đầu tìm hiểu về AWS Site-to-Site VPN để kết nối với mạng on-premise.
* **Chuẩn bị thành công và tham gia Workshop GenAI Builder**, có được kinh nghiệm thực hành với các công cụ generative AI của AWS và hiểu cách tích hợp chúng vào kiến trúc đám mây.
* **Phát triển kỹ năng ban đầu trong thiết kế mạng đám mây**, cho phép lập kế hoạch cho một kiến trúc ứng dụng đa tầng cơ bản, an toàn bằng cách sử dụng các khái niệm VPC đã học trong tuần này.