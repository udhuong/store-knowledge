Cú pháp của **Cronjob** thường được dùng để lập lịch chạy một lệnh hoặc script vào một thời điểm nhất định trong Linux/Unix. Nó có cú pháp như sau:

```txt
* * * * * command-to-be-executed
│ │ │ │ │
│ │ │ │ └─── Thứ trong tuần (0 - 7) (Chủ Nhật là 0 hoặc 7)
│ │ │ └───── Tháng (1 - 12)
│ │ └─────── Ngày trong tháng (1 - 31)
│ └───────── Giờ (0 - 23)
└─────────── Phút (0 - 59)
```

Ví dụ

| Cú pháp Cron  | Ý nghĩa                                                   |
| --------------- | ----------------------------------------------------------- |
| `0 0 * * *`   | Chạy vào**0 giờ 0 phút mỗi ngày**               |
| `*/5 * * * *` | Chạy**mỗi 5 phút**                                 |
| `0 */2 * * *` | Chạy**mỗi 2 giờ**(vào phút 0)                    |
| `0 8 * * 1`   | Chạy**8h sáng mỗi thứ Hai**                       |
| `30 22 1 * *` | Chạy**22h30 ngày 1 mỗi tháng**                    |
| `0 0 1 1 *`   | Chạy**vào nửa đêm ngày 1 tháng 1 (năm mới)** |

### Một số ký tự đặc biệt:

* `*` — mọi giá trị (vd: * ở phút tức là mỗi phút)
* `,` — phân tách nhiều giá trị (vd: `1,15,30`)
* `-` — chỉ khoảng giá trị (vd: `1-5` nghĩa là từ thứ Hai đến thứ Sáu)
* `/` — bước nhảy (vd: `*/10` nghĩa là mỗi 10 phút)

### Ví dụ nâng cao:

* `*/15 9-17 * * 1-5` → Mỗi 15 phút, từ 9h đến 17h, từ thứ Hai đến thứ Sáu
* `0 12 */2 * *` → Chạy lúc 12 giờ trưa, **mỗi 2 ngày**

## File cấu hình

Mặc định, các cấu hình **cron jobs** trong hệ thống Linux/Unix (bao gồm cả Alpine Linux trong Docker) được lưu trong các file sau:

**/etc/crontab:**

* Đây là file cấu hình cron system-wide (toàn hệ thống).
* Chứa các cron jobs được chạy bởi các người dùng khác nhau trên hệ thống, bao gồm cả các cron jobs được chạy bởi `root` và các user khác.

**/etc/cron.d/:**

* Đây là thư mục chứa các file cron job có thể được thêm vào để thực thi các công việc theo lịch.
* Các file trong thư mục này cũng có định dạng tương tự như trong `/etc/crontab` nhưng được phân tách ra thành các file riêng biệt.

**/var/spool/cron/crontabs:**

* Đây là nơi lưu trữ các file cron của người dùng. Mỗi người dùng trên hệ thống (bao gồm root) có thể có một file cron riêng tại đây.
* Các file cron này chỉ chứa cron jobs của một người dùng cụ thể và thường không có thông tin về người dùng hoặc quyền truy cập root.
* Đường dẫn chính xác sẽ là `/var/spool/cron/crontabs/<username>` với `<username>` là tên người dùng (ví dụ: `www-data`, `root`, v.v.).

**Tóm lại:**

* Các **cron jobs** có thể được cấu hình trong `/etc/crontab`, thư mục `/etc/cron.d/`, hoặc các file cron riêng của từng người dùng trong `/var/spool/cron/crontabs/`.
* Đối với Docker, nếu bạn sử dụng phương pháp sao chép cron job vào `/etc/cron.d/` trong container, cron job sẽ được lưu trữ trong thư mục `/etc/cron.d/`.
