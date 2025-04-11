## Supervisor là gì?

Supervisor là một công cụ dùng để **quản lý tiến trình (process manager)** trên Linux.

Hiểu đơn giản, Supervisor giúp bạn chạy các chương trình như các service (kiểu giống như systemd hoặc service trong Linux), và đảm bảo nếu chương trình bị crash hoặc server reboot thì Supervisor sẽ tự khởi động lại nó.

> **Dùng Supervisor khi nào?**
>
> * Chạy queue worker (ví dụ Laravel Queue).
> * Chạy websocket server.
> * Chạy bot hoặc cron job lâu dài.
> * Chạy service backend không phải là system service.

## Ưu điểm của Supervisor

* Dễ cài đặt và cấu hình.
* Quản lý nhiều process.
* Restart tự động nếu process bị lỗi.
* Log stdout và stderr tiện theo dõi.
* Có thể start/stop/restart từng process mà không ảnh hưởng cái khác.




## Giải thích tổng thể:

| Mục đích                                                     | Section                       |
| --------------------------------------------------------------- | ----------------------------- |
| Supervisor cần socket Unix để `supervisorctl`giao tiếp    | `[unix_http_server]`        |
| Supervisor cần expose RPC interface để điều khiển process | `[rpcinterface:supervisor]` |
| `supervisorctl`cần biết cách kết nối socket nào         | `[supervisorctl]`           |
| Định nghĩa các process php-fpm, queue worker                | `[program:*]`               |
