---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Bốn cách cấp quyền truy cập liên tài khoản trong AWS

Tác giá: Anshu Bathla và Jay Goradia trên24 tháng 2 năm 2025 trong AWS Identity and Access Management (IAM) , Thực hành tốt nhất , Trung cấp (200) , Bảo mật, Danh tính và Tuân thủ , Hướng dẫn kỹ thuật Liên kết cố định 

Khi môi trường Amazon Web Services (AWS) của bạn phát triển, bạn có thể cần cấp quyền truy cập tài nguyên cho nhiều tài khoản. Điều này có thể xuất phát từ nhiều lý do, chẳng hạn như cho phép vận hành tập trung trên nhiều tài khoản AWS, chia sẻ tài nguyên giữa các nhóm hoặc dự án trong tổ chức, hoặc tích hợp với các dịch vụ của bên thứ ba. Tuy nhiên, việc cấp quyền truy cập cho nhiều tài khoản đòi hỏi bạn phải cân nhắc kỹ lưỡng các yêu cầu về bảo mật, tính khả dụng và khả năng quản lý.

Trong bài đăng trên blog này, chúng tôi sẽ khám phá bốn cách khác nhau để cấp quyền truy cập liên tài khoản bằng các chính sách dựa trên tài nguyên. Mỗi phương pháp đều có những đánh đổi riêng, và lựa chọn tốt nhất phụ thuộc vào yêu cầu và trường hợp sử dụng cụ thể của bạn.

**Đánh giá các kỹ thuật khác nhau để cấp quyền truy cập liên tài khoản**

Quyền truy cập liên tài khoản được cấp thông qua các chính sách dựa trên danh tính và chính sách dựa trên tài nguyên trong AWS Identity and Access Management (IAM) . Chính sách dựa trên danh tính gắn với vai trò IAM, trong khi chính sách dựa trên tài nguyên gắn với các tài nguyên như thùng Amazon Simple Storage Service (Amazon S3) và khóa AWS Key Management Service (AWS KMS) . Chính sách dựa trên tài nguyên yêu cầu bạn chỉ định một hoặc nhiều chủ thể (người dùng hoặc vai trò IAM) được phép truy cập tài nguyên.

Lựa chọn của bạn về cách xác định nguyên tắc trong chính sách dựa trên tài nguyên sẽ ảnh hưởng đến một số khía cạnh của cả tính bảo mật và tính khả dụng của giải pháp. Bài viết này tập trung vào việc hiểu rõ tác động này và đưa ra những đánh đổi phù hợp cho trường hợp sử dụng của bạn.

**Một kịch bản ví dụ**

Hãy tưởng tượng bạn có một thùng S3 trong tài khoản AWS (Tài khoản A) cần được truy cập bởi các chủ thể khác nhau trong một tài khoản AWS khác (Tài khoản B). Trong trường hợp này, chúng tôi giả định rằng các chủ thể trong Tài khoản B có quyền truy cập cần thiết vào S3 trong các chính sách dựa trên danh tính của họ, và chúng tôi sẽ tập trung vào việc tạo các chính sách dựa trên tài nguyên trong Tài khoản A. Mặc dù các phương pháp được giải thích ở đây sử dụng Amazon S3, nhưng các khái niệm được thảo luận áp dụng cho tất cả các dịch vụ AWS hỗ trợ chính sách dựa trên tài nguyên . Trong các phần sau, chúng tôi sẽ hướng dẫn bốn cách khác nhau để cấp quyền truy cập liên tài khoản trong trường hợp này và thảo luận về những đánh đổi của từng phương pháp.

**Phương pháp 1: Cấp quyền truy cập vào vai trò IAM cụ thể bằng cách sử dụng phần tử Chính của chính sách dựa trên tài nguyên**

Trong ví dụ này, bạn sử dụng chính sách nhóm S3 để cấp quyền truy cập cho vai trò IAM cụ thể ( RoleFromAccountB) trong Tài khoản B bằng cách chỉ định Tên tài nguyên Amazon (ARN) của vai trò IAM trong Principalphần tử của chính sách trong Tài khoản A.
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRoleInThePrincipalElement",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:role/RoleFromAccountB"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}

Sử dụng chính sách nhóm này, nếu ai đó trong Tài khoản B xóa hoặc tạo lại vai trò ( RoleFromAccountB), thì vai trò đó sẽ không thể truy cập vào amzn-s3-demo-bucket-account-anhóm nữa, ngay cả khi vai trò đó được tạo lại với cùng tên. Lý do là khi bạn lưu chính sách này, ARN của vai trò sẽ được ánh xạ đến ID duy nhất của vai trò , trông giống như sau: AROADBQP57FF2AEXAMPLE. Bạn sẽ thấy mã định danh vai trò trong Principalphần tử của các chính sách dựa trên tài nguyên nếu bạn xem chúng sau khi xóa vai trò mà chúng đã tham chiếu.

Hành vi này là cố ý. Chính sách dựa trên tài nguyên chỉ cho phép phiên bản cụ thể của vai trò mà bạn đặt làm chính tại thời điểm tạo chính sách. Điều này giúp ngăn chặn việc truy cập ngoài ý muốn vào tài nguyên của bạn nếu bạn xóa một vai trò nhưng quên cập nhật chính sách dựa trên tài nguyên để xóa vai trò đó. Hành vi này cũng có thể gây ra rủi ro về tính khả dụng vì vai trò ( RoleFromAccountB) sẽ có ID duy nhất mới khi được tạo lại và sẽ không còn quyền truy cập vào nhóm. Vai trò có thể được tạo lại vì một số lý do, bao gồm cả việc vô tình khi bạn sử dụng các công cụ như cơ sở hạ tầng dưới dạng mã.

Bạn có thể cân nhắc lựa chọn phương pháp này nếu:

- Bạn sở hữu các vai trò trong cả Tài khoản A và Tài khoản B và có thể kiểm soát việc tạo và xóa các vai trò này.
- Bạn muốn chính sách dựa trên tài nguyên trong Tài khoản A ngừng cấp quyền truy cập khi vai trò được chỉ định ( RoleFromAccountB) bị xóa.
- Bạn ưu tiên kiểm soát quyền truy cập chi tiết hơn các mối lo ngại tiềm ẩn về tính khả dụng nếu vai trò ( RoleFromAccountB) bị xóa.
  
**Phương pháp 2: Cấp quyền truy cập vào tài khoản bằng cách sử dụng phần tử Chính của chính sách dựa trên tài nguyên**

Trong ví dụ này, bạn cấp quyền truy cập cho một tài khoản cụ thể trong Principalphần tử của chính sách dựa trên tài nguyên. Chính sách dựa trên tài nguyên này của Tài khoản A cho phép bất kỳ người dùng hoặc vai trò nào từ Tài khoản B cũng có chính sách dựa trên danh tính cấp cho họ quyền đọc các đối tượng.

Lưu ý: Bạn có thể sử dụng một trong hai "Principal": {"AWS": "111122223333"}hoặc "Principal": {"AWS": "arn:aws:iam::111122223333:root"}trong Principalphần tử. Chúng tương đương nhau và ARN dạng dài không đại diện cho người dùng gốc.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInThePrincipalElement",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*"
    }
  ]
}

Chính sách dựa trên tài nguyên này giúp tránh vấn đề về khả năng truy cập tiềm ẩn đã được thảo luận trong Phương pháp 1. Nếu một vai trò trong Tài khoản B cần có quyền truy cập vào thùng được tạo lại, vai trò đó vẫn sẽ có quyền truy cập sau khi vai trò đó được tạo lại. Điều này là do bạn không chỉ định vai trò trong Principalphần tử—thay vào đó, bạn chỉ định một tài khoản. Nếu sử dụng Phương pháp 2, bạn phải thoải mái ủy quyền các quyết định kiểm soát truy cập cho chủ sở hữu của tài khoản đó.

Phương pháp này ủy quyền rõ ràng các quyết định kiểm soát truy cập cho IAM trong tài khoản còn lại (Tài khoản B). Các chủ thể trong Tài khoản B có quyền truy cập vào nhóm này nếu được chính sách dựa trên danh tính của họ cho phép.
- Bạn có thể cân nhắc lựa chọn phương pháp này nếu:
- Bạn cần cấp quyền truy cập cho nhiều chủ thể trong Tài khoản B.
- Bạn muốn ủy quyền quyết định truy cập trong tài khoản nơi chủ sở hữu tồn tại (Tài khoản B).
Bạn ưu tiên tính dễ quản lý và khả dụng hơn là kiểm soát truy cập chi tiết.

**Phương pháp 3: Cấp quyền truy cập vào vai trò IAM cụ thể bằng điều kiện aws:PrincipalArn**

Phương pháp này mở rộng Phương pháp 2 và thêm một điều kiện chỉ cấp quyền truy cập cho một vai trò IAM cụ thể. Tương tự như Phương pháp 2, bạn sử dụng số tài khoản làm giá trị của Principalphần tử, nhưng cũng sử dụng aws:PrincipalArnkhóa điều kiện để giới hạn quyền truy cập vào một chủ thể cụ thể trong Tài khoản B.

Khóa điều kiện aws:PrincipalArn là khóa điều kiện toàn cục so sánh ARN của principal đã tạo yêu cầu với ARN mà bạn chỉ định trong chính sách. Đối với các vai trò IAM, ngữ cảnh yêu cầu sẽ trả về ARN của vai trò đó, chứ không phải ARN của người dùng đã đảm nhận vai trò đó.
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountInPrincipalAndRoleInPrincipalArn",
      "Principal": {
        "AWS": "111122223333"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "ArnEquals": {
          "aws:PrincipalArn": "arn:aws:iam::111122223333:role/RoleFromAccountB"
        }
      }
    }
  ]
}

Chính sách này đi kèm với các lợi ích về tính khả dụng tương tự như chính sách trong Phương pháp 2: quyền truy cập vào tài nguyên này sẽ vẫn tồn tại sau khi tạo lại vai trò. Điều này là do vai trò chỉ được dịch sang mã định danh duy nhất khi được sử dụng trong Principalphần tử. Nó không được dịch sang mã định danh duy nhất khi được sử dụng trong điều kiện. Nếu vai trò ( RoleFromAccountB) trong Tài khoản B được tạo lại, dù vô tình hay cố ý, chính sách sẽ tiếp tục cấp quyền truy cập vì vai trò này khớp với ARN vai trò được chỉ định trong khóa điều kiện của chính sách dựa trên tài nguyên trong Tài khoản A. Do đó, Phương pháp 3 cung cấp một cách tiếp cận cân bằng giữa tính khả dụng và bảo mật.

Bạn có thể cân nhắc lựa chọn phương pháp này nếu:

- Bạn có thể yên tâm rằng chính sách này sẽ tiếp tục cấp quyền truy cập vào vai trò được chỉ định trong aws:PrincipalArnkhóa điều kiện nếu vai trò đó ( RoleFromAccountB) được tạo lại.
- Bạn không sở hữu Tài khoản B mà bạn cấp quyền truy cập và không kiểm soát được thời điểm vai trò đó có thể được tạo lại.
- Bạn muốn cân bằng giữa tính khả dụng và tính bảo mật.
  
**Phương pháp 4: Cấp quyền truy cập cho toàn bộ tổ chức AWS Organizations**

Phương pháp này tập trung vào một trường hợp sử dụng khác và không phải là giải pháp thay thế cho các phương pháp đã liệt kê ở trên. Hãy sử dụng phương pháp này nếu bạn có một tài nguyên (trong ví dụ này là một thùng S3) mà bạn muốn chia sẻ với toàn bộ tổ chức, nhưng không muốn chia sẻ với bất kỳ ai bên ngoài.
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessToAnEntireOrganization",
      "Principal": {
        "AWS": "*"
      },
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-demo-bucket-account-a/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgId": "o-12345"
        },
        "StringNotEquals": {
          "aws:PrincipalAccount": "${aws:ResourceAccount}"
        }
      }
    }
  ]
}

Không có cách nào để chỉ định một tổ chức bằng cách sử dụng Principalphần tử của chính sách dựa trên tài nguyên, vì vậy bạn phải sử dụng khóa điều kiện aws:PrincipalOrgId để hạn chế quyền truy cập vào một tổ chức cụ thể. Trong chính sách này, bạn chỉ định một ký tự đại diện trong Principalphần tử, cho biết bất kỳ ai cũng có thể truy cập vào bucket. Sau đó, điều kiện sẽ giảm "bất kỳ ai" xuống chỉ còn những chủ tài khoản AWS thuộc về tổ chức được chỉ định và có chính sách dựa trên danh tính cho phép họ truy cập.

Sau đó, bạn thêm một khối điều kiện bổ sung để so sánh aws:PrincipalAccountkhóa điều kiện với khóa điều kiện aws:ResourceAccount bằng cách sử dụng biến chính sách . Khối điều kiện bổ sung này là tùy chọn và loại trừ tài khoản sở hữu bucket (Tài khoản A) khỏi câu lệnh allow. Lý do sử dụng khối điều kiện bổ sung này là để các chủ thể trong Tài khoản A vẫn cần câu lệnh allow trong chính sách dựa trên danh tính của họ để truy cập bucket này. Nếu bạn chọn loại trừ phép aws:PrincipalAccountso sánh này, các chủ thể trong Tài khoản A sẽ được cấp quyền truy cập vào bucket mà không cần câu lệnh allow rõ ràng trong chính sách dựa trên danh tính của họ. Logic đánh giá chính sách chỉ yêu cầu chính sách dựa trên danh tính hoặc chính sách dựa trên tài nguyên (nhưng không phải cả hai) để cho phép yêu cầu khi chủ thể và tài nguyên nằm trong cùng một tài khoản.

Bạn có thể cân nhắc lựa chọn phương pháp này nếu:

Bạn có một nguồn tài nguyên chung mà toàn bộ tổ chức của bạn có thể truy cập được.

# Phần kết luận
Việc lựa chọn phương pháp cấp quyền truy cập liên tài khoản đòi hỏi bạn phải cân nhắc kỹ lưỡng các yêu cầu và trường hợp sử dụng của mình. Mỗi phương pháp trong bốn phương pháp được thảo luận trong bài đăng trên blog này đều có những ưu điểm và nhược điểm riêng. Bằng cách hiểu rõ các phương pháp này và ý nghĩa của chúng, bạn có thể quyết định phương pháp phù hợp nhất để cấp quyền truy cập liên tài khoản vào tài nguyên AWS của mình. Hãy nhớ thường xuyên xem xét và kiểm tra các chính sách dựa trên tài nguyên của bạn để đảm bảo chúng phù hợp với các yêu cầu về bảo mật và quyền truy cập của bạn.

