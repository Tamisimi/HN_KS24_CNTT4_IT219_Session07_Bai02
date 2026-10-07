# Bài 2 — Quản trị Tường lửa UFW cho Cụm Dịch vụ Multi-port

## Mục tiêu
Cấu hình UFW: chặn mặc định incoming, mở SSH (22), HTTP (80), Spring Boot (8082); không mở MySQL (3306).

## Các lệnh đã thực hiện

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 8082/tcp

sudo ufw enable
sudo ufw status verbose
```

## Kết quả `sudo ufw status verbose`

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
8082/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
8082/tcp (v6)              ALLOW IN    Anywhere (v6)
```

## Giải thích ngắn

| Cổng | Trạng thái | Lý do |
|------|------------|--------|
| 22/tcp | ALLOW IN | SSH quản trị từ xa |
| 80/tcp | ALLOW IN | Nginx HTTP |
| 8082/tcp | ALLOW IN | Spring Boot kiểm tra từ xa |
| 3306/tcp | Không có trong danh sách | Bị chặn bởi `default deny incoming` — MySQL không public Internet |

Chính sách mặc định `deny incoming` + `allow outgoing` đảm bảo chỉ các cổng được `allow` tường minh mới vào được máy chủ.
