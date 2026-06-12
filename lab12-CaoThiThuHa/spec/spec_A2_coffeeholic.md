# Product Spec: Coffeeholic

*Tài liệu đặc tả sản phẩm Day 06 của nhóm Coffeeholic*

## 1. Thông tin nhóm

| MSSV | Họ và tên |
| --- | --- |
| 2A202600765 | Cao Thị Thu Hà |
| 2A202600709 | Hà Trung Kiên |
| 2A202600873 | Nguyễn Lâm Phương Thảo |
| 2A202600689 | Nguyễn Bình Huy |

## 2. Tóm tắt sản phẩm

**Coffeeholic** là công cụ gợi ý quán cafe theo phong cách không gian thay vì chỉ dựa trên từ khóa văn bản. Người dùng chọn những hình ảnh quán cafe có "vibe" đúng gu của mình, sau đó hệ thống dùng AI để tìm ra các quán có không gian tương đồng nhất.

### Mục tiêu của lát cắt MVP

- Hiển thị bộ ảnh đại diện ban đầu để người dùng chọn nhanh.
- Nhận từ 1 đến 2 lựa chọn của người dùng.
- Tính embedding trung bình và đo độ tương đồng bằng `Cosine Similarity`.
- Trả về 3 quán cafe phù hợp nhất với phong cách đã chọn.

## 3. Bằng chứng vấn đề (Evidence)

Nhu cầu giải quyết bài toán này xuất phát từ quan sát thực tế và hành vi rõ ràng của nhóm người dùng mục tiêu.

- **Trải nghiệm tìm quán hiện tại chưa tốt:** Khi người dùng tìm trên bản đồ với các từ khóa như "yên tĩnh", "cổ điển", "ấm cúng", hệ thống thường ưu tiên keyword hơn là cảm nhận thẩm mỹ. Kết quả trả về vì thế dễ lệch với vibe mà người dùng thực sự muốn.
- **Người dùng phải tự ghép nhiều nguồn thông tin:** Nhiều bạn phải lưu ảnh trên TikTok, Instagram hoặc Facebook, sau đó mới đi dò địa chỉ trên Google Maps. Quy trình này mất thời gian và làm đứt mạch trải nghiệm.
- **Nhu cầu tìm quán theo phong cách là có thật:** Các nội dung như "review quán cafe", "quán cafe Hà Nội", "quán cafe Sài Gòn" thường xuyên đạt lượng tương tác cao trên TikTok và Facebook. Điều đó cho thấy người dùng quan tâm mạnh tới trải nghiệm không gian, nhưng công cụ tìm kiếm hiện tại chưa hỗ trợ họ diễn đạt nhu cầu này đủ tốt.

## 4. Lát cắt để build (Build Slice)

Prototype tập trung vào một bài toán hẹp nhưng có giá trị rõ ràng:

- Người dùng muốn tìm quán cafe theo phong cách không gian.
- Hệ thống hiển thị các ảnh đại diện ban đầu để xử lý bài toán cold start.
- Sau khi người dùng chọn ảnh phù hợp, AI tính vector đại diện cho sở thích đó.
- Hệ thống gợi ý ra 3 quán có độ tương đồng cao nhất về hình ảnh và không gian.
- Trong phạm vi MVP, tập dữ liệu là tập tĩnh đã được kiểm duyệt thủ công để tăng độ tin cậy cho demo.

## 5. AI Product Canvas

| Trục | Nội dung |
| --- | --- |
| **Value** | Sản phẩm giúp người dùng tìm quán cafe theo thẩm mỹ không gian mà không cần mô tả dài hoặc tìm đúng từ khóa. Trải nghiệm chính là chọn bằng mắt, thay vì tìm bằng văn bản. |
| **Trust** | Nếu AI gợi ý chưa đúng, người dùng có thể nhận ra ngay qua hình ảnh và dùng nút `Kết quả không phù hợp` để yêu cầu làm lại. Độ tin cậy còn được tăng nhờ dữ liệu đầu vào đã qua kiểm duyệt thủ công. |
| **Feasibility** | MVP khả thi trong thời gian ngắn vì chỉ dùng tập dữ liệu tĩnh khoảng 20 đến 40 quán, embedding được xử lý trước, còn runtime chỉ cần truy xuất và tính `Cosine Similarity`. |
| **Learning Signal** | Các tín hiệu như `Kết quả không phù hợp`, hành vi chọn seed, hoặc hành vi mở chỉ đường có thể được lưu lại để cải thiện logic gợi ý ở các phiên bản sau. |

## 6. Augment hay Automate?

Sản phẩm đi theo hướng **Augmentation**.

- AI làm phần khó: hiểu sở thích thị giác, xử lý dữ liệu và rút ra danh sách gợi ý tốt nhất.
- Người dùng vẫn giữ quyền quyết định cuối cùng: xem quán, cân nhắc khoảng cách và chọn nơi phù hợp.

### Lý do chọn hướng này

Sở thích về thẩm mỹ là yếu tố chủ quan. Nếu tự động hóa hoàn toàn, hệ thống rất dễ đưa ra lựa chọn không đúng gu và làm người dùng mất niềm tin. Hướng tăng năng lực phù hợp hơn vì AI đóng vai trò hỗ trợ khám phá, không thay thế quyết định cá nhân.

## 7. Bốn đường đi của trải nghiệm

### Happy Path

- Người dùng chọn 1 đến 2 ảnh đại diện đúng gu.
- Hệ thống tính độ tương đồng và trả về 3 quán có không gian gần nhất.
- Người dùng xem thông tin quán, lộ trình và ra quyết định.

### Khi AI không chắc

- Người dùng chọn các ảnh có phong cách quá đối lập.
- Hệ thống phát hiện tín hiệu nhiễu và hiển thị thông báo như: *"Rất khó để gợi ý chính xác không gian theo lựa chọn này."*
- Người dùng được yêu cầu chọn lại hoặc thu hẹp sở thích.

### Khi AI sai

- Kết quả trả về không đúng vibe người dùng mong muốn.
- Người dùng dùng nút `Kết quả không phù hợp` để loại batch gợi ý hiện tại.

### Khi người dùng sửa

- Hệ thống reset lại luồng chọn ảnh ban đầu.
- Đồng thời lưu log về lựa chọn trước đó để phục vụ phân tích sai lệch và cải tiến thuật toán.

## 8. Những kiểu lỗi đáng lo nhất

### Lỗi 1. Vector Cancellation làm giảm độ chính xác

- **Nguyên nhân:** Người dùng chọn các hình ảnh mang phong cách hoàn toàn trái ngược nhau.
- **Hậu quả:** Vector trung bình bị triệt tiêu, kết quả gợi ý trở nên thiếu nhất quán.
- **Cách xử lý:** Giới hạn số lượng ảnh được chọn, đặt ngưỡng `Cosine Similarity`, và từ chối trả kết quả nếu độ chắc chắn quá thấp.

### Lỗi 2. Sai lệch giữa hình ảnh và thực tế

- **Nguyên nhân:** Ảnh quán đã chỉnh sửa quá mạnh hoặc chất lượng không gian ngoài đời đã thay đổi.
- **Hậu quả:** Người dùng đến nơi nhưng trải nghiệm không khớp kỳ vọng, làm giảm niềm tin vào sản phẩm.
- **Cách xử lý:** Dùng tập dữ liệu tĩnh đã được đội ngũ kiểm duyệt thủ công trong giai đoạn MVP để đảm bảo ảnh và thực tế bám sát nhau nhất có thể.

## 9. Kế hoạch kiểm thử và bằng chứng demo

### Kịch bản tiêu chuẩn

- **Đầu vào:** Người dùng chọn 1 ảnh quán có phong cách cổ điển hoặc vintage.
- **Kỳ vọng:** Hệ thống trả về 3 quán có thiết kế và ánh sáng tương đồng.

### Kịch bản góc cạnh

- **Đầu vào:** Người dùng chọn đồng thời 1 ảnh dark-academia và 1 ảnh không gian mở, sáng mạnh.
- **Kỳ vọng:** Hệ thống nhận diện lựa chọn thiếu nhất quán và yêu cầu người dùng chọn lại thay vì cố trả ra kết quả sai.

### Bằng chứng chuẩn bị cho demo

- Bộ dữ liệu tĩnh ở định dạng `CSV/JSON`.
- Mã nguồn tính toán embedding và `Cosine Similarity`.
- Video hoặc quy trình chạy thử sẵn cho cả happy case và corner case để giảm rủi ro khi trình bày.

## 10. Phân công

| Thành viên | Phụ trách |
| --- | --- |
| **Huy** | Quản lý tài liệu chuyên môn, Product Canvas, sơ đồ UX Flow, xây dựng nội dung slide và thuyết trình. |
| **Hà** | Thiết kế frontend và kiểm thử hệ thống. |
| **Kiên** | Chuẩn bị dataset tĩnh khoảng 30 quán, gắn nhãn hình ảnh và xuất vector embedding. |
| **Thảo** | Phát triển backend, xây dựng logic vector trung bình, `Cosine Similarity` và API trả kết quả. |
