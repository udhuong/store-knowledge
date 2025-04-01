## TypeScript là gì?

**Định nghĩa:**

* TypeScript là một ngôn ngữ lập trình mã nguồn mở được phát triển bởi Microsoft. Nó là một  **superset của JavaScript**
* Nghĩa là TypeScript mở rộng JavaScript bằng cách thêm các tính năng mới, đặc biệt là  **hệ thống kiểu tĩnh (static typing)** .
* TypeScript **không phải** là một ngôn ngữ biên dịch (compiled language) theo nghĩa truyền thống như C, C++ hoặc Java. vì nó không tạo ra mã máy (machine code) trực tiếp.
* **TypeScript không phải là ngôn ngữ thông dịch** như JavaScript, vì trình duyệt không hiểu TypeScript mà cần biên dịch trước.
* TypeScript là một ngôn ngữ **biên dịch sang JavaScript** (transpiled language).

**Đặc điểm:**

Trình biên dịch TypeScript (TS Compiler) sẽ chuyển mã TypeScript thành JavaScript thuần để chạy trên trình duyệt hoặc Node.js.

## Kiểu dữ liệu

### Kiểu dữ liệu nguyên thủy (Primitive Types)

```typescript
// number – Số nguyên và số thực
let age: number = 25;
let price: number = 99.99;

// string – Chuỗi ký tự
let name: string = "John Doe";

// boolean – Giá trị đúng/sai
let isActive: boolean = true;

// bigint – Số nguyên lớn (ES2020 trở lên)
let bigNumber: bigint = 9007199254740991n;

// symbol – Dùng để tạo giá trị duy nhất
let uniqueKey: symbol = Symbol("key");

// undefined – Biến chưa được gán giá trị
let notDefined: undefined = undefined;

// null – Giá trị rỗng
let emptyValue: null = null;
```


### Kiểu dữ liệu phức tạp (Reference Types)

```typescript
// Array<T> – Mảng
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];

// Tuple – Mảng có số lượng phần tử và kiểu dữ liệu cố định
let person: [string, number] = ["Alice", 30];

// Object – Đối tượng
let user: { name: string; age: number } = {
  name: "John",
  age: 25,
};

// Function – Hàm có kiểu dữ liệu
function sum(a: number, b: number): number {
  return a + b;
}
```

### Các kiểu dữ liệu đặc biệt

```typescript
// any – Bỏ qua kiểm tra kiểu dữ liệu
let data: any = "Hello";
data = 123; // Hợp lệ

// unknown – Tương tự any, nhưng an toàn hơn (cần kiểm tra kiểu trước khi sử dụng)
let input: unknown;
input = "Hello";
if (typeof input === "string") {
  console.log(input.toUpperCase());
}

// void – Dùng cho hàm không trả về giá trị
function logMessage(message: string): void {
  console.log(message);
}

// never – Dùng cho các hàm không bao giờ kết thúc hoặc luôn ném lỗi
function throwError(message: string): never {
  throw new Error(message);
}

// enum – Kiểu liệt kê
enum Status {
  Pending,
  InProgress,
  Done,
}
let taskStatus: Status = Status.InProgress;
```


### Các kiểu nâng cao


```typescript
// Union Type (|) – Chấp nhận nhiều kiểu dữ liệu
let id: number | string;
id = 123;
id = "abc";

// Intersection Type (&) – Kết hợp nhiều kiểu
type Employee = { name: string };
type Manager = { department: string };
let boss: Employee & Manager = { name: "John", department: "HR" };

// Type Alias – Định nghĩa kiểu dữ liệu tùy chỉnh
type User = { id: number; name: string };
let user1: User = { id: 1, name: "Alice" };

// Interface – Tương tự type, nhưng có thể mở rộng
interface IUser {
  id: number;
  name: string;
}
let user2: IUser = { id: 2, name: "Bob" };

// Literal Type – Giới hạn giá trị có thể nhận
let direction: "left" | "right" | "up" | "down";
direction = "left"; // Hợp lệ

// Optional & Readonly – Thuộc tính có thể không bắt buộc hoặc không thay đổi
interface Person {
  name: string;
  age?: number; // Optional
  readonly id: number; // Readonly
}
let p: Person = { name: "John", id: 1 };
// p.id = 2; // Lỗi
```

### Generics (Kiểu dữ liệu tổng quát)

Dùng để tạo các kiểu dữ liệu linh hoạt và tái sử dụng.

```typescript
function identity<T>(arg: T): T {
  return arg;
}
console.log(identity<number>(10));
console.log(identity<string>("Hello"));
```
