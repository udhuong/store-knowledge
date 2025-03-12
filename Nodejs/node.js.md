**Không có thread natively theo cách truyền thống**
- Node.js là single-threaded theo mô hình event loop và non-blocking I/O.
- Worker Threads trong Node.js không hoàn toàn giống thread của Java vì chúng chạy trong process riêng biệt, nhưng vẫn chia sẻ dữ liệu thông qua Message Passing.
- Nếu cần xử lý I/O nhiều (API, web server), Node.js là lựa chọn mạnh mẽ nhờ event loop siêu nhanh.