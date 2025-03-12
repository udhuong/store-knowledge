**Có thread natively (hỗ trợ đa luồng thật sự)**
- Mỗi thread trong Java là một OS-level thread (luồng của hệ điều hành), chạy song song thực sự trên nhiều lõi CPU.
- Quản lý thread: Java cung cấp Thread Pool, ExecutorService, giúp tối ưu việc tạo và quản lý nhiều luồng.
- Nếu cần tính toán nặng, xử lý đa lõi CPU, Java với thread thật sự sẽ tối ưu hơn.