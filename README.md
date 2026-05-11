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

- **Docker** (phiên bản 20.10+)
- **Docker Compose** (phiên bản 1.29+)
- **Ubuntu** (Ubuntu 24.04.4 LTS)
- **Cloudflare** (để public website)

### Kiểm tra phiên bản:
```bash
docker --version
docker compose --version
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

**Output:**
```
Creating wordpress_mariadb ... done
Creating wordpress_phpmyadmin ... done
Creating wordpress_app ... done
```

### **BƯỚC 5: Kiểm tra các services**

```bash
# Xem trạng thái containers
docker compose ps

# Xem logs (nếu cần debug)
docker compose logs -f wordpress
```

**Output mong đợi:**
```
NAME                    STATUS              PORTS
wordpress_mariadb       Up (healthy)        3306->3306
wordpress_phpmyadmin    Up                  8081->80
wordpress_app           Up                  80->80
```

---

## 🌐 Truy Cập Các Dịch Vụ

| Dịch vụ | URL | Tài khoản | Mật khẩu |
|---------|-----|----------|---------|
| **WordPress** | `http://localhost` | Sẽ cài ở bước 6 | Sẽ cài ở bước 6 |
| **phpMyAdmin** | `http://localhost:8081` | `wordpress_user` | `wordpress_password_456` |
| **MySQL Root** | MySQL CLI | `root` | `root_password_123` |

---

## 📝 BƯỚC 6: Cài Đặt WordPress

1. Mở trình duyệt, truy cập: **http://localhost**

2. Chọn ngôn ngữ → Tiếp theo

3. Điền thông tin:
   - **Site Title**: Tên website của bạn
   - **Username**: Tên user admin (ví dụ: `admin`)
   - **Password**: Mật khẩu mạnh
   - **Email**: Email của bạn

4. Nhấp **Install WordPress**

5. Đăng nhập vào WordPress với user vừa tạo

✅ **Xong!** WordPress đã sẵn sàng

---

## ✍️ BƯỚC 7: Tạo 2 Bài Viết

### **Bài Viết #1: Giới Thiệu Bản Thân**

1. Đăng nhập WordPress
2. **Dashboard** → **Posts** → **Add New**
3. Tiêu đề: `Giới Thiệu Bản Thân`
4. Nội dung:
   - Thông tin cá nhân (Họ tên, Ngày sinh, MSSV, ...)
   - Sở thích, mục tiêu
   - Kỹ năng
   - Thêm **ảnh** của bạn (Upload image)
   - Thêm **video** (YouTube embed hoặc upload)
   - Thêm **audio** (nếu có)

5. Nhấp **Publish**

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

✅ **Xong!** Cả 2 bài viết đã online

---

## 🌍 BƯỚC 8: Public Website Với Cloudflare Tunnel

### **8.1: Cài Đặt Cloudflare CLI (cloudflared)**

```bash
# Download cloudflared
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb

# Cài đặt
sudo dpkg -i cloudflared.deb

# Kiểm tra
cloudflared --version
```

### **8.2: Đăng Nhập Cloudflare**

```bash
cloudflared tunnel login
```

Lệnh này sẽ:
1. Mở trình duyệt → Cloudflare login
2. Chọn domain của bạn
3. Tạo certificate tự động trong `~/.cloudflared/`

### **8.3: Tạo Tunnel**

```bash
# Tạo tunnel mới
cloudflared tunnel create wordpress-tunnel
```

Output:
```
Tunnel credentials written to ~/.cloudflared/xxxxxxx.json
Tunnel wordpress-tunnel created successfully.
```

### **8.4: Cấu Hình Tunnel (file config)**

Tạo file cấu hình:

```bash
# Tạo file config (nếu chưa có)
nano ~/.cloudflared/config.yml
```

Paste nội dung:

```yaml
tunnel: wordpress-tunnel
credentials-file: ~/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: wordpress.<your-domain.com>
    service: http://localhost:80
  - service: http_status:404
```

**Thay thế:**
- `wordpress.<your-domain.com>` → subdomain của bạn (ví dụ: `wordpress.example.com`)
- `<TUNNEL_ID>` → ID tunnel từ file json

### **8.5: Chạy Tunnel**

```bash
# Chạy tunnel
cloudflared tunnel run wordpress-tunnel
```

Hoặc chạy ở background:

```bash
nohup cloudflared tunnel run wordpress-tunnel > cloudflare.log 2>&1 &
```

### **8.6: Cập Nhật DNS trên Cloudflare**

1. Vào **Cloudflare Dashboard**
2. Chọn domain của bạn
3. **DNS** → **Records**
4. Thêm CNAME record:
   - **Type**: CNAME
   - **Name**: `wordpress` (hoặc subdomain tùy chọn)
   - **Target**: `wordpress-tunnel.cfargotunnel.com`
   - **Proxy status**: Proxied (nút cam)

### **8.7: Test Truy Cập**

```bash
# Kiểm tra tunnel status
cloudflared tunnel info wordpress-tunnel

# Hoặc vào: https://wordpress.<your-domain.com>
```

✅ **Website của bạn đã online!**

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

## 📄 License

Dự án này tạo cho mục đích học tập - Bài tập môn TEE0421

**Ngày tạo**: 2026-05-11  
**Hạn chót**: 2026-05-12 23:59

---

**Chúc bạn hoàn thành bài tập thành công! 🎉**
