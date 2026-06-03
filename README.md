# portfolio-infra

Dịch vụ **dùng chung khi chạy Docker**, trên network `portfolio_net`:

- **SeaweedFS** — `portfolio_seaweedfs`, S3 API `:8333`, master UI `:9333`, filer UI `:8888`, admin UI `:23646` (credential trong `docker-compose.yml`).
- **Redis** — `portfolio_redis`, `:6379`.

Các project khác (ví dụ **portfolio-nodejs**, **portfolio-be**) **không nhân bản object storage**; chỉ cần join `portfolio_net` và trỏ `STORAGE_ENDPOINT` / `AWS_ENDPOINT` tới `http://seaweedfs:8333`.

Thứ tự gợi ý: `docker network create portfolio_net` (một lần) → `docker compose up -d` trong thư mục này → sau đó up stack từng app.

## Migrate dữ liệu từ MinIO cũ sang SeaweedFS

Volume MinIO cũ không dùng trực tiếp được cho SeaweedFS. Nếu đã có dữ liệu trong bucket `portfolio` hoặc `chat`, chạy migration một lần:

```bash
docker compose up -d seaweedfs legacy-minio
docker compose run --rm storage-migrate
docker compose stop legacy-minio
```

Sau khi kiểm tra media cũ đọc được từ SeaweedFS, có thể chạy stack bình thường chỉ với `docker compose up -d`.
