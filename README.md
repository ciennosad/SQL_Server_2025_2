# SQL_Server_2025_Bài_tập_2
**Tên Sinh Viên**: Dương Đình Kiền

**Lớp**: K59KMT

**Mã SV**: K235480106109

**Chủ đề**: Quản Lý Thư Viện

## Phần 1: THIẾT KẾ VÀ KHỞI TẠO CƠ SỞ DỮ LIỆU
### 1.1 Tạo Database
<img width="1912" height="1079" alt="image" src="https://github.com/user-attachments/assets/99bb6f75-a38b-4947-8138-2aa2742619a1" />

## 1.2 Tạo 3 bảng có quan hệ với nhau
<img width="1917" height="1072" alt="image" src="https://github.com/user-attachments/assets/840fb15e-0c10-467f-8259-55240565558d" />

## Phần 2: XÂY DỰNG FUNCTION

## 2.1 Hãy cho biết trong SQL Server có những loại function bild_in (hàm có sẵn):
<img width="1915" height="1070" alt="image" src="https://github.com/user-attachments/assets/c9ade0a4-8f90-4200-8a9f-b5350e6b357e" />

## 2.2 Hàm do người dùng tự viết trong SQL thường mang mục đích gì? 

- Hàm do người dùng tự viết trong SQL (UDF) chủ yếu được tạo ra để đóng gói các đoạn tính toán hoặc xử lý dữ liệu phức tạp thành một khối riêng, giúp bạn gọi lại nhiều lần trong nhiều truy vấn khác nhau mà không cần viết lặp lại. Nhờ đó, câu SQL trở nên ngắn gọn, dễ đọc và dễ bảo trì hơn. Hàm này cũng giúp tập trung các quy tắc nghiệp vụ (như tính thuế, chuẩn hóa mã, kiểm tra điều kiện) vào một nơi duy nhất, đảm bảo kết quả luôn nhất quán khi áp dụng ở nhiều vị trí. Ngoài ra, UDF cho phép bạn mở rộng khả năng xử lý của SQL khi các hàm có sẵn không đáp ứng đủ nhu cầu thực tế. Lưu ý: hàm chỉ nên dùng để tính toán và trả kết quả, không dùng để trực tiếp thay đổi dữ liệu trong bảng, và cần kiểm tra hiệu năng khi chạy trên tập dữ liệu lớn.

## Nó có những loại nào? 

- Hàm do người dùng tự viết trong SQL thường được chia thành ba loại chính. Đầu tiên là **hàm vô hướng (Scalar)**, nhận dữ liệu đầu vào và trả về duy nhất một giá trị, thích hợp để tính toán hoặc chuẩn hóa thông tin cho từng dòng. Thứ hai là **hàm trả về bảng (Table-Valued)**, cho kết quả là một tập dữ liệu dạng bảng có thể dùng trực tiếp trong mệnh đề `FROM` hoặc `JOIN`; loại này gồm dạng inline (chạy nhanh, viết gọn bằng một câu `SELECT`) và dạng nhiều câu lệnh (xử lý logic phức tạp hơn). Cuối cùng là **hàm tổng hợp (Aggregate)**, dùng để thống kê trên nhiều dòng và trả về một giá trị duy nhất, tuy ít phổ biến và khả năng hỗ trợ tùy thuộc vào từng hệ quản trị cơ sở dữ liệu.

## Mỗi loại thường được dùng khi nào? 

- Hàm vô hướng thường được dùng khi bạn cần tính toán hoặc chuẩn hóa một giá trị duy nhất cho từng dòng dữ liệu, ví dụ như tính tuổi từ ngày sinh, áp dụng công thức thuế cho từng hóa đơn, hay định dạng lại mã nội bộ. Vì trả về kết quả đơn lẻ, nó rất tiện để lồng trực tiếp vào mệnh đề `SELECT`, `WHERE` hoặc các biểu thức điều kiện.
- Hàm trả về bảng phù hợp khi bạn cần đóng gói một tập hợp nhiều dòng dữ liệu để dùng như một bảng ảo trong truy vấn chính, chẳng hạn lấy danh sách đơn hàng đang xử lý của một khách hàng hoặc tổng hợp doanh thu theo tháng để `JOIN` với bảng khác. Dạng inline thích hợp cho logic đơn giản, tốc độ cao, còn dạng nhiều câu lệnh dùng khi cần khai báo biến, xử lý trung gian hoặc điều kiện rẽ nhánh phức tạp.
- Hàm tổng hợp thường được dùng khi bạn cần gom nhiều dòng thành một giá trị thống kê tùy chỉnh mà hàm hệ thống không hỗ trợ, như tính trung bình có trọng số, tìm giá trị xuất hiện nhiều nhất, hay nối chuỗi có điều kiện theo nhóm; loại này chủ yếu phục vụ báo cáo và phân tích, thường đi kèm với `GROUP BY`. Nói ngắn gọn, bạn chọn hàm vô hướng để xử lý từng giá trị đơn lẻ, hàm trả về bảng khi cần một tập dữ liệu để ghép vào truy vấn, và hàm tổng hợp khi cần thống kê nhóm theo logic nghiệp vụ riêng.
  
## Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?

- Dù SQL đã cung cấp sẵn nhiều hàm hệ thống, việc tự viết hàm riêng vẫn cần thiết vì những lý do thực tế sau. Hàm hệ thống chỉ xử lý các thao tác cơ bản và chung chung (cộng trừ, cắt chuỗi, định dạng ngày...), trong khi mỗi doanh nghiệp lại có quy tắc nghiệp vụ đặc thù như tính thưởng, xếp hạng khách hàng hay chuẩn hóa mã nội bộ mà hàm có sẵn không đáp ứng được. Khi cùng một công thức phức tạp phải dùng lại ở nhiều báo cáo hoặc truy vấn, hàm riêng giúp bạn viết một lần và gọi lại nhiều lần, nhờ đó việc chỉnh sửa chỉ cần thực hiện tại một nơi duy nhất, giảm thiểu sai sót. Nó còn giúp đóng gói các biểu thức dài dòng thành một tên gọi dễ hiểu, khiến câu SQL ngắn gọn, rõ nghĩa và dễ bảo trì hơn. Quan trọng nhất, hàm tự viết đảm bảo tính nhất quán trong team: mọi người đều gọi chung một logic chuẩn, tránh trường hợp mỗi người tự tính theo cách khác nhau dẫn đến kết quả lệch nhau. Nói ngắn gọn, hàm hệ thống lo phần nền tảng, còn hàm tự viết là công cụ để “khâu” nghiệp vụ riêng vào hệ thống một cách gọn gàng, chuẩn xác và dễ quản lý.

## 2.3 Scalar Function: Tính tiền phạt (trễ hạn)
<img width="1918" height="1074" alt="image" src="https://github.com/user-attachments/assets/2fafedea-b1e2-4e19-b1c7-df2f230c1395" />

## 2.4 Table-Valued Function: Sách còn mượn được theo thể loại
<img width="1915" height="1079" alt="image" src="https://github.com/user-attachments/assets/e803f1f7-8a5c-4234-8367-cfd9783b259b" />

## 2.5: Multi-statement Table-Valued Function: Thống kế mượn sách độc giả\
<img width="1918" height="1073" alt="image" src="https://github.com/user-attachments/assets/2fc1cd1c-8b8f-4ec8-8597-b1bb024837ef" />

## 
















