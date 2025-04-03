## Repository

> Repository Pattern — một mẫu thiết kế rất phổ biến khi làm việc với CSDL (database). Giúp tách biệt logic truy vấn dữ liệu khỏi logic xử lý nghiệp vụ.

Thường sẽ có 1 interface định nghĩa các method thao tác với db, sau đó sẽ có 1 class implement triển khai chi tết các method đó.

**Có 2 cách sử dụng:**

- class implement interface
- class extend abstract class và implement interface

**Cách 1:**

- tạo interface với các method create, update, findByIds, deleteById
- class implement các method đó

**Cách 2:**

- tạo 1 interface ClassARepository có các method riêng. VD: findByUsername
- tạo 1 abstract class BaseRepository
  + triển khai các method chung cơ bản. VD: create, update
  + có 1 method abstract getModel() (hoặc bất kì tên gì để gán model hoặc entity)
- class ClassARepositoryImpl extends BaseRepository impelement ClassARepository
  + triển khai method getModel(). Trả về model hoặc entity cụ thể.
  + triển khai các method riêng (findByUsername)
- như này ClassARepositoryImpl sẽ có cả 3 method, và k cần phải viết lại triển khai create và update

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
