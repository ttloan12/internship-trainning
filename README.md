# internship-trainning
### 1. Đóng gói image
Các hướng dẫn được thực hiện tại https://github.com/nhanhoadocs/create-images-openstack/blob/master/docs/Ubuntu2004.md
<details>
  <summary>
#  một số khái niệm liên quan </summary>
- LVM (Logical Volume Manager) là một hệ thống quản lý ổ đĩa trong Linux, cho phép tạo các phân vùng linh hoạt hơn so với việc sử dụng phân vùng tĩnh truyền thống (partition).
Với LVM, bạn có thể dễ dàng thay đổi kích thước, thêm, hoặc di chuyển các phân vùng mà không cần khởi động lại hệ thống.
- Image không dùng LVM có nghĩa là hệ điều hành hoặc ứng dụng trong image đó sử dụng phân vùng truyền thống (như ext4, xfs, v.v.) thay vì LVM để quản lý không gian đĩa.
Điều này thường dẫn đến việc quản lý phân vùng ít linh hoạt hơn, nhưng lại đơn giản hơn trong một số trường hợp, đặc biệt là khi không cần phải mở rộng hoặc thu hẹp các phân vùng sau khi hệ thống đã được triển khai.
- Queens là một phiên bản ổn định với nhiều cải tiến và tính năng mới, nhưng không còn nhận được cập nhật bảo mật hoặc hỗ trợ chính thức từ OpenStack Foundation,
vì đã có các phiên bản mới hơn. Nếu có thể, bạn nên xem xét việc nâng cấp lên một phiên bản mới hơn để nhận được các bản vá bảo mật và cải tiến hiệu năng.
Phiên bản này phù hợp với các môi trường sản xuất yêu cầu sự ổn định và tính năng tiên tiến trong các dịch vụ mạng, quản lý container và quản lý tài nguyên.
vì đã có các phiên bản mới hơn. Phiên bản này phù hợp với các môi trường sản xuất yêu cầu sự ổn định và tính năng tiên tiến trong các dịch vụ mạng, quản lý container và quản lý tài nguyên.
** tạo mới VM tại webvirtcloud
- tạo WebvirtCloud cần tạo docker trước
- docker version được chỉnh sửa trong `sudo nano ./webvirtcloud.sh-`. Tạo ra version vượt mức yêu cầu nhưng vẫn cảnh báo lỗi thì sửa ở đoạn docker-version
``` required_version="25.0.0"
` required_version="25.0.0"

# Compare versions using a simple logic
if [ "$(printf '%s\n' "$required_version" "$docker_version" | sort -V | head -n1)" = "$required_version" ]; then
    echo -e "\nDocker version $docker_version is sufficient.\n"
else
    echo -e "\nDocker version $docker_version is not sufficient. Please update Docker to version $required_version or later.\n" 
    exit 1
fi  ```
fi  `

- bắt đầu web thì có mariadb, rabbitmq
- chạy docker ps để hiện container ID 
- chạy lệnh ` docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container-id  ` - tìm ra ip cần. Thay thế container-id tương ứng
- vào trag web với ip trên
</details>
### 2. Linux basic
Các khái niệm, lệnh, và chức năng liên quan đến quản trị hệ thống Linux
<details>
  <summary>
Lệnh ls thường  </summary> được sử dụng để xác định các tập tin và thư mục trong thư mục làm việc. Lệnh này là một trong nhiều lệnh Linux thường được sử dụng mà bạn nên biết.
Lệnh này có thể được sử dụng một mình mà không cần bất kỳ đối số nào và nó sẽ cung cấp cho chúng ta kết quả đầu ra với tất cả thông tin chi tiết về các tệp và thư mục trong thư mục làm việc hiện tại. Lệnh này có rất nhiều tính linh hoạt trong việc hiển thị dữ liệu ở đầu ra. Kiểm tra hình ảnh dưới đây để biết đầu ra.
</details>

### 3. Openstack
- Các thành phần openstack
- Cài đặt keystone
- Cài đặt Glance
- Cài đặt Nova
- Về Neutron
-
