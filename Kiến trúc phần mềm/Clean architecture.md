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
src/
├── domain/                        # Entities & Domain Service (logic nghiệp vụ thuần)
│   ├── model/                     # Entities, Value Objects, Aggregate Roots
│   │   ├── Order.java
│   │   └── Product.java
│   ├── service/                   # Domain Services
│   │   └── OrderDomainService.java
│   ├── repository/                # Repository interface
│   │   └── OrderRepository.java
│   └── event/                     # Domain events
│       └── OrderCreatedEvent.java
│
├── application/                   # Use cases (Application business rules)
│   ├── usecase/                   # Use case service (Application Service)
│   │   └── PlaceOrderUseCase.java
│   ├── dto/                       # Request/Response DTOs
│   │   ├── PlaceOrderRequest.java
│   │   └── PlaceOrderResponse.java
│   └── mapper/                    # DTO <-> Entity mappers
│       └── OrderMapper.java
│
├── infrastructure/                # Frameworks, Drivers (implementations)
│   ├── repository/                # Repository implementations
│   │   └── JpaOrderRepository.java
│   ├── external/                  # External services (API client, Payment, Email...)
│   │   └── PaymentGatewayClient.java
│   ├── queue/                     # Message queue (RabbitMQ, Kafka, Redis queue)
│   │   └── OrderCreatedQueue.java
│   ├── job/                       # Background jobs, schedulers
│   │   └── OrderCleanupJob.java
│   └── config/                    # App configurations (Spring config, Beans...)
│       └── AppConfig.java
│
├── interface/                     # Adapters layer (Controller, CLI, Websocket, etc.)
│   ├── api/                       # REST or GraphQL API
│   │   ├── controller/            # Controllers
│   │   │   └── OrderController.java
│   │   ├── security/              # Auth & Security (JWT, OAuth, etc.)
│   │   │   └── SecurityConfig.java
│   │   └── exception/             # Exception handling
│   │       └── GlobalExceptionHandler.java
│   ├── job/                       # Scheduler/cron adapter (Trigger background jobs)
│   │   └── JobRunner.java
│   ├── queue/                     # Queue listener adapter
│   │   └── OrderQueueListener.java
│   └── cli/                       # Command line interface (nếu có)
│       └── OrderCommand.java
│
└── Application.java               # Main entrypoint

```

```
src/
├── Domain/
│   ├── User/
│   │   ├── Entities/
│   │   │   └── User.php
│   │   ├── ValueObjects/
│   │   │   └── Email.php
│   │   ├── Repositories/
│   │   │   └── UserRepositoryInterface.php
│   │   └── Events/
│   │       └── UserRegistered.php
│
├── Application/
│   ├── User/
│   │   ├── Commands/
│   │   │   └── RegisterUserCommand.php
│   │   ├── Handlers/
│   │   │   └── RegisterUserHandler.php
│   │   ├── Services/
│   │   │   └── RegisterUserService.php
│   │   └── DTOs/
│   │       └── RegisterUserDTO.php
│
├── Infrastructure/
│   ├── Persistence/
│   │   └── Repositories/
│   │       └── EloquentUserRepository.php
│   ├── Mail/
│   │   └── UserWelcomeMailer.php
│   └── Queue/
│       └── SendWelcomeEmailJob.php
│
├── Presentation/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   └── UserController.php
│   │   └── Requests/
│   │       └── RegisterUserRequest.php
│   ├── Console/
│   │   └── Commands/
│   │       └── SendMailCommand.php
│   └── Routes/
│       ├── web.php
│       └── api.php
│
└── Support/
    ├── Providers/
    │   └── AppServiceProvider.php
    └── Helpers.php

```

### **Giải thích từng phần**

| Layer                    | Vai trò                                                   |
| ------------------------ | ---------------------------------------------------------- |
| **Domain**         | Quy tắc nghiệp vụ cốt lõi, Entity, ValueObject        |
| **Application**    | Use case, flow nghiệp vụ, DTO, command-handler           |
| **Infrastructure** | Giao tiếp với công nghệ: DB, mail, queue, external API |
| **Presentation**   | Nhận yêu cầu từ client: HTTP, Console, Job, Listener   |
| **Support**        | Service Provider, config, helper chung                     |
