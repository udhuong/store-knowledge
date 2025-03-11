### Life cycle:



### Eloquent và Builder
- Eloquent ORM:
  - Làm việc với dữ liệu theo kiểu hướng đối tượng (OOP), ánh xạ mỗi bảng thành một Model.
  - Ưu điểm:
    - Giảm nguy cơ SQL Injection, Hỗ trợ Timestamps, Soft Delete, Mutators, Accessors.
    - sử dụng quan hệ dễ

- Query Builder
  - Viết truy vấn SQL bằng cách sử dụng các phương thức của Laravel mà không cần dùng Eloquent.
  - Ưu điểm:
    - Linh hoạt hơn khi truy vấn dữ liệu phức tạp.
    - Nhanh hơn Eloquent vì không cần ánh xạ dữ liệu thành object.
    - Hỗ trợ thao tác trên nhiều bảng dễ dàng. 
