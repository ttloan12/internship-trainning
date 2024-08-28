 **Quản lý User, group** 

1. *User*

User là người có thể truy cập đến hệ thống.

User có username và password.

Có hai loại user: super user và regular user.

Mỗi user còn có một định danh riêng gọi là UID.

Định danh của người dùng bình thường sử dụng giá trị bắt đầu từ 500.

2. *Group*

Group là tập hợp nhiều user lại.

Mỗi user luôn là thành viên của một group.

Khi tạo một user thì mặc định một group được tạo ra.

Mỗi group còn có một định danh riêng gọi là GID.

*3. Tập lệnh quản lý User và Group*

Tạo User:
Cú pháp: #useradd [option] -c “Thông tin người dùng”
-d<thư c=”” n=””>
-m : Tạo thư mục cá nhân nếu chưa tồn tại
-g<nhóm a=”” i=”” ng=””>
Ví dụ: #useradd –c “Nguyen Van A – Server Admin” –g serveradmin vana</nhóm></thư>

**Thay đổi thông tin cá nhân:**
Cú pháp: #usermod [option]

Những option tương tự Useradd

Ví dụ: #usermod –g kinhdoanh vana //chuyển vana từ nhóm server admin sang nhóm kinh doanh.

**Xóa người dùng**
Cúpháp : #userdel [option]

**Vídụ** : #userdel –r vana

**Khóa/Mở khóa người dùng**
passwd –l  / passwd –u

usermod –L / usermod –U

Trong /etc/shadow có thể khóa tài khoản bằng cách thay từ khóa x bằng từ khóa *.

**Tạo nhóm:**
**Cú pháp**: #groupadd

Ví dụ: #groupadd serveradmin

**Xóa nhóm**
**Cú pháp**: #groupdel

Ví dụ: #groupdel

**Xem thông tin về User và Group**
**Cú pháp**: #id

Ví dụ: #id -g vana //xem GroupID của user vana

**Cú pháp**: #groups

Ví dụ: #groups vana //xem tên nhóm của user vana

**4. Những file liên quan đến User và Group**

\#/etc/passwd
Mỗi dòng trong tập tin gồm có 7 trường, được phân cách bởi dấu hai chấm.

\#/etc/group
Mỗi dòng trong tập tin gồm có 4 trường, được phân cách bởi dấu hai chấm.

\#/etc/shadow
Lưu mật khẩu đã được mã hóa và chỉ có user root mới được quyền đọc.









