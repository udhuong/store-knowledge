## DispatcherServlet

`DispatcherServlet` là trung tâm điều phối tất cả các request trong Spring MVC. Nó hoạt động như một  **Front Controller** , tiếp nhận các yêu cầu từ client, sau đó điều phối chúng đến các thành phần thích hợp để xử lý và trả về phản hồi.

### **Luồng hoạt động của DispatcherServlet**

Khi một request được gửi đến Spring MVC, nó sẽ trải qua các bước sau:

1. **Client gửi request**
   * Người dùng gửi một HTTP request đến server, ví dụ: `GET /users`.
2. **Request đi qua Front Controller (`DispatcherServlet`)**
   * `DispatcherServlet` tiếp nhận request và xác định nó cần được xử lý bởi component nào.
3. **Tìm kiếm Handler phù hợp (`HandlerMapping`)**
   * `DispatcherServlet` sử dụng `HandlerMapping` để tìm ra controller nào sẽ xử lý request dựa vào URL và method HTTP.
4. **Gọi Handler Adapter để xử lý request (`HandlerAdapter`)**
   * Khi tìm thấy handler (controller phù hợp), `DispatcherServlet` sẽ sử dụng `HandlerAdapter` để gọi phương thức của controller đó.
5. **Controller xử lý logic và trả về `ModelAndView`**
   * Controller nhận request, xử lý logic (ví dụ: lấy dữ liệu từ database) và trả về một `ModelAndView` hoặc một `ResponseEntity` (nếu dùng REST API).
6. **Xử lý kết quả và chọn View Resolver**
   * Nếu request yêu cầu một giao diện HTML, `DispatcherServlet` sử dụng `ViewResolver` để tìm file giao diện phù hợp (ví dụ: `Thymeleaf` hoặc `JSP`).
   * Nếu là REST API, dữ liệu JSON sẽ được trả về.
7. **Trả response về cho client**
   * `DispatcherServlet` gửi phản hồi cuối cùng về cho client.

### **Luồng chạy của DispatcherServlet trong Spring Boot**

1. **Client gửi request** 📨
   * Người dùng gửi HTTP request đến ứng dụng Spring Boot (ví dụ: `GET /users/1`).
2. **Request đi qua `DispatcherServlet` (Front Controller)**
   * `DispatcherServlet` tiếp nhận request và chuyển tiếp xử lý đến các thành phần thích hợp.
3. **Tìm kiếm Handler phù hợp (`HandlerMapping`)**
   * `DispatcherServlet` sử dụng `RequestMappingHandlerMapping` để tìm kiếm controller nào sẽ xử lý request dựa trên annotation `@RequestMapping`, `@GetMapping`, `@PostMapping`,...
4. **Gọi Handler Adapter (`HandlerAdapter`) để xử lý request**
   * `DispatcherServlet` tìm `HandlerAdapter` thích hợp và gọi phương thức của Controller.
5. **Controller xử lý logic nghiệp vụ và trả về kết quả**
   * Controller xử lý dữ liệu (gọi service, repository, database,...)
   * Trả về một `ResponseEntity`, `ModelAndView`, hoặc `JSON/XML` tùy theo kiểu ứng dụng.
6. **Chọn View (nếu cần) thông qua `ViewResolver`**
   * Nếu ứng dụng là REST API → Không cần `ViewResolver`, mà Spring Boot sẽ tự động trả về JSON/XML bằng `MappingJackson2HttpMessageConverter`.
   * Nếu ứng dụng có giao diện web → `DispatcherServlet` sử dụng `ViewResolver` (ví dụ: Thymeleaf, JSP, Freemarker) để render trang HTML.
7. **DispatcherServlet gửi response về client**
   * Dữ liệu được gửi về client dưới dạng JSON/XML hoặc HTML.

### **So sánh Spring MVC truyền thống vs Spring Boot**

| **Tính năng**                      | **Spring MVC truyền thống**                   | **Spring Boot**                        |
| ------------------------------------------ | ----------------------------------------------------- | -------------------------------------------- |
| **Cấu hình `DispatcherServlet`** | Cần khai báo trong `web.xml`hoặc `Java Config` | Tự động cấu hình, không cần khai báo |
| **Cấu hình `ViewResolver`**      | Cần đăng ký thủ công                            | Được cấu hình tự động                |
| **Tạo Bean `HandlerMapping`**     | Cần khai báo trong `@Configuration`               | Spring Boot tự động cấu hình            |
| **Trả về JSON/XML**                | Cần cấu hình `Jackson`hoặc `MessageConverter` | Spring Boot tự động chuyển đổi         |

## IoC (Inversion of Control) - Đảo ngược quyền điều khiển

**IoC (Inversion of Control)** là một nguyên tắc lập trình giúp đảo ngược quyền kiểm soát việc khởi tạo dependencies.

Với IoC, Spring sẽ tự động tạo và quản lý `UserRepository`, bạn chỉ cần sử dụng nó mà không cần khởi tạo thủ công

Spring chịu trách nhiệm tạo và cung cấp `UserRepository` mà không cần `new` thủ công. Đây chính là nguyên tắc IoC.

## DI (Dependency Injection) - Tiêm phụ thuộc

**DI (Dependency Injection)** là một cách để hiện thực hóa  **IoC** , bằng cách  **tiêm (inject) dependencies vào một class thay vì để class đó tự tạo chúng** .

**Có 3 cách tiêm phụ thuộc (DI) trong Spring:**

### **Constructor Injection (Khuyến khích dùng)**

**Ưu điểm** :

* Dễ dàng kiểm thử (Unit Test).
* Hỗ trợ `final` để đảm bảo biến không bị thay đổi sau khi khởi tạo.

### **Setter Injection**

**Ưu điểm**: Có thể thay đổi dependency trong runtime.

**Nhược điểm**: Không đảm bảo dependency luôn có giá trị (có thể bị set `null`).

### **Field Injection (Không khuyến khích)**

**Nhược điểm** :

* Khó kiểm thử vì không thể inject dependency từ bên ngoài.
* Không thể đánh dấu `final`, khiến đối tượng có thể bị thay đổi.

## **IoC Container là gì?**

**IoC Container** là một **công cụ** thực thi nguyên tắc IoC, giúp quản lý việc tạo, cung cấp, và inject dependencies vào ứng dụng.

**IoC Container giúp:**

* Tự động khởi tạo object và inject dependencies ( **Dependency Injection - DI** ).
* Giảm sự phụ thuộc giữa các class ( **Loose Coupling** ).
* Dễ dàng thay đổi, mở rộng, và test code ( **Maintainability & Testability** ).

## Bean Factory và Application Context

BeanFactory là  **IoC Container tối giản** , cung cấp cơ chế Dependency Injection cơ bản.

ApplicationContext mở rộng BeanFactory và bổ sung nhiều tính năng mạnh mẽ hơn.

**So sánh BeanFactory vs ApplicationContext**

| Đặc điểm                   | **BeanFactory**   | **ApplicationContext**       |
| ------------------------------ | ----------------------- | ---------------------------------- |
| **Loading Beans**        | Lazy loading (khi cần) | Eager loading (ngay khi app start) |
| **Event Handling**       | ❌ Không hỗ trợ      | ✅ Hỗ trợ event listeners        |
| **Internationalization** | ❌ Không hỗ trợ      | ✅ Hỗ trợ i18n                   |
| **Annotation Scanning**  | ❌ Không hỗ trợ      | ✅ Hỗ trợ @ComponentScan         |
| **AOP Support**          | ❌ Không hỗ trợ      | ✅ Hỗ trợ AOP                    |
| **Enterprise Ready**     | ❌ Không phù hợp     | ✅ Được khuyên dùng           |

## Bean

Trong Spring, **Bean** là một đối tượng do Spring IoC Container quản lý. Các Bean này được khai báo, khởi tạo, và quản lý hoàn toàn bởi Spring.

**Các cách khởi tạo Bean trong Spring**

**Cách 1: Dùng Annotation (@Component, @Service, @Repository)**

* Spring tự động quét và tạo Bean khi sử dụng các annotation này.

**Cách 2: Dùng @Bean trong Java Configuration (Cách hiện đại nhất)**

* Bạn có thể tạo Bean bằng cách định nghĩa trong một class cấu hình (`@Configuration`).
* **Ưu điểm** : Dễ kiểm soát, không cần scan package.

Cách 3: Dùng XML Configuration (Cách cũ, ít dùng)

* **Nhược điểm** : Rườm rà, khó bảo trì.  **Spring Boot không khuyến khích cách này** .

Cách 4: Dùng `@PostConstruct` để khởi tạo sau khi Bean được tạo

* @Lazy để chỉ khởi tạo bean khi được gọi

### **Scope của Bean trong Spring**

Spring hỗ trợ nhiều **scope** để kiểm soát vòng đời của Bean.

| **Scope**                                | **Mô tả**                                       | **Khi nào dùng?**                      |
| ---------------------------------------------- | ------------------------------------------------------- | ---------------------------------------------- |
| **singleton**(Mặc định)               | Một instance duy nhất trong toàn bộ ứng dụng      | Hầu hết các service                         |
| **prototype**                            | Mỗi lần gọi `getBean()`sẽ tạo một instance mới | Khi cần đối tượng không dùng chung      |
| **request**(Chỉ dùng với Web App)     | Một instance cho mỗi HTTP request                     | Dùng cho Controller xử lý request           |
| **session**(Chỉ dùng với Web App)     | Một instance cho mỗi HTTP session                     | Lưu dữ liệu user theo session               |
| **application**(Chỉ dùng với Web App) | Một instance chung cho toàn bộ ứng dụng            | Giống singleton nhưng chỉ trong context web |

## @PostConstruct trong Spring là gì?

`@PostConstruct` là một annotation trong Java (thuộc `javax.annotation`), được Spring sử dụng để đánh dấu  **một method sẽ tự động chạy sau khi bean được khởi tạo và dependency injection hoàn tất** .

**Khi nào nên dùng `@PostConstruct`?**

* Load dữ liệu ban đầu vào cache.
* Mở kết nối database hoặc khởi tạo tài nguyên cần thiết.
* Kiểm tra và thiết lập một số cấu hình sau khi inject dependencies.

`@PostConstruct` sẽ  **không hoạt động trong Spring Boot 3+** . **Cách thay thế:** Dùng `@EventListener(ApplicationReadyEvent.class)` . Phương thức này chạy ngay khi Spring Boot hoàn tất khởi tạo Bean.

## AOP

**AOP (Aspect-Oriented Programming - Lập trình hướng khía cạnh)** là một kỹ thuật lập trình giúp **tách biệt các chức năng phụ trợ (cross-cutting concerns)** ra khỏi logic chính của ứng dụng.

💡 **Ví dụ:** Logging, bảo mật, transaction management, cache,... **nên được tách riêng** thay vì viết lặp lại trong nhiều class.

**Vì sao cần AOP?**

**AOP giúp tách các chức năng này** ra khỏi logic chính của ứng dụng, giúp code  **gọn gàng, dễ mở rộng và dễ bảo trì** .

## @Anotation

```java
package com.example.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD) // Chỉ áp dụng cho phương thức
@Retention(RetentionPolicy.RUNTIME) // Giữ lại lúc runtime để AOP đọc được
public @interface LogExecutionTime {
    // Có thể thêm thuộc tính nếu cần, ví dụ:
    String value() default ""; // Giá trị tùy chọn để mô tả
}
```

## **SpEL** (Spring Expression Language)

Trong Spring, **SpEL** (Spring Expression Language) là một ngôn ngữ biểu thức mạnh mẽ được sử dụng để truy vấn, thao tác dữ liệu hoặc thực hiện các phép tính tại runtime, cho phép bạn viết các biểu thức linh hoạt để xử lý logic.

SpEL là một công cụ rất linh hoạt trong Spring, giúp bạn xử lý logic động mà không cần viết mã cứng (hardcode).

## Spring Data

## Testing

```
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
  
    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void testGetAllUsers() {
        // Mock dữ liệu
        List<User> mockUsers = Arrays.asList(new User(1L, "John", "john@gmail.com"));
        when(userRepository.findAll()).thenReturn(mockUsers);

        // Gọi method và kiểm tra kết quả
        List<User> users = userService.getAllUsers();
        assertEquals(1, users.size());
        assertEquals("John", users.get(0).getName());
    }

    @Test
    void testGetUserByEmail() {
        User mockUser = new User(1L, "Alice", "alice@gmail.com");
        when(userRepository.findByEmail("alice@gmail.com")).thenReturn(mockUser);

        User user = userService.getUserByEmail("alice@gmail.com");
        assertNotNull(user);
        assertEquals("Alice", user.getName());
    }
}
```

**Giải thích:**

* `@Mock` tạo một mock object cho `UserRepository`.
* `@InjectMocks` inject `UserRepository` vào `UserService`.

* `when(...).thenReturn(...)` giả lập dữ liệu trả về từ database.
