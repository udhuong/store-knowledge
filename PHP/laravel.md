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
| **Phương thức** | **Chức năng chính**                           | **Ví dụ thực tế**                                                | **Thời điểm chạy**                              |
| --------------- | --------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------- |
| **`register`**  | Đăng ký service vào container                 | Đăng ký **LoggerService** bằng singleton                         | Trước khi tất cả service provider hoàn tất      |
| **`boot`**      | Thực thi logic sau khi mọi service đã đăng ký | Sử dụng **LoggerService** để ghi log mỗi lần có request truy cập | Sau khi tất cả service provider đã đăng ký xong |


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


## Middleware
**Middleware** là các class trung gian xử lý request, response trước hoặc sau khi nó đến controller. 
- Tách biết các logic kiểm tra request trước khi cho nó vào controller hoặc chỉnh sửa response trước khi trả lại cho người dùng.
- Trước khi request vào controller: Bạn có thể kiểm tra quyền truy cập, xác thực người dùng.
- Sau khi response trả về: Bạn có thể chỉnh sửa nội dung response hoặc thêm header.
- Có thể sử dụng middleware toàn cục hoặc đăng ký và gọi middleware trong route

## Câu hỏi thường gặp
Phân biệt register và boot trong AppServiceProvider hay trong các class SerivceProvider?
