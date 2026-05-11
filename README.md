# WORDPRESS WITH DOCKER COMPOSE

> Bài tập 03: Sử dụng WordPress để tạo Website  
> Môn: Phát triển ứng dụng với mã nguồn mở (TEE0421)  
> Lớp: 58KTPM  
> Deadline: 23h59 ngày 12 tháng 5 năm 2026

---

## 📋 Mô Tả Dự Án

Dự án này sử dụng **Docker Compose** để khởi chạy một hệ thống WordPress đầy đủ bao gồm:

- **MariaDB**: Hệ quản trị cơ sở dữ liệu
- **phpMyAdmin**: Công cụ quản lý database trực quan
- **WordPress**: Nền tảng CMS để tạo website

Tất cả dữ liệu được lưu trữ persistent trong thư mục `volumes/` để không bị mất khi tắt container.

---

## 🔧 Yêu Cầu Hệ Thống

Trước khi bắt đầu, đảm bảo bạn đã cài đặt:

- **Docker** (phiên bản 29.4.0)
- **Ubuntu** (Ubuntu 24.04.4 LTS)
- **Cloudflare** (để public website)

### Kiểm tra phiên bản:
```bash
docker --version
```

### Nếu chưa cài, cài đặt Docker trên Ubuntu:
```bash
# Cập nhật package manager
sudo apt update
sudo apt upgrade -y

# Cài Docker
sudo apt install -y docker.io docker compose

# Thêm user vào docker group (để không phải dùng sudo)
sudo usermod -aG docker $USER
newgrp docker

# Khởi động Docker
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 📁 Cấu Trúc Dự Án

```
wordpress-project/
├── docker-compose.yml          # Cấu hình Docker services
├── .env                        # Biến môi trường (credentials)
├── .env.example                # Template .env
├── .gitignore                  # Các file không commit lên Git
├── README.md                   # File này
└── volumes/                    # Dữ liệu persistent (không commit)
    ├── wordpress/              # File WordPress & bài viết
    └── mariadb/                # Database
```

---

## 🚀 CÁC BƯỚC SETUP

### **BƯỚC 1: Clone hoặc tải project**

```bash
https://github.com/nhukhiem3143/BT3-WordPress--Dev-App-OpenSrc.git
```

### **BƯỚC 2: Cấu hình file .env**

```bash
# Copy .env.example thành .env
cp .env.example .env

# Mở file .env với editor để chỉnh sửa (tuỳ chọn)
nano .env
```

**Nội dung .env :**

```env
MYSQL_ROOT_PASSWORD=root_password_123
MYSQL_DATABASE=wordpress_db
MYSQL_USER=wordpress_user
MYSQL_PASSWORD=wordpress_password_456
WORDPRESS_PORT=8080
PHPMYADMIN_PORT=8083
```

> ⚠️ **Lưu ý**: Đổi các password thành giá trị mạnh hơn cho bảo mật

### **BƯỚC 3: Tạo thư mục volumes**

```bash
# Tạo thư mục để lưu dữ liệu
mkdir -p volumes/wordpress
mkdir -p volumes/mariadb

# Phân quyền (tuỳ chọn)
chmod -R 755 volumes/
```
<img width="917" height="130" alt="image" src="https://github.com/user-attachments/assets/1f8d5225-cab2-44c4-bbb8-df5b23480daf" />


### **BƯỚC 4: Khởi chạy Docker Compose**

```bash
# Chạy tất cả services ở background
docker compose up -d
```
<img width="1172" height="253" alt="image" src="https://github.com/user-attachments/assets/8f48157a-5fd1-4916-ad52-2e23424a2707" />


### **BƯỚC 5: Kiểm tra các services**

```bash
# Xem trạng thái containers
docker compose ps

# Xem logs (nếu cần debug)
docker compose logs -f wordpress
```

<img width="1589" height="238" alt="image" src="https://github.com/user-attachments/assets/15144c05-946d-4032-b435-42970bd40a40" />

---

## 🌐 Truy Cập Các Dịch Vụ

| Dịch vụ | URL | Tài khoản | Mật khẩu |
|---------|-----|----------|---------|
| **WordPress** | `http://http://192.168.100.2:8080` | Sẽ cài ở bước 6 | Sẽ cài ở bước 6 |
| **phpMyAdmin** | `http://192.168.100.2:8083` | `username` | `pass` |
| **MySQL Root** | MySQL CLI | `root` | `root_password_123` |

### Trang WordPress
<img width="1867" height="982" alt="image" src="https://github.com/user-attachments/assets/271bc73c-788d-4b35-b701-53749255399d" />  

### Trang phpMyAdmin
<img width="1867" height="999" alt="image" src="https://github.com/user-attachments/assets/b14a4790-22e4-4ca8-b3a2-cd8a9fcf8683" />

---

## 📝 BƯỚC 6: Cài Đặt WordPress

1. Mở trình duyệt, truy cập: **http://192.168.100.2:8080**

2. Chọn ngôn ngữ → Tiếp theo

<img width="1867" height="982" alt="image" src="https://github.com/user-attachments/assets/271bc73c-788d-4b35-b701-53749255399d" />

3. Điền thông tin:
   - **Site Title**: Tên website của bạn
   - **Username**: Tên user admin (ví dụ: `admin`)
   - **Password**: Mật khẩu mạnh
   - **Email**: Email của bạn
   - 
<img width="1874" height="970" alt="image" src="https://github.com/user-attachments/assets/f92070a6-ae42-4f37-8758-9c1d3ea3a537" /> 

4. Nhấp **Install WordPress**

<img width="1875" height="981" alt="image" src="https://github.com/user-attachments/assets/86f6b14f-8ab8-47df-a9ad-9b5ab25da83b" />

5. Đăng nhập vào WordPress với user vừa tạo

<img width="1876" height="988" alt="image" src="https://github.com/user-attachments/assets/cf1b004d-c680-42e7-bfd3-6d96a9f5838f" />

✅ **Xong!** WordPress đã sẵn sàng

<img width="1886" height="989" alt="image" src="https://github.com/user-attachments/assets/609fe29f-3ea3-44b1-887d-45b41e816f32" />

---

## ✍️ BƯỚC 7: Tạo 2 Bài Viết

### **Bài Viết #1: Giới Thiệu Bản Thân**

1. Đăng nhập WordPress
2. **Dashboard** → **Posts** → **Add New**
3. 
<img width="1882" height="995" alt="image" src="https://github.com/user-attachments/assets/a1007b4c-07eb-4e04-a891-652d84f87d49" />

4. Tiêu đề: `Giới Thiệu Bản Thân`
5. Nội dung:
   - Thông tin cá nhân (Họ tên, Ngày sinh, MSSV, ...)
   - Sở thích, mục tiêu
   - Kỹ năng
   - Thêm **ảnh** của bạn (Upload image)
   - Thêm **video** (YouTube embed hoặc upload)
   - Thêm **audio** (nếu có)

6. Nhấp **Publish**
<img width="1880" height="974" alt="image" src="https://github.com/user-attachments/assets/6f1e2c35-67b3-48d4-bb49-c7f14cc8bab1" />
<img width="1895" height="987" alt="image" src="https://github.com/user-attachments/assets/2d380bab-b3f6-479b-93ab-189dac4a0e70" />
<img width="1868" height="971" alt="image" src="https://github.com/user-attachments/assets/b94d4163-9902-41fe-af4c-47592f2159f0" />


### **Bài Viết #2: Giới Thiệu Ngành Học**

1. **Posts** → **Add New**
2. Tiêu đề: `Ngành Học Yêu Thích - TNUT`
3. Nội dung:
   - Thông tin về ngành (mô tả, lợi ích)
   - Các khoá học chính
   - Cơ hội việc làm
   - Thêm **hình ảnh** minh hoạ (Cơ sở, thực hành, ...)
   - Thêm **video** (giới thiệu ngành, kỳ công nghiệp, ...)

4. Nhấp **Publish**

<img width="1890" height="981" alt="image" src="https://github.com/user-attachments/assets/750d71bc-9f58-48b9-8e51-ca6e3e731f84" />
<img width="1890" height="966" alt="image" src="https://github.com/user-attachments/assets/c30ca42d-b40f-420f-8633-d44594e184ea" />
<img width="1881" height="965" alt="image" src="https://github.com/user-attachments/assets/0c1f767c-7442-481d-9c9b-61d8ca10e5a2" />
<img width="1875" height="765" alt="image" src="https://github.com/user-attachments/assets/18ca986e-9b18-48e9-88f8-47a92f156387" />
<img width="1872" height="989" alt="image" src="https://github.com/user-attachments/assets/f4b2242b-b67a-4777-8edb-cb5b8017e42c" />
<img width="1881" height="963" alt="image" src="https://github.com/user-attachments/assets/14026a9a-660d-4f46-af6b-dc340e2717c4" />

✅ **Xong!** Cả 2 bài viết đã online

---

## 🌍 BƯỚC 8: Public Website Với Cloudflare Tunnel

# Public Website Với Cloudflare Tunnel

Thay vì cài đặt trực tiếp Cloudflare Tunnel trên Ubuntu, hệ thống sẽ chạy Tunnel dưới dạng một Docker Container.
Cách này giúp:

* Đồng bộ toàn bộ hệ thống bằng Docker
* Dễ backup và deploy
* Tự khởi động cùng các service khác
* Dễ quản lý bằng Docker Compose

---

# Thêm Service Cloudflared

Mở file:

```bash
docker-compose.yml
```

Thêm đoạn sau vào phần `services:`:

```yml
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: camdo_cloudflared
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TOKEN}
    restart: unless-stopped
    depends_on:
      - django
    networks:
      - camdo_net
```
---

# Thêm Token Cloudflare

Mở file:

```bash
.env
```

Thêm:

```env
CLOUDFLARE_TOKEN=eyJhIjoi...
```
---

# 8.3. Lấy Tunnel Token Trên Cloudflare

## Bước 1: Truy cập Cloudflare Zero Trust

Đăng nhập Cloudflare:

```text
https://dash.cloudflare.com/
```

Vào:

```text
Zero Trust
→ Networks
→ Tunnels
```

---

## Bước 2: Tạo Tunnel

Chọn:

```text
Create a Tunnel
```

Đặt tên:

```text
wordpress-tunnel
```


---

## Bước 3: Chọn Docker

Cloudflare sẽ hiện lệnh dạng:

```bash
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token xxxxx
```

Copy phần:

```text
xxxxx
```

đó chính là:

```env
CLOUDFLARE_TOKEN
```
<img width="1877" height="981" alt="image" src="https://github.com/user-attachments/assets/4dff7fae-13fa-449a-8e01-e2c18d8f3382" />


---

# 8.4. Khởi Động Hệ Thống

Build và chạy toàn bộ container:

```bash
docker compose up -d --build
```

---

# 8.5. Tạo Public Hostname

Vào:

```text
Cloudflare Zero Trust
→ Networks
→ Tunnels
→ Chọn Tunnel
→ Public Hostname
```

Chọn:

```text
Add a public hostname
```

Điền:

| Field     | Value             |
| --------- | ----------------- |
| Subdomain | camdo             |
| Domain    | nhukhiem.id.vn    |
| Type      | HTTP              |
| URL       | http://wordpress:80   |

<img width="1891" height="974" alt="image" src="https://github.com/user-attachments/assets/5e7c4aa6-2a02-4ef8-a181-ed4d6ebdef66" />


---

### **8.7: Test Truy Cập**
#### Bài viết 1 : Giới thiệu bản thân 

<img width="1899" height="1010" alt="image" src="https://github.com/user-attachments/assets/d85d778a-34c0-42dc-8d85-bd7204340d77" />

#### Bài viết 2 : Giới thiệu ngành học

<img width="1864" height="991" alt="image" src="https://github.com/user-attachments/assets/5c436474-8e50-4bd0-b910-e3958a010361" />

---

## 🛠️ QUẢN LÝ DOCKER

### **Xem trạng thái**
```bash
docker compose ps
```

### **Xem logs**
```bash
# Logs WordPress
docker compose logs -f wordpress

# Logs MariaDB
docker compose logs -f mariadb

# Logs phpMyAdmin
docker compose logs -f phpmyadmin
```

### **Tắt tất cả services**
```bash
docker compose down
```

### **Khởi động lại**
```bash
docker compose restart
```

### **Xóa containers nhưng giữ dữ liệu**
```bash
docker compose down
# Data vẫn ở volumes/
```

### **Xóa toàn bộ (cảnh báo!)**
```bash
docker compose down -v
# Xóa cả volumes → dữ liệu bị mất
```

---

## 📊 PHPYADMIN - Quản Lý Database

### **Truy cập**
- URL: `http://localhost:8081`
- Username: `wordpress_user`
- Password: `wordpress_password_456`

### **Các thao tác**
1. Xem cơ sở dữ liệu WordPress
2. Backup database (Export)
3. Chỉnh sửa dữ liệu (tuỳ chọn)

---

## 🔍 TROUBLESHOOTING

### **Lỗi: Port 80 đã bị sử dụng**
```bash
# Cách 1: Dùng port khác
# Sửa .env: WORDPRESS_PORT=8080
# Truy cập: http://localhost:8080

# Cách 2: Tìm process sử dụng port 80
sudo lsof -i :80
sudo kill -9 <PID>
```

### **Lỗi: Permission denied (volumes/)**
```bash
# Phân quyền thư mục
sudo chown -R $USER:$USER volumes/
chmod -R 755 volumes/
```

### **Lỗi: MariaDB không khởi động**
```bash
# Xem logs
docker compose logs mariadb

# Xóa volumes và chạy lại
rm -rf volumes/mariadb
docker compose up -d
```

### **Lỗi: WordPress không kết nối database**
```bash
# Kiểm tra biến môi trường trong .env
# Đảm bảo MYSQL_HOST = mariadb
# Restart WordPress
docker compose restart wordpress
```

### **Lỗi: Cloudflare Tunnel không hoạt động**
```bash
# Kiểm tra tunnel đang chạy
cloudflared tunnel list

# Xem log tunnel
cat cloudflare.log

# Kiểm tra DNS đã cập nhật
nslookup wordpress.<your-domain.com>
```

---

## 💾 BACKUP DỮ LIỆU

### **Backup MySQL**
```bash
# Export database
docker compose exec mariadb mysqldump -u root -p${MYSQL_ROOT_PASSWORD} \
  ${MYSQL_DATABASE} > backup_$(date +%Y%m%d_%H%M%S).sql
```

### **Backup WordPress Files**
```bash
# Copy volumes folder
cp -r volumes/ volumes_backup_$(date +%Y%m%d)
```

---

## 📝 NHẬN XÉT VỀ WORDPRESS MÃ NGUỒN MỞ

> Phần này hãy viết nhận xét của bạn dựa trên kinh nghiệm sau khi hoàn thành bài tập

### Những điểm nên nhận xét:

1. **Tính dễ sử dụng**
   - WordPress có giao diện thân thiện không?
   - Có khó để tạo content không?
   - Cần bao lâu để làm quen?

2. **Tài nguyên tiêu tốn**
   - Chiếm bao nhiêu CPU?
   - Chiếm bao nhiêu RAM?
   - Chiếm bao nhiêu disk space?
   - Có cần tối ưu hóa không?

3. **Tính linh hoạt**
   - Dễ custom theme/plugin không?
   - Plugin ecosystem có lớn không?
   - Có giới hạn nào không?

4. **Ưu điểm**
   - Dễ cài đặt, setup nhanh
   - Có hỗ trợ lớn từ cộng đồng
   - Miễn phí, mã nguồn mở
   - Có sẵn nhiều tính năng (SEO, comments, media, ...)
   - Tích hợp tốt với các dịch vụ bên ngoài

5. **Nhược điểm**
   - Security cần chú ý (plugins lỏng lẻo)
   - Performance có thể chậm nếu không optimize
   - Database có thể phình to
   - Dependency on plugins (risky)
   - Khó custom nếu không biết PHP

6. **So sánh với CMS khác**
   - So với Joomla, Drupal, ...
   - So với static site generator (Hugo, Jekyll, ...)
   - So với custom code từ đầu

---

## 📚 TÀI LIỆU THAM KHẢO

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [WordPress Official](https://wordpress.org/)
- [Cloudflare Tunnel Guide](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
- [MariaDB Documentation](https://mariadb.com/docs/)

---

## ✅ CHECKLIST HOÀN THÀNH

- [ ] Cài Docker & Docker Compose
- [ ] Setup file docker compose.yml
- [ ] Cấu hình .env
- [ ] Chạy docker compose up -d
- [ ] Truy cập WordPress tại http://localhost
- [ ] Cài đặt WordPress
- [ ] Tạo bài viết #1: Giới thiệu bản thân
- [ ] Tạo bài viết #2: Giới thiệu ngành học
- [ ] Cài Cloudflare CLI
- [ ] Tạo Cloudflare Tunnel
- [ ] Public website lên internet
- [ ] Test truy cập từ bên ngoài
- [ ] Viết nhận xét về WordPress
- [ ] Commit code lên Git (không commit .env, volumes/)
- [ ] Submit bài tập

---

## 📧 Liên Hệ & Hỗ Trợ

Nếu gặp vấn đề:
1. Kiểm tra phần **Troubleshooting** ở trên
2. Xem logs: `docker compose logs -f`
3. Tìm kiếm trên Google/Stack Overflow
4. Hỏi giảng viên hoặc nhóm

---

**Chúc bạn hoàn thành bài tập thành công! 🎉**
