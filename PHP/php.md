```php
continue 2; // Dừng 2 vòng lặp lồng nhau
```

PDO

PHP-FPM là gì?

## Swoole và roadrunner
- Gọi là application server
- đều là công cụ mạnh mẽ giúp tăng tốc Laravel (hoặc các framework PHP khác) bằng cách loại bỏ PHP-FPM truyền thống và tối ưu xử lý request!

Application Server cho PHP:
- Thay thế PHP-FPM, giúp PHP chạy như server lâu dài, không phải load lại framework mỗi request.

Giữ framework trong RAM (Resident Memory):
- Boot framework 1 lần duy nhất khi khởi động server.
- Cache class, service provider, config... giúp giảm đáng kể thời gian xử lý request.

Worker Pool (đa tiến trình xử lý song song):
- Chạy nhiều worker process để xử lý nhiều request đồng thời (giống như load balancing nội bộ).
- Tự động restart worker khi lỗi hoặc đạt giới hạn request để tránh memory leak.

Kết nối persistent (giữ kết nối lâu dài):
- Database, Redis, cache... không phải kết nối lại mỗi request, giúp tiết kiệm thời gian + tài nguyên.

Production-ready:
- Cả hai đều ổn định và tối ưu cho môi trường production.
- Dễ dàng tích hợp với Docker, Kubernetes, hoặc các hệ thống scale lớn.

Tương thích với Laravel Octane:
- Laravel Octane hỗ trợ cả Swoole và RoadRunner, giúp bạn dễ dàng chạy Laravel với hiệu suất cao.

Cấu hình linh hoạt:
- Cho phép điều chỉnh số lượng worker, timeout, memory limit, và các cài đặt khác để tối ưu theo nhu cầu cụ thể.

## RoadRunner và Worker là gì trong PHP?
**Thông thường, PHP chạy qua PHP-FPM, mỗi request sẽ:**
1. Load framework (Laravel, Symfony...).
2. Kết nối database, cache....
3. Xử lý request và trả response.
4. Đóng kết nối và giải phóng bộ nhớ.
Vấn đề là quá trình này tốn thời gian, đặc biệt với ứng dụng lớn!

**Worker là gì?**
- Worker là một tiến trình PHP độc lập mà server tạo ra để xử lý request.
- Khi có request đến, RoadRunner sẽ gửi request đó cho 1 worker, worker này sẽ chạy Laravel, trả response rồi sẵn sàng nhận request tiếp theo.

**Worker Pool là gì?**
- Worker pool là tập hợp nhiều worker chạy song song.
- RoadRunner fork ra nhiều tiến trình PHP khi khởi động, mỗi tiến trình là 1 worker độc lập.
- Có thể cấu hình số lượng worker theo nhu cầu (ví dụ: 10 worker).
- Khi có request:
  - RoadRunner nhận request từ client.
  - Phân phối request:
    - Nếu có worker rảnh, RoadRunner gửi ngay request đó cho worker.
    - Nếu tất cả worker bận, request phải xếp hàng đợi.
  - Worker xử lý request, trả response về cho RoadRunner, rồi tiếp tục đợi request mới.
  
**Worker sinh ra để giải quyết:**
- Giữ trạng thái ứng dụng trong RAM (không phải load Laravel mỗi request).
- Tái sử dụng kết nối database, cache (không cần kết nối lại).
- Chia tải request bằng nhiều worker song song (giống load balancing).

**Quản lý Worker trong RoadRunner**
RoadRunner là một PHP application server chạy trên Go, hoạt động như một reverse proxy:
- Nhận request HTTP, chuyển request qua gRPC đến PHP worker.
- Worker pool giữ nhiều worker hoạt động song song để xử lý request nhanh hơn.
- Nếu worker bị lỗi, RoadRunner tự động restart worker để tránh gián đoạn.
- Tăng giảm số lượng worker linh hoạt tùy theo tải hệ thống.
```yaml
# roadrunner.yaml
http:
  address: 0.0.0.0:8000
  workers:
    command: "php artisan octane:start --server=roadrunner --host=0.0.0.0 --port=8000"
    pool:
      num_workers: 8            # Số lượng worker
      max_jobs: 100             # Số job tối đa mỗi worker trước khi restart
      allocate_timeout: 60s     # Thời gian tối đa cấp phát worker
      destroy_timeout: 60s      # Thời gian tối đa để kill worker

```

**Worker pool giúp:**
- Chạy song song n worker để xử lý nhiều request cùng lúc.
- Auto-scale: Khi tải cao, bạn có thể tăng số worker để chịu tải tốt hơn.
- Tự động khởi động lại worker khi đạt giới hạn (giải phóng memory leak).

**Khi nào nên dùng RoadRunner?**
- API tốc độ cao không cần WebSocket hoặc real-time phức tạp.
- Microservice với nhiều endpoint nhỏ, yêu cầu xử lý nhanh.
- Ứng dụng tải lớn cần scale linh hoạt mà vẫn giữ ổn định.
- Nếu cần real-time, WebSocket, hoặc nhiệm vụ bất đồng bộ, thì nên chọn **Swoole** thay vì **RoadRunner**!