**Định nghĩa:**
Onion Architecture là một mô hình kiến trúc phần mềm tập trung vào việc tách biệt các lớp trách nhiệm, giúp ứng dụng dễ bảo trì và mở rộng.
![Ảnh lỗi!](https://miro.medium.com/v2/resize:fit:962/1*Pd6l7Za_Pl8BI-kltDQukg.png)

**Gồm các tầng:**
- Domain Layer: 
  - Là lõi của hệ thống, chứa logic nghiệp vụ, thực thể (entities), và quy tắc kinh doanh, không phụ thuộc vào bất kỳ công nghệ nào.
- Application Layer: 
  - Xử lý các trường hợp sử dụng (use cases) và phối hợp dữ liệu giữa Domain và các lớp bên ngoài, phụ thuộc vào Domain.
- Infrastructure Layer:
  - Quản lý truy cập dữ liệu, dịch vụ bên ngoài (api), và các mối quan tâm cơ sở hạ tầng, phụ thuộc vào Domain và Application.
- Presentation Layer: 
  - Xử lý giao diện người dùng hoặc API hoặc giao diện web, phụ thuộc vào Application và có thể là Domain.

**Cây thư mục:**
` 
src
  main
    java/com/example
      domain
        entity
          User.java
          Order.java
        contract
          UserRepository.java
          OrderRepository.java
        exception
      application
        action
          UserAction.java
          OrderAction.java
      infrastructure
        api
        db
          entity
            UserEntity.java
            OrderEntity.java
          repository
            UserRepositoryImpl.java
            OrderRepositoryImpl.java
      presentation
        http
          request
              GetUserDetailRequest.java
              CreateOrderRequest.java
          controller
              GetUserDetailController.java
              CreateOrderController.java
          response
        job
        consume
      configuation 
test
`
```sh
src
└── main
    └── java/com/example
        ├── domain
        │   ├── entity
        │   │   ├── User.java
        │   │   └── Order.java
        │   ├── contract
        │   │   ├── UserRepository.java
        │   │   └── OrderRepository.java
        │   └── exception
        ├── application
        │   └── action
        │       ├── UserAction.java
        │       └── OrderAction.java
        ├── infrastructure
        │   ├── api
        │   └── db
        │       ├── entity
        │       │   ├── UserEntity.java
        │       │   └── OrderEntity.java
        │       └── repository
        │           ├── UserRepositoryImpl.java
        │           └── OrderRepositoryImpl.java
        ├── presentation
        │   ├── http
        │   │   ├── request
        │   │   │   ├── GetUserDetailRequest.java
        │   │   │   └── CreateOrderRequest.java
        │   │   ├── controller
        │   │   │   ├── GetUserDetailController.java
        │   │   │   └── CreateOrderController.java
        │   │   └── response
        │   ├── job
        │   └── consume
        └── configuation 
test
```

**Mối Quan Hệ Phụ Thuộc:**
Domain: Không phụ thuộc vào bất kỳ dự án nào, là lõi của hệ thống.
Application: Phụ thuộc vào Domain, chứa logic ứng dụng.
Infrastructure: Phụ thuộc vào Domain và Application, thực hiện các giao diện được định nghĩa trong Application.
Presentation: Phụ thuộc vào Application, xử lý giao diện người dùng.
WebApp: Dự án chính, tham chiếu tất cả các lớp trên và thiết lập tiêm phụ thuộc (dependency injection) để kết nối Infrastructure với Application.

**Chi tiết các layer:**
Domain: Lõi nghiệp vụ, chứa thực thể và giao diện.
Application: Xử lý use cases, phụ thuộc vào Domain.
Infrastructure: Quản lý cơ sở hạ tầng, phụ thuộc vào Domain và Application.
Presentation: Xử lý giao diện, phụ thuộc vào Application.