# NGuyenTuanAnh_BT5_PTUDVMNM
## Nguyễn Tuấn Anh 
## K225480106002
## bài tập 5 Môn: Phát triển ứng dụng với mã nguồn mở 
## bài làm:
## Lý Thuyết

1 Docker là gì?

- Docker là một nền tảng mã nguồn mở cho phép các lập trình viên tự động hóa việc triển khai, quản lý và chạy các ứng dụng bên trong các môi trường ảo hóa biệt lập được gọi là Container.

- Thay vì ảo hóa toàn bộ phần cứng (như Máy ảo - Virtual Machine), Docker sử dụng cơ chế ảo hóa ở cấp độ hệ điều hành (chạy chung nhân OS của máy host). Điều này giúp container cực kỳ nhẹ, khởi động trong vài giây và tốn rất ít tài nguyên.

 + Docker Image: Giống như một "bản thiết kế" hoặc gói phần mềm đóng gói sẵn chứa tất cả những gì cần thiết để ứng dụng chạy được (mã nguồn, thư viện, môi trường...).

 + Docker Container: Là một "thể hiện" (instance) đang chạy của Docker Image. Bạn có thể tạo, khởi động, dừng hoặc xóa container từ một image.

2. Các từ khóa (Keywords) trong docker-compose.yml

- File docker-compose.yml được cấu trúc theo định dạng YAML để định nghĩa và quản lý đa container (multi-container). Dưới đây là các từ khóa cốt lõi chia theo từng thành phần:

- Cấu trúc tổng thể (Top-level)

 + version: Định nghĩa phiên bản cấu trúc của Docker Compose (ví dụ: '3.8').

 + services: Nơi định nghĩa các container cấu thành nên ứng dụng.

 + networks: Định nghĩa mạng lưới để các container giao tiếp với nhau.

 + volumes: Định nghĩa nơi lưu trữ dữ liệu bền vững (persistent data), không bị mất khi container bị xóa.

- Các từ khóa mô tả một Service (Container)

 + image -> Chỉ định Docker Image sẽ được dùng để kéo về và chạy container.

 + build -> Chỉ định đường dẫn đến Dockerfile để tự động build image thay vì dùng image có sẵn.

 + ports -> Ánh xạ (mapping) cổng từ máy Host vào cổng bên trong Container (Host:Container).

 + environment -> Khai báo các biến môi trường (Environment Variables) cho ứng dụng bên trong container.

 + volumes -> Gắn một ổ đĩa (volume) đã định nghĩa hoặc một thư mục từ máy host vào container.

 + networks -> Chỉ định container này thuộc mạng lưới (network) nào để giao tiếp nội bộ.

 + depends_on -> Thể hiện sự phụ thuộc giữa các service (Ví dụ: Web phải chờ Database chạy trước).

 + restart -> Chính sách khởi động lại container (ví dụ: always, unless-stopped).

- Dưới đây là một file docker-compose.yml hoàn chỉnh kết hợp đầy đủ các thành phần:
```
version: '3.8'

services:
  # Định nghĩa service ứng dụng Web (Node.js/Python...)
  web-app:
    image: node:18-alpine
    command: npm start
    ports:
      - "8080:3000" # Máy ngoài truy cập cổng 8080, container nhận ở cổng 3000
    environment:
      - DB_HOST=db-service
      - NODE_ENV=production
    networks:
      - app-network
    depends_on:
      - db-service # web-app sẽ chờ db-service khởi động trước
```
```
  # Định nghĩa service Database (PostgreSQL)
  db-service:
    image: postgres:15
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret_pass
    volumes:
      - db-data:/var/lib/postgresql/data # Lưu dữ liệu DB vào volume riêng
    networks:
      - app-network
    restart: always
```
```
# Khai báo các thành phần dùng chung toàn cục
networks:
  app-network:
    driver: bridge # Tạo một mạng ảo dạng bridge bảo mật nội bộ
```
```
volumes:
  db-data: # Khai báo volume để dữ liệu không bị mất khi xóa container
```

3. Ưu điểm khi triển khai ứng dụng sử dụng Docker

- Tính nhất quán (Consistency / "It works on my machine"): Giải quyết triệt để lỗi "chạy được trên máy em nhưng lỗi trên máy server". Docker đóng gói mọi thứ cần thiết, đảm bảo ứng dụng chạy giống hệt nhau ở môi trường Dev, Staging, và Production.

- Tiết kiệm tài nguyên và Hiệu năng cao: So với Máy ảo (VM), Docker container không cần chạy một hệ điều hành riêng. Do đó, nó sử dụng ít RAM/CPU hơn, cho phép chạy hàng chục container trên cùng một phần cứng mà VM không kham nổi.

- Khởi động siêu tốc: Container khởi động chỉ trong vài giây (vì chỉ là khởi động tiến trình), trong khi VM mất vài phút (vì phải boot toàn bộ OS).

- Dễ dàng mở rộng (Scalability): Rất phù hợp với kiến trúc Microservices. Khi tải tăng cao, bạn có thể dễ dàng nhân bản (scale up) số lượng container bằng một câu lệnh (ví dụ: docker compose up --scale web-app=3).

- Cách ly an toàn (Isolation): Mỗi container là một môi trường độc lập. Nếu một ứng dụng bị hack hoặc crash, nó sẽ không làm ảnh hưởng trực tiếp đến các ứng dụng khác trên cùng máy chủ.

4. Quy trình triển khai ứng dụng lên Máy chủ Offline (Không có Internet)

Bước 1: Chuẩn bị cài đặt Docker trên Máy chủ Offline

- Vì máy chủ không có internet, không thể dùng lệnh apt-get install docker hay yum install.

 + Cách làm: Tải trước các gói cài đặt Docker (file .deb hoặc .rpm) tương ứng với hệ điều hành của Server và các gói phụ thuộc (dependencies) từ một máy có mạng.     Sau đó copy qua USB/Ổ cứng di động vào Server và tiến hành cài đặt offline (ví dụ với Ubuntu: dpkg -i *.deb).

Bước 2: Đóng gói (Export) Docker Image từ Laptop cá nhân

- Trên máy laptop (đã test app OK), tiến hành xuất các Image ứng dụng thành các file nén dạng .tar.

- Lệnh thực hiện:

```
# Nếu dùng docker thường:
docker save -o my-web-app.tar node:18-alpine
docker save -o my-db.tar postgres:15

# Hoặc lưu nhanh tất cả image trong dự án
docker save -o all-images.tar image_name_1 image_name_2
```

Bước 3: Copy toàn bộ tài liệu sang Máy chủ

- Sử dụng các thiết bị ngoại vi (USB, ổ cứng di động) hoặc mạng LAN nội bộ (nếu có kết nối với máy Server) để copy các file sau sang Server:

 + Các file .tar chứa Docker Images vừa xuất ở Bước 2.

 + File docker-compose.yml (và mã nguồn/file cấu hình nếu có dùng lệnh build hoặc volumes).

Bước 4: Nạp (Import) Image và Chạy ứng dụng trên Máy chủ

- Trên máy chủ, thực hiện nạp lại các file .tar thành Docker Image và khởi chạy.

- Lệnh nạp Image:
```
  docker load -i my-web-app.tar
docker load -i my-db.tar
```

-  Kiểm tra xem image đã vào hệ thống chưa: docker images
-  Khởi chạy: Di chuyển vào thư mục chứa file docker-compose.yml đã copy sang, và gõ lệnh quen thuộc:

```
docker compose up -d
```

- Ứng dụng sẽ tự động dựng lên, kết nối mạng nội bộ và hoạt động bình thường như trên laptop mà không cần một giọt internet nào!

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





































