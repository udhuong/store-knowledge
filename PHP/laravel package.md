
## **Scout** - Tìm kiếm cho mô hình Eloquent

Tìm kiếm toàn văn bản (Full-Text Search) cho Eloquent

Chức năng:

* Cung cấp tìm kiếm toàn văn bản cho  **Eloquent models** .
* Hỗ trợ nhiều công cụ tìm kiếm: **Algolia, Meilisearch, TNTSearch, Elasticsearch, v.v.**
* Kết hợp với **queue** để xử lý chỉ mục nhanh hơn.

Khi nào dùng?

* Khi bạn cần **tìm kiếm nhanh** trên database nhưng SQL `LIKE` không đủ nhanh.
* Khi cần **tìm kiếm nâng cao** với khả năng đánh chỉ mục mạnh mẽ.

Cài đặt & sử dụng:

```php
composer require laravel/scout
php artisan vendor:publish --provider="Laravel\Scout\ScoutServiceProvider"

use Laravel\Scout\Searchable;

class Post extends Model {
    use Searchable;
}

Post::search('Laravel')->get();
```

## **Octane** - Máy chủ ứng dụng hiệu suất cao

Tăng tốc Laravel với Swoole/RoadRunner

Chức năng:

* Chạy Laravel với **Swoole hoặc RoadRunner** để loại bỏ tải lại framework trong mỗi request.
* **Giữ kết nối database, Redis, cache** trong bộ nhớ để tăng tốc đáng kể.
* Cực kỳ phù hợp cho ứng dụng  **real-time, API tốc độ cao** .

Khi nào dùng?

* Khi Laravel cần phục vụ  **hàng nghìn request/giây** .
* Khi API bị chậm do phải load lại framework mỗi request.
* Khi muốn **tận dụng bộ nhớ trong RAM** thay vì khởi tạo mọi thứ từ đầu.

Cài đặt & chạy:

```bash
composer require laravel/octane
php artisan octane:install
php artisan octane:start --server=swoole
```

## **Reverb** - WebSockets nhanh, có thể mở rộng

WebSockets tốc độ cao

Chức năng:

* Giúp bạn tạo WebSocket server  **không cần Pusher** .
* Tích hợp dễ dàng với **Laravel Broadcasting** và  **Laravel Echo** .

Khi nào dùng?

* Khi cần WebSocket **real-time** (chat, thông báo, dashboard thời gian thực).
* Khi không muốn sử dụng dịch vụ bên ngoài như  **Pusher** .

Cài đặt & chạy:

```php
composer require laravel/reverb
php artisan reverb:start

// Lắng nghe WebSocket sự kiện trong Laravel Echo:
Echo.channel('chat-room')
    .listen('MessageSent', (e) => {
        console.log(e.message);
    });

```

## **Echo** - Lắng nghe sự kiện WebSocket

Chức năng:

* Giúp frontend (Vue/React) **lắng nghe sự kiện real-time** từ Laravel.
* Kết hợp với  **Pusher, Reverb hoặc Laravel WebSockets** .

Khi nào dùng?

* Khi muốn hiển thị  **chat real-time, thông báo, cập nhật trực tiếp** .
* Khi đã có WebSocket server (Pusher/Reverb).

Cài đặt:

```php
npm install --save laravel-echo pusher-js

// Dùng với Pusher:
import Echo from 'laravel-echo';
window.Echo = new Echo({
    broadcaster: 'pusher',
    key: 'your-pusher-key',
    cluster: 'mt1',
    forceTLS: true
});
```

## **Pennant** - Quản lý feature flag

Chức năng:

* Bật/tắt **tính năng trong Laravel** mà không cần sửa code.
* Hữu ích cho  **A/B Testing, triển khai dần dần** .

Khi nào dùng?

* Khi muốn thử nghiệm tính năng trước khi mở cho toàn bộ người dùng.
* Khi cần **phân quyền động** dựa trên trạng thái feature.

Cài đặt & sử dụng:

```php
composer require laravel/pennant

// Thêm feature:
Feature::define('new-dashboard', function ($user) {
    return $user->is_admin;
});

// Kiểm tra trong Blade:
@if (Feature::active('new-dashboard'))
    <x-new-dashboard />
@else
    <x-old-dashboard />
@endif

```

## **Cashier** - Thanh toán và đăng ký

Xử lý thanh toán với Stripe/Paddle

Chức năng:

* Quản lý **subscription (đăng ký gói dịch vụ)** với  **Stripe hoặc Paddle** .
* Hỗ trợ  **hóa đơn, hủy đăng ký, dùng thử, nâng cấp gói** .

Khi nào dùng?

* Khi muốn tích hợp **thanh toán Stripe/Paddle** dễ dàng.

Cài đặt:

```php
composer require laravel/cashier

// Tạo subscription:
$user->newSubscription('default', 'price_123')->create($paymentMethodId);
```

## **Socialite** - Xác thực mạng xã hội

Xác thực OAuth (Google, Facebook, GitHub...)

Khi nào dùng?

* Khi muốn cho phép  **đăng nhập bằng mạng xã hội** .

Cài đặt:

```php
composer require laravel/socialite

// Đăng nhập Google:
return Socialite::driver('google')->redirect();

// Nhận thông tin user:
$user = Socialite::driver('google')->user();
```

## **Sanctum** - Xác thực API


Khi nào dùng?

* Khi muốn API authentication nhẹ, dễ dùng.

Cài đặt:

```
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"

// Tạo token:
$token = $user->createToken('API Token')->plainTextToken;

// Sử dụng token trong request:
Authorization: Bearer {TOKEN}
```

## **Sail** - Phát triển Docker cục bộ

**Dùng khi:** Muốn chạy Laravel trên Docker mà không cần tự cấu hình.

**Cài đặt & chạy:**

```bash
composer require laravel/sail
./vendor/bin/sail up
```

## **Pint** - Công cụ định dạng code

**Dùng khi:** Muốn tự động chuẩn hóa coding style.

**Cài đặt & chạy:**

```bash
composer require laravel/pint --dev
./vendor/bin/pint
```

## **Horizon** - Giám sát Redis queues

**Dùng khi:** Cần UI để quản lý job queue trong Redis.

**Cài đặt & chạy:**

```bash
composer require laravel/horizon
php artisan horizon
```

## **Dusk** - Kiểm thử trình duyệt tự động

**Dùng khi:** Muốn kiểm thử UI tự động.

**Cài đặt & chạy:**

```bash
composer require --dev laravel/dusk
php artisan dusk
```

## **Telescope** - Debugging và theo dõi cục bộ

**Dùng khi:** Cần theo dõi request, query, jobs.

**Cài đặt & chạy:**

```bash
composer require laravel/telescope
php artisan telescope:install
```

## **Pulse** - Phân tích hiệu suất

**Dùng khi:** Cần giám sát hiệu suất hệ thống.

**Cài đặt & chạy:**

```bash
composer require laravel/pulse
php artisan pulse:install
```
