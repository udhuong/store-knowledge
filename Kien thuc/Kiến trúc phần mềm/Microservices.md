## Khái niệm
- Microservices là một kiến trúc phần mềm
- chia thành nhiều dịch vụ nhỏ và độc lập. Mỗi dịch vụ là một thành phần riêng lẻ, có cơ sở dữ liệu riêng (hoặc chia sẻ tùy trường hợp).
- Giao tiếp qua API: Các dịch vụ trao đổi dữ liệu với nhau bằng REST API, gRPC, hoặc Message Broker (Kafka, RabbitMQ).
- Triển khai độc lập: Có thể triển khai, cập nhật, hoặc mở rộng từng dịch vụ mà không ảnh hưởng đến hệ thống tổng thể.

**Ví dụ: Một hệ thống thương mại điện tử có thể chia thành các microservice như:**
- User Service: Quản lý thông tin người dùng.
- Product Service: Quản lý sản phẩm.
- Order Service: Xử lý đơn hàng.
- Payment Service: Thanh toán và xử lý giao dịch.

Mỗi dịch vụ hoạt động như một ứng dụng nhỏ, chạy độc lập và giao tiếp với nhau qua mạng.

## Ưu điểm
- Dễ mở rộng: Có thể scale từng dịch vụ riêng biệt (ví dụ, chỉ scale Payment Service khi lượng giao dịch tăng).
- Triển khai nhanh: Mỗi nhóm phát triển có thể làm việc và triển khai từng dịch vụ độc lập.
- Khả năng chịu lỗi: Nếu một dịch vụ gặp sự cố, toàn bộ hệ thống không bị sập hoàn toàn.
- Linh hoạt công nghệ: Mỗi dịch vụ có thể sử dụng công nghệ khác nhau (Java, Node.js, Python...) mà không ràng buộc lẫn nhau.

## Thách thức của Microservices
- Phức tạp khi quản lý: Hệ thống nhiều dịch vụ đòi hỏi công cụ hỗ trợ giám sát, log tập trung và xử lý lỗi phân tán.
- Triển khai & CI/CD: Phải xây dựng pipeline CI/CD tốt để quản lý nhiều service.
- Giao tiếp giữa các service: Có thể gặp vấn đề về network latency hoặc cần xử lý circuit breaker để tránh cascading failure (sự cố lan truyền).

## Khi nào nên dùng Microservices?
- Ứng dụng lớn, nhiều tính năng phức tạp.
- Đội ngũ phát triển lớn, cần làm việc song song trên nhiều module.
- Yêu cầu khả năng mở rộng cao, chịu tải lớn.
- Muốn linh hoạt trong công nghệ và chiến lược triển khai.
- Nếu ứng dụng nhỏ và ít thay đổi, có thể dùng Monolithic sẽ đơn giản và dễ quản lý hơn.