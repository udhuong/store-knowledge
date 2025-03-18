### Git convention

Format:

```txt
<type>(<scope>): <description>

Ví dụ:
feat(auth): add login functionality
fix(ui): resolve button alignment issue
```

* **`<type>`** : Loại thay đổi trong commit (bắt buộc)
  * `feat`: Tính năng mới
  * `fix`: Sửa lỗi
  * `chore`: Công việc lặt vặt (không ảnh hưởng đến code)
  * `docs`: Cập nhật tài liệu
  * `style`: Chỉnh sửa style/code formatting (không ảnh hưởng đến logic)
  * `refactor`: Tối ưu code nhưng không thay đổi chức năng
  * `test`: Thêm hoặc sửa test
  * `perf`: Cải thiện hiệu suất
  * `ci`: Thay đổi liên quan đến CI/CD
  * `build`: Thay đổi về build system hoặc dependencies
* **`<scope>`** : Phạm vi thay đổi (không bắt buộc)
* **`<description>`** : Mô tả ngắn gọn nội dung commit (bắt buộc)
