---
title: "Bản đề xuất"
date: 2025-09-09
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# NỀN TẢNG TRỰC TUYẾN ĐỂ THEO DÕI VÀ DỰ BÁO QUỸ ĐẠO BÃO 

## Kỹ thuật tự nhận thức trắc địa (Geodesic-Aware Deep Learning) cho dự đoán hướng di chuyển chính xác: Một phương pháp tiếp cận dựa trên kêt hợp "mô phỏng vật lý" và "tăng cường dữ liệu"

### 1. Tóm tắt điều hành  

Dữ liệu chuỗi thời gian đóng vai trò là một trong những dạng biểu diễn thông tin quan trọng nhất trong các ứng dụng khoa học và công nghiệp hiện đại. Nó cần thiết để hiểu các quá trình động như xu hướng kinh tế, mô hình tiêu thụ năng lượng và sự thay đổi khí tượng theo thời gian. Đặc biệt, dự báo thời tiết phụ thuộc mạnh mẽ vào dữ liệu chuỗi thời gian để dự đoán điều kiện khí quyển trong tương lai, quỹ đạo bão và các biến động theo mùa dựa trên dữ liệu lịch sử.

Với sự phát triển nhanh chóng trong nghiên cứu về học sâu và mạng nơ-ron, dự án của chúng tôi hướng đến việc phát triển một mô hình dự báo tiên tiến có khả năng dự đoán chính xác đường đi, cường độ và tổng quãng đường di chuyển của các cơn bão trong vài ngày tiếp theo. Các dự đoán này có thể hỗ trợ hệ thống cảnh báo sớm, giúp chính quyền và cư dân tại các khu vực bị ảnh hưởng có thể đưa ra biện pháp phòng tránh trước khi bão đổ bộ.

Để vượt qua những hạn chế của công nghệ và nghiên cứu hiện tại, nghiên cứu này giới thiệu một số kỹ thuật và thuật toán mới, bao gồm hai phương pháp tăng cường dữ liệu mới cho chuỗi thời gian trắc địa và một cơ chế mã hóa không gian được thiết kế để nâng cao hiệu suất dự đoán của điện toán tích chập. Hệ thống cuối cùng sẽ được tích hợp vào kiến trúc đám mây không máy chủ trên AWS, đảm bảo khả năng mở rộng, tính khả dụng cao và hiệu quả chi phí cho việc theo dõi và phân tích bão theo thời gian thực.

Kiến trúc hệ thống tận dụng một số dịch vụ AWS để tạo thành một quy trình xử lý và triển khai dữ liệu được quản lý hoàn toàn. Các hàm AWS Lambda đóng vai trò là là xương sống cho hoạt động tính toán không máy chủ, được kích hoạt tự động bởi Amazon EventBridge để thu thập và xử lý dữ liệu bão mới từ các nguồn khí tượng mở theo lịch trình. Dữ liệu đã xử lý được lưu trữ an toàn trên Amazon S3, trong khi AWS CodePipeline và CodeBuild tự động hóa việc tích hợp và triển khai liên tục các phiên bản mô hình mới. Mô hình sau khi huấn luyện được lưu trữ và cung cấp qua Amazon API Gateway, cho phép nền tảng dự báo trực tuyến thực hiện các yêu cầu suy luận nhẹ, theo thời gian thực. Mọi hoạt động của hệ thống được giám sát thông qua Amazon CloudWatch, cung cấp khả năng quan sát hoạt động, phát hiện lỗi và các số liệu hiệu suất.

Phương pháp đầu tiên được đề xuất, Khung Tăng cường Dữ liệu Phai mờ Theo Thời Gian Theo Từng Bước (Stepwise Temporal Fading Augmentation - STFA), là một khuôn khổ tăng cường dữ liệu chuỗi thời gian mới, mô phỏng sự suy giảm tự nhiên của ảnh hưởng từ các quan sát trong quá khứ. Khác với các phương pháp tiếp cận truyền thống dựa trên việc gây nhiễu ngẫu nhiên hoặc bơm nhiễu, STFA áp dụng các trọng số phai mờ cho các bước thời gian trước đó trong khi vẫn bảo toàn thông tin gần đây. Quá trình này tạo ra các chuỗi tổng hợp đa dạng và chân thực, từ đó cải thiện độ mạnh mẽ và khả năng khái quát hóa của mô hình. Kỹ thuật này sẽ được đánh giá trên các tác vụ dự báo quỹ đạo bão, vốn dựa trên dữ liệu vĩ độ-kinh độ tuần tự.

Phương pháp thứ hai, Tăng cường Hướng đi Trắc địa Hợp lý (Plausible Geodesic Bearing Augmentation - PGBA), giới thiệu một chiến lược tăng cường dựa trên phạm vi khả thi của hướng di chuyển và khoảng cách của bão. Bằng cách phân tích hướng đi trắc địa (geodesic bearing) và khoảng cách giữa các vị trí bão liên tiếp, PGBA xác định một biên giới chuyển động thực tế, trong đó các quỹ đạo tổng hợp mới được tạo ra. Cách tiếp cận này nâng cao khả năng của mô hình trong việc nắm bắt sự biến đổi không gian tự nhiên và độ bất định về hướng trong chuyển động của bão.

Ngoài ra, nghiên cứu này còn khám phá một biểu diễn không gian-thời gian (spatial-temporal representation) của dữ liệu chuỗi thời gian, cho phép áp dụng các mạng nơ-ron tích chập (Convolutional Neural Networks - CNNs) để nắm bắt cả sự phụ thuộc về không gian và thời gian. Biểu diễn này tận dụng thế mạnh của điện toán tích chập để mô hình hóa các tương tác cục bộ trên không gian và thời gian. Nó sẽ đóng vai trò là cơ sở (baseline) để so sánh với các mô hình Mạng Tích chập Thời gian (Temporal Convolutional Network - TCN) được huấn luyện bằng các phương pháp tăng cường dữ liệu đã được đề xuất.

Các mạng nơ-ron truyền thống, dù được sử dụng cho mô hình hóa dữ liệu chuỗi thời gian hay dữ liệu ảnh, chủ yếu chỉ học các mẫu thống kê từ dữ liệu. Tuy nhiên, trong nhiều hệ thống vật lý thực tế, các mô hình hoàn toàn dựa trên dữ liệu như vậy có thể không tuân thủ các ràng buộc tự nhiên, chẳng hạn như các mối quan hệ trọng lực hoặc trắc địa. Để khắc phục hạn chế này, chúng tôi kết hợp các nguyên lý của Học máy Thông tin Vật lý (Physics-Informed Machine Learning - PIML) vào phương pháp tiếp cận của mình.khoảng cách trắc địa và phương vị được lấy từ dữ liệu vĩ độ-kinh độ và được tích hợp vào quá trình huấn luyện của mô hình như như những đặc trưng mang ý nghĩa vật lý. Hơn nữa, chúng tôi sử dụng công thức Haversine — công thức tính khoảng cách hình cầu giữa hai điểm - làm một số hạng mất mát phụ, bổ sung cho các số liệu sai số chuẩn như MSE, RMSE, MAE và MAPE.

Bằng cách kết hợp các phương pháp tăng cường dữ liệu được đề xuất, các nguyên tắc học máy thông tin vật lý (PIML) và một cơ sở hạ tầng học sâu không máy chủ được vận hành bởi các dịch vụ AWS, nghiên cứu này hướng đến mục tiêu phát triển một khuôn khổ dự báo quỹ đạo bão có khả năng mở rộng, mạnh mẽ và chính xác. Hệ thống thu được không chỉ nâng cao trình độ mô hình hóa chuỗi thời gian trắc địa mà còn chứng minh tính thực tiễn của việc triển khai các hệ thống dự báo môi trường dựa trên AI như các ứng dụng bản địa đám mây (cloud-native) bền bỉ, giúp tăng cường khả năng chuẩn bị và đảm bảo an toàn cho các khu vực dễ bị ảnh hưởng bởi bão. 

### 2. Tuyên bố vấn đề  

### Vấn đề hiện tại?

Để phát triển một nền tảng đáng tin cậy nhằm theo dõi và đưa ra cảnh báo về đường đi của bão trong tương lai, việc xây dựng một mô hình máy học vừa chính xác vừa có khả năng đưa ra các dự đoán đáng tin cậy là hết sức quan trọng. Thành phần theo dõi có thể được giải quyết bằng cách liên tục thu thập và cập nhật dữ liệu từ các nguồn khí tượng công cộng. Tuy nhiên, các tập dữ liệu này thường bị giới hạn về mặt địa lý và chứa nhiều thông tin trùng lặp hoặc không đầy đủ.

Ngược lại, thành phần dự báo lại có độ phức tạp cao hơn. Việc đạt được dự báo chuỗi thời gian chính xác trong bối cảnh này thường phải đối mặt với hai thách thức chính: (1) tính đa dạng và phạm vi bao phủ của dữ liệu có sẵn còn hạn chế, và (2) sự thiếu vắng các nguyên tắc vật lý nền tảng, điều này làm hạn chế khả năng của mô hình trong việc phản ánh các động lực địa vật lý cơ bản của hành vi bão.

- **Thiếu dữ liệu**: Nhiều tác vụ dự báo chuỗi thời gian gặp hạn chế về lượng dữ liệu huấn luyện. Mặc dù có nhiều phương pháp tăng cường dữ liệu, nhưng rất ít phương pháp tập trung trực tiếp vào sự suy giảm tầm quan trọng của các giá trị trong quá khứ theo thời gian.

- **Bỏ qua yếu tố vật lý:**: Hầu hết các mạng nơ-ron chỉ học từ dữ liệu thô mà không xét đến các ràng buộc vật lý trong thế giới thực. Trong các tác vụ dự đoán quỹ đạo (ví dụ: bão), điều này thường dẫn đến những kết quả phi thực tế.


*Mục tiêu của nhóm* :
+ Phát triển một phương pháp tăng cường chuỗi thời gian mới (STFA) nhằm cải thiện độ bền vững và khả năng khái quát của mô hình.    
+ Tích hợp các ràng buộc dựa trên vật lý vào quá trình huấn luyện mô hình, thu hẹp khoảng cách giữa học máy dựa trên dữ liệu và động lực học của thế giới thực.
+ Kiểm tra sức mạnh của mạng tích chập (convolution2D) trong việc dự báo quỹ đạo.
+ Xây dựng một nền tảng trực tuyến cung cấp thông tin mới nhất về các cơn bão hiện tại và các dự đoán chính xác về quỹ đạo của chúng.
 

### Giải Pháp 

#### A - Tăng cường cường dữ liệu bằng Stepwise Temporal Fading (STFA)

STFA biến đổi các chuỗi thời gian bằng cách giảm dần ảnh hưởng của các giá trị trong quá khứ. Khác với phương pháp thêm nhiễu ngẫu nhiên, nó áp dụng một cách có hệ thống các hệ số phai mờ theo từng bước vào các nhóm dữ liệu cũ.

Cho một chuỗi đơn biến là:

$$
X = [x_0, x_1, \ldots, x_{T-1}]
$$

trong đó $T$ là độ dài của chuỗi $X$.

**Các tham số**:

- $n$: khoảng giá trị gần nhất muốn giữ nguyên
- $S$: số dải được áp dụng phai dần
- $L = T - n$: độ dài của vùng phai dần
- $k = \frac{L}{S}$: số giá trị trong mỗi dải
- $I_b$: tập chỉ số của dải thứ $b$

\\[
I_b = \{\, i \mid L - b \cdot k \;\leq\; i \;\leq\; L - (b-1)\cdot k - 1 \,\}
\\]

**Biến đổi:**:

Ký hiệu chuỗi dữ liệu đã được tăng cường là:

$$
X = [x_0, \ldots, x_{T-1}]
$$


với các quy tắc biến đổi sau:

$$
x_t =
\begin{cases}
x_t, & t \in \{T-n, \ldots, T-1\}, \\\
m_b \, x_t, & t \in I_b, \\\
m_{S+1} \, x_t, & t < \min(I_S),
\end{cases}
$$

trong đó các hệ số nhân $m_b \in (0,1)$ giảm dần đều từ các nhóm dữ liệu gần nhất tới các dữ liệu cũ hơn.

Công thức này duy trì độ thực tế, chính xác của các dữ liệu gần đây trong khi kiểm soát chặt chẽ hơn ảnh hưởng của các giá trị xa trong chuỗi. Việc tăng cường này buộc mô hình tập trung vào các mẫu hình mạnh mẽ hơn thay vì chỉ phụ thuộc vào dữ liệu thô, đồng thời gia tăng tính đa dạng của dữ liệu theo các tham số được chọn.

#### B - Tăng Cường theo Hướng Di Chuyển Hợp Lý (PGBA - Plausible Geodesic Bearing Augmentation)

Kỹ thuật Tăng cường Hướng Di chuyển (dựa trên) Trắc địa Hợp lý (PGBA) nâng cao tính chân thực và khả năng kiểm soát của việc tạo ra quỹ đạo trong các tác vụ dự báo chuỗi thời gian không gian địa lý. Khác với các phương pháp nhiễu loạn ngẫu nhiên thông thường, PGBA đưa vào yếu tố ngẫu nhiên nhưng vẫn đảm bảo sự hợp lý trong các ràng buộc vật lý của chuyển động cơ bản. Các quỹ đạo được tạo ra bắt nguồn từ các mối quan hệ hình học của các quan sát trong quá khứ thay vì từ các bước hoàn toàn ngẫu nhiên, dẫn đến các đường đi mượt mà hơn và tính biến thiên có ý nghĩa trong tập dữ liệu huấn luyện. Kỹ thuật này được áp dụng cho **mỗi bốn vị trí** trong một chuỗi dữ liệu.

PGBA đóng vai trò là phương pháp tăng cường bổ trợ cho STFA, làm phong phú thêm tính đa dạng của các mẫu huấn luyện trong khi vẫn bảo toàn cấu trúc động học ban đầu. Mục tiêu của nó là tạo ra các quỹ đạo dư thừa nhưng vẫn đảm bảo tính nhất quán vật lý, từ đó nắm bắt được các biến thể tiềm năng trong chuyển động của bão hoặc các hiện tượng không gian địa lý tương tự.

 **Cơ chế cốt lõi**

Xét một quỹ đạo bão được biểu diễn bằng một chuỗi $n$ các vị trí địa lý theo thứ tự:

$$
P = [P_1, P_2, \ldots, P_n]
$$

Chúng tôi chia chuỗi này thành các khối nhỏ gồm 4 điểm, với $P_i$ là điểm bắt đầu của mỗi khối, được xác định bởi vĩ độ và kinh độ:

$$
P_i = (\phi_i, \lambda_i)
$$

Ở đây, $\phi_i$ và $\lambda_i$ lần lượt biểu thị cho vĩ độ và kinh độ theo đơn vị radian.

Khoảng cách trắc địa $d_i$ và góc phương vị $\theta_i$ giữa hai điểm liên tiếp $P_i$ và $P_{i+1}$ được xác định như sau:

$$
d_i = \text{Distance}(P_i, P_{i+1}), \qquad
\theta_i = \text{Bearing}(P_i, P_{i+1})
$$

Để tạo ra biến đổi hợp lý, PGBA làm nhiễu góc phương vị bằng cách thêm một nhiễu ngẫu nhiên nhỏ có phân phối đều $\epsilon_i$:

$$
\theta_i^{\text{aug}} = \theta_i + \epsilon_i, \qquad
\epsilon_i \sim \text{Uniform}(-\delta, \delta)
$$

trong đó $\delta$ là giới hạn góc có thể điều chỉnh để kiểm soát phạm vi lệch.

Hai điểm đầu tiên luôn được giữ nguyên và được sử dụng để tính toán khoảng cách và hướng di chuyển.
Điểm tiếp theo được tăng cường sau đó được tính toán bằng công thức định vị trắc địa, giữ nguyên khoảng cách trong khi cho phép hướng di chuyển thay đổi trong phạm vi nhiễu ngẫu nhiên:

$$
P_{i+2}^{\text{aug}} = \text{Destination}(P_i, d_i, \theta_i^{\text{aug}})
$$

Quá trình này bảo toàn khoảng cách giữa các điểm $d_i$ trong khi làm lệch hướng một cách nhẹ để tạo ra các độ lệch hợp lý về mặt vật lý.


 **Làm mượt và Hiệu chỉnh Đa Bước**
 
Để nâng cao độ mượt và tạo ra các quỹ đạo cong tự nhiên, PGBA áp dụng một hiệu chỉnh tại mỗi điểm thứ tư.
Gọi $P_{i+3}^{\text{aug}}$ là điểm thứ tư. Nó được tính toán lại sao cho góc phương vị $\theta_{i+3}^{\text{corr}}$ của nó tối thiểu hóa độ lệch so với điểm gốc $P_{i+3}$:

$$
\theta_{i+3}^{\text{corr}} =
\arg\min_{\theta};
\text{Distance}\Big(
\text{Destination}(P_{i+2}^{\text{aug}}, d_{i+2}, \theta),;
P_{i+3}
\Big)
$$

Sau đó, điểm tăng cường đã hiệu chỉnh được xác định như sau:

$$
P_{i+3}^{\text{aug}} =
\text{Destination}(P_{i+2}^{\text{aug}}, d_{i+2}, \theta_{i+3}^{\text{corr}})
$$

Bước này đảm bảo sự chuyển tiếp mượt mà giữa nhiều điểm trong khi vẫn duy trì tính hợp lý về mặt vật lý.

Lưu ý rằng hai điểm đầu tiên và điểm cuối cùng trong mỗi chuỗi thời gian trắc địa luôn được giữ nguyên không thay đổi.

#### C - Học máy (Machine Lerning) dựa trên vật lý

Các mô hình mạng nơ-ron như RNNs, CNNs, và Transformers không cần công thức hay quy tắc đặc thù cho từng tác vụ để đạt hiệu suất tốt, miễn là chúng được huấn luyện với đủ dữ liệu. Ví dụ, trong tác vụ dịch máy như dịch từ tiếng Đức sang tiếng Anh bằng RNN, không có quy tắc ngữ pháp nào được cung cấp trong quá trình huấn luyện (không tính stemming, letmat, pos tagging, ... chỉ nói đến quá trình huấn luyện). Tuy nhiên, mô hình vẫn có thể tạo ra bản dịch mạch lạc, thể hiện một trong những điểm mạnh chính của học sâu: khả năng học trực tiếp các mẫu phức tạp từ dữ liệu. Ngược lại, các phương pháp truyền thống — chẳng hạn như các hệ thống dịch dựa trên quy tắc (ví dụ: Google Translate trước những năm 2000) — phụ thuộc nhiều vào quy tắc ngữ pháp và từ điển. Mặc dù chính xác, nhưng các hệ thống này thường thiếu linh hoạt và thất bại khi gặp từ có nhiều nghĩa hoặc các cấu trúc phụ thuộc vào ngữ cảnh.

Lấy cảm hứng từ sự khác biệt đó, mục tiêu của chúng tôi là kết hợp sức mạnh của học sâu với các công thức do con người định nghĩa để đạt hiệu suất tốt hơn. Cụ thể trong lĩnh vực địa lý này, chúng tôi sẽ thử đưa các lợi ích từ công thức Haversine vào quá trình huấn luyện để tính toán khoảng cách và góc phương vị giữa hai vị trí trên một hình cầu. Những yếu tố này cung cấp cho mô hình một cấu trúc bổ sung và độ lệch quy nạp, định hướng việc học vượt ra ngoài các tương quan thuần túy thống kê.

**Công thức Haversine**

*Đối với việc tính toán khoảng cách*

Công thức Haversine được sử dụng để tính khoảng cách cung lớn giữa hai điểm trên bề mặt hình cầu — tức là đường đi ngắn nhất trên bề mặt Trái Đất.

$$
d = 2r \, \arcsin\!\left( 
  \sqrt{ 
    \sin^2\!\left(\frac{\Delta \varphi}{2}\right)
    + \cos(\varphi_1)\cos(\varphi_2)
      \sin^2\!\left(\frac{\Delta \lambda}{2}\right)
  } 
\right)
$$

Trong đó:
- $$(\varphi_1, \lambda_1)$$ và $$(\varphi_2, \lambda_2)$$ là vĩ độ và kinh độ của hai điểm (tính bằng raidian). 
- $\Delta \varphi = \varphi_2 - \varphi_1$
- $\Delta \lambda = \lambda_2 - \lambda_1$
- $r$ là bán kính Trái Đất (≈ 6,371 km).

Trong khuôn khổ của chúng tôi, thay vì chỉ dựa vào các hàm mất mát tiêu chuẩn như MSE, RMSE hay MAPE, chúng tôi đề xuất sử dụng công thức Haversine để tính khoảng cách làm hàm mất mát chính. Khi mô hình xuất ra tọa độ vĩ độ và kinh độ cho vị trí bão tiếp theo, công thức Haversine trực tiếp đo lường khoảng cách giữa điểm dự đoán và điểm thực tế. Khoảng cách gần 0 cho thấy dự đoán có độ chính xác cao, trong khi khoảng cách lớn báo hiệu một sai số đáng kể.

*Đối với việc tính toán góc phương vị (bearing)*

Công thức tính góc phương vị được suy ra từ Công thức Haversine, cho biết hướng đi từ một điểm địa lý này đến một điểm khác dọc theo đường tròn lớn:

$$\theta = \text{atan2}\!\left(\sin(\Delta \lambda)\cos(\varphi_2),\, \cos(\varphi_1)\sin(\varphi_2) - \sin(\varphi_1)\cos(\varphi_2)\cos(\Delta \lambda)\right)$$

Trong đó:

- $(\varphi_1, \lambda_1)$ là điểm xuất phát.
- $(\varphi_2, \lambda_2)$ là điểm kết thúc.
- $\Delta \lambda$ là hiệu số kinh độ.

Kết quả $\theta$ đại diện cho góc phương vị ban đầu (azimuth) được đo theo chiều kim đồng hồ từ hướng Bắc thực.

Trong quá trình triển khai, chúng tôi tận dụng triệt để việc sử dụng Công thức Haversine để tính toán hai đặc trưng bổ sung — "khoảng cách" và "góc phương vị" — được thêm vào tập dữ liệu.
Các đặc trưng này cung cấp cho mô hình thông tin phong phú hơn về quỹ đạo bão, đồng thời vẫn duy trì mục tiêu cốt lõi là dự đoán vị trí địa lý tiếp theo.

## D - Tổng quan ngắn về những hiểu lầm phổ biến trong các phương pháp tiếp cận mô hình hóa chuỗi

Trong lĩnh vực mô hình hóa chuỗi thuộc học sâu, các kiến trúc dạng hồi quy như RNN, LSTM và GRU thường được coi là giải pháp mặc định. Nhận thức này đã khiến nhiều người thực hành và nhà nghiên cứu bỏ qua các kiến trúc thay thế, đặc biệt là Mạng Nơ-ron Tích chập (CNN) - vốn thường được liên hệ chủ yếu với các tác vụ xử lý hình ảnh. Sách giáo khoa và các khóa học thường phân loại các tác vụ như mô hình hóa ngôn ngữ, dịch máy, hoặc các dự đoán tuần tự khác là lĩnh vực của các mạng hồi quy, trong khi các mạng tích chập thường chỉ được giới thiệu trong bối cảnh dữ liệu không gian như hình ảnh. Kết quả là, tiềm năng của CNN cho mô hình hóa chuỗi thường bị đánh giá thấp hoặc bị bỏ qua.

**Các mạng tích chập** (Convolutional networks) mang lại một số ưu điểm khiến chúng phù hợp cho dữ liệu tuần tự. Tính song song vốn có của chúng cho phép huấn luyện nhanh hơn đáng kể so với các mô hình tuần tự nghiêm ngặt. Ngoài ra, CNN có hiệu quả cao trong việc nắm bắt các quan hệ không gian và thời gian cục bộ - một đặc tính có thể được tận dụng trong dự báo chuỗi thời gian và các tác vụ tuần tự khác. Tuy vậy, CNN thường bị hiểu nhầm là không phù hợp cho chuỗi do thiếu cơ chế bộ nhớ rõ ràng và không có thứ tự thời gian nội tại.

Trong nghiên cứu của chúng tôi, chúng tôi tập trung vào dự báo quỹ đạo bão, nơi dữ liệu bao gồm các bản ghi chuỗi thời gian của tọa độ vĩ độ và kinh độ. Chúng tôi chứng minh rằng loại dữ liệu tuần tự này có thể được mô hình hóa hiệu quả bằng cách sử dụng CNN, tận dụng hiệu suất tính toán và khả năng nắm bắt các mẫu hình không-thời gian cục bộ của chúng. Để hỗ trợ điều này, chúng tôi mã hóa các vị trí thành một biểu diễn ma trận 2D, cho phép mạng tích chập trích xuất và học các mẫu hình từ dữ liệu hiệu quả hơn. Mỗi mục trong ma trận tương ứng với một "điểm ảnh" của một hình ảnh, một cách tiếp cận mà chúng tôi gọi là **Quỹ đạo-dưới-dạng-Hình ảnh** (Trajectory-as-Image). Phương pháp của chúng tôi sử dụng một CNN tiêu chuẩn làm cơ sở, sau đó so sánh nó với các mô hình chuỗi chuyên biệt hơn, bao gồm TCN, LSTM và RNN, đồng thời kết hợp các kỹ thuật tăng cường dữ liệu khác nhau, bao gồm hai phương pháp mới được đề xuất trong công trình này.

Thông qua quá trình thử nghiệm và đánh giá có hệ thống, chúng tôi mong muốn thách thức quan điểm phổ biến cho rằng kiến trúc tích chập không phù hợp với dữ liệu chuỗi. Bằng cách làm nổi bật hiệu quả của CNN trong mô hình hóa chuỗi, chúng tôi hy vọng mở rộng góc nhìn của cộng đồng nghiên cứu và thực hành, khuyến khích họ xem xét điện toán tích chập như một hướng tiếp cận khả thi và cạnh tranh trong dự báo chuỗi thời gian cũng như các tác vụ dự đoán tuần tự khác.


### Lợi ích và Hiệu quả đầu tư

- **Tăng hiệu suất**:  STFA + PGBA tạo ra các chuỗi tổng hợp có cấu trúc giúp tăng cường độ mạnh mẽ của mô hình, giảm hiện tượng quá khớp và cải thiện khả năng khái quát hóa trên các quỹ đạo bão chưa từng thấy.

- **Nhận thức vật lý**: Việc tích hợp các nguyên lý địa lý như khoảng cách và góc phương vị giúp tăng khả năng diễn giải và đảm bảo kết quả dự đoán phù hợp với các ràng buộc vật lý.

- **Hướng nghiên cứu mới**: Thiết lập hai mô hình mới cho việc tăng cường chuỗi thời gian dựa trên sự phai mờ liên quan theo thời gian, mở rộng bộ công cụ phương pháp luận cho học máy chuỗi.

- **Scalability and Reusability**: Khuôn khổ kết hợp STFA + PGBA + PIML có thể được mở rộng sang các lĩnh vực dự báo chuỗi khác như nhu cầu năng lượng, lưu lượng giao thông và xu hướng tài chính.

**Tác động tổng thể**: Bằng cách cải thiện độ ổn định và khả năng diễn giải của mô hình trong khi vẫn duy trì tính mở rộng, phương pháp được đề xuất mang lại cả giá trị khoa học lẫn hiệu quả thực tiễn trong đầu tư tính toán.


### 3. Kiến trúc giải pháp  

Nền tảng trực tuyến cung cấp cho người dùng thông tin cập nhật về các cơn bão gần đây và một công cụ mạnh mẽ để dự báo quỹ đạo bão. Người dùng có thể xem dữ liệu bão gần đây hoặc chạy các dự đoán bằng các mô hình ML. Kết quả được hiển thị trực quan trên bản đồ, cho thấy vị trí, thời gian và đường đi dự đoán của bão.

Nền tảng được xây dựng bằng kiến trúc không máy chủ trên AWS để giảm chi phí vận hành trong khi vẫn đảm bảo khả năng mở rộng và độ tin cậy. Nội dung frontend được lưu trữ trên Amazon S3 và phân phối toàn cầu thông qua CloudFront, đảm bảo truy cập với độ trễ thấp. Yêu cầu của người dùng được định tuyến qua API Gateway tới các hàm Lambda, nơi xử lý tính toán dự đoán và truy xuất dữ liệu. Các mô hình ML đã được huấn luyện trước và tập dữ liệu bão gần đây được lưu trữ an toàn trong S3, với các bản cập nhật hàng tuần được quản lý tự động bởi trình thu thập dữ liệu kích hoạt qua EventBridge. Các khóa API nhạy cảm được lưu trữ trong Secrets Manager, và hiệu suất hệ thống được giám sát thông qua nhật ký và số liệu trên CloudWatch. IAM thực thi quyền truy cập tối thiểu cho tất cả các dịch vụ.

Các mô hình dự báo tận dụng các kỹ thuật STFA (Tăng cường Phai mờ Thời gian Theo từng Bước) và PGBA (Tăng cường Hướng đi Trắc địa Hợp lý) được đề xuất. Các phương pháp này tạo ra các quỹ đạo chuỗi thời gian tổng hợp chân thực, bảo toàn mức độ liên quan theo thời gian và tính nhất quán không gian, giúp cải thiện đáng kể độ mạnh mẽ và độ chính xác của mô hình. Bằng cách tích hợp STFA và PGBA, nền tảng cung cấp các dự báo đường đi bão chính xác hơn, giúp người dùng hiểu rõ hơn về hành vi của bão và đưa ra quyết định sáng suốt.

Kiến trúc này cho phép tạo ra một nền tảng phản hồi nhanh, hiệu quả về chi phí và bảo mật, nơi người dùng có thể trực quan hóa thông tin bão theo thời gian thực và khám phá các dự báo quỹ đạo bão với bản đồ tương tác, được hỗ trợ bởi các phương pháp tăng cường tiên tiến để nâng cao hiệu suất mô hình.

<p align="center">
  <img src="/images/2-Proposal/trainning_process.png" alt="Picture 1" />
  <br/>
  <strong style="font-size: 18px;">Hình 1 : Sơ đồ huấn luyện mô hình Machine Learning</strong>
</p>

<p align="center">
  <img src="/images/2-Proposal/skynet_diagram.png" alt="Picture 2" />
  <br/>
  <strong style="font-size: 18px;">Hình 2 : Kiến Trúc Platform</strong>
</p>


### Các dịch vụ AWS được sử dụng

* **Amazon S3**: Lưu trữ các tệp frontend tĩnh, các mô hình ML đã được huấn luyện trước và dữ liệu bão mới nhất.
* **AWS Lambda**: Chạy các mô hình dự đoán, truy xuất dữ liệu bão và thực thi tự động hóa thu thập dữ liệu web.
* **Amazon API Gateway**: Xử lý các yêu cầu từ frontend cho dự báo và dữ liệu bão.
* **Amazon CloudFront**: Phân phối nội dung tĩnh trên toàn cầu với độ trễ thấp.
* **Amazon Route 53**: Định tuyến lưu lượng người dùng đến CloudFront.
* **Amazon EventBridge**: Lập lịch trình thu thập dữ liệu hàng tuần.
* **AWS Secrets Manager**: Lưu trữ an toàn các khóa API từ bên ngoài.
* **Amazon CloudWatch**: Giám sát nhật ký Lambda, số liệu hiệu suất và tình trạng hệ thống.
* **AWS IAM**: Gắn quyền truy cập tối thiểu (least-privilege) cho tất cả các dịch vụ.

### Thiết kế thành phần

* **Lớp Frontend**: Được lưu trữ trên **S3** và phân phối thông qua **CloudFront**.
* **Lớp Backend**: Xử lý dự báo và truy xuất dữ liệu bằng **API Gateway** và **Lambda**.
* **lưu trữ Dữ liệu**: Các mô hình ML được lưu trữ trong S3, dữ liệu bão mới nhất được cập nhật hàng tuần bởi trình thu thập dữ liệu.
* **Tự động hóa**: **EventBridge** kích hoạt **Crawler Lambda** hàng tuần để lấy dữ liệu bão từ các nguồn bên ngoài.
* **Bảo mật & Giám sát**: Các thông tin nhạy cảm được lưu trữ trong **Secrets Manager**, số liệu và nhật ký được thu thập thông qua **CloudWatch**.

### 4. Triển Khai Kỹ Thuật

**Các Giai Đoạn Triển Khai**
Dự án này có ba phần chính: xây dựng pipeline dự đoán, thiết lập thu thập dữ liệu và triển khai nền tảng web. Mỗi phần trải qua bốn giai đoạn:

*   **Thiết Kế Kiến Trúc**: Lên kế hoạch cho hệ thống serverless AWS, các hàm Lambda, cấu trúc S3. (Tuần 1-2)
*   **Ước Tính Chi Phí**: Sử dụng AWS Pricing Calculator để đánh giá tính khả thi và điều chỉnh thiết kế (Tuần 1-2).
*   **Tối Ưu Hóa Kiến Trúc**: Điều chỉnh bộ nhớ Lambda, cách sử dụng S3 và bộ nhớ đệm để giảm chi phí (Tuần 2-4).
*   **Phát Triển, Kiểm Thử, Triển Khai**: Triển khai các hàm Lambda, lập lịch sự kiện, tích hợp mô hình ML và frontend web với Next.js (Tuần 4-8).

**Yêu Cầu Kỹ Thuật**

*   **Các Mô Hình ML**: Các mô hình quỹ đạo được đào tạo sẵn lưu trữ trong S3 (.h5/.pth), được tải bởi hàm Lambda dự đoán.
*   **Dữ Liệu Bão**: Các tệp JSON được cập nhật hàng tuần, lưu trữ trong S3, được sử dụng để hiển thị frontend và xác thực dự đoán.
*   **Cơ Sở Hạ Tầng Serverless**: Lambda cho dự đoán, truy xuất và thu thập; API Gateway cho các yêu cầu frontend; CloudFront/S3 để phân phối nội dung.
*   **Bảo Mật**: Secrets Manager cho khóa API, IAM cho quyền truy cập tối thiểu.
*   **Giám Sát**: CloudWatch để ghi log và các số liệu Lambda Insights.

### 5. Tiến Độ & Các Mốc Quan Trọng

**Dòng Thời Gian Dự Án**

* **Trước Kỳ Thực Tập (Tuần 1)**: Lập kế hoạch, nghiên cứu các API thời tiết bên ngoài và chuẩn bị mô hình ML.
* **Trong Kỳ Thực Tập (Tuần 1-8)**:

  * Tuần 1-2: Nghiên cứu AWS, thiết kế kiến trúc và ước tính chi phí.
  * Tuần 2-4: Tối ưu hóa kiến trúc, cấu hình quy trình không máy chủ và tích hợp các mô hình ML.
  * Tuần 4-8: Triển khai các hàm Lambda, thiết lập frontend, kiểm thử hệ thống và triển khai lên môi trường production.
* **Sau Khi Ra Mắt**: Thu thập dữ liệu liên tục và giám sát trong tối đa 1 năm.

### 6. Ước Tính Ngân Sách

**Khu vực:** ap-southeast-1 (Singapore)

Chi phí ước tính hàng tháng để vận hành nền tảng dự đoán bão trên AWS như sau:

**A. Frontend & Phân Phối Nội Dung**

* **Amazon S3 (Tệp Tĩnh):** Lưu trữ 5 GB tệp frontend (HTML, CSS, JS) và xử lý 10 GB chuyển dữ liệu mỗi tháng. Chi phí ≈ **$0.54/tháng**.
* **Amazon CloudFront:** Xử lý 50 GB chuyển dữ liệu và lên đến 1 triệu yêu cầu (trong phạm vi miễn phí). Chi phí ≈ **$6.00/tháng**.
* **Amazon Route 53:** 1 hosted zone và 1 triệu truy vấn DNS mỗi tháng. Chi phí ≈ **$0.90/tháng**.
* **AWS Certificate Manager (ACM):** Cung cấp chứng chỉ TLS cho truy cập HTTPS bảo mật. Miễn phí.

**Tổng cho Frontend & CDN:** ≈ **$7.4/tháng**

**B. Backend (Xử Lý API & ML)**

*   **Amazon API Gateway:** Xử lý 1 triệu yêu cầu HTTP API mỗi tháng, mỗi yêu cầu có kích thước khoảng 1 MB. Chi phí ≈ **$2.5/tháng**.
*   **Lambda (Dự đoán Bão):** Dự đoán quỹ đạo bão sử dụng các mô hình ML. Chạy ~1,000 lần mỗi ngày với 512 MB bộ nhớ được cấp phát và 1 GB bộ nhớ tạm thời. Mỗi lần thực thi kéo dài ~5 giây. Chi phí ≈ **$2.54/tháng**.
*   **Lambda (Lấy Dữ Liệu Bão Gần Đây):** Lấy dữ liệu bão từ S3 cho frontend. Chạy ~20,000 lần mỗi ngày với 512 MB bộ nhớ và 512 MB bộ nhớ tạm thời trong 1 giây mỗi lần thực thi. Chi phí ≈ **$0.00/tháng** (nằm trong free tier).

**Tổng cho Backend:** ≈ **$4.54/tháng**

**C. Tự Động Hóa & Thu Thập Dữ Liệu**

*   **Amazon EventBridge:** Lập lịch thu thập dữ liệu bão hàng tuần (1 cron trigger mỗi ngày). Miễn phí theo AWS free tier.
*   **Lambda (Trình Thu Thập Web):** Lấy dữ liệu từ các API bên ngoài hàng tuần. Sử dụng 128 MB bộ nhớ và 512 MB bộ nhớ tạm thời, ~30 giây mỗi lần thực thi. Chi phí ≈ **$0.00/tháng** (free tier).
*   **AWS Secrets Manager:** Lưu trữ 5 khóa API để truy cập an toàn vào các dịch vụ thời tiết bên ngoài. Chi phí ≈ **$2.00/tháng**.

**Tổng cho Tự Động Hóa & Thu Thập Dữ Liệu:** ≈ **$2.0/tháng**

**D. Giám Sát & Ghi Log**

*   **Amazon CloudWatch Logs:** Thu thập logs từ tất cả các hàm Lambda và chuyển đến S3 với thời gian lưu giữ 1 tháng. Khoảng 2 GB logs mỗi tháng. Chi phí ≈ **$0.57/tháng**.
*   **CloudWatch Metrics (Lambda Insights):** Giám sát 8 metrics trên các hàm Lambda. 10 metrics đầu tiên miễn phí, và chỉ ghi lại cho dự đoán và dữ liệu thu thập. Chi phí ≈ **$0.00/tháng**.

**Tổng cho Giám Sát & Ghi Log:** ≈ **$0.57/tháng**

**E. Lưu Trữ & Truyền Dữ Liệu**

*   **S3 (Bucket Mô Hình):** Lưu trữ các mô hình ML (~1 GB) và xử lý ~60,000 yêu cầu GET mỗi tháng. Chi phí ≈ **$0.05/tháng**.
*   **S3 (Bucket Dữ Liệu Bão Gần Đây):** Lưu trữ dữ liệu bão gần đây (~1 GB) với ~60,000 yêu cầu GET và 30 yêu cầu PUT mỗi tháng. Chi phí ≈ **$0.27/tháng**.

**Tổng cho Lưu Trữ & Truyền Dữ Liệu:** ≈ **$0.32/tháng**

**F. Di chuyển lên AWS**

* Dùng AWS CodePipeline và CodeBuild cho 10 phút chỉnh sửa mỗi tháng ≈ $0.90/month.

**Tổng Chi Phí Hàng Tháng Ước Tính**

* Frontend & CDN: $7.4
* Backend (API + ML): $4.54
* Tự động hóa (Crawler + Secrets): $2.0
* Giám sát & Ghi log: $0.57
* Lưu trữ & Truyền dữ liệu: $0.32
* Phí di chuyển kỹ thuật : $0.90

**TỔNG CỘNG ≈ $15.73/tháng**

### 7. Đánh Giá Rủi Ro

**Các Rủi Ro Chính**

* **Sự cố Mạng**: Mức độ ảnh hưởng trung bình, khả năng xảy ra trung bình.
* **Nguồn Dữ liệu Không Khả Dụng**: Mức độ ảnh hưởng trung bình, khả năng xảy ra thấp.
* **Lỗi Mô Hình ML**: Mức độ ảnh hưởng cao, khả năng xảy ra thấp.
* **Vượt Quá Chi Phí**: Mức độ ảnh hưởng trung bình, khả năng xảy ra thấp.

**Chiến Lược Giảm Thiểu**

* **Mạng**: Lưu vào bộ nhớ đệm (cache) dữ liệu bão gần đây trong S3 để cho phép hiển thị frontend trong thời gian xảy ra sự cố.
* **Nguồn Dữ liệu**: Lưu trữ dữ liệu bão lịch sử để dự phòng.
* **Mô Hình ML**: Xác thực và kiểm tra mô hình thường xuyên.
* **Chi Phí**: Giám sát mức sử dụng AWS và thiết lập cảnh báo ngân sách.

**Kế Hoạch Dự Phòng**

* Chuyển sang cập nhật thủ công nếu API bên ngoài thất bại.
* Khôi phục (rollback) về mô hình ML trước đó bằng cách sử dụng tính năng versioning của S3 nếu mô hình mới thất bại.

### 8. Kết Quả Kỳ Vọng

**Cải Tiến Kỹ Thuật:**

* Dự đoán quỹ đạo bão thời gian thực với các đường đi được hiển thị hóa.
* Hệ thống không máy chủ có khả năng mở rộng, xử lý hàng nghìn yêu cầu/ngày.

**Giá Trị Lâu Dài:**

* Dữ liệu bão tập trung cho nghiên cứu và phân tích.
* Khung (framework) có thể tái sử dụng cho các tác vụ dự đoán không gian địa lý khác.
* Chi phí vận hành hàng tháng thấp (< $20/tháng).
