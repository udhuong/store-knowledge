### java swing and javafx

cross platform: flutter, react, PHPNative


overview: concurrent package:

- khóa: synchronize, reentrant lock,  reentrance read write lock
- đồng bộ thời gian giữa các luồng: Phaser, CountDownLatch, Semaphore , CyclicBarrier
- class đa luồng:

  + Queue: ConcurrentLinkQueue, ConcurrentLinkDeque, BlockingQueue
  + List: CopyOnWriteArrayList
  + Set: ConcurrentSkipListSet, CopyOnWriteArraySet
  + Map: ConcurrentHashMap, ConcurrentSkipListMap
- atomic class
- Java Core:

  + Collection: Map, List, Set, Queue, Stack và các triển khai, phân biệt cách dùng của các class
  + Stream: khái niệm, cách dùng, các hàm có trong stream (lập trình hàm)
  + Hàm lamda, functional interface
  + Lập trình đa luồng, bất đồng bộ (e note trước đó r)
  + Reflection trong java (liên quan đến field, method, class)
  + Annotation (target, retention, thuộc tính)
  + Exception: Checked exception vs Uncheck Exception, ...
  + biến và hàm static
  + JDBC: Statement vs Prepare Statement, Connection, connection pool, ...
  + Servlet: khái niệm, mục đích, ...
  + JPA
- Spring:

  + Luồng chạy: từ Dispatcher Servlet -> ...
  + Khái niệm: Ioc, DI
  + Bean Factory, Application Context
  + Bean, scope của bean, các cách khởi tạo
  + Spring security: Luồng chạy, cách hoạt động
  + AOP: Khái niệm, khi nào dùng, Point cut, Jointpoint, Advice
  + SpEL
  + Spring data
  + các thể loại annotation trong spring
  + Testing
