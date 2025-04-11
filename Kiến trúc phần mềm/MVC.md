## Khái niệm

- MVC là viết tắt của Model-View-Controller, là một mẫu kiến trúc phần mềm
- được sử dụng phổ biến trong phát triển ứng dụng web.
- Nó giúp tách biệt logic xử lý, giao diện và luồng điều khiển, giúp mã dễ bảo trì và mở rộng.

## Thành phần

**Model (M) — Dữ liệu và logic xử lý**

- Vai trò: Đại diện cho dữ liệu và logic nghiệp vụ của ứng dụng.
- Nhiệm vụ: Xử lý truy xuất dữ liệu từ database, thực hiện các tính toán hoặc xử lý logic.

**View (V) — Giao diện hiển thị**

- Vai trò: Hiển thị thông tin cho người dùng và nhận đầu vào.
- Nhiệm vụ: Render giao diện dựa trên dữ liệu từ Model.

**Controller (C) — Bộ điều khiển**

- Vai trò: Điều phối luồng dữ liệu giữa Model và View.
- Nhiệm vụ: Nhận request từ người dùng, gọi Model để xử lý, và trả về View phù hợp.

```sh
my_php_mvc/
├── index.php                # Entry point của ứng dụng
├── controllers/
│   └── UserController.php   # Bộ điều khiển xử lý yêu cầu
├── models/
│   └── User.php             # Model xử lý dữ liệu
├── views/
│   ├── header.php           # Phần header chung
│   ├── user_list.php        # Giao diện danh sách user
│   └── footer.php           # Phần footer chung
```

Nguời dùng gửi request -> controller nhận request -> gửi tới model xử lý logic và thao tác DB -> trả kết quả vê controller -> controller trả kêế quả hiển thị qua view


## Khi dùng thêm **Service** và **Repository**

| Thành phần         | Vai trò chính                                                                                           |
| -------------------- | --------------------------------------------------------------------------------------------------------- |
| **Controller** | Nhận request từ client, gọi Service xử lý, và trả response.                                        |
| **Service**    | Chứa**logic nghiệp vụ**(business logic). Xử lý tính toán, validate, flow xử lý phức tạp. |
| **Repository** | Chỉ tập trung vào**lấy dữ liệu từ database**(CRUD). Không chứa logic.                      |
| **Model**      | Thường là đối tượng data (Entity), ánh xạ với database.                                         |

### Luồng xử lý chuẩn:

1. **Controller** nhận yêu cầu từ người dùng.
2. **Controller** gọi  **Service** .
3. **Service** xử lý nghiệp vụ, nếu cần dữ liệu thì gọi  **Repository** .
4. **Repository** thao tác với database và trả dữ liệu về  **Service** .
5. **Service** xử lý thêm (nếu cần), trả dữ liệu về  **Controller** .
6. **Controller** trả response cho người dùng.

### Ví dụ cụ thể:

Giả sử chức năng: **Đăng ký tài khoản**

**Controller** :

* Nhận request từ form đăng ký.
* Gọi `UserService.register(requestData)`.

**Service (UserService)** :

* Validate dữ liệu.
* Mã hóa password.

* Gọi `UserRepository.create(data)` để lưu user mới vào DB.

**Repository (UserRepository)** :

* Thực hiện query `INSERT INTO users (...) VALUES (...)`.
* Trả về user mới tạo.

**Model (User entity)** :

* Chỉ là object đại diện cho bảng `users`, chứa các property: `id`, `name`, `email`, `password`, ...
