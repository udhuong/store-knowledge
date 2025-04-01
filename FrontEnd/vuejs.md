## **Vue là gì?**

**Định nghĩa:**

**Vue.js** là một **framework JavaScript** dùng để xây dựng giao diện người dùng (UI) và ứng dụng web  **SPA (Single Page Application)** .

**Đặc điểm:**

* **Reactive (Phản ứng nhanh)** → Khi dữ liệu thay đổi, giao diện cập nhật ngay lập tức.
* **Component-based (Dựa trên thành phần)** → Ứng dụng được chia nhỏ thành nhiều thành phần (component) để tái sử dụng.
* **Virtual DOM** → Vue cập nhật UI hiệu quả mà không cần thao tác trực tiếp với DOM thật.
* **Two-way Data Binding** → Dữ liệu giữa `model` và `view` đồng bộ tự động.
* **Easy to integrate (Dễ tích hợp)** → Có thể sử dụng độc lập hoặc tích hợp vào dự án có sẵn.

## Lifecycle

![alt](https://vuejs.org/assets/lifecycle.MuZLBFAS.png)


Vue có các giai đoạn chính:

1. **Khởi tạo (Creation)**
2. **Render (Mounting)**
3. **Cập nhật (Updating)**
4. **Hủy bỏ (Unmounting)**

Các hook quan trọng trong Vue 3

| Hook                  | Khi nào chạy?                      | Ứng dụng                     |
| --------------------- | ------------------------------------ | ------------------------------ |
| `setup()`           | Trước khi component được tạo   | Khởi tạo biến, gọi API     |
| `onBeforeMount()`   | Trước khi component render ra DOM  | Xử lý trước khi hiển thị |
| `onMounted()`       | Khi component đã render xong       | Gọi API, thao tác với DOM   |
| `onBeforeUpdate()`  | Trước khi component cập nhật DOM | Kiểm tra dữ liệu thay đổi |
| `onUpdated()`       | Khi DOM đã cập nhật xong         | Xử lý sau khi update         |
| `onBeforeUnmount()` | Trước khi component bị xóa       | Cleanup event, clear interval  |
| `onUnmounted()`     | Khi component bị xóa khỏi DOM     | Cleanup tài nguyên           |

## Composable functions

Composable Functions là các hàm tái sử dụng được viết bằng Composition API trong Vue 3.

**Khi nào nên dùng Composable Functions?**

* Khi **có logic chung lặp lại** trong nhiều component
* Khi **cần tách biệt logic ra khỏi UI** để dễ bảo trì

* Khi **cần tái sử dụng logic** ở nhiều nơi trong ứng dụng

```typescript
// src/composables/useCounter.ts
import { ref } from "vue";

export function useCounter() {
  const count = ref(0);

  const increase = () => {
    count.value++;
  };

  return { count, increase };
}

```

```typescript
<script setup>
import { useCounter } from "@/composables/useCounter";

const { count, increase } = useCounter();
</script>

<template>
  <p>Số lần click: {{ count }}</p>
  <button @click="increase">Tăng</button>
</template>

```

## Prop & Emit

Prop (Properties) là cách cha truyền dữ liệu xuống con trong Vue.

Emit dùng để component con gửi sự kiện lên cha.

### **So sánh Prop vs Emit**

| **Thuộc tính**    | **Prop**                       | **Emit**                           |
| ------------------------- | ------------------------------------ | ---------------------------------------- |
| **Hướng truyền** | **Từ cha → con**             | **Từ con → cha**                 |
| **Mục đích**     | Truyền dữ liệu từ cha xuống con | Con gửi sự kiện lên cha              |
| **Dùng trong**     | `defineProps()`                    | `defineEmits()`                        |
| **Ví dụ**         | `<Child :message="text" />`        | `<Child @increase="handleIncrease" />` |

## EventBus

Định nghĩa:

**EventBus** là một **cơ chế truyền sự kiện** giữa các component  **không có quan hệ cha-con** .

**Vấn đề:** Trong Vue,  **`props` và `emit` chỉ hoạt động giữa cha-con** . Nhưng nếu hai component  **không liên quan** , ta cần một cách khác để giao tiếp → **EventBus là giải pháp!**

Sử dụng mitt (thư viện event bus nhẹ cho Vue 3)

## Proivde và Inject

Định nghĩa:

**provide/inject** giúp truyền dữ liệu từ  **component cha xuống component con nhiều cấp** , mà  **không cần dùng prop** .

**Lợi ích:**

* Truyền dữ liệu xuống nhiều cấp component dễ dàng
* Giúp **code gọn hơn** so với dùng `props`
* Phù hợp với **dữ liệu chung** (config, theme, trạng thái, v.v.)

Sử dụng

```typescript
// Cha
const message = ref("Xin chào từ cha!");
provide("sharedMessage", message);

// Con
const message = inject("sharedMessage");
```

### Kết hợp provide/inject với reactive data

Nếu giá trị truyền xuống là  **`ref` hoặc `reactive`** , thì component con  **có thể thay đổi nó** .

### So sánh provide/inject với prop & Vuex/Pinia

| **Tính năng**       | **prop/emit**                     | **provide/inject**                    | **Vuex/Pinia**        |
| --------------------------- | --------------------------------------- | ------------------------------------------- | --------------------------- |
| **Truyền dữ liệu** | Cha → Con (trực tiếp)                | Cha → Con nhiều cấp                      | Mọi nơi                   |
| **Độ linh hoạt**   | Cố định theo cấp component          | Có thể truy cập từ nhiều component con | Global                      |
| **Dễ sử dụng**     | Dễ nhưng rối khi nhiều cấp         | Gọn, dễ dùng                             | Phải setup store           |
| **Khi nào dùng?**   | Dữ liệu ngắn gọn, cha-con gần nhau | Dữ liệu dùng chung ở nhiều cấp        | Quản lý trạng thái lớn |

### **Khi nào dùng provide/inject?**

Khi **cần truyền dữ liệu nhiều cấp** mà **không muốn props quá dài**

Khi **có dữ liệu dùng chung** nhưng **không cần Vuex/Pinia**

Khi **viết component library** (VD: truyền theme, config)

**Tóm lại:**

* **props** tốt cho giao tiếp **cha → con** (1 cấp)
* **provide/inject** giúp truyền dữ liệu **xuống nhiều cấp**
* **Vuex/Pinia** phù hợp cho **state toàn cục**

## Vuex & Pinia

Vuex và Pinia đều là **thư viện quản lý state** trong Vue.js, giúp **lưu trữ & quản lý trạng thái ứng dụng** một cách hiệu quả.

**Vuex** là thư viện **quản lý state cũ** (Vue 2 & Vue 3 hỗ trợ nhưng phức tạp).

**Pinia** là **thế hệ mới của Vuex** (Vue 3 khuyên dùng, dễ hơn, tối ưu hơn).

### **Tại sao cần Vuex/Pinia?**

Khi ứng dụng lớn dần, việc truyền dữ liệu giữa component bằng `props` & `emit` trở nên  **khó kiểm soát** .

**Vuex/Pinia giúp chia sẻ dữ liệu giữa nhiều component dễ dàng** mà không cần truyền `props` qua nhiều cấp.

## Directive

| Directive                  | Mô tả                                                |
| -------------------------- | ------------------------------------------------------ |
| `v-bind`                 | Gán giá trị động cho thuộc tính HTML            |
| `v-model`                | Ràng buộc dữ liệu hai chiều (`two-way binding`) |
| `v-if / v-else / v-show` | Điều kiện hiển thị component                      |
| `v-for`                  | Lặp qua danh sách                                    |
| `v-on`hoặc `@`        | Lắng nghe sự kiện (`@click`,`@input`)           |
| `v-slot`                 | Sử dụng slot trong component                         |
| `v-html`                 | Render HTML động                                     |
| `v-text`                 | Thay thế nội dung văn bản                          |

## Router (Vue Router – Điều hướng trang)

**Vue Router** giúp tạo Single Page Application (SPA) với nhiều trang mà không cần reload.

## Slots (Truyền nội dung vào component)

**Slots** giúp truyền nội dung từ component cha vào component con linh hoạt hơn.

## Teleport (Đưa component ra ngoài vị trí cha)

**Teleport** giúp **render một phần tử ra ngoài cấu trúc DOM gốc** (thường dùng cho modal, tooltip).

```typescript
<template>
  <button @click="show = true">Mở Modal</button>
  <Teleport to="body">
    <div v-if="show" class="modal">
      <p>Nội dung modal</p>
      <button @click="show = false">Đóng</button>
    </div>
  </Teleport>
</template>

<script setup>
import { ref } from "vue";
const show = ref(false);
</script>

```

## KeepAlive (Giữ trạng thái component khi chuyển trang)

**Giữ nguyên trạng thái của component khi thay đổi route** .

```typescript
<keep-alive>
  <router-view />
</keep-alive>

```

## Suspense (Xử lý component async)

Hiển thị loading trong khi chờ component async load xong.

```typescript
<Suspense>
  <template #default>
    <AsyncComponent />
  </template>
  <template #fallback>
    <p>Loading...</p>
  </template>
</Suspense>

```
