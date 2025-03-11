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