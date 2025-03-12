## Life cycle:

**Request tới index.php:** Web server chuyển request vào public/index.php.
**Khởi động framework:** Load autoload Composer và boot Laravel. Chuyển request vào Kernel để xử lý.
**HTTP Kernel xử lý:**

- Load service providers.
- Chạy qua các middleware.

**Route điều hướng:** Tìm route phù hợp để gọi controller.
**Controller xử lý:** Logic chính, truy vấn model, validate, tính toán.
**Model làm việc với DB:** Truy vấn DB, lưu hoặc cập nhật dữ liệu nếu cần.
**View (nếu có):** Render giao diện với Blade hoặc trả JSON response.
**Response trả về:** Gửi kết quả cho client và kết thúc request.

- Laravel dừng xử lý và giải phóng tài nguyên.

![lifecycle laravel](https://images.viblo.asia/b4bce647-722e-4064-ac19-b7e9e0d0573e.png)

## Các thành phần chính

- Routing (Điều hướng)
- Middleware
- Service Container & Service Provider
- Controller
- Model & Eloquent ORM
- View & Blade Template
- Migration & Seeder
- Request & Response
- Artisan CLI
- Authentication & Authorization

## Service container (Kho chưa dịch vụ), Service provider

- **Service Container** là một công cụ mạnh mẽ, nơi đăng ký và khởi động các thành phần, service mà ứng dụng cần sử dụng.
- nó quản lý phụ thuộc (Dependency Injection). Nó giúp Laravel tự động lấy và tiêm các phụ thuộc vào class mà không cần khởi tạo thủ công.

**Cách đăng ký vào container**

- Cách 1: `app()->bind('App\Services\LoggerService', fn() -> new LoggerService());`
- Cách 2: `app()->singleton('App\Services\LoggerService', fn() -> new LoggerService()); // Chỉ tạo 1 lần suốt request`
- lưu ý:
  - bind tạo instance mới sau mỗi lần gọi LoggerService, singleton sẽ chỉ tạo 1 lần.
  - cả 2 đều Lazy loading: ý chỉ khởi tạo khi cần
  - bind: Khi cần instance độc lập (VD: kết nối API tạm thời)
  - singleton: Khi cần 1 instance duy nhất (VD: logger, cache)
- bind:

```php
  $logger1 = app('LoggerService'); 
  $logger2 = app('LoggerService');

  dd($logger1 === $logger2); // false
```

- singleton:

```php
  $logger1 = app('LoggerService'); 
  $logger2 = app('LoggerService');

  dd($logger1 === $logger2); // true
```

Sau khi đăng ký xong, bạn có thể sử dụng LoggerService ở bất cứ đâu mà không cần khởi tạo thủ công!

**Tóm lại**
Service Container: Là hệ thống quản lý phụ thuộc, tự động tạo và tiêm các class vào nhau.
Service Provider: Là nơi đăng ký và cấu hình các service, giúp Laravel biết cách khởi tạo chúng.

**Facade**

- Facade trong Laravel là class tĩnh cung cấp giao diện đơn giản để gọi hàm service nội bộ.
- Laravel tự động quản lý qua Service Container, ko cần khởi tạo kiểu DI hoặc new mỗi lần sử dụng.
- Các bước:
  - Tạo class sử dụng
  - Tạo class facade extend Facade
  - Đăng ký Service Provider
  - Sử dụng: ClassA::methodUse

```php
namespace App\Services;

class GreetingService
{
    public function sayHello($name)
    {
        return "Xin chào, " . $name . "!";
    }
}
//=============================//
namespace App\Facades;

use Illuminate\Support\Facades\Facade;

class Greeting extends Facade
{
    protected static function getFacadeAccessor()
    {
        return 'greeting';
    }
}
//=============================//
use App\Services\GreetingService;

public function register()
{
    $this->app->singleton('greeting', function () {
        return new GreetingService();
    });
}
//=============================//
use App\Facades\Greeting;

Route::get('/hello/{name}', function ($name) {
    return Greeting::sayHello($name);
});
```

## Method register() và boot() trong ServiceProvider

**register**

- Mục đích: Đăng ký các service, binding class vào Service Container.
- Thời điểm chạy: Trước khi tất cả các service provider khác được khởi động.
- Lưu ý: không nên sử dụng bất kỳ service nào đã được đăng ký ở đây, vì chúng có thể chưa sẵn sàng!

**boot**

- Mục đích: Thực hiện các hành động cần thiết sau khi tất cả các service đã đăng ký xong.
- Thời điểm chạy: Sau khi tất cả provider đã chạy xong phương thức register.
- Lưu ý: Tại đây có thể sử dụng các service đã đăng ký hoặc cấu hình route, event, policy, v.v.

```php
namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use App\Services\LoggerService;
use Illuminate\Support\Facades\Route;

class LoggerServiceProvider extends ServiceProvider
{
    /**
     * Đăng ký service vào container
     */
    public function register()
    {
        // Đăng ký LoggerService vào container
        $this->app->singleton(LoggerService::class, function ($app) {
            return new LoggerService();
        });
    }

    /**
     * Chạy sau khi tất cả provider đã khởi động
     */
    public function boot()
    {
        // Ghi log khi có request
        $logger = app(LoggerService::class);
    
        Route::matched(function ($event) use ($logger) {
            $route = $event->route->getName();
            $logger->logAccess("User accessed route: " . ($route ?? 'unknown'));
        });
    }
}

```

| **Phương thức** | **Chức năng chính**                         | **Ví dụ thực tế**                                                   | **Thời điểm chạy**                           |
| ------------------------ | ---------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------ |
| **`register`**   | Đăng ký service vào container                    | Đăng ký**LoggerService** bằng singleton                             | Trước khi tất cả service provider hoàn tất       |
| **`boot`**       | Thực thi logic sau khi mọi service đã đăng ký | Sử dụng**LoggerService** để ghi log mỗi lần có request truy cập | Sau khi tất cả service provider đã đăng ký xong |

## Eloquent và Builder

- Eloquent ORM:
  - Làm việc với dữ liệu theo kiểu hướng đối tượng (OOP), ánh xạ mỗi bảng thành một Model.
  - Ưu điểm:
    - Giảm nguy cơ SQL Injection, Hỗ trợ Timestamps, Soft Delete, Mutators, Accessors.
    - sử dụng quan hệ dễ
  - VD: `$users = User::where('status', 'active')->get();`
- Query Builder
  - Viết truy vấn SQL bằng cách sử dụng các phương thức của Laravel mà không cần dùng Eloquent.
  - Ưu điểm:
    - Linh hoạt hơn khi truy vấn dữ liệu phức tạp.
    - Nhanh hơn Eloquent vì không cần ánh xạ dữ liệu thành object.
    - Hỗ trợ thao tác trên nhiều bảng dễ dàng.
  - VD: `$userCount = DB::table('users')->count();`
- Accessor và Mutator trong Eloquent:
  - Accessor cho phép tùy chỉnh giá trị của một thuộc tính khi lấy ra từ cơ sở dữ liệu.
  - Mutator tự động thay đổi giá trị của thuộc tính trước khi lưu vào database.

## Middleware

**Middleware** là các class trung gian xử lý request, response trước hoặc sau khi nó đến controller.

- Tách biết các logic kiểm tra request trước khi cho nó vào controller hoặc chỉnh sửa response trước khi trả lại cho người dùng.
- Trước khi request vào controller: Bạn có thể kiểm tra quyền truy cập, xác thực người dùng.
- Sau khi response trả về: Bạn có thể chỉnh sửa nội dung response hoặc thêm header.
- Có thể sử dụng middleware toàn cục hoặc đăng ký và gọi middleware trong route

## Laravel Octane hoặc Swoole

**Truyền thống:**
- Kết nối theo request (Stateless Connection). mô hình request-response truyền thống của PHP
  - Mỗi request HTTP là 1 tiến trình độc lập.
  - Laravel khởi động toàn bộ framework mỗi khi nhận request.
  - Kết nối database được tạo mới khi request bắt đầu và đóng lại khi request kết thúc.
- Quá trình chạy:
  1. **Client gửi request** → PHP-FPM nhận.
  2. **Laravel boot lên** , load tất cả service providers.
  3. **Tạo kết nối database:** Laravel dùng PDO để kết nối MySQL/PostgreSQL.
  4. **Xử lý query:** Thực hiện truy vấn, trả kết quả.
  5. **Đóng kết nối:** Khi request kết thúc, PHP sẽ **hủy kết nối** với DB.
- Nhược điểm:
  - **Tốn thời gian kết nối lại** mỗi request (~ vài ms).
  - **Không tái sử dụng connection** , làm tăng độ trễ.
  - **Tốn tài nguyên** khi phải liên tục mở/đóng kết nối.

**Database Connection Pool (chưa có sẵn)**
  - Laravel bản gốc không có connection pool (pool kết nối) để tái sử dụng kết nối giữa các request. Mỗi request là 1 kết nối mới hoàn toàn.
  - Nếu muốn connection pool trước đây, phải dùng library ngoài như:
    - Laravel Connection Pool
    - PDO Connection Pool với Redis (hoặc Memcached) để lưu connection.

**Thay đổi:**
Laravel Octane là package chính thức của Laravel, giúp chạy ứng dụng PHP theo mô hình server lâu dài và giữ trạng thái giữa các request.
- Laravel chạy như một server liên tục.
- Boot framework 1 lần duy nhất, giữ sẵn kết nối và cấu hình trong RAM.
- Tái sử dụng tài nguyên (DB connection, singleton service) cho các request sau.  

Octane hỗ trợ 2 trình điều khiển (application server):  
- Swoole: Cực nhanh, hỗ trợ coroutine, WebSocket, timer.
- RoadRunner: Dùng Golang, dễ cài đặt và nhẹ.

**Swoole là gì?**
- Swoole là một extension PHP, PHP >= 8.1
- giúp tạo server bất đồng bộ, tương tự như Node.js nhưng mạnh mẽ hơn nhờ tích hợp sâu với PHP.
- Tính năng nổi bật của Swoole:
  - HTTP Server: Nhận và xử lý request mà không cần Apache/Nginx.
  - Coroutine: Xử lý nhiều tác vụ cùng lúc (IO, database) mà không chặn luồng chính.
  - Connection Pool: Giữ kết nối với database, Redis mà không đóng sau mỗi request.
  - WebSocket + TCP/UDP Server: Tạo real-time ứng dụng như chat, noti dễ dàng.
- Swoole biến PHP thành ngôn ngữ chạy server như Node.js, và Laravel Octane giúp bạn tận dụng sức mạnh đó!

- Sự khác biệt khi dùng Octane + Swoole

| **Tính năng**              | **Laravel truyền thống**                           | **Laravel + Octane + Swoole**                   |
|----------------------------|----------------------------------------------------|------------------------------------------------|
| **Boot framework**         | Mỗi request                                        | Chỉ boot 1 lần duy nhất                         |
| **Kết nối database**       | Mỗi request tạo và đóng kết nối                    | Giữ kết nối DB trong RAM (Connection Pool)      |
| **Xử lý tác vụ bất đồng bộ**| Không hỗ trợ                                      | Hỗ trợ coroutine, xử lý song song nhiều tác vụ  |
| **WebSocket / Real-time**  | Cần cài thêm package (pusher, socket.io)           | Swoole tích hợp WebSocket sẵn                   |
| **Hiệu suất**              | ~ 100-200 req/s                                    | **~ 1000-2000 req/s** hoặc cao hơn              |
| **Tài nguyên tiêu thụ**    | Cao (vì phải khởi động lại app mỗi request)         | Thấp (do tái sử dụng tài nguyên)                |

- Khi nào nên dùng Laravel Octane + Swoole?
🔹 Ứng dụng tốc độ cao: API, microservice, hệ thống cần phản hồi nhanh.
🔹 Real-time app: Chat, thông báo, game online (dùng WebSocket).
🔹 Hệ thống nhiều kết nối đồng thời: App nhiều request nhỏ lẻ (ví dụ like/comment).
🔹 Ứng dụng nền tảng lớn: Số lượng user cao, cần tối ưu tối đa tài nguyên.

- Trước và sau khi có Octane sử dụng coroutine
```php
Route::get('/sync', function () {
    $users = DB::table('users')->get();      // Query 1 300ms
    $posts = DB::table('posts')->get();      // Query 2 300ms
    $comments = DB::table('comments')->get(); // Query 3 300ms

    return compact('users', 'posts', 'comments'); // Tổng 900ms
});

//================//

use Illuminate\Support\Facades\DB;
use Laravel\Octane\Facades\Octane;

Route::get('/async', function () {
    [$users, $posts, $comments] = Octane::concurrently([
        fn () => DB::table('users')->get(),      // Query 1
        fn () => DB::table('posts')->get(),      // Query 2
        fn () => DB::table('comments')->get(),   // Query 3
    ]);

    return compact('users', 'posts', 'comments'); // Tổng 300ms
});

Route::get('/fetch-data', function () {
    [$users, $posts] = Octane::concurrently([
        fn () => Http::get('https://jsonplaceholder.typicode.com/users')->json(),
        fn () => Http::get('https://jsonplaceholder.typicode.com/posts')->json(),
    ]);

    return compact('users', 'posts'); // Tổng thời gian sẽ là hàm thực thi lâu nhất
});
```

**Giới thiệu nhanh về Swoole và RoadRunner**

| **Tính năng**                | **Swoole**                                                                                       | **RoadRunner**                                                                                           |
|------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **Ngôn ngữ**                  | **PHP Extension** (viết bằng C)                                                                  | **Standalone Binary** (viết bằng Golang)                                                                   |
| **Cách hoạt động**            | Laravel chạy trực tiếp trên **Swoole server** (không cần Nginx/Apache).                           | RoadRunner làm **Reverse Proxy** (thay thế Nginx), truyền request qua **gRPC** đến các worker PHP.          |
| **Kiểu xử lý**                | **Asynchronous + Coroutine** (đa nhiệm không chặn)                                               | **Synchronous + Fiber (thread nhẹ)**                                                                       |
| **Hỗ trợ WebSocket**          | ✅ Tích hợp sẵn (WebSocket, TCP/UDP server, timer)                                                 | ❌ Không hỗ trợ WebSocket trực tiếp (phải dùng thư viện ngoài)                                              |
| **Quản lý worker**            | Tích hợp sẵn trong **Swoole**                                                                     | RoadRunner quản lý worker cực tốt (restart worker khi lỗi, tự động scale)                                   |
| **Tương thích PHP**           | Cần cài thêm **PHP extension** (không dễ trên shared hosting)                                     | **Không cần cài PHP extension** (chạy độc lập với PHP)                                                      |
| **Độ phức tạp cài đặt**       | Hơi phức tạp (vì phải biên dịch Swoole và cấu hình thêm)                                          | **Dễ dàng hơn** (chỉ cần 1 file `rr` binary và cấu hình YAML)                                              |
| **Hiệu suất**                 | Cực nhanh 🚀 (nhanh hơn PHP-FPM rất nhiều)                                                        | Rất nhanh ⚡ (không nhanh bằng Swoole trong tác vụ bất đồng bộ, nhưng nhanh hơn PHP-FPM đáng kể)            |
| **Trường hợp sử dụng phù hợp** | Real-time app, chat, thông báo live, game online, API cần tốc độ cao                              | API service, microservice, app truyền thống cần hiệu năng tốt nhưng không cần real-time phức tạp           |

🔹 **Swoole:** Lý tưởng cho **ứng dụng real-time**, **WebSocket**, và **nhiệm vụ bất đồng bộ**.  
🔸 **RoadRunner:** Phù hợp với **API REST**, **microservice**, hoặc các **ứng dụng PHP truyền thống** cần tốc độ cao mà dễ triển khai.


## Câu hỏi thường gặp

**Phân biệt register và boot trong AppServiceProvider hay trong các class SerivceProvider?**

**Laravel hỗ trợ những phương thức authentication nào?**

- Authentication cơ bản (Session-based): Đây là cách xác thực truyền thống bằng email/mật khẩu.
  - Laravel sử dụng session để lưu trạng thái đăng nhập!
  - Dùng khi: Web app truyền thống, không cần API phức tạp.
  - Cách triển khai:
    - Laravel Breeze / Jetstream: Tạo sẵn đăng ký, đăng nhập, quên mật khẩu.
    - Middleware auth: Bảo vệ route yêu cầu đăng nhập.
    - Auth Facade: Để kiểm tra người dùng,
- API Authentication (Token-based): Laravel hỗ trợ API token để bảo vệ API endpoint mà không cần session!
  - Dùng khi: REST API, SPA hoặc mobile app.
  - Cách triển khai:
    - Laravel Sanctum: Bảo mật API nhỏ hoặc SPA.
    - Laravel Passport: Hỗ trợ OAuth2 cho API lớn.
- Social Authentication (OAuth, Socialite): Laravel hỗ trợ OAuth thông qua Laravel Socialite, giúp bạn dễ dàng tích hợp Facebook, Google, GitHub,...
  - Dùng khi: Web app, cho phép đăng nhập bằng mạng xã hội để tăng trải nghiệm người dùng!
  - Cách triển khai:
    - Cài Socialite
- Two-Factor Authentication (2FA)
- Email/Phone Verification
- HTTP Basic Authentication
- Role-based Authentication (ACL)

**Event và Listener trong Laravel hoạt động thế nào?**

- Event là một lớp đại diện cho một sự kiện nào đó trong hệ thống.
- Ví dụ: Người dùng đăng ký tài khoản, Đơn hàng được tạo, Email được gửi, v.v.
- Listener là lớp xử lý sự kiện — nó sẽ lắng nghe sự kiện và thực hiện hành động tương ứng.
- Ví dụ: Gửi email chào mừng sau khi người dùng đăng ký!
- Laravel dùng file EventServiceProvider để đăng ký sự kiện và listener
- Kích hoạt sự kiện: event(new UserRegistered($user));
- Muốn xử lý Listener bất đồng bộ, Listener implement ShouldQueue
- Event Subscriber để gom nhiều listener lại

**Broadcasting là gì? Khi nào nên sử dụng broadcasting?**

- Broadcasting trong Laravel là cách để gửi sự kiện từ server đến client theo thời gian thực, thường qua WebSocket.
- có thể tạo ra các tính năng như chat trực tuyến, thông báo tức thời, trạng thái hoạt động, và nhiều hơn nữa — mà không cần reload trang!
- Khi dispatch một event, nếu event đó implement interface ShouldBroadcast, Laravel sẽ gửi event ra một channel mà các client đang lắng nghe có thể nhận được ngay lập tức!
- Luồng hoạt động:
  - Server-side: Server phát ra event.
  - Broadcast driver: Laravel gửi event qua dịch vụ (Pusher, Ably, Redis...).
  - Client-side: Client lắng nghe channel và nhận dữ liệu real-time.
- Các loại channel trong broadcasting:
  - Public Channel: Ai cũng có thể lắng nghe.
  - Private Channel: Cần xác thực trước khi nghe.
  - Presence Channel: Giống private, nhưng biết ai đang tham gia.
- Tối ưu broadcasting:
  - implements ShouldBroadcast, ShouldQueue. Implement thêm ShouldQueue

**Cách tối ưu performance cho Laravel?**

- Server và Nginx/Apache:
  - PHP-FPM: Dùng PHP-FPM để xử lý request nhanh hơn.
  - OPcache: Kích hoạt OPcache để lưu bytecode PHP, giảm thời gian compile mã
  - HTTP/2: Dùng HTTP/2 tăng tốc độ truyền tải nhờ multiplexing.
  - gzip hoặc Brotli: Nén nội dung trả về để giảm băng thông
- Cache mọi thứ có thể:
  - php artisan config:cache
  - php artisan route:cache
  - php artisan view:cache
  - Response cache
- Queue và Job để giảm tải
  - Queue xử lý tác vụ nặng
  - Redis làm queue driver
- Database tối ưu hóa
  - index hợp lý
  - Eager Loading với with()
  - chunk nhỏ query
- Asset và frontend tối ưu.
  - Compile và minify css,js,image
  - dùng cdn
  - lazy load ảnh
- Tối ưu mã nguồn
  - sử dụng binding Singleton
  - xóa service provider ko dùng
- Giám sát và profiling
- Đưa Laravel lên production đúng cách:
  - Database connection pool: Dùng pool như Laravel Octane hoặc Swoole để giữ kết nối.
