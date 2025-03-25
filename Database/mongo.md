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
```

Lấy dữ liệu

```js
db.users.find() # Lấy tất cả
db.users.find({ age: { $gte: 25 } }) # Lấy user có age >= 25
db.users.findOne({ name: "Alice" }) # Lấy user đầu tiên có name = Alice
db.users.find({ _id: ObjectId("605c72f7e7e6f58f8c8f4f1d") })
```

Update

```js
db.users.updateOne({ name: "John" }, { $set: { age: 26 } })
db.users.updateMany({}, { $inc: { age: 1 } }) # Tăng age của tất cả user lên 1
```

Xóa

```js
db.users.deleteOne({ name: "John" })
db.users.deleteMany({ age: { $gt: 30 } }) # Xóa user có age > 30
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
db.articles.find({ $text: { $search: "\"MongoDB tutorial\"" } }) // MongoDB hỗ trợ tìm kiếm văn bản trên các field có text index.
db.articles.find({ $text: { $search: "MongoDB" } }) // (Tìm cụm từ chính xác "MongoDB tutorial")
```
