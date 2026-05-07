# phat_trien_ung_dung_voi_ma_nguon_mo_2
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

## BÀI LÀM
# 1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)

# 2.
- Khởi động docker
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3c29496f-0725-4019-a182-4e2aa9f46866" />

- TẠO PROJECT
 > - Tạo thư mục project -> tạo thư mục Django
<img width="795" height="150" alt="image" src="https://github.com/user-attachments/assets/67a39984-7c9e-4c23-9ebe-adfcf242d4c7" />
— TẠO DOCKERFILE
   > - Dockerfile dùng để build container Django.
Đi vào thư mục django_app : cd django_app
Tạo file Dockerfile: sudo nano Dockerfile
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cc94bd43-9111-451b-b037-da4f03266a5b" />

- TẠO requirements.txt
> - Tạo file requirements.txt : sudo nano requirements.txt
<img width="742" height="905" alt="image" src="https://github.com/user-attachments/assets/18a12588-1a89-41ee-970b-4884862e467a" />

- TẠO docker-compose.yml
  > -  Quay lại thư mục gốc : cd
  > -  Tạo docker-compose.yml: sudo nano docker-compose.yml
<img width="747" height="905" alt="image" src="https://github.com/user-attachments/assets/11feb0e3-d923-44d0-9c52-c13fd3542697" />

- BUILD VÀ CHẠY
 > - Build project: sudo docker compose up -d --build
      >  - Docker sẽ:
>          tải MariaDB
>          tải PhpMyAdmin
>          build Django

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce5b458b-0149-4097-8717-e4ffa34a0502" />

> - Kiểm tra container
>      sudo docker ps

<img width="1647" height="408" alt="image" src="https://github.com/user-attachments/assets/97ae5656-114f-4122-b9b7-b38bc8449bc6" />

- Tạo app Cầm đồ
> - Tạo app : sudo docker compose exec django python manage.py startapp camdo
> - Kiểm tra app đã tạo chưa : ls django_app
<img width="1272" height="92" alt="image" src="https://github.com/user-attachments/assets/87c8bf05-ad93-43bc-9dce-102d150cd0b5" />

- Sửa settings.py để kết nối MariaDB
> - Mở file: sudo nano django_app/config/settings.py
> - tìm đoạn Tìm đoạn này
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}  và thay thành như trong ảnh
<img width="752" height="938" alt="image" src="https://github.com/user-attachments/assets/f6a39887-150f-4ced-abd8-d0288cdfecd2" />

- Khai báo app camdo
> - Trong cùng file settings.py
   > - Tìm: INSTALLED_APPS = [ . và Thêm dòng: 'camdo',
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/556fdce7-d2a1-465f-9f17-f51b29088ca4" />
- sau đó Restart Django : docker compose restart django
- Kiểm tra Django có lỗi không: sudo docker logs django_camdo
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c2043c6a-67d4-4015-83f8-09046a296b16" />

- tạo models.py
Mở file: nano django_app/camdo/models.py
<img width="767" height="923" alt="image" src="https://github.com/user-attachments/assets/0e62838d-277a-402b-bc3f-35d52cb439a5" />

- tạo migration
Chạy: docker compose exec django python manage.py makemigrations
<img width="1232" height="157" alt="image" src="https://github.com/user-attachments/assets/dc2e90cd-8b9f-441f-98c0-6e4f6d4b5470" />

- Sau đó migrate vào MariaDB: docker compose exec django python manage.py migrate
<img width="1352" height="541" alt="image" src="https://github.com/user-attachments/assets/4bd77c4b-4087-4e6f-a51e-fb9c6f4bd97d" />

- ## Cấu hình Django Admin
- Tạo tài khoản admin : docker compose exec django python manage.py createsuperuser
<img width="1216" height="245" alt="image" src="https://github.com/user-attachments/assets/32f05e7b-0a16-4202-affb-8a5936dcd816" />

- Đăng ký model vào admin
> - Mở file: nano django_app/camdo/admin.py
<img width="742" height="875" alt="image" src="https://github.com/user-attachments/assets/2ad78955-84ac-4b11-bd31-03d657158252" />

- Restart Django: docker compose restart django
- Truy cập Django Admin
  > - Mở trình duyệt: http://localhost:8000/admin
  > - Hoặc: http://IP_UBUNTU:8000/admin
<img width="687" height="440" alt="image" src="https://github.com/user-attachments/assets/13b7c723-d601-456f-9b8c-2651a8a225b2" />
- Sau khi login thành công
> - KQ được trang admin, yêu cầu đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/74afe113-a5e1-4596-ae9d-e062a077b959" />

-  kiểm tra FK bằng PhpMyAdmin
<img width="686" height="677" alt="image" src="https://github.com/user-attachments/assets/815c0b1d-5faa-4351-b2db-a7dcb8d9cf67" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0df37feb-1e20-4454-afb4-f459220da08a" />

- Test FK hoạt động đúng
- Thêm dữ liệu thử
> - Thêm khách hàng
<img width="690" height="786" alt="image" src="https://github.com/user-attachments/assets/bae38e31-9df1-4e2e-a0d5-afd7240416ee" />
<img width="685" height="498" alt="image" src="https://github.com/user-attachments/assets/042676f4-a390-470b-9197-0aa4fa59a684" />

> - Thêm tài sản
<img width="681" height="832" alt="image" src="https://github.com/user-attachments/assets/157b05ce-156e-44c4-a173-8df7d6474438" />

<img width="696" height="590" alt="image" src="https://github.com/user-attachments/assets/4ba19686-2bd1-489c-8e81-936bf97842b8" />

> - Thêm hợp đồng cầm
<img width="756" height="901" alt="image" src="https://github.com/user-attachments/assets/b255c4a2-4d82-405d-86c5-4dd6d7bccd24" />



