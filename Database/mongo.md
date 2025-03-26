## Query redash

```json
{
  "collection": "sop_script_packages",
  "query": {
    "pkg_order": 1287981393,
    "script_type": 3,
    "deleted": null
  },
  "limit": 100
}


{
  "collection": "sop_script_conditions",
  "query": {
    "_id": { "$oid": "67cc00e79d656e47160245f0" }
  },
  "limit": 100
}


```

## Mongo

Mặc định, mỗi document trong MongoDB có một **_id** với kiểu dữ liệu  **ObjectId** .

```js
{ "_id": ObjectId("507f1f77bcf86cd799439011") }
```

**Độ dài 12 byte** , gồm:

* **4 byte** : Thời gian tạo.
* **5 byte** : ID máy/tiến trình.
* **3 byte** : Counter duy nhất.
* Tự động tạo khi không cung cấp `_id`.

**Tạo ObjectId thủ công**: ObjectId()

## Các kiểu dữ liệu

| Kiểu Dữ Liệu             | Mô Tả                                  |
| --------------------------- | ---------------------------------------- |
| **String**            | Chuỗi UTF-8                             |
| **Int32, Int64**      | Số nguyên                              |
| **Double**            | Số thực (dấu chấm động)            |
| **Boolean**           | `true`/`false`                       |
| **Array**             | Mảng dữ liệu                          |
| **Object (Document)** | Đối tượng JSON lồng nhau            |
| **ObjectId**          | ID duy nhất của MongoDB                |
| **Date**              | Ngày tháng                             |
| **Null**              | Giá trị rỗng                          |
| **Regex**             | Biểu thức chính quy                   |
| **BinData**           | Dữ liệu nhị phân (ảnh, file, video) |
| **Code**              | JavaScript Code                          |
| **Symbol**            | Chuỗi đặc biệt (hiếm dùng)         |
| **Decimal128**        | Số thực chính xác cao                |
| **MinKey & MaxKey**   | Giá trị nhỏ nhất/lớn nhất          |

### **Khi Nào Dùng Kiểu Dữ Liệu Nào?**

* **String** → Lưu tên, mô tả.
* **Number** → Lưu tuổi, giá cả, số lượng.
* **Boolean** → Lưu trạng thái bật/tắt.
* **Array** → Lưu danh sách (tag, sở thích).
* **Object** → Lưu quan hệ lồng nhau (địa chỉ, thông tin bổ sung).
* **ObjectId** → Làm khóa chính trong MongoDB.
* **Date** → Lưu ngày tạo, ngày cập nhật.
* **Decimal128** → Tính toán tài chính (tiền tệ).
* **BinData** → Lưu trữ file, ảnh.
* **Regex** → Tìm kiếm nâng cao.

## Các thao tác lệnh cơ bản

MongoDB không yêu cầu phải tạo collection trước, nó sẽ tự động tạo khi chèn dữ liệu. Tuy nhiên, cũng có thể tạo thủ công bằng lệnh:

```js
db.createCollection("users")
```

Chèn dữ liệu vào collection (nếu chưa có collection `users`, nó sẽ tự động tạo):

```js
db.users.insertOne({ name: "John Doe", age: 30 })
db.users.insertMany([
   { name: "Alice", age: 30, email: "alice@example.com" },
   { name: "Bob", age: 28, email: "bob@example.com" }
])
db.users.insertOne({ name: "Alice", createdAt: new Date() })
```

Lấy dữ liệu

```js
db.users.find() # Lấy tất cả
db.users.find({ age: { $gte: 25 } }) # Lấy user có age >= 25
db.users.findOne({ name: "Alice" }) # Lấy user đầu tiên có name = Alice
db.users.find({ _id: ObjectId("605c72f7e7e6f58f8c8f4f1d") })
```

**Tóm tắt cú pháp SQL vs MongoDB**

| **SQL**                                                                              | **MongoDB**                                                                                                      |
| ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `SELECT * FROM users;`                                                                   | `db.users.find();`                                                                                                   |
| `SELECT name, email FROM users;`                                                         | `db.users.find({}, { name: 1, email: 1, _id: 0 });`                                                                  |
| `SELECT * FROM users WHERE age > 25;`                                                    | `db.users.find({ age: { $gt: 25 } });`                                                                               |
| `SELECT * FROM users ORDER BY age DESC;`                                                 | `db.users.find().sort({ age: -1 });`                                                                                 |
| `SELECT * FROM users LIMIT 5;`                                                           | `db.users.find().limit(5);`                                                                                          |
| `SELECT * FROM users WHERE name LIKE 'A%';`                                              | `db.users.find({ name: { $regex: "^A", $options: "i" } });`                                                          |
| `SELECT COUNT(*) FROM users;`                                                            | `db.users.countDocuments();`                                                                                         |
| `SELECT * FROM users WHERE age IN (25, 30, 35);`                                         | `db.users.find({ age: { $in: [25, 30, 35] } });`                                                                     |
| `SELECT users.name, orders.amount FROM users JOIN orders ON users._id = orders.user_id;` | `db.users.aggregate([ { $lookup: { from: "orders", localField: "_id", foreignField: "user_id", as: "orders" } } ]);` |

Query field

```json
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

* `{}` → Không có điều kiện lọc.
* `{ name: 1, email: 1, _id: 0 }` → Chỉ lấy `name`, `email`, ẩn `_id`.

Update

```js
db.users.updateOne({ name: "John" }, { $set: { age: 26 } })
db.users.updateMany({}, { $inc: { age: 1 } }) // Tăng age của tất cả user lên 1
db.users.replaceOne({ name: "Alice" }, { name: "Alice", age: 27, email: "alice@example.com" }) // Thay thế toàn bộ dữ liệu của Alice)
```

Xóa

```js
db.users.deleteOne({ name: "John" })
db.users.deleteMany({ age: { $gt: 30 } }) # Xóa user có age > 30
```

Xóa toàn bộ collection

```js
db.users.drop()
```

## Index

Lấy index hiện tại

```js
db.users.getIndexes()
```

Tạo index

```js
db.users.createIndex({ name: 1 })
```

* `1` là **ascending index** (sắp xếp tăng dần).
* `-1` là **descending index** (sắp xếp giảm dần).

```js
db.users.find({ name: "Alice" }).explain("executionStats")
```

Index tìm kiếm văn bản

```js
db.articles.createIndex({ title: "text", content: "text" })
```

Index nhiều field

```js
db.users.createIndex({ name: 1, age: -1 })

db.users.find({ name: "Alice" })          # ✅ Sử dụng Index
db.users.find({ name: "Alice", age: 25 }) # ✅ Sử dụng Index
db.users.find({ age: 25 })                # ❌ Không sử dụng Index (chỉ hỗ trợ nếu tìm theo "name" trước)
```

Unique index

```js
db.users.createIndex({ email: 1 }, { unique: true })
```

TTL Index (Tự động xóa dữ liệu)

```js
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```

Sparse Index (Bỏ qua document thiếu field)

Mặc định, MongoDB index tất cả document, kể cả document  **không có field đó** . Nếu muốn chỉ index những document có field cụ thể, dùng `sparse: true`:

```js
db.users.createIndex({ phone: 1 }, { sparse: true })
```

Partial Index (Chỉ index một phần dữ liệu)

Nếu chỉ muốn index một số document thỏa mãn điều kiện nào đó:

```js
db.users.createIndex({ age: 1 }, { partialFilterExpression: { age: { $gt: 18 } } })
```

Xóa index

```js
db.users.dropIndex("tên index")
db.users.dropIndexes()
```

## Query

### Truy vấn Cơ Bản (Basic Queries)

```js
db.users.find({ name: "Alice" }) // truy vấn =
db.users.find({ age: { $gt: 18 } })
db.users.find({ age: { $lt: 30 } })
db.users.find({ age: { $gte: 18, $lte: 30 } })
db.users.find({ age: { $ne: 25 } })
db.users.find({ age: { $in: [20, 25, 30] } })
db.users.find({ age: { $nin: [20, 25, 30] } })
```

### Truy vấn với Toán Tử Logic ($and, $or, $not)

```js
db.users.find({ $and: [{ age: { $gte: 18 } }, { age: { $lte: 30 } }] })
db.users.find({ $or: [{ age: { $lt: 18 } }, { age: { $gt: 60 } }] })
db.users.find({ age: { $not: { $gte: 18 } } })
```

### Truy vấn Trên Field Dữ Liệu Đặc Biệt

```js
db.users.find({ email: { $exists: true } })
db.users.find({ age: { $type: "number" } })
```

### Truy vấn trên Mảng (Array Queries)

Tìm document có giá trị trong mảng

```js
db.users.find({ hobbies: "reading" }) // (Nếu user có hobbies: ["reading", "coding"], MongoDB sẽ tìm thấy)
```

Tìm document chứa tất cả giá trị trong mảng

```js
db.users.find({ hobbies: { $all: ["reading", "coding"] } }) // (Chỉ tìm user có cả reading và coding trong hobbies)
```

Tìm document có số lượng phần tử trong mảng

```js
db.users.find({ hobbies: { $size: 2 } }) // (Tìm user có hobbies chứa đúng 2 phần tử)
```

### Truy vấn Văn Bản (Text Search)

```js
db.articles.createIndex({ title: "text", content: "text" }) // đánh index text
db.articles.find({ $text: { $search: "MongoDB" } }) // MongoDB hỗ trợ tìm kiếm văn bản trên các field có text index.
db.articles.find({ $text: { $search: "\"MongoDB tutorial\"" } }) // (Tìm cụm từ chính xác "MongoDB tutorial")
```

### Truy vấn với Regex (Regular Expressions)

Dùng để tìm kiếm dữ liệu dạng chuỗi.

```js
db.users.find({ name: /^A/ })  // Tìm user có tên bắt đầu bằng 'A'
db.users.find({ name: /son$/ }) // Tìm user có tên kết thúc bằng 'son'
db.users.find({ name: /john/i }) // Không phân biệt hoa thường
```

### **Truy vấn với Aggregation (Nhóm & Xử lý dữ liệu)**

Aggregation giúp xử lý dữ liệu giống như `GROUP BY` trong SQL.

```js
// Tính tổng số lượng user theo tuổi
db.users.aggregate([
  { $group: { _id: "$age", total: { $sum: 1 } } }
])

// Tính tuổi trung bình của user
db.users.aggregate([
  { $group: { _id: null, avgAge: { $avg: "$age" } } }
])

// Lọc chỉ lấy user trên 18 tuổi rồi nhóm lại
db.users.aggregate([
  { $match: { age: { $gte: 18 } } },
  { $group: { _id: "$age", count: { $sum: 1 } } }
])
```

### Phân Trang và Sắp Xếp (Pagination & Sorting)

```js
// Sắp xếp theo tuổi (Tăng dần)
db.users.find().sort({ age: 1 })

// Sắp xếp theo tuổi (Giảm dần)
db.users.find().sort({ age: -1 })

// Phân trang (Skip & Limit)
db.users.find().skip(10).limit(5) // (Lấy 5 user, bỏ qua 10 user đầu tiên)
```

### Truy vấn lồng

MongoDB hỗ trợ lưu trữ một document bên trong document khác.

```json
{
  "name": "Alice",
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}

```

* Dùng để lưu thông tin có quan hệ.
* Truy vấn lồng nhau dễ dàng:

```js
db.users.find({ "address.city": "New York" })
```

## Quan hệ trong mongo

MongoDB là **NoSQL** nên không có hệ thống quan hệ ràng buộc chặt chẽ như  **SQL (MySQL, PostgreSQL)** . Tuy nhiên, MongoDB vẫn hỗ trợ **quan hệ giữa các bảng (collection)** theo hai cách chính:

1. **Embedded Documents** (Nhúng)
2. **Reference (Tham chiếu)**

### Embedded Documents (Nhúng)

Dữ liệu được lưu **trực tiếp bên trong document** thay vì tạo bảng riêng như SQL.

#### One-to-Many

```json
{
  "_id": ObjectId("1"),
  "name": "Alice",
  "addresses": [
    { "street": "123 Main St", "city": "New York" },
    { "street": "456 Elm St", "city": "Los Angeles" }
  ]
}

```

**Ưu điểm**

* Truy vấn nhanh, không cần `JOIN`.
* Dữ liệu được lưu trữ cùng nhau, giúp tăng hiệu suất.

**Nhược điểm**

* Dữ liệu trùng lặp nếu nhiều document có chung thông tin.
* Không phù hợp khi dữ liệu con thay đổi thường xuyên.

### Reference (Tham Chiếu)

Dữ liệu được tách thành **collection riêng** và dùng `_id` để liên kết.

#### One-to-Many

```json
// users
{
  "_id": ObjectId("1"),
  "name": "Alice",
  "addresses": [ ObjectId("101"), ObjectId("102") ]
}

// addresses
{
  "_id": ObjectId("101"),
  "street": "123 Main St",
  "city": "New York",
  "userId": ObjectId("1")
}
```

**Ưu điểm**

* Tránh trùng lặp dữ liệu.
* Phù hợp khi dữ liệu thay đổi thường xuyên.

**Nhược điểm**

* Truy vấn chậm hơn do cần `lookup`.
* Cần nhiều truy vấn hơn để lấy dữ liệu đầy đủ.

### Sử Dụng `$lookup` Để JOIN (Tham Chiếu)

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",   # Bảng cần JOIN
      localField: "_id", # Khóa chính (User ID)
      foreignField: "userId", # Khóa ngoại (User ID trong Orders)
      as: "user_orders"  # Tên field chứa kết quả
    }
  }
])

```

#### One-to-One

```js
db.users.aggregate([
  {
    $match: { _id: ObjectId("1") }  // Lọc theo user_id
  },
  {
    $lookup: {
      from: "profiles",  
      localField: "_id",  
      foreignField: "userId",  
      as: "profile" 
    }
  },
  { $unwind: "$profile" },  // Bỏ mảng kết quả
  { $project: { profile: 1, _id: 0 } }  // Ẩn các trường không cần thiết
])

```

 **Tips**

* Dùng `$unwind` nếu chỉ muốn lấy 1 phần tử trong mảng.
* Dùng `$match` trước `$lookup` để tăng hiệu suất.
* Dùng `$project` để ẩn bớt dữ liệu không cần thiết.

#### One-to-Many

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",  
      localField: "_id",  
      foreignField: "userId",  
      as: "orders"  
    }
  }
])
```

```json
[
  {
    "_id": ObjectId("1"),
    "name": "Alice",
    "orders": [
      { "_id": ObjectId("101"), "userId": ObjectId("1"), "total": 50 },
      { "_id": ObjectId("102"), "userId": ObjectId("1"), "total": 75 }
    ]
  }
]

```

#### Many-to-Many

Lấy tất cả nhóm của một user

```json
db.user_groups.aggregate([
  {
    $match: { userId: ObjectId("1") }  # Lọc theo user
  },
  {
    $lookup: {
      from: "groups",
      localField: "groupId",
      foreignField: "_id",
      as: "group_info"
    }
  },
  { $unwind: "$group_info" },  # Bỏ mảng kết quả
  { $project: { group_info: 1, _id: 0 } }  # Ẩn các trường không cần thiết
])
```

```json
[
  { "group_info": { "_id": ObjectId("10"), "groupName": "Developers" } },
  { "group_info": { "_id": ObjectId("20"), "groupName": "Designers" } }
]
```

Lấy danh sách User trong một Group

```json
db.user_groups.aggregate([
  {
    $match: { groupId: ObjectId("10") }
  },
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "user_info"
    }
  },
  { $unwind: "$user_info" },
  { $project: { user_info: 1, _id: 0 } }
])

```

```json
[
  { "user_info": { "_id": ObjectId("1"), "name": "Alice" } },
  { "user_info": { "_id": ObjectId("2"), "name": "Bob" } }
]
```

## Aggregation Framework

Aggregation Framework trong MongoDB là một công cụ mạnh mẽ giúp **xử lý và phân tích dữ liệu** bằng cách thực hiện nhiều bước biến đổi trên dữ liệu, tương tự như `GROUP BY` trong SQL.

* **Hoạt động theo pipeline (chuỗi các bước)**
* **Hỗ trợ nhóm dữ liệu, lọc, tính toán, sắp xếp**
* **Hiệu suất cao hơn find() khi xử lý dữ liệu phức tạp**

```js
db.collection.aggregate([
  { stage_1 },
  { stage_2 },
  ...
  { stage_n }
])

```

Dữ liệu đi qua từng bước (stage), mỗi bước biến đổi dữ liệu trước khi đưa sang bước tiếp theo.

Dưới đây là các bước quan trọng thường dùng:

| Stage                                                                                | Chức Năng                                          |
| ------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| `$match`                                                                           | Lọc dữ liệu (giống `WHERE`trong SQL)           |
| `$project`                                                                         | Chọn hoặc tính toán lại các trường dữ liệu |
| `$group`                                                                           | Nhóm dữ liệu (giống `GROUP BY`trong SQL)       |
| `$sort`                                                                            | Sắp xếp dữ liệu                                  |
| `$limit`                                                                           | Giới hạn số lượng kết quả                     |
| `$skip`                                                                            | Bỏ qua một số bản ghi đầu tiên                |
| `$unwind`                                                                          | Bóc tách phần tử trong mảng                     |
| `$lookup`                                                                          | JOIN với collection khác                           |
| `$count`                                                                           | Đếm số bản ghi                                   |
| `$addFields`                                                                       | Thêm hoặc cập nhật field mới                    |
| `$set`       | Giống `$addFields`, nhưng có thể dùng thay thế `$project` |                                                      |
| `$facet`                                                                           | Chạy nhiều pipeline đồng thời                   |
| `$merge`                                                                           | Lưu kết quả vào một collection khác            |

### **Khi Nào Dùng Aggregation Framework?**

**Nên Dùng Khi**

* Cần xử lý dữ liệu phức tạp (tính toán, nhóm, sắp xếp, lọc).
* Khi **truy vấn phải JOIN giữa nhiều bảng** (Dùng `$lookup`).
* Cần giảm tải dữ liệu trả về bằng `$project`, `$match`, `$limit`.
* Tạo **báo cáo thống kê** (Dùng `$group`, `$count`, `$sum`).

**Không Nên Dùng Khi**

* Chỉ cần tìm kiếm đơn giản (Dùng `find()` sẽ nhanh hơn).
* Dữ liệu thay đổi liên tục (Aggregation có thể làm chậm hiệu suất).
* Dữ liệu quá lớn mà không có **index tốt** (Gây chậm).

## $slice

`$slice` là **toán tử trong MongoDB** giúp  **cắt giảm số lượng phần tử trong một mảng** .

* Dùng trong **find()** và **aggregate()**
* Có thể lấy  **n phần tử đầu** ,  **n phần tử cuối** , hoặc **lấy từ vị trí bất kỳ**

```json
{ $slice: [ <mảng>, <số_lượng> ] }
{ $slice: [ <mảng>, <bắt_đầu>, <số_lượng> ] }
```

```js
{
  "_id": ObjectId("1"),
  "title": "MongoDB Guide",
  "comments": ["Great!", "Nice post!", "Very helpful!", "Awesome!", "Thanks!"]
}

db.posts.find({}, { comments: { $slice: 3 } })

{
  "_id": ObjectId("1"),
  "title": "MongoDB Guide",
  "comments": ["Great!", "Nice post!", "Very helpful!"]
}
```

```js
{
  "_id": ObjectId("1"),
  "name": "Alice",
  "orders": [
    { "orderId": 1001, "total": 50 },
    { "orderId": 1002, "total": 75 },
    { "orderId": 1003, "total": 100 }
  ]
}

db.users.aggregate([
  {
    $project: {
      name: 1,
      recentOrders: { $slice: ["$orders", -2] }
    }
  }
])

{
  "_id": ObjectId("1"),
  "name": "Alice",
  "recentOrders": [
    { "orderId": 1002, "total": 75 },
    { "orderId": 1003, "total": 100 }
  ]
}

```
