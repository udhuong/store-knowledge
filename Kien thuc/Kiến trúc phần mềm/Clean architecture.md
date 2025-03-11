**Định nghĩa:**
- Clean Architecture là một mô hình kiến trúc phần mềm. Nó giúp tổ chức code theo cách dễ bảo trì, dễ mở rộng, và tách biệt các tầng trong ứng dụng.
- Mô hình này dựa trên nguyên tắc Separation of Concerns (SoC), giúp chia tách logic kinh doanh khỏi các chi tiết triển khai như cơ sở dữ liệu, giao diện người dùng, framework...

![Ảnh lỗi!](https://edwardthienhoang.wordpress.com/wp-content/uploads/2018/01/cleanarchitecture-5c6d7ec787d447a81b708b73abba1680.jpg)

**Gồm các tầng:**
- Entities (Domain Layer): Chứa các model cốt lõi và logic nghiệp vụ đơn giản.
- Use Cases (Application Layer): Chứa logic nghiệp vụ chính của hệ thống.
- Interfaces (Adapter Layer): Chứa Controller (API) và giao tiếp với người dùng.
- Infrastructure (Framework & Database Layer): Chứa kết nối DB, lưu trữ file, gọi API bên ngoài.

**Cây thư mục:**

```sh
📂 src
└── 📂 main
    └── 📂 java/com/example
        ├── 📂 domain # 🌟 Entities (Domain Layer)
        │   ├── 📂 entity
        │   │   ├── User.java
        │   │   └── Order.java
        │   └── 📂 contract
        │       ├── UserRepository.java
        │       └── OrderRepository.java
        ├── 📂 application # 🌟 Use Cases (Application Layer)
        │   ├── UserService.java
        │   └── OrderService.java
        ├── 📂 infrastructure # 🌟 Adapter Layer (Database, API)
        │   ├── 📂 api
        │   └── 📂 repository
        │       ├── UserRepositoryImpl.java
        │       └── OrderRepositoryImpl.java
        ├── 📂 interfaces # 🌟 API Layer (Controllers, REST)
        │   ├── 📂 http
        │   │   ├── 📂 request
        │   │   │   ├── GetUserDetailRequest.java
        │   │   │   └── CreateOrderRequest.java
        │   │   ├── 📂 controller
        │   │   │   ├── GetUserDetailController.java
        │   │   │   └── CreateOrderController.java
        │   │   └── 📂 response
        │   ├── 📂 job
        │   └── 📂 consume
        └── 📂 configuation 
test

```