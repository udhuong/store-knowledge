# Life cycle

| Bước                   | Mô tả                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------- |
| Request tới index.php   | Web server chuyển request vào public/index.php.                                  |
| Khởi động framework   | Load autoload Composer và boot Laravel. Chuyển request vào Kernel để xử lý. |
| HTTP Kernel xử lý      | Load service providers. Chạy qua các middleware.                                 |
| Route điều hướng     | Tìm route phù hợp để gọi controller.                                         |
| Controller xử lý       | Logic chính, truy vấn model, validate, tính toán.                              |
| Model làm việc với DB | Truy vấn DB, lưu hoặc cập nhật dữ liệu nếu cần.                           |
| View (nếu có)          | Render giao diện với Blade hoặc trả JSON response.                             |
| Response trả về        | Gửi kết quả cho client và kết thúc request.                                  |
| Hoàn thành             | Laravel dừng xử lý và giải phóng tài nguyên.                               |

![lifecycle laravel](https://images.viblo.asia/b4bce647-722e-4064-ac19-b7e9e0d0573e.png)

# Các thành phần chính

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

# Service container, Service provider

**Định nghĩa:**

Service Container: Là hệ thống quản lý phụ thuộc (Dependency Injection), tự động tạo và tiêm các class vào nhau.
Service Provider: Là nơi đăng ký và cấu hình các service, giúp Laravel biết cách khởi tạo chúng.

**Cách đăng ký vào container:**

Cách 1: `app()->bind('App\Services\LoggerService', fn() -> new LoggerService());`

Cách 2: `app()->singleton('App\Services\LoggerService', fn() -> new LoggerService()); // Chỉ tạo 1 lần trong suốt request`

Lưu ý:

- bind tạo instance mới sau mỗi lần gọi LoggerService, singleton sẽ chỉ tạo 1 lần.
- cả 2 đều Lazy loading: chỉ khởi tạo khi cần
- bind: Khi cần instance độc lập (VD: kết nối API tạm thời)
- singleton: Khi cần 1 instance duy nhất (VD: logger, cache)

**Ví dụ:**

```php
  //bind:
  $logger1 = app('LoggerService'); 
  $logger2 = app('LoggerService');

  dd($logger1 === $logger2); // false

  //singleton:
  $logger1 = app('LoggerService'); 
  $logger2 = app('LoggerService');

  dd($logger1 === $logger2); // true
```

Sau khi đăng ký xong,có thể sử dụng LoggerService ở bất cứ đâu mà không cần khởi tạo thủ công!

# **Facade**

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

# Method register() và boot() trong ServiceProvider

|                    | register()                                                                                                         | boot()                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| Mục đích        | Đăng ký các service, binding class vào Service Container                                                      | Thực hiện các hành động cần thiết sau khi tất cả các service đã đăng ký xong |
| Thời điểm chạy | Trước khi tất cả các service provider khác được khởi động                                              | Sau khi tất cả provider đã chạy xong phương thức register                            |
| Lưu ý            | Không nên sử dụng bất kỳ service nào đã được đăng ký ở đây, vì chúng có thể chưa sẵn sàng | Có thể sử dụng các service đã đăng ký hoặc cấu hình route, event, policy, v.v.  |

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

# Eloquent và Builder

**Eloquent ORM:**

- Là 1 Active record
- Làm việc với dữ liệu theo kiểu hướng đối tượng (OOP), ánh xạ mỗi bảng thành một Model.
- Ưu điểm:
  - Giảm nguy cơ SQL Injection, Hỗ trợ Timestamps, Soft Delete, Mutators, Accessors.
  - sử dụng quan hệ dễ
- VD: `$users = User::where('status', 'active')->get();`

**Query Builder:**

- Viết truy vấn SQL bằng cách sử dụng các phương thức của Laravel mà không cần dùng Eloquent.
- Ưu điểm:
  - Linh hoạt hơn khi truy vấn dữ liệu phức tạp.
  - Nhanh hơn Eloquent vì không cần ánh xạ dữ liệu thành object.
  - Hỗ trợ thao tác trên nhiều bảng dễ dàng.
- VD: `$userCount = DB::table('users')->count();`

**Accessor và Mutator trong Eloquent:**

- Accessor: Thay đổi giá trị khi lấy ra từ DB.
- Mutator: Thay đổi giá trị trước khi lưu vào DB.

**Active Record vs Data Mapper**

Cả hai đều là mẫu thiết kế (design patterns) giúp kết nối mã nguồn với cơ sở dữ liệu

| **Tiêu chí**             | **Active Record**                                       | **Data Mapper**                                          |
| -------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------------- |
| **Kiểu thiết kế**       | Mỗi class đại diện cho 1 bảng và bản ghi.              | Model và DB tách biệt, có lớp Mapper trung gian.          |
| **CRUD**                   | Tích hợp sẵn trong model (`save()`, `delete()`).       | Xử lý bởi lớp Mapper riêng biệt, model không biết SQL. |
| **Dễ dàng sử dụng**    | Dễ học, dễ dùng, nhanh triển khai.                       | Phức tạp hơn, phải viết nhiều mã hơn.                  |
| **Tính linh hoạt**       | Kém linh hoạt khi logic phức tạp hoặc nhiều bảng.      | Rất linh hoạt, dễ mở rộng và tối ưu hệ thống lớn.   |
| **Hiệu suất**            | Nhanh với các thao tác đơn giản.                        | Tối ưu hơn khi xử lý truy vấn phức tạp.                |
| **Ứng dụng thực tế**   | **Eloquent (Laravel)**, **Active Record (Rails)** | **Doctrine (Symfony)**, **Hibernate (Java)**       |
| **Phù hợp với dự án** | Nhỏ đến trung bình, CRUD đơn giản.                     | Hệ thống lớn, nghiệp vụ phức tạp, cần tối ưu cao.    |

**Khi nào nên dùng cái nào?**

* **Dùng Active Record:**
  * Khi làm dự án nhỏ hoặc trung bình.
  * Khi cần tốc độ phát triển nhanh.
  * CRUD đơn giản, không nhiều logic phức tạp.
* **Dùng Data Mapper:**
  * Khi làm hệ thống lớn, nhiều bảng và mối quan hệ phức tạp.
  * Khi muốn tách biệt hoàn toàn logic nghiệp vụ và logic truy vấn.
  * Khi cần tối ưu hiệu suất cao với truy vấn phức tạp.

# Middleware

**Middleware** là các class trung gian xử lý request, response trước hoặc sau khi nó đến controller.

- Tách biết các logic kiểm tra request trước khi cho nó vào controller hoặc chỉnh sửa response trước khi trả lại cho người dùng.
- Trước khi request vào controller: có thể kiểm tra quyền truy cập, xác thực người dùng.
- Sau khi response trả về: có thể chỉnh sửa nội dung response hoặc thêm header.
- Có thể sử dụng middleware toàn cục hoặc đăng ký và gọi middleware trong route

# Event và **Listener**

**Định nghĩa**

* **Event** (Sự kiện) là một hành động hoặc sự việc diễn ra trong ứng dụng
* **Event** chỉ đơn giản là một **class** chứa thông tin về sự kiện đó. Dữ liệu này sẽ truyền cho các Listener sử dụng

Ví dụ:

* Người dùng đăng ký tài khoản.
* Đơn hàng được tạo thành công.
* Bài viết mới được xuất bản.

```php
namespace App\Events;

use App\Models\User;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class UserRegistered {
    use Dispatchable, SerializesModels;

    public $user;

    public function __construct(User $user) {
        $this->user = $user;
    }
}

```

**Listener**

**Listener** (Trình nghe) là **class** chịu trách nhiệm xử lý hành động cụ thể khi một sự kiện xảy ra. Có thể sử dụng **Listener** để gửi email, ghi log, cập nhật dữ liệu,...

```php
namespace App\Listeners;

use App\Events\UserRegistered;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Queue\InteractsWithQueue;
use App\Mail\WelcomeEmail;
use Mail;

class SendWelcomeEmail {
    public function __construct() {}

    public function handle(UserRegistered $event) {
        Mail::to($event->user->email)->send(new WelcomeEmail($event->user));
    }
}

```

**Đăng ký Event & Listener**

Laravel tự động quét event và listener nếu bạn dùng **Laravel 8+** trở lên. Tuy nhiên, \vẫn có thể đăng ký thủ công trong file `EventServiceProvider.php`:

```php
protected $listen = [
    UserRegistered::class => [
        SendWelcomeEmail::class,
    ],
];

// Kích hoạt event
event(new UserRegistered($user));
```

**Tại sao nên dùng event và listner?**

* **Tách biệt logic:** Giúp mã nguồn dễ bảo trì hơn.
* **Dễ dàng mở rộng:** Dễ thêm nhiều listener mà không ảnh hưởng đến code cũ.
* **Hỗ trợ Queue:** Giảm tải cho server bằng cách xử lý tác vụ nặng ở background.

**Lưu ý:**

- Muốn xử lý Listener bất đồng bộ, Listener implement ShouldQueue
- Event Subscriber để gom nhiều listener lại

# Job/Queue

**Định nghĩa**

**Job** là một lớp đại diện cho một **tác vụ cụ thể** mà bạn muốn thực hiện

Jobs giúp **đóng gói logic** và có thể chạy **ngay lập tức** hoặc đưa vào **queue** để xử lý **bất đồng bộ**

Queue giúp **xử lý các tác vụ tốn thời gian (ví dụ như gửi email, tạo báo cáo, resize ảnh)  **ngoài tiến trình chính** , giúp ứng dụng phản hồi nhanh hơn và tránh bị chậm.**

Queue là  **hàng đợi nhiệm vụ** , nơi các **job** (tác vụ) được xếp vào và xử lý theo nguyên tắc **FIFO** (First In, First Out).

**Ví dụ thực tế:** Khi người dùng đăng ký tài khoản:

1. **Tác vụ chính:** Lưu thông tin user vào database (chạy ngay).
2. **Tác vụ phụ:** Gửi email chào mừng (đẩy vào queue và xử lý sau).

Nhờ đó, người dùng nhận phản hồi nhanh, không phải đợi quá trình gửi email hoàn tất.

**Khi nào nên dùng queue?**

* **Gửi email** : Tránh làm người dùng đợi lâu.
* **Xử lý file nặng** : Resize ảnh, xuất file PDF lớn,...
* **Thông báo real-time** : Push thông báo qua WebSocket.
* **Đồng bộ dữ liệu** : Ví dụ: đẩy data sang hệ thống bên ngoài.

**Thông tin thêm:**

Chạy queue `php artisan queue:work` chạy hết queue là dừng nên cần chạy nền liên tục với Supervisor (php) hoặc PM2 (chủ yếu cho node.js, php dùng vẫn oki)

Có thể tạo nhiều worker queue để tối ưu tốc độ xử lý queue `php artisan queue:work --queue=orders --sleep=3 --tries=3` chỉ định tên queue cụ thể và tăng worker lên

# Event/Listener và Queue khác nhau thế nào?

| **Tiêu chí**             | **Event/Listener**                                                                                  | **Queue (Job)**                                                                                                      |
| -------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Chức năng chính**     | Kích hoạt và xử lý nhiều hành động khi một sự kiện xảy ra (như "Người dùng đăng ký"). | Đẩy tác vụ vào hàng đợi và xử lý sau để tránh làm chậm tiến trình chính (như gửi email, resize ảnh). |
| **Thời điểm thực thi** | **Thực thi ngay lập tức** khi event xảy ra (trừ khi kết hợp với queue).                     | **Thực thi bất đồng bộ**, có thể chạy sau hoặc delay theo thời gian đặt trước.                         |
| **Ví dụ**                | Sau khi tạo đơn hàng: gửi email, tạo log, cập nhật điểm thưởng cùng lúc.                    | Sau khi tạo đơn hàng: gửi email xác nhận qua queue để không làm người dùng đợi lâu.                       |
| **Đa nhiệm**             | Một event có thể kích hoạt nhiều listener cùng lúc.                                               | Một job thường xử lý một tác vụ cụ thể (nhưng có thể gọi nhiều job liên tiếp).                            |
| **Queue hỗ trợ**         | Có thể kết hợp listener với queue để xử lý bất đồng bộ.                                      | Queue bản chất là xử lý bất đồng bộ, mạnh mẽ khi cần tối ưu hiệu suất.                                     |

# Cache

**Laravel cung cấp hệ thống cache mạnh mẽ giúp tăng tốc độ và giảm tải cơ sở dữ liệu bằng cách lưu trữ dữ liệu tạm thời. Hệ thống này hỗ trợ nhiều driver khác nhau như  file, database, Redis, Memcached ...**

**Khi nào nên dùng Cache?**

* Dữ liệu không thay đổi thường xuyên (Danh mục sản phẩm, bài viết phổ biến).
* Giảm tải database, cải thiện tốc độ.
* Cần tối ưu hiệu suất API hoặc trang web.

**Không nên dùng Cache khi:**

* Dữ liệu thay đổi liên tục (Số dư tài khoản, trạng thái đơn hàng).
* Cần hiển thị thông tin **real-time** (chat, thông báo tức thì).

# **Command**

**Command** là các lệnh tùy chỉnh tạo ra để chạy các tác vụ phức tạp từ terminal thông qua **Artisan CLI**

**Một số use case thực tế:**

- Tạo command để Task Scheduling sử dụng
- Chủ động thực thi 1 logic nào đó đặc biệt, chạy 1 lần thôi hoặc nếu cần thì mới chạy
  - Ví dụ hệ thống đơn hàng có 1 số đơn bị lỗi, tạo command để truyền mã đơn vào và xóa khỏi bảng nào đó.
  - Cần convert dữ liệu cột A sang cột B trong 1 bảng hay bảng khác theo logic đặc biệt
  - Test dữ liệu chủ động, debug

Thường command sẽ sử dụng 1 số phương thức hỗ trợ show log để theo dõi và debug

Không nên để logic quá phức tạp ở command, logic phức tạp nên để ở class khác và command gọi vào class đó để thực thi

# **Task Scheduling**

**Định nghĩa:**

* **Task Scheduling** để tự động hóa các công việc định kỳ như  **gửi email**,  **làm sạch database**,  **backup dữ liệu**, mà không cần viết crontab phức tạp
* Cần thiết lập trong cronjob của OS, dòng này sẽ chạy lệnh **`schedule:run` mỗi phút** để Laravel kiểm tra và kích hoạt các task đã lên lịch  `cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1`

**Hướng dẫn sử dụng:**

* Bản chất là 1 tạo command sau đó thiết lập schedule cho phép chạy tự động command đó theo thiết lập
* Chỉnh sửa file `app/Console/Kernel.php`

```php
protected function schedule(Schedule $schedule)
{
    $schedule->command('emails:send-reminders')->dailyAt('08:00'); //Chạy command định kỳ
    $schedule->job(new ProcessReports)->daily(); //Chạy job
}

```

**Vấn đề khi chạy task đồng bộ**

Mặc định, lên lịch chạy task (ví dụ: gửi email hoặc tạo báo cáo lớn), Laravel sẽ **thực thi ngay lập tức** trong tiến trình chính. Điều này có thể gây ra:

* ⏳ **Chậm hệ thống:** Task nặng làm treo ứng dụng.
* 🔒 **Block request:** Người dùng phải chờ task chạy xong mới tiếp tục thao tác.
* 🚨 **Timeout:** Task quá lâu có thể bị server kill mất kết nối.

👉 Để giải quyết vấn đề này, cần kết hợp **Scheduler** với **Queue** để  **đẩy task sang hàng đợi** , xử lý nền mà không làm gián đoạn ứng dụng chính! 🎯

**Khi nào nên dùng Scheduler?**

✅ **Nên dùng:**

* Gửi email nhắc nhở hoặc thông báo.
* Xóa log cũ, làm sạch database.
* Tạo báo cáo hoặc backup dữ liệu định kỳ.
* Đồng bộ dữ liệu với hệ thống khác.

❌ **Không nên dùng:**

* Xử lý tác vụ lớn cần real-time (dùng Queue tốt hơn).
* Task cần phản hồi ngay lập tức cho người dùng.

# Laravel Octane

**Truyền thống:**

- Kết nối theo request (Stateless Connection). mô hình request-response truyền thống của PHP
  - Mỗi request HTTP là 1 process độc lập.
  - Laravel khởi động toàn bộ framework mỗi khi nhận request.
  - Kết nối database được tạo mới khi request bắt đầu và đóng lại khi request kết thúc.
- Quá trình chạy:
  1. **Client gửi request** → PHP-FPM nhận.
  2. **Laravel boot lên**, load tất cả service providers.
  3. **Tạo kết nối database:** Laravel dùng PDO để kết nối MySQL/PostgreSQL.
  4. **Xử lý query:** Thực hiện truy vấn, trả kết quả.
  5. **Đóng kết nối:** Khi request kết thúc, PHP sẽ **hủy kết nối** với DB.
- Nhược điểm:
  - **Tốn thời gian kết nối lại** mỗi request (~ vài ms).
  - **Không tái sử dụng connection**, làm tăng độ trễ.
  - **Tốn tài nguyên** khi phải liên tục mở/đóng kết nối.
- Database Connection Pool (chưa có sẵn)
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

# **Swoole là gì?**

- Swoole là một extension PHP, PHP >= 8.1
- Giúp tạo server bất đồng bộ, tương tự như Node.js nhưng mạnh mẽ hơn nhờ tích hợp sâu với PHP.
- Tính năng nổi bật của Swoole:

  - HTTP Server: Nhận và xử lý request mà không cần Apache/Nginx.
  - Coroutine: Xử lý nhiều tác vụ cùng lúc (IO, database) mà không chặn luồng chính.
  - Connection Pool: Giữ kết nối với database, Redis mà không đóng sau mỗi request.
  - WebSocket + TCP/UDP Server: Tạo real-time ứng dụng như chat, noti dễ dàng.
- Swoole biến PHP thành ngôn ngữ chạy server như Node.js, và Laravel Octane giúp tận dụng sức mạnh đó!
- Sự khác biệt khi dùng Octane + Swoole

| **Tính năng**                      | **Laravel truyền thống**                   | **Laravel + Octane + Swoole**                   |
| ------------------------------------------ | -------------------------------------------------- | ----------------------------------------------------- |
| **Boot framework**                   | Mỗi request                                       | Chỉ boot 1 lần duy nhất                            |
| **Kết nối database**               | Mỗi request tạo và đóng kết nối             | Giữ kết nối DB trong RAM (Connection Pool)         |
| **Xử lý tác vụ bất đồng bộ** | Không hỗ trợ                                    | Hỗ trợ coroutine, xử lý song song nhiều tác vụ |
| **WebSocket / Real-time**            | Cần cài thêm package (pusher, socket.io)        | Swoole tích hợp WebSocket sẵn                      |
| **Hiệu suất**                      | ~ 100-200 req/s                                    | **~ 1000-2000 req/s** hoặc cao hơn            |
| **Tài nguyên tiêu thụ**          | Cao (vì phải khởi động lại app mỗi request) | Thấp (do tái sử dụng tài nguyên)                |

**Khi nào nên dùng Laravel Octane + Swoole?**
🔹 Ứng dụng tốc độ cao: API, microservice, hệ thống cần phản hồi nhanh.
🔹 Real-time app: Chat, thông báo, game online (dùng WebSocket).
🔹 Hệ thống nhiều kết nối đồng thời: App nhiều request nhỏ lẻ (ví dụ like/comment).
🔹 Ứng dụng nền tảng lớn: Số lượng user cao, cần tối ưu tối đa tài nguyên.

**Trước và sau khi có Octane sử dụng coroutine**

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

| **Tính năng**                       | **Swoole**                                                                   | **RoadRunner**                                                                                                |
| ------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Ngôn ngữ**                        | **PHP Extension** (viết bằng C)                                            | **Standalone Binary** (viết bằng Golang)                                                                    |
| **Cách hoạt động**                | Laravel chạy trực tiếp trên**Swoole server** (không cần Nginx/Apache). | RoadRunner làm**Reverse Proxy** (thay thế Nginx), truyền request qua **gRPC** đến các worker PHP. |
| **Kiểu xử lý**                     | **Asynchronous + Coroutine** (đa nhiệm không chặn)                       | **Synchronous + Fiber (thread nhẹ)**                                                                         |
| **Hỗ trợ WebSocket**                | ✅ Tích hợp sẵn (WebSocket, TCP/UDP server, timer)                              | ❌ Không hỗ trợ WebSocket trực tiếp (phải dùng thư viện ngoài)                                            |
| **Quản lý worker**                  | Tích hợp sẵn trong**Swoole**                                              | RoadRunner quản lý worker cực tốt (restart worker khi lỗi, tự động scale)                                   |
| **Tương thích PHP**                | Cần cài thêm**PHP extension** (không dễ trên shared hosting)           | **Không cần cài PHP extension** (chạy độc lập với PHP)                                                |
| **Độ phức tạp cài đặt**        | Hơi phức tạp (vì phải biên dịch Swoole và cấu hình thêm)                | **Dễ dàng hơn** (chỉ cần 1 file `rr` binary và cấu hình YAML)                                       |
| **Hiệu suất**                       | Cực nhanh 🚀 (nhanh hơn PHP-FPM rất nhiều)                                     | Rất nhanh ⚡ (không nhanh bằng Swoole trong tác vụ bất đồng bộ, nhưng nhanh hơn PHP-FPM đáng kể)      |
| **Trường hợp sử dụng phù hợp** | Real-time app, chat, thông báo live, game online, API cần tốc độ cao         | API REST service, microservice, app truyền thống cần hiệu năng tốt nhưng không cần real-time phức tạp   |

# Gates và Policies

**Gates** và **Policies** là hai cơ chế quan trọng để quản lý quyền truy cập ( **Authorization** ). Chúng giúp kiểm soát xem người dùng có quyền thực hiện một hành động nào đó hay không.

## 1. **Gates (Cổng)**

* Gates là các hàm đơn giản để xác định quyền.
* Chúng được định nghĩa trong `App\Providers\AuthServiceProvider`.
* Gates phù hợp cho các trường hợp không liên quan đến một model cụ thể.
* Mặc định truyền dữ liệu vào là user đã đăng nhập
* Có thể sử dụng trong controller, middleware hoặc blade

## 2. **Policies (Chính sách)**

* Policies là các class chuyên biệt để kiểm soát quyền trên một model cụ thể.
* Chúng được sử dụng khi cần phân quyền dựa trên từng bản ghi của model.

## **Khi nào dùng Gates và Policies?**

| Tiêu chí        | Gates                                        | Policies                                |
| ----------------- | -------------------------------------------- | --------------------------------------- |
| Phù hợp với    | Quyền tổng quát (không liên quan model) | Quyền dựa trên model                 |
| Cách triển khai | Hàm closure trong `AuthServiceProvider`   | Class riêng biệt                      |
| Ví dụ           | Quyền truy cập trang admin                 | Chỉ tác giả có thể sửa bài viết |

# IoC (Inversion of Control) - Đảo ngược quyền điều khiển


# DI (Dependency Injection) - Tiêm phụ thuộc


# Laravel Passport hoặc Laravel Sanctum

### **1. Khi nào nên dùng Laravel Passport?**

Passport phù hợp khi bạn cần hệ thống xác thực mạnh mẽ với OAuth2, đặc biệt là trong các ứng dụng lớn, microservices hoặc có nhiều loại client khác nhau.

✅ Dùng Laravel Passport khi:

* Cần **OAuth2** (chuẩn công nghiệp cho xác thực API).
* Có ứng dụng **đa nền tảng** (SPA, mobile, web, microservices).
* Cần **mã thông báo truy cập lâu dài** (long-lived access tokens).
* Cần cấp **Client Credentials Tokens** hoặc  **Personal Access Tokens** .
* Cần **refresh token** để lấy access token mới mà không yêu cầu người dùng đăng nhập lại.

📌 **Ví dụ:** Ứng dụng mobile và web đều truy cập API chung, hoặc một hệ thống có nhiều service giao tiếp với nhau.

### **2. Khi nào nên dùng Laravel Sanctum?**

Sanctum nhẹ hơn, dễ triển khai hơn và phù hợp với các ứng dụng SPA hoặc mobile có backend Laravel.

✅ Dùng Laravel Sanctum khi:

* Xây dựng **Single Page Application (SPA)** hoặc **mobile app** cần xác thực API.
* Không cần OAuth2, chỉ cần **token đơn giản** hoặc  **cookie-based authentication** .
* Cần xác thực người dùng theo phiên (session-based authentication).
* Cần một hệ thống  **bảo mật API đơn giản** , dễ tích hợp với frontend.

📌 **Ví dụ:** Một ứng dụng React/VueJS dùng API Laravel để xác thực user mà không cần OAuth2.

### **Tóm lại:**

* Nếu bạn cần  **OAuth2** , dùng  **Passport** .
* Nếu chỉ cần xác thực API đơn giản, dùng  **Sanctum** .

# Câu hỏi thường gặp

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
