# portfolio-infra

Dịch vụ **dùng chung khi chạy Docker**, trên network `portfolio_net`:

- **MinIO** — `portfolio_minio`, API `:9000`, console `:9001` (credential trong `docker-compose.yml`).
- **Redis** — `portfolio_redis`, `:6379`.

Các project khác (ví dụ **portfolio-nodejs**, **portfolio-be**) **không nhân bản MinIO**; chỉ cần join `portfolio_net` và trỏ `MINIO_ENDPOINT` / `AWS_ENDPOINT` tới `http://portfolio_minio:9000`.

Thứ tự gợi ý: `docker network create portfolio_net` (một lần) → `docker compose up -d` trong thư mục này → sau đó up stack từng app.
