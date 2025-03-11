### Repository
> Repository Pattern — một mẫu thiết kế rất phổ biến khi làm việc với CSDL (database). Giúp tách biệt logic truy vấn dữ liệu khỏi logic xử lý nghiệp vụ.

Thường sẽ có 1 interface định nghĩa các method thao tác với db, sau đó sẽ có 1 class implement triển khai chi tết các method đó.

**Có 2 cách sử dụng:**
- class implement interface
- class extend abstract class và implement interface
  
**Cách 1:**
- tạo interface với các method create, update, findByIds, deleteById
- class implement các method đó

**Cách 2:**
- tạo 1 interface ClassARepository có các method riêng. VD: findByUsername
- tạo 1 abstract class BaseRepository
  + triển khai các method chung cơ bản. VD: create, update
  + có 1 method abstract getModel() (hoặc bất kì tên gì để gán model hoặc entity)
- class ClassARepositoryImpl extends BaseRepository impelement ClassARepository
  + triển khai method getModel(). Trả về model hoặc entity cụ thể.
  + triển khai các method riêng (findByUsername)
- như này ClassARepositoryImpl sẽ có cả 3 method, và k cần phải viết lại triển khai create và update  