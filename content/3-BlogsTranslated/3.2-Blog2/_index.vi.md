---
title: "Blog 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Nhận thông tin chi tiết về tuân thủ trong môi trường AWS của bạn bằng Amazon Q Business

Tác giả:  Jegan Sundarapandian, Neeharika Naidu và Snehal Nahar trên22 THÁNG 5 NĂM 2025 trong Amazon Q Business , AWS Config , Giải pháp khách hàng , Quản lý & Quản trị , Công cụ quản lý , Hướng dẫn kỹ thuật Liên kết cố định 

Các tổ chức doanh nghiệp quản lý nhiều tài khoản AWS phải đối mặt với sự phức tạp khi cơ sở hạ tầng đám mây của họ mở rộng. Sự tăng trưởng theo cấp số nhân về tài nguyên, cùng với các yêu cầu cấu hình đa dạng trên các đơn vị kinh doanh khác nhau, tạo ra những thách thức đáng kể trong việc duy trì giám sát hiệu quả môi trường AWS.

AWS Config là một dịch vụ liên tục đánh giá, kiểm tra và thẩm định cấu hình và mối quan hệ giữa các tài nguyên của bạn trên AWS, tại chỗ và trên các đám mây khác. Dữ liệu AWS Config được lưu trữ trong các bucket Amazon S3 an toàn .
Khi kết hợp với khả năng xử lý ngôn ngữ tự nhiên của Amazon Q Business , dữ liệu AWS Config này có thể được sử dụng để phân tích cấu hình tài nguyên AWS, thu thập thông tin chi tiết và thực hiện hành động.

Trong bài đăng trên blog này, chúng tôi sẽ trình bày cách các nhóm bảo mật và tuân thủ giờ đây có thể sử dụng AWS Config và Amazon Q Business để có được cái nhìn sâu sắc về môi trường AWS của họ. Bằng cách tận dụng các truy vấn ngôn ngữ tự nhiên, các nhóm có thể truy cập thông tin chi tiết quan trọng về tuân thủ và chủ động xác định các rủi ro tiềm ẩn cũng như xác định các hành động khắc phục.

# Tổng quan về giải pháp
Giải pháp của chúng tôi giải quyết thách thức này bằng cách tích hợp AWS Config, Amazon S3 và Amazon Q Business để tạo ra một giao diện sử dụng ngôn ngữ tự nhiên cho việc truy vấn cấu hình tài nguyên AWS. Cách thức hoạt động như sau:

1. Trích xuất dữ liệu AWS Config liên quan: Giải pháp của chúng tôi định kỳ trích xuất dữ liệu AWS Config từ một Amazon S3 Bucket trung tâm và sao chép dữ liệu này vào một S3 Bucket an toàn trong một tài khoản kiểm toán. Việc này được thực hiện cho một danh sách các Tài khoản và Khu vực AWS đã chọn.
2. Xử lý dữ liệu bằng Amazon Q Business: Sau đó, chúng tôi sẽ cấu hình Q Business để phân tích dữ liệu AWS Config được lưu trữ trong bucket S3 an toàn trong tài khoản kiểm tra của bạn và tạo cơ sở kiến ​​thức có thể truy vấn bằng ngôn ngữ tự nhiên.
3. Truy vấn Cơ sở tri thức bằng ngôn ngữ tự nhiên: Với cơ sở tri thức do Q Business tạo ra, người dùng giờ đây có thể đặt câu hỏi bằng ngôn ngữ tự nhiên về môi trường AWS của họ, chẳng hạn như "Những phiên bản EC2 nào đang chạy trong tài khoản us-west-2 của tôi?" hoặc "Cấu hình cơ sở dữ liệu RDS của tôi trong môi trường phát triển là gì?". Sau đó, Q Business sẽ cung cấp thông tin liên quan từ dữ liệu AWS Config cơ bản.
   
Bằng cách triển khai giải pháp này, các thành viên trong nhóm bảo mật của bạn có thể dễ dàng truy cập và hiểu cấu hình tài nguyên AWS, trạng thái tuân thủ của chúng và xác định các hành động khắc phục bằng các câu hỏi ngôn ngữ tự nhiên đơn giản.

# Kiến trúc giải pháp


<p align="center">
  <img src="/images/3-BlogsTranslated/3.2-Blog2/blog3.2_1.png" alt="Hình 1" />
  <br/>
  <strong style="font-size: 18px;">Hình 1: Sơ đồ kiến ​​trúc giải pháp</strong>
</p>


# Điều kiện tiên quyết
Trong môi trường đa tài khoản AWS, tài khoản kiểm tra và tài khoản lưu trữ nhật ký là các tài khoản AWS dùng chung được các nhóm bảo mật và tuân thủ sử dụng. Tài khoản lưu trữ nhật ký chứa một kho lưu trữ Amazon S3 trung tâm để lưu trữ bản sao của tất cả nhật ký, bao gồm cả tệp nhật ký AWS CloudTrail và AWS Config cho tất cả các tài khoản khác trong Tổ chức AWS của bạn.

Tài khoản kiểm toán nên được giới hạn cho các nhóm bảo mật và tuân thủ, với vai trò kiểm toán viên (chỉ đọc) và quản trị viên (truy cập đầy đủ) trên tất cả các tài khoản trong Tổ chức AWS. Các vai trò này được thiết kế để các nhóm bảo mật và tuân thủ sử dụng để thực hiện kiểm toán thông qua các cơ chế của AWS.

Giải pháp này được thiết kế để triển khai trong một tài khoản kiểm toán, có thể cung cấp một môi trường riêng biệt để lưu trữ dữ liệu AWS Config cần thiết cho Amazon Q Business. Chúng tôi khuyến nghị bạn triển khai giải pháp này trong một tài khoản kiểm toán riêng biệt với tài khoản lưu trữ nhật ký trung tâm.

Trước khi đi sâu vào giải pháp, chúng ta hãy xem xét các điều kiện tiên quyết cần có để bắt đầu:

1. Quyền truy cập AWS Identity and Access Management (IAM) cần thiết trong kho lưu trữ nhật ký và tài khoản kiểm tra nơi giải pháp này sẽ được triển khai.
2. Truy cập vào Bảng điều khiển AWS của tài khoản kiểm toán.
3. AWS CLI để triển khai các hiện vật cần thiết bằng AWS CloudFormation
4. SAM CLI – Cài đặt SAM CLI. Giao diện dòng lệnh mô hình ứng dụng không máy chủ (SAM CLI) là phần mở rộng của AWS CLI, bổ sung chức năng xây dựng và kiểm thử ứng dụng Lambda.
5. Thiết lập Q Business với các quyền cần thiết .
6. Cấu hình phiên bản IAM Identity Center cho ứng dụng Amazon Q Business để cho phép quản lý quyền truy cập của người dùng cuối vào ứng dụng Amazon Q Business của bạn.
   
# Cách xây dựng và triển khai giải pháp

Bước 1: Triển khai vai trò chỉ đọc S3 trong tài khoản nhật ký lưu trữ bucket S3 trung tâm cho AWS Config.
Sử dụng AWS CLI trên tài khoản nhật ký, tạo vai trò ConfigDataReadRole với chính sách tin cậy.
```yaml
aws iam create-role \
    --role-name ConfigDataReadRole \
    --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<REPLACE WITH AUDIT ACCOUNT ID>:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}'
```

Sau đó đính kèm chính sách được quản lý AmazonS3ReadOnlyAccess.

aws iam attach-role-policy \
    --role-name ConfigDataReadRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

  

Bước 2: Triển khai giải pháp trong tài khoản kiểm toán

Sử dụng AWS CLI trên tài khoản kiểm tra, triển khai giải pháp.

git clone https://github.com/aws-samples/sample-Compliance-Insights-Using-Amazon-Q
cd sample-Compliance-Insights-using-Amazon-Q
sam deploy —guided —capabilities CAPABILITY_NAMED_IAM

Tham số triển khai SAM:
Tên ngăn xếp: Tên của ngăn xếp AWS CloudFormation đã triển khai.
Ví dụ:Stack Name : config-copy-stack

Khu vực AWS: Khu vực AWS nơi ngăn xếp sẽ được triển khai.
Ví dụ:AWS Region : us-east-1

Tham số SourceBucketArn: Bucket ARN cho Central AWS Config S3 Bucket trong tài khoản Nhật ký.
Ví dụ:Parameter SourceBucketArn: arn:aws:s3:::aws-controltower-logs-123456789101-us-east-1

Tham số AccountList: Cung cấp danh sách các số tài khoản AWS được phân tách bằng dấu phẩy mà từ đó dữ liệu AWS Config sẽ được trích xuất và sao chép vào tài khoản kiểm tra.
Ví dụ:Parameter AccountList: 012345678910,123456789101

Tham số RegionList: Cung cấp danh sách các vùng AWS được phân tách bằng dấu phẩy mà từ đó dữ liệu AWS Config sẽ được trích xuất vào tài khoản kiểm tra.
Ví dụ:Parameter RegionList : us-east-1,eu-west-1

Tham số SourceAccountId: Cung cấp số tài khoản AWS của tài khoản lưu trữ nhật ký AWS trong tổ chức của bạn, nơi chứa nguồn nhật ký AWS Config.
Ví dụ:Parameter SourceAccountId : 123456789101

Các đầu vào SAM CLI khác
#Shows you resources changes to be deployed and require a 'Y' to initiate deploy
Confirm changes before deploy [Y/n]: Y

#SAM needs permission to be able to create roles to connect to the resources in your template
Allow SAM CLI IAM role creation [Y/n]: Y

#Preserves the state of previously provisioned resources when an operation fails
Disable rollback [Y/n]: Y

Save arguments to configuration file [Y/n]: Y
SAM configuration file [samconfig.toml]: samconfig.toml
SAM configuration environment [default]: default

Sau khi quá trình triển khai SAM hoàn tất, hãy chạy lệnh SAM sau và ghi lại OutputValue của OutputKey – DestinationBucketName. Tên S3 Bucket này sẽ được sử dụng làm tham số khi triển khai Mẫu Q Business CloudFormation ở Bước 3.


sam list stack-outputs --stack-name config-copy-stack --output table

Bước 3: Triển khai và cấu hình Q Business
Amazon Q Business là một dịch vụ cho phép bạn xây dựng các ứng dụng tìm kiếm và phân tích thông minh dựa trên dữ liệu doanh nghiệp. Trong ví dụ này, chúng tôi sẽ sử dụng Q Business để phân tích dữ liệu liên quan đến tuân thủ được lưu trữ trong bucket S3.

Mẫu CloudFormation mà chúng tôi sẽ sử dụng cung cấp các tài nguyên sau:
1. Ứng dụng Q Business: Ứng dụng chính sẽ lưu trữ trải nghiệm phân tích tuân thủ của chúng tôi.
2. Chỉ mục Q Business: Chỉ mục mà ứng dụng sẽ sử dụng để nhanh chóng tìm kiếm và truy xuất dữ liệu có liên quan.
3. Bộ truy xuất Q Business: Kết nối ứng dụng với chỉ mục, cho phép chức năng tìm kiếm và truy xuất.
4. Nguồn dữ liệu Q Business: Cấu hình bucket S3 làm nguồn dữ liệu cho ứng dụng. Nguồn dữ liệu S3 sẽ được thiết lập để đồng bộ hóa tác vụ chạy lúc 8 giờ sáng thứ Hai hàng tuần theo múi giờ UTC.
5. iao diện web Q Business: Cung cấp giao diện web tùy chỉnh để tương tác với ứng dụng Q Business.
6. Quyền và chính sách IAM: Cấp các quyền cần thiết cho tài nguyên Q Business để truy cập vào bucket S3 và thực hiện các hành động trong ứng dụng.
7. 
Hãy cùng tìm hiểu các bước triển khai ứng dụng Q Business này.

**Triển khai mẫu CloudFormation**

1. Đăng nhập vào AWS Console và điều hướng đến CloudFormation.
2. Nhấp vào “Tạo ngăn xếp” và chọn “Với tài nguyên mới (chuẩn)”.
3. Tải xuống mẫu CloudFormation từ Github để triển khai Q Business.
4. Chọn “Tải lên tệp mẫu” và chọn mẫu CloudFormation mà bạn đã được cung cấp.
5. Điền các tham số bắt buộc:
QBusinessApplicationName: Tên ứng dụng Amazon Q Business của bạn.
S3BucketName: Tên của bucket S3 chứa dữ liệu tuân thủ đã được thu thập trước đó ở Bước 3.
UseIDC: Đặt thành "true" nếu bạn muốn sử dụng AWS IAM Identity Center để xác thực người dùng.
UseIdP: Đặt thành "true" nếu bạn muốn sử dụng Nhà cung cấp Danh tính (IdP) bên ngoài để xác thực người dùng.
IdentityCenterArn: ARN của phiên bản IAM Identity Center của bạn (bắt buộc nếu UseIDC là "true").
ExternalIdPArn: ARN của IdP bên ngoài của bạn (bắt buộc nếu UseIdP là "true").
6. Xem lại mẫu và các thông số của nó, sau đó nhấp vào “Tiếp theo” để tiếp tục.
7. Ở trang tiếp theo, hãy cấu hình bất kỳ tùy chọn ngăn xếp bổ sung nào nếu cần, sau đó nhấp vào “Tiếp theo”.
8. Xem lại chi tiết ngăn xếp và xác nhận bất kỳ khả năng cần thiết nào, sau đó nhấp vào "Tạo ngăn xếp" để triển khai các tài nguyên
   
Việc triển khai CloudFormation sẽ mất vài phút để hoàn tất. Khi ngăn xếp ở trạng thái "CREATE_COMPLETE", bạn có thể chuyển sang các bước tiếp theo.

**Chỉ định người dùng và nhóm**

Sau khi triển khai CloudFormation, bạn sẽ cần phải chỉ định người dùng và nhóm theo cách thủ công cho ứng dụng Q Business. Cách thực hiện như sau:

1. Trong AWS Console , hãy điều hướng đến Amazon Q Business.
2. Nhấp vào tên ứng dụng được tạo bởi mẫu.
3. Trong phần “Quyền truy cập của người dùng”, chọn “Quản lý quyền truy cập của người dùng”.
4. Chọn “Thêm nhóm và người dùng”, sau đó chọn tùy chọn “Thêm và chỉ định người dùng mới” hoặc “Chỉ định người dùng và nhóm hiện có”.
5. Cung cấp tên nhóm/người dùng.
6. Chọn nhóm và chọn “Chỉ định”.
7. Chọn gói đăng ký phù hợp (ví dụ: Q Business Pro).
8. Chọn “Xác nhận” để hoàn thành bài tập.


**Truy cập Trải nghiệm Web Doanh nghiệp Q**

Giờ đây, bạn có thể truy cập trải nghiệm web tùy chỉnh cho ứng dụng Q Business của mình bằng URL mà mẫu CloudFormation xuất ra. Trải nghiệm web cung cấp giao diện thân thiện với người dùng để tìm kiếm, duyệt và tương tác với dữ liệu tuân thủ được lưu trữ trong bucket S3 của bạn.

# Kiểm tra giải pháp

Để kiểm tra giải pháp, chúng tôi sẽ đăng nhập vào ứng dụng Amazon Q Business bằng thông tin đăng nhập và tương tác bằng các câu hỏi sau:

Để kiểm tra giải pháp, chúng tôi sẽ đăng nhập vào Ứng dụng Amazon Q Business bằng thông tin đăng nhập của những người dùng đã được chỉ định cho ứng dụng Q Business trước đó. Hãy tương tác bằng các câu hỏi sau:

1. Liệt kê tất cả các thùng S3 không tuân thủ
2. Khi nào thì nhóm "tên của nhóm" được tạo ra và khi nào nó trở nên không tuân thủ
3. Làm thế nào chúng ta có thể khắc phục lỗi bucket không tuân thủ "tên của bucket"
   

<p align="center">
  <img src="/images/3-BlogsTranslated/3.2-Blog2/blog3.2_2.png" alt="Hình 2" />
  <br/>
  <strong style="font-size: 18px;">Hình 2: Để liệt kê các thùng S3 không tuân thủ</strong>
</p>
   
<p align="center">
  <img src="/images/3-BlogsTranslated/3.2-Blog2/blog3.2_3.png" alt="Hình 3" />
  <br/>
  <strong style="font-size: 18px;">Hình 3: Để tìm hiểu khi nào thùng S3 không tuân thủ</strong>
</p>


<p align="center">
  <img src="/images/3-BlogsTranslated/3.2-Blog2/blog3.2_4.png" alt="Hình 4" />
  <br/>
  <strong style="font-size: 18px;">Hình 4: Để khắc phục thùng S3 không tuân thủ</strong>
</p>




# Dọn dẹp

1. Xóa ứng dụng config-copy-stack mà bạn đã tạo, sử dụng SAM CLI.
sam delete
2. Xóa vai trò chỉ đọc S3 trong tài khoản nhật ký.
aws iam delete-role \
--role-name ConfigDataReadRole
3. Xóa Người dùng và Nhóm trong Tài khoản Kiểm tra.
4. Xác định vị trí và xóa ngăn xếp được tạo bởi triển khai mẫu CloudFormation
   
# Phần kết luận

Bằng cách tích hợp AWS Config và Amazon Q Business, bạn có thể khai thác sức mạnh của xử lý ngôn ngữ tự nhiên để có được những hiểu biết sâu sắc về môi trường AWS của mình. Giải pháp này cho phép các thành viên trong nhóm của bạn dễ dàng truy cập và hiểu cấu hình tài nguyên AWS của họ. Khi cơ sở hạ tầng đám mây của bạn phát triển, giải pháp này có thể giúp bạn luôn nắm bắt được cấu hình tài nguyên và đưa ra quyết định sáng suốt để tối ưu hóa môi trường AWS. Triển khai tích hợp AWS Config và Amazon Q Business này để có được tầm nhìn sâu sắc hơn và thông tin chi tiết về tuân thủ trên toàn bộ môi trường AWS của bạn.

