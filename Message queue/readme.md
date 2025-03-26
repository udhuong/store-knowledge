## Message Broker là gì?

Message Broker là một phần mềm trung gian (middleware) đóng vai trò như một cầu nối để hỗ trợ việc trao đổi thông tin giữa các ứng dụng, hệ thống hoặc dịch vụ khác nhau. Nó hoạt động bằng cách nhận thông điệp (messages) từ một nguồn (gọi là producer hoặc sender), xử lý hoặc định tuyến chúng, sau đó chuyển tiếp đến các đích đến phù hợp (gọi là consumer hoặc receiver).

### **Lợi ích của Message Broker**

* **Tách biệt các thành phần** : Các ứng dụng có thể giao tiếp mà không cần kết nối trực tiếp.
* **Tăng tính chịu lỗi** : Nếu một hệ thống bị lỗi, message broker có thể lưu trữ tin nhắn và gửi lại khi hệ thống hoạt động trở lại.
* **Cân bằng tải** : Có thể phân phối thông điệp đến nhiều worker xử lý, giúp tăng hiệu suất.
* **Hỗ trợ giao tiếp bất đồng bộ** : Dịch vụ có thể gửi tin nhắn mà không cần chờ phản hồi ngay lập tức.

### **Các kiểu Message Broker**

1. **Point-to-Point (Hàng đợi – Queue)**
   * Một producer gửi tin nhắn vào queue.
   * Một consumer nhận và xử lý tin nhắn.
   * Mỗi tin nhắn chỉ được xử lý bởi một consumer.
   * Ví dụ: RabbitMQ, ActiveMQ.
2. **Publish/Subscribe (Pub/Sub – Chủ đề)**
   * Một publisher gửi tin nhắn lên một chủ đề (topic).
   * Nhiều subscriber có thể nhận tin nhắn từ chủ đề đó.
   * Ví dụ: Kafka, Redis Pub/Sub, Google Pub/Sub.

### **Một số Message Broker phổ biến**

* **RabbitMQ** : Hỗ trợ nhiều mô hình giao tiếp, sử dụng hàng đợi (queue-based).
* **Apache Kafka** : Hỗ trợ xử lý dữ liệu theo luồng (stream processing), phù hợp với big data.
* **Redis Pub/Sub** : Nhẹ, nhanh, nhưng không đảm bảo tin nhắn được lưu trữ lâu dài.
* **ActiveMQ** : Một giải pháp message queue mạnh mẽ cho Java.


## Message Queue là gì?

**Message Queue (MQ)** là một cơ chế hàng đợi tin nhắn, giúp các hệ thống giao tiếp với nhau theo mô hình  **bất đồng bộ** . Nó hoạt động theo nguyên tắc  **producer-consumer** , trong đó:

* **Producer** : Gửi tin nhắn vào hàng đợi (queue).
* **Queue** : Lưu trữ tin nhắn tạm thời cho đến khi được xử lý.
* **Consumer** : Nhận và xử lý tin nhắn từ hàng đợi.

### **Tại sao dùng Message Queue?**

* **Giảm tải & Cân bằng tải** : Consumer có thể xử lý tin nhắn theo tốc độ của nó mà không bị quá tải.
* **Tăng khả năng chịu lỗi** : Nếu một service bị lỗi, tin nhắn vẫn được lưu trữ để xử lý sau.
* **Giao tiếp bất đồng bộ** : Producer không cần chờ consumer xử lý xong.
* **Mở rộng hệ thống dễ dàng** : Có thể thêm nhiều consumer để tăng hiệu suất.

### **Cách hoạt động của Message Queue**

1. **Producer** gửi tin nhắn vào hàng đợi.
2. **Message Queue** lưu trữ tin nhắn tạm thời.
3. **Consumer** lấy tin nhắn từ hàng đợi và xử lý.
4. Khi xử lý xong, tin nhắn có thể bị xóa khỏi hàng đợi hoặc được lưu trữ tùy vào cấu hình.

### **Các hệ thống Message Queue phổ biến**

* **RabbitMQ** : Dựa trên AMQP, hỗ trợ hàng đợi mạnh mẽ.
* **Apache Kafka** : Dành cho xử lý dữ liệu theo luồng (stream processing).
* **ActiveMQ** : Một lựa chọn phổ biến cho Java.
* **AWS SQS** : Dịch vụ hàng đợi tin nhắn trên AWS.
* **Redis Lists (Queue)** : Redis có thể được dùng như một hàng đợi nhẹ.
