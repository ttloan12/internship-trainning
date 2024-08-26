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
**Các khái niệm, lệnh, và chức năng liên quan đến quản trị hệ thống Linux**
<details>
 
**Tập lệnh**
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

**Cấu trúc phân cấp**
<details>
 <summary>  </summary> 
/

Đây là điểm nhập của tất cả các thư mục và được mô tả như một dấu gạch chéo về phía trước, đây thực sự là ngôi nhà của Hệ điều hành. Mọi thứ đều ở trong đó. Không phải mọi người dùng đều có đặc quyền đọc và ghi vào thư mục này; chỉ quản trị viên hoặc người dùng được phép của hệ điều hành mới có quyền truy cập vào các đặc quyền đó.

/bin

Đây là thư mục có tất cả các tệp nhị phân của một số chương trình quan trọng trên hệ điều hành. Thư mục này chứa dữ liệu về các lệnh được sử dụng nhiều nhất liên quan đến tạo (mkdir), di chuyển (mv), sao chép (cp), liệt kê (ls) và xóa (rm) một thư mục hoặc tệp. Theo Tiêu chuẩn hệ thống tệp của Linux, thư mục này không thể có thư mục con.

/boot

Đây là thư mục xử lý việc kích hoạt Hệ điều hành Linux. Trước hết, bạn không cần phải sửa đổi bất kỳ thứ gì trong thư mục này, nếu không, bạn không thể thay đổi bất kỳ thứ gì trong đó trừ khi bạn có quyền của quản trị viên. Bạn nên tránh xa làm bất cứ điều gì trong thư mục này, nếu không sẽ rất khó để thiết lập lại nó.

/dev

Thư mục này chứa các tệp của các thiết bị như Thiết bị USB hoặc Ổ cứng. Hầu hết các tệp được tạo trong thời gian khởi động hoặc khi thiết bị được gắn vào.

/etc

Điều này có vẻ hơi buồn cười đối với bạn, nhưng thư mục này dành cho những loại tệp cấu hình và thư mục mà hệ thống không biết phải đặt chúng ở đâu. Vì vậy, nó là một thư mục “et Cetra” cho Hệ điều hành Linux.

Thư mục này chủ yếu chứa các tệp cục bộ của chương trình tĩnh ảnh hưởng đến tất cả người dùng. Vì thư mục này chủ yếu chứa các tệp liên quan đến cấu hình, tốt hơn nên gọi nó là “Mọi thứ cần cấu hình”.

/home

Đây là thư mục chứa hầu hết dữ liệu cá nhân của người dùng. Người dùng dành phần lớn thời gian của mình ở đây vì Tải xuống, Tài liệu, Máy tính để bàn và tất cả các thư mục cơ bản được yêu cầu và phổ biến khác đều nằm trong thư mục “/ home” này. Tất cả các tệp cấu hình dấu chấm của người dùng cũng có trong đây.

/lib

Đây là những thư mục nơi các thư viện được lưu trữ. Thư viện là một số tệp cần thiết cho bất kỳ ứng dụng nào để thực hiện một số tác vụ hoặc chức năng. Ví dụ, các thư viện này có thể cần thiết bởi các tệp nhị phân trong thư mục / bin .

/media

Đây là thư mục nơi tất cả các thiết bị lưu trữ được kết nối bên ngoài được tự động gắn kết. Chúng ta không cần phải làm gì trong thư mục này vì nó được quản lý bởi chính Hệ điều hành, nhưng nếu chúng ta muốn mount các thiết bị lưu trữ theo cách thủ công, chúng ta có thư mục / mnt cho mục đích đó.

/mnt

Đây là thư mục mà bạn có thể tìm thấy các ổ đĩa được gắn kết khác. Ví dụ: ổ USB, Ổ cứng gắn ngoài hoặc Ổ đĩa mềm. Điều này ngày nay không được sử dụng vì các thiết bị được tự động gắn vào thư mục / media, nhưng đây là nơi chúng ta có thể gắn các thiết bị lưu trữ của mình theo cách thủ công.

/opt

Đây là thư mục tùy chọn. Đây là thư mục chứa phần mềm được cài đặt thủ công bởi các nhà cung cấp.

/proc

Đây là thư mục có các tệp giả. Các tệp giả chứa thông tin về các quy trình.

/root

Cũng giống như / home directory, / root là nhà của Administrator hay còn gọi là superuser. Vì đây là thư mục của superuser, tốt hơn hết là bạn không nên chạm vào nó trừ khi bạn có đầy đủ kiến ​​thức về những gì bạn đang làm.

/run

Thư mục này được sử dụng để lưu trữ dữ liệu tạm thời của các tiến trình đang chạy trên Hệ điều hành.

/sbin

Thư mục này cũng giống như thư mục / bin, nhưng nó được sử dụng bởi superuser, và đó là lý do tại sao “s” được sử dụng trước bin.

/snap

Đây là thư mục chứa các gói snap được lưu trữ trong đó.

/srv

Thư mục này lưu trữ dữ liệu của các dịch vụ đang chạy trên hệ thống. Ví dụ, nó giữ dữ liệu nếu một máy chủ đang chạy trên Hệ điều hành.

/sys

Thư mục này luôn được tạo trong thời gian khởi động, vì vậy nó là một thư mục ảo như / dev, và nó là thư mục khi bạn muốn giao tiếp với Kernal. Nó cũng chứa thông tin liên quan đến các thiết bị được kết nối.

/tmp

Đây là thư mục tạm thời và chứa các tập tin tạm thời của các ứng dụng đang chạy trên hệ thống.

/usr

Thư mục này chứa các ứng dụng được cài đặt và sử dụng bởi người dùng. Nó còn được gọi là “Tài nguyên Hệ thống UNIX”. Nó cũng có thư mục / bin, / sbin và / lib riêng, khác với thư mục / bin, / sbin và / lib của superuser.

/var

Đây là một thư mục có thể thay đổi chứa các tệp và thư mục có kích thước dự kiến ​​sẽ tăng lên theo thời gian và mức độ sử dụng của hệ thống.

 </details>

**Phân quyền trong Linux**

<details>
 <summary>  </summary> 
 - Ownership và Permission
Có 3 loại chủ sở hữu một file/thư mục trên Linux đó là user, group, other
Các quyền đọc, ghi, thực thi được ký hiểu là r, w, x tương ứng với các số là 4, 2, 1.
Lệnh chmod để thay đổi quyền, chown để thay đổi user sở hữu, chrgrp để thay đổi group sở hữu.
 
| file type | user | group | other | name|
| :-------- |:---- |:----- |:----- |:--- |
| d         |rwx   |r-x    |r-x    | dir1|   
| -         |rw-   |r--    |r--    |file1|

 d- thư mục           
 
 -- file
 
 r- đọc
 
 w- ghi
 
 x- thực thi
 
 -- không có quyền 

 u = user= rwx

 g= group= rw

 o= other= r
 </details>
 
**Quyền sở hữu file trong Linux**
<details>
 <summary>  </summary> 

- Nếu bạn muốn loại user nào có quyền nào với file hoặc folder, thì bạn có thể thực thi lệnh chmod để điều khiển việc này theo ý bạn.

Trước tiên nếu muốn xem quyền của file đang ở trong tình trạng nào, bạn có thể thực thi lệnh ls -l

Ví dụ, ls -l file1.txt sẽ hiện ra kết quả:

```-rwxr–rw- 1 user user 0 Jan 19 12:59 file1.txt```

1 – Số của hard links. Hard link là link tới một file đã tồn tại.

user user – Phần này lần lượt hiện chủ của file (owner) và nhóm của chủ file (group) này. Với ví dụ này, chủ file có tên là user và group của nó là user

0 – Thể hiện kích thước của file.

Jan 19 12:59 – Ngày chỉnh sửa cuối cùng.

file1.txt – Tên của thư mục / file

Bên dưới là hướng dẫn chỉ bạn cách sử dụng chmod để đổi quyền của file và thư mục bằng cách thêm số cho đúng. Mỗi loại có số riêng của nó:

r (read) – 4

w (write) – 2

x (execute) – 1

Vì vậy nếu bạn muốn đặt file1.txt với các quyền ở ví dụ trên sao cho owner quyền đọc (r), ghi (w), thực thi (x), nhóm có quyền đọc (r), và những người khác có quyền đọc ghi (r) + (w), bạn sử dụng lệnh:

```chmod 746 file1.txt```

Kết quả nếu bạn kiểm tra quyền của file1.txt sẽ là:

```-rwxr–rw- 1 user user 0 Jan 19 12:59 file1.txt```

- Chown được dùng để đổi owners (chủ sở hữu) của file và folder. Thông thường bạn cần có quyền root để làm lệnh này. Cấu trúc lệnh này cơ bản như sau:
  
```chown [owner/group owner] [file name]```

nếu chúng ta có một file tên là “demo.txt” và muốn đổi chủ sở hữu của file tới cho “jerry” và group owner thành “clients”, vì thông thường khi bạn thay đổi owner bạn cần thay đổi luôn group owner, bạn cần dùng lệnh sau:

```chown jerry:clients demo.txt```

Như bạn thấy, chúng tôi phân biệt giữa owner và group owner với dấu 2 chấm “:”. Nếu chỉ muốn đổi chủ sở hữu của file, chúng ta dùng lệnh sau:

```chown jerry demo.txt```

Chỉ cần bỏ bớt nhóm sở hữu và chỉ cần điền tên chủ sở hữu mới của file, trong trường hợp này, nhóm sở hữu sẽ không đổi. Một ví dụ tương tự sẽ là nếu muốn đổi nhóm sở hữu của file, lệnh cần được viết như sau:

```chown :clients demo.txt```

Trong trường hợp này, chỉ nhóm chủ sở hữu được đổi thành clients (chủ sở hữu sẽ không đổi).

 </details>

 **Package Management Centos 7**

<details>
 <summary>  </summary> 
 
 Quản lý các gói là một khía cạnh quan trọng của quản trị hệ thống và phát triển trong môi trường Linux, chẳng hạn như CentOS. Có thể dùng YUM hoặc DNF. Dưới đây là 20 lệnh cơ bản giúp quản lý hiệu quả các gói của hệ thống CentOS, đảm bảo hệ thống chạy trơn tru và an toàn.

1. Install a package:	sudo yum install package_name

2. Update a package:	sudo yum update package_name

3. Remove a package:	sudo yum remove package_name

4. Search for a package:	yum search keyword

5. List all installed packages:	yum list installed

6. Check for available updates:	yum check-update

7. Clean cached data:	sudo yum clean all

8. List enabled repositories:	yum repolist

9. Enable a repository:	sudo yum-config-manager --enable repo_name

10. Disable a repository:	sudo yum-config-manager --disable repo_name

11. Upgrade all packages:	sudo yum upgrade

12. Show package information:	yum info package_name

13. Find which package provides a file:	yum provides file_name

14. Check package dependencies:	yum deplist package_name

15. Download a package without installing:	yum download package_name

16. Install a local RPM package:	sudo yum localinstall /full/path/to/package_name.rpm --- ví dụ sudo yum locatinstall ./telnet-0.17-85.e19.x86-64.

17. Lock a package version:	sudo yum versionlock package_name

18. Unlock a package version:	sudo yum versionlock delete package_name

19. Reinstall a package:	sudo yum reinstall package_name

20. List all available packages:	yum list all
    
 </details>
 
**Data Backup With Rsync**
<details>
 <summary>  </summary> 
Sao lưu dữ liệu là một phần thiết yếu của cả cơ sở hạ tầng cá nhân và doanh nghiệp. Các máy có hệ điều hành Linux có thể sử dụng rsync và ssh để tạo điều kiện thuận lợi cho quá trình này.

Rsync là một tiện ích dòng lệnh cho phép chuyển các tập tin đến các vị trí cục bộ và từ xa. Rsync rất tiện lợi khi sử dụng vì nó được mặc định đi kèm với hầu hết các bản phân phối Linux. Có thể tùy chỉnh công cụ bằng cách sử dụng nhiều tùy chọn có sẵn.

Trong trường hợp này sử dụng SSH kết hợp với rsync để bảo mật việc truyền tệp.

- Điều kiện tiên quyết

Quyền sudo hoặc root hoặc người dùng có quyền truy cập vào thư mục sao lưu và thư mục đích

Truy cập SSH vào máy chủ thông qua dòng lệnh/cửa sổ thiết bị đầu cuối

Rsync được cài đặt trên máy cục bộ và máy đích

- Cú pháp Rsync cơ bản cho việc truyền dữ liệu cục bộ và bên ngoài
Cú pháp sử dụng công cụ rsync khác nhau đối với truyền dữ liệu cục bộ và từ xa.

Đối với bản sao lưu cục bộ, cú pháp tuân theo mẫu cơ bản sau:
```
rsync 
options
 SOURCE DESTINATION 
```

Để chuyển tập tin đến một vị trí bên ngoài, chúng ta sẽ sử dụng một mẫu hơi khác một chút:

```rsync 
options 
 SOURCE user@IP_or_hostname:DESTINATION
```

Trong cả hai trường hợp, nguồn và đích đều là một thư mục hoặc đường dẫn tệp.

- Sử dụng Rsync để sao lưu dữ liệu cục bộ
Chúng ta sẽ bắt đầu bằng cách thực hiện sao lưu một thư mục trên cùng một máy Linux. Đường dẫn có thể là bất kỳ vị trí nào – phân vùng khác, ổ cứng, bộ lưu trữ ngoài, v.v.

Sử dụng đường dẫn đầy đủ cho cả nguồn và đích để tránh lỗi.

Ví dụ, để sao lưu Dir1 từ Documents tới /media/hdd2/rscync_backup , hãy sử dụng lệnh rsync theo mẫu này:

```rsync -av /home/test/Documents/Dir1 /media/hdd2/rsync_backup```
/home/test# rsync -av /home/test/Documents/Dir1 / media/hdd2/rsync-backup

- Sử dụng Rsync để sao lưu dữ liệu qua mạng
Để sao lưu dữ liệu an toàn qua mạng , rsync sử dụng SSH để truyền dữ liệu. Máy chủ của bạn cần được thiết lập để cho phép kết nối SSH.

Sau khi kết nối được với máy từ xa qua SSH , bạn có thể bắt đầu sao lưu dữ liệu vào một vị trí trên máy đó.

Ví dụ, để sao lưu Dir1 để sao lưu trên một máy khác qua mạng, hãy nhập:

```rsync -av /home/test/Documents/Dir1 test@192.168.56.101:/home/test/backup```

Bạn có thể kiểm tra xem các tập tin có thực sự nằm trên máy chủ từ xa hay không:

![image](https://github.com/user-attachments/assets/c52594ac-c7c0-47d9-8f7c-b27341d5cc26)

Nếu bạn kết nối lần đầu tiên, bạn sẽ cần nhập mật khẩu và xác nhận khi nhận được lời nhắc. Không cần nhập tên người dùng để chuyển từ xa nếu bạn muốn kết nối với tư cách là người dùng hiện tại.


- Nén dữ liệu khi sao lưu bằng Rsync
  
Để tiết kiệm dung lượng, bạn có thể nén dữ liệu trước khi chuyển sang vị trí khác. Bạn có thể sử dụng tùy chọn tích hợp của rsync để nén dữ liệu hoặc sử dụng một công cụ khác để thực hiện việc đó trước khi chạy rsync.

Để nén dữ liệu trong quá trình truyền, hãy sử dụng ```-z``` lệnh chuyển đổi ```rsync```.

```rsync -avz /home/test/Documents/Dir1 test@192.168.56.101:/home/test/backup```

Một lựa chọn khác là sử dụng```zip```lệnh để nén các tệp hoặc thư mục của bạn và sau đó chạy ```rsync```. Trong trường hợp của chúng tôi, chúng tôi sẽ nén Dir1 vào Dir.zip :

```zip /home/test/Documents/Dir1.zip /home/test/Documents/Dir1```

Sau đó chuyển tập tin đó đến một vị trí khác:

```rsync -avz /home/test/Documents/Dir1.zip test@192.168.56.101:/home/test/backup```

Bây giờ, bạn có một bản sao nén của thư mục trên máy chủ từ xa. Bạn cũng có thể thực hiện điều này để chuyển dữ liệu cục bộ nếu bạn muốn sao lưu trên ổ đĩa hoặc phân vùng khác.

  </details>
### 3. Openstack
- Các thành phần openstack
- Cài đặt keystone
- Cài đặt Glance
- Cài đặt Nova
- Về Neutron
-
