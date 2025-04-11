## Khái niệm

- MVVM là viết tắt của Model-View-ViewModel, một kiến trúc phần mềm phổ biến
- giúp tổ chức mã nguồn gọn gàng, dễ bảo trì và mở rộng.
- là một biến thể của mô hình MVC, thường dùng trong các ứng dụng có giao diện người dùng phức tạp

## Thành phần

**Model (M) — Dữ liệu và logic nghiệp vụ**

- Đại diện cho dữ liệu và logic nghiệp vụ.
- Xử lý thao tác với database, API hoặc nguồn dữ liệu khác.

**View (V) — Giao diện hiển thị**

- Chỉ tập trung vào hiển thị giao diện.
- Không chứa logic xử lý dữ liệu.
- Lắng nghe thay đổi từ ViewModel và cập nhật giao diện tương ứng.

**ViewModel  (VM) — Cầu nối giữa Model và View, xử lý logic giao diện**

- Kết nối View và Model.
- Chứa logic xử lý của giao diện (nhưng không biết gì về View cụ thể).
- Sử dụng data binding để tự động cập nhật View khi Model thay đổi. (Vuejs)
