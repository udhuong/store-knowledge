**Có thread natively (hỗ trợ đa luồng thật sự)**

- Mỗi thread trong Java là một OS-level thread (luồng của hệ điều hành), chạy song song thực sự trên nhiều lõi CPU.
- Quản lý thread: Java cung cấp Thread Pool, ExecutorService, giúp tối ưu việc tạo và quản lý nhiều luồng.
- Nếu cần tính toán nặng, xử lý đa lõi CPU, Java với thread thật sự sẽ tối ưu hơn.

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

### Garbage Collector

**Garbage Collector (GC) trong Java** là một cơ chế **tự động thu hồi bộ nhớ** của **các object không còn được sử dụng** để  **giải phóng RAM và tối ưu hiệu suất chương trình** .

📌 **Java sử dụng GC để quản lý bộ nhớ heap và tránh memory leak.**

📌 **Lập trình viên không cần giải phóng bộ nhớ thủ công như trong C/C++.**

### Bộ nhớ Heap

#### **Bộ nhớ Heap là gì?**

📌 **Heap Memory** là vùng bộ nhớ  **dùng để lưu trữ object trong Java** .

📌 **Bất kỳ object nào được tạo bằng từ khóa `new` sẽ nằm trong Heap.**

📌 **Bộ nhớ Heap được quản lý bởi Garbage Collector (GC)** để thu hồi object không còn sử dụng.

🔥 **Đặc điểm của Heap Memory:**

✅ Chứa object có thể được **chia sẻ** giữa các thread.

✅ Khi không còn tham chiếu, GC sẽ thu hồi để tránh memory leak.

✅ Tự động mở rộng nếu cần (lên đến giới hạn tối đa `-Xmx`).

```java
class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }
}

public class HeapExample {
    public static void main(String[] args) {
        Person p1 = new Person("Alice");  // Object "Alice" nằm trong Heap
        Person p2 = new Person("Bob");    // Object "Bob" cũng nằm trong Heap
    }
}

```

**Heap Memory được chia thành 3 phần chính** để tối ưu quản lý bộ nhớ:

| **Vùng Heap**               | **Chức năng**                                                  |
| ---------------------------------- | ---------------------------------------------------------------------- |
| **Young Generation**         | Chứa object mới tạo, GC chạy thường xuyên.                      |
| **Old Generation (Tenured)** | Chứa object tồn tại lâu dài. GC chạy ít hơn nhưng chậm hơn. |
| **Metaspace**                | Chứa thông tin class, method, không bị GC dọn dẹp.               |

## **Heap vs Stack – Khác nhau thế nào?**

| **Tiêu chí**           | **Heap**             | **Stack**                          |
| ------------------------------ | -------------------------- | ---------------------------------------- |
| **Dùng để lưu trữ** | Object (instance)          | Biến cục bộ, lời gọi hàm           |
| **Quản lý bộ nhớ**   | Bởi Garbage Collector     | Quản lý tự động (LIFO)              |
| **Tốc độ**            | Chậm hơn (GC thu hồi)   | Nhanh hơn (vì nhỏ và có cấu trúc) |
| **Tầm vực biến**      | Object tồn tại lâu hơn | Biến sẽ bị xóa khi hàm kết thúc   |
| **Lỗi phổ biến**      | OutOfMemoryError           | StackOverflowError                       |

```java
public class MemoryExample {
    public static void main(String[] args) {
        int x = 10;         // Lưu trong Stack
        Person p = new Person("Alice");  // Object "Alice" lưu trong Heap
    }
}

```

🔥 **`x` nằm trong Stack, `p` nằm trong Heap. Khi hàm kết thúc, `x` bị xóa ngay, còn `p` chờ GC thu hồi.**

✅ JVM yêu cầu OS cấp phát thêm RAM (nếu chưa đạt giới hạn `-Xmx`).

✅ Nếu Heap đầy, JVM sẽ chạy GC để thu hồi bộ nhớ.

✅ Nếu không còn bộ nhớ Heap và GC không thể giải phóng đủ, **Java sẽ bị lỗi `OutOfMemoryError:`**

#### String Pool

**String Pool** là một khu vực đặc biệt trong bộ nhớ Heap của Java, nơi các đối tượng `String` được lưu trữ để tối ưu hóa hiệu suất và giảm lãng phí bộ nhớ.

**1. Cách hoạt động của String Pool**

* Khi bạn tạo một `String` bằng cách sử dụng **literal** (chuỗi đặt trong dấu ngoặc kép), Java sẽ kiểm tra xem chuỗi đó đã có trong **String Pool** chưa.
* Nếu có, nó sẽ trả về tham chiếu đến chuỗi có sẵn thay vì tạo một đối tượng mới.
* Nếu chưa có, nó sẽ tạo một đối tượng `String` mới trong String Pool.

**2. Lợi ích của String Pool**

* **Tiết kiệm bộ nhớ:** Vì các chuỗi giống nhau sẽ được tái sử dụng.
* **Cải thiện hiệu suất:** So sánh bằng `==` nhanh hơn so với `.equals()`.

**3. Nhược điểm**

* Nếu có quá nhiều chuỗi khác nhau, **String Pool** có thể chiếm nhiều bộ nhớ và gây ra  **OutOfMemoryError** .
* Không thể thay đổi chuỗi sau khi đã được đặt trong Pool do tính **immutable** của `String`.

### Exception

Trong Java, exception được chia thành 3 loại chính:

1. **Checked Exception**
   * Được kiểm tra tại thời điểm biên dịch (compile-time).
   * Bắt buộc phải xử lý nếu không sẽ gây lỗi khi biên dịch.
   * Ví dụ: `IOException`, `SQLException`, `FileNotFoundException`.
2. **Unchecked Exception**
   * Xảy ra trong quá trình thực thi (runtime) và không bắt buộc phải xử lý.
   * Thường là lỗi do lập trình viên, có thể tránh được nếu viết code cẩn thận.
   * Ví dụ: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`.
3. **Error**
   * Lỗi nghiêm trọng liên quan đến môi trường hoặc hệ thống.
   * Không thể khắc phục trong mã nguồn.
   * Ví dụ: `OutOfMemoryError`, `StackOverflowError`.

```php
                 Throwable
                 /      \
         Exception     Error
         /       \
Checked    Unchecked

```

### I/O (Đọc/Ghi File)

**Chính Dùng Để Đọc/Ghi File:**

* **`File`** : Đại diện cho file hoặc thư mục trong hệ thống tệp.
* **`FileReader` / `FileWriter`** : Đọc/ghi file theo ký tự.
* **`BufferedReader` / `BufferedWriter`** : Đọc/ghi file hiệu quả bằng cách sử dụng bộ nhớ đệm. giúp đọc từng dòng, tối ưu hiệu suất.
* **`FileInputStream` / `FileOutputStream`** : Đọc/ghi file theo byte.
* **`DataInputStream` / `DataOutputStream`** : Đọc/ghi dữ liệu nguyên thủy.
* **`RandomAccessFile`** : Đọc/ghi file ngẫu nhiên.

**So Sánh Các Cách Đọc/Ghi File**

| **Phương thức**                 | **Đọc/Ghi** | **Ưu điểm**                             | **Nhược điểm**                        |
| ---------------------------------------- | ------------------- | ------------------------------------------------ | ----------------------------------------------- |
| `FileReader`/`FileWriter`            | Theo ký tự        | Dễ sử dụng, phù hợp với văn bản          | Không tối ưu cho dữ liệu lớn              |
| `BufferedReader`/`BufferedWriter`    | Theo dòng          | Nhanh hơn do có bộ đệm                      | Cần đóng `BufferedReader`sau khi sử dụng |
| `FileInputStream`/`FileOutputStream` | Theo byte           | Phù hợp cho file nhị phân (ảnh, video, PDF) | Không thể đọc theo dòng                    |
| `RandomAccessFile`                     | Ngẫu nhiên        | Đọc/ghi tại vị trí bất kỳ trong file      | Cú pháp phức tạp hơn                       |

**Lưu Ý Khi Làm Việc Với File**

* **Luôn đóng file sau khi sử dụng** để tránh rò rỉ tài nguyên.
* **Sử dụng `try-with-resources`** để đảm bảo file được đóng tự động.
* **Kiểm tra sự tồn tại của file** trước khi đọc hoặc ghi.
* **Xử lý exception** bằng `try-catch` để tránh chương trình bị lỗi khi gặp sự cố.

Ví dụ với `try-with-resources`:

```php
import java.io.*;

public class TryWithResourcesExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}

```

`try-with-resources` là một cơ chế trong Java (từ Java 7 trở đi) giúp tự động đóng tài nguyên (resources) như file, stream, socket, v.v. sau khi sử dụng. Bất kỳ đối tượng nào triển khai interface `AutoCloseable` hoặc `Closeable` đều có thể sử dụng với `try-with-resources`.

Sau khi khối `try` kết thúc, Java sẽ tự động gọi `close()` trên resource mà không cần lập trình viên gọi `close()` thủ công.

* `BufferedReader` được khai báo trong dấu ngoặc `()` của `try`. Điều này đảm bảo rằng khi khối `try` kết thúc, `BufferedReader` sẽ tự động đóng, ngay cả khi có ngoại lệ xảy ra.
* Không cần gọi `reader.close()` thủ công.

**Nên sử dụng `try-with-resources` thay vì `try-finally`** vì nó đơn giản hơn và ít lỗi hơn.

**Thao tác file cơ bản:**

```java
// Kiểm tra file tồn tại
File file = new File("test.txt");
file.exists()
```

## Các cơ chế khóa luồng

Starvation: khi một **luồng liên tục bị trì hoãn** vì  **các luồng khác được ưu tiên hơn. **Sử dụng fair mode (`true`) trong `ReentrantReadWriteLock`** để đảm bảo công bằng giữa đọc và ghi.**

| **Cơ chế khóa**                                   | **Ưu điểm**                                                                                           | **Nhược điểm**                                                                       | **Hiệu suất**                                | **Khi nào nên dùng?**                                                                |
| ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **`synchronized`**                                 | ✅ Dễ sử dụng, tích hợp sẵn trong Java                                                                   | ❌ Hiệu suất kém vì chặn toàn bộ luồng khác                                           | ⏳ Chậm (do block toàn bộ luồng truy cập)       | Khi cần đồng bộ hóa đơn giản, không cần hiệu suất cao                             |
| **`ReentrantLock`**                                | ✅ Linh hoạt hơn `synchronized`, hỗ trợ `tryLock()`,`lockInterruptibly()`                            | ❌ Phải quản lý khóa thủ công (`lock()`và `unlock()`)                               | ⚡ Nhanh hơn `synchronized`khi có nhiều luồng  | Khi cần kiểm soát timeout, kiểm tra trạng thái khóa                                    |
| **`ReentrantReadWriteLock`**                       | ✅ Tách biệt khóa đọc (`readLock()`) và khóa ghi (`writeLock()`) để cải thiện hiệu suất đọc | ❌ Phức tạp hơn, có thể gây**starvation**(ưu tiên đọc làm ghi bị chặn lâu) | 🚀 Hiệu suất cao nếu đọc nhiều hơn ghi        | Khi có nhiều luồng đọc hơn ghi (cache, cấu trúc dữ liệu chia sẻ)                   |
| **`StampedLock`**                                  | ✅ Hỗ trợ `tryOptimisticRead()`,`tryConvertToReadLock()`giúp tối ưu hóa                              | ❌ Không reentrant, khó sử dụng, dễ bị deadlock nếu không dùng đúng                 | 🚀 Cao nhất nếu tận dụng `tryOptimisticRead()` | Khi cần hiệu suất tối đa, nhiều đọc hơn ghi, không cần reentrant                   |
| **`Atomic`(`AtomicInteger`,`AtomicLong`, …)** | ✅ Rất nhanh, không cần khóa (dùng**CAS - Compare And Swap** )                                      | ❌ Chỉ hoạt động với**biến đơn giản**(int, long, boolean, …)                   | ⚡ Cực nhanh, không chặn luồng                   | Khi chỉ cần thao tác trên biến đơn giản mà không cần khóa toàn bộ đối tượng |

#### **`synchronized` (Khóa Mặc Định của Java) - sinh qùa lai**

🔥  **Ưu điểm** :

✅  **Dễ dùng** , chỉ cần thêm `synchronized` vào method hoặc block code.

✅  **Quản lý khóa tự động** , không lo `unlock()` bị quên.

✅ **An toàn** với mọi tình huống đa luồng.

❌  **Nhược điểm** :

❌  **Chặn toàn bộ luồng khác** , kể cả khi chỉ có một luồng ghi còn lại là đọc.

❌  **Không hỗ trợ timeout hoặc kiểm tra trạng thái khóa** .

❌  **Chậm hơn `ReentrantLock` và `StampedLock` trong trường hợp có nhiều luồng** .

📌  **Ứng dụng** :

✔ Khi cần  **đồng bộ đơn giản** .

✔ Khi có **ít luồng truy cập** và hiệu suất không phải là vấn đề.

---

#### **`ReentrantLock` (Khóa Linh Hoạt Hơn `synchronized`) - ri en trừn**

🔥  **Ưu điểm** :

✅ Hỗ trợ **`tryLock()`** (không chờ khóa nếu đã bị giữ).

✅ Hỗ trợ **`lockInterruptibly()`** (cho phép hủy luồng khi chờ khóa).

✅ Hiệu suất **tốt hơn `synchronized`** khi có nhiều luồng.

❌  **Nhược điểm** :

❌  **Cần gọi `lock()` và `unlock()` thủ công** , dễ quên gây deadlock.

❌ **Không có phân tách khóa đọc và khóa ghi** như `ReentrantReadWriteLock`.

📌  **Ứng dụng** :

✔ Khi cần kiểm soát khóa tốt hơn `synchronized`.

✔ Khi cần **thử lấy khóa mà không bị chặn mãi mãi** (`tryLock()`).

---

#### **`ReentrantReadWriteLock` (Khóa Đọc/Ghi)**

🔥 **Ưu điểm:**

✅  **Tách biệt khóa đọc (`readLock()`) và khóa ghi (`writeLock()`)** , giúp tăng hiệu suất.

✅ **Hỗ trợ nhiều luồng đọc cùng lúc** nếu không có luồng ghi nào.

✅ **Hữu ích khi đa số thao tác là đọc** (ví dụ: bộ nhớ cache).

❌ **Nhược điểm:**

❌  **Có thể gây starvation cho luồng đọc** , nếu có quá nhiều luồng ghi liên tục giữ `writeLock()`, khiến luồng đọc không thể chạy.

❌ **Chậm hơn `StampedLock`** khi lượng truy cập lớn, do cơ chế chờ công bằng giữa đọc và ghi.

📌 **Ứng dụng:**

✔ Khi **có nhiều luồng đọc hơn ghi** (cấu trúc dữ liệu chia sẻ, cache).

✔ Khi  **cần bảo vệ dữ liệu nhưng vẫn tối ưu hóa hiệu suất đọc** .

✔ Khi **cần một cơ chế khóa reentrant** nhưng muốn phân biệt giữa đọc và ghi.

---

#### **`StampedLock` (Khóa Hiệu Suất Cao Nhất)**

🔥  **Ưu điểm** :

✅ Hỗ trợ **`tryOptimisticRead()`** – cho phép đọc mà không cần khóa thực sự.

✅ Hỗ trợ **chuyển đổi giữa write lock và read lock** (`tryConvertToReadLock()`).

✅ **Tối ưu hiệu suất đọc** cao hơn `ReentrantReadWriteLock`.

❌  **Nhược điểm** :

❌  **Không reentrant** , cùng một luồng không thể lock nhiều lần.

❌  **Dễ bị deadlock nếu quên mở khóa** .

📌  **Ứng dụng** :

✔ Khi **hầu hết thao tác là đọc** và  **chỉ có một số ít thao tác ghi** .

✔ Khi cần  **hiệu suất cao nhất có thể** .

---

#### **`Atomic` (`AtomicInteger`, `AtomicLong`, …)**

🔥  **Ưu điểm** :

✅  **Không cần dùng khóa thực sự** , sử dụng **CAS (Compare-And-Swap)** để thay đổi giá trị một cách an toàn.

✅ **Hiệu suất cao hơn tất cả các loại khóa khác** trong các thao tác trên biến đơn giản.

❌  **Nhược điểm** :

❌  **Chỉ hoạt động với kiểu dữ liệu nguyên thủy (int, long, boolean, …)** .

❌  **Không phù hợp nếu cần đồng bộ hóa nhiều biến hoặc cấu trúc dữ liệu phức tạp** .

📌  **Ứng dụng** :

✔ Khi chỉ cần **tăng/giảm một biến đơn giản** mà không cần khóa (`AtomicInteger.incrementAndGet()`).

✔ Khi muốn  **tối ưu hiệu suất mà vẫn đảm bảo an toàn dữ liệu** .

## Fair Mode

**Fair Mode** là một tùy chọn trong `ReentrantLock` và `ReentrantReadWriteLock` giúp đảm bảo **thứ tự công bằng** giữa các luồng. Khi một luồng chờ khóa, nó sẽ được **xếp hàng theo thứ tự yêu cầu** thay vì bị luồng khác vượt mặt (starvation). Khi bật  **Fair Mode** , các luồng  **sẽ lấy khóa theo thứ tự yêu cầu (FIFO - First In, First Out)** .

📌 **Cách bật Fair Mode:**

* `new ReentrantLock(true)` → Bật công bằng.
* `new ReentrantReadWriteLock(true)` → Bật công bằng cho  **ReadWriteLock** .

✅  **Dùng Fair Mode khi** :

* Cần **tránh starvation** (luồng bị bỏ qua mãi mãi).
* Có nhiều luồng  **đợi khóa trong thời gian dài** .
* Cần đảm bảo  **công bằng giữa các luồng** .

❌  **Không nên dùng nếu** :

* Hiệu suất quan trọng hơn công bằng ( **Fair Mode chậm hơn** ).
* Muốn tận dụng khả năng giành khóa nhanh của non-fair mode.
* Chỉ có một số ít luồng truy cập khóa.

## Các cơ chế đồng bộ luồng

| Cơ chế                                    | Cách hoạt động                                                                  | Khi nào dùng                                                                                                                                         | Ưu điểm                                                             | Nhược điểm                                                                              |
| ------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **CountDownLatch**                    | Chờ số lượng luồng kết thúc trước khi tiếp tục                           | Khi có**số lượng luồng cố định**cần hoàn thành trước khi tiếp tục (VD: tải tài nguyên xong rồi khởi chạy ứng dụng)        | Đơn giản, hiệu quả                                                | Chỉ dùng**một lần**(không reset lại được)                                    |
| **CyclicBarrier (xi click be ri ờ)** | Chờ tất cả luồng đến checkpoint trước khi tiếp tục                        | Khi**cần đồng bộ nhiều luồng**tại một điểm chung (VD: tất cả luồng phải sẵn sàng trước khi tiếp tục xử lý tiếp theo)      | Có thể**tái sử dụng**(khác `CountDownLatch`)             | Nếu một luồng bị chậm,**tất cả phải chờ**                                    |
| **Phaser**                            | Đồng bộ theo**nhiều giai đoạn** , có thể thêm/xóa luồng linh hoạt | Khi có**nhiều phase (giai đoạn)**cần đồng bộ (VD: nhiều bước trong pipeline xử lý dữ liệu)                                              | Linh hoạt hơn `CyclicBarrier`, có thể**thêm bớt luồng** | Phức tạp hơn `CyclicBarrier`                                                           |
| **Semaphore (sém mơ phò)**         | Giới hạn số lượng luồng có thể truy cập tài nguyên cùng lúc            | Khi cần**kiểm soát truy cập tài nguyên giới hạn**(VD: giới hạn số lượng kết nối database, số lượng thread truy cập vào file) | Hiệu quả khi giới hạn số luồng truy cập tài nguyên            | Không đảm bảo đồng bộ giữa các luồng như `CountDownLatch`hay `CyclicBarrier` |

### CountDownLatch – Chờ nhiều luồng hoàn thành trước khi tiếp tục

Chỉ dùng latch được 1 lần, ko dùng lại được. Luồng chín sẽ gọi await

CountDownLatch  **không ngăn luồng chạy song song** . Nó chỉ làm nhiệm vụ **đếm ngược** và **chặn** những luồng gọi `await()` đến khi `countDown()` được gọi đủ số lần.

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3); // Cần 3 countDown()

        for (int i = 1; i <= 3; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " đang chạy...");
                try {
                    Thread.sleep((long) (Math.random() * 3000)); // Giả lập xử lý mất thời gian
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " hoàn thành.");
                latch.countDown(); // Giảm đếm xuống 1
            }).start();
        }

        System.out.println("Chờ tất cả luồng hoàn thành...");
        latch.await(); // Chặn luồng chính cho đến khi countDown() đủ 3 lần
        System.out.println("Tất cả luồng đã xong, tiếp tục thực thi!");
    }
}


```

### CyclicBarrier – Đồng bộ tất cả luồng tại checkpoint

`CyclicBarrier` là một cơ chế đồng bộ giúp **tất cả luồng phải chờ nhau tại một điểm chung (checkpoint)** trước khi tiếp tục.

Dùng lại barrier nhiều lần. Khi một luồng gọi `await()`, nó sẽ **chờ** các luồng khác cũng gọi `await()`.

```java
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("Tất cả luồng đã đến checkpoint!, phải có 3 luồng gọi await() trước khi tiếp tục."));

        for (int i = 1; i <= 3; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " đến checkpoint.");
                try {
                    barrier.await(); // Chờ tất cả 3 luồng đến
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}

```

### Phaser – Đồng bộ theo nhiều giai đoạn

```java
import java.util.concurrent.Phaser;

public class PhaserExample {
    public static void main(String[] args) {
        Phaser phaser = new Phaser(3); // Có 3 luồng cần đồng bộ

        for (int i = 1; i <= 3; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " hoàn thành Phase 1.");
                phaser.arriveAndAwaitAdvance(); // Chờ tất cả luồng hoàn thành Phase 1

                System.out.println(Thread.currentThread().getName() + " hoàn thành Phase 2.");
                phaser.arriveAndAwaitAdvance(); // Chờ tất cả luồng hoàn thành Phase 2
            }).start();
        }
    }
}

```

### **Semaphore** - Giới hạn số lượng luồng có thể truy cập tài nguyên cùng lúc

```java
import java.util.concurrent.Semaphore;

class PrintQueue {
    private final Semaphore semaphore;

    public PrintQueue(int printers) {
        this.semaphore = new Semaphore(printers);
    }

    public void printDocument(String user) {
        try {
            System.out.println(user + " is waiting to print...");
            semaphore.acquire(); // Chờ nếu không có máy in trống
            System.out.println(user + " is printing a document.");
            Thread.sleep(2000); // Giả lập thời gian in
            System.out.println(user + " has finished printing.");
            semaphore.release(); // Giải phóng máy in cho người khác
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class SemaphoreExample {
    public static void main(String[] args) {
        PrintQueue printQueue = new PrintQueue(2); // Tối đa 2 máy in hoạt động cùng lúc

        for (int i = 1; i <= 5; i++) {
            String user = "User " + i;
            new Thread(() -> printQueue.printDocument(user)).start();
        }
    }
}
```

## An Toàn Luồng (Thread Safety)

**Định nghĩa:**

An toàn luồng có nghĩa là chương trình có thể chạy song song nhiều luồng mà không xảy ra  **race condition (tranh chấp dữ liệu)** , đảm bảo  **tính toàn vẹn dữ liệu** .

### Các Cơ Chế Đảm Bảo An Toàn Luồng

| **Cơ chế**                   | **Ưu điểm**                                                 | **Nhược điểm**                                                          | **Khi nào dùng?**                                         |
| ------------------------------------ | -------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **`synchronized`**           | Dễ sử dụng, khóa toàn bộ phương thức hoặc khối code       | Hiệu suất thấp, chặn luồng                                                   | Khi cần đơn giản và không quan tâm hiệu suất             |
| **`ReentrantLock`**          | Kiểm soát khóa tốt hơn `synchronized`, hỗ trợ `tryLock()` | Cần `lock()`và `unlock()`, dễ bị deadlock                                 | Khi cần kiểm soát chi tiết việc khóa                        |
| **`Atomic Variables`**       | Không cần khóa, hiệu suất cao                                   | Chỉ hoạt động với kiểu dữ liệu đơn giản (`int`,`long`,`boolean`) | Khi chỉ cần cập nhật biến đơn giản (counter, flag)        |
| **`Concurrent Collections`** | Hiệu suất cao, không cần khóa toàn bộ                         | Tốn bộ nhớ hơn `HashMap`,`ArrayList`                                      | Khi dùng danh sách, hàng đợi trong môi trường đa luồng  |
| **`ThreadLocal`**            | Biến riêng cho từng luồng, không cần đồng bộ                | Khó quản lý bộ nhớ, dễ gây memory leak                                     | Khi mỗi luồng cần lưu dữ liệu riêng (Session, Transaction) |

### So Sánh Các Collections Trong Đa Luồng vs Không Đa Luồng

| **Loại Collection** | **Không Hỗ Trợ Đa Luồng** | **Hỗ Trợ Đa Luồng (Thread-Safe)**                           |
| -------------------------- | ------------------------------------ | --------------------------------------------------------------------- |
| **Queue**            | `LinkedList`,`ArrayDeque`        | `ConcurrentLinkedQueue`,`ConcurrentLinkedDeque`,`BlockingQueue` |
| **List**             | `ArrayList`,`LinkedList`         | `CopyOnWriteArrayList`                                              |
| **Set**              | `HashSet`,`TreeSet`              | `ConcurrentSkipListSet`,`CopyOnWriteArraySet`                     |
| **Map**              | `HashMap`,`TreeMap`              | `ConcurrentHashMap`,`ConcurrentSkipListMap`                       |

#### **Queue – Hàng Đợi Thread-Safe**

**✔ Dùng khi:** Cần một hàng đợi mà nhiều luồng có thể thêm/xóa phần tử mà không bị lỗi.

📌 **Thay thế cho `LinkedList` hoặc `ArrayDeque`** (vốn không an toàn luồng).

| **Queue Type**         | **Mô tả**                                                                              | **Khi nào dùng?**                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `ConcurrentLinkedQueue<E>` | Hàng đợi không chặn (non-blocking), sử dụng**CAS (Compare-And-Swap)**để tránh khóa. | Khi cần hiệu suất cao, nhiều luồng có thể thêm/xóa mà không chặn. |
| `ConcurrentLinkedDeque<E>` | Hàng đợi hai đầu không chặn (non-blocking deque).                                       | Khi cần thêm/xóa phần tử từ cả hai đầu danh sách.                   |
| `BlockingQueue<E>`         | Hàng đợi có thể chặn khi đầy hoặc rỗng.                                              | Khi cần kiểm soát chặt chẽ số lượng phần tử trong hàng đợi.      |

#### **List – Danh Sách Thread-Safe**

📌  **Thay thế cho `ArrayList`** , vì `ArrayList` không an toàn khi nhiều luồng cùng thêm/xóa dữ liệu.

| **List Type**         | **Mô tả**                                                                                 | **Khi nào dùng?**                          |
| --------------------------- | ------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| `CopyOnWriteArrayList<E>` | Khi ghi dữ liệu, một bản sao mới được tạo ra, đảm bảo an toàn mà không cần khóa. | Khi có nhiều luồng đọc nhưng ít luồng ghi. |

📌 **Lưu ý:** Vì nó sao chép dữ liệu mỗi khi cập nhật, nó **chậm hơn** `ArrayList` khi ghi dữ liệu nhiều lần.

#### **Set – Tập Hợp Thread-Safe**

📌  **Thay thế cho `HashSet`, `TreeSet`** , vì hai loại này không an toàn khi đa luồng.

| **Set Type**           | **Mô tả**                                                                     | **Khi nào dùng?**                                     |
| ---------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `ConcurrentSkipListSet<E>` | Sắp xếp phần tử tự động (giống `TreeSet`), hỗ trợ truy cập đồng thời. | Khi cần một tập hợp có thứ tự nhưng vẫn thread-safe. |
| `CopyOnWriteArraySet<E>`   | Tương tự `CopyOnWriteArrayList`, tạo bản sao mới khi thay đổi.              | Khi có nhiều luồng đọc nhưng ít thay đổi dữ liệu.  |

#### **Map – Bản Đồ Dữ Liệu Thread-Safe**

📌  **Thay thế cho `HashMap`, `TreeMap`** , vì `HashMap` không an toàn khi đa luồng (dễ bị lỗi race condition).

| **Map Type**              | **Mô tả**                                                                                                      | **Khi nào dùng?**                                 |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| `ConcurrentHashMap<K, V>`     | Chia khóa thành nhiều**segment** , mỗi segment có thể khóa riêng → Nhanh hơn `synchronized HashMap`. | Khi có nhiều luồng truy cập và cần hiệu suất cao. |
| `ConcurrentSkipListMap<K, V>` | Tương tự `TreeMap`, đảm bảo thứ tự các phần tử.                                                           | Khi cần Map có thứ tự nhưng vẫn thread-safe.        |

📌 **So sánh `ConcurrentHashMap` với `HashMap`**

* `HashMap` dễ bị lỗi khi nhiều luồng ghi cùng lúc → Có thể gây `ConcurrentModificationException`.
* `ConcurrentHashMap` chia nhỏ dữ liệu để giảm số lượng luồng bị chặn.

#### Atomic class

**Các Lớp Atomic Quan Trọng**

| **Lớp Atomic**       | **Dữ liệu quản lý**    | **Thay thế cho**                     |
| --------------------------- | -------------------------------- | ------------------------------------------- |
| `AtomicInteger`           | Một số nguyên (`int`)       | `volatile int`,`synchronized int`       |
| `AtomicLong`              | Một số nguyên lớn (`long`) | `volatile long`,`synchronized long`     |
| `AtomicBoolean`           | Một giá trị `true/false`    | `volatile boolean`                        |
| `AtomicReference<T>`      | Một tham chiếu đối tượng   | `volatile Object`,`synchronized Object` |
| `AtomicIntegerArray`      | Mảng `int[]`                  | `synchronized int[]`                      |
| `AtomicLongArray`         | Mảng `long[]`                 | `synchronized long[]`                     |
| `AtomicReferenceArray<T>` | Mảng đối tượng `T[]`      | `synchronized Object[]`                   |

📌 **Ưu điểm:** Không cần dùng `synchronized`, giúp truy cập dữ liệu an toàn và nhanh hơn trong môi trường đa luồng.

### CompletableFuture

`CompletableFuture` là một API mạnh mẽ trong Java  **8+** , giúp **xử lý tác vụ bất đồng bộ** mà không cần quản lý thủ công `Thread` hoặc `ExecutorService`.

📌 **Tại sao nên dùng?**

* ✅ **Viết code ngắn gọn hơn** so với `Future`.
* ✅ **Không cần chờ đợi thủ công** (`get()` trong `Future` có thể bị block).
* ✅ **Hỗ trợ callback** (`thenApply`, `thenAccept`, `thenRun`).
* ✅ **Dễ dàng kết hợp nhiều tác vụ song song** (`thenCombine`, `thenCompose`).
* ✅ **Hỗ trợ xử lý ngoại lệ** (`exceptionally`, `handle`).

#### **Tổng quan các phương thức của `CompletableFuture`**

| **Loại tác vụ**                         | **Phương thức**                                | **Mô tả**                                                                                               | **Khi nào sử dụng?**                                                                                                         |
| ------------------------------------------------ | ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Chạy tác vụ không có kết quả**    | `runAsync(Runnable)`                                  | Chạy một tác vụ bất đồng bộ mà**không có giá trị trả về**                                  | Khi chỉ cần thực hiện hành động như ghi log, gửi email, gọi API mà không cần kết quả                                   |
| **Chạy tác vụ có kết quả**           | `supplyAsync(Supplier<T>)`                            | Chạy một tác vụ bất đồng bộ và**trả về kết quả**                                             | Khi cần thực hiện một tác vụ có giá trị trả về như tính toán, lấy dữ liệu từ DB, gọi API                           |
| **Xử lý kết quả**                      | `thenApply(Function<T, R>)`                           | **Chuyển đổi giá trị**của `CompletableFuture`                                                     | Khi cần**biến đổi giá trị**nhận được từ một tác vụ trước                                                        |
|                                                  | `thenAccept(Consumer<T>)`                             | Nhận kết quả nhưng**không trả về gì**                                                             | Khi chỉ cần thực hiện hành động như ghi log, gửi thông báo, nhưng không cần thay đổi giá trị                        |
|                                                  | `thenRun(Runnable)`                                   | Chạy tiếp một tác vụ**mà không nhận giá trị trước đó**                                      | Khi chỉ cần chạy tiếp một tác vụ độc lập sau khi hoàn thành                                                               |
| **Kết hợp nhiều `CompletableFuture`** | `thenCombine(CompletableFuture, BiFunction<T, U, R>)` | Kết hợp kết quả từ hai `CompletableFuture` **độc lập**                                          | Khi cần tổng hợp dữ liệu từ hai nguồn khác nhau (ví dụ: API 1 trả về user, API 2 trả về order)                          |
|                                                  | `thenCompose(Function<T, CompletableFuture<R>>)`      | Dùng kết quả của `CompletableFuture`này để tạo `CompletableFuture`khác                             | Khi tác vụ sau**phụ thuộc**vào kết quả của tác vụ trước (ví dụ: lấy user xong rồi dùng userID để lấy order) |
| **Chạy nhiều tác vụ song song**        | `allOf(CompletableFuture...)`                         | Chạy**nhiều `CompletableFuture`cùng lúc**và**chờ tất cả hoàn thành**                    | Khi cần đợi tất cả tác vụ hoàn thành trước khi tiếp tục (ví dụ: tải nhiều tệp một lúc)                            |
|                                                  | `anyOf(CompletableFuture...)`                         | Chạy**nhiều `CompletableFuture`cùng lúc**và**trả về kết quả đầu tiên hoàn thành**   | Khi chỉ cần một kết quả đầu tiên (ví dụ: gửi request đến nhiều server và lấy response nhanh nhất)                    |
| **Xử lý lỗi**                           | `exceptionally(Function<Throwable, T>)`               | Bắt lỗi và**trả về giá trị thay thế**nếu có lỗi                                                | Khi muốn đảm bảo chương trình không bị crash nếu có lỗi xảy ra                                                           |
|                                                  | `handle(BiFunction<T, Throwable, R>)`                 | Xử lý cả**lỗi và kết quả**(có lỗi thì xử lý, không lỗi thì dùng giá trị bình thường) | Khi muốn kiểm tra lỗi nhưng vẫn sử dụng kết quả nếu không có lỗi                                                         |

📌 **Ghi chú:**

* Các phương thức có thể kết hợp để tạo pipeline xử lý bất đồng bộ hiệu quả.
* Sử dụng `ExecutorService` khi muốn kiểm soát số lượng luồng chạy trong `CompletableFuture`.

### ExecutorService

`ExecutorService` là một interface trong `java.util.concurrent` giúp quản lý **Thread Pool** hiệu quả.

* ✔ Thay vì tạo và quản lý từng `Thread` thủ công, `ExecutorService` giúp  **tái sử dụng luồng** , cải thiện hiệu suất.
* ✔ Hỗ trợ **bất đồng bộ** bằng cách chạy các tác vụ trong nền.
* ✔ Hỗ trợ  **lập lịch** ,  **giới hạn số luồng** ,  **tắt luồng khi không sử dụng** .

| **Phương thức**               | **Mô tả**                                                                | **Ưu điểm**                                           | **Nhược điểm**                                | **Khi nào dùng?**                                                                   |
| -------------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `newFixedThreadPool(n)`              | Tạo Thread Pool với `n`luồng cố định.                                    | Kiểm soát số luồng, tránh quá tải.                      | Có thể gây chờ đợi nếu tất cả luồng bận.     | Khi số lượng task ổn định.                                                            |
| `newCachedThreadPool()`              | Tạo Thread Pool có kích thước**động** .                             | Tạo luồng nhanh, xử lý tốt task ngắn hạn.               | Dễ gây quá tải nếu số luồng quá lớn.           | Khi task có tần suất thay đổi liên tục.                                              |
| `newSingleThreadExecutor()`          | Chỉ có**một luồng duy nhất** , chạy tuần tự.                       | Đảm bảo thứ tự thực thi, tránh race condition.          | Hiệu suất thấp nếu có nhiều task.                 | Khi cần chạy task theo thứ tự.                                                          |
| `newScheduledThreadPool(n)`          | Tạo Pool có thể lập lịch chạy task định kỳ.                             | Hỗ trợ**lập lịch chạy**sau một khoảng thời gian. | Tốn tài nguyên nếu nhiều task chạy liên tục.    | Khi cần chạy task định kỳ hoặc theo thời gian.                                       |
| `newSingleThreadScheduledExecutor()` | Tương tự `newScheduledThreadPool()`, nhưng chỉ có **1 luồng** .   | Đảm bảo các task chạy tuần tự theo lịch trình.        | Chạy chậm nếu có nhiều task cần lập lịch.       | Khi cần lên lịch task nhưng chỉ muốn chạy từng task một.                           |
| `newWorkStealingPool(n)`             | Tận dụng CPU đa nhân,**tự động đánh cắp task**giữa các luồng. | Tối ưu hiệu suất cho nhiều task nhỏ.                     | Không kiểm soát được số lượng luồng.          | Khi có**rất nhiều task nhỏ**cần xử lý đồng thời.                            |
| `newThreadPerTaskExecutor()`         | Tạo một luồng riêng cho**mỗi task** .                                 | Đơn giản, dễ dùng cho task độc lập.                    | Tốn tài nguyên hệ thống nếu có quá nhiều task. | Khi mỗi task cần chạy trên một luồng riêng.                                          |
| `newVirtualThreadPerTaskExecutor()`  | Tạo**Virtual Thread** , siêu nhẹ, có thể tạo hàng triệu luồng.    | Hiệu suất cao, không bị giới hạn số luồng.             | Chỉ hỗ trợ từ Java 19+.                             | Khi cần xử lý**rất nhiều task song song**mà không bị giới hạn tài nguyên. |

### CompletableFuture và ExecutorService

Kết hợp `CompletableFuture` với `ExecutorService` để  **tối ưu hiệu suất** , **kiểm soát số lượng luồng** và  **tùy chỉnh hành vi thực thi** . Dưới đây là các cách sử dụng phổ biến.

#### **Sử dụng `ExecutorService` trong `supplyAsync()` và `runAsync()`**

Mặc định, `CompletableFuture.supplyAsync()` và `CompletableFuture.runAsync()` sử dụng  **ForkJoinPool.commonPool().**

```java
import java.util.concurrent.*;

public class CompletableFutureWithExecutor {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName() + " - Fetching data...");
            return "Data received";
        }, executor);

        future.thenAccept(result -> 
            System.out.println(Thread.currentThread().getName() + " - Result: " + result)
        );

        executor.shutdown();
    }
}
```

#### Kết hợp nhiều tác vụ với ExecutorService

```java
import java.util.concurrent.*;

public class MultiTaskExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        CompletableFuture<String> task1 = CompletableFuture.supplyAsync(() -> {
            return "Task 1 completed";
        }, executor);

        CompletableFuture<String> task2 = CompletableFuture.supplyAsync(() -> {
            return "Task 2 completed";
        }, executor);

        CompletableFuture<String> task3 = CompletableFuture.supplyAsync(() -> {
            return "Task 3 completed";
        }, executor);

        CompletableFuture<Void> allTasks = CompletableFuture.allOf(task1, task2, task3);

        allTasks.join(); // Đợi tất cả tác vụ hoàn thành

        System.out.println(task1.get());
        System.out.println(task2.get());
        System.out.println(task3.get());

        executor.shutdown();
    }
}

```

#### Xử lý nhiều task và trả về list kết quả

```java
import java.util.List;
import java.util.concurrent.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class CompletableFutureListExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        // Tạo danh sách các CompletableFuture
        List<CompletableFuture<String>> futures = List.of(
            CompletableFuture.supplyAsync(() -> "Task 1 completed", executor),
            CompletableFuture.supplyAsync(() -> "Task 2 completed", executor),
            CompletableFuture.supplyAsync(() -> "Task 3 completed", executor)
        );

        // Chờ tất cả hoàn thành và thu thập kết quả vào List
        CompletableFuture<List<String>> allTasks = CompletableFuture
            .allOf(futures.toArray(new CompletableFuture[0])) // Chạy song song
            .thenApply(v -> futures.stream()
                .map(CompletableFuture::join) // Lấy kết quả từ mỗi CompletableFuture
                .collect(Collectors.toList()) // Thu thập vào danh sách
            );

        // Lấy kết quả cuối cùng
        List<String> results = allTasks.get();
        System.out.println(results);

        executor.shutdown();
    }
}

```

**Giải thích cách hoạt động**

1. **Tạo danh sách `CompletableFuture`** , mỗi tác vụ chạy song song trên `ExecutorService`.
2. **Sử dụng `CompletableFuture.allOf()`** để chờ tất cả các task hoàn thành.
3. **Dùng `thenApply()` để thu thập kết quả** :

* `futures.stream().map(CompletableFuture::join).collect(Collectors.toList())`
* `join()` lấy kết quả của từng `CompletableFuture` (tương tự `get()` nhưng không cần `throws Exception`).

**Sử dụng `.get()` trên `allTasks`** để lấy danh sách kết quả cuối cùng.

### Reflection

Reflection trong Java cho phép truy cập và thao tác với **class, field, method, constructor** trong  **runtime** , ngay cả khi không biết trước tên class hoặc method.

| **Tác vụ**           | **Reflection API**                                  |
| ---------------------------- | --------------------------------------------------------- |
| Lấy class của một object  | `Class<?> clazz = obj.getClass();`                      |
| Lấy danh sách fields       | `clazz.getDeclaredFields()`                             |
| Lấy danh sách methods      | `clazz.getDeclaredMethods()`                            |
| Truy cập field private      | `field.setAccessible(true); field.get(obj);`            |
| Gọi method private          | `method.setAccessible(true); method.invoke(obj, args);` |
| Tạo object bằng Reflection | `constructor.newInstance(args);`                        |
| Kiểm tra annotation         | `method.isAnnotationPresent(MyAnnotation.class);`       |

### Biến và static trong java

Trong Java, từ khóa `static` giúp  **gán thuộc tính hoặc phương thức cho class thay vì object** , có nghĩa là  **chúng thuộc về class chứ không phải instance** .

#### **Biến `static` (Static Variable)**

**📌 Đặc điểm:**

✔ Thuộc về **class** thay vì object.

✔ **Dùng chung** cho tất cả instance của class.

✔ Được tạo **một lần duy nhất** khi class được load vào bộ nhớ.

#### **Hàm `static` (Static Method)**

**📌 Đặc điểm:**

✔ Có thể gọi mà  **không cần tạo object** .

✔ **Chỉ có thể truy cập biến `static`** (không truy cập biến instance).

✔ Không thể dùng `this` hoặc `super` trong `static method`.

#### **Static Block (`static {}`)**

**📌 Đặc điểm:**

✔ Được chạy  **một lần duy nhất khi class được load vào bộ nhớ** .

✔ Dùng để  **khởi tạo biến `static` phức tạp** .

#### **Static Class (Nested Static Class)**

**📌 Đặc điểm:**

✔ Một class  **bên trong một class khác** , có từ khóa `static`.

✔  **Không thể truy cập biến non-static của outer class** .

✔ Có thể gọi trực tiếp mà không cần tạo object outer class.

#### **Sự khác nhau giữa `static` và non-static**

| Đặc điểm             | `static`                                                         | Non-`static`                       |
| ------------------------ | ------------------------------------------------------------------ | ------------------------------------ |
| Thuộc về               | **Class**                                                    | **Object (instance)**          |
| Truy cập                | Gọi trực tiếp bằng `ClassName.staticMethod()`                | Phải tạo object để gọi          |
| Biến                    | Dùng chung cho mọi object                                        | Mỗi object có bản sao riêng      |
| Dùng `this`/`super` | ❌ Không thể                                                     | ✅ Có thể                          |
| Khi nào dùng?          | **Hàm tiện ích** , biến dùng chung (counter, config...) | Khi mỗi object có dữ liệu riêng |

### Servlet

Servlet là một thành phần của **Java EE** dùng để xử lý **request từ client (thường là trình duyệt)** và trả về  **response từ server** .

### JDBC (Java Database Connectivity)

**JDBC** là  **API chuẩn của Java giúp kết nối và làm việc với database** .

📌 **JDBC giúp Java giao tiếp với database bằng cách gửi SQL và nhận kết quả.**

📌 **Có thể làm việc với nhiều loại database như MySQL, PostgreSQL, Oracle, SQL Server, v.v.**

📌 **Cần viết SQL thủ công để thao tác với dữ liệu.**

🔥 **Dùng JDBC khi:**

✅ Cần truy vấn dữ liệu từ database.

✅ Cần thao tác CRUD (Create, Read, Update, Delete).

✅ Cần hiệu suất cao hơn Hibernate/JPA trong một số trường hợp.

### **JPA (Java Persistence API)**

**Một chuẩn Java giúp lưu/truy vấn database bằng Object (ORM). Thường dùng với Hibernate (implementation phổ biến nhất).**

📌  **JPA KHÔNG phải là một thư viện/framework cụ thể** , mà nó là  **một giao diện chuẩn** . Các thư viện như **Hibernate, EclipseLink, OpenJPA** chính là các  **JPA implementation (triển khai)** .

📌  **JPA chỉ là quy chuẩn** , còn **Hibernate là thư viện cụ thể** giúp JPA hoạt động.

**JPA giúp gì cho lập trình viên?**

🔥 **Không cần viết SQL thủ công**

🔥 **Làm việc với database bằng Java object (Entity)**

🔥 **Tự động ánh xạ bảng → class, cột → thuộc tính**

🔥 **Dễ dàng chuyển đổi giữa các loại database (MySQL, PostgreSQL, Oracle, v.v.)**

🔥 **Tích hợp tốt với Spring Boot**

### Hibernate

**Hibernate** là  **một thư viện ORM mạnh mẽ cho Java** , giúp **làm việc với database mà không cần viết SQL thủ công** .

**Nó là một implementation (triển khai) của JPA** – nghĩa là Hibernate thực thi các quy tắc của JPA nhưng cũng có nhiều tính năng nâng cao hơn.
