# K58KTP_Baitap2web.
Bài tập 02: Lập trình web.
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
GHI CHÚ:
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
==============================
DEADLINE: 26/10/2025
==============================
# Bài làm
## 2.1. Cài đặt Apache web server:

Truy cập trang chủ của apache để cài đặt
ấn download -> a number of third party vendors
<img width="959" height="1035" alt="image" src="https://github.com/user-attachments/assets/3519212c-1cdb-4a9a-9c5e-c3ef9f7d02b9" />
- Chọn bộ xử lí phù hợp với máy để tải xuống
<img width="959" height="1035" alt="image" src="https://github.com/user-attachments/assets/ceccbdc8-d81c-4952-99b9-469bb9980f4e" />
-- Giải nén tại ổ D
  <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/a6af530d-74ac-4adc-928a-e7b2b202b725" />
  - Cấu hình file D:\Apache24\conf\httpd.conf .
  - <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/482161ec-8577-4124-9959-50686509fb27" />
  - đổi SeverName www.example.com:80 thành ServerName localhost:80
<img width="959" height="1035" alt="image" src="https://github.com/user-attachments/assets/5e42253c-6867-4525-aad5-738de155114b" />
-đổi Define SRVROOT "c:/Apache24 thành Define SRVROOT "D:/Apache24"
<img width="703" height="740" alt="image" src="https://github.com/user-attachments/assets/b590e199-ef48-4708-884b-db7e6b5e7abb" />
-xóa dấu # tại phần Include conf/extra/httpd-vhosts.conf
- Cấu hình D:Apache24\conf\extra\httpd-vhosts.conf
<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/15a29778-6345-414b-b1a8-fdf4b97f68f0" />
<img width="1920" height="1080" alt="Annotation 2025-10-25 221552" src="https://github.com/user-attachments/assets/56c726a9-fae5-4b93-b6ad-ec8bc1c2af39" />
-sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain
<img width="1920" height="1080" alt="Annotation 2025-10-25 221341" src="https://github.com/user-attachments/assets/6965fc78-ea59-4c45-bb13-278f3474446c" />
-  tạo file index dươis dạng txt,
<img width="965" height="1071" alt="image" src="https://github.com/user-attachments/assets/83d41e13-af24-41b9-adf7-b3b27ff8f620" />
thao tác dòng lệnh trên file D:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache
.<img width="782" height="519" alt="image" src="https://github.com/user-attachments/assets/a2738a3c-aa6e-461d-93ab-cd84b463fcfb" />
<img width="1920" height="1080" alt="Annotation 2025-10-25 221233" src="https://github.com/user-attachments/assets/972234c5-d639-4fcc-98fc-8d5cbd1e7edc" />
## 2.2 Cài đặt nodejs và nodered => Dùng làm backend: - Cài đặt nodejs:
<img width="618" height="487" alt="Annotation 2025-10-25 222244" src="https://github.com/user-attachments/assets/569fd9ed-f4d5-4762-8852-6556d29fabfa" />
<img width="626" height="490" alt="Annotation 2025-10-25 222317" src="https://github.com/user-attachments/assets/f74a1db7-77cb-4999-8f5f-10d6df5178fd" />
- Chọn tải xuống ở ổ D -
<img width="622" height="496" alt="Annotation 2025-10-25 222523" src="https://github.com/user-attachments/assets/f9f84cb2-82a8-4384-a829-a76423a7cfe5" />
<img width="625" height="487" alt="Annotation 2025-10-25 222600" src="https://github.com/user-attachments/assets/4d73b4c5-c47f-4d4a-81e3-caeb431ebe68" />
<img width="633" height="491" alt="Annotation 2025-10-25 222620" src="https://github.com/user-attachments/assets/99dd6a17-490b-4627-9ea6-4a7b569b4e39" />
<img width="627" height="487" alt="Annotation 2025-10-25 222709" src="https://github.com/user-attachments/assets/b1ac3801-ba74-40a3-9986-409bfb6b0329" />
- cài đặt nodered -
<img width="1097" height="526" alt="Annotation 2025-10-25 223423" src="https://github.com/user-attachments/assets/80adb75f-050a-4a1a-a4b2-69d37050512d" />
- khởi động nodered :
<img width="1920" height="1080" alt="Annotation 2025-10-25 224638" src="https://github.com/user-attachments/assets/c148e51f-4fe0-4c43-bd94-258c6e6a1c3f" />

## 2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022)
<img width="1359" height="1080" alt="image" src="https://github.com/user-attachments/assets/c91fdbd8-d9f5-478f-94bc-2abd7378c529" />
-Cài đặt thư viên trên nodered - truy cập giao diện nodered bằng url: http://localhost:1880
<img width="1920" height="1080" alt="Annotation 2025-10-25 224638" src="https://github.com/user-attachments/assets/86cdef37-42dd-443b-bbc1-3bbd9f53b07e" />
## 2.4 cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
  <img width="1920" height="1080" alt="Annotation 2025-10-26 095446" src="https://github.com/user-attachments/assets/8cf16bd4-c38c-4c88-a487-25ac014ba6f1" />
  - Sửa file `D:\nodejs\nodered\work\settings.js` : tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới adminAuth: { type: "credentials", users: [{ username: "admin", password: "chuỗi_mã_hoá_mật_khẩu", permissions: "*" }] },
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a04dd9f8-82ec-4c1f-aa1d-f2241977b7e9" />
- chạy lại nodered
- <img width="814" height="402" alt="image" src="https://github.com/user-attachments/assets/f384b374-b78e-45c9-ba4e-e2ef53b1b0e1" />
- nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
  <img width="961" height="1030" alt="image" src="https://github.com/user-attachments/assets/14788aaa-2ac2-4db8-95b3-b1363bd88cd1" />
## 2.5. tạo api back-end bằng nodered:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/388ab6a8-9269-493f-a130-f4cafc43eb56" />
<img width="960" height="1030" alt="image" src="https://github.com/user-attachments/assets/5c870ef0-17a9-4f96-9b22-ec711eebf0cb" />
## 2.6. Tạo giao diện front-end - html form gồm các file : index.html, fullname.js, fullname.css
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/be8d5e0d-f5cb-47ea-8065-47459c81d881" />
- kết quả -
<img width="960" height="1030" alt="image" src="https://github.com/user-attachments/assets/33f36a36-5f92-4054-b755-1a05700698eb" />
## 2.7. Nhận xét bài làm của mình:
 Hiểu quá trình cài đặt các phần mềm và thư viện:

Biết cài đặt và cấu hình Apache, Node.js, Node-RED, SQL Server.

Hiểu chức năng từng phần mềm trong hệ thống (Apache: web server, Node-RED: xử lý API, SQL Server: lưu dữ liệu).

Biết cài đặt các thư viện trong Node-RED (node-red-contrib-mssql-plus, node-red-contrib-cron-plus, node-red-contrib-telegrambot, …).

Hiểu cách cấu hình tài khoản quản trị Node-RED trong file settings.js và khởi động lại bằng lệnh nssm restart a1-nodered.

 Hiểu cách sử dụng Node-RED để tạo API back-end:

Dùng các node:
[HTTP In] → [Function] → [MSSQL] → [HTTP Response].

Node HTTP In: nhận request từ client (URL /timkiem?q=...).

Node Function: xử lý tham số và sinh câu SQL.

Node MSSQL: truy vấn dữ liệu từ SQL Server.

Node HTTP Response: trả kết quả JSON về cho client.

Biết test API qua trình duyệt hoặc công cụ gọi URL → kết quả trả về đúng dữ liệu.

 Hiểu cách front-end tương tác với back-end:

Front-end gồm 3 file: index.html, nguyenvanhoan.css, nguyenvanhoan.js.

Giao diện có form nhập từ khóa tìm kiếm.

File JS dùng fetch() gửi request đến API Node-RED (http://localhost:1880/timkiem?q=...).

Nhận JSON phản hồi → hiển thị kết quả lên trang web.

Hiểu luồng hoạt động:
Người dùng → Front-end → API Node-RED → CSDL → Node-RED trả JSON → Hiển thị kết quả.

nhưng thứ học được từ bài tập:

Đã hiểu toàn bộ quy trình xây dựng ứng dụng web 3 tầng:
Database (SQL Server) → Back-end (Node-RED) → Front-end (Apache, HTML/CSS/JS).

Biết cách các phần mềm kết nối và trao đổi dữ liệu.

Hoàn thành đúng yêu cầu và hiểu rõ cách vận hành hệ thống.
