
## Process (Tiến trình) && Thread (Luồng)

### Process  
- Là gì?: Một tiến trình là một đơn vị hoạt động độc lập, có bộ nhớ và tài nguyên riêng biệt.
- Hoạt động thế nào?: Mỗi tiến trình chạy trong không gian bộ nhớ riêng, không chia sẻ bộ nhớ trực tiếp với tiến trình khác (trừ khi bạn dùng kỹ thuật như IPC - Interprocess Communication).
- Trong PHP: PHP truyền thống là ngôn ngữ không có multithreading, mỗi request của người dùng tạo ra một process PHP mới.

Ưu điểm: Độc lập, ổn định (nếu một process crash, các process khác không bị ảnh hưởng).
Nhược điểm: Tốn tài nguyên, khởi tạo chậm hơn so với thread.

### Child Processes  
- Là gì?: Child processes là các tiến trình riêng biệt được tạo ra từ process chính. Mỗi tiến trình có bộ nhớ và tài nguyên độc lập, chạy hoàn toàn tách biệt.
- Hoạt động thế nào?: Giao tiếp giữa process cha và con thông qua IPC (Inter-Process Communication), thường bằng pipes hoặc message passing.
- Dùng khi nào?: Khi bạn cần chạy tác vụ lớn mà muốn cách ly hoàn toàn với process chính (ví dụ spawn nhiều dịch vụ con).
Ưu điểm: Hoàn toàn độc lập, tránh crash toàn hệ thống nếu lỗi xảy ra.
Nhược điểm: Tốn tài nguyên, tạo process mới chậm hơn so với thread.

### Thread  
- Là gì?: Thread là đơn vị nhỏ hơn process, chạy bên trong process và chia sẻ cùng không gian bộ nhớ.
- Hoạt động thế nào?: Các thread trong cùng một process có thể truy cập chung bộ nhớ, dễ dàng trao đổi dữ liệu mà không cần IPC.
- Trong PHP (với Swoole): PHP không hỗ trợ luồng natively, nhưng Swoole cung cấp cơ chế tạo luồng bằng Swoole\Coroutine hoặc Swoole\Server.

Ưu điểm: Nhẹ, tạo nhanh, giao tiếp dễ dàng.
Nhược điểm: Dễ bị lỗi race condition nếu không quản lý vùng nhớ kỹ.

**So sánh Process và Thread**
| **Tiêu chí**         | **Process**                                     | **Thread**                                          |
| -------------------- | ----------------------------------------------- | --------------------------------------------------- |
| **Bộ nhớ**           | Riêng biệt                                      | Chia sẻ trong cùng process                          |
| **Tài nguyên**       | Tốn nhiều tài nguyên (RAM, CPU)                 | Nhẹ, dùng ít tài nguyên hơn                         |
| **Thời gian tạo**    | Chậm hơn do cần cấp phát bộ nhớ riêng           | Nhanh vì dùng bộ nhớ đã có                          |
| **Độ an toàn**       | Cao (process chết không ảnh hưởng process khác) | Thấp hơn (dễ dính lỗi tranh chấp bộ nhớ)            |
| **Khả năng mở rộng** | Tốt cho xử lý song song, scale theo nhiều CPU   | Tốt cho tác vụ nhẹ, cần xử lý đồng thời trong 1 CPU |

### Event Loop (Vòng lặp sự kiện)
- Là gì?: Event loop là cơ chế giúp Node.js xử lý các tác vụ bất đồng bộ mà không cần nhiều thread. Nó lắng nghe sự kiện, khi sự kiện xảy ra thì gọi callback tương ứng.
- Hoạt động thế nào?: Khi gặp tác vụ I/O (như đọc file, truy vấn DB), Node.js sẽ đẩy tác vụ đó sang thread pool hoặc hệ thống, rồi tiếp tục xử lý các phần còn lại. Khi tác vụ hoàn thành, callback - được đẩy vào hàng đợi và chạy khi event loop rảnh.
- Dùng khi nào?: Khi cần xây dựng server hoặc ứng dụng xử lý nhiều kết nối cùng lúc mà không muốn block chương trình.

Ưu điểm: Nhẹ, nhanh, tối ưu xử lý I/O.
Nhược điểm: Nếu có tác vụ CPU nặng, event loop sẽ bị block (trừ khi dùng worker threads).

Ví dụ với Node.js Event Loop:
```javascript
console.log('Start');

setTimeout(() => {
    console.log('Callback executed');
}, 1000);

console.log('End');
```
Kết quả
```txt
Start  
End  
Callback executed (sau 1 giây)
```

**Event Loop chạy theo từng giai đoạn (phases):**
- Timers Phase: Xử lý setTimeout, setInterval.
- I/O Phase: Xử lý các tác vụ I/O (đọc/ghi file, request).
- Microtask Queue: Chạy các promise, process.nextTick (ưu tiên cao nhất).
- Close Phase: Đóng các kết nối, dọn dẹp tài nguyên.
- 📌 Mẹo nhớ nhanh: Timers → I/O → Microtasks → Close 🎯

**Khi nào nên dùng Event Loop?**
Event Loop rất mạnh trong các trường hợp cần xử lý nhiều tác vụ đồng thời, nhưng không muốn tạo quá nhiều thread hoặc process.

🔸 Web Server:
Node.js chạy HTTP server bằng Event Loop, xử lý hàng nghìn kết nối cùng lúc mà không cần tạo thread cho mỗi kết nối.

🔸 Ứng dụng Real-time:
Chat app, WebSocket, game online, nơi có nhiều sự kiện liên tục xảy ra (tin nhắn, di chuyển, cập nhật trạng thái).

🔸 I/O nhiều, CPU nhẹ:
Khi ứng dụng chủ yếu làm I/O (đọc/ghi file, gọi API, truy vấn DB) mà không cần xử lý nặng về CPU.


**Ưu và nhược điểm của Event Loop**
🔸 Ưu điểm:
Nhẹ, nhanh: Không cần tạo quá nhiều thread, tiết kiệm tài nguyên.
Đơn giản: Dễ viết mã bất đồng bộ với callback hoặc async/await.
Hiệu quả cho I/O: Đặc biệt mạnh khi xử lý nhiều kết nối đồng thời.

🔸 Nhược điểm:
CPU-bound tasks kém hiệu quả: Tác vụ nặng (ví dụ mã hóa, phân tích dữ liệu lớn) có thể block event loop.
Khó debug: Mã bất đồng bộ nhiều tầng có thể gây khó hiểu, dễ xảy ra callback hell (trừ khi dùng Promise/async-await).

## Task queue:
```js
console.log('Start');

setTimeout(() => {
    console.log('Inside timeout');
}, 1000);

console.log('End');
```
**Quy trình hoạt động của Event Loop cho đoạn mã trên:**
Chạy tác vụ đồng bộ (Main Thread):
- console.log('Start'): In ngay ra màn hình → Không qua Event Loop.
- Gặp setTimeout: Node đăng ký timer với libuv, đặt thời gian là 1000ms.
➜ Callback (console.log('Inside timeout')) được đưa vào Timer Queue và chờ hết thời gian.
- console.log('End'): In ngay ra màn hình → Không qua Event Loop.

Chờ Timer kết thúc:
- Timer chạy độc lập bên ngoài, Event Loop không chờ — Node tiếp tục xử lý các tác vụ khác nếu có.
- Sau 1 giây, timer hoàn thành, callback (console.log('Inside timeout')) được đẩy vào Task Queue.

Event Loop xử lý Queue:
- Sau khi toàn bộ tác vụ đồng bộ đã chạy xong, Event Loop kiểm tra Task Queue.
- Callback từ setTimeout đã chờ sẵn → Event Loop lấy ra và chạy.

## Multi-process và Worker Pool

- Multi-process: Tạo nhiều tiến trình (process) độc lập để xử lý song song.
  - Chỉ là kỹ thuật chạy nhiều process song song.
  - Mỗi process là một chương trình riêng biệt, có bộ nhớ riêng, không chia sẻ dữ liệu trực tiếp với process khác
  - Sau khi xử lý xong, mỗi process sẽ đóng lại hoặc chờ request mới
  - Multi-process giống như mở 100 tab trình duyệt độc lập, mỗi tab hoạt động tách biệt nhau.
- Worker pool: Là một tập hợp các worker (process hoặc thread) chạy sẵn để xử lý công việc.
  - Là cách tổ chức nhiều process hoặc thread thành nhóm, để tái sử dụng và tránh lãng phí tài nguyên.
  - có request pool sẽ lấy 1 worker ra xử lý, xong thì trả lại vào pool để tái sử dụng.
  - Worker pool giống như quầy phục vụ với 10 nhân viên, ai rảnh thì nhận khách, xong việc thì quay lại chờ!
- Các process là cô lập ko thể truy cập lẫn nhau:
  - Chỉ có thể chia sẻ qua bên thứ 3 hay cache, db, queue, ...

## Thread pool 


## Worker threads
- Là gì?: Worker threads là các luồng độc lập, chạy song song với luồng chính, giúp thực hiện các tác vụ nặng mà không làm block ứng dụng chính.
- Hoạt động thế nào?: Chúng chạy trong cùng một process, nhưng có stack và bộ nhớ riêng. Dữ liệu được truyền qua message passing (gửi/nhận tin nhắn).
- Dùng khi nào?: Khi bạn có tác vụ cần nhiều CPU (ví dụ mã hóa, xử lý ảnh) và không muốn làm chậm event loop chính.

Ưu điểm: Nhẹ, nhanh, không cần tạo process mới.
Nhược điểm: Không hoàn toàn tách biệt, dễ bị lỗi nếu không xử lý message cẩn thận.