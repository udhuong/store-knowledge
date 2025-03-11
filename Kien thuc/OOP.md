## OOP
1. Tính đóng gói (Encapsulation)
  > Đóng gói dữ liệu và hành vi vào cùng một lớp, đồng thời che giấu thông tin bên trong bằng cách giới hạn quyền truy cập. 

  - Cách triển khai: Sử dụng các access modifier như private, public, protected, và cung cấp các method getter, setter.
  - 🎯 Lợi ích: Bảo vệ dữ liệu, dễ bảo trì!
  
2. Tính kế thừa (Inheritance)
  > Cho phép một lớp con kế thừa thuộc tính và method của lớp cha để tái sử dụng mã và mở rộng chức năng.  

  - Cách triển khai: Dùng từ khóa extends.
  - 🎯 Lợi ích: Tái sử dụng mã, giảm trùng lặp!

3. Tính đa hình (Polymorphism)
  > Cho phép một hành động có thể thực hiện theo nhiều cách khác nhau.

  - Method Overloading (Nạp chồng): Cùng tên method, khác tham số.
  - Method Overriding (Ghi đè): Lớp con triển khai lại method của lớp cha.
  - 🎯 Lợi ích: Linh hoạt, dễ mở rộng!

4. Tính trừu tượng (Abstraction)
  > Ẩn đi chi tiết triển khai, chỉ hiển thị phần cần thiết.
  
  - Abstract class: Lớp trừu tượng có thể chứa method trừu tượng (không có thân hàm).
  - Interface: Giao diện chỉ chứa các method trừu tượng
  - 🎯 Lợi ích: Giảm độ phức tạp, tập trung vào logic cốt lõi!
<br>

## Interface và Abstract class
Interface:
- là một bản thiết kế (blueprint) mà các lớp khác có thể triển khai (implement)
- chỉ chứa các hằng số, các khai báo public method, không có phần triển khai (implementation)
- khi 1 class implement interface đó phải triển khai chi tiết method đó
- ko có thuộc tính
- 1 class có thể implement nhiều interface

Abstract Class:
- là một class trừu tượng, có thể chứa cả method trừu tượng (chưa triển khai) và method cụ thể (đã triển khai). 
- cho phép xây dựng một phần hành vi chung (method chung) mà các class con có thể kế thừa và sử dụng.
- khi 1 class kế thừa sẽ triển khai các method abstract
- có thuộc tính
- 1 class chỉ có thể extend 1 abstract class
- nếu 1 abstract class kế thừa 1 abstract class thì có thể ko triển khai hết các abstract class

## Biến instance và biến class (biến có static)
**biến instance:**
- thuộc về từng đối tượng.
- truy cập: Qua đối tượng: object.variable
- mỗi đối tượng có một bản sao riêng.
- tồn tại khi đối tượng được tạo bằng new, mất đi khi đối tượng bị hủy
- dùng để lưu trữ trạng thái riêng của từng đối tượng (ví dụ: name, age).

**biến static:**
- thuộc về lớp, tất cả các đối tượng dùng chung một bản sao duy nhất.
- truy cập: Qua lớp hoặc đối tượng: Class.variable hoặc object.variable.
- chỉ có một bản duy nhất tồn tại suốt vòng đời chương trình.
- tồn tại từ lúc lớp được load vào bộ nhớ, mất đi khi chương trình kết thúc hoặc class bị unload.
- dùng để lưu trữ thông tin dùng chung cho tất cả các đối tượng (ví dụ: species).

```java
public class Person {
    static String species = "Human"; // Biến lớp (static)
    String name;                     // Biến instance
}
```
`species` là `static`, nên mọi đối tượng đều dùng chung giá trị. Nếu thay đổi species, tất cả các đối tượng đều thấy sự thay đổi đó!

## overload, override và final
**overload:**
+ tạo ra nhiều phiên bản của cùng một phương thức trong cùng một lớp, nhưng với khác biệt về tham số truyền vào.
+ quy tắc Overload:
  + Tên phương thức giống nhau.
  + Danh sách tham số khác nhau (về số lượng, kiểu dữ liệu hoặc thứ tự tham số).
  + Không phụ thuộc vào kiểu trả về (nếu chỉ khác kiểu trả về sẽ lỗi).
+ Sử dụng Overload khi nào?
  + Khi bạn muốn tối ưu hóa tính linh hoạt của lớp, hỗ trợ nhiều trường hợp khác nhau.
  + Ví dụ: Hàm in ra màn hình có thể nhận nhiều kiểu dữ liệu khác nhau (số, chuỗi, đối tượng,...).

**override:**
+ là việc ghi đè lại phương thức của lớp cha trong lớp con, để cung cấp một triển khai cụ thể phù hợp với lớp con đó 
+ Quy tắc Override:
  + Tên phương thức giống nhau.
  + Danh sách tham số giống hệt lớp cha.
  + Kiểu trả về tương thích hoặc là kiểu con.
  + Phạm vi truy cập không được hẹp hơn lớp cha.
  + Không ghi đè phương thức final hoặc static.
+ Sử dụng Override khi nào?
  + Khi muốn tùy chỉnh hành vi của lớp con mà vẫn giữ giao diện chung với lớp cha.
  + Ví dụ: Các loài động vật đều có hành động phát ra âm thanh, nhưng tiếng kêu khác nhau.

**final:**
+ được sử dụng để ngăn chặn kế thừa hoặc ghi đè (override). 
+ có thể sử dụng final với để cố định biến, class hoặc method!
+ trong java có thể final với biến (ko thay đổi giá trị, như là const trong php, nếu là biến tham chiếu, thì địa chỉ không đổi, nhưng nội dung có thể thay đổi.)

## Composition và Inheritance
Composition là khi một lớp chứa một hoặc nhiều đối tượng của lớp khác như các thuộc tính của nó. Điều này giúp một đối tượng "bao gồm" các thành phần khác để sử dụng các chức năng hoặc dữ liệu của chúng.

Ví dụ: Một chiếc xe ô tô có động cơ, bánh xe, ghế ngồi, v.v.
Ở đây, ô tô được tạo thành từ nhiều thành phần nhỏ hơn — đây chính là Composition.

**So sánh Composition và Inheritance**

| **Tiêu chí**          | **Composition**                          | **Inheritance (Kế thừa)**      |
| --------------------- | ---------------------------------------- | ------------------------------ |
| **Mối quan hệ**       | "has-a" (có một)                         | "is-a" (là một)                |
| **Mức độ kết nối**    | Lỏng lẻo (**Loose coupling**)            | Chặt chẽ (**Tight coupling**)  |
| **Khả năng thay thế** | Dễ thay thế thành phần                   | Khó thay đổi hành vi lớp cha   |
| **Tái sử dụng mã**    | Dễ dàng tái sử dụng thành phần           | Phải mở rộng lớp con           |
| **Đa dạng hành vi**   | Linh hoạt, có thể thay đổi trong runtime | Cố định, phụ thuộc vào lớp cha |


## Câu hỏi thường gặp  
**OOP là gì? Nêu các đặc điểm chính?**  
- OOP (Object-Oriented Programming) là lập trình hướng đối tượng, giúp tổ chức mã theo mô hình đối tượng.  
- 4 đặc điểm chính:
  - Encapsulation (Đóng gói)
  - Inheritance (Kế thừa)
  - Polymorphism (Đa hình)
  - Abstraction (Trừu tượng)

**Sự khác biệt giữa Abstraction và Encapsulation?**  
- Abstraction: Ẩn chi tiết triển khai, chỉ hiển thị những gì cần thiết (dùng abstract class & interface).
- Encapsulation: Bảo vệ dữ liệu bằng cách giới hạn quyền truy cập (dùng private, protected, public).  
 
**Tại sao lại dùng Interface thay vì Abstract Class?**  
- Interface hỗ trợ đa kế thừa, giúp tách biệt hành vi mà nhiều lớp có thể chia sẻ.
- Abstract Class phù hợp khi cần một số phương thức có sẵn (code chung).

**Khi nào nên dùng kế thừa, khi nào nên dùng composition?**  
- Kế thừa khi có quan hệ "IS-A" (Ví dụ: Dog kế thừa Animal).
- Composition khi có quan hệ "HAS-A" (Ví dụ: Car có Engine).

**Class là gì? Object là gì?**  
- Class là khuôn mẫu định nghĩa thuộc tính & hành vi của đối tượng.
- Object là thể hiện cụ thể của một class.

**Constructor có thể bị override không? (Không!)**  
- Không, vì constructor không được kế thừa, chỉ có thể gọi super() trong constructor lớp con.
 
**Tính đa hình hoạt động như thế nào trong runtime?**  
- Thông qua method overriding, chương trình quyết định gọi phương thức của lớp con thay vì lớp cha.
 
**Có hỗ trợ đa kế thừa không? (Java thì không, nhưng hỗ trợ thông qua interface)** 
- Java không hỗ trợ đa kế thừa với class, nhưng có thể dùng interface để mô phỏng. 