- **về Crontab**

Crontab (CRON TABLE) là một tiện ích cho phép thực hiện các tác vụ một cách tự động theo định kỳ. Crontab là một file chứa đựng bảng biểu (schedule) của các entries được chạy.

Bằng cách sử dụng các lệnh trong Linux Crontab ta có thể tạo những task chạy vào những giờ cụ thể đặt trước, như vào giờ nào trong ngày, vào giờ nào trong ngày vào thứ mấy trong tuần….

- **hoạt động**

Một cron schedule đơn giản là một text file. Mỗi người dùng có một cron schedule riêng, file này thường nằm ở /var/spool/cron. Crontab files không cho phép bạn tạo hoặc chỉnh sửa trực tiếp với bất kỳ trình text editor nào, trừ phi bạn dùng lệnh crontab.

**Một số lệnh thường dùng:**

```php
crontab -e: tạo,  chỉnh sửa các crontab
crontab -l: Xem các Crontab đã tạo
crontab -r: xóa file crontab
```

Hầu hết tất cả VPS đều được cài đặt sẵn crontab, tuy nhiên vẫn có trường hợp VPS không có. Nếu bạn sử dụng lệnh crontab -l mà thấy output trả lại -bash: crontab: command not found thì cần tự cài crontab thủ công.

- **Cài đặt crontab**

```none
yum install cronie
```

Start crontab và tự động chạy mỗi khi reboot:

```php
service crond start
chkconfig crond on
```



**Cấu trúc của crontab**

Một crontab file có 5 trường xác định thời gian, cuối cùng là lệnh sẽ được chạy định kỳ, cấu trúc như sau:

```php
Explain*     *     *     *     *     command to be executed
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- day of week (0 - 6) (Sunday=0)
|     |     |     +------- month (1 - 12)
|     |     +--------- day of month (1 - 31)
|     +----------- hour (0 - 23)
+------------- min (0 - 59)
```

**Ví dụ:**

**Chạy script 30 phút 1 lần**

```php
30 * * * * command
```



**Chạy script vào 3 giờ sáng mỗi ngày**

```php
0 3 * * * command
```

**Tạo một tác vụ hoạt động vào một giờ cụ thể**

- Backup dữ liệu website: vào ngày 07 tháng 10 lúc 08:11 AM

```php
11 08 10 07 * /home/framgia.vn/backup
```

**Tạo 1 tác vụ thực hiện 2 lần trong một ngày**

Backup dữ liệu website: 2 lần trong một ngày lúc 7:00 và 21:00 hàng ngày.

```php
00 07,21  * * * /home/framgia.vn/backup
```

**Tạo một tác vụ chỉ thực hiện vào các giờ cụ thể**

từ thứ 2 đến thứ 6

```php
 00 09-18 * * 1-5 /home/hostingaz.info/full-backup
```

#### Một số giá trị thời gian cho Crontab

```php
Explain| Keyword | Equivalent          |
|---------|---------------------|
| @yearly | 0 0 1 1 *           |
| @daily  | 0 0 * * *           |
| @hourly | 0 * * * *           |
| @reboot | chạy lúc khởi động. |
```



##### Tạo một tác vụ chạy vào phút đầu tiên của năm

```php
@yearly /home/framgia.vn/backup
```

##### Tạo một tác vụ chạy vào phút đầu tiên của tháng

```php
@monthly /home/framgia.vn/backup
```

**Tạo một tác vụ chạy khi khởi động lại**

```php
@reboot CMD
```