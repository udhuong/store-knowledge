## Các loại Design Pattern

### Creational Patterns (Nhóm khởi tạo)

Giúp tạo đối tượng một cách linh hoạt và tối ưu.

#### **Singleton**

Đảm bảo rằng **chỉ có một thể hiện duy nhất** của một class trong suốt vòng đời của ứng dụng và cung cấp một điểm truy cập toàn cục đến nó.

**Khi nào sử dụng Singleton?**

* Khi bạn cần **đảm bảo rằng chỉ có một đối tượng duy nhất** trong toàn bộ ứng dụng (VD: kết nối database, logger).
* Khi bạn cần **một điểm truy cập toàn cục** đến đối tượng đó.
* Khi bạn muốn **quản lý trạng thái chung** trong ứng dụng mà không tạo nhiều phiên bản của class.


```php
class Database {
    private static ?Database $instance = null;
    private \PDO $connection;

    private function __construct() {
        $this->connection = new \PDO("mysql:host=localhost;dbname=test", "root", "");
    }

    public static function getInstance(): Database {
        if (self::$instance === null) {
            self::$instance = new Database();
        }
        return self::$instance;
    }

    public function getConnection(): \PDO {
        return $this->connection;
    }
}

// Sử dụng Singleton
$db = Database::getInstance()->getConnection();
```

#### **Factory Method**

**Mục đích:** Tạo đối tượng mà không cần chỉ định class cụ thể.

**Khi nào sử dụng Factory Method?**

* Khi bạn cần  **tạo đối tượng mà không biết chính xác lớp nào sẽ được sử dụng** .
* Khi cần **mở rộng mà không sửa đổi mã nguồn gốc** (Open/Closed Principle - OCP).
* Khi một lớp có nhiều phiên bản khác nhau nhưng  **cùng tuân theo một interface chung** .

**Ứng dụng thực tế:**

* Tạo đối tượng kết nối API
* Factory trong Laravel (`app()->make()`)

```php
interface Notification {
    public function send(string $message): void;
}

class EmailNotification implements Notification {
    public function send(string $message): void {
        echo "Gửi Email: " . $message;
    }
}

class SmsNotification implements Notification {
    public function send(string $message): void {
        echo "Gửi SMS: " . $message;
    }
}

class NotificationFactory {
    public static function create(string $type): Notification {
        return match ($type) {
            'email' => new EmailNotification(),
            'sms' => new SmsNotification(),
            default => throw new Exception("Loại không hợp lệ"),
        };
    }
}

// Sử dụng
$notification = NotificationFactory::create('email');
$notification->send("Chào bạn!");
```

#### **Abstract Factory**

Cho phép tạo ra **nhóm các đối tượng liên quan** mà không cần chỉ định trực tiếp class cụ thể.

```php
// Giao diện cho các thành phần UI
interface Button {
    public function render();
}

interface Checkbox {
    public function render();
}

// Implement Light Theme
class LightButton implements Button {
    public function render() {
        return "Render Light Button";
    }
}

class LightCheckbox implements Checkbox {
    public function render() {
        return "Render Light Checkbox";
    }
}

// Implement Dark Theme
class DarkButton implements Button {
    public function render() {
        return "Render Dark Button";
    }
}

class DarkCheckbox implements Checkbox {
    public function render() {
        return "Render Dark Checkbox";
    }
}

// Abstract Factory Interface
interface UIFactory {
    public function createButton(): Button;
    public function createCheckbox(): Checkbox;
}

// Light Theme Factory
class LightThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new LightButton();
    }

    public function createCheckbox(): Checkbox {
        return new LightCheckbox();
    }
}

// Dark Theme Factory
class DarkThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new DarkButton();
    }

    public function createCheckbox(): Checkbox {
        return new DarkCheckbox();
    }
}

// Hàm sử dụng Abstract Factory
function renderUI(UIFactory $factory) {
    $button = $factory->createButton();
    $checkbox = $factory->createCheckbox();

    echo $button->render() . "\n";
    echo $checkbox->render() . "\n";
}

// Chọn theme Light
$lightTheme = new LightThemeFactory();
renderUI($lightTheme);

// Chọn theme Dark
$darkTheme = new DarkThemeFactory();
renderUI($darkTheme);
```

#### **Builder**

Dùng để tạo object phức tạp với nhiều bước.

**Ứng dụng thực tế:**

* Laravel Query Builder

```php
class User {
    public function __construct(
        public string $name,
        public string $email,
        public ?string $address = null
    ) {}
}

class UserBuilder {
    private string $name;
    private string $email;
    private ?string $address = null;

    public function setName(string $name): self {
        $this->name = $name;
        return $this;
    }

    public function setEmail(string $email): self {
        $this->email = $email;
        return $this;
    }

    public function setAddress(string $address): self {
        $this->address = $address;
        return $this;
    }

    public function build(): User {
        return new User($this->name, $this->email, $this->address);
    }
}

// Sử dụng Builder
$user = (new UserBuilder())
    ->setName("John")
    ->setEmail("john@example.com")
    ->setAddress("New York")
    ->build();

```

#### **Prototype**

Sao chép một object hiện có thay vì tạo mới từ đầu.

```php
// Giao diện Prototype
interface Prototype {
    public function clone(): Prototype;
}

// Lớp gốc cần sao chép
class User implements Prototype {
    public $name;
    public $email;

    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }

    public function clone(): Prototype {
        return new User($this->name, $this->email);
    }

    public function display() {
        echo "User: {$this->name}, Email: {$this->email}\n";
    }
}

// Sử dụng Prototype
$originalUser = new User("John Doe", "john@example.com");
$clonedUser = $originalUser->clone();
$clonedUser->name = "Jane Doe"; // Thay đổi thông tin của bản sao

$originalUser->display(); // Output: User: John Doe, Email: john@example.com
$clonedUser->display();   // Output: User: Jane Doe, Email: john@example.com
```

### Structural Patterns (Nhóm cấu trúc)

Giúp tổ chức các class và object để tạo thành hệ thống linh hoạt và dễ bảo trì.

#### **Adapter**

Chuyển đổi interface của một class thành interface mà client mong đợi.

Khi nào sử dụng Adapter?

* Khi bạn muốn tích hợp một hệ thống cũ vào một hệ thống mới mà không thể thay đổi code của hệ thống cũ.
* Khi hai thành phần trong hệ thống có giao diện không tương thích nhưng vẫn cần làm việc với nhau.

* Khi bạn cần sử dụng lại một thư viện bên thứ ba nhưng giao diện của nó không phù hợp với hệ thống hiện tại.

```php
// Lớp cũ cần tích hợp (không thể thay đổi code của nó)
class OldSystem {
    public function getOldData() {
        return "Data from Old System";
    }
}

// Giao diện mới mà hệ thống mong đợi
interface NewSystem {
    public function getData();
}

// Adapter để chuyển đổi từ OldSystem sang NewSystem
class Adapter implements NewSystem {
    private $oldSystem;

    public function __construct(OldSystem $oldSystem) {
        $this->oldSystem = $oldSystem;
    }

    public function getData() {
        return $this->oldSystem->getOldData(); // Chuyển đổi sang giao diện mới
    }
}

// Sử dụng Adapter
$oldSystem = new OldSystem();
$adapter = new Adapter($oldSystem);
echo $adapter->getData(); // Output: Data from Old System
```

#### **Bridge**

Tách abstraction (giao diện) khỏi implementation (triển khai) để dễ mở rộng.

**Khi nào sử dụng Bridge?**

* Khi bạn muốn **tách biệt phần logic** của một hệ thống thành hai phần  **độc lập** .
* Khi có **nhiều biến thể** của một class và không muốn bị ràng buộc bởi kế thừa cứng nhắc.
* Khi cần **hỗ trợ nhiều nền tảng** mà không làm phức tạp code.

```php
// Giao diện cho phần Implementation (thiết bị thực tế)
interface Device {
    public function turnOn();
    public function turnOff();
}

// Các thiết bị cụ thể
class TV implements Device {
    public function turnOn() {
        return "TV is turned ON";
    }

    public function turnOff() {
        return "TV is turned OFF";
    }
}

class Radio implements Device {
    public function turnOn() {
        return "Radio is turned ON";
    }

    public function turnOff() {
        return "Radio is turned OFF";
    }
}

// Abstraction (Giao diện điều khiển)
class RemoteControl {
    protected $device;

    public function __construct(Device $device) {
        $this->device = $device;
    }

    public function turnOn() {
        return $this->device->turnOn();
    }

    public function turnOff() {
        return $this->device->turnOff();
    }
}

// Mở rộng Abstraction (thêm chức năng nâng cao)
class AdvancedRemoteControl extends RemoteControl {
    public function mute() {
        return "Device is muted";
    }
}

// Sử dụng Bridge Pattern
$tvRemote = new RemoteControl(new TV());
echo $tvRemote->turnOn() . "\n";  // Output: TV is turned ON
echo $tvRemote->turnOff() . "\n"; // Output: TV is turned OFF

$radioRemote = new AdvancedRemoteControl(new Radio());
echo $radioRemote->turnOn() . "\n";  // Output: Radio is turned ON
echo $radioRemote->mute() . "\n";    // Output: Device is muted
echo $radioRemote->turnOff() . "\n"; // Output: Radio is turned OFF
```

#### **Composite**

Cho phép xử lý nhóm object như thể chúng là một object đơn. Giúp xử lý  **các đối tượng theo cấu trúc cây** , trong đó **cả đối tượng đơn lẻ và nhóm đối tượng** có thể được xử lý một cách thống nhất.

**Khi nào sử dụng Composite?**

* Khi bạn cần **biểu diễn cấu trúc phân cấp** (VD: cây thư mục, menu, tổ chức công ty).
* Khi bạn muốn  **xử lý cả đối tượng đơn lẻ và nhóm đối tượng theo cùng một cách** .
* Khi bạn cần  **thêm, xóa hoặc duyệt qua các đối tượng con một cách linh hoạt** .

```php
// Giao diện Component chung cho cả đối tượng đơn và nhóm đối tượng
interface FileSystem {
    public function show();
}

// Đối tượng Leaf (Tập tin đơn lẻ)
class File implements FileSystem {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function show() {
        echo "File: {$this->name}\n";
    }
}

// Đối tượng Composite (Thư mục có thể chứa tập tin hoặc thư mục con)
class Folder implements FileSystem {
    private $name;
    private $children = [];

    public function __construct($name) {
        $this->name = $name;
    }

    public function add(FileSystem $component) {
        $this->children[] = $component;
    }

    public function show() {
        echo "Folder: {$this->name}\n";
        foreach ($this->children as $child) {
            echo "  - ";
            $child->show();
        }
    }
}

// Sử dụng Composite Pattern
$file1 = new File("Document.txt");
$file2 = new File("Image.jpg");

$folder1 = new Folder("My Documents");
$folder1->add($file1);
$folder1->add($file2);

$folder2 = new Folder("Projects");
$file3 = new File("Project1.docx");
$folder2->add($file3);

$rootFolder = new Folder("Root");
$rootFolder->add($folder1);
$rootFolder->add($folder2);

$rootFolder->show();
```

#### **Decorator**

Thêm chức năng cho object mà không cần thay đổi code gốc.

**Mục đích:** Thêm chức năng cho object mà không cần thay đổi code gốc.

**Ứng dụng thực tế:**

* Middleware trong Laravel
* Thêm tính năng cho UI Component

```php
interface Coffee {
    public function cost(): int;
}

class BasicCoffee implements Coffee {
    public function cost(): int {
        return 5;
    }
}

class MilkDecorator implements Coffee {
    public function __construct(private Coffee $coffee) {}

    public function cost(): int {
        return $this->coffee->cost() + 2;
    }
}

class SugarDecorator implements Coffee {
    public function __construct(private Coffee $coffee) {}

    public function cost(): int {
        return $this->coffee->cost() + 1;
    }
}

// Sử dụng Decorator
$coffee = new BasicCoffee();
$coffee = new MilkDecorator($coffee);
$coffee = new SugarDecorator($coffee);

echo "Giá cà phê: " . $coffee->cost() . "$";  // Kết quả: 8$

```

#### **Facade**

Cung cấp một giao diện đơn giản để ẩn đi hệ thống phức tạp bên dưới.

**Khi nào sử dụng Facade?**

* Khi hệ thống **quá phức tạp** với nhiều lớp hoặc thư viện và bạn muốn cung cấp **một giao diện đơn giản** để sử dụng.
* Khi bạn muốn **giảm sự phụ thuộc** của client vào các thành phần bên trong hệ thống.
* Khi cần **tổ chức lại code** để dễ quản lý và bảo trì hơn.

```php
// Các lớp con phức tạp
class AudioSystem {
    public function start() {
        return "Audio system started\n";
    }
}

class VideoSystem {
    public function start() {
        return "Video system started\n";
    }
}

class StreamingService {
    public function connect() {
        return "Connected to streaming service\n";
    }
}

// Lớp Facade đơn giản hóa việc sử dụng các hệ thống con
class HomeTheaterFacade {
    private $audio;
    private $video;
    private $streaming;

    public function __construct() {
        $this->audio = new AudioSystem();
        $this->video = new VideoSystem();
        $this->streaming = new StreamingService();
    }

    public function startMovie() {
        echo $this->audio->start();
        echo $this->video->start();
        echo $this->streaming->connect();
        echo "Movie started!\n";
    }
}

// Sử dụng Facade để khởi động hệ thống giải trí
$homeTheater = new HomeTheaterFacade();
$homeTheater->startMovie();
```

#### **Flyweight**

Dùng chung object để tiết kiệm bộ nhớ khi có nhiều instance giống nhau.

**Khi nào sử dụng Flyweight?**

* Khi ứng dụng có **rất nhiều đối tượng giống nhau** gây lãng phí bộ nhớ.
* Khi cần **tối ưu hiệu suất** bằng cách  **chia sẻ đối tượng thay vì tạo mới** .
* Khi có  **nhiều đối tượng có dữ liệu chung** , chỉ lưu trữ dữ liệu riêng biệt cần thiết.

```php
// Flyweight (Đối tượng dùng chung)
class TreeType {
    private $name;
    private $color;
    private $texture;

    public function __construct($name, $color, $texture) {
        $this->name = $name;
        $this->color = $color;
        $this->texture = $texture;
    }

    public function getInfo() {
        return "TreeType: {$this->name}, Color: {$this->color}, Texture: {$this->texture}\n";
    }
}

// Flyweight Factory (Quản lý và chia sẻ đối tượng)
class TreeFactory {
    private static $treeTypes = [];

    public static function getTreeType($name, $color, $texture) {
        $key = md5($name . $color . $texture);
        if (!isset(self::$treeTypes[$key])) {
            self::$treeTypes[$key] = new TreeType($name, $color, $texture);
        }
        return self::$treeTypes[$key];
    }
}

// Context (Lưu trữ trạng thái riêng)
class Tree {
    private $x;
    private $y;
    private $treeType;

    public function __construct($x, $y, TreeType $treeType) {
        $this->x = $x;
        $this->y = $y;
        $this->treeType = $treeType;
    }

    public function draw() {
        echo "Tree at ({$this->x}, {$this->y}) -> " . $this->treeType->getInfo();
    }
}

// Sử dụng Flyweight Pattern
$tree1 = new Tree(10, 20, TreeFactory::getTreeType("Oak", "Green", "Rough"));
$tree2 = new Tree(15, 30, TreeFactory::getTreeType("Oak", "Green", "Rough"));
$tree3 = new Tree(40, 50, TreeFactory::getTreeType("Pine", "Dark Green", "Smooth"));

$tree1->draw();
$tree2->draw();
$tree3->draw();
```

#### **Proxy**

Tạo một lớp thay thế để kiểm soát truy cập vào object khác.

**Khi nào sử dụng Proxy?**

* Khi bạn muốn **kiểm soát quyền truy cập** vào một đối tượng (VD: xác thực user trước khi truy cập dữ liệu).
* Khi đối tượng thực  **tốn tài nguyên để khởi tạo** , Proxy giúp **tạo nó khi cần thiết** (Lazy Initialization).
* Khi cần **ghi log, caching, hoặc thực hiện các thao tác trung gian** trước khi gọi đối tượng thực.

```php
// Giao diện chung cho cả Proxy và Real Subject
interface Image {
    public function display();
}

// Đối tượng thực (Real Subject)
class RealImage implements Image {
    private $filename;

    public function __construct($filename) {
        $this->filename = $filename;
        $this->loadFromDisk();
    }

    private function loadFromDisk() {
        echo "Loading image: {$this->filename}\n";
    }

    public function display() {
        echo "Displaying image: {$this->filename}\n";
    }
}

// Proxy kiểm soát việc tải ảnh (Lazy Initialization)
class ProxyImage implements Image {
    private $realImage;
    private $filename;

    public function __construct($filename) {
        $this->filename = $filename;
    }

    public function display() {
        if ($this->realImage === null) {
            $this->realImage = new RealImage($this->filename);
        }
        $this->realImage->display();
    }
}

// Sử dụng Proxy Pattern
$image1 = new ProxyImage("photo1.jpg");
$image2 = new ProxyImage("photo2.jpg");

$image1->display(); // Ảnh chỉ được tải từ disk khi gọi display()
$image1->display(); // Lần sau ảnh không cần tải lại
$image2->display();
```

### Behavioral Patterns (Nhóm hành vi)

Xác định cách các object giao tiếp với nhau.

#### **Chain of Responsibility**

Chuyển request qua một chuỗi xử lý cho đến khi có object xử lý nó.

**Mục đích** : Cho phép gửi yêu cầu qua một chuỗi các handler cho đến khi có một handler xử lý được nó.

**Ứng dụng thực tế** : Middleware trong Laravel, bộ lọc yêu cầu HTTP, hệ thống xử lý lỗi.

```php
interface Handler {
    public function setNext(Handler $handler): Handler;
    public function handle(string $request);
}

class ConcreteHandlerA implements Handler {
    private $nextHandler;

    public function setNext(Handler $handler): Handler {
        $this->nextHandler = $handler;
        return $handler;
    }

    public function handle(string $request) {
        if ($request == "A") {
            echo "Handled by ConcreteHandlerA\n";
        } elseif ($this->nextHandler) {
            $this->nextHandler->handle($request);
        }
    }
}

class ConcreteHandlerB implements Handler {
    private $nextHandler;

    public function setNext(Handler $handler): Handler {
        $this->nextHandler = $handler;
        return $handler;
    }

    public function handle(string $request) {
        if ($request == "B") {
            echo "Handled by ConcreteHandlerB\n";
        } elseif ($this->nextHandler) {
            $this->nextHandler->handle($request);
        }
    }
}

// Sử dụng Chain of Responsibility
$handlerA = new ConcreteHandlerA();
$handlerB = new ConcreteHandlerB();
$handlerA->setNext($handlerB);

$handlerA->handle("A"); // Output: Handled by ConcreteHandlerA
$handlerA->handle("B"); // Output: Handled by ConcreteHandlerB
```

#### **Command**

Đóng gói một hành động thành object để dễ dàng undo/redo.

```php
interface Command {
    public function execute();
}

class LightOnCommand implements Command {
    private $light;

    public function __construct($light) {
        $this->light = $light;
    }

    public function execute() {
        $this->light->turnOn();
    }
}

class Light {
    public function turnOn() {
        echo "The light is on\n";
    }
}

// Sử dụng Command
$light = new Light();
$lightOn = new LightOnCommand($light);
$lightOn->execute(); // Output: The light is on
```

#### **Interpreter**

Xử lý ngôn ngữ riêng bằng cách diễn giải các câu lệnh.

```php
// Interface chung cho tất cả các biểu thức
interface Expression {
    public function interpret(array $context);
}

// Biểu thức cộng
class AddExpression implements Expression {
    private $left;
    private $right;

    public function __construct(Expression $left, Expression $right) {
        $this->left = $left;
        $this->right = $right;
    }

    public function interpret(array $context) {
        return $this->left->interpret($context) + $this->right->interpret($context);
    }
}

// Biểu thức trừ
class SubtractExpression implements Expression {
    private $left;
    private $right;

    public function __construct(Expression $left, Expression $right) {
        $this->left = $left;
        $this->right = $right;
    }

    public function interpret(array $context) {
        return $this->left->interpret($context) - $this->right->interpret($context);
    }
}

// Biểu thức số nguyên (terminal expression)
class NumberExpression implements Expression {
    private $number;

    public function __construct($number) {
        $this->number = $number;
    }

    public function interpret(array $context) {
        return $this->number;
    }
}

// Xây dựng biểu thức: (5 + 10) - 2
$expression = new SubtractExpression(
    new AddExpression(new NumberExpression(5), new NumberExpression(10)),
    new NumberExpression(2)
);

// Thực thi biểu thức
$result = $expression->interpret([]);
echo "Result: " . $result . "\n"; // Output: Result: 13
```

#### **Iterator**

Cung cấp cách duyệt qua các phần tử trong collection mà không lộ ra cấu trúc bên trong.

```php
// Interface chung cho tất cả các biểu thức
interface Expression {
    public function interpret(array $context);
}

// Biểu thức cộng
class AddExpression implements Expression {
    private $left;
    private $right;

    public function __construct(Expression $left, Expression $right) {
        $this->left = $left;
        $this->right = $right;
    }

    public function interpret(array $context) {
        return $this->left->interpret($context) + $this->right->interpret($context);
    }
}

// Biểu thức trừ
class SubtractExpression implements Expression {
    private $left;
    private $right;

    public function __construct(Expression $left, Expression $right) {
        $this->left = $left;
        $this->right = $right;
    }

    public function interpret(array $context) {
        return $this->left->interpret($context) - $this->right->interpret($context);
    }
}

// Biểu thức số nguyên (terminal expression)
class NumberExpression implements Expression {
    private $number;

    public function __construct($number) {
        $this->number = $number;
    }

    public function interpret(array $context) {
        return $this->number;
    }
}

// Xây dựng biểu thức: (5 + 10) - 2
$expression = new SubtractExpression(
    new AddExpression(new NumberExpression(5), new NumberExpression(10)),
    new NumberExpression(2)
);

// Thực thi biểu thức
$result = $expression->interpret([]);
echo "Result: " . $result . "\n"; // Output: Result: 13
```

#### **Mediator**

Giảm sự phụ thuộc trực tiếp giữa các object bằng cách sử dụng một trung gian.

**Khi nào sử dụng Mediator?**

* Khi có **nhiều đối tượng cần giao tiếp với nhau** nhưng không muốn chúng phụ thuộc trực tiếp vào nhau.
* Khi cần  **giảm sự ràng buộc giữa các lớp** , giúp dễ bảo trì và mở rộng.
* Khi muốn **tổ chức các sự kiện hoặc thông báo** giữa các thành phần trong hệ thống.

```php
// Interface Mediator chung
interface Mediator {
    public function sendMessage($message, Colleague $colleague);
}

// Lớp Colleague (Các thành phần giao tiếp thông qua Mediator)
abstract class Colleague {
    protected $mediator;

    public function __construct(Mediator $mediator) {
        $this->mediator = $mediator;
    }
}

// Lớp UserA (Colleague cụ thể)
class UserA extends Colleague {
    public function send($message) {
        echo "UserA gửi tin nhắn: $message\n";
        $this->mediator->sendMessage($message, $this);
    }

    public function receive($message) {
        echo "UserA nhận tin nhắn: $message\n";
    }
}

// Lớp UserB (Colleague cụ thể)
class UserB extends Colleague {
    public function send($message) {
        echo "UserB gửi tin nhắn: $message\n";
        $this->mediator->sendMessage($message, $this);
    }

    public function receive($message) {
        echo "UserB nhận tin nhắn: $message\n";
    }
}

// Lớp ChatMediator (Mediator cụ thể)
class ChatMediator implements Mediator {
    private $users = [];

    public function addUser(Colleague $user) {
        $this->users[] = $user;
    }

    public function sendMessage($message, Colleague $colleague) {
        foreach ($this->users as $user) {
            if ($user !== $colleague) {
                $user->receive($message);
            }
        }
    }
}

// Sử dụng Mediator Pattern
$mediator = new ChatMediator();

$userA = new UserA($mediator);
$userB = new UserB($mediator);

$mediator->addUser($userA);
$mediator->addUser($userB);

$userA->send("Chào UserB!");
$userB->send("Chào UserA! Tôi đã nhận được tin nhắn.");
```

#### **Memento**

Lưu trạng thái object để có thể khôi phục sau này.

```php
// Memento: Lưu trữ trạng thái của đối tượng
class Memento {
    private $state;

    public function __construct($state) {
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }
}

// Originator: Lớp mà trạng thái của nó sẽ được lưu trữ
class Originator {
    private $state;

    public function setState($state) {
        echo "State set to: $state\n";
        $this->state = $state;
    }

    public function getState() {
        return $this->state;
    }

    // Tạo memento lưu trạng thái
    public function saveStateToMemento() {
        return new Memento($this->state);
    }

    // Khôi phục trạng thái từ memento
    public function restoreStateFromMemento(Memento $memento) {
        $this->state = $memento->getState();
        echo "State restored to: $this->state\n";
    }
}

// Caretaker: Lưu trữ memento và không làm thay đổi nội dung của nó
class Caretaker {
    private $memento;

    public function saveMemento(Memento $memento) {
        $this->memento = $memento;
    }

    public function getMemento() {
        return $this->memento;
    }
}

// Sử dụng Memento Pattern
$originator = new Originator();
$caretaker = new Caretaker();

// Thay đổi và lưu trạng thái
$originator->setState("State #1");
$caretaker->saveMemento($originator->saveStateToMemento());

$originator->setState("State #2");
$caretaker->saveMemento($originator->saveStateToMemento());

$originator->setState("State #3");

// Khôi phục lại trạng thái trước đó
$originator->restoreStateFromMemento($caretaker->getMemento()); // Khôi phục lại "State #2"
$originator->restoreStateFromMemento($caretaker->getMemento()); // Khôi phục lại "State #1"
```

#### **Observer**

Một object thông báo đến nhiều object khác khi có thay đổi.

**Mục đích:** Khi một object thay đổi, nó sẽ thông báo đến tất cả các observer.

**Ứng dụng thực tế:**

* Laravel Events & Listeners
* Hệ thống thông báo

```php
interface Observer {
    public function update(string $message): void;
}

class EmailSubscriber implements Observer {
    public function update(string $message): void {
        echo "Email nhận thông báo: " . $message;
    }
}

class EventManager {
    private array $observers = [];

    public function subscribe(Observer $observer): void {
        $this->observers[] = $observer;
    }

    public function notify(string $message): void {
        foreach ($this->observers as $observer) {
            $observer->update($message);
        }
    }
}

// Sử dụng Observer
$manager = new EventManager();
$manager->subscribe(new EmailSubscriber());
$manager->notify("Có sự kiện mới!");
```

#### **State**

Thay đổi hành vi của object khi trạng thái của nó thay đổi.

```php
// Interface State: định nghĩa hành vi chung của các trạng thái
interface State {
    public function handleRequest();
}

// Concrete States: Các lớp trạng thái cụ thể
class ConcreteStateA implements State {
    public function handleRequest() {
        echo "Đang xử lý ở trạng thái A\n";
    }
}

class ConcreteStateB implements State {
    public function handleRequest() {
        echo "Đang xử lý ở trạng thái B\n";
    }
}

// Context: Đối tượng sử dụng các trạng thái
class Context {
    private $state;

    public function __construct(State $state) {
        $this->state = $state;
    }

    public function setState(State $state) {
        $this->state = $state;
    }

    public function request() {
        $this->state->handleRequest();
    }
}

// Sử dụng State Pattern
$context = new Context(new ConcreteStateA());
$context->request();  // Output: Đang xử lý ở trạng thái A

// Thay đổi trạng thái
$context->setState(new ConcreteStateB());
$context->request();  // Output: Đang xử lý ở trạng thái B
```

#### **Strategy**

Chọn thuật toán hoặc phương thức xử lý lúc runtime mà không làm thay đổi code.

**Mục đích:** Chọn thuật toán xử lý linh hoạt.

**Ứng dụng thực tế:**

* Hệ thống thanh toán
* Xử lý sắp xếp dữ liệu

```php
interface PaymentStrategy {
    public function pay(int $amount): void;
}

class PayPalPayment implements PaymentStrategy {
    public function pay(int $amount): void {
        echo "Thanh toán qua PayPal: $" . $amount;
    }
}

class PaymentProcessor {
    public function __construct(private PaymentStrategy $strategy) {}

    public function process(int $amount): void {
        $this->strategy->pay($amount);
    }
}

// Sử dụng Strategy
$payment = new PaymentProcessor(new PayPalPayment());
$payment->process(100);
```

#### **Template Method**

Xác định một skeleton của thuật toán nhưng để subclass tự triển khai một số bước.

Khi nào sử dụng Template Method?

* Khi bạn có một  **thuật toán chung** , nhưng một số bước của thuật toán cần được tùy chỉnh hoặc thay đổi bởi các lớp con.
* Khi muốn **đảm bảo sự nhất quán trong quá trình thực thi** một thuật toán, nhưng cũng cần linh hoạt trong cách thực hiện các bước cụ thể của thuật toán.
* Khi muốn **tách biệt phần cấu trúc chung** của thuật toán khỏi phần chi tiết thực thi cụ thể.

```php
// Lớp trừu tượng với phương thức mẫu (Template Method)
abstract class AbstractClass {
    // Phương thức mẫu xác định thuật toán chung
    public function templateMethod() {
        $this->stepOne();
        $this->stepTwo();
        $this->stepThree();
    }

    // Các phương thức này có thể được ghi đè ở lớp con
    abstract protected function stepOne();
    abstract protected function stepTwo();
    abstract protected function stepThree();
}

// Lớp con 1 thực hiện các bước cụ thể của thuật toán
class ConcreteClassA extends AbstractClass {
    protected function stepOne() {
        echo "ConcreteClassA: Thực hiện bước 1\n";
    }

    protected function stepTwo() {
        echo "ConcreteClassA: Thực hiện bước 2\n";
    }

    protected function stepThree() {
        echo "ConcreteClassA: Thực hiện bước 3\n";
    }
}

// Lớp con 2 thực hiện các bước cụ thể khác của thuật toán
class ConcreteClassB extends AbstractClass {
    protected function stepOne() {
        echo "ConcreteClassB: Thực hiện bước 1\n";
    }

    protected function stepTwo() {
        echo "ConcreteClassB: Thực hiện bước 2 khác\n";
    }

    protected function stepThree() {
        echo "ConcreteClassB: Thực hiện bước 3 khác\n";
    }
}

// Sử dụng Template Method Pattern
$classA = new ConcreteClassA();
$classA->templateMethod();
// Output:
// ConcreteClassA: Thực hiện bước 1
// ConcreteClassA: Thực hiện bước 2
// ConcreteClassA: Thực hiện bước 3

$classB = new ConcreteClassB();
$classB->templateMethod();
// Output:
// ConcreteClassB: Thực hiện bước 1
// ConcreteClassB: Thực hiện bước 2 khác
// ConcreteClassB: Thực hiện bước 3 khác
```

#### **Visitor**

Thêm hành vi mới cho class mà không sửa đổi nó.Những Design Pattern hay được sử dụng nhất

**Khi nào sử dụng Visitor?**

* Khi bạn có một cấu trúc đối tượng phức tạp và muốn **thêm các hành vi mới** vào các đối tượng mà không thay đổi các lớp đó.
* Khi các hành vi liên quan đến các đối tượng cần được **tách biệt** ra khỏi lớp của đối tượng để giúp mở rộng và bảo trì hệ thống dễ dàng hơn.
* Khi bạn cần **thực thi các hành vi khác nhau** trên các đối tượng mà không làm thay đổi các lớp của chúng, ví dụ như các phép toán trên các loại hình đối tượng khác nhau.

```php
// Visitor Interface: Định nghĩa phương thức cho các lớp ConcreteVisitor
interface Visitor {
    public function visitElementA(ElementA $element);
    public function visitElementB(ElementB $element);
}

// Các lớp Element (Các đối tượng mà chúng ta sẽ "thăm")
interface Element {
    public function accept(Visitor $visitor);
}

class ElementA implements Element {
    public function accept(Visitor $visitor) {
        $visitor->visitElementA($this);
    }

    public function operationA() {
        echo "ElementA: Thực hiện operationA\n";
    }
}

class ElementB implements Element {
    public function accept(Visitor $visitor) {
        $visitor->visitElementB($this);
    }

    public function operationB() {
        echo "ElementB: Thực hiện operationB\n";
    }
}

// ConcreteVisitor: Các lớp Visitor thực hiện hành vi cụ thể
class ConcreteVisitor1 implements Visitor {
    public function visitElementA(ElementA $element) {
        echo "ConcreteVisitor1: Thực thi hành vi trên ElementA\n";
        $element->operationA();
    }

    public function visitElementB(ElementB $element) {
        echo "ConcreteVisitor1: Thực thi hành vi trên ElementB\n";
        $element->operationB();
    }
}

class ConcreteVisitor2 implements Visitor {
    public function visitElementA(ElementA $element) {
        echo "ConcreteVisitor2: Thực thi hành vi khác trên ElementA\n";
        $element->operationA();
    }

    public function visitElementB(ElementB $element) {
        echo "ConcreteVisitor2: Thực thi hành vi khác trên ElementB\n";
        $element->operationB();
    }
}

// Sử dụng Visitor Pattern
$elements = [new ElementA(), new ElementB()];

$visitor1 = new ConcreteVisitor1();
$visitor2 = new ConcreteVisitor2();

echo "Visitor1:\n";
foreach ($elements as $element) {
    $element->accept($visitor1);
}

echo "\nVisitor2:\n";
foreach ($elements as $element) {
    $element->accept($visitor2);
}
```

## Những Design Pattern hay được sử dụng nhất

**Singleton** (được dùng nhiều trong quản lý kết nối DB, logging, caching).

**Factory Method** (được sử dụng rộng rãi trong việc tạo object).

**Builder** (dùng khi tạo object có nhiều thuộc tính).

**Observer** (phổ biến trong hệ thống event-driven).

**Strategy** (dùng để chọn thuật toán xử lý linh hoạt).

**Decorator** (hay dùng trong UI và xử lý dữ liệu).
