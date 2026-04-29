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

## Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?
