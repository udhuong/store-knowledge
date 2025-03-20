# **Kafka là gì?**

![alt](https://images.viblo.asia/44c303f8-2857-4c81-9e3c-0afbec8d4372.png)

### **Định nghĩa:**

Apache Kafka là một nền tảng **stream processing** (xử lý luồng dữ liệu) phân tán, mã nguồn mở, được thiết kế để xử lý dữ liệu theo thời gian thực với hiệu suất cao

Nó chủ yếu được sử dụng để **truyền tải, lưu trữ và xử lý dữ liệu theo luồng (streaming data)** giữa các hệ thống.

### **Kiến trúc của Kafka**

Kafka có một kiến trúc dựa trên **pub-sub (publish-subscribe)** và bao gồm các thành phần chính sau:

1. **Producer** : Thành phần gửi (publish) dữ liệu vào Kafka.
2. **Broker** : Máy chủ chạy Kafka, chịu trách nhiệm lưu trữ và phân phối dữ liệu.
3. **Topic** : Dữ liệu trong Kafka được tổ chức thành các **chủ đề** (topic), giống như các kênh trong hệ thống pub-sub.
4. **Partition** : Mỗi topic có thể được chia thành nhiều **phân vùng** (partition) để tăng khả năng mở rộng và xử lý song song.
5. **Consumer** : Thành phần nhận (subscribe) dữ liệu từ Kafka.
6. **Zookeeper** : Dịch vụ dùng để quản lý cluster Kafka, theo dõi trạng thái của các broker và phân phối leader election.

### **Kafka dùng để làm gì?**

* **Streaming data pipelines** : Xây dựng các pipeline xử lý dữ liệu theo thời gian thực.
* **Message broker** : Làm trung gian giao tiếp giữa các dịch vụ trong hệ thống microservices.
* **Event sourcing** : Ghi lại mọi sự kiện trong hệ thống để phân tích hoặc khôi phục trạng thái.
* **Log aggregation** : Thu thập và phân tích log từ nhiều nguồn khác nhau.

# **Kafka Producer**

Kafka **Producer** là thành phần chịu trách nhiệm **gửi dữ liệu (messages/events)** vào Kafka  **topics** .

### **Cách hoạt động**

**Chọn Topic** : Producer quyết định gửi dữ liệu vào **topic** nào.

**Xác định Partition** :

* Kafka có thể tự động chọn partition.
* Producer cũng có thể chỉ định partition cụ thể.
* Nếu không có partition chỉ định, Kafka sử dụng **partitioner** để phân bổ dữ liệu dựa trên **key** (nếu có).

**Gửi Message** : Producer gửi dữ liệu dưới dạng **record** (key-value pair).

### **Cấu hình quan trọng**

* **Batching** : Gom nhiều message lại và gửi cùng lúc để tối ưu hiệu suất.
* **Compression** : Hỗ trợ Gzip, Snappy để giảm băng thông khi gửi dữ liệu.
* **Retries** : Tự động thử lại nếu gặp lỗi mạng hoặc lỗi server.
* **Idempotent Producer** : Đảm bảo không gửi trùng lặp message trong trường hợp lỗi.

# **Kafka Consumer**

Kafka **Consumer** là thành phần chịu trách nhiệm **nhận và xử lý dữ liệu** từ Kafka  **topics** .

### **Cách hoạt động**

1. **Đăng ký (Subscribe) vào một hoặc nhiều Topic** .
2. **Nhận Message** từ Kafka thông qua các  **Partition** .
3. **Xử lý dữ liệu** nhận được (ví dụ: lưu vào database, gửi đến service khác...).
4. **Commit Offset** để Kafka biết Consumer đã xử lý dữ liệu đến đâu.

### **Kafka Consumer Group**

* Một **Consumer Group** là một tập hợp nhiều Consumer cùng tiêu thụ dữ liệu từ một  **Topic** .
* Mỗi **Partition** của Topic chỉ được xử lý bởi **một Consumer duy nhất** trong nhóm.
* Nếu có nhiều Partition hơn Consumer, một Consumer có thể nhận nhiều Partition.

### **Cấu hình quan trọng của Kafka Consumer**

* **auto.offset.reset** :
* `"earliest"`: Đọc từ đầu topic nếu chưa có offset.
* `"latest"`: Chỉ đọc các message mới khi Consumer khởi động.
* **enable.auto.commit** :
* `true`: Kafka tự động lưu offset sau một khoảng thời gian.
* `false`: Developer tự kiểm soát offset (tốt hơn để tránh mất dữ liệu).
* **group.id** : ID của Consumer Group.

# Serialization / Deserialization

* **Serialization** : Quá trình **biến đổi dữ liệu (Object, Struct, JSON, etc.) thành byte** để có thể gửi qua mạng hoặc lưu trữ.
* **Deserialization** : Quá trình **chuyển đổi byte thành dữ liệu gốc** để có thể sử dụng trong ứng dụng.

### **Các kiểu Serialization phổ biến trong Kafka**

| **Format**   | **Ưu điểm**              | **Nhược điểm**                           |
| ------------------ | --------------------------------- | -------------------------------------------------- |
| **JSON**     | Đơn giản, dễ đọc, dễ debug | Không tối ưu hiệu suất, tốn băng thông     |
| **Avro**     | Nhẹ, hỗ trợ Schema Evolution   | Cần Schema Registry, khó debug hơn JSON         |
| **Protobuf** | Hiệu suất cao, mạnh hơn Avro  | Cần Google Protocol Buffers, khó đọc hơn JSON |

# Quản lý Offset

**Offset** là một số thứ tự đánh dấu vị trí của message trong một **Partition** của Kafka. Mỗi message trong Kafka đều có một **offset duy nhất** trong Partition của nó.

* **Kafka không xóa offset cũ** mà chỉ tiếp tục tăng dần.
* Consumer **phải lưu Offset** để biết đã xử lý đến đâu, tránh đọc lại hoặc bỏ sót message.

### **Auto Commit**

* **Kafka tự động lưu offset** sau một khoảng thời gian (`auto.commit.interval.ms`).
* **Dễ sử dụng** , không cần code quản lý offset.
* **Rủi ro mất dữ liệu** nếu Consumer bị crash  **trước khi xử lý xong message** .
* Kafka sẽ tự động commit offset sau mỗi  **5 giây** .
* Nếu Consumer **chưa xử lý xong** message mà bị crash, Kafka **vẫn coi như đã xử lý** và Consumer có thể bỏ lỡ dữ liệu.

### **Manual Commit**

* Consumer **tự quyết định khi nào commit offset** sau khi **chắc chắn đã xử lý xong** message.
* **An toàn hơn** vì tránh mất dữ liệu hoặc đọc lại dữ liệu cũ.
* **Cần code thêm** để quản lý offset.

Cách 1: Synchronous Commit (Commit đồng bộ)

* **Consumer.commitSync()** gửi request commit **ngay lập tức** và đợi Kafka xác nhận.
* **An toàn** nhưng **chậm hơn** do phải đợi phản hồi từ Kafka.

Cách 2: Asynchronous Commit (Commit bất đồng bộ)

* **Consumer.commitAsync()** gửi request commit  **nhưng không cần đợi Kafka xác nhận** .
* **Nhanh hơn** nhưng có thể mất dữ liệu nếu Consumer bị crash trước khi Kafka commit offset.

Cách 3: Commit theo từng Message (Commit Offset cụ thể)

* **Tự commit offset của từng message** thay vì commit toàn bộ batch.
* **Đảm bảo dữ liệu không bị mất** nếu Consumer crash.

### **Khi nào dùng Auto Commit vs Manual Commit?**

**Dùng Auto Commit khi:**

* Không quá quan trọng nếu mất một ít dữ liệu.
* Xử lý nhanh, không cần lưu offset chính xác.

**Dùng Manual Commit khi:**

* Cần đảm bảo  **không mất dữ liệu** .
* Xử lý message tốn thời gian (lưu DB, gọi API...).
* Hệ thống có thể gặp lỗi hoặc restart thường xuyên.

### Reset Offset

**Reset Offset** giúp Consumer đọc lại từ vị trí mong muốn trong topic, tránh mất dữ liệu hoặc đọc dữ liệu cũ.

Reset Offset bằng CLI:

* Reset về earliest (Đọc từ đầu topic)
* Reset về latest (Chỉ đọc message mới)
* Reset về offset cụ thể
* Reset về timestamp cụ thể (Dựa vào thời gian gửi message)

# Stateful & Stateless Processing

Trong **Kafka Streams** và hệ thống xử lý dữ liệu nói chung, có hai loại xử lý chính:

1. **Stateless Processing** – Xử lý không lưu trạng thái
2. **Stateful Processing** – Xử lý có lưu trạng thái

### **Khi nào dùng Stateful vs Stateless?**

✅ **Dùng Stateless Processing khi:**

* Không cần lưu trạng thái giữa các message.
* Xử lý nhanh, nhẹ, ít tài nguyên.
* Các phép toán chỉ phụ thuộc vào từng message riêng lẻ.

✅ **Dùng Stateful Processing khi:**

* Cần nhớ trạng thái giữa các message (tính tổng, đếm, join...).
* Cần cửa sổ thời gian (windowing).
* Cần join dữ liệu từ nhiều topic.

# Tối ưu & Triển khai Kafka

## Kafka Cluster

Kafka Cluster là một hệ thống phân tán gồm **nhiều broker Kafka làm việc cùng nhau** để quản lý và xử lý dữ liệu một cách hiệu quả. Việc sử dụng **multi-broker setup** giúp Kafka  **tăng khả năng mở rộng (scalability)** , **đảm bảo tính sẵn sàng (high availability)** và  **chống lỗi (fault tolerance)** .

### **Broker (Máy chủ Kafka)**

* **Broker** là thành phần chịu trách nhiệm lưu trữ dữ liệu và xử lý yêu cầu từ Producer & Consumer.
* Mỗi broker có một **ID duy nhất** để phân biệt trong cluster.
* Khi có nhiều broker, dữ liệu sẽ được **chia nhỏ (partitioning) và sao chép (replication)** giữa các broker.

### **Topic & Partitions**

* **Topic** là nơi chứa dữ liệu, giống như một danh mục.
* **Partition** là cách Kafka chia nhỏ dữ liệu trong topic và phân phối chúng giữa các broker.
* Giúp Kafka xử lý song song, tăng tốc độ xử lý.
* Nếu một broker bị lỗi, dữ liệu vẫn được truy xuất từ các broker khác.

### **Replication (Sao chép dữ liệu)**

Replication giúp đảm bảo dữ liệu **không bị mất** khi một broker bị lỗi. Mỗi partition có một **leader** và nhiều **replica** trên các broker khác.

### **Zookeeper**

Zookeeper là thành phần  **quản lý Kafka Cluster** , bao gồm:

* Theo dõi trạng thái của broker.
* Quản lý **leader election** (chọn broker nào làm leader khi broker cũ bị lỗi).
* Lưu trữ metadata (thông tin về topic, partitions, leader...).

# Consumer Group

### **Định nghĩa**

* **Consumer Group** là một nhóm gồm **nhiều consumer** cùng nhau **đọc dữ liệu từ một topic** trong Kafka.
* Mỗi **consumer trong group** sẽ đọc một phần dữ liệu từ các **partition** của topic, giúp **xử lý song song** và  **tăng hiệu suất tiêu thụ dữ liệu** .
* **Mỗi message trong một partition chỉ được xử lý bởi một consumer duy nhất trong group** , giúp đảm bảo  **tính cân bằng tải (load balancing)** .

### Cách hoạt động

**📌 Nguyên tắc phân phối dữ liệu trong Consumer Group:**

* **Mỗi partition chỉ được đọc bởi một consumer trong nhóm** (tránh việc cùng một message bị xử lý hai lần).
* **Một consumer có thể đọc nhiều partition** nếu số consumer nhỏ hơn số partition.

📌 **Ví dụ:**

* **Topic `orders` có 3 partitions (`P0`, `P1`, `P2`)** .
* **Consumer Group `order-group` có 3 consumer (`C1`, `C2`, `C3`)** .
* Kafka sẽ phân phối partitions như sau:

| **Partition** | **Consumer** |
| ------------------- | ------------------ |
| `P0`              | `C1`             |
| `P1`              | `C2`             |
| `P2`              | `C3`             |

👉  **Mỗi consumer sẽ đọc dữ liệu từ một partition duy nhất** .

📌 **Trường hợp Consumer ít hơn Partition**

* **Topic `orders` có 3 partitions (`P0`, `P1`, `P2`)** .
* **Consumer Group chỉ có 2 consumer (`C1`, `C2`)** .

| **Partition** | **Consumer**                        |
| ------------------- | ----------------------------------------- |
| `P0`              | `C1`                                    |
| `P1`              | `C2`                                    |
| `P2`              | `C1`(có thể xử lý nhiều partition) |

👉 **Consumer C1 phải xử lý 2 partitions (P0, P2), còn C2 chỉ xử lý 1 partition (P1).**

📌 **Trường hợp Consumer nhiều hơn Partition**

* **Topic `orders` có 3 partitions (`P0`, `P1`, `P2`)** .
* **Consumer Group có 4 consumer (`C1`, `C2`, `C3`, `C4`)** .

| **Partition** | **Consumer**                                |
| ------------------- | ------------------------------------------------- |
| `P0`              | `C1`                                            |
| `P1`              | `C2`                                            |
| `P2`              | `C3`                                            |
|                     | **`C4` Không nhận dữ liệu (vì dư)** |

👉 **Consumer C4 sẽ không nhận dữ liệu, vì số partition ít hơn số consumer.**

### Consumer Group ID

Mỗi consumer phải có một **Consumer Group ID** để Kafka biết nó thuộc nhóm nào.

📌 **Ví dụ:** Consumer Group `order-group` đọc từ topic `orders`

👉 **Tất cả consumer trong group `order-group` sẽ cùng nhau tiêu thụ dữ liệu từ topic `orders`.**

### **So sánh Consumer Group vs Standalone Consumer**

| **Tiêu chí**           | **Consumer Group**                                               | **Standalone Consumer**                      |
| ------------------------------ | ---------------------------------------------------------------------- | -------------------------------------------------- |
| **Khả năng mở rộng** | Có thể thêm nhiều consumer để xử lý dữ liệu song song.       | Chỉ có một consumer đọc dữ liệu.            |
| **Tốc độ xử lý**    | Nhanh hơn do phân phối dữ liệu giữa nhiều consumer.             | Chậm hơn vì chỉ có một consumer.             |
| **Cân bằng tải**      | Có, Kafka tự động phân chia partitions.                           | Không có, vì chỉ có một consumer.            |
| **Chống lỗi**          | Nếu một consumer chết, Kafka tự động phân chia lại partitions. | Nếu consumer chết, không có cơ chế failover. |



# Giảm lag message

Khi Kafka bị **lag message** (LAG cao), tức là consumer không kịp đọc dữ liệu từ topic, dẫn đến backlog ngày càng lớn. Dưới đây là **các cách giảm message lag** trong Kafka:

## **Tăng số lượng Consumer trong Consumer Group**

Nếu số lượng partitions đủ lớn, **tăng thêm consumer** giúp chia tải đọc dữ liệu nhanh hơn.

📌 **Ví dụ:**

* **Topic `orders` có 6 partitions (`P0` → `P5`)** .
* **Consumer Group `order-group` chỉ có 3 consumer (`C1`, `C2`, `C3`)** .
* Kafka sẽ phân phối như sau:

| **Partition** | **Consumer** |
| ------------------- | ------------------ |
| `P0`              | `C1`             |
| `P1`              | `C2`             |
| `P2`              | `C3`             |
| `P3`              | `C1`             |
| `P4`              | `C2`             |
| `P5`              | `C3`             |

👉 **Giải pháp:**

* Thêm **3 consumer nữa (`C4`, `C5`, `C6`)** để mỗi consumer chỉ đọc  **1 partition** , giúp tăng tốc độ xử lý.

⏳ **Lưu ý:**

* **Nếu số partition ít hơn số consumer** , một số consumer sẽ không hoạt động.
* **Nếu số partition quá ít** , hãy tăng số partition lên.

## **Tăng số Partition của Topic**

Nếu consumer đã đạt tối đa công suất, bạn có thể **tăng số partition** để chia nhỏ dữ liệu.

⏳ **Lưu ý:**

* Kafka **không tự động cân bằng lại dữ liệu cũ** trên partitions mới.
* **Cần có nhiều consumer hơn** để tận dụng partition mới.

## **Giảm Load cho Consumer (Tối ưu Consumer)**

Nếu consumer xử lý chậm, hãy tối ưu lại chúng:

✔  **Tăng batch size** : Đọc nhiều message hơn mỗi lần poll

✔ **Sử dụng multi-threading** để xử lý message song song

✔  **Giảm thời gian xử lý từng message** , tránh I/O blocking

✔ **Tối ưu database query** nếu consumer ghi dữ liệu vào DB

## **Tăng Poll Interval và Fetch Size (Giảm Network Overhead)**

Consumer có thể bị lag nếu nó  **poll dữ liệu quá ít hoặc quá chậm** .

📌 **Tăng `fetch.min.bytes` để lấy nhiều dữ liệu hơn mỗi lần poll:**

📌 **Tăng `max.poll.records` để đọc nhiều message hơn mỗi lần:**

📌 **Giảm `session.timeout.ms` để phát hiện consumer bị chết nhanh hơn:**

⏳ **Lưu ý:**

* Tăng giá trị quá cao có thể khiến consumer bị quá tải.
* Cần kiểm tra thử nghiệm với hệ thống thực tế.

## **Dùng Kafka Streams hoặc Batch Processing Thay vì Realtime Processing**

Nếu hệ thống cần  **xử lý nhiều dữ liệu** , nhưng không cần  **realtime 100%** , bạn có thể **dùng Kafka Streams hoặc Batch Processing** thay vì xử lý từng message riêng lẻ.

📌 **Ví dụ dùng Kafka Streams để nhóm dữ liệu theo thời gian:**

👉 **Lợi ích:**

* Giảm số lần consumer phải đọc dữ liệu.
* Tối ưu hiệu suất xử lý.

## **Bật Consumer Parallelism (Xử lý song song)**

Consumer có thể  **xử lý message tuần tự hoặc song song** . Nếu xử lý tuần tự, nó có thể bị chậm.

📌 **Dùng Executor Service để xử lý song song trong Java:**

👉 **Lợi ích:**

* Giảm thời gian xử lý từng message.
* Tăng throughput của consumer.

## **Giảm Lag Bằng Cách Reset Offset**

Nếu message backlog quá lớn, có thể **reset offset** để đọc dữ liệu mới nhất thay vì xử lý backlog.

## **Tóm tắt Cách Giảm Lag Message trong Kafka**

| **Cách**                                     | **Lợi ích**                                        |
| --------------------------------------------------- | ---------------------------------------------------------- |
| **1. Thêm Consumer vào Consumer Group**     | Chia tải đọc dữ liệu nhanh hơn (nếu partition đủ) |
| **2. Tăng số Partition**                    | Giúp phân phối dữ liệu tốt hơn                      |
| **3. Tối ưu Consumer**                      | Tăng batch size, multi-threading, tối ưu DB             |
| **4. Tăng Fetch Size & Poll Interval**       | Giảm network overhead                                     |
| **5. Dùng Kafka Streams / Batch Processing** | Giảm tải cho consumer                                    |
| **6. Bật Multi-threaded Consumer**           | Tăng tốc xử lý message                                 |
| **7. Tối ưu Kafka Broker**                  | Tăng throughput Kafka                                     |
| **8. Reset Offset nếu cần**                 | Bỏ backlog để giảm lag                                 |



# Giữ message được consume theo thứ tự

Ảnh hưởng của các cách tối ưu Kafka đến Message Order

| **Cách tối ưu**                       | **Ảnh hưởng đến thứ tự**                                                                                | **Giải pháp giữ thứ tự**                         |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Thêm consumer vào Consumer Group**   | ✅**Ảnh hưởng nếu có nhiều partition**(vì Kafka **không đảm bảo thứ tự giữa partitions** ) | 🛠**Giữ cùng key trên một partition**             |
| **Tăng số partition**                  | ✅**Ảnh hưởng vì một key có thể bị chia ra nhiều partition khác nhau**                               | 🛠**Dùng cùng partition key (sticky partitioning)** |
| **Dùng Multi-threading trong Consumer** | ✅**Ảnh hưởng nếu message không xử lý theo thứ tự**                                                   | 🛠**Xử lý tuần tự trong từng partition**         |
| **Batch Processing / Kafka Streams**     | ✅**Có thể ảnh hưởng nếu message được nhóm lại**                                                    | 🛠**Giữ dữ liệu cùng key trong một nhóm**       |
| **Reset Offset (latest, earliest)**      | ✅**Ảnh hưởng nếu reset về latest (bỏ message cũ)**                                                     | 🛠**Dùng earliest nếu cần đọc từ đầu**        |

Có 1 cách khác là tăng pod với cấu hình tương tự pod đang chạy, nhưng cần đảm bảo số partition >=  số consumer

## **Kết luận**

✔  **Kafka đảm bảo thứ tự trong cùng một partition** , nhưng không đảm bảo giữa nhiều partition.

✔ **Nếu cần giữ thứ tự, luôn sử dụng cùng partition key để message của một entity luôn vào cùng một partition.**

✔ **Không dùng multi-threading nếu cần xử lý message theo thứ tự.**

✔ **Nếu tăng partition, cần đảm bảo dữ liệu cùng key luôn vào cùng partition.**

✔ **Chọn reset offset cẩn thận để tránh mất dữ liệu giữa chừng.**


# Filter Message

#### **Dùng Kafka Headers để Filter Message (Consumer-side Filtering)**

 **Ý tưởng** : Khi producer gửi message, thêm một **header** (`group_allowed=a`). Consumer kiểm tra **header** trước khi xử lý.

#### **Sử dụng 2 Topic Riêng Biệt (Topic-based Filtering)**

 **Ý tưởng** : Producer gửi message vào 2 topic khác nhau, chỉ **Consumer Group A** đăng ký topic đó.

#### **Dùng Kafka ACLs (Access Control List)**

 **Ý tưởng** : Kafka ACLs có thể chặn Consumer Group B đọc từ topic.

#### **Producer gửi message với Key (Partition-based Filtering)**

 **Ý tưởng** : Producer chỉ gửi message vào  **partition mà Group A có thể đọc** , nhưng Group B không có consumer cho partition đó.


# Filter trong kafka

## 1. **Filtering ở Producer (Tránh gửi dữ liệu không cần thiết)**

* Producer có thể quyết định chỉ gửi những message phù hợp với tiêu chí nhất định vào Kafka topic.
* Ví dụ, chỉ gửi các giao dịch có giá trị trên 1000 USD.

## 2. **Filtering ở Consumer (Bỏ qua message không cần thiết)**

Consumer có thể đọc tất cả message từ topic nhưng chỉ xử lý những message thỏa mãn điều kiện.

Nhược điểm:  **Vẫn tốn băng thông vì tải toàn bộ dữ liệu về consumer** .

## 3. **Sử dụng Kafka Streams để Filter Message Trước Khi Consumer Nhận**

Kafka Streams cho phép bạn xử lý dữ liệu ngay tại cluster Kafka trước khi gửi đến consumer.

**Ưu điểm:** Chỉ gửi dữ liệu cần thiết đến consumer, giảm tải network.

## 4. **Sử dụng Kafka Consumer Group với Filtering**

* Bạn có thể tạo **consumer group** chuyên xử lý một loại dữ liệu nhất định.
* Ví dụ: Một consumer group chỉ xử lý giao dịch trên 1000 USD.
* **Ưu điểm:** Mỗi nhóm consumer có thể xử lý dữ liệu khác nhau.

## Chọn phương pháp nào?

* Nếu bạn muốn giảm tải  **Kafka Cluster** , hãy filter ngay ở  **Producer** .
* Nếu bạn cần xử lý linh hoạt hơn, hãy filter ở  **Kafka Streams** .
* Nếu bạn chỉ muốn filter ở client side, thì **Consumer Filtering** có thể đủ.
