# internship-trainning
### 1. Đóng gói image
Các hướng dẫn được thực hiện tại https://github.com/nhanhoadocs/create-images-openstack/blob/master/docs/Ubuntu2004.md
<details>
 
#  <summary> một số khái niệm liên quan </summary>
- LVM (Logical Volume Manager) là một hệ thống quản lý ổ đĩa trong Linux, cho phép tạo các phân vùng linh hoạt hơn so với việc sử dụng phân vùng tĩnh truyền thống (partition).
Với LVM, bạn có thể dễ dàng thay đổi kích thước, thêm, hoặc di chuyển các phân vùng mà không cần khởi động lại hệ thống.
- Image không dùng LVM có nghĩa là hệ điều hành hoặc ứng dụng trong image đó sử dụng phân vùng truyền thống (như ext4, xfs, v.v.) thay vì LVM để quản lý không gian đĩa.
Điều này thường dẫn đến việc quản lý phân vùng ít linh hoạt hơn, nhưng lại đơn giản hơn trong một số trường hợp, đặc biệt là khi không cần phải mở rộng hoặc thu hẹp các phân vùng sau khi hệ thống đã được triển khai.
- Queens là một phiên bản ổn định với nhiều cải tiến và tính năng mới, nhưng không còn nhận được cập nhật bảo mật hoặc hỗ trợ chính thức từ OpenStack Foundation,
vì đã có các phiên bản mới hơn. Nếu có thể, bạn nên xem xét việc nâng cấp lên một phiên bản mới hơn để nhận được các bản vá bảo mật và cải tiến hiệu năng.
Phiên bản này phù hợp với các môi trường sản xuất yêu cầu sự ổn định và tính năng tiên tiến trong các dịch vụ mạng, quản lý container và quản lý tài nguyên.
vì đã có các phiên bản mới hơn. Phiên bản này phù hợp với các môi trường sản xuất yêu cầu sự ổn định và tính năng tiên tiến trong các dịch vụ mạng, quản lý container và quản lý tài nguyên.
    
**tạo mới VM tại webvirtcloud**
- tạo WebvirtCloud cần tạo docker trước
- docker version được chỉnh sửa trong `sudo nano ./webvirtcloud.sh-`. Tạo ra version vượt mức yêu cầu nhưng vẫn cảnh báo lỗi thì sửa ở đoạn docker-version
``` 
 required_version="25.0.0"

## Compare versions using a simple logic
if [ "$(printf '%s\n' "$required_version" "$docker_version" | sort -V | head -n1)" = "$required_version" ]; then
    echo -e "\nDocker version $docker_version is sufficient.\n"
else
    echo -e "\nDocker version $docker_version is not sufficient. Please update Docker to version $required_version or later.\n" 
    exit 1
fi  
``` 
- bắt đầu web thì có mariadb, rabbitmq
- chạy docker ps để hiện container ID 
- chạy lệnh ` docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container-id  ` - tìm ra ip cần. Thay thế container-id tương ứng
- vào trag web với ip trên
</details>
### 2. Linux basic
Các khái niệm, lệnh, và chức năng liên quan đến quản trị hệ thống Linux
<details>

-  <summary>Lệnh ls thường  </summary> được sử dụng để xác định các tập tin và thư mục trong thư mục làm việc. Lệnh này là một trong nhiều lệnh Linux thường được sử dụng mà bạn nên biết.
- Lệnh pwd: Hiển thị hiện tại thư mục làm việc.
- Lệnh mkdir: Tạo một thư mục.
- Lệnh cd: Để điều hướng giữa các thư mục khác nhau
- Lệnh rmdir: Loại bỏ các thư mục trống khỏi danh sách thư mục.
- Lệnh cp: Sao chép tập tin từ thư mục này sang thư mục khác.
- Lệnh mv: Đổi tên và thay thế các tập tin
- Lệnh rm: Xóa tập tin
- Lệnh uname: Lệnh lấy cơ sở thông tin về hệ điều hành
- Lệnh locate: Tìm một tập tin trong cơ sở dữ liệu.
- Lệnh touch: Tạo file trống
- Lệnh ln: Tạo đường tắt cho các tập tin khác
- Lệnh cat: Hiển thị tập tin nội dung ở đầu thiết bị
- Lệnh clear: Xóa phần cuối của thiết bị 
- Lệnh ps: Hiển thị các tiến trình trong terminal
- Lệnh grep: Tìm kiếm một công cụ ở đầu ra
- Lệnh echo: Hiển thị các tiến trình đang hoạt động trên terminal
- Lệnh wget tải tập tin từ internet.
- Lệnh whereis: 
- Lệnh df: Kiểm tra chi tiết của tập tin hệ thống
- Lệnh wc : Kiểm tra dòng, số từ và ký tự trong tệp bằng các tùy chọn khác nhau
  - wc -w show number from
  - wc -l display number line
  - wc -m show số lượng ký tự có trong một tệp
</details>

### 3. Openstack
- Các thành phần openstack
- Cài đặt keystone
- Cài đặt Glance
- Cài đặt Nova
- Về Neutron
-
