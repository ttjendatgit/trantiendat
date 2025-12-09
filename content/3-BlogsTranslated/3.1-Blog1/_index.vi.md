---
title: "Blog 1"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Nâng cao bảo mật viễn thông với AWS

Tác giả: Kal Krishnan , Danny Cortegaca và Ruben Merz trên07 THÁNG 2 NĂM 2025 trong Thực hành tốt nhất , Trung cấp (200) , Bảo mật, Nhận dạng & Tuân thủ , Lãnh đạo tư tưởng Liên kết cố định 

# Triển khai hướng dẫn tăng cường khả năng hiển thị và củng cố của CISA cho cơ sở hạ tầng truyền thông
Để ứng phó với các sự cố an ninh mạng gần đây do các tác nhân từ Cộng hòa Nhân dân Trung Hoa gây ra, một số cơ quan an ninh mạng do Cơ quan An ninh Mạng và Cơ sở hạ tầng Hoa Kỳ (CISA) dẫn đầu đã cùng nhau ban hành hướng dẫn toàn diện về bảo mật cơ sở hạ tầng truyền thông. Khi các nhà cung cấp dịch vụ truyền thông (CSP) di chuyển khối lượng công việc của mình lên đám mây, họ phải thực hiện các bước để triển khai hiệu quả các biện pháp bảo mật này trong môi trường đám mây.

Bài đăng trên blog này mô tả cách CSP có thể sử dụng các tính năng của Amazon Web Services (AWS) để triển khai hướng dẫn này trong khi vẫn được hưởng lợi từ những lợi thế của đám mây.
Hướng dẫn tập trung vào hai lĩnh vực chính:

- Tăng cường khả năng hiển thị: Cho phép các nhóm bảo mật giám sát, phát hiện và ứng phó với các mối đe dọa tiềm ẩn thông qua khả năng hiển thị toàn diện vào các tài sản kỹ thuật số.
  
- Tăng cường hệ thống và thiết bị: Triển khai các biện pháp kiểm soát và cấu hình bảo mật mạnh mẽ để giảm thiểu lỗ hổng và giúp ngăn chặn truy cập trái phép.


 # Tổng quan về các khái niệm cơ bản về đám mây
Trước khi tìm hiểu hướng dẫn cụ thể trong bài viết này, điều quan trọng là phải hiểu các khuyến nghị bảo mật áp dụng khác nhau như thế nào cho môi trường đám mây công cộng so với cơ sở hạ tầng riêng. Một xu hướng phổ biến trong ngành viễn thông là coi đám mây công cộng chỉ đơn thuần là phiên bản mở rộng của đám mây riêng. Điều này có thể dẫn đến hiểu lầm về khả năng bảo mật và việc sử dụng không hiệu quả các tính năng bảo mật gốc của đám mây công cộng.

Sự khác biệt cơ bản nằm ở cách kiến ​​trúc đám mây công cộng—đặc biệt dành cho nhiều đối tượng thuê, với khả năng cô lập đối tượng thuê mạnh mẽ làm nền tảng cho thiết kế của chúng.

Trong AWS, tài nguyên ảo được cô lập theo mặc định và yêu cầu cấu hình rõ ràng để kết nối. Ví dụ: khi bạn tạo đám mây riêng ảo (VPC) bằng Amazon VPC , mạng được cô lập về mặt logic này không cho phép lưu lượng truy cập vào hoặc ra cho đến khi các tuyến đường và cổng cụ thể được cấu hình một cách có chủ đích. Tương tự, các thùng Amazon Simple Storage Service (Amazon S3) theo mặc định là riêng tư, yêu cầu cấu hình rõ ràng để cấp quyền truy cập. Sự cô lập này mở rộng đến lõi cơ sở hạ tầng ảo hóa của chúng tôi thông qua AWS Nitro System , cung cấp khả năng cô lập khối lượng công việc chưa từng có—ngay cả các nhà điều hành AWS có đặc quyền cao nhất cũng không có quyền truy cập kỹ thuật vào khối lượng công việc của khách hàng. Hơn nữa, dữ liệu di chuyển giữa các máy ảo chạy Nitro System hoặc trên mạng xương sống toàn cầu của chúng tôi được mã hóa tự động, cung cấp các lớp bảo vệ bổ sung ngoài mã hóa do khách hàng triển khai.

Triết lý bảo mật theo thiết kế và bảo mật theo mặc định này thấm nhuần trong toàn bộ thiết kế và vận hành dịch vụ AWS. Nó không chỉ đơn thuần là một lựa chọn thiết kế—mà còn là một mệnh lệnh kinh doanh được thúc đẩy bởi nhu cầu cấp thiết về khả năng phục hồi hoạt động và niềm tin của khách hàng vào mô hình đám mây công cộng. Cam kết của chúng tôi đối với các nguyên tắc này được thể hiện qua việc chúng tôi tham gia ký kết Cam kết Bảo mật theo Thiết kế của CISA .

Khi khách hàng AWS vận hành trên nền tảng đám mây công cộng, việc hiểu rõ mô hình chia sẻ trách nhiệm là vô cùng quan trọng. Mô hình này phân định rõ ràng trách nhiệm bảo mật: AWS chịu trách nhiệm bảo mật đám mây , trong khi khách hàng chịu trách nhiệm bảo mật trong đám mây . Việc phân chia trách nhiệm này giúp giảm đáng kể gánh nặng vận hành của bạn, bởi vì AWS đảm nhận trách nhiệm bảo mật mọi thứ liên quan và bên trong các dịch vụ đám mây mà họ cung cấp, từ bảo vệ vật lý cho các trung tâm dữ liệu. Nhờ đó, bạn có thể tập trung nguồn lực bảo mật vào những việc quan trọng nhất—bảo vệ ứng dụng và khối lượng công việc—trong khi AWS xử lý phần việc nặng nhọc, không phân biệt của bảo mật cơ sở hạ tầng.

Mô hình chia sẻ trách nhiệm này càng trở nên có lợi hơn khi xét đến tính kinh tế theo quy mô vốn có trong hoạt động đám mây công cộng. Quy mô khổng lồ của AWS cho phép chúng tôi đầu tư nhiều hơn vào việc bảo mật nền tảng so với một doanh nghiệp đơn lẻ có thể đạt được một cách độc lập, tạo ra hiệu ứng nhân lên bảo mật có lợi cho tất cả khách hàng. Một ví dụ thuyết phục về lợi thế về quy mô này là chương trình tình báo mối đe dọa toàn diện của chúng tôi , triển khai các cảm biến honeypot trên toàn bộ mạng lưới toàn cầu của chúng tôi. Các cảm biến này quan sát hơn 100 triệu tương tác và thăm dò mối đe dọa tiềm ẩn hàng ngày. Sử dụng trí tuệ nhân tạo và học máy (AI/ML), chúng tôi phân tích thông tin này và thực hiện các hành động nhanh chóng, thường là tự động để giảm thiểu các mối đe dọa. Chỉ riêng trong nửa đầu năm 2023, chương trình này đã cho phép chúng tôi phân tích nguồn gốc của khoảng 230.000 sự kiện từ chối dịch vụ phân tán (DDoS) Lớp 7. Chúng tôi cũng cung cấp thông tin tình báo này cho khách hàng thông qua các dịch vụ như Amazon GuardDuty , mở rộng lợi ích của quy mô của chúng tôi cho khách hàng.

Quy mô hoạt động của AWS không chỉ cho phép thu thập thông tin tình báo về mối đe dọa một cách mẫu mực mà còn đòi hỏi việc tự động hóa toàn diện các hoạt động bảo mật. Một số tác vụ thường xuyên, chẳng hạn như triển khai tính năng và bản vá, cũng như cập nhật cấu hình, được tự động hóa hoàn toàn thông qua các quy trình triển khai. Tự động hóa còn mang lại lợi ích bổ sung là loại bỏ con người khỏi vòng lặp, do đó giảm thiểu nguy cơ mắc lỗi.

Quy mô của chúng tôi cũng tạo điều kiện thuận lợi cho việc tuân thủ toàn diện các tiêu chuẩn bảo mật trong nhiều ngành và khu vực pháp lý. Sự hiện diện toàn cầu và cơ sở khách hàng đa dạng của chúng tôi đòi hỏi việc tuân thủ các yêu cầu bảo mật nghiêm ngặt nhất trên toàn thế giới. Thông qua Chương trình Tuân thủ AWS , chúng tôi đã đạt được 143 tiêu chuẩn bảo mật và chứng nhận tuân thủ, từ các tiêu chuẩn ISO về bảo mật và quyền riêng tư trên nền tảng đám mây đến các quy định cụ thể của ngành trong lĩnh vực tài chính và chăm sóc sức khỏe, cũng như các chương trình bảo mật của chính phủ. Điều này bao gồm việc xác minh độc lập các tuyên bố của chúng tôi về các đặc tính cô lập của cơ sở hạ tầng ảo hóa Nitro System. Do đó, bạn được hưởng lợi từ việc tuân thủ theo quy mô này, có quyền truy cập vào cơ sở hạ tầng an toàn, được chứng nhận, triển khai các hệ thống bảo mật tiên tiến.

Đây là một số lý do tại sao, trong bài đăng trên blog có tiêu đề Tại sao ưu tiên đám mây không phải là vấn đề bảo mật , Trung tâm An ninh mạng quốc gia của Vương quốc Anh đã kết luận rằng “ việc sử dụng đám mây một cách an toàn nên là mối quan tâm chính của bạn – chứ không phải là bảo mật cơ bản của đám mây công cộng ”.

Mặt khác, đám mây riêng thường nằm trong tầm kiểm soát của một tổ chức duy nhất và được thuê riêng lẻ, cung cấp khả năng cô lập khối lượng công việc tương đối yếu. Tài nguyên ảo trong đám mây riêng thường được mặc định kết nối với nhau khi được tạo, do đó cần các bước cụ thể để tăng cường khả năng cô lập. Các thao tác thủ công, với khả năng xảy ra sai sót cũng như sự tham gia tiềm ẩn của các tác nhân đe dọa, thường vẫn chiếm phần lớn trong quy trình làm việc của đám mây riêng. Chúng hiếm khi phải trải qua mức độ giám sát bảo mật như đám mây công cộng thường xuyên phải chịu. Những điều này, cùng với những khác biệt khác , đồng nghĩa với việc rủi ro bảo mật trong mỗi dịch vụ vốn đã khác nhau, do đó cần có các biện pháp kiểm soát bảo mật và kiến ​​trúc giải pháp riêng biệt để giảm thiểu những rủi ro này.

# Triển khai hướng dẫn tăng cường bảo mật với AWS

Tài nguyên và dữ liệu đám mây của bạn được lưu trữ trong một tài khoản AWS. Tài khoản đóng vai trò như một ranh giới cô lập quản lý danh tính và quyền truy cập. Khi bạn cần chia sẻ tài nguyên và dữ liệu giữa hai tài khoản, bạn phải cho phép rõ ràng quyền truy cập này. Điều này giúp giảm thiểu rủi ro di chuyển ngang giữa các tài khoản.

Việc thiết kế môi trường AWS đúng cách sẽ đặt nền tảng vững chắc giúp bạn đáp ứng hướng dẫn an ninh mạng của CISA. AWS Control Tower , hợp tác với AWS Organizations , cho phép bạn thiết lập một môi trường đa tài khoản được thiết kế tốt dựa trên các phương pháp bảo mật tốt nhất.

Để biết hướng dẫn chi tiết về cách tạo vùng hạ cánh an toàn cho khối lượng công việc viễn thông, hãy tham khảo bài đăng trên blog toàn diện của chúng tôi về chủ đề này.

Chúng tôi đã phân tích các khuyến nghị trong hướng dẫn của CISA và nhóm chúng thành sáu danh mục thuộc hai lĩnh vực chính. Vui lòng tham khảo trang GitHub được liên kết ở cuối bài viết này, nơi có hướng dẫn chi tiết hơn về các dịch vụ AWS liên quan có thể hỗ trợ bạn triển khai từng biện pháp bảo mật riêng lẻ trong hướng dẫn.

# Ghi nhật ký và giám sát
Hướng dẫn trong danh mục này nhấn mạnh tầm quan trọng của việc tăng cường khả năng hiển thị để hiểu rõ hoạt động mạng, điều này rất cần thiết cho việc phát hiện các bất thường và ứng phó với sự cố. Các biện pháp kiểm soát bảo mật chính bao gồm: có khả năng quản lý tài sản mạnh mẽ, cho phép ghi nhật ký ở nhiều cấp độ khác nhau, tập trung ghi nhật ký, bảo vệ cơ sở hạ tầng ghi nhật ký và giám sát, và sử dụng các công cụ bảo mật để phát hiện các bất thường và sự cố.

Khả năng hiển thị nâng cao là một lợi thế vốn có của mô hình đám mây công cộng, đặc biệt là trong AWS. Tính minh bạch này không chỉ là một tính năng bổ sung, mà còn là một nhu cầu cơ bản được thúc đẩy bởi mô hình kinh doanh lấy API làm trọng tâm, trả tiền theo mức sử dụng. Để lập hóa đơn chính xác cho khách hàng, AWS đã tích hợp các chức năng theo dõi và ghi nhật ký toàn diện vào kiến ​​trúc cốt lõi của mình. Nhờ đó, AWS cung cấp các công cụ mạnh mẽ cho phép bạn thu thập, tập trung và giám sát nhật ký trên mọi lớp tải công việc mạng của mình. Mức độ hiển thị này vượt xa những gì thường đạt được trong cơ sở hạ tầng truyền thống, mang đến cho bạn cái nhìn sâu sắc chưa từng có về tài sản CNTT và hoạt động của người dùng.

Một hướng dẫn quan trọng khác là tập trung hóa việc ghi nhật ký liên quan đến bảo mật, đồng thời tách biệt cơ sở hạ tầng ghi nhật ký khỏi các môi trường sản xuất khác. Bạn có thể triển khai hướng dẫn này trong AWS bằng cách sử dụng Amazon Security Lake cùng với OpenSearch được triển khai trong các tài khoản riêng biệt, với quyền truy cập chỉ giới hạn cho tổ chức bảo mật. Ngoài ra, giải pháp này cung cấp phương pháp triển khai thực tiễn tốt nhất về việc tạo các đường ống thu thập và tiếp nhận để cho phép tập trung hóa và kiểm tra các nguồn nhật ký trên toàn bộ khối lượng công việc AWS của bạn mà không cần sử dụng Security Lake.

# Quản lý cấu hình và thay đổi

Hướng dẫn trong danh mục này nhấn mạnh vào việc tập trung hóa, bảo mật và bảo vệ cấu hình. Hướng dẫn này nhấn mạnh tầm quan trọng của việc phát hiện và cung cấp cảnh báo về các sửa đổi trái phép, kiểm tra cấu hình để đảm bảo tuân thủ và quy trình quản lý thay đổi tự động hóa các thay đổi thường xuyên nhằm giảm thiểu sai lệch ngoài ý muốn.

Trong AWS, bạn có thể triển khai cơ sở hạ tầng và cấu hình dưới dạng mã, cho phép lưu trữ tập trung cấu hình trong các kho lưu trữ, theo dõi các thay đổi thông qua kiểm soát phiên bản và triển khai các thay đổi thông qua các quy trình quản lý thay đổi đã được phê duyệt. Bạn có thể sử dụng kho lưu trữ mã và các quy trình tích hợp liên tục và phân phối liên tục (CI/CD) để tự động hóa việc triển khai các cấu hình này, giúp bạn tăng tốc độ triển khai, duy trì tính nhất quán, đơn giản hóa việc quản lý và triển khai quy trình kiểm soát thay đổi nghiêm ngặt và có thể kiểm tra.

Bất kể cơ sở hạ tầng được triển khai và quản lý như thế nào, bạn có thể sử dụng dịch vụ AWS Config để tự động theo dõi trạng thái hiện tại và lịch sử của một tập hợp lớn thông tin cấu hình trên hơn 100 dịch vụ AWS và hàng trăm loại tài nguyên của chúng. Bạn cũng có thể viết các quy tắc AWS Config tùy chỉnh để thực hiện các hành động tự động bất cứ khi nào tài nguyên nhạy cảm bị sửa đổi, hoặc tận dụng hơn 400 quy tắc được AWS quản lý trong AWS Config để gửi cảnh báo hoặc tự động khắc phục khi tài nguyên quan trọng thay đổi trạng thái.

# Quản lý danh tính và quyền truy cập
Hướng dẫn trong danh mục này nhấn mạnh tầm quan trọng của việc quản lý tài khoản và quyền hoạt động, sử dụng các phương pháp xác thực chống lừa đảo, triển khai đặc quyền tối thiểu thông qua kiểm soát truy cập dựa trên vai trò, quản lý truy cập khẩn cấp và giới hạn phiên.

Xác thực và ủy quyền, vốn là những thành phần quan trọng của kiểm soát truy cập, được quản lý thông qua AWS Identity and Access Management (IAM) , AWS IAM Identity Center và AWS Organizations. AWS cung cấp cho bạn khả năng quản lý quyền ở quy mô lớn trên các danh tính, tài nguyên và dịch vụ, bao gồm việc yêu cầu sử dụng xác thực đa yếu tố (MFA) cho các lần đăng nhập. Hơn nữa, các khả năng này hỗ trợ khách hàng tuân thủ nguyên tắc đặc quyền tối thiểu bằng cách khuyến khích quản lý thông tin xác thực theo phiên, có giới hạn thời gian bằng cách sử dụng AWS Security Token Service (AWS STS) .

Phần mềm chạy trên đám mây cần gọi API đám mây sẽ tự động nhận thông tin xác thực tạm thời và được luân chuyển thường xuyên thông qua các vai trò IAM dành cho Amazon Elastic Compute Cloud (Amazon EC2) , Amazon Elastic Container Service (Amazon ECS) , Amazon Elastic Kubernetes Service (Amazon EKS) và AWS Lambda , giúp loại bỏ nhu cầu sử dụng thông tin xác thực dài hạn có thể bị rò rỉ hoặc bị xâm phạm. Quyền truy cập vào API đám mây từ phần mềm tại chỗ có thể được khởi động an toàn từ các công nghệ quản lý danh tính doanh nghiệp bằng cách sử dụng AWS IAM Roles Anywhere . Bạn thậm chí có thể bảo vệ xác thực đối với các công nghệ không phải đám mây bằng cách kết hợp các vai trò và sử dụng AWS Secrets Manager để bảo vệ và tự động luân chuyển các bí mật như mật khẩu cơ sở dữ liệu.

# Quản lý mạng và lưu lượng

Hướng dẫn trong danh mục này nhấn mạnh vào việc phân đoạn khối lượng công việc và mạng để hạn chế khả năng di chuyển ngang và tiếp xúc với internet, giám sát và điều chỉnh luồng lưu lượng bằng cách sử dụng chính sách và bảo mật các cổng không sử dụng.

Bạn có thể đạt được khả năng phân đoạn mạng vi mô, một khía cạnh quan trọng của kiến ​​trúc bảo mật hiện đại, thông qua VPC và mạng con. Ví dụ: bạn có thể tách biệt các thành phần hướng internet của ứng dụng với các thành phần không yêu cầu quyền truy cập này bằng cách đặt chúng vào các VPC riêng biệt và chỉ cho phép truy cập internet trên VPC yêu cầu. Bạn có thể kiểm soát lưu lượng truy cập trong và giữa các phân đoạn bằng cách sử dụng nhiều dịch vụ mạng khác nhau—ví dụ như bảng định tuyến, cổng internet, cổng trung chuyển và dịch vụ tường lửa. Việc phân đoạn này giảm thiểu rủi ro từ các hoạt động trái phép bắt nguồn từ internet và hạn chế khả năng di chuyển ngang trong trường hợp bị xâm phạm.

Để triển khai hướng dẫn về quản lý ngoài băng tần, bạn có thể thiết kế các kết nối mạng của mình để tách lưu lượng quản lý khỏi lưu lượng tín hiệu mạng bằng cách sử dụng các mạng con—ví dụ: một phiên bản EC2 duy nhất có thể có nhiều giao diện mạng đàn hồi (ENI) được đính kèm vào các mạng con khác nhau hoặc thậm chí các VPC khác nhau : một giao diện chỉ cho phép lưu lượng quản lý và một giao diện khác chỉ cho phép lưu lượng tín hiệu.

# Mã hóa mạnh mẽ để mã hóa dữ liệu khi lưu trữ và khi truyền tải

Hướng dẫn trong danh mục này nhấn mạnh việc sử dụng bộ mã hóa mạnh, phiên bản bảo mật của giao thức mã hóa và chứng chỉ dựa trên PKI để bảo vệ dữ liệu khi lưu trữ và khi truyền tải.

Mã hóa, nền tảng của bảo vệ dữ liệu, được giải quyết toàn diện trong các dịch vụ AWS. Các điểm cuối API của dịch vụ AWS hỗ trợ TLS 1.3 (và tối thiểu là TLS 1.2) với bộ mã hóa dựa trên tiêu chuẩn an toàn, khóa mã hóa và các tính năng bảo mật nâng cao như HKDF (hàm dẫn xuất khóa trích xuất và mở rộng dựa trên HMAC) để tăng cường bảo mật. Các dịch vụ AWS quản lý bí mật của khách hàng được gửi qua đường truyền cũng hỗ trợ mật mã hậu lượng tử . Ví dụ: AWS Key Management Service (AWS KMS) , AWS Certificate Manager và AWS Secrets Manager hỗ trợ tùy chọn trao đổi khóa hậu lượng tử kết hợp cho giao thức mã hóa mạng TLS. Khi sử dụng Giao thức cổng biên (BGP), AWS sử dụng Cơ sở hạ tầng khóa công khai tài nguyên (RPKI) và Ủy quyền nguồn gốc tuyến đường (ROA ) để bảo vệ không gian địa chỉ IP của Amazon và các tuyến đường khỏi cấu hình sai và chiếm đoạt.

Bạn cũng có thể sử dụng các dịch vụ mã hóa của AWS như AWS KMS, AWS CloudHSM và AWS Certificate Manager để bảo mật dữ liệu khi truyền tải và khi lưu trữ. Khóa bạn tạo trong AWS KMS được bảo vệ bởi các mô-đun bảo mật phần cứng (HSM) được xác thực theo chuẩn FIPS 140-2 Cấp độ 3, và không có cơ chế nào cho phép bất kỳ ai, kể cả người vận hành dịch vụ AWS, xem, truy cập hoặc xuất dữ liệu khóa dạng văn bản thuần túy.

AWS Secrets Manager giúp bạn quản lý, truy xuất và luân chuyển thông tin xác thực cơ sở dữ liệu, thông tin xác thực ứng dụng, mã thông báo OAuth, khóa API và các thông tin bí mật khác một cách an toàn trong suốt vòng đời của chúng. Để biết thêm chi tiết về các giải pháp mã hóa và phương pháp hay nhất của AWS, vui lòng tham khảo Phương pháp hay nhất về mã hóa cho các dịch vụ AWS .

# Quản lý lỗ hổng

Hướng dẫn này nhấn mạnh việc giảm thiểu rủi ro khai thác thông qua quản lý vòng đời phù hợp, vá lỗi thường xuyên và loại bỏ các giao thức không an toàn. AWS giúp giải quyết các yêu cầu này thông qua cả trách nhiệm chung và các phương pháp tiếp cận kiến ​​trúc sáng tạo.

Theo mô hình chia sẻ trách nhiệm, AWS quản lý bảo mật của cơ sở hạ tầng cơ bản. Điều này bao gồm việc duy trì các hệ thống và dịch vụ được cập nhật, vô hiệu hóa các giao thức không an toàn và các cổng không sử dụng, đồng thời cung cấp Bản tin Bảo mật để thông báo kịp thời về lỗ hổng. Các dịch vụ AWS được hỗ trợ thông qua các điều khoản được xác định theo hợp đồng , do đó bạn không cần phải lo lắng về các thành phần cơ sở hạ tầng hết hạn sử dụng.

Đối với các ứng dụng của bạn, AWS cho phép một phương pháp tiếp cận mang tính chuyển đổi để quản lý lỗ hổng thông qua các tài nguyên tạm thời và cơ sở hạ tầng bất biến. Thay vì duy trì các phiên bản tồn tại lâu dài yêu cầu vá lỗi liên tục, bạn có thể duy trì một Amazon Machine Image (AMI) duy nhất, được củng cố và cập nhật thường xuyên làm hình ảnh vàng của mình . Khi cần cập nhật, thay vì vá các phiên bản đang chạy, bạn chỉ cần triển khai các phiên bản mới với mã ứng dụng được cài đặt từ AMI đã cập nhật. Các phương pháp tiếp cận tương tự cũng áp dụng cho khối lượng công việc dựa trên container. Khối lượng công việc dựa trên AWS Lambda giảm thiểu trách nhiệm vá lỗi của bạn hơn nữa, vì chỉ cần cập nhật mã chứa logic kinh doanh của bạn (và bất kỳ lớp hỗ trợ nào bạn đã chọn)—AWS tự động vá các trình quản lý siêu giám sát, hệ điều hành và container cơ bản. Phương pháp tiếp cận này cho phép bạn giữ hệ thống của mình ở trạng thái an toàn đã biết đồng thời giảm cả bề mặt mối đe dọa và chi phí vận hành. Bạn có thể tăng cường bảo mật hơn nữa bằng cách sử dụng các tính năng mạng của AWS như nhóm bảo mật để vô hiệu hóa các giao thức không an toàn, chẳng hạn như thực thi HTTPS thay vì HTTP.

# Phần kết luận
Hướng dẫn toàn diện từ các cơ quan an ninh mạng cung cấp một khuôn khổ thiết yếu để bảo mật cơ sở hạ tầng viễn thông. Như đã trình bày trong bài viết này, AWS cung cấp một bộ dịch vụ và khả năng mạnh mẽ, phù hợp với các khuyến nghị từ CISA và các chính phủ đồng minh. Từ khả năng hiển thị nâng cao thông qua ghi nhật ký và giám sát, đến quản lý danh tính, phân đoạn mạng, mã hóa và quản lý lỗ hổng mạnh mẽ, AWS cung cấp các công cụ bạn cần để triển khai các biện pháp kiểm soát bảo mật này một cách hiệu quả, đồng thời duy trì hiệu suất hoạt động. Mô hình chia sẻ trách nhiệm, kết hợp với sự đổi mới liên tục của AWS về bảo mật, cho phép các công ty viễn thông xây dựng và duy trì môi trường đám mây an toàn, linh hoạt.
