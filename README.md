# NAD_HQTCSDL
**Họ & tên: Nguyễn Hữu Doan**  
**Lớp: K59KMT.K01**  
**MSSV: K235480106008**  
---

1. Download và cài đặt SQL Server 2025
<img width="1920" height="1080" alt="{FD172F3E-B494-42F7-A40D-325EAB1B065A}" src="https://github.com/user-attachments/assets/d29ea86b-162c-4467-95b5-4d422d56c1a9" />

2. Cấu hình cho SQL Server làm việc ở cổng động(Dynamic Port), TCP(36008):
<img width="1920" height="1080" alt="{AB16E8B4-BC94-4CEB-A5C3-E2A3F25F59C5}" src="https://github.com/user-attachments/assets/e413e18f-5fb1-4496-adb4-fc31fbd99386" />


3. Kiểm tra xem Service SQL Server bằng Command Prompt
```powershell
netstat -ano | findstr LISTENING
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/48625a60-b871-43a1-8283-43dd9c483ca2" />



4. Cài Đặt SQL Server Management Studio
<img width="1920" height="1080" alt="{03B20F32-4ACC-4D22-8605-9D552C2BDD87}" src="https://github.com/user-attachments/assets/a5fb485d-67dc-44fc-bb06-c5832f1e87c1" />

   
5. Chạy phần mềm ssms để Đăng nhập vào SQL Server bằng 2 cách:
Windows Authentication
<img width="1920" height="1080" alt="{900295EE-FB4B-497F-A8C5-C68DDCB67CE1}" src="https://github.com/user-attachments/assets/2e5ce9d3-1351-44a3-ad5c-10b3e71af2bf" />

SQL Server Authentication 
<img width="1920" height="1080" alt="{F9D9546E-C197-4A8D-8CE1-CBBE850EEB03}" src="https://github.com/user-attachments/assets/44550ce8-22d9-4794-a032-2803caa2c180" />


6. File đã tạo ra database
   <img width="1920" height="1080" alt="{5ADA9B13-CA46-44D0-8F9F-C32196FC9778}" src="https://github.com/user-attachments/assets/2e1eb018-5992-4581-9b0c-46438f4e4497" />



7. Tạo bảng dữ liệu và Primary Key(masv)
<img width="1920" height="1080" alt="{902E0B4E-BDC4-467A-B5CD-8886B1F31CD9}" src="https://github.com/user-attachments/assets/5cd8b6f2-dd12-46fd-8c58-6ae68c8f81c8" />


8. Import dữ liệu từ file csv mẫu vào bảng
```powershell
BULK INSERT SinhVien
FROM 'E:\SQLData\svtnut.csv'
WITH (
    FIRSTROW = 2,
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    CODEPAGE = '65001',
    TABLOCK
);
```
   <img width="1920" height="1080" alt="{2C4DDD59-B95B-4CAB-A637-8B04B9425141}" src="https://github.com/user-attachments/assets/42aa2a40-f740-4a66-9af6-a21f7f74931a" />

9. Kiểm tra xem số dòng của bảng sau khi import: 12020 dòng
```powershell
SELECT 
    masv AS N'Mã SV',
    hotensv AS N'Họ tên',
    malop AS N'Mã lớp',
    ngaysinh AS N'Ngày sinh',
    noisinh AS N'Nơi sinh',
    diachi AS N'Địa chỉ'
FROM dbo.SinhVien
WHERE masv = 'K235480106008';

SELECT 
    masv AS N'Mã SV',
    hotensv  AS N'Họ tên',
    malop AS N'Mã lớp',
    ngaysinh AS N'Ngày sinh',
    noisinh AS N'Nơi sinh',
    diachi AS N'Địa chỉ'
FROM dbo.SinhVien;
```
<img width="1920" height="1080" alt="{C66D6461-DA3F-42E8-AD09-70F2BD3B272F}" src="https://github.com/user-attachments/assets/95784086-86c5-40a1-8e22-0eaeac19375f" />


   
10. Thêm 1 row vào bảng với dữ liệu là thông tin sinh viên
```powershell
INSERT INTO SinhVien (masv, hotensv, malop, ngaysinh, noisinh, diachi)
VALUES ('K235480106008', N'Nguyễn Hữu Doan', 'K59KMT.K01', '2005-01-11', N'Bắc Giang', N'Bắc Giang');
```
<img width="1903" height="1080" alt="{935048FB-E51C-4597-9EDD-9D2E3F19D162}" src="https://github.com/user-attachments/assets/9a7cee31-650d-4ee2-8084-518ac25d7be7" />



11. Cập nhật trường noisinh thành 'Sao Hoả' cho những dòng có noisinh và diachi đề là null
```powershell
UPDATE dbo.SinhVien
SET diachi = N'Sao Hỏa'
WHERE noisinh = N'Sao Hỏa'
  AND (diachi IS NULL OR diachi = 'NULL');
```
<img width="1920" height="1080" alt="{84E4F3A4-6C42-4F85-8DD2-BF6E71B6FD7B}" src="https://github.com/user-attachments/assets/47a81b12-93ed-440c-a807-d30f4e37194e" />




12. Tạo bảng SaoHoa gồm những sinh viên có nơi sinh ở 'Sao Hoả'
```powershell
IF OBJECT_ID('SaoHoa', 'U') IS NOT NULL 
    DROP TABLE SaoHoa;
SELECT *
INTO SaoHoa
FROM SinhVien
WHERE NoiSinh = N'Sao Hoả';
SELECT * FROM SaoHoa;
```
<img width="1920" height="1080" alt="{7C500D5B-D057-493C-8667-32D1ED1393E4}" src="https://github.com/user-attachments/assets/0846bde5-1fb5-49e3-a228-3022b0669687" />

13. Gõ lệnh xoá (delete) trong bảng SaoHoa những sinh viên cùng họ với em, vd em họ nguyễn thì xoá những sv họ 'Nguyễn'
```powershell
SELECT COUNT(*) AS SoLuongNguyen
FROM SaoHoa
WHERE hotensv LIKE N'Nguyen%' ;
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bc72b4f6-8e7e-4487-a31f-ffd71e1e4b01" />

14. Sử dụng giao diện đồ hoạ của ssms: Xuất toàn bộ kết quả của các bước 6,7,8,9,10,11,12,13 ra file dulieu.sql , keyword gợi ý: sử dụng tính năng GEN SCRIPT struct+data cho database
<img width="1920" height="1080" alt="{DCFF7A70-6CE2-42AB-AF4E-456D23972C44}" src="https://github.com/user-attachments/assets/e9ec0914-3e12-4d6d-bf63-c6a9df4cee77" />

15. Sử dụng giao diện đồ hoạ của ssms: Xoá csdl đã tạo, sau khi xoá thành công, kiểm tra tại path (path chọn ở bước 6) xem còn tồn tại 2 file của bước 6 không?
<img width="1920" height="1080" alt="{4C7A6DFA-F09D-455E-8D21-D70C07D65E53}" src="https://github.com/user-attachments/assets/11d618c9-716f-4e2d-b283-7741312fbe05" />

16. Tạo cửa sổ mới để gõ lệnh: mở file dulieu.sql của bước 15, chạy toàn bộ các lệnh này. REFRESH lại cây liệt kê các database => kiểm chứng kết quả được tạo ra tương đương với các bước 6,7,8,9,10,11,12,13.
 <img width="1916" height="1080" alt="{854D74F5-FA91-4CB5-A200-FBBC5CB0736A}" src="https://github.com/user-attachments/assets/cb0af4ce-2099-450a-b830-27681ddb2ac6" />

17. upload file dulieu.sql lên github repository của em (repository mà em đang edit file README.md)ository


