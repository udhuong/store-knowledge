**Kiến trúc Hexagonal (Hexagonal Architecture)** , còn được gọi là  **"Ports and Adapters"** , là một cách tổ chức code nhằm tạo ra các ứng dụng linh hoạt, dễ bảo trì và dễ kiểm thử. Nó giúp tách biệt logic nghiệp vụ (business logic) khỏi các thành phần bên ngoài như cơ sở dữ liệu, API, giao diện người dùng, v.v.

# **Cấu trúc tổng quan**

Kiến trúc này chia ứng dụng thành ba phần chính:

1. **Core (Business Logic - Domain Layer)**
   * Chứa các quy tắc nghiệp vụ thuần túy.
   * Không phụ thuộc vào framework, cơ sở dữ liệu, hoặc công nghệ bên ngoài.
2. **Ports (Cổng giao tiếp)**
   * Là giao diện (interface) giúp Core giao tiếp với bên ngoài.
   * Ví dụ: Repository (để làm việc với DB), Service (để giao tiếp với API khác).
3. **Adapters (Bộ chuyển đổi)**
   * Hiện thực (implement) các `Ports` để kết nối với thế giới bên ngoài.
   * Ví dụ: Adapter kết nối với PostgreSQL, Kafka, Facebook API,...

# **Lợi ích của Kiến trúc Hexagonal**

* **Tách biệt rõ ràng giữa logic nghiệp vụ và các thành phần bên ngoài**
* **Dễ kiểm thử (unit test, integration test)**
* **Dễ thay đổi công nghệ (chẳng hạn thay đổi từ MySQL sang MongoDB mà không ảnh hưởng đến Core)**
* **Dễ mở rộng, bảo trì và áp dụng các nguyên tắc SOLID**

# **Khi nào nên sử dụng kiến trúc Hexagonal?**

* Khi bạn cần **tạo ứng dụng linh hoạt** dễ thay đổi công nghệ.
* Khi bạn muốn  **tách biệt rõ ràng giữa business logic và công nghệ bên ngoài** .
* Khi bạn muốn  **áp dụng Clean Architecture và DDD (Domain-Driven Design)** .
* Khi bạn  **định hướng Microservices** .

```txt
src/
├── domain/                            # Core domain logic
│   ├── model/                         # Entities, Value Objects
│   │   ├── Order.java
│   │   └── Product.java
│   ├── service/                       # Domain services
│   │   └── OrderDomainService.java
│   ├── port/                          # Ports: input + output (interfaces)
│   │   ├── input/                     # Use case interface (Application service interface)
│   │   │   └── PlaceOrderUseCase.java
│   │   └── output/                    # Output ports (Repository, External service interface)
│   │       ├── OrderRepository.java
│   │       └── PaymentGateway.java
│   └── event/                         # Domain events (nếu có)
│       └── OrderCreatedEvent.java
│
├── application/                       # Use case implementation
│   ├── service/                       # Application services implement input ports
│   │   └── PlaceOrderService.java
│   ├── dto/                           # Request/Response DTOs
│   │   ├── PlaceOrderRequest.java
│   │   └── PlaceOrderResponse.java
│   └── mapper/                        # DTO <-> Domain Model mappers
│       └── OrderMapper.java
│
├── adapter/                           # Adapters (implement output ports / input ports)
│   ├── in/                            # Input adapters (driving)
│   │   ├── web/                       # REST API adapter
│   │   │   ├── controller/            # Controllers
│   │   │   │   └── OrderController.java
│   │   │   ├── security/              # Security config (JWT, OAuth)
│   │   │   │   └── SecurityConfig.java
│   │   │   └── exception/             # Global exception handler
│   │   │       └── GlobalExceptionHandler.java
│   │   ├── cli/                       # Command line interface (nếu có)
│   │   │   └── OrderCommand.java
│   │   └── queue/                     # Queue consumers (e.g., Kafka, RabbitMQ)
│   │       └── OrderQueueListener.java
│   │
│   └── out/                           # Output adapters (driven)
│       ├── persistence/               # Persistence adapters (JPA, MongoDB...)
│       │   ├── JpaOrderRepository.java
│       │   └── OrderEntity.java
│       ├── external/                  # External services (payment gateways, email, etc.)
│       │   └── PaymentGatewayAdapter.java
│       └── eventpublisher/            # Event publisher (e.g., Spring events, Kafka)
│           └── OrderEventPublisher.java
│
├── config/                            # Application config (Spring config, Beans...)
│   └── AppConfig.java
│
└── Application.java                   # Main entrypoint

```
