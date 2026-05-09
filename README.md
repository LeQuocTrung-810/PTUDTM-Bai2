# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421

Lớp: 58KTPM

**Bài tập 02:** 
# SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ
## deadline : 23h59 ngày 09 tháng 5 năm 2026.

1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)
   
2. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ: 
- Mariadb  : chứa csdl của hệ thống này
- Phpmyadmin: để soi được csdl (chỉ để xem, ko cần tạo bảng từ đây, django sẽ làm hết)
- Django: build 1 docker container (dùng Dockerfile): trên nền python, sử dụng django, nhớ mount thư mục để dễ edit, edit dùng: sudo nano ten_file

sau khi có 3 service này trong file docker-compose.yml :
- run nó, cấu hình để Django nhận csdl mariadb (sửa file settings.py), cấu hình user login ban đầu, mô tả các bảng trong models.py, .... (đc phép sử dụng AI để làm) => KQ được trang admin, y/c đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng. các trường là khoá ngoại chỉ việc chọn text (mặc dù là csdl tại trường FK đó lưu ID của PK mà nó tham chiếu : sử dụng phpmyadmin để kiểm chứng)
- chú ý kết hợp ssh để chạy lệnh tác động vào django và sudo nano để edit file.
- sử dụng template (file html, sử dụng cú pháp jinja2), lấy context từ 1 view home_page, để tạo trang liệt kê các con nợ đến hạn mà chưa trả tiền!
- sử dụng cloudflare tunnel để public kết quả lên 1 sub-domain => chụp kết quả

Hướng dẫn:
- Tạo thư mục để chứa image tự buid cho django
- Vào thư mục đó tạo file tên: Dockerfile (nội dung hỏi AI xem file này cần có nội dung gì, full comment cho từng dòng lệnh trong file này => mục tiêu kép: để hiểu và để hệ thống chạy được)
- AI sẽ nói cần thêm file requirements.txt để cài các thư viện cho python (cài qua lệnh pip) => tạo file requirements.txt với nội dung tưng ứng, trong file này cũng comment được => comment xem thư viện nào dùng để làm gì
- Sau mỗi lần sửa đỏi có thể phải chạy lệnh dạng : **docker compose exec TÊN_SERVICE_DJANGO_CỦA_BẠN python manage.py migrate** để tác động vào django (còn nhiều lệnh khác chứ ko luôn như này), để django thay đổi csdl hoặc thay đổi cấu hình.

## TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ
<img width="963" height="1280" alt="image" src="https://github.com/user-attachments/assets/cee6766c-3229-437b-ad5a-b1e31663ea58" />  
## CHUẨN BỊ MÔI TRƯỜNG DOCKER
Tạo một thư mục mới cho dự án mkdir pawn_shop && cd pawn_shop  
Tạo file requirements.txt  
<img width="1280" height="963" alt="image" src="https://github.com/user-attachments/assets/249d870c-466d-4ce4-8e00-754ba1d1640b" />  
Tạo file Dockerfile (nano Dockerfile)  
<img width="1280" height="963" alt="image" src="https://github.com/user-attachments/assets/31d0af42-f373-4d6a-88ae-81e8ec6d032a" />  
Tạo file docker-compose.yml để liên kết 3 services (MariaDB, PhpMyAdmin, Django).  
<img width="1280" height="963" alt="image" src="https://github.com/user-attachments/assets/47d791e4-811d-4828-96d4-eaffcac736b7" />
## KHỞI TẠO DJANGO VÀ CẤU HÌNH CSDL  
Build image và tạo project Django  
```
sudo docker compose build
sudo docker compose run --rm web django-admin startproject config .
sudo docker compose run --rm web python manage.py startapp core
```

Cấu hình kết nối MariaDB (sudo nano config/settings.py)  
<img width="1763" height="1015" alt="image" src="https://github.com/user-attachments/assets/2617c60a-608d-4d9f-a018-a193c4fef9ab" />  
Đăng nhập vào PHP  
<img width="1263" height="610" alt="image" src="https://github.com/user-attachments/assets/d85274ab-82f3-4269-ad5d-2152cea66352" />  
Đăng nhập vào django để thêm 1 vài con nợ  
<img width="1358" height="741" alt="image" src="https://github.com/user-attachments/assets/ca470d6a-7788-4899-a234-3412319a7488" />  
## TẠO MODELS & ADMIN QUẢN LÝ  
Định nghĩa Models (sudo nano core/models.py)  

Đăng ký lên Admin (sudo nano core/admin.py)

Khởi chạy và Áp dụng CSDL  

```
docker compose up -d
docker compose exec web python manage.py makemigrations
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
```

