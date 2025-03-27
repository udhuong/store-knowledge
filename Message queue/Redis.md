Redis Cluster lưu dữ liệu bằng cách **phân mảnh (sharding)** trên nhiều node, nhưng khi truy vấn dữ liệu, nó  **không tìm ở tất cả node** , mà chỉ tìm ở  **node chứa key đó** .

### Cách Redis Cluster lưu và tìm dữ liệu

**Phân mảnh bằng Hash Slot**

* Redis Cluster chia **toàn bộ key space** thành  **16384 hash slots** .
* Mỗi node trong cluster sẽ chịu trách nhiệm một số hash slot nhất định.
* Khi bạn lưu một key, Redis tính toán hash slot của key đó bằng công thức:

```txt
HASH_SLOT = CRC16(key) % 16384
```

* Sau đó, Redis sẽ lưu key vào node chịu trách nhiệm cho hash slot đó.

**Tìm kiếm dữ liệu**

* Khi lấy dữ liệu (`GET key`), Redis cũng tính hash slot của key.
* Redis biết  **node nào đang giữ hash slot đó** , nên truy vấn  **đến đúng node đó** , không tìm ở tất cả các node.
* Nếu truy vấn nhầm node, Redis trả về lỗi  **MOVED** , kèm theo địa chỉ node đúng.

Ví dụ:

Giả sử có 3 node trong cluster:

* **Node A** giữ slot `0-5460`
* **Node B** giữ slot `5461-10922`
* **Node C** giữ slot `10923-16383`

```txt
HASH_SLOT = CRC16("user:123") % 16384
```

Ví dụ slot tính ra là `8000`, thì key này được lưu vào  **Node B** .

Sau này khi `GET user:123`, Redis sẽ truy vấn  **thẳng vào Node B** .

#### **Lưu ý**

* **Không cần tìm ở tất cả node** , vì Redis Cluster đã biết key ở đâu.
* Nếu cluster thay đổi (thêm/xóa node), Redis sẽ **di chuyển hash slot** giữa các node để cân bằng tải.
* **Multi-key operation (như `MGET`, `MSET`) chỉ hoạt động khi tất cả key thuộc cùng một hash slot** .
