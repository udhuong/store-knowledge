## Event Driven Architecture (EDA) là gì?

Event-Driven Architecture (Kiến trúc hướng sự kiện) là một mô hình kiến trúc mà thành phần của hệ thống giao tiếp với nhau thông qua các sự kiện (event).

Khi một sự kiện xảy ra, nó sẽ được "publish", các thành phần khác nếu quan tâm (subscriber) sẽ "listen" và xử lý.

Định nghĩa ngắn gọn:

* **Publisher** : Phát ra event.
* **Subscriber / Consumer** : Lắng nghe event và xử lý.
* **Event Bus / Broker** : Cầu nối trung gian để truyền event (Kafka, RabbitMQ, Redis Streams...).

Ví dụ đơn giản:

1. **Bạn đặt hàng trong website** (Service Order): Service Order publish event: `OrderPlacedEvent`
2. **Service Email** : Lắng nghe `OrderPlacedEvent` và gửi email xác nhận cho khách.
3. **Service Inventory** : Lắng nghe `OrderPlacedEvent` và giảm số lượng tồn kho.
4. **Service Analytics** : Lắng nghe `OrderPlacedEvent` để ghi nhận phân tích đơn hàng.

### Ưu điểm của Event Driven Architecture

| Ưu điểm                     | Giải thích                                                                              |
| ------------------------------ | ----------------------------------------------------------------------------------------- |
| 📦 Tách biệt chặt chẽ      | Các service giao tiếp qua event nên không phụ thuộc chặt vào nhau.                |
| ⚡ Hiệu năng cao, scale tốt | Dễ mở rộng hệ thống khi có thêm consumer mới mà không cần thay đổi producer. |
| 🔄 Dễ mở rộng               | Muốn thêm tính năng mới chỉ cần thêm consumer mới lắng nghe event.              |
| 🧪 Dễ test từng service      | Test độc lập, kiểm thử từng consumer mà không cần service khác.                 |

### Nhược điểm

| Nhược điểm                | Giải thích                                                                         |
| ----------------------------- | ------------------------------------------------------------------------------------ |
| 😵 Debug khó hơn            | Event bất đồng bộ nên khó trace flow end-to-end.                               |
| 🔄 Data consistency           | Eventual consistency: Dữ liệu nhất quán theo thời gian, không phải tức thì. |
| 🧩 Phải thiết kế kỹ event | Thiết kế event schema, event versioning cẩn thận để tránh vỡ flow.           |

## Khi nào dùng EDA?

* Microservices giao tiếp với nhau.
* Hệ thống cần mở rộng dễ dàng (scalable).
* Xử lý nghiệp vụ phức tạp, cần trigger nhiều hành động cho 1 sự kiện.
* Hệ thống thời gian thực (Realtime Analytics, Notification System, IoT).

## So sánh nhanh với các mô hình khác

| Mô hình    | Giao tiếp                    | Mối quan hệ      | Độ mở rộng               |
| ------------ | ----------------------------- | ------------------ | ---------------------------- |
| Monolithic   | Trực tiếp qua function call | Chặt chẽ         | Khó mở rộng               |
| REST API     | HTTP request                  | Request - Response | Khá tốt, nhưng đồng bộ |
| Event Driven | Event qua message broker      | Lỏng lẻo         | Rất tốt, dễ mở rộng     |

## Một số công cụ thường dùng trong Event Driven Architecture

* **Message Broker** : Kafka, RabbitMQ, Redis Streams, AWS SNS/SQS.
* **Event Processing Framework** : Spring Cloud Stream, Axon Framework.
* **Monitoring** : Kafka Manager, Elastic Stack (ELK), Prometheus + Grafana.
