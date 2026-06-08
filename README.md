# NGuyenTuanAnh_BT5_PTUDVMNM
## Nguyễn Tuấn Anh 
## K225480106002
## bài tập 5 Môn: Phát triển ứng dụng với mã nguồn mở 
## bài làm:
## Lý Thuyết

## Thực hành áp dụng:

Bước 1: Tạo cấu trúc thư mục dự án
```bash
mkdir app-monitor
```

<img width="1231" height="825" alt="image" src="https://github.com/user-attachments/assets/d4313083-83b8-4d5b-8059-19e9dbff053a" />

- Di chuyển vào thư mục vừa tạo.
```
cd app-monitor
```

<img width="1218" height="837" alt="image" src="https://github.com/user-attachments/assets/6725a311-eb71-4551-87b1-3c7201a5eb1f" />

- Tạo thư mục con flask-api
```
mkdir flask-api
```
<img width="1218" height="837" alt="image" src="https://github.com/user-attachments/assets/8521793c-46b0-48c6-a77d-d76046c95c50" />

- Tạo thư mục con frontend
```
mkdir frontend
```
<img width="1406" height="995" alt="image" src="https://github.com/user-attachments/assets/df62eda1-1cfb-4bb5-9f03-61ebacff3ba0" />

- Tạo thư mục con nginx
```
mkdir nginx
```
<img width="1212" height="810" alt="image" src="https://github.com/user-attachments/assets/38697fe8-4946-49b9-8115-57e375f0f4cf" />

- Tạo file docker-compose.yml

 + Mở trình soạn thảo tạo file compose
 ```
nano docker-compose.yml
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/695a7a5d-b830-43dc-b6f0-32cbd022770b" />

- Thiết lập Backend Flask API

 + Di chuyển vào thư mục flask-api
```
cd flask-api
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/70ebb4ce-1b42-41fb-9d44-b94862efbd68" />

 + Tạo file cấu hình requirements.txt
```
nano requirements.txt
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eecb49ed-685b-49f1-a76b-17c17c4d9b0a" />

 + Tạo file cấu hình Dockerfile
```
nano Dockerfile
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd3aaba7-d690-4d58-b9e5-2242f29ee83f" />

 + Tạo file mã nguồn app.py
```
nano app.py
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97a9386d-a165-4d29-bc99-75f20d6e57ce" />

- Cấu hình Nginx và giao diện Frontend

 + Mở file cấu hình Nginx
```
nano nginx/nginx.conf
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6d1c265d-84d3-4d5f-89ab-1f0dd8de512e" />

 + Mở file giao diện Web Frontend
```
nano frontend/index.html
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c13a3273-6480-4a94-b3fc-a99cf4c84191" />

- Khởi chạy cụm Container bằng Docker Compose

 + Khởi chạy hệ thống mạng Docker
```
docker-compose up -d
```

 + Cập nhật danh sách phần mềm
```
sudo apt update
```
<img width="1920" height="915" alt="image" src="https://github.com/user-attachments/assets/1512ccc2-00b2-43e7-ae83-bd0c1325b6e4" />





































