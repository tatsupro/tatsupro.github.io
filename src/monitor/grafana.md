# Grafana

Observability and data visualization platform.

## Cài đặt

Thêm service sau vào `docker-compose.yml`:

```yml
grafana:
  image: grafana/grafana:latest
  restart: unless-stopped
  volumes:
    - ./data/var/lib/grafana:/var/lib/grafana
  ports:
    - "3000:3000"
```

Sau đó đăng nhập với tên người dùng và mật khẩu mặc định đều là `admin`.
