## WebSocket là gì?

WebSocket là một giao thức giao tiếp hai chiều. Cho phép kết nối **liên tục** giữa client và server thông qua một kênh TCP duy nhất. Điều này giúp WebSocket hoạt động hiệu quả hơn so với HTTP, vốn yêu cầu client gửi request mới có thể nhận được phản hồi từ server.

### Cách hoạt động

WebSocket sử dụng  **HTTP handshake ban đầu** , nhưng sau đó chuyển đổi sang kết nối **duy trì liên tục** mà không cần gửi request nhiều lần như HTTP.

#### Quá trình thiết lập WebSocket

**1. Client gửi request mở kết nối WebSocket**

* Client gửi một HTTP request với phương thức `GET` đến server, yêu cầu mở kết nối WebSocket.
* Header đặc biệt có trong request:

```makefile
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

**2. Server phản hồi và nâng cấp kết nối**

* Nếu server hỗ trợ WebSocket, nó sẽ gửi phản hồi xác nhận và nâng cấp kết nối:

```makefile
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

**3. Kết nối WebSocket được thiết lập**

* Lúc này, kết nối giữa client và server là  **duy trì liên tục** , cho phép gửi/nhận dữ liệu mà không cần request mới.

**4. Truyền dữ liệu giữa client và server**

* Cả hai bên có thể gửi dữ liệu bất cứ lúc nào bằng các frame WebSocket.
* Dữ liệu có thể ở dạng **text** hoặc  **binary** .

**5. Đóng kết nối WebSocket**

* Client hoặc server có thể gửi tín hiệu để đóng kết nối (`close frame`).

## **WebSocket so với HTTP truyền thống**

| Đặc điểm            | WebSocket                                         | HTTP (AJAX, Polling)                               |
| ----------------------- | ------------------------------------------------- | -------------------------------------------------- |
| Kiểu kết nối         | Hai chiều (full-duplex)                          | Một chiều (client request → server response)    |
| Truyền dữ liệu       | Liên tục, không cần request mới              | Phải gửi request mới nhận được dữ liệu    |
| Overhead                | Chỉ cần thiết lập một lần                   | Tạo nhiều request nên overhead cao              |
| Độ trễ (Latency)     | Thấp                                             | Cao (do request-response cycle)                    |
| Tiêu thụ tài nguyên | Ít hơn (sử dụng một kết nối TCP duy nhất) | Nhiều hơn (nhiều request tạo nhiều kết nối) |

## **Ứng dụng của WebSocket**

WebSocket rất hữu ích cho các ứng dụng  **real-time** , bao gồm:

* Chat, nhắn tin (Facebook Messenger, WhatsApp Web, Slack...)
* Bảng điều khiển (Dashboard) cập nhật real-time
* Streaming dữ liệu (video, âm thanh, IoT, game online)
* Giao dịch tài chính (stock market, crypto trading)

## HTTP và HTTPS là gì?

### HTTP là gì?

**HTTP (HyperText Transfer Protocol)** là giao thức truyền tải siêu văn bản, được sử dụng để giao tiếp giữa trình duyệt web (client) và máy chủ web (server).

**Cách hoạt động của HTTP**

* Khi nhập một URL vào trình duyệt (ví dụ: `http://example.com`), trình duyệt sẽ gửi một **HTTP request** đến server.
* Server nhận yêu cầu, xử lý và gửi lại **HTTP response** chứa nội dung trang web.
* Trình duyệt hiển thị nội dung này cho người dùng.

**Đặc điểm của HTTP**

* Hoạt động theo mô hình  **request-response** .
* Không bảo mật, dữ liệu truyền qua mạng  **không được mã hóa** .
* Dễ bị tấn công kiểu  **man-in-the-middle (MITM)** , khiến hacker có thể đánh cắp thông tin.

### **HTTPS là gì?**

**HTTPS (HyperText Transfer Protocol Secure)** là phiên bản bảo mật của HTTP. Nó sử dụng **SSL/TLS** để mã hóa dữ liệu trước khi gửi đi, giúp bảo vệ thông tin người dùng.

 **Cách hoạt động của HTTPS**

* Khi trình duyệt truy cập một trang web sử dụng HTTPS (`https://example.com`), nó sẽ kiểm tra **chứng chỉ SSL/TLS** của website.
* Nếu chứng chỉ hợp lệ, trình duyệt và server thiết lập **kết nối an toàn** và tất cả dữ liệu trao đổi sẽ được  **mã hóa** .

**Đặc điểm của HTTPS**

* **Bảo mật dữ liệu** : Dữ liệu được mã hóa trước khi truyền, ngăn chặn hacker đánh cắp thông tin.
* **Xác thực website** : Người dùng có thể kiểm tra chứng chỉ SSL/TLS để biết website có đáng tin cậy hay không.
* **Cần chứng chỉ SSL/TLS** : Website phải có **SSL certificate** để sử dụng HTTPS.

## Socket.IO

**Socket.IO** là một thư viện JavaScript giúp thiết lập **giao tiếp hai chiều (full-duplex) theo thời gian thực** giữa client và server.

Nó được xây dựng dựa trên **WebSocket** nhưng cung cấp thêm nhiều tính năng mạnh mẽ như  **fallback (tự động chuyển sang polling nếu WebSocket không hoạt động), broadcast (gửi tin nhắn đến nhiều client), rooms (phòng chat riêng), namespaces (kênh giao tiếp riêng biệt), và auto-reconnect (tự động kết nối lại khi mất mạng)** .

### **Cấu trúc cơ bản**

Socket.IO gồm hai phần chính:

* **Thư viện client** : Chạy trên trình duyệt hoặc ứng dụng di động.
* **Thư viện server** : Chạy trên Node.js để xử lý kết nối.

Cả hai phần này giao tiếp với nhau thông qua **WebSocket** (hoặc fallback nếu cần).

### **Quá trình kết nối**

Khi một client kết nối đến server bằng Socket.IO, quy trình sau diễn ra:

1. **Handshake (Bắt tay ban đầu)**
   * Client gửi yêu cầu kết nối đến server.
   * Server phản hồi và thiết lập kết nối WebSocket hoặc fallback.
2. **Truyền dữ liệu**
   * Client và server có thể gửi tin nhắn hoặc dữ liệu theo sự kiện (`emit`, `on`).
3. **Xử lý mất kết nối**
   * Nếu kết nối bị mất (mất mạng, server crash...), Socket.IO hỗ trợ  **tự động kết nối lại** .

### **Kết luận**

**Socket.IO giúp xây dựng ứng dụng real-time dễ dàng hơn** so với WebSocket.

Hỗ trợ nhiều tính năng mạnh như  **Rooms, Namespaces, Broadcasting, Auto-Reconnect** .

**Được sử dụng phổ biến** trong  **chat apps, thông báo real-time, game, streaming** .

## TCP/UDP
