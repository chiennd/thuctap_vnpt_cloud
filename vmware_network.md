# Card mạng trong vmware và linux

#Mục lục
[1. Giới thiệu về card mạng trong VMware](#cardmang)
	* [1.1 Switch ảo](#swao)
	* [1.2 DHCP server ảo](#dhcpao)
	* [1.3 Các chế độ card mạng trên vmware](#typeofnic)
	
[2. Thiết lập IP ](#ip)
	* [2.1 Thiết lập IP bằng câu lệnh.](#sualenh)
	* [Thiết lập IP bằng câu lệnh.](#suafile)
	* [Thêm 2 card mạng trên máy server](#themcardmang)
	

<a name="cardmang"></a>
#1. Card mạng trong VMware
<a name="swao"></a>
##1.1: Switch ảo (Virtual Switch):
* Cũng giống như switch vật lý, một Virtual Switch kết nối các thành phần mạng ảo lại với nhau.

* Những  switch ảo hay còn gọi là mạng ảo, chúng có tên là **VMnet0, VMnet1, VMnet2…** một số switch ảo được gắn vào mạng một cách mặc định.

* Mặc định khi ta cài Wmware thì có sẵn 3 Switch ảo như sau: 
	* VMnet0 chế độ Bridged (cầu nối)
	* VMnet8 chế độ NAT
	* VMnet1 chế độ Host-only.

* Ta có thể thêm, bớt, chỉnh các option của VMnet bằng cách vào menu `Edit -> Virtual Network Editor`.

<a name="dhcpao"></a>
##1.2: DHCP server ảo (Dynamic Host Configuration):
* Là  server ảo cung cấp địa chỉ IP cho các máy ảo trong việc kết nối máy ảo vào các Switch ảo không có tính năng `Bridged (VMnet0)`.

* Ví dụ như DHCP ảo cấp đến các máy ảo có kết nối đến `Host-only` và `NAT`.

* **LAN Segment:** Các card mạng của máy ảo có thể gắn kết với nhau thành từng LAN Segment. Không giống như VMnet, LAN Segment chỉ **kết nối các card ảo lại với nhau** mà **không có những tính năng như DHCP hoặc kết nối chung với một card mạng ảo** được tạo bên ngoài (các VMware Network Adapter VMnet được tạo bên ngoài máy thật).

<a name="typeofnic"></a>
##1.3: Các chế độ của card mạng trên máy ảo:

* **Chế độ Bridge**: ở chế độ này thì card mạng trên máy ảo sẽ được gắn vào VMnet0 và VMnet0 này liên kết trực tiếp với card mạng vật lý. Ở chế độ này máy ảo sẽ kết nối internet thông qua lớp card mạng vật lý và có chung lớp mạng với card mạng vật lý.

* **Chế độ NAT**: ở chế độ này thì card mạng của máy ảo kết nối với VMnet8, VNnet8 cho phép máy ảo đi internet thông qua cơ chế NAT (NAT device).

	Lúc này lớp mạng bên trong máy ảo khác hoàn toàn với lớp mạng của card vật lý bên ngoài. IP của card mạng sẽ được cấp bởi DHCP VMnet8 cấp, trong trường hợp bạn muốn thiết lập IP tĩnh cho card mạng máy ảo bạn phải đảm bảo chung lớp mạng với VNnet8 thì máy ảo mới có thể đi internet.

* **Cơ chế Host-only**: ở cơ chế này máy ảo được kết nối với VMnet có tính có tính năng Host-only, trong trường hợp này là VMnet1 (bạn có thể add nhiều VMnet Host-only). VMnet Host-only kết nối ra một card mạng ảo tương ứng ngoài máy thật.

	Ở chế độ này máy ảo không có kết nối internet. IP của máy ảo được cấp bởi DHCP của VMnet tương ứng.Trong nhiều trường hợp đặc biệt cần cấu hình riêng, ta có thể tắt DHCP trên VMnet và cấu hình IP bằng tay cho máy ảo.

<a name="ip"></a>
#2. Thiết lập địa chỉ IP
* Xem tất cả thông tin về card mạng
```sh
# ifconfig -a
```
* Kết quả
```sh
eth0      Link encap:Ethernet  HWaddr 00:0c:29:bd:b6:fc  
          inet addr:10.145.25.210  Bcast:10.145.25.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:febd:b6fc/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2591 errors:0 dropped:0 overruns:0 frame:0
          TX packets:42 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:209211 (209.2 KB)  TX bytes:3797 (3.7 KB)

eth1      Link encap:Ethernet  HWaddr 00:0c:29:bd:b6:06  
          inet addr:10.10.10.100  Bcast:10.10.10.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:febd:b606/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:681 errors:0 dropped:0 overruns:0 frame:0
          TX packets:266 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:55067 (55.0 KB)  TX bytes:37259 (37.2 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

<a name="sualenh"> </a>
##2.1 Thiết lập IP bằng câu lệnh. (Chỉ có giá trị tức thời, sẽ mất khi khởi động lại.)
* Câu lệnh: 
```sh
# ifconfig ethX IP-address netmask subnet-mask
```
* Trong đó:
	* `ethX:` là tên card mạng
	* `IP-address:` là địa chỉ ip cần gán.
	* `subnet-mask:` là địa chỉ netmask.

* Ví dụ: Đặt địa chỉ ip `10.145.25.210`, netmask là `255.255.255.0` cho card mạng `eth0`
```sh
# ifconfig eth0 10.145.25.210 netmask 255.255.255.0
```
* Kết quả:

![](http://i.imgur.com/MvNQjIp.png)

<a name ="suafile"></a>
##2.2 Thiết lập bằng cách sửa file. (Có giá trị mãi mãi)
* Thư mục chứa file cấu hình `/etc/network/interfaces/`
* Nội dung file

```sh
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet dhcp
```
* Cấu hình IP động cho card mạng
```sh
auto eth0
iface eth0 inet dhcp
```
* Trong đó: 
	* `eth0`: tên card mạng
	* `dhcp`: chỉ ra rằng card mạng sẽ tự động nhận địa chỉ ip.
	
* Cấu hình IP tĩnh cho card mạng
```sh
auto eth1
iface eth1 inet static
address 10.10.10.100
netmask 255.255.255.0
gateway 10.10.10.1
network 10.10.10.0
broadcast 10.10.10.255
dns-nameserver 8.8.8.8
```
* Trong đó:
	* `eth1`: tên card mạng
	* `static`: chỉ ra rằng, card mạng sẽ nhận địa chỉ ip do người quản trị đặt.
	* `address`: địa chỉ ip.
	* `netmask`: địa chỉ netmask.
	* `gateway`: địa chỉ gateway.
	* `network`: địa chỉ mạng.
	* `broadcast`: địa chỉ quảng bá.
	* `dns-nameserver`: địa chỉ dns server phân giải tên miền.
	
<a name="themcardmang"></a>
#3. Thêm 2 card mạng trên máy server
* Chọn máy cần thêm card mạng, vào phần setting của máy, ta có cửa sổ hiện ra.
Thực hiện theo các bước để thêm card mạng

![](http://i.imgur.com/oSqSzTO.png)

* Chọn kiểu card mạng và nhấn kết thúc.

![](http://i.imgur.com/Ol8Htcq.png)

* Các kiểu card mạng đã được giới thiệu ở phần trên.
