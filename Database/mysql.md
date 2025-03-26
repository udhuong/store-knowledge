## **Các kiểu dữ liệu**

MySQL cung cấp nhiều loại dữ liệu khác nhau để lưu trữ thông tin hiệu quả. Chúng được chia thành các nhóm chính sau:

1. **Kiểu số (Numeric Data Types)**
2. **Kiểu chuỗi (String Data Types)**
3. **Kiểu ngày và thời gian (Date & Time Data Types)**
4. **Kiểu JSON và đặc biệt khác**

### **1. Kiểu số (Numeric Data Types)**

Dùng để lưu trữ các số nguyên, số thực và số thập phân.

| Kiểu dữ liệu             | Kích thước | Phạm vi                                                   | Mô tả                               |
| --------------------------- | ------------- | ---------------------------------------------------------- | ------------------------------------- |
| **TINYINT**           | 1 byte        | -128 đến 127 (SIGNED) / 0 đến 255 (UNSIGNED)           | Lưu trữ số rất nhỏ               |
| **SMALLINT**          | 2 byte        | -32,768 đến 32,767                                       | Lưu trữ số nhỏ                    |
| **MEDIUMINT**         | 3 byte        | -8,388,608 đến 8,388,607                                 | Lưu trữ số trung bình             |
| **INT (INTEGER)**     | 4 byte        | -2,147,483,648 đến 2,147,483,647                         | Lưu trữ số nguyên tiêu chuẩn    |
| **BIGINT**            | 8 byte        | -9,223,372,036,854,775,808 đến 9,223,372,036,854,775,807 | Lưu trữ số nguyên lớn            |
| **FLOAT**             | 4 byte        | ~3.402823466E+38                                           | Số thực, có độ chính xác thấp |
| **DOUBLE (REAL)**     | 8 byte        | ~1.7976931348623157E+308                                   | Số thực, có độ chính xác cao   |
| **DECIMAL (NUMERIC)** | Tùy chọn    | Định dạng số thập phân chính xác                   |                                       |

**Khi nào dùng số nguyên hay số thực?**

* Dùng **INT, BIGINT** nếu không cần số thập phân.
* Dùng **FLOAT, DOUBLE** nếu cần số thập phân nhưng không yêu cầu độ chính xác tuyệt đối (ví dụ: tính toán khoa học).
* Dùng **DECIMAL** nếu cần độ chính xác tuyệt đối (ví dụ: lưu số tiền).

---

### **2. Kiểu chuỗi (String Data Types)**

Dùng để lưu trữ văn bản, ký tự và dữ liệu nhị phân.

| Kiểu dữ liệu      | Kích thước      | Mô tả                                                                                          |
| -------------------- | ------------------ | ------------------------------------------------------------------------------------------------ |
| **CHAR(n)**    | 1 - 255 ký tự    | Lưu chuỗi có độ dài cố định, nhanh hơn `VARCHAR`                                     |
| **VARCHAR(n)** | 1 - 65,535 ký tự | Lưu chuỗi có độ dài thay đổi, tiết kiệm bộ nhớ                                       |
| **TEXT**       | 1 - 4GB            | Lưu trữ văn bản lớn (có các biến thể `TINYTEXT`,`TEXT`,`MEDIUMTEXT`,`LONGTEXT`) |
| **BLOB**       | 1 - 4GB            | Lưu dữ liệu nhị phân (hình ảnh, âm thanh, video)                                         |
| **ENUM**       | 1 - 2 byte         | Lưu tập hợp các giá trị cố định (vd:`ENUM('male', 'female')`)                         |
| **SET**        | 1 - 8 byte         | Lưu trữ danh sách giá trị, có thể chọn nhiều giá trị cùng lúc                       |

**Khi nào dùng CHAR, VARCHAR hay TEXT?**

* **CHAR** : Dùng khi chuỗi có độ dài cố định (vd: mã quốc gia 2 ký tự `US`, `VN`).
* **VARCHAR** : Dùng khi chuỗi có độ dài thay đổi (vd: tên khách hàng).
* **TEXT** : Dùng khi lưu trữ văn bản dài (vd: bài viết blog).

### **3. Kiểu ngày và thời gian (Date & Time Data Types)**

Dùng để lưu trữ ngày, giờ.

| Kiểu dữ liệu     | Kích thước | Phạm vi                                      | Mô tả                               |
| ------------------- | ------------- | --------------------------------------------- | ------------------------------------- |
| **DATE**      | 3 byte        | 1000-01-01 đến 9999-12-31                   | Lưu ngày (YYYY-MM-DD)               |
| **DATETIME**  | 8 byte        | 1000-01-01 00:00:00 đến 9999-12-31 23:59:59 | Lưu ngày + giờ                     |
| **TIMESTAMP** | 4 byte        | 1970-01-01 00:00:01 đến 2038-01-19 03:14:07 | Lưu ngày + giờ theo Unix Timestamp |
| **TIME**      | 3 byte        | -838:59:59 đến 838:59:59                    | Lưu thời gian (giờ, phút, giây)  |
| **YEAR**      | 1 byte        | 1901 đến 2155                               | Lưu năm                             |

**Khi nào dùng DATETIME hay TIMESTAMP?**

* **DATETIME** : Dùng khi cần lưu trữ mốc thời gian độc lập với múi giờ.
* **TIMESTAMP** : Dùng khi cần mốc thời gian có thể thay đổi theo múi giờ (vd: thời gian tạo bản ghi).

### **4. Kiểu JSON và đặc biệt khác**

Dùng để lưu trữ dữ liệu đặc biệt.

| Kiểu dữ liệu    | Mô tả                                                                      |
| ------------------ | ---------------------------------------------------------------------------- |
| **JSON**     | Lưu trữ dữ liệu JSON (vd:`{"name": "John", "age": 30}`)                |
| **BIT**      | Lưu trữ dữ liệu nhị phân (vd:`BIT(1)`cho giá trị `0`hoặc `1`) |
| **GEOMETRY** | Lưu dữ liệu không gian (GIS - Geographic Information System)             |

**Khi nào dùng JSON?**

* Khi cần lưu trữ dữ liệu động, linh hoạt (vd: thuộc tính sản phẩm với các tùy chọn khác nhau).
* Khi cần truy vấn dữ liệu JSON trực tiếp trong MySQL.

### **Tóm tắt cách chọn kiểu dữ liệu MySQL**

| Loại dữ liệu          | Khi nào dùng?                       | Ví dụ                |
| ------------------------ | ------------------------------------- | ---------------------- |
| **INT, BIGINT**    | Lưu số nguyên                      | ID, số lượng        |
| **FLOAT, DOUBLE**  | Số thực có thể không chính xác | Tính toán khoa học  |
| **DECIMAL**        | Số thực có độ chính xác cao    | Giá tiền             |
| **VARCHAR**        | Chuỗi có độ dài thay đổi       | Tên khách hàng      |
| **TEXT**           | Văn bản dài                        | Mô tả sản phẩm     |
| **DATE, DATETIME** | Lưu ngày, giờ                      | Ngày tạo đơn hàng |
| **TIMESTAMP**      | Lưu thời gian với múi giờ        | Log hệ thống         |
| **JSON**           | Lưu trữ dữ liệu động            | Cấu hình sản phẩm  |

**Lưu ý quan trọng:**

* **Dùng kiểu dữ liệu nhỏ nhất có thể** để tiết kiệm bộ nhớ.
* **Đánh index trên các cột thường xuyên JOIN hoặc WHERE** để tối ưu hiệu suất.
* **Tránh dùng TEXT nếu không cần thiết** , vì nó không thể đánh index tốt như VARCHAR.

## **Các kiểu lưu trữ dữ liệu (Storage Engines)**

| **Storage Engine**      | **Hỗ trợ Transactions** | **Khóa cấp hàng** | **Tốc độ**               | **Ứng dụng phù hợp**                  |
| ----------------------------- | ------------------------------- | -------------------------- | --------------------------------- | ----------------------------------------------- |
| **InnoDB**(Mặc định) | ✅                              | ✅ Có                     | Trung bình                       | Hệ thống giao dịch, thương mại điện tử |
| **MyISAM**              | ❌                              | ❌                         | Nhanh với SELECT                 | Hệ thống đọc nhiều, ít ghi                |
| **MEMORY**              | ❌                              | ❌                         | Rất nhanh                        | Cache dữ liệu tạm thời                      |
| **ARCHIVE**             | ❌                              | ❌                         | Chậm                             | Lưu trữ log, dữ liệu lịch sử              |
| **CSV**                 | ❌                              | ❌                         | Chậm                             | Xuất dữ liệu ra file CSV                     |
| **NDB Cluster**         | ✅                              | ✅                         | Trung bình                       | Hệ thống phân tán lớn                      |
| **BLACKHOLE**           | ❌                              | ❌                         | Nhanh (Vì không lưu dữ liệu) | Log query, kiểm thử                           |

### **InnoDB (Mặc định)**

**Đặc điểm:**

* **Hỗ trợ ACID (Atomicity, Consistency, Isolation, Durability)** → Bảo toàn tính toàn vẹn dữ liệu.
* **Hỗ trợ khóa cấp hàng (Row-level locking)** → Tối ưu hiệu suất khi nhiều người dùng truy cập cùng lúc.
* **Hỗ trợ giao dịch (Transactions) với COMMIT và ROLLBACK.**
* **Hỗ trợ FOREIGN KEY (Khóa ngoại)** → Đảm bảo ràng buộc dữ liệu giữa các bảng.
* **Sử dụng chỉ mục B-Tree và Clustered Index** → Tìm kiếm nhanh.

**Khi nào dùng InnoDB?**

* Khi cần đảm bảo **toàn vẹn dữ liệu** và hỗ trợ giao dịch.
* Hệ thống  **ERP, tài chính, thương mại điện tử** .

### MyISAM (Cũ, ít dùng)

**Đặc điểm:**

* **Nhanh hơn InnoDB khi chỉ đọc dữ liệu** → Phù hợp với hệ thống báo cáo, đọc nhiều hơn ghi.
* **Hỗ trợ khóa cấp bảng (Table-level locking)** → Không phù hợp với hệ thống có nhiều thao tác ghi đồng thời.
* **Không hỗ trợ giao dịch (Transactions).**
* **Không hỗ trợ FOREIGN KEY (Khóa ngoại).**
* **Lưu trữ dữ liệu theo dạng MyISAM Table (.MYD, .MYI, .FRM).**

**Khi nào dùng MyISAM?**

* Hệ thống **chỉ đọc dữ liệu nhiều, ít cập nhật** (Blog, hệ thống log, analytics).

### **MEMORY (HEAP)**

**Đặc điểm:**

* **Lưu trữ dữ liệu trong RAM** → Truy vấn cực nhanh.
* **Không lưu dữ liệu vào ổ cứng** → Mất dữ liệu khi restart MySQL.
* **Hỗ trợ khóa bảng (Table-level locking).**
* **Chỉ phù hợp với dữ liệu tạm thời, không hỗ trợ BLOB/TEXT.**

**Khi nào dùng MEMORY?**

* Cache dữ liệu tạm thời, bảng lookup nhanh (Session, leaderboard).

## **Index**

* **B-TREE INDEX** : Hữu ích cho so sánh (`=`, `<`, `>`, `BETWEEN`).
* **HASH INDEX** : Dùng trong trường hợp chỉ có `=` (không hỗ trợ `<`, `>`).
* **COVERING INDEX** : Nếu truy vấn chỉ cần dữ liệu từ INDEX, MySQL không cần đọc dữ liệu từ bảng chính.

### Giải thích các cột quan trọng trong EXPLAIN

| **Cột**          | **Ý nghĩa**                                                   |
| ----------------------- | --------------------------------------------------------------------- |
| **id**            | ID của truy vấn, số lớn hơn chạy trước (nếu có subquery).   |
| **select_type**   | Loại truy vấn (`SIMPLE`,`SUBQUERY`,`DERIVED`,`UNION`).      |
| **table**         | Bảng đang được truy vấn.                                        |
| **partitions**    | Partition nào đang được quét (nếu có sử dụng partitioning). |
| **type**          | Kiểu truy vấn, quan trọng nhất để tối ưu.                     |
| **possible_keys** | Các index có thể được dùng.                                    |
| **key**           | Index thực sự được dùng.                                        |
| **key_len**       | Độ dài của index đang sử dụng.                                 |
| **ref**           | Cách MySQL so sánh cột với giá trị của index.                  |
| **rows**          | Số dòng MySQL dự đoán sẽ quét.                                 |
| **filtered**      | Tỉ lệ (%) hàng còn lại sau khi áp dụng `WHERE`.              |
| **Extra**         | Thông tin bổ sung về truy vấn.                                    |

#### **Các giá trị quan trọng**

##### **`type` - Kiểu truy vấn (Quan trọng nhất!)**

Phản ánh hiệu suất truy vấn, theo thứ tự từ  **tốt → xấu** :

| **type**   | **Ý nghĩa**                                                          | **Tốt/Xấu** |
| ---------------- | ---------------------------------------------------------------------------- | ------------------- |
| **system** | Truy vấn 1 dòng duy nhất, nhanh nhất.                                    | Rất tốt           |
| **const**  | Đọc 1 dòng duy nhất (do `WHERE`trên `PRIMARY KEY`hoặc `UNIQUE`). | Rất tốt           |
| **eq_ref** | Dùng với `JOIN`trên `PRIMARY KEY`hoặc `UNIQUE`.                    | Tốt                |
| **ref**    | Dùng với `INDEX`, nhưng không duy nhất.                               | Tốt                |
| **range**  | Dùng index cho các điều kiện `BETWEEN`,`<`,`>`,`IN`.            | Tạm được        |
| **index**  | Duyệt toàn bộ index, không tốt nếu quá nhiều dữ liệu.              | Xấu                |
| **ALL**    | Quét toàn bộ bảng, rất chậm.                                           | Rất xấu           |

Nếu thấy **`ALL`** hoặc  **`index`** , cần **tạo INDEX** hoặc tối ưu truy vấn.

##### **`key` - Index đang sử dụng**

* Nếu `NULL` → MySQL **không dùng index** (cần tối ưu).
* Nếu có giá trị → MySQL  **đang dùng index** .

##### **`rows` - Số dòng MySQL dự đoán sẽ quét**

* Giá trị  **càng nhỏ càng tốt** .
* Nếu quét hàng triệu dòng, cần tối ưu bằng  **INDEX** .

##### **`Extra` - Thông tin bổ sung**

Một số giá trị  **xấu cần tránh** :

| **Giá trị**       | **Ý nghĩa**                                            | **Tối ưu thế nào?**                     |
| ------------------------- | -------------------------------------------------------------- | ------------------------------------------------- |
| **Using filesort**  | MySQL phải sắp xếp dữ liệu riêng                         | Thêm `INDEX`phù hợp                          |
| **Using temporary** | MySQL tạo bảng tạm                                          | Dùng `INDEX`để tránh `GROUP BY`tạm thời |
| **Using where**     | Dữ liệu lọc trong `WHERE`, có thể chưa dùng `INDEX` | Kiểm tra `INDEX`                               |

**Tốt nhất nên có `Using index`** , nghĩa là MySQL đọc dữ liệu trực tiếp từ INDEX, không cần truy cập bảng.

## **Các loại JOIN**

| JOIN Type                 | Mô tả                                                                    |
| ------------------------- | -------------------------------------------------------------------------- |
| **INNER JOIN**      | Chỉ lấy bản ghi khớp ở cả hai bảng.                                 |
| **LEFT JOIN**       | Lấy toàn bộ từ bảng trái + khớp từ bảng phải.                    |
| **RIGHT JOIN**      | Lấy toàn bộ từ bảng phải + khớp từ bảng trái.                    |
| **FULL OUTER JOIN** | Lấy tất cả bản ghi từ cả hai bảng, nếu không khớp thì `NULL`. |
| **CROSS JOIN**      | Kết hợp tất cả bản ghi của hai bảng (Cartesian Product).            |

### **INNER JOIN - Kết hợp các bản ghi khớp nhau**

* MySQL duyệt từng dòng trong  **bảng A** .
* Với mỗi dòng của  **bảng A** , nó sẽ tìm các dòng phù hợp trong **bảng B** theo điều kiện `ON`.
* Chỉ những dòng có **kết quả khớp nhau** giữa hai bảng mới được lấy.

### **LEFT JOIN - Giữ toàn bộ dữ liệu từ bảng trái**

* MySQL duyệt từng dòng của  **bảng A** .
* Nó tìm dòng khớp trong  **bảng B** .
* Nếu có kết quả, nó lấy dữ liệu từ cả hai bảng.
* Nếu không có kết quả,  **cột từ bảng B sẽ là `NULL`** .

### **RIGHT JOIN - Giữ toàn bộ dữ liệu từ bảng phải**

* MySQL duyệt từng dòng của  **bảng B** .
* Nó tìm dòng khớp trong  **bảng A** .
* Nếu có kết quả, nó lấy dữ liệu từ cả hai bảng.
* Nếu không có kết quả,  **cột từ bảng A sẽ là `NULL`** .

## **FULL OUTER JOIN - Kết hợp cả hai bảng (MySQL không hỗ trợ trực tiếp)**

* Lấy toàn bộ dữ liệu từ **bảng A** (giống `LEFT JOIN`).
* Lấy toàn bộ dữ liệu từ **bảng B** (giống `RIGHT JOIN`).
* Nếu có bản ghi khớp → Kết hợp dữ liệu.
* Nếu không khớp → Hiển thị `NULL`.

**MySQL không hỗ trợ FULL OUTER JOIN trực tiếp** nhưng có thể mô phỏng bằng `UNION`

### **CROSS JOIN - Kết hợp tất cả bản ghi của hai bảng**

* MySQL **không cần điều kiện JOIN** (`ON`).
* Kết hợp **mỗi dòng của bảng A với mỗi dòng của bảng B** ( **Cartesian Product** ).
* Nếu bảng A có `m` dòng, bảng B có `n` dòng, kết quả sẽ có  **`m × n` dòng** .

### **Nên JOIN theo hướng nào để tối ưu?**

* 2 cột join ON cần được index
* **Nếu có điều kiện `WHERE`, nên lọc trước trên bảng nhỏ (`100k records`) rồi JOIN với bảng lớn (`5 triệu records`).**
* **Vì sao?**

  * Nếu lọc trên bảng lớn (`5 triệu records`), kết quả JOIN vẫn có thể rất lớn.
  * Nếu lọc trên bảng nhỏ trước (`100k records`), số bản ghi tham gia JOIN sẽ ít hơn → giảm tải JOIN.

### **Cách tối ưu JOIN bảng lớn**

| Cách tối ưu                                        | Mô tả                                                       |
| ----------------------------------------------------- | ------------------------------------------------------------- |
| **Chỉ SELECT cột cần thiết**                | Tránh `SELECT *`, chỉ lấy dữ liệu cần dùng           |
| **INDEX trên cột JOIN**                       | Tạo INDEX trên các cột dùng trong JOIN để tăng tốc   |
| **Dùng `INNER JOIN`nếu có thể**           | Tránh `LEFT JOIN`nếu không cần thiết                   |
| **Lọc dữ liệu trước khi JOIN (`WHERE`)** | Giảm số dòng JOIN giúp truy vấn nhanh hơn               |
| **Dùng `EXPLAIN`để kiểm tra**             | Xem kế hoạch thực thi truy vấn để phát hiện vấn đề |
| **Sử dụng `INDEX COVERING`**                | Đọc dữ liệu từ INDEX thay vì truy cập bảng            |
| **Dùng `PARTITION`nếu dữ liệu rất lớn** | Chia nhỏ bảng để truy vấn nhanh hơn                     |

## UNION

`UNION` trong MySQL được sử dụng để kết hợp kết quả của **hai hoặc nhiều truy vấn SELECT** thành một tập kết quả duy nhất.

**Đặc điểm quan trọng:**

* Số cột của các truy vấn phải **giống nhau** (cả số lượng và kiểu dữ liệu).
* **Mặc định loại bỏ dữ liệu trùng** (nếu không muốn loại bỏ, dùng `UNION ALL`).
* Cột trong `ORDER BY` phải có trong tập kết quả cuối cùng.

```sql
SELECT column1, column2 FROM table1
UNION ALL
SELECT column1, column2 FROM table2;
```

### **Khi nào dùng UNION thay vì JOIN?**

| **UNION**                                                                                        | **JOIN**                                                                    |
| ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| Kết hợp kết quả từ nhiều bảng theo hàng dọc (dữ liệu từ nhiều bảng có cùng cấu trúc) | Kết hợp dữ liệu giữa các bảng theo hàng ngang (quan hệ giữa các bảng) |
| Số lượng cột trong các bảng phải giống nhau                                                    | Có thể có số lượng cột khác nhau                                          |
| UNION có thể loại bỏ bản ghi trùng                                                               | JOIN không loại bỏ bản ghi trùng (trừ khi có DISTINCT)                     |

Dùng UNION khi cần hợp nhất dữ liệu từ nhiều bảng có cấu trúc giống nhau.

Dùng JOIN khi cần kết hợp dữ liệu từ nhiều bảng có quan hệ với nhau.

### **Tối ưu UNION để tăng tốc độ**

* **Dùng UNION ALL nếu không cần loại bỏ trùng lặp** → Vì UNION phải sắp xếp để tìm trùng, nên UNION ALL sẽ nhanh hơn.
* **Đảm bảo các truy vấn con sử dụng INDEX** → Giúp tăng hiệu suất khi lấy dữ liệu.
* **Tránh ORDER BY nếu không cần thiết** → ORDER BY làm MySQL phải sắp xếp toàn bộ kết quả, làm chậm truy vấn.

## SUBQUERY

* Là truy vấn lồng bên trong một truy vấn khác (`SELECT`, `FROM`, `WHERE`, `HAVING`).
* Thường dùng để  **lọc dữ liệu, tính toán giá trị trung gian, hoặc kết hợp dữ liệu** .

```sql
SELECT id, name 
FROM customers 
WHERE id IN (
    SELECT customer_id 
    FROM orders 
    GROUP BY customer_id 
    HAVING SUM(total_amount) > 10000
);


SELECT customer_id, total_spent
FROM (
    SELECT customer_id, SUM(total_amount) AS total_spent
    FROM orders 
    GROUP BY customer_id
) AS temp_table
WHERE total_spent > 10000;


```

Subquery trong FROM giúp xử lý dữ liệu trước khi truy vấn chính lấy kết quả.

## WITH (Mysql 8.0)

* Giống subquery nhưng  **có thể tái sử dụng trong cùng truy vấn** .
* Giúp **cải thiện khả năng đọc và hiệu suất** của truy vấn.
* Chỉ tồn tại trong một truy vấn,  **không lưu trữ trong MySQL** .

Dễ đọc hơn so với subquery trong FROM, đặc biệt khi có nhiều lần sử dụng.

```sql
WITH customer_sales AS (
    SELECT customer_id, SUM(total_amount) AS total_spent
    FROM orders 
    GROUP BY customer_id
)
SELECT customer_id FROM customer_sales WHERE total_spent > 10000;

```

## VIEW

* Là **bảng ảo** giúp lưu trữ truy vấn phức tạp để dùng lại nhiều lần.
* Không lưu trữ dữ liệu, chỉ lưu  **câu lệnh truy vấn** .
* Có thể sử dụng như bảng bình thường.

Lưu ý: VIEW giúp tái sử dụng truy vấn nhưng không tối ưu hiệu suất nếu dữ liệu lớn.

```sql
CREATE VIEW vip_customers AS 
SELECT customer_id, SUM(total_amount) AS total_spent
FROM orders 
GROUP BY customer_id
HAVING total_spent > 10000;
```

### **So sánh SUBQUERY, WITH (CTE) và VIEW**

| **Tiêu chí**              | **SUBQUERY**                 | **WITH (CTE)**                                   | **VIEW**                       |
| --------------------------------- | ---------------------------------- | ------------------------------------------------------ | ------------------------------------ |
| **Tồn tại**               | Trong từng truy vấn              | Trong truy vấn hiện tại                             | Vĩnh viễn                          |
| **Hiệu suất**             | Chậm hơn nếu dùng nhiều lần  | Tối ưu hơn với truy vấn phức tạp                | Có thể tối ưu nếu MySQL cache   |
| **Dễ đọc**               | Khó đọc nếu lồng nhiều cấp  | Dễ đọc hơn                                         | Rất dễ đọc, chỉ cần `SELECT` |
| **Có thể UPDATE/DELETE?** | Không                             | Không                                                 | Có thể, nhưng có hạn chế       |
| **Khi nào dùng?**         | Khi cần truy vấn con đơn giản | Khi cần tái sử dụng dữ liệu trong một truy vấn | Khi cần tái sử dụng lâu dài    |

## Các kiểu cấu hình MySQL

### **Single Instance (Standalone) – Chạy độc lập**

**Cách hoạt động:**

* Một máy chủ MySQL duy nhất chứa toàn bộ dữ liệu.
* Các ứng dụng kết nối trực tiếp đến máy chủ này.

**Ưu điểm:**

* Dễ cài đặt và quản lý.
* Phù hợp với ứng dụng nhỏ hoặc phát triển cá nhân.

**Nhược điểm:**

* Không có khả năng chịu lỗi (nếu server hỏng, toàn bộ hệ thống ngừng hoạt động).
* Khó mở rộng khi lượng dữ liệu lớn.

### **Replication – Master-Slave (Nhân bản dữ liệu)**

**Cách hoạt động:**

* **Master** : Nhận tất cả các thao tác  **INSERT, UPDATE, DELETE** .
* **Slave** : Chỉ nhận **SELECT** (đọc dữ liệu) từ Master.
* Slave tự động đồng bộ dữ liệu từ Master thông qua  **binary log** .

**Ưu điểm:**

* **Tăng hiệu suất đọc** : Có thể phân tán truy vấn đọc (SELECT) qua nhiều Slave.
* **Cải thiện tính sẵn sàng** : Nếu Master gặp sự cố, có thể chuyển Slave thành Master.
* **Sao lưu dễ dàng hơn** : Có thể sao lưu trên Slave mà không ảnh hưởng đến Master.

**Nhược điểm:**

* **Không tự động failover** (nếu Master hỏng, cần chuyển đổi thủ công).
* **Dữ liệu có thể trễ trên Slave** do quá trình đồng bộ.

### **Replication – Master-Master**

**Cách hoạt động:**

* Cả hai server đều là **Master** và có thể ghi và đọc dữ liệu.
* Dữ liệu được đồng bộ **hai chiều** giữa cả hai Master.

**Ưu điểm:**

* **Cả hai server đều nhận truy vấn ghi và đọc** → Giảm tải tốt hơn.
* **Tự động đồng bộ dữ liệu hai chiều** .
* **Dễ mở rộng và chịu lỗi tốt hơn Master-Slave** .

**Nhược điểm:**

* **Khó xử lý xung đột dữ liệu** nếu hai Master ghi dữ liệu trùng nhau.
* **Độ trễ có thể cao nếu mạng chậm** .,

### **MySQL Sharding – Phân vùng dữ liệu**

**Cách hoạt động:**

* **Dữ liệu được chia nhỏ (shard) ra nhiều server khác nhau** .
* Mỗi server chỉ chứa một phần dữ liệu nhất định.
* Các ứng dụng phải xác định cách truy vấn đúng shard.

**Ưu điểm:**

* **Giúp mở rộng dữ liệu lớn** mà không bị giới hạn bởi một server.
* **Hiệu suất cao hơn so với các mô hình khác** .

**Nhược điểm:**

* **Phức tạp trong triển khai** (cần routing logic trên ứng dụng).
* **Khó xử lý truy vấn JOIN giữa các shard** .

### **MySQL Cluster (NDB Cluster)**

**Cách hoạt động:**

* **Dữ liệu được phân tán trên nhiều node** để đảm bảo tính sẵn sàng.
* Sử dụng **NDB Storage Engine** thay vì InnoDB/MyISAM.
* Có **Data Nodes, SQL Nodes, Management Nodes** để quản lý dữ liệu.

**Ưu điểm:**

* **Tính sẵn sàng cao** : Nếu một node hỏng, dữ liệu vẫn an toàn.
* **Mở rộng ngang tốt** : Dễ dàng thêm node mới mà không gián đoạn.
* **Hiệu suất cao với dữ liệu lớn** .

**Nhược điểm:**

* **Cấu hình phức tạp** hơn các mô hình khác.
* **Tiêu tốn nhiều tài nguyên** hơn so với Master-Slave.

### **MySQL Group Replication (Tương tự Galera Cluster)**

**Cách hoạt động:**

* Mọi node trong hệ thống đều có quyền đọc và ghi dữ liệu.
* Khi một node ghi dữ liệu, tất cả các node khác sẽ cập nhật cùng lúc.
* Nếu một node bị lỗi, hệ thống vẫn tiếp tục hoạt động bình thường.

**Ưu điểm:**

* **Tính sẵn sàng cao hơn Master-Slave** .
* **Tự động failover nếu một node gặp sự cố** .
* **Không bị tình trạng dữ liệu cũ như Master-Slave** .

**Nhược điểm:**

* **Chậm hơn so với Master-Slave do cần đồng bộ dữ liệu nhiều hơn** .
* **Khó cấu hình hơn** .
