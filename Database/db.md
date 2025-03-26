## ORM

- ORM (Object-Relational Mapping) là một kỹ thuật trong lập trình
- Giúp tương tác với cơ sở dữ liệu dễ dàng bằng cách map (ánh xạ) các bảng trong cơ sở dữ liệu thành các đối tượng (object) trong mã nguồn
- Lợi ích:
  - Giảm thiểu mã SQL thủ công, dễ đọc và dễ bảo trì.
  - Xây dựng quan hệ phức tạp giữa các bảng một cách tự nhiên.
  - Bảo mật hơn và tối ưu hóa hiệu năng mà không cần lo lắng về các truy vấn thấp tầng.

## ProxySQL

**ProxySQL** là một **high-performance MySQL proxy** giúp cải thiện hiệu suất, tính sẵn sàng (HA - High Availability) và khả năng mở rộng của MySQL. Nó hoạt động như một  **Load Balancer** , **Query Router** và **caching layer** giữa ứng dụng và database.

### **Tại Sao Cần ProxySQL?**

**Load Balancing** – Phân phối truy vấn đọc/ghi giữa nhiều MySQL node.

**Failover Tự Động** – Nếu một node MySQL bị lỗi, ProxySQL sẽ tự động chuyển hướng truy vấn sang node khác.

**Query Caching** – Cache kết quả truy vấn giúp giảm tải MySQL.

**Read/Write Splitting** – Gửi truy vấn `SELECT` đến  **replica** , `INSERT/UPDATE/DELETE` đến  **primary** .

**Query Routing** – Tự động định tuyến truy vấn đến node thích hợp dựa trên cấu hình.

**Firewall & Query Filtering** – Chặn các truy vấn nguy hiểm hoặc tối ưu bằng query rewrite.

### Cách ProxySQL Hoạt Động

#### **Read/Write Splitting (Tách Truy Vấn Đọc/Ghi)**

* ProxySQL tự động **gửi truy vấn đọc (`SELECT`)** đến  **replica** .
* Các truy vấn ghi (`INSERT`, `UPDATE`, `DELETE`) sẽ được gửi đến  **primary node** .

#### **Load Balancing (Cân Bằng Tải)**

* ProxySQL có thể **phân phối truy vấn** giữa nhiều MySQL node theo tỉ lệ (`weight`).
* **Ví dụ:** Nếu có 3 replica, có thể chia tải như sau:
  * **Node 1** : 40%
  * **Node 2** : 30%
  * **Node 3** : 30%

#### **Query Caching (Giảm Tải Truy Vấn Lặp Lại)**

* ProxySQL có thể **cache kết quả của các truy vấn SELECT** để giảm tải MySQL.
* Thời gian cache có thể cấu hình để phù hợp với ứng dụng.
* **Ví dụ:** Cache query trong 10 giây:

#### **Query Firewall & Query Rewrite**

* Chặn các truy vấn nguy hiểm (như `SELECT *` hoặc `DELETE FROM users`).
* **Thay đổi hoặc tối ưu query trước khi gửi đến MySQL** .

### **Kết Luận**

ProxySQL giúp  **tăng tốc MySQL** , giảm tải, hỗ trợ  **Read/Write Splitting** , Load Balancing, Query Caching.

Nếu có  **nhiều MySQL server** , ProxySQL là giải pháp lý tưởng để  **tối ưu hiệu suất** .

Dùng ProxySQL kết hợp với **MySQL Replication hoặc Cluster** để đảm bảo  **khả năng mở rộng và chịu lỗi cao** .
