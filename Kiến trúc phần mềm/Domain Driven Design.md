## DDD là gì?

Là  **phương pháp thiết kế phần mềm** , tập trung giải quyết các bài toán nghiệp vụ phức tạp, tách biệt rõ ràng các phần:  **Domain, Application, Infrastructure, Interface/UI** .

## Các thành phần chính trong DDD

**Entity**

* Là đối tượng nghiệp vụ có **định danh duy nhất** (id).
* Ví dụ: `User`, `Order`, `Product`...

**Value Object**

* Không có định danh riêng, so sánh bằng  **giá trị** .
* Ví dụ: `Address` (2 address giống hệt nhau thì coi như cùng 1 Address).

**Aggregate và Aggregate Root**

* Tập hợp nhiều Entity và Value Object tạo thành 1 khối logic.
* **Aggregate Root** là entry point duy nhất để thao tác với Aggregate.
* Ví dụ: `Order` là Aggregate Root, nó chứa các `OrderItem`.

**Repository**

* Interface để thao tác lưu trữ dữ liệu Aggregate Root.
* Ví dụ: `OrderRepository`, `UserRepository`.

**Service**

* Chứa các logic nghiệp vụ phức tạp không phù hợp đặt trong Entity/Aggregate.
* Có thể là:
  * **Domain Service** (logic nghiệp vụ thuần)
  * **Application Service** (xử lý flow của use case)

**Factory**

* Dùng để tạo mới các Aggregate hoặc Entity phức tạp.

**Application Layer**

* Xử lý flow của use case.
* Gọi domain service, repository, gửi sự kiện...

**Infrastructure Layer**

* Chứa code kỹ thuật: database, API ngoài, email, file storage...

**Domain Event**

* Thông báo khi có sự kiện trong domain, ví dụ: "OrderPlacedEvent".

## Tầng trong DDD (Layered Architecture)

```txt
┌──────────────────────────────┐
│        Presentation          │  (Controller / API / UI)
└──────────────────────────────┘
            │
┌──────────────────────────────┐
│      Application Layer       │  (Flow xử lý use case)
└──────────────────────────────┘
            │
┌──────────────────────────────┐
│         Domain Layer         │  (Entity, Aggregate, Value Object, Service)
└──────────────────────────────┘
            │
┌──────────────────────────────┐
│     Infrastructure Layer     │  (DB, Cache, Email, API ngoài)
└──────────────────────────────┘

```

### Ví dụ thực tế (Đơn giản)

Giả sử chức năng **Đặt hàng (Place Order):**

* **Entity** : `Order`, `Product`, `Customer`
* **Value Object** : `Address`, `Money`
* **Aggregate Root** : `Order`
* **Repository** : `OrderRepository`
* **Domain Service** : `ShippingCalculator`
* **Application Service** : `PlaceOrderService`
* **Domain Event** : `OrderPlacedEvent`

Luồng:

1. Controller nhận request => Gọi Application Service.
2. Application Service gọi Domain Service + Repository để xử lý.
3. Domain Service tính phí vận chuyển.
4. Application Service lưu Order mới qua Repository.
5. Bắn `OrderPlacedEvent` để xử lý tiếp (email, log...).

## Phân biệt Domain Service và Application Service

### Domain Service

* Là nơi chứa  **logic nghiệp vụ thuần tuý** , tức là những quy tắc kinh doanh, những quy tắc mà nếu công ty bạn đổi framework, đổi ngôn ngữ, thì nó vẫn giữ nguyên.
* Nó không phụ thuộc vào flow, không phụ thuộc vào infrastructure.
* Thường là khi logic nghiệp vụ quá phức tạp mà **không thể nằm gọn trong Entity** được nữa, thì mình tách ra thành Domain Service.

OK: Ngắn gọn lại thằng class này n nhận dữ liệu vào từ param có đầy đủ dữ liệu và xử lý n không query gì cả. Mà thằng gọi nó và truyền dữ liệu là Application Service

```java
public class TransferMoneyService {
    public void transfer(Account from, Account to, Money amount) {
        if (!from.canWithdraw(amount)) {
            throw new BusinessException("Insufficient funds");
        }
        from.withdraw(amount);
        to.deposit(amount);
    }
}
```

* Quy tắc nghiệp vụ: "Không đủ tiền thì không cho chuyển."
* Không biết HTTP request là gì, không biết flow nào gọi nó, chỉ lo xử lý nghiệp vụ.

### **Application Service**

* Là tầng **điều phối** (orchestrator).
* Nhiệm vụ:  **thực thi flow của 1 use case cụ thể** , gọi các Domain Service, repository, gửi event, log, ...
* Không chứa logic nghiệp vụ sâu, chỉ điều phối flow thôi.
* Biết về infrastructure, transaction, logging, etc.

OK: Ngắn gọn thằng này sẽ gọi repo để lấy dữ liệu, truyền dữ liệu vào thằng domain service để n xử lý. Domain service chỉ là các logic nhỏ lẻ, thằng này n sẽ điều hướng các logic đó. Và nó chỉ thao tác với interface repo thôi nhá

```java
public class TransferMoneyApplicationService {
    private final TransferMoneyService transferMoneyService;
    private final AccountRepository accountRepository;

    public void transferMoney(String fromId, String toId, BigDecimal amount) {
        Account from = accountRepository.findById(fromId);
        Account to = accountRepository.findById(toId);
      
        transferMoneyService.transfer(from, to, new Money(amount));

        accountRepository.save(from);
        accountRepository.save(to);
    }
}

```

* Nó biết flow: lấy account, xử lý, lưu lại.
* Gọi Domain Service để xử lý nghiệp vụ thuần.
* Biết về persistence (repository).

### Tổng kết

| Tiêu chí                       | Domain Service                  | Application Service                             |
| -------------------------------- | ------------------------------- | ----------------------------------------------- |
| Nhiệm vụ                       | Chứa logic nghiệp vụ thuần  | Điều phối flow của use-case                 |
| Phụ thuộc tầng nào           | Chỉ thuộc tầng Domain        | Thuộc tầng Application, gọi các tầng khác |
| Biết repository không?         | Không                          | Có                                             |
| Biết transaction không?        | Không                          | Có                                             |
| Biết flow của use-case không? | Không                          | Có                                             |
| Test unit                        | Test theo nghiệp vụ đơn lẻ | Test flow tổng thể (integration test nhỏ)    |

* **Domain Service** : “Tôi chỉ biết làm đúng nghiệp vụ, không quan tâm ai gọi tôi.”
* **Application Service** : “Tôi là người điều phối, ai cần gì tôi chỉ đạo.”

### Mô tả luồng:

1. **Controller / API Layer**
   * Nhận request từ outside (HTTP, Message Queue, ...).
   * Chỉ gọi `Application Service`, không xử lý logic gì cả.
2. **Application Service**
   * Điều phối flow: gọi repo lấy data, gọi Domain Service xử lý nghiệp vụ.
   * Nếu cần transaction, mở transaction tại đây.
   * Sau khi xử lý xong, lưu lại qua repository.
3. **Domain Layer**
   * **Entity** : đại diện cho đối tượng nghiệp vụ chính (User, Order, Product).
   * **Value Object** : đại diện cho giá trị bất biến (Money, Address).
   * **Domain Service** : chứa nghiệp vụ phức tạp không thuộc riêng Entity nào.
4. **Repository Interface**
   * Application Service chỉ biết interface, không biết chi tiết lưu trữ.
5. **Infrastructure Layer**
   * Repository thực thi cụ thể (JPA Repository, MyBatis, File, API từ service khác, ...).
