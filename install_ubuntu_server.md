#Hướng dẫn cài đặt Ubuntu server 14.04 trên máy ảo vmware
##Mục lục
[1. Giới thiệu về Ubuntu server] (#Gioithieu)
[2. Hướng dẫn cài đặt Ubuntu server](#Caidat)


<a name="Gioithieu"></a>
#1. Giới thiệu về Ubuntu server

* Ubuntu: là một hệ điều hành máy tính dựa trên Debian GNU/Linux, một bản phân phối Linux thông dụng.
* Ubuntu là phần mềm mã nguồn mở tự do, có nghĩa là người dùng được tự do chạy, sao chép, phân phối, nghiên cứu, thay đổi và cải tiến phần mềm theo điều khoản của giấy phép GNU GPL.
* Ubuntu được tài trợ bởi Canonical Ltd (chủ sở hữu là một người Nam Phi Mark Shuttleworth). Thay vì bán Ubuntu, Canonical tạo ra doanh thu bằng cách bán hỗ trợ kĩ thuật.
* Bằng việc để cho Ubuntu tự do và mở mã nguồn, Canonical có thể tận dụng tài năng của những nhà phát triển ở bên ngoài trong các thành phần cấu tạo của Ubuntu mà không cần phải tự mình phát triển.

* Ubuntu server là một phiên bản của ubuntu được sử dụng cho các server.

<a name="Caidat"></a>
#2. Hướng dẫn cài đặt Ubuntu server

###2.1 Chọn ngôn ngữ cho ubuntu.
![](http://i.imgur.com/Xnj3Gkj.png)

###2.2 Chọn Install Ubuntu Server để cài đặt hệ điều hành
![](http://i.imgur.com/IZjhvlc.png)

###2.3 Chọn ngôn ngữ cài đặt.
![](http://i.imgur.com/FNrn6E4.png)

###2.4 Chọn khu vực địa lý
![](http://i.imgur.com/jw5OHEm.png)

###2.5 
Hình dưới là cửa sổ thông báo rằng khu vực địa lý chọn ở trên không tương ứng với ngôn ngữ đã chọn vì ban đầu chúng ta chọn English làm ngôn ngữ nhưng vùng địa lý lại là Việt Nam.
Vì vậy, bạn phải chọn thủ công.
![](http://i.imgur.com/LYEfQ4c.png)


###2.6 Cài đặt bàn phím
Bạn có muốn tự động dò tìm mẫu bàn phím? chọn NO để tự chọn mẫu bàn phím.

![](http://i.imgur.com/XkexRMn.png)

![](http://i.imgur.com/OA7Iu5p.png)

###2.7 Chọn tên hiển thị cho server
![](http://i.imgur.com/byCwqhJ.png)

###2.8 Đặt tên đầy đủ cho user
![](http://i.imgur.com/Rc6Kc7d.png)

###2.9 Đặt username
![](http://i.imgur.com/WFS14MX.png)

###2.10 Đặt mật khẩu cho username ở trên
![](http://i.imgur.com/6jF3nwh.png)

###2.11 Bạn có muốn mã hóa thư mục `/home`
![](http://i.imgur.com/oC5kDJ8.png)

###2.12 Cấu hình múi giờ.
![](http://i.imgur.com/f7RqdiX.png)

###2.13 Phân vùng ổ cứng cho ubuntu
* `Guided - use entire disk:` Tự động phân vùng.
* `Guided - use entire disk and with set up LVM:` Tự động phân vùng với LVM
* `Guided - use entire disk and with set encrypted up LVM:` Tự động phân vùng và mã hóa LVM.
* `Manual:` Phân vùng bằng tay.
* Tìm hiểu thêm về LVM: https://github.com/hocchudong/Logical-Volume-Manager-LVM-

* Ở đây, mình đã chọn cái 1, tự động phân vùng.
![](http://i.imgur.com/u46HwLZ.png)

* Sẽ có 2 phân vùng được tạo ra.
	* phân vùng primary chứa 20.4GB, định dạng ext4, chứa thư mục /
	* Phân vùng logical chứ 1.1GB, định dạng swap, dùng để làm RAM ảo cho máy tính.
![](http://i.imgur.com/TSjhhgv.png)

* Đồng ý tạo 2 phân vùng.
![](http://i.imgur.com/UIVatnd.png)

###2.14 Cấu hình http proxy
Nếu mạng của bạn phải sử dụng http proxy để có thể truy cập internet thì hãy điền thông tin proxy vào dưới.
![](http://i.imgur.com/j8IjBUD.png)

###2.15 Tùy chọn cập nhật các gói
* No automatic update: Không tự động cập nhật
* Install security updates automatically: Tự động cập nhật các bản bảo mật.
* Mange system with Landscape: 

![](http://i.imgur.com/Rbe54q2.png)

###2.16 Chọn các gói phần mềm sẽ được cài đặt cùng với ubuntu
* ở đây có các gói như ssh server, dns server, mail server,.... 

![](http://huongdanit.com/noidunghdit/uploads/2015/02/install_ubuntu_28.jpg)

###2.17 Cài đặt boot loader (GRUB) trên ổ đĩa
![](http://i.imgur.com/0mo1DOU.png)

###2.18 Kết thúc cài đặt.
![](http://i.imgur.com/kavUbyY.png)
Nhận continues để khởi động lại máy











