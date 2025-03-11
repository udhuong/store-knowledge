### OOP

### SOLID
**Vì sao nên áp dụng SOLID?**
- Tăng chất lượng mã: Dễ đọc, dễ hiểu, dễ bảo trì.
- Giảm phụ thuộc: Mỗi thành phần ít bị ảnh hưởng khi thay đổi thành phần khác.
- Tăng khả năng mở rộng: Dễ thêm tính năng mới mà không phá mã cũ.
- Thân thiện với kiểm thử (Testability): Code dễ viết unit test, giảm chi phí phát triển dài hạn.

**1. Single Responsibility Principle (SRP) — Nguyên tắc trách nhiệm đơn lẻ:**
>Một class chỉ nên có một lý do để thay đổi.

- Ý nghĩa: Mỗi class nên chỉ chịu trách nhiệm về một nhiệm vụ hoặc một chức năng duy nhất. Nếu class làm quá nhiều việc, khi cần thay đổi một chức năng, có thể vô tình làm hỏng những chức năng khác.

- 🎯 Lợi ích: Code dễ đọc, dễ bảo trì, giảm thiểu ảnh hưởng lan truyền khi thay đổi.

- Ví dụ sai: Một class User vừa quản lý thông tin người dùng, vừa lo lưu trữ file log.

- Ví dụ đúng:
  - User chỉ quản lý thông tin người dùng.
  - Logger đảm nhận việc ghi log.

**2. Open/Closed Principle (OCP) — Nguyên tắc mở/đóng**
> Class nên mở để mở rộng nhưng đóng để chỉnh sửa.

- Ý nghĩa: Bạn nên có thể thêm tính năng mới bằng cách mở rộng class (thông qua kế thừa hoặc interface), chứ không phải chỉnh sửa mã nguồn hiện có.

- 🎯 Lợi ích: Dễ dàng mở rộng mà không phá vỡ mã cũ, giảm rủi ro lỗi phát sinh khi thay đổi yêu cầu.

- Ví dụ sai: Bạn có một class Shape, và mỗi lần thêm hình mới (ví dụ: hình tam giác), bạn phải chỉnh sửa phương thức draw().

- Ví dụ đúng: Tạo lớp cơ sở Shape với phương thức trừu tượng draw(), sau đó mỗi hình (Rectangle, Circle, Triangle) tự triển khai phương thức riêng.

**3. Liskov Substitution Principle (LSP) — Nguyên tắc thay thế Liskov**
> Class con có thể thay thế class cha mà không làm thay đổi hành vi của chương trình.

- Ý nghĩa: Class con nên có thể dùng thay thế class cha mà không làm thay đổi logic tổng thể của hệ thống. Nếu class con phá vỡ hành vi mong đợi, nghĩa là bạn đã vi phạm LSP.

- Ví dụ sai: Class Rectangle và Square (kế thừa từ Rectangle). Nhưng Square ghi đè setter chiều rộng/chiều cao làm mất tính nhất quán, gây ra lỗi bất ngờ.

- Ví dụ đúng: Thiết kế Square và Rectangle thành hai class độc lập, cùng triển khai interface Shape.

- 🎯 Lợi ích: Đảm bảo tính kế thừa đúng đắn, tránh lỗi khó đoán khi thay thế class.


**4. Interface Segregation Principle (ISP) — Nguyên tắc phân tách interface**
> Không nên ép class triển khai những phương thức mà nó không dùng.

- Ý nghĩa: Thay vì tạo một interface khổng lồ chứa nhiều phương thức không liên quan, hãy chia nhỏ thành các interface đặc thù.

- Ví dụ sai: Interface Animal có các phương thức run(), fly(), swim(). Khi triển khai Dog, bạn phải implement cả fly() dù không cần.

- Ví dụ đúng: Chia nhỏ thành Runnable, Flyable, Swimmable, và Dog chỉ cần triển khai Runnable.

- 🎯 Lợi ích: Code dễ bảo trì, giảm phụ thuộc không cần thiết, tăng tính linh hoạt.


**5. Dependency Inversion Principle (DIP) — Nguyên tắc đảo ngược phụ thuộc**
> Class nên phụ thuộc vào abstraction (trừu tượng) thay vì phụ thuộc vào implementation (cụ thể).

- Ý nghĩa: Thay vì tạo đối tượng trực tiếp trong class, hãy truyền nó từ bên ngoài vào (Dependency Injection). Điều này giúp bạn dễ dàng thay đổi hoặc thay thế thành phần mà không cần sửa mã nguồn chính.

- Ví dụ sai: Class Order trực tiếp tạo đối tượng MySQLDatabase để lưu đơn hàng.

- Ví dụ đúng: Order nhận vào một interface Database thông qua constructor, và bạn có thể truyền MySQLDatabase, MongoDB, hay PostgreSQL tùy nhu cầu.

- 🎯 Lợi ích: Tăng khả năng mở rộng, dễ dàng thay thế thành phần mà không ảnh hưởng đến hệ thống.

### ACID
ACID là viết tắt của bốn thuộc tính quan trọng đảm bảo tính toàn vẹn và độ tin cậy của giao dịch trong hệ quản trị cơ sở dữ liệu. Đó là:

**Atomicity (Tính nguyên tử)**
- Đảm bảo rằng một giao dịch hoặc là được thực hiện hoàn toàn, hoặc là không thực hiện gì cả. Nếu có lỗi xảy ra trong quá trình xử lý, mọi thay đổi sẽ được hoàn tác về trạng thái ban đầu.
- Ví dụ: Khi chuyển tiền từ tài khoản A sang B, nếu bước ghi nợ tài khoản A thành công nhưng ghi có tài khoản B thất bại, hệ thống sẽ hoàn nguyên số tiền ở tài khoản A.

**Consistency (Tính nhất quán)**
- Đảm bảo rằng sau khi giao dịch hoàn tất, dữ liệu luôn ở trạng thái hợp lệ, tuân thủ mọi quy tắc, ràng buộc toàn vẹn của hệ thống.
- Ví dụ: Nếu có ràng buộc tổng số tiền trong ngân hàng không thay đổi, thì sau mỗi giao dịch, tổng tiền trong các tài khoản phải bằng với trước khi giao dịch diễn ra.

**Isolation (Tính cô lập)**
- Đảm bảo rằng các giao dịch chạy đồng thời không ảnh hưởng đến nhau, dữ liệu trung gian của giao dịch này không bị lộ ra hoặc làm sai lệch kết quả của giao dịch khác.
- Ví dụ: Khi hai người cùng mua vé trên một chuyến bay, hệ thống phải đảm bảo không bán cùng một ghế cho cả hai.

**Durability (Tính bền vững)**
- Đảm bảo rằng khi giao dịch đã được xác nhận thành công, dữ liệu sẽ được lưu trữ vĩnh viễn, ngay cả khi hệ thống gặp sự cố như mất điện hoặc treo máy.
- Ví dụ: Sau khi xác nhận chuyển tiền, thông tin giao dịch sẽ không bị mất dù máy chủ gặp trục trặc.

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


### CAP:
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
ACID mạnh để đảm bảo dữ liệu chính xác (ví dụ: giao dịch ngân hàng).
BASE (Basically Available, Soft state, Eventually consistent) để tăng tốc độ và khả năng mở rộng (ví dụ: mạng xã hội, sàn thương mại điện tử).

### DRY:
Tránh lặp lại mã không cần thiết, nếu một đoạn mã xuất hiện nhiều lần, hãy tách thành hàm hoặc module để tái sử dụng.

### KISS (Keep It Simple, Stupid)
Viết mã càng đơn giản, dễ hiểu càng tốt. Tránh thiết kế quá phức tạp, làm tăng khả năng lỗi và khó bảo trì.

### YAGNI (You Aren't Gonna Need It)
Chỉ xây dựng chức năng cần thiết cho hiện tại, tránh dự đoán quá xa và thêm những tính năng chưa cần dùng tới.