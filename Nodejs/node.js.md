### **Node.js là gì**

**Định nghĩa:**

* **Node.js là một môi trường runtime cho JavaScript** (không phải một ngôn ngữ lập trình).
* Nó cho phép  **chạy JavaScript bên ngoài trình duyệt** , trên máy chủ hoặc hệ thống backend.
* Node.js được xây dựng trên **V8 engine** (công cụ chạy JavaScript của Google Chrome).
* Sử dụng javascript và chạy thông dịch

📌 **Tóm lại:** Bạn có thể viết  **mã JavaScript thuần trong Node.js** , giống như khi chạy JavaScript trong trình duyệt, nhưng có thêm các API để làm việc với  **tệp tin, mạng, quy trình hệ thống...** .

**Node.js thuần hoạt động thế nào?**

* Node.js có **các module gốc (core modules)** như `http`, `fs`, `path`, `os`, `events` giúp bạn lập trình backend mà không cần framework như Express.js.
* Bạn có thể tạo một **web server, đọc/ghi file, kết nối database** chỉ bằng JavaScript.

```javascript
// Khởi tạo server đơn giản
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, Node.js!');
});

server.listen(3000, () => console.log('Server running on port 3000'));

// Tạo một API JSON đơn giản
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Hello, API from Node.js!' }));
});

server.listen(4000, () => console.log('API running on port 4000'));


```

* Dùng **module `http`** để tạo server.
* Server lắng nghe trên cổng `3000` và phản hồi "Hello, Node.js!".

**Đặc điểm nổi bật:**

* **Single-thread, Non-blocking I/O** : Hoạt động theo mô hình đơn luồng, xử lý bất đồng bộ giúp tăng hiệu suất.
* **Event-driven** : Hoạt động dựa trên cơ chế sự kiện, giúp xử lý nhiều yêu cầu cùng lúc mà không cần tạo luồng mới.
* **Sử dụng JavaScript** : Dành cho những ai đã quen với JavaScript trên trình duyệt.
* **Có npm (Node Package Manager)** : Hỗ trợ hàng ngàn thư viện giúp phát triển nhanh hơn.

**Node.js dùng để làm gì?**

- **Xây dựng API RESTful** (Express.js, Fastify, NestJS)
- **Ứng dụng real-time** (Chat app, game, livestream)
- **Viết server cho ứng dụng web/mobile** (React, Vue, Angular, Flutter)
- **Xử lý file, stream video, audio**
- **Phát triển Microservices**
- **Viết Tool CLI (Command Line Interface)**

Node.js dựa trên mô hình **Event Loop** (Vòng lặp sự kiện) để xử lý tác vụ bất đồng bộ mà không cần tạo nhiều luồng, giúp tiết kiệm tài nguyên.

**Cách hoạt động của Node.js**

* **Nhận yêu cầu** từ client.
* **Gửi tác vụ I/O (đọc file, gọi API, truy vấn DB, v.v.) vào hàng đợi** và tiếp tục xử lý các yêu cầu khác mà không chờ kết quả.
* **Khi tác vụ hoàn thành** , nó sẽ gửi phản hồi về cho client thông qua cơ chế **callback** hoặc  **Promise/async-await** .

**Framework phổ biến:**

| Framework            | Mô tả                                        |
| -------------------- | ---------------------------------------------- |
| **Express.js** | Framework phổ biến nhất, nhẹ, dễ dùng    |
| **NestJS**     | Mạnh mẽ, hỗ trợ TypeScript, hướng module |
| **Fastify**    | Nhanh hơn Express, tối ưu hiệu suất       |
| **Koa.js**     | Nhẹ hơn Express, linh hoạt                  |
| **Socket.io**  | Hỗ trợ WebSocket, real-time chat, game       |

#### Express.js

**Express.js là gì? - Framework Backend phổ biến nhất**

* **Express.js** là một  **web framework nhẹ dành cho Node.js** , giúp xây dựng **REST API** hoặc **web server** một cách nhanh chóng.
* Được sử dụng rộng rãi vì **đơn giản, linh hoạt** và có  **cộng đồng lớn** .

✅ **Ưu điểm của Express.js**

* **Nhẹ, đơn giản, linh hoạt** , dễ học, dễ dùng.
* **Thư viện middleware phong phú** (body-parser, morgan, cors, helmet...).
* **Hỗ trợ TypeScript** , nhưng không bắt buộc.

❌ **Nhược điểm của Express.js**

* **Không có kiến trúc mặc định** , dễ làm code trở nên lộn xộn khi dự án lớn.
* **Tự quản lý cấu trúc code** , không có hệ thống module như NestJS.
* **Hiệu suất không cao** bằng Fastify.

#### Fastify

**Fastify là gì? - Framework hiệu suất cao**

* **Fastify** là một **web framework hiệu suất cao** dành cho Node.js, tập trung vào  **tốc độ và tối ưu tài nguyên** .
* Fastify được thiết kế để nhanh hơn **Express.js** bằng cách sử dụng kiến trúc bất đồng bộ và  **serialization nhanh hơn** .

✅ **Ưu điểm của Fastify**

* **Hiệu suất cao hơn Express.js** (~2x nhanh hơn).
* **Ít tốn tài nguyên hơn** , tốt cho ứng dụng quy mô lớn.
* **Tích hợp sẵn schema validation** giúp kiểm soát dữ liệu tốt hơn.

❌ **Nhược điểm của Fastify**

* **Ít middleware hơn Express.js** , cần tự viết nhiều hơn.
* **Cộng đồng nhỏ hơn Express.js** nhưng đang phát triển.

## **Nodejs ORM**

Sequelize, Prisma

## **Không có thread natively theo cách truyền thống**

- Node.js là single-threaded theo mô hình event loop và non-blocking I/O.
- Worker Threads trong Node.js không hoàn toàn giống thread của Java vì chúng chạy trong process riêng biệt, nhưng vẫn chia sẻ dữ liệu thông qua Message Passing.
- Nếu cần xử lý I/O nhiều (API, web server), Node.js là lựa chọn mạnh mẽ nhờ event loop siêu nhanh.

Event Loop + Task Queue + async/await chính là bộ ba quyền lực giúp Node.js xử lý hàng ngàn kết nối đồng thời mà không bị chặn! 🔥

## Bất đồng bộ

**Phân loại:**

* Sử dụng Callbacks (ES5)
* Sử dụng Promises (ES6)
* **Sử dụng Async/Await (ES8)**

**Chi tiết:**

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


### I/O (Đọc/Ghi File)

Node.js cung cấp module **`fs` (File System)** để đọc, ghi, chỉnh sửa file một cách dễ dàng. Module này hỗ trợ cả **đồng bộ (Blocking)** và  **bất đồng bộ (Non-blocking)** .

```javascript
const data = fs.readFileSync('example.txt', 'utf8'); // Đọc file đồng bộ
fs.readFile('example.txt', 'utf8', () -> {}); // Đọc file bất đồng bộ, dùng callback
const data = await fs.readFile('example.txt', 'utf8'); // Dùng async/await

fs.writeFileSync('output.txt', 'Hello Node.js!'); // Ghi file đồng bộ
fs.writeFile('output.txt', 'Hello Async Node.js!', () -> {}); // Ghi file bất đồng bộ, dùng callback
await fs.writeFile('output.txt', 'Hello Async/Await Node.js!'); / Dùng async/await

fs.appendFileSync('output.txt', '\nThêm nội dung mới!');
fs.appendFile('output.txt', '\nThêm nội dung mới!', () -> {});
await fs.appendFile('output.txt', '\nThêm nội dung mới với async/await!');

fs.unlinkSync('output.txt'); // Xóa file
fs.unlink('output.txt', () -> {}}) // xóa file bất đồng bộ, dùng callback
await fs.unlink('output.txt');

// Streams – Đọc/Ghi File Lớn
const readStream = fs.createReadStream('bigfile.txt', { encoding: 'utf8' });
const writeStream = fs.createWriteStream('bigfile_copy.txt');
```
