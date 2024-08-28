# Giới thiệu về LVM

Logical Volume Management(LVM) dùng quản lí các thiết bị lưu trữ. LVM là một tiện ích cho phép chia không gian đĩa cứng thành những Logical Volume từ đó giúp cho việc thay đổi kích thước trở nên dễ dàng 

Một số khái niệm liên quan:

- Physical volume: là một đĩa cứng vật lý hoặc là partition
- Volume group: là một nhóm các physical volume ( ổ đĩa ảo )
- logical volume: là các phân vùng ảo của ổ đĩa ảo

Một số lệnh cần thiết:

- Lệnh `fdisk` : Dùng để quản lý việc phân vùng trong ổ cứng. Là một công cụ hữu dụng tron linux [tìm hiểu thêm FDISK](https://github.com/duckmak14/thuctapsinh/blob/master/NiemDT/Linux/docs/Fdisk.md)

- Lệnh `mount` : Dùng để gắn một phân vùng vào thư mục root để có thể sử dụng được nó [tìm hiểu thêm về mount](https://github.com/duckmak14/thuctapsinh/blob/master/Anhduc/liunux/docs/mount.md)

- Lệnh dd : Dùng Sao lưu và hồi phục toàn bộ dữ liệu ổ cứng hoặc một partition và kiểm tra tốc độ đọc của kiểu lưu trữ dữ liệu trong LVM

  ## 2. Ưu điểm và Nhược điểm của LVM[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#2-uu-điem-va-nhuoc-điem-cua-lvm)

  Ưu điểm :

- Không để hệ thống bị gián đoạn hoạt động

- Không làm hỏng dịch vụ

- Có thể kế hợp swap

- Có thể tạo ra các vùng dung lượng lớn nhỏ tuỳ ý.

Nhược điểm:

- Các bước thiết lập phức tạp và khó khăn hơn

- Càng gắn nhiều đĩa cứng và thiết lập càng nhiều LVM thì hệ thống khởi động càng lâu.

- Khả năng mất dữ liệu cao khi một trong số các đĩa cứng bị hỏng.

- Windows không thể nhận ra vùng dữ liệu của LVM. Nếu Dual-boot ,Windows sẽ không thể truy cập dữ liệu trong LVM.

  ## 3. Những thành phần trong LVM[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#3-nhung-thanh-phan-trong-lvm)

  

  - HDD : là một thiết bị lưu trữ máy tính. Nó là loại bộ nhớ không thay đổi và không bị mất dữ liệu khi ta ngừng cung cấp nguồn điện cho chúng
  - Partition: là các phân vùng của ổ cứng. Mỗi một ổ cứng có 4 partition. Trong đó bao gồm 2 loại là primary partition và extended partition
    - primary partition: còn được gọi là phân vùng chính, có thể khởi động và mỗi ổ cứng chỉ có tối đa 4 phân vùng này
    - extended partition: Hay còn được gọi là phân vùng mở rộng của ổ cứng

  Cách thức hoạt động các tầng của LVM:

  - Tầng đầu tiên : hard drives là tầng các disk ban đầu khi chưa chia phân vùng

  - Partitions: Sau đó ta chia các disk ra thành các phân vùng nhỏ hơn

  - Physical volume : từ một partitions ta sẽ tạo ra được một physical

  - group volume : Ta sẽ ghép nhiều physical volume thành một group volume

  - Logical volume : Ta sẽ có thể tạo ra được logical volume

    ## II.Hướng dẫn sử dụng LVM[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#iihuong-dan-su-dung-lvm)

    Để có thể nhìn rõ hơn về sự hoạt động của LVM thì dưới đây tôi sẽ làm một ví dụ về LVM để các bạn có thể hiểu rõ hơn về nó.

    Chuẩn bị:

    - Một VM có os centos
    - VM có 3 disk
    - Đã chia phân vùng và mount vào thư mục root

    **Lưu ý : Nếu phân vùng được mount vào thư mục root rồi thì không thể tạo `physical colume`.Muốn thao tác được tới các phân vùng trước tiên ta phải umount tất cả các phân vùng hay là logical mà ta muốn tác động đến.**

    ## 1. Thao tác tạo[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#1-thao-tac-tao)

    *Lưu ý : Nếu phân vùng được mount vào thư mục root rồi thì không thể tạo `physical colume`*

    **Tạo physical volume**

    - Ta sử dụng lệnh

       

      ```
      pvcreate
      ```

       

      theo cú pháp

      ```
      pvcreate /dev/(tên phân vùng)
      ```

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_7.png)

    Ta kiểm tra lại bằng lệnh `pvs` xem ta tạo được thành công physical volume hay chưa

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot.png)

    **Tạo một Group volume**

    - Ta sử dụng lệnh

       

      ```
      vgcreate
      ```

       

      theo cú pháp

      ```
      vgcreate (ten_group) /dev/(tên phân vùng 1) /dev/(tên phân vùng 2)
      ```

    Ta sử dụng lệnh vgs để kiểm tra xem group volume.

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_1.png)

    **Tạo một Logical volume theo cú pháp**

    ```
    lvcreate -L size_volume -n (ten logical) (tên group volume)
    ```

    Để kiểm tra logical volume thì ta sử dụng lệnh `lvs`

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_2.png)

    Sau khi tạo xong một logical thì sẽ phải định dạng xong một logical để có thể mount nó vào cây thư mục root như trong phần lệnh mount tôi đã hướng dẫn thực hiện mount một phân vùng thì ta mới có thể sử dụng được logical này.

    ## 2. Thay đổi dung lượng[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#2-thay-đoi-dung-luong)

    **Thay đổi dung lượng physical volume ta sử dụng lệnh theo cú pháp**

    ```
    lvextend -L (n) /dev/(tên group)/(tên logical)
    ```

    Trong đó

    - `lvextend` : là lệnh tăng dung lượng
    - `lvreduce` : là lệnh dùng để giảm dung lượng
    - `-L` : là option của lệnh
    - `(n)` : là số dùng để tăng giảm dung lượng theo ý muốn của ta

    Sau khi ta thay đổi dung lượng như ta muốn thì ta sử dụng lệnh `resize2fs` để xác nhận thay đổi

    ```
    resize2fs (tên group volume)(tên logical volume)
    ```

    ở đây tôi sẽ làm một kịch bản về việc thay đổi dung lượng. Đầu tiên tôi sẽ chọn ra logical mà tôi muốn thay đổi sau đó tôi sẽ thay đổi nó rồi kiểm tra lại xem nó thay đổi hay là chưa. sau khi thay đổi về dung lượng thì ta cần phải mount vào để có thể sử dụng nó.

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_5.png)

    Muốn tăng dung lượng thì ta làm giống như với giảm dung lượng chỉ cần thay đổi lệnh `lvreduce` thành lệnh `lvextend`

    **Thay đổi dung lượng của một group volume**

    Khi thay đổi dung lượng của một group volume thì ta sẽ thêm hoặc xóa phân vùng thuộc group để có thể thay đổi dung lượng của nó.

    Đầu tiên ta kiểm tra xem có phân vùng nào chưa thuộc group thì ta sẽ thêm phân vùng đó vào một group là cách để tăng kích thước cho group đó

    Tôi sẽ thêm vdb3 vào group g_volume

    Đầu tiên ta kiểm tra dung lượng của group

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_11.png)

    ```
    vgextend /dev/g_volume /dev/vdc2
    ```

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_9.png)

    ta kiểm tra lại dung lượng của group bằng lệnh vgs một lần nữa và thấy sự thay đổi của nó

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_10.png)

    ## 3.Thao tác xóa[#](https://blog.cloud365.vn/linux tutorial/tong-quan-lvm/#3thao-tac-xoa)

    **Xóa một logical ta sử dụng lệnh theo cú pháp**

    ```
    lvremove /dev/(ten group)/(ten logical)
    ```

    Đầu tiên ta kiểm tra xem có những logical nào

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_4.png)

    Ở đây tôi chọn xóa logical cuối cùng khoanh đỏ

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_6.png)

    ở dòng cuối cùng đã ghi xóa thành công logical

    **Xóa một group volume ta thực hiện theo cú pháp**

    ```
    vgremove /dev/(tên group)
    ```

    Đầu tiên tôi kiểm tra xem có những group volume nào sau đó chọn ra một group. Ở đây tôi chọn xóa `g_volume` rồi kiểm tra lại xem nó đã bị xóa hay chưa.

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_12.png)

    **Xóa một physical volume ta thưc hiện theo cú pháp**

    ```
    pvremove /dev/(tên physical)
    ```

    ![img](https://blog.cloud365.vn/images/img_lvm/tong_quan/screenshot_13.png)