**Không có thread natively theo cách truyền thống**
- Node.js là single-threaded theo mô hình event loop và non-blocking I/O.
- Worker Threads trong Node.js không hoàn toàn giống thread của Java vì chúng chạy trong process riêng biệt, nhưng vẫn chia sẻ dữ liệu thông qua Message Passing.
- Nếu cần xử lý I/O nhiều (API, web server), Node.js là lựa chọn mạnh mẽ nhờ event loop siêu nhanh.


Event Loop + Task Queue + async/await chính là bộ ba quyền lực giúp Node.js xử lý hàng ngàn kết nối đồng thời mà không bị chặn! 🔥


## Bất đồng bộ
- Callback là một hàm được truyền vào như tham số cho một hàm khác, và sẽ được gọi lại (call back) khi tác vụ hoàn thành.
- Named Callbacks (Hàm có tên):
  - Tách các callback thành hàm riêng.
- Promise (chaining) (then/catch):
  - Chuỗi các tác vụ với .then và .catch.
- async/await:
  - Viết mã bất đồng bộ như mã đồng bộ, cực kỳ dễ đọc và gọn gàng.
  - async: Định nghĩa một hàm bất đồng bộ — hàm này luôn trả về một Promise.
  - await: Tạm dừng việc thực thi của hàm async cho đến khi Promise được giải quyết (resolved). Sau đó, mã phía sau mới tiếp tục chạy.
  - Ghi nhớ: await chỉ dùng được bên trong hàm async!