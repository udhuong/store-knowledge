## SOLID
**SOLID là viết tắt của 5 chữ cái đầu trong 5 nguyên tắc thiết kế hướng đối tượng**

**Vì sao nên áp dụng SOLID?**
- Tăng chất lượng mã: Dễ đọc, dễ hiểu, dễ bảo trì.
- Giảm phụ thuộc: Mỗi thành phần ít bị ảnh hưởng khi thay đổi thành phần khác.
- Tăng khả năng mở rộng: Dễ thêm tính năng mới mà không làm hỏng code cũ
- Dễ dàng kiểm thử: Code dễ viết unit test, giảm chi phí phát triển dài hạn.

**1. Single Responsibility Principle (SRP) — Nguyên tắc trách nhiệm đơn lẻ:**
>Một class chỉ nên có một lý do để thay đổi.

- Ý nghĩa: Mỗi class chỉ nên có 1 nhiệm vụ, chức năng duy nhất. Nếu class làm quá nhiều việc, khi cần thay đổi một chức năng nó có thể vô tình làm hỏng những chức năng khác.

- Ví dụ sai: Một class User vừa quản lý thông tin người dùng, vừa lưu trữ file log.

- Ví dụ đúng: Tách thành 2 class riêng
  - User chỉ quản lý thông tin người dùng.
  - Logger đảm nhận việc ghi log.

- 🎯 Lợi ích: Code dễ đọc, dễ bảo trì, giảm thiểu ảnh hưởng lan truyền khi thay đổi.

**2. Open/Closed Principle (OCP) — Nguyên tắc mở/đóng**
> Class nên mở để mở rộng nhưng đóng để chỉnh sửa.

- Ý nghĩa: class có thể thêm tính năng mới bằng cách mở rộng class (thông qua kế thừa hoặc interface), chứ không phải chỉnh sửa mã nguồn hiện có.

- Ví dụ sai: có class Shape, mỗi lần thêm hình mới (ví dụ: hình tam giác), phải chỉnh sửa method draw().

- Ví dụ đúng: Tạo class cơ sở Shape với method trừu tượng draw(), sau đó mỗi hình (Rectangle, Circle, Triangle) tự triển khai method draw() theo cách riêng.

- 🎯 Lợi ích: Dễ dàng mở rộng mà không phá vỡ mã cũ, giảm rủi ro lỗi phát sinh khi thay đổi yêu cầu.

**3. Liskov Substitution Principle (LSP) — Nguyên tắc thay thế Liskov**
> Class con có thể thay thế class cha mà không làm thay đổi hành vi của chương trình.

- Ý nghĩa: Class concó thể thay thế class cha mà không làm thay đổi logic tổng thể của chương trình. Nếu class con phá vỡ hành vi mong đợi thì vi phạm nguyên tắc

- Ví dụ sai: Class Rectangle và Square (kế thừa từ Rectangle). Nhưng Square ghi đè setter chiều rộng/chiều cao làm mất tính nhất quán, gây ra lỗi bất ngờ.

- Ví dụ đúng: Thiết kế Square và Rectangle thành hai class độc lập, cùng triển khai interface Shape.

- 🎯 Lợi ích: Đảm bảo tính kế thừa đúng đắn, tránh lỗi khó đoán khi thay thế class.

**4. Interface Segregation Principle (ISP) — Nguyên tắc phân tách interface**
> Không nên ép class triển khai những method mà nó không dùng.

- Ý nghĩa: Thay vì tạo một interface khổng lồ chứa nhiều method không liên quan, hãy chia nhỏ thành các interface đặc thù nhỏ hơn.

- Ví dụ sai: Interface Animal có các method run(), fly(), swim(). Khi triển khai Dog, bạn phải implement cả fly() dù không cần.

- Ví dụ đúng: Chia nhỏ thành Runnable, Flyable, Swimmable, và Dog chỉ cần triển khai Runnable.

- 🎯 Lợi ích: Code dễ bảo trì, giảm phụ thuộc không cần thiết, tăng tính linh hoạt.

**5. Dependency Inversion Principle (DIP) — Nguyên tắc đảo ngược phụ thuộc**
> Modules cấp cao không nên phụ thuộc vào modules cấp thấp. Class nên phụ thuộc vào abstraction (trừu tượng) thay vì phụ thuộc vào implementation (cụ thể).

- Ý nghĩa: Thay vì tạo đối tượng trực tiếp trong class, hãy truyền nó từ bên ngoài vào (Dependency Injection). Điều này giúp bạn dễ dàng thay đổi hoặc thay thế thành phần mà không cần sửa mã nguồn chính.

- Ví dụ sai: Class Order trực tiếp tạo đối tượng new MySQLDatabase để lưu đơn hàng.

- Ví dụ đúng: Order nhận vào một interface Database thông qua constructor, áu đó truyền MySQLDatabase, MongoDB, hay PostgreSQL tùy nhu cầu.

- 🎯 Lợi ích: Tăng khả năng mở rộng, dễ dàng thay thế thành phần mà không ảnh hưởng đến hệ thống.
<br>

## ACID
ACID là viết tắt của bốn thuộc tính quan trọng đảm bảo tính toàn vẹn và độ tin cậy của giao dịch trong hệ quản trị cơ sở dữ liệu. Đó là:

**Atomicity (Tính nguyên tử)**
- Giao dịch phải là toàn vẹn hoặc không có gì cả.
- Nếu một phần của giao dịch thất bại, toàn bộ giao dịch sẽ bị hủy bỏ, và cơ sở dữ liệu trở về trạng thái ban đầu trước khi thực hiện giao dịch.
- Ví dụ: Chuyển tiền giữa hai tài khoản — nếu trừ tiền ở tài khoản A thành công nhưng cộng tiền ở tài khoản B thất bại, toàn bộ giao dịch sẽ bị hủy.

**Consistency (Tính nhất quán)**
- Sau khi giao dịch hoàn thành, cơ sở dữ liệu phải ở trạng thái hợp lệ, tuân thủ mọi ràng buộc và quy tắc đã thiết lập.
- Ví dụ: Nếu một giao dịch rút tiền vi phạm hạn mức tài khoản, giao dịch sẽ bị từ chối để đảm bảo tính nhất quán của dữ liệu.

**Isolation (Tính cô lập)**
- Các giao dịch diễn ra đồng thời không được ảnh hưởng lẫn nhau.
- Hệ thống đảm bảo kết quả của một giao dịch không bị ảnh hưởng bởi các giao dịch đang diễn ra song song.
- Ví dụ: Khi hai người dùng cùng chỉnh sửa cùng một bản ghi, hệ thống phải đảm bảo thứ tự thực hiện hoặc khóa bản ghi để tránh sai lệch dữ liệu.

**Durability (Tính bền vững)**
- Sau khi giao dịch được xác nhận (commit), dữ liệu phải được lưu vĩnh viễn và không bị mất ngay cả khi hệ thống gặp sự cố.
- Điều này thường được đảm bảo nhờ các cơ chế như ghi log, sao lưu dữ liệu, và hệ thống khôi phục.
- Ví dụ: Sau khi xác nhận chuyển tiền, thông tin giao dịch sẽ không bị mất dù máy chủ gặp trục trặc.

**Ví dụ thực tế**

Giả sử bạn thực hiện chuyển 1 triệu đồng từ tài khoản A sang tài khoản B:

- **Atomicity**: Nếu hệ thống trừ tiền A nhưng không cộng tiền B, toàn bộ giao dịch sẽ bị hủy.
- **Consistency**: Sau giao dịch, tổng số tiền giữa A và B phải bằng tổng trước đó.
- **Isolation**: Nếu có một giao dịch khác đang cập nhật tài khoản A cùng lúc, giao dịch chuyển tiền sẽ phải đợi hoặc xử lý riêng biệt.
- **Durability**: Khi giao dịch thành công và đã "commit," dữ liệu được lưu vĩnh viễn, không mất dù server bị sập.

**Triển khai và đánh đổi**

**🎯 1. Atomicity (Tính nguyên tử)**
- Cách triển khai:
  - Sử dụng log giao dịch (write-ahead log) để ghi lại các thay đổi trước khi thực sự áp dụng lên cơ sở dữ liệu. Nếu có lỗi, hệ thống sẽ dùng log này để khôi phục lại trạng thái cũ.
  - Undo/Redo Logs: Để hoàn tác hoặc làm lại các thao tác khi cần.

- Đánh đổi:
  - Hiệu năng: Ghi log và giữ trạng thái giao dịch đòi hỏi tài nguyên hệ thống (RAM, disk I/O).
  - Độ trễ: Việc đảm bảo nguyên tử có thể làm chậm tốc độ xử lý, đặc biệt trong các giao dịch phức tạp hoặc nhiều nút trong hệ thống phân tán.

**🧩 2. Consistency (Tính nhất quán)**
- Cách triển khai:
  - Ràng buộc toàn vẹn (constraints), trigger, và khóa ngoại (foreign key) để đảm bảo dữ liệu luôn hợp lệ.
  - Kiểm tra trước & sau giao dịch để đảm bảo dữ liệu không rơi vào trạng thái lỗi.
- Đánh đổi:
  - Hiệu suất ghi chậm: Kiểm tra tính nhất quán trong từng bước có thể làm giảm tốc độ giao dịch.
  - Khả năng mở rộng kém: Trong hệ thống phân tán, việc giữ nhất quán mạnh (strong consistency) có thể gây ra độ trễ cao do cần đồng bộ hóa giữa các nút.

**🔗 3. Isolation (Tính cô lập)**
- Cách triển khai:
  - Locking (khóa): Khóa bảng, dùng để ngăn truy cập đồng thời gây xung đột dữ liệu.
  - Multi-Version Concurrency Control (MVCC): Tạo các bản sao dữ liệu tạm thời để giao dịch đọc và ghi không chặn nhau.
- Cấp độ cô lập (Isolation Levels):
  - Read Uncommitted → Ít cô lập, nhanh nhưng dễ sai sót.
  - Read Committed → An toàn hơn, nhưng vẫn có thể gặp non-repeatable read.
  - Repeatable Read → Cân bằng tốt giữa an toàn và hiệu suất.
  - Serializable → Mức cao nhất, nhưng rất tốn tài nguyên và dễ gặp deadlock.

- Đánh đổi:
  - Độ trễ cao khi tăng cô lập: Mức cô lập cao như Serializable làm giảm khả năng xử lý đồng thời.
  - Deadlock: Khi nhiều giao dịch cùng khóa tài nguyên và chờ nhau, hệ thống có thể rơi vào bế tắc.

**🛡 4. Durability (Tính bền vững)**
- Cách triển khai:
  - Redo Log & Checkpointing: Dùng file log và lưu trữ trạng thái định kỳ.
  - Replica & Backup: Nhân bản dữ liệu ra nhiều nơi để tránh mất mát khi máy chủ gặp sự cố.
- Đánh đổi:
  - Chi phí lưu trữ: Cần không gian đĩa lớn để lưu log và bản sao dữ liệu.
  - Hiệu năng ghi giảm: Ghi log và đồng bộ dữ liệu làm tăng độ trễ của giao dịch.

<br>

## CAP
 Một hệ thống không thể đảm bảo hoàn hảo cả 3 yếu tố cùng lúc.

- Consistency (Nhất quán)
- Availability (Khả dụng)
- Partition Tolerance (Khả năng chịu phân mảnh)

| Loại hệ thống                                                                     | Đảm bảo                    | Chấp nhận hy sinh         |
| --------------------------------------------------------------------------------- | -------------------------- | ------------------------- |
| CP (Consistency + Partition Tolerance)                                            | Nhất quán và chịu lỗi mạng | Mất khả dụng khi lỗi mạng |
| AP (Availability + Partition Tolerance)                                           | Khả dụng và chịu lỗi mạng  | Mất nhất quán tạm thời    |
| CA (Consistency + Availability) (không tồn tại trong hệ thống phân tán hoàn toàn) | Nhất quán và khả dụng      | Không chịu được phân mảnh |

SQL databases (MySQL, PostgreSQL) thường ưu tiên Consistency và Partition Tolerance (CP).  
NoSQL databases (Cassandra, MongoDB) ưu tiên Availability và Partition Tolerance (AP).

**Tùy theo nhu cầu, bạn có thể chọn hệ thống:**  
- ACID mạnh để đảm bảo dữ liệu chính xác (ví dụ: giao dịch ngân hàng).  
- BASE (Basically Available, Soft state, Eventually consistent) để tăng tốc độ và khả năng mở rộng (ví dụ: mạng xã hội, sàn thương mại điện tử).

## DRY (Don't Repeat Yourself)
Tránh lặp lại mã không cần thiết, nếu một đoạn mã xuất hiện nhiều lần, hãy tách thành hàm hoặc module để tái sử dụng.

## KISS (Keep It Simple, Stupid)
Viết mã càng đơn giản, dễ hiểu càng tốt. Tránh thiết kế quá phức tạp, làm tăng khả năng lỗi và khó bảo trì.

## YAGNI (You Aren't Gonna Need It)
Chỉ xây dựng chức năng cần thiết cho hiện tại, tránh dự đoán quá xa và thêm những tính năng chưa cần dùng tới.