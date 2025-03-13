**Có thread natively (hỗ trợ đa luồng thật sự)**

- Mỗi thread trong Java là một OS-level thread (luồng của hệ điều hành), chạy song song thực sự trên nhiều lõi CPU.
- Quản lý thread: Java cung cấp Thread Pool, ExecutorService, giúp tối ưu việc tạo và quản lý nhiều luồng.
- Nếu cần tính toán nặng, xử lý đa lõi CPU, Java với thread thật sự sẽ tối ưu hơn.

overview: concurrent package:

- khóa: synchronize, lock, reentrant lock,  reentrance read write lock
- đồng bộ thời gian giữa các luồng: Phaser, CountDownLatch, Semaphore , CyclicBarrier
- class đa luồng:

  + Queue: ConcurrentLinkQueue, ConcurrentLinkDeque, BlockingQueue
  + List: CopyOnWriteArrayList
  + Set: ConcurrentSkipListSet, CopyOnWriteArraySet
  + Map: ConcurrentHashMap, ConcurrentSkipListMap
- atomic class
- Java Core:

  + Collection: Map, List, Set, Queue, Stack và các triển khai, phân biệt cách dùng của các class
  + Stream: khái niệm, cách dùng, các hàm có trong stream (lập trình hàm)
  + Hàm lamda, functional interface
  + Lập trình đa luồng, bất đồng bộ (e note trước đó r)
  + Reflection trong java (liên quan đến field, method, class)
  + Annotation (target, retention, thuộc tính)
  + Exception: Checked exception vs Uncheck Exception, ...
  + biến và hàm static
  + JDBC: Statement vs Prepare Statement, Connection, connection pool, ...
  + Servlet: khái niệm, mục đích, ...
  + JPA
- Spring:

  + Luồng chạy: từ Dispatcher Servlet -> ...
  + Khái niệm: Ioc, DI
  + Bean Factory, Application Context
  + Bean, scope của bean, các cách khởi tạo
  + Spring security: Luồng chạy, cách hoạt động
  + AOP: Khái niệm, khi nào dùng, Point cut, Jointpoint, Advice
  + SpEL
  + Spring data
  + các thể loại annotation trong spring
  + Testing

# **Tính năng nâng cao trong Java Core**

### **Generics**

**Định nghĩa:**

Generics (hay còn gọi là kiểu tổng quát) là một tính năng quan trọng trong Java, Linh hoạt trong việc định nghĩa kiểu của dữ liệu

Kiểu dữ liệu tổng quát, giúp code linh hoạt hơn (`List<T>`, `Map<K, V>`)

**Lợi ích:**

* Kiểm tra lỗi tại thời điểm biên dịch.
* Loại bỏ ép kiểu thủ công.
* Mã nguồn dễ đọc hơn.

**Sử dụng:**

* Generics với Lớp
* Generics với Interface
* Generics với Method
* Generics với Wildcards

### **Lambda Expressions**

Lambda là một cách viết ngắn gọn của các biểu thức hàm. Đơn giản hóa việc viết code khi làm việc với **functional interfaces**

### **Functional Interfaces**

**Functional Interface** là các interface chỉ có duy nhất **một phương thức trừu tượng**

Khá nhiều, nằm trong package `java.util.function`

**Một số Functional Interfaces quan trọng:**

| Functional Interface              | Phương thức chính      | Chức năng                                                                    |
| --------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| **`Predicate<T>`**        | `boolean test(T t)`      | Kiểm tra một điều kiện, thường dùng với `filter()`                  |
| **`Function<T, R>`**      | `R apply(T t)`           | Chuyển đổi dữ liệu từ kiểu `T` sang kiểu `R`, dùng với `map()` |
| **`Consumer<T>`**         | `void accept(T t)`       | Xử lý một giá trị mà không trả về kết quả                           |
| **`Supplier<T>`**         | `T get()`                | Cung cấp một giá trị (không cần tham số đầu vào)                     |
| **`UnaryOperator<T>`**    | `T apply(T t)`           | Giống `Function<T, T>`, nhưng đầu vào và đầu ra cùng một kiểu     |
| **`BinaryOperator<T>`**   | `T apply(T t1, T t2)`    | Giống `BiFunction<T, T, T>`, nhưng đầu vào và đầu ra cùng kiểu     |
| **`BiFunction<T, U, R>`** | `R apply(T t, U u)`      | Nhận hai tham số và trả về kết quả                                      |
| **`BiPredicate<T, U>`**   | `boolean test(T t, U u)` | Kiểm tra điều kiện với hai tham số                                       |
| **`BiConsumer<T, U>`**    | `void accept(T t, U u)`  | Xử lý hai tham số mà không trả về kết quả                             |

### **Streams API**

**Stream API** là một tính năng giúp **xử lý dữ liệu theo luồng (stream processing)** một cách hiệu quả và dễ đọc hơn.

Thực hiện các thao tác trên tập hợp dữ liệu như danh sách (`List`), tập hợp (`Set`), mảng (`Array`) mà không cần viết vòng lặp `for` hoặc `while` truyền thống.

Xử lý dữ liệu với `map()`, `filter()`, `reduce()` Hỗ trợ xử lý song song với `parallelStream()`

**Đặc điểm:**

* **Làm việc theo kiểu functional (hàm)** : Giúp code ngắn gọn hơn.
* **Hỗ trợ xử lý song song (parallel processing)** : Tăng hiệu suất trên CPU đa luồng.
* **Không làm thay đổi dữ liệu gốc** : Stream tạo ra một luồng dữ liệu mới.

**Thao tác trung gian (Intermediate Operations)**

| Toán tử                                     | Chức năng                                                     | Ví dụ                                                |
| --------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------ |
| **`filter(Predicate<T>)`**            | Lọc dữ liệu theo điều kiện                                | Lọc số chẵn                                         |
| **`map(Function<T, R>)`**             | Biến đổi dữ liệu từ kiểu `T` sang `R`                | Chuyển số thành chuỗi                              |
| **`flatMap(Function<T, Stream<R>>)`** | Biến đổi từng phần tử thành `Stream<R>` rồi gộp lại | Chuyển danh sách danh sách thành danh sách phẳng |
| **`sorted()`**                        | Sắp xếp tăng dần theo `Comparable`                        | Sắp xếp danh sách số                               |
| **`sorted(Comparator<T>)`**           | Sắp xếp theo điều kiện tùy chỉnh                         | Sắp xếp theo độ dài chuỗi                        |
| **`distinct()`**                      | Loại bỏ phần tử trùng lặp                                 | Xóa số trùng trong danh sách                       |
| **`limit(n)`**                        | Lấy `n` phần tử đầu tiên                                | Lấy 3 số đầu tiên                                 |
| **`skip(n)`**                         | Bỏ qua `n` phần tử đầu                                   | Bỏ qua 2 phần tử đầu tiên                        |
| **`peek(Consumer<T>)`**               | Dùng để debug trong Stream (in từng giá trị)              | In từng phần tử sau mỗi bước xử lý             |

**Thao tác kết thúc (Terminal Operations):**

| Toán tử                                  | Chức năng                                                        | Ví dụ                                       |
| ------------------------------------------ | ------------------------------------------------------------------ | --------------------------------------------- |
| **`collect(Collectors.toList())`** | Chuyển Stream thành danh sách (`List`)                        | Trả về danh sách mới                      |
| **`collect(Collectors.toSet())`**  | Chuyển Stream thành tập hợp (`Set`)                          | Loại bỏ phần tử trùng                    |
| **`forEach(Consumer<T>)`**         | Duyệt từng phần tử trong Stream                                | In từng phần tử                            |
| **`count()`**                      | Đếm số lượng phần tử trong Stream                           | Đếm số phần tử chẵn                     |
| **`findFirst()`**                  | Lấy phần tử đầu tiên (nếu có)                              | Tìm số đầu tiên lớn hơn 10             |
| **`findAny()`**                    | Lấy phần tử bất kỳ (thường dùng với `parallelStream()`) | Tìm số bất kỳ lớn hơn 10                |
| **`allMatch(Predicate<T>)`**       | Kiểm tra tất cả phần tử có thỏa mãn điều kiện không    | Kiểm tra tất cả số có lớn hơn 0?       |
| **`anyMatch(Predicate<T>)`**       | Kiểm tra có ít nhất một phần tử thỏa mãn điều kiện     | Có số nào lớn hơn 5?                     |
| **`noneMatch(Predicate<T>)`**      | Kiểm tra không có phần tử nào thỏa mãn điều kiện        | Không có số âm?                           |
| **`reduce(BinaryOperator<T>)`**    | Gộp tất cả phần tử lại thành một giá trị duy nhất       | Tính tổng danh sách số                    |
| **`min(Comparator<T>)`**           | Tìm giá trị nhỏ nhất theo điều kiện so sánh               | Tìm số nhỏ nhất trong danh sách          |
| **`max(Comparator<T>)`**           | Tìm giá trị lớn nhất theo điều kiện so sánh               | Tìm số lớn nhất trong danh sách          |
| **`toArray()`**                    | Chuyển Stream thành mảng                                        | Chuyển danh sách số thành mảng `int[]` |

### **Annotations**

Annotation trong Java là một cơ chế để gắn thêm **metadata (siêu dữ liệu)** vào code mà không ảnh hưởng đến logic chương trình.

Annotation giúp trình biên dịch kiểm tra code, tạo tài liệu, hoặc xử lý logic đặc biệt trong runtime.

`@Override`, `@FunctionalInterface`, `@SuppressWarnings`

### Collection

Collection là một giao diện (interface) chính trong gói java.util, đóng vai trò là nền tảng cho các cấu trúc dữ liệu như danh sách (List), tập hợp (Set), hàng đợi (Queue), v.v.

Collection không phải là một lớp cụ thể mà là một giao diện, được các lớp như ArrayList, HashSet, LinkedList, v.v., triển khai (implement).

**Collection** (interface)

* **List**: Danh sách có thứ tự, cho phép trùng lặp (ví dụ: **ArrayList**, **LinkedList**).
* **Set**: Tập hợp không trùng lặp (ví dụ: **HashSet**, **TreeSet**).
* **Queue**: Hàng đợi, hỗ trợ thêm/xóa theo thứ tự nhất định (ví dụ: **PriorityQueue**, **LinkedList**).


Các Interface Chính Trong Collection

**List** :

**List** lưu các phần tử  **theo thứ tự chèn vào** , cho phép phần tử  **trùng lặp** .

* **ArrayList**: Danh sách động, truy xuất nhanh (O(1)).
* **LinkedList**: Danh sách liên kết, chèn/xóa nhanh (O(1)).
* **Vector**: Giống `ArrayList` nhưng  **đồng bộ hóa** .
* **Stack**: Ngăn xếp LIFO (Last In First Out).

Khi nào dùng `ArrayList` hay `LinkedList`?

* **Dùng `ArrayList`** khi cần truy cập ngẫu nhiên nhanh, dữ liệu ổn định, ít thao tác thêm/xóa giữa danh sách.
* **Dùng `LinkedList`** khi có nhiều thao tác chèn/xóa ở giữa danh sách, nhưng ít truy cập ngẫu nhiên.

Thông thường, `ArrayList` được sử dụng phổ biến hơn vì hiệu suất tốt hơn trong đa số trường hợp.

**Set** :

**Set** lưu các phần tử **không trùng nhau** và  **không có thứ tự** .

* **HashSet**: Không có thứ tự, hiệu suất cao (O(1)).
* **LinkedHashSet**: Giữ thứ tự chèn vào.
* **TreeSet**: Sắp xếp phần tử theo **tự nhiên** (O(log n)).

**Queue** :

**Queue** lưu các phần tử theo cơ chế  **FIFO (First In First Out)** .

Các class triển khai  **Queue** :

* **PriorityQueue**: Hàng đợi ưu tiên, sắp xếp phần tử theo  **độ ưu tiên** .
* **Deque (Double Ended Queue)**: Hỗ trợ thêm/xóa từ cả hai đầu.

**Map:**

**Map** lưu dữ liệu dưới dạng  **Key-Value** , trong đó  **Key là duy nhất** .

* **HashMap**: Không có thứ tự.
* **LinkedHashMap**: Giữ thứ tự chèn vào.
* **TreeMap**: Sắp xếp theo Key.

Garbage Collector

| Phương thức chính |
| --------------------- |

|  |
| - |
