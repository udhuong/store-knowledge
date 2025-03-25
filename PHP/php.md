## PHP là gì?

* PHP Là Ngôn Ngữ Thông Dịch (Interpreted Language)
* Ngôn ngữ thông dịch là loại ngôn ngữ được thực thi từng dòng một, khi nào chạy mới biên dịch
* PHP có thể nhúng trực tiếp vào HTML.

## PHP-FPM

**Định nghĩa**

- PHP FastCGI Process Manager — là một daemon (dịch vụ nền) giúp xử lý các yêu cầu PHP thông qua giao thức FastCGI.
- Cần kết hợp với web server như Nginx hoặc Apache!

**PHP-FPM hoạt động thế nào?**

- Web server nhận request:
  - Khi có request đến (ví dụ: /home), Nginx/Apache nhận yêu cầu trước.
- Gửi request qua FastCGI:
  - Web server không trực tiếp chạy PHP mà sẽ gửi request qua FastCGI bằng Unix socket hoặc TCP tới PHP-FPM.
- PHP-FPM xử lý request:
  - PHP-FPM có nhiều process worker để chạy mã PHP.
  - Mỗi request sẽ được giao cho 1 worker.
  - Worker sẽ boot framework, chạy mã PHP, truy vấn DB, xử lý logic, rồi trả lại kết quả.
- Trả kết quả về web server:
  - Worker xử lý xong thì trả response về web server, rồi từ đó trả lại cho trình duyệt.
  - Sau khi hoàn thành request, PHP-FPM hủy worker process (giải phóng bộ nhớ, kết nối DB, cache...).

**Luồng đi**

- User -> Nginx -> PHP-FPM -> Laravel -> MySQL/Redis -> Laravel -> PHP-FPM -> Nginx -> User

**Ưu điểm của PHP-FPM:**

- Ổn định và dễ cấu hình: Đã tồn tại lâu, được hỗ trợ rộng rãi.
- Worker pool: Quản lý nhiều process để xử lý request song song.
- Tự động restart process: Tránh memory leak bằng cách restart worker sau N request hoặc - khi đạt giới hạn RAM.
- Isolated request: Mỗi request chạy trong 1 worker riêng, không ảnh hưởng đến các request khác.

**Nhược điểm của PHP-FPM:**

- Mỗi request boot framework từ đầu: Dù request nhỏ hay lớn, PHP-FPM phải khởi động lại - Laravel, load toàn bộ file, tạo kết nối DB, cache... rồi mới xử lý.
- Không giữ kết nối lâu dài: DB, Redis, cache... đều bị đóng lại sau khi trả response, - làm tăng latency ở mỗi request.
- Tốn tài nguyên: Mỗi request cần 1 worker riêng biệt. Nếu lượng request tăng cao, số worker nhiều có thể tốn RAM và CPU rất lớn.

**Khi nào nên dùng PHP-FPM?**

- Dự án nhỏ hoặc trung bình, không cần tốc độ siêu cao.
- Hệ thống đã quen dùng Nginx/Apache, chưa muốn thay đổi hạ tầng lớn.
- Cần ổn định, dễ bảo trì, không quá quan tâm đến từng ms tốc độ.

**PHP-FPM và RoadRunner đều dùng worker pool, nhưng PHP-FPM tạo-hủy worker liên tục còn RoadRunner thì giữ worker sống lâu dài và không boot lại Laravel mỗi request.**

## Swoole và RoadRunner

- Gọi là application server
- Đều là công cụ mạnh mẽ giúp tăng tốc Laravel (hoặc các framework PHP khác) bằng cách loại bỏ PHP-FPM truyền thống và tối ưu xử lý request!
- Với Swoole — PHP như ngôn ngữ async hiện đại!

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

## ReactPHP

## Kết nối Database

3 cách kết nối:

* mysqli_connect(). chỉ mysql
* new mysqli. chỉ mysql
* new PDO(). Hỗ trợ nhiều csdl

**PDO (PHP Data Objects)** là một cách an toàn và linh hoạt để kết nối và làm việc với cơ sở dữ liệu trong PHP. Nó hỗ trợ nhiều hệ quản trị CSDL như MySQL, PostgreSQL, SQLite, Oracle,...

## I/O (Đọc/Ghi File)

**Tóm tắt nhanh**

| Hoạt động               | Hàm                                   |
| -------------------------- | -------------------------------------- |
| Mở file                   | `fopen()`                            |
| Đóng file                | `fclose()`                           |
| Ghi file                   | `fwrite()`, `file_put_contents()` |
| Đọc file                 | `fread()`, `file_get_contents()`  |
| Đọc từng dòng          | `fgets()`,`file()`                 |
| Xóa file                  | `unlink()`                           |
| Kiểm tra file tồn tại   | `file_exists()`                      |
| Đọc/Ghi CSV              | `fgetcsv()`,`fputcsv()`            |
| Quản lý thư mục        | `mkdir()`,`rmdir()`,`scandir()`  |
| Đổi tên/di chuyển file | `rename()`                           |

**Các chế độ mở file**

| Chế độ | Ý nghĩa                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------- |
| `r`     | Mở file để đọc. Con trỏ file bắt đầu từ đầu file.                                   |
| `r+`    | Mở file để đọc và ghi. Con trỏ file bắt đầu từ đầu file.                           |
| `w`     | Mở file để ghi. Xóa nội dung cũ nếu file tồn tại, tạo mới nếu không có.           |
| `w+`    | Mở file để đọc và ghi. Xóa nội dung cũ nếu file tồn tại, tạo mới nếu không có. |
| `a`     | Mở file để ghi, giữ nguyên nội dung cũ, con trỏ ở cuối file.                          |
| `a+`    | Mở file để đọc và ghi, giữ nguyên nội dung cũ, con trỏ ở cuối file.                |
| `x`     | Mở file để ghi, tạo file mới. Lỗi nếu file đã tồn tại.                               |
| `x+`    | Mở file để đọc và ghi, tạo file mới. Lỗi nếu file đã tồn tại.                     |

Kết Luận

* **`fopen()`** và **`fclose()`** để mở và đóng file.
* **`fwrite()`** và **`file_put_contents()`** để ghi dữ liệu vào file.
* **`fread()`** , **`fgets()`** và **`file_get_contents()`** để đọc dữ liệu từ file.
* **`file_exists()`** ,  **`unlink()`** , **`rename()`** để kiểm tra, xóa, di chuyển file.
* **`fputcsv()`** và **`fgetcsv()`** để làm việc với CSV.
* **`mkdir()`** ,  **`rmdir()`** , **`scandir()`** để thao tác với thư mục.

Ví dụ:

```php
$file = fopen("example.txt", "w"); // Mở file ở chế độ ghi
fwrite($file, "Xin chào!\n"); // Ghi dữ liệu vào file
fwrite($file, "Chào mừng đến với PHP File I/O.\n"); 
fclose($file); // Đóng file

// Ghi nhanh bằng file_put_contents()
file_put_contents("example.txt", "Hello, PHP File I/O!");
file_put_contents("example.txt", "Dòng mới thêm vào file.\n", FILE_APPEND); // Thêm vào cuối file
```

## Magic method

Magic method trong PHP là các phương thức đặc biệt của một class, được PHP tự động gọi khi một sự kiện cụ thể xảy ra. Chúng bắt đầu bằng hai dấu gạch dưới (`__`).l

| Magic Method                        | Mô tả                                                                   |
| ----------------------------------- | ------------------------------------------------------------------------- |
| `__construct()`                   | Khởi tạo đối tượng                                                  |
| `__destruct()`                    | Hủy đối tượng                                                        |
| `__get($name)`                    | Gọi thuộc tính không tồn tại                                        |
| `__set($name, $value)`            | Gán giá trị cho thuộc tính không tồn tại                          |
| `__isset($name)`                  | Kiểm tra `isset()`hoặc `empty()`trên thuộc tính không tồn tại |
| `__unset($name)`                  | Xóa thuộc tính không tồn tại                                        |
| `__call($name, $arguments)`       | Gọi phương thức không tồn tại từ đối tượng                    |
| `__callStatic($name, $arguments)` | Gọi phương thức không tồn tại từ class tĩnh                      |
| `__toString()`                    | Chuyển đổi đối tượng thành chuỗi                                 |
| `__invoke($arguments)`            | Gọi đối tượng như một hàm                                         |
| `__sleep()`                       | Xác định các thuộc tính cần serialize                              |
| `__wakeup()`                      | Khôi phục dữ liệu khi unserialize                                     |
| `__clone()`                       | Được gọi khi một đối tượng bị clone                             |
| `__debugInfo()`                   | Kiểm soát thông tin khi `var_dump()`được gọi                     |

## Thuật toán sắp xếp

* **Quicksort** (trước PHP 5.3)

  * Được sử dụng trong các hàm như `sort()`, `asort()`, `ksort()`, `usort()`, `uasort()`, `uksort()`, v.v.
  * Trung bình có độ phức tạp  **O(n log n)** .
* **Heapsort** (PHP 5.3 - PHP 7.0)

  * Được sử dụng để thay thế **Quicksort** nhằm cải thiện hiệu suất trong một số trường hợp.
  * Độ phức tạp  **O(n log n)** .
* **Timsort** (từ PHP 7.0 trở đi)

  * Đây là một thuật toán kết hợp giữa **Merge Sort** và  **Insertion Sort** .
  * Cải thiện hiệu suất cho dữ liệu gần như đã sắp xếp.
  * Được sử dụng trong hầu hết các hàm sắp xếp mặc định của PHP từ PHP 7.0.

### Các hàm sắp xếp phổ biến trong PHP:

| Hàm PHP     | Sắp xếp theo         | Dữ liệu giữ nguyên key | Thứ tự    |
| ------------ | ---------------------- | -------------------------- | ----------- |
| `sort()`   | Giá trị              | Không                     | Tăng dần  |
| `rsort()`  | Giá trị              | Không                     | Giảm dần  |
| `asort()`  | Giá trị              | Có                        | Tăng dần  |
| `arsort()` | Giá trị              | Có                        | Giảm dần  |
| `ksort()`  | Key                    | Có                        | Tăng dần  |
| `krsort()` | Key                    | Có                        | Giảm dần  |
| `usort()`  | Hàm callback          | Không                     | Tuỳ chỉnh |
| `uasort()` | Hàm callback          | Có                        | Tuỳ chỉnh |
| `uksort()` | Hàm callback theo key | Có                        | Tuỳ chỉnh |
