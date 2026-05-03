# SQL_Server_2025_Bài_tập_2
**Tên Sinh Viên**: Dương Đình Kiền

**Lớp**: K59KMT

**Mã SV**: K235480106109

**Chủ đề**: Quản Lý Thư Viện

## Giới Thiệu Chung

Bài tập này nhằm rèn luyện kỹ năng lập trình cơ sở dữ liệu trên SQL Server thông qua 5 nội dung chính: thiết kế database, xây dựng function, store procedure, trigger và cursor. Tài liệu này trình bày logic cách làm từng phần để bạn hiểu bản chất và áp dụng linh hoạt.

## Phần 1: THIẾT KẾ VÀ KHỞI TẠO CƠ SỞ DỮ LIỆU

## Mục Tiêu

- Tạo Database có tên Quanlythuvien_k235480106109
- 3 bảng gồm: Sach, DocGia, MuonTra (DocGia là người mượn sách)
- Thiết kế 3 bảng có quan hệ với nhau có đầy đủ thuộc tính: Primary Key, Foreign Key, Check Cóntraint, Dèault Value


### 1.1 Tạo Database



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6287b01b-4124-428a-a368-70f228467ffc" />


Hình ảnh tạo **Database** thành công

## 1.2 Tạo 3 bảng có quan hệ với nhau

- Bảng Sach: MaSach (PK), TenSach, TacGia, NamXuatBan (CHECK 1900-2100), SoLuong (CHECK >=0), GiaTien

```

   CREATE TABLE [Sach] (
    [MaSach] INT PRIMARY KEY,                        
    [TenSach] NVARCHAR(255) NOT NULL,                  
    [TacGia] NVARCHAR(100) NOT NULL,
    [NhaXuatBan] NVARCHAR(150),
    [NamXuatBan] INT CHECK ([NamXuatBan] BETWEEN 1900 AND 2100),  
    [SoLuong] INT NOT NULL CHECK ([SoLuong] >= 0),     
    [GiaTien] DECIMAL(12,3) CHECK ([GiaTien] >= 0),    
    [TheLoai] NVARCHAR(50),
    [NgayNhapKho] DATETIME DEFAULT GETDATE(),         
    [ConSauKhong] BIT DEFAULT 1                        
);
GO

```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/20257b4b-6682-41f0-baab-262a53360f0d" />


                         Hình ảnh đã tạo thàn công bảng sách

## Bảng DocGia: Bảng DocGia: MaDocGia (PK), HoTen, NgaySinh, Email (UNIQUE), NgayDangKy (DEFAULT GETDATE()), TinhTrang.

```

    CREATE TABLE [DocGia] (
    [MaDocGia] INT PRIMARY KEY,                       
    [HoTen] NVARCHAR(100) NOT NULL,
    [NgaySinh] DATE CHECK ([NgaySinh] <= GETDATE()),   
    [GioiTinh] NVARCHAR(10) CHECK ([GioiTinh] IN (N'Nam', N'Nữ', N'Khác')), 
    [SoDienThoai] VARCHAR(15) UNIQUE,                 
    [Email] NVARCHAR(100) UNIQUE,
    [DiaChi] NVARCHAR(255),
    [NgayDangKy] DATETIME DEFAULT GETDATE(),
    [TinhTrang] BIT DEFAULT 1                          
);
GO 

```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89d25350-187c-419a-8cb0-815a848e51f2" />

                                Đã tạo xong bảng thông tin người mượn sách

- Bảng MuonTra: MaMuon (PK, IDENTITY), MaDocGia (FK), MaSach (FK), NgayMuon, HanTra, NgayTra, TinhTrang (CHECK giá trị hợp lệ)

```
    CREATE TABLE [MuonTra] (
    [MaMuon] INT PRIMARY KEY,                          
    [MaDocGia] INT NOT NULL,                          
    [MaSach] INT NOT NULL,                             
    [NgayMuon] DATETIME DEFAULT GETDATE() NOT NULL,
    [HanTra] DATETIME NOT NULL,
    [NgayTraThucTe] DATETIME,                          
    [TinhTrangSachKhiMuon] NVARCHAR(50) DEFAULT N'Tốt',
    [TinhTrangSachKhiTra] NVARCHAR(50),
    [TienPhat] DECIMAL(10,3) DEFAULT 0 CHECK ([TienPhat] >= 0),  
    [GhiChu] NVARCHAR(500),


```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a08c3553-e4d3-4503-83bf-1e16c38758c1" />

                    Đã tạo xong bảng mượn trả trong bảng lưu giữ thông tin của người dùng ngày mượn và ngày trả sách

```

CREATE TABLE [Sach] (
    [MaSach] INT PRIMARY KEY,                        
    [TenSach] NVARCHAR(255) NOT NULL,                  
    [TacGia] NVARCHAR(100) NOT NULL,
    [NhaXuatBan] NVARCHAR(150),
    [NamXuatBan] INT CHECK ([NamXuatBan] BETWEEN 1900 AND 2100),  
    [SoLuong] INT NOT NULL CHECK ([SoLuong] >= 0),     
    [GiaTien] DECIMAL(12,3) CHECK ([GiaTien] >= 0),    
    [TheLoai] NVARCHAR(50),
    [NgayNhapKho] DATETIME DEFAULT GETDATE(),         
    [ConSauKhong] BIT DEFAULT 1                        
);
GO

CREATE TABLE [DocGia] (
    [MaDocGia] INT PRIMARY KEY,                       
    [HoTen] NVARCHAR(100) NOT NULL,
    [NgaySinh] DATE CHECK ([NgaySinh] <= GETDATE()),   
    [GioiTinh] NVARCHAR(10) CHECK ([GioiTinh] IN (N'Nam', N'Nữ', N'Khác')), 
    [SoDienThoai] VARCHAR(15) UNIQUE,                 
    [Email] NVARCHAR(100) UNIQUE,
    [DiaChi] NVARCHAR(255),
    [NgayDangKy] DATETIME DEFAULT GETDATE(),
    [TinhTrang] BIT DEFAULT 1                          
);
GO


CREATE TABLE [MuonTra] (
    [MaMuon] INT PRIMARY KEY,                          
    [MaDocGia] INT NOT NULL,                          
    [MaSach] INT NOT NULL,                             
    [NgayMuon] DATETIME DEFAULT GETDATE() NOT NULL,
    [HanTra] DATETIME NOT NULL,
    [NgayTraThucTe] DATETIME,                          
    [TinhTrangSachKhiMuon] NVARCHAR(50) DEFAULT N'Tốt',
    [TinhTrangSachKhiTra] NVARCHAR(50),
    [TienPhat] DECIMAL(10,3) DEFAULT 0 CHECK ([TienPhat] >= 0),  
    [GhiChu] NVARCHAR(500),

    
    CONSTRAINT [FK_MuonTra_DocGia] 
        FOREIGN KEY ([MaDocGia]) REFERENCES [DocGia]([MaDocGia])
        ON UPDATE CASCADE ON DELETE NO ACTION,         
    
    CONSTRAINT [FK_MuonTra_Sach] 
        FOREIGN KEY ([MaSach]) REFERENCES [Sach]([MaSach])
        ON UPDATE CASCADE ON DELETE NO ACTION,

   
    CONSTRAINT [CK_NgayTra_Sau_NgayMuon] 
        CHECK ([NgayTraThucTe] IS NULL OR [NgayTraThucTe] >= [NgayMuon]),

  
    CONSTRAINT [CK_HanTra_Sau_NgayMuon] 
        CHECK ([HanTra] >= [NgayMuon])
);
GO 


```

<img width="1916" height="1077" alt="Screenshot 2026-04-29 220101" src="https://github.com/user-attachments/assets/55c38e1a-b54f-4c4f-8c6c-dafaa7c6352e" />

                                              Hình ảnh này là kết quả chạy thành công 3 bảng 


## Phần 2: XÂY DỰNG FUNCTION

## 2.1 Hãy cho biết trong SQL Server có những loại function bild_in (hàm có sẵn):

```

-- Một số System Functions:


-- 1. Hàm ngày tháng: GETDATE(), DATEADD(), DATEDIFF()
SELECT GETDATE() AS [ThoiGianHienTai],
       DATEADD(DAY, 14, GETDATE()) AS [HanTraSau14Ngay],
       DATEDIFF(DAY, '2024-01-01', GETDATE()) AS [SoNgayDaQua];

-- 2. Hàm xử lý chuỗi: CONCAT(), FORMAT(), LEN()
SELECT CONCAT(N'Độc giả: ', [HoTen]) AS [ThongTin],
       FORMAT([NgayDangKy], 'dd/MM/yyyy') AS [NgayDK_Format],
       LEN([Email]) AS [DoDaiEmail]
FROM [DocGia];

-- 3. Hàm chuyển đổi: CAST(), CONVERT(), ISNULL()
SELECT CAST([GiaTien] AS VARCHAR(50)) + N' VNĐ' AS [GiaHienThi],
       ISNULL(CONVERT(NVARCHAR(50),[NgayTraThucTe],103), N'Chưa trả') AS [TrangThaiTra]
FROM [Sach] s LEFT JOIN [MuonTra] mt ON s.[MaSach] = mt.[MaSach];

-- 4. Hàm hệ thống: SYSTEM_USER, DB_NAME(), OBJECT_ID()
SELECT SYSTEM_USER AS [NguoiDung],
       DB_NAME() AS [TenDatabase],
       OBJECT_ID(N'[Sach]') AS [ID_BangSach];


```

<img width="1915" height="1070" alt="image" src="https://github.com/user-attachments/assets/c9ade0a4-8f90-4200-8a9f-b5350e6b357e" />

Hình ảnh cho thấy những hàm function

## 2.2 Hàm do người dùng tự viết trong SQL thường mang mục đích gì? 

- Hàm do người dùng tự viết trong SQL (UDF) chủ yếu được tạo ra để đóng gói các đoạn tính toán hoặc xử lý dữ liệu phức tạp thành một khối riêng, giúp bạn gọi lại nhiều lần trong nhiều truy vấn khác nhau mà không cần viết lặp lại. Nhờ đó, câu SQL trở nên ngắn gọn, dễ đọc và dễ bảo trì hơn. Hàm này cũng giúp tập trung các quy tắc nghiệp vụ (như tính thuế, chuẩn hóa mã, kiểm tra điều kiện) vào một nơi duy nhất, đảm bảo kết quả luôn nhất quán khi áp dụng ở nhiều vị trí. Ngoài ra, UDF cho phép bạn mở rộng khả năng xử lý của SQL khi các hàm có sẵn không đáp ứng đủ nhu cầu thực tế. Lưu ý: hàm chỉ nên dùng để tính toán và trả kết quả, không dùng để trực tiếp thay đổi dữ liệu trong bảng, và cần kiểm tra hiệu năng khi chạy trên tập dữ liệu lớn.

## Nó có những loại nào? 

- Hàm do người dùng tự viết trong SQL thường được chia thành ba loại chính:
- Đầu tiên là **hàm vô hướng (Scalar)**, nhận dữ liệu đầu vào và trả về duy nhất một giá trị, thích hợp để tính toán hoặc chuẩn hóa thông tin cho từng dòng.
- Thứ hai là **hàm trả về bảng (Table-Valued)**, cho kết quả là một tập dữ liệu dạng bảng có thể dùng trực tiếp trong mệnh đề `FROM` hoặc `JOIN`; loại này gồm dạng inline (chạy nhanh, viết gọn bằng một câu `SELECT`) và dạng nhiều câu lệnh (xử lý logic phức tạp hơn).
- Cuối cùng là **hàm tổng hợp (Aggregate)**, dùng để thống kê trên nhiều dòng và trả về một giá trị duy nhất, tuy ít phổ biến và khả năng hỗ trợ tùy thuộc vào từng hệ quản trị cơ sở dữ liệu.

## Mỗi loại thường được dùng khi nào? 

- Hàm vô hướng thường được dùng khi bạn cần tính toán hoặc chuẩn hóa một giá trị duy nhất cho từng dòng dữ liệu, ví dụ như tính tuổi từ ngày sinh, áp dụng công thức thuế cho từng hóa đơn, hay định dạng lại mã nội bộ. Vì trả về kết quả đơn lẻ, nó rất tiện để lồng trực tiếp vào mệnh đề `SELECT`, `WHERE` hoặc các biểu thức điều kiện.
- Hàm trả về bảng phù hợp khi bạn cần đóng gói một tập hợp nhiều dòng dữ liệu để dùng như một bảng ảo trong truy vấn chính, chẳng hạn lấy danh sách đơn hàng đang xử lý của một khách hàng hoặc tổng hợp doanh thu theo tháng để `JOIN` với bảng khác. Dạng inline thích hợp cho logic đơn giản, tốc độ cao, còn dạng nhiều câu lệnh dùng khi cần khai báo biến, xử lý trung gian hoặc điều kiện rẽ nhánh phức tạp.
- Hàm tổng hợp thường được dùng khi bạn cần gom nhiều dòng thành một giá trị thống kê tùy chỉnh mà hàm hệ thống không hỗ trợ, như tính trung bình có trọng số, tìm giá trị xuất hiện nhiều nhất, hay nối chuỗi có điều kiện theo nhóm; loại này chủ yếu phục vụ báo cáo và phân tích, thường đi kèm với `GROUP BY`. Nói ngắn gọn, bạn chọn hàm vô hướng để xử lý từng giá trị đơn lẻ, hàm trả về bảng khi cần một tập dữ liệu để ghép vào truy vấn, và hàm tổng hợp khi cần thống kê nhóm theo logic nghiệp vụ riêng.
  
## Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?

- Dù SQL đã cung cấp sẵn nhiều hàm hệ thống, việc tự viết hàm riêng vẫn cần thiết vì những lý do thực tế sau. Hàm hệ thống chỉ xử lý các thao tác cơ bản và chung chung (cộng trừ, cắt chuỗi, định dạng ngày...), trong khi mỗi doanh nghiệp lại có quy tắc nghiệp vụ đặc thù như tính thưởng, xếp hạng khách hàng hay chuẩn hóa mã nội bộ mà hàm có sẵn không đáp ứng được. Khi cùng một công thức phức tạp phải dùng lại ở nhiều báo cáo hoặc truy vấn, hàm riêng giúp bạn viết một lần và gọi lại nhiều lần, nhờ đó việc chỉnh sửa chỉ cần thực hiện tại một nơi duy nhất, giảm thiểu sai sót. Nó còn giúp đóng gói các biểu thức dài dòng thành một tên gọi dễ hiểu, khiến câu SQL ngắn gọn, rõ nghĩa và dễ bảo trì hơn. Quan trọng nhất, hàm tự viết đảm bảo tính nhất quán trong team: mọi người đều gọi chung một logic chuẩn, tránh trường hợp mỗi người tự tính theo cách khác nhau dẫn đến kết quả lệch nhau. Nói ngắn gọn, hàm hệ thống lo phần nền tảng, còn hàm tự viết là công cụ để “khâu” nghiệp vụ riêng vào hệ thống một cách gọn gàng, chuẩn xác và dễ quản lý.

## 2.3 Scalar Function: Tính tiền phạt (trễ hạn)
- Trả về danh sách quyển sách đang được mượ bởi 1 độc giả mà chưa trả

```

IF OBJECT_ID(N'[dbo].[fn_SachDangMuon]', N'IF') IS NOT NULL
    DROP FUNCTION [dbo].[fn_SachDangMuon];
GO

CREATE FUNCTION [fn_SachDangMuon]
(
    @MaDocGia INT
)
RETURNS TABLE
AS
RETURN
(
    SELECT 
        mt.[MaMuon],
        s.[MaSach],
        s.[TenSach],
        s.[TacGia],
        mt.[NgayMuon],
        mt.[HanTra],
        DATEDIFF(DAY, mt.[HanTra], GETDATE()) AS [SoNgayQuaHan]
    FROM [MuonTra] mt
    INNER JOIN [Sach] s ON mt.[MaSach] = s.[MaSach]
    WHERE mt.[MaDocGia] = @MaDocGia 
      AND mt.[NgayTraThucTe] IS NULL
);
GO

-- 1. Thêm độc giả Nguyễn Văn A
INSERT INTO dbo.DocGia (MaDocGia, HoTen, NgaySinh,GioiTinh, SoDienThoai, Email, DiaChi)
VALUES (1003, N'Nguyễn Văn AA', N'2003-03-01','Nam', '0863957396',N'nguyenvana1@Gmail.com', N'Tích Lương-Thái Nguyên');

-- 2. Thêm sách 
INSERT INTO dbo.Sach (MaSach, TenSach,TacGia,NhaXuatBan,SoLuong,GiaTien,TheLoai,NgayNhapKho,ConSauKhong)
VALUES (1, N'Tuổi Trẻ Đáng Giá Bao Nhiêu',N'Rosie Nguyễn',N'Văn Học Việt Nam',8,'199.000', N'Sách Văn Học',N'2020-06-08',0);

-- 3. Thêm phiếu mượn
INSERT INTO dbo.MuonTra (MaMuon, MaDocGia, MaSach, NgayMuon, HanTra, NgayTraThucTe, TienPhat,GhiChu)
VALUES (2003, 1003, 1, '2026-04-01', '2026-04-15', NULL, 5,N'Hẹn Trả Trước');



--SELECT * FROM dbo.DocGia WHERE MaDocGia = 1003;
SELECT * FROM dbo.Sach WHERE MaSach = 1;
SELECT * FROM dbo.MuonTra WHERE MaMuon = 2003;



SELECT * FROM dbo.[fn_SachDangMuon](1003);


```


<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/93301e13-dc45-47bf-b637-6f878b7d5437" />


Hình ảnh cho thấy danh sách 1 độc giả  đang mượn chưa trả bị quá hạn trả

## 2.4 Table-Valued Function: Sách còn mượn được theo thể loại

- Thống kê chi tiết hoạt động mượn sách của tất cả độc giả:
- Tổng số lần mượn
- Số sách đang mượn
- Tổng tiền phạt


```

IF OBJECT_ID(N'[dbo].[fn_TinhSoNgayQuaHan]', N'FN') IS NOT NULL
    DROP FUNCTION [dbo].[fn_TinhSoNgayQuaHan];
GO

CREATE FUNCTION dbo.fn_TinhSoNgayQuaHan
(
    @MaMuon INT
)
RETURNS INT
AS
BEGIN
    DECLARE @NgayQuaHan INT;
    
    SELECT @NgayQuaHan = 
        CASE 
            WHEN NgayTraThucTe IS NULL AND GETDATE() > HanTra 
                THEN DATEDIFF(DAY, HanTra, GETDATE())
            WHEN NgayTraThucTe IS NOT NULL AND NgayTraThucTe > HanTra 
                THEN DATEDIFF(DAY, HanTra, NgayTraThucTe)
            ELSE 0 
        END
    FROM dbo.MuonTra
    WHERE MaMuon = @MaMuon;
    
    RETURN ISNULL(@NgayQuaHan, 0);
END;
GO

```



<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/337d0a55-87b5-46ca-ad04-47890a558d6d" />


Hình ảnh kết quả chạy thành công

```

-- 1. Thêm độc giả Nguyễn Văn A
INSERT INTO dbo.DocGia (MaDocGia, HoTen, NgaySinh,GioiTinh, SoDienThoai, Email, DiaChi)
VALUES (1003, N'Nguyễn Văn AA', N'2003-03-01','Nam', '0863957396',N'nguyenvana1@Gmail.com', N'Tích Lương-Thái Nguyên');

-- 2. Thêm sách 
INSERT INTO dbo.Sach (MaSach, TenSach,TacGia,NhaXuatBan,SoLuong,GiaTien,TheLoai,NgayNhapKho,ConSauKhong)
VALUES (1, N'Tuổi Trẻ Đáng Giá Bao Nhiêu',N'Rosie Nguyễn',N'Văn Học Việt Nam',8,'199.000', N'Sách Văn Học',N'2020-06-08',0);

-- 3. Thêm phiếu mượn
INSERT INTO dbo.MuonTra (MaMuon, MaDocGia, MaSach, NgayMuon, HanTra, NgayTraThucTe, TienPhat,GhiChu)
VALUES (2003, 1003, 1, '2026-04-01', '2026-04-15', NULL, 5,N'Hẹn Trả Trước');

-- Xem dữ liệu vừa insert
SELECT * FROM dbo.DocGia WHERE MaDocGia = 1003;
SELECT * FROM dbo.Sach WHERE MaSach = 1;
SELECT * FROM dbo.MuonTra WHERE MaMuon = 2003;

```


<img width="1918" height="1074" alt="image" src="https://github.com/user-attachments/assets/d27c4871-27f5-41e3-9722-7585b7948559" />

                  Kết quả in ra thông tin của người mượn sách

```

SELECT 
    mt.MaMuon,
    mt.MaDocGia,
    mt.MaSach,
    mt.NgayMuon,
    mt.HanTra,
    mt.NgayTraThucTe,
    mt.TienPhat,
    dbo.fn_TinhSoNgayQuaHan(mt.MaMuon) AS SoNgayQuaHan,
    dbo.fn_TinhSoNgayQuaHan(mt.MaMuon) * 5000 AS TienPhatDuKien,
    
    -- Tổng lần mượn của độc giả này
    (SELECT COUNT(*) FROM dbo.MuonTra WHERE MaDocGia = mt.MaDocGia) AS TongLanMuon,
    
    -- Số sách đang mượn (chưa trả)
    (SELECT COUNT(*) FROM dbo.MuonTra 
     WHERE MaDocGia = mt.MaDocGia AND NgayTraThucTe IS NULL) AS SoSachDangMuon

FROM dbo.MuonTra mt
WHERE mt.MaMuon = 2003;


``

<img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/5a6b4da5-866a-454e-a4c7-e65c42b6603f" />


In ra thông tin quá hạn trả và bị phạt tiền

<img width="1919" height="1075" alt="image" src="https://github.com/user-attachments/assets/200722a8-364d-4542-b9b0-139b9bbc5b3b" />



kết quả trả về dánh sách đang được mượn bởi 1 độc giả cụ thể


## 2.5: Multi-statement Table-Valued Function: Thống kế mượn sách của độc giả

```

-- Trả về bảng thống kê: tổng số lần mượn, sách đang mượn, tổng tiền phạt
CREATE FUNCTION [fn_ThongKeDocGia]
(
    @MaDocGia INT
)
RETURNS @KetQua TABLE
(
    [MaDocGia] INT,
    [HoTen] NVARCHAR(100),
    [TongLanMuon] INT,
    [DangMuon] INT,
    [TongTienPhat] DECIMAL(12,3)
)
AS
BEGIN
    DECLARE @HoTen NVARCHAR(100);
    DECLARE @TongLanMuon INT, @DangMuon INT, @TongPhat DECIMAL(12,3);
    
    SELECT @HoTen = [HoTen] FROM [DocGia] WHERE [MaDocGia] = @MaDocGia;
    
    SELECT 
        @TongLanMuon = COUNT(*),
        @DangMuon = SUM(CASE WHEN [NgayTraThucTe] IS NULL THEN 1 ELSE 0 END),
        @TongPhat = ISNULL(SUM([TienPhat]), 0)
    FROM [MuonTra]
    WHERE [MaDocGia] = @MaDocGia;
    
  
    INSERT INTO @KetQua
    VALUES (@MaDocGia, @HoTen, @TongLanMuon, @DangMuon, @TongPhat);
    
    RETURN;
END;
GO

SELECT * FROM dbo.[fn_ThongKeDocGia](101);

```

<img width="1918" height="1073" alt="image" src="https://github.com/user-attachments/assets/2fc1cd1c-8b8f-4ec8-8597-b1bb024837ef" />

Hình ảnh kết quả in ra bảng mượn sách của độc giả

## Phần 3: XÂY DỰNG STORRED PROCEDURE

## 3.1 Một số SP trong SQL có sẵn: Một số SP có sẵn trong SQL và ví dụ SP, giải thích những SP đó
<img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/fd2cd669-a1d5-4388-94f0-b34bf37656f4" />

Hình ảnh này là một số SP và ví dụ có giải thích

## 3.2 Stored Procedure đơn giản để thực hiện các lệnh sau: `INSERT`, `UPDATE` và kiểm tra điều kiện

<img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/1a0e3702-5054-4453-b5a4-8b805ea8d2ed" />

Hình ảnh dùng lệnh INSERT để thực hiện thêm dữ liệu vào bảng
 
## 3.3 Stored procedure sử dụng tham số `OUTPUT`: Tính tổng tiền phạt của độc giả

<img width="1916" height="1077" alt="image" src="https://github.com/user-attachments/assets/da651677-334d-4d64-811f-9996c94db748" />

Hình ảnh dùng tham số OUTPUT để tính tổng tiền phạt của người mượn sách
## 3.4 Stored Procedure trả về Result Set: Báo cáo mượn sách

<img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/ae37c652-f0ec-40d0-8ab4-e10994e5e6f6" />

Hình ảnh dòng code

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/66e7e2ba-758b-472e-bfcd-cd565b0c988b" />

Hình ảnh kết quả chạy của dòng code trên

## Phần 4: TRIGGER VÀ XỬ LÝ LOGIC NGHIỆP VỤ

## 4.1 Trigger tự động cập nhật: Cập nhật số lượng sách khi mượn/trả

<img width="1910" height="1078" alt="image" src="https://github.com/user-attachments/assets/14a00fd0-e595-4c70-b56b-4454dddeef3c" />

Hình ảnh code

<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/b00bd64a-d28a-45ba-8cef-f10a34edf72a" />

Hình ảnh kết quả trước khi mượn sách

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/add04b53-5fa3-4096-840d-7ee31f07103b" />

Hình ảnh kết quả, số lượng sách ban đầu bảng A thay đổi dữ liệu bảng A, trigger tự động ở bảng B là cho mượn 1 quyển sách thì số lượng bảng A thay đổi 

## 4.2 Viết Trigger cho bảng A và bảng B: Thử nghiệm  Trigger vòng lặp (A -> B -> A)

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/64bfd1fb-4e8b-4773-81e1-47e733e65f3f" />

  Hình ảnh thêm cột tổng lần mượn và số sách đang mượn vào bảng **DocGia** đã có sẵn

 <img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/0e38c06f-1869-44e4-a230-243b92cdf241" />

  Cột **TongLanMuon** và **SoSachDangMuon** đã có trong bảng DocGia

- Viết Trigger cho bảng A:
  
<img width="1919" height="1075" alt="image" src="https://github.com/user-attachments/assets/b66e0daa-5d24-4a94-a9c1-feb3c40a9975" />

Hình ảnh code của Trigger A

- Viết Trigger cho bảng B:

<img width="1915" height="1076" alt="image" src="https://github.com/user-attachments/assets/ef0a0adb-5676-4676-8b01-32fb55ebdfe1" />

Hình ảnh code của Trigger B

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ab1618e6-91f0-4d72-b412-4d1a1992a17d" />


Hình ảnh kết quả đã thực hiện yêu cầu nhập vào 1 dữ liệu **bảng A** cập nhật dữ liệu mới nhập **bảng A** sang **bảng B** và từ **bảng B** cập nhật dữ liệu sang **bảng A**

Minh họa dễ hiểu từ trong kết quả hình ảnh: Nhập dữ liệu **MaDocGia 2002** bảng A cập nhật dữ liệu **MaDocGia 2002** sang  bảng B và từ bảng B sang bảng A (quy trình được thực hiện theo 1 vòng lặp)

## Phần 5: CURSOR VÀ DUYỆT DỮ LIỆU

## Phần 5.1 Dùng CURSOR: Gửi gmail nhắc nhở độc giả quá hạn trả sách

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/91e7c733-55ae-4af9-bf6e-c3d878dbfeea" />

<img width="1914" height="1070" alt="image" src="https://github.com/user-attachments/assets/40284567-0e7a-40a8-8700-d697bdb631c2" />

<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/cc3143d2-8015-4a13-9ffa-0bda0ac048e1" />

Hình ảnh code

<img width="1919" height="1074" alt="image" src="https://github.com/user-attachments/assets/aef32c6f-a067-4ccd-8a28-3afe8d49ea4f" />

Hình ảnh chứng mình thông báo cho những độc giả mượn sách mà sách đã quá hạn

## Phần 5.2 Giải pháp khồng dùng CURSOR: So sánh khi dùng và không dùng CURSOR


```IF OBJECT_ID('usp_GuiNhacNhoQuaHan_SetBased', 'P') IS NOT NULL
    DROP PROCEDURE usp_GuiNhacNhoQuaHan_SetBased;
GO

CREATE PROCEDURE [usp_GuiNhacNhoQuaHan_SetBased]
AS
BEGIN
    SET NOCOUNT ON;
    
    SELECT 
        N'📧 [NHẮC NHỜ] Mã mượn: ' + CAST(mt.[MaMuon] AS VARCHAR(20)) +
        N' | Độc giả: ' + dg.[HoTen] +
        N' | Sách: ' + s.[TenSach] +
        N' | Quá hạn ' + CAST(DATEDIFF(DAY, mt.[HanTra], GETDATE()) AS VARCHAR(10)) + N' ngày' +
        N' | Hạn trả: ' + CONVERT(VARCHAR(10), mt.[HanTra], 103) AS [ThongBao]
    FROM [MuonTra] mt
    INNER JOIN [DocGia] dg ON mt.[MaDocGia] = dg.[MaDocGia]
    INNER JOIN [Sach] s ON mt.[MaSach] = s.[MaSach]
    WHERE mt.[NgayTraThucTe] IS NULL      
      AND mt.[HanTra] < GETDATE();         
    
    PRINT N'✓ Hoàn thành (Set-based)';
END;
GO

PRINT N'✓ Đã tạo procedure usp_GuiNhacNhoQuaHan_SetBased thành công';

INSERT INTO [MuonTra] ([MaMuon], [MaDocGia], [MaSach], [NgayMuon], [HanTra], [NgayTraThucTe])
SELECT 2001, 101, 1, DATEADD(DAY, -20, GETDATE()), DATEADD(DAY, -6, GETDATE()), NULL
WHERE NOT EXISTS (
    SELECT 1 FROM MuonTra WHERE MaMuon = 2001
);

PRINT N'✓ Đã thêm dữ liệu tes'
EXEC [usp_GuiNhacNhoQuaHan_SetBased];```























