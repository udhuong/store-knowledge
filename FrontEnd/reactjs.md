## ReactJS là gì?

**Định nghĩa:**

**ReactJS** là một thư viện JavaScript mã nguồn mở, được phát triển bởi  **Facebook (Meta)** , dùng để xây dựng giao diện người dùng ( **UI - User Interface** ), đặc biệt là các ứng dụng web  **đơn trang (SPA - Single Page Application)** .

**Tại sao nên dùng ReactJS?**

* Component-Based (Hướng thành phần) → Code dễ quản lý, tái sử dụng cao.
* Virtual DOM → Cập nhật UI nhanh hơn, tối ưu hiệu suất.
* One-Way Data Binding (Ràng buộc dữ liệu một chiều) → Tránh lỗi không mong muốn.
* Hỗ trợ Server-Side Rendering (SSR) với Next.js → Tối ưu SEO.
* Cộng đồng lớn → Nhiều thư viện hỗ trợ như Redux, React Query, Tailwind, v.v.

## Lifecycle

![alt](https://images.viblo.asia/c3c37d71-9a8f-4250-b7a3-d01cb1cc525e.png)


3 giai đoạn chính:

* **Mounting (Giai đoạn khởi tạo)**
* **Updating (Giai đoạn cập nhật)**
* **Unmounting (Giai đoạn hủy bỏ)**

### **React Lifecycle**

| **Giai đoạn** | **Class Component**  | **Function Component (useEffect)**        |
| --------------------- | -------------------------- | ----------------------------------------------- |
| **Mounting**    | `componentDidMount()`    | `useEffect(() => {...}, [])`                  |
| **Updating**    | `componentDidUpdate()`   | `useEffect(() => {...}, [deps])`              |
| **Unmounting**  | `componentWillUnmount()` | `useEffect(() => { return () => {...} }, [])` |

Function Component dùng `useEffect()` để thay thế lifecycle trong Class Component

### **Khi nào sử dụng lifecycle?**

| **Tình huống**                            | **Dùng Hook nào?**                      |
| ------------------------------------------------- | ----------------------------------------------- |
| Lấy dữ liệu API khi component mount            | `useEffect(() => {...}, [])`                  |
| Theo dõi giá trị của `state`hoặc `props` | `useEffect(() => {...}, [state])`             |
| Xóa event listener khi component unmount         | `useEffect(() => { return () => {...} }, [])` |
