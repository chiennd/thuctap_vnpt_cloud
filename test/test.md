#Keystone
* Mô hình triển khai nền tảng dịch vụ Cloud cung cấp cho người dùng quyền được truy cập các vào tài nguyên như máy ảo, bộ nhớ, mạng,...
* Tính năng quan trọng của bất kỳ cloud nào là làm cách nào để cung cấp điều khiển truy cập vào các tài nguyên. 
* Trong OpenStack, project keystone chịu trách nhiệm cung cấp điều khiển truy cập vào toàn bộ tài nguyên cloud. 
* Keystone chứng mình nó là một thành phần quan trọng trong cloud.

#1. Các chức năng cơ bản của Keystone
##1.1 Identity:
* Identity xác định ai là người truy cập vào tài nguyên
cloud.
* Trong openstack, idenity thường được dùng
để định danh user.
* idenity có thể lưu user vào Keystone database.
* Trong môi trường thương mại,  Identity thường sử dụng bên thứ ba. 
* Keystone có thể lấy lại thông tin định danh người dùng từ idenity bên thứ ba.

##1.2 Authentication:
* Authentication xử lý việc xác nhận định danh người dùng.
* Trong nhiều trường hợp, authentication là thực hiện xác nhận thông tin đăng nhập user và mật khẩu.
* Lúc Ban đầu, Keystone có khả năng thực hiện tất cả authentication. Điều đó là không được khuyến khích với môi trường doanh nghiệp.
* Trong môi trường doanh nghiệp, password cần được bảo vệ và quản lý.
* Keystone dễ dàng tích hợp với các nền tảng authentication đã tồn tại khác là LDAP, Active Directory.
* Khi người dùng định danh bằng mật khẩu, authentication sẽ tạo ra token cho bước authentication tiếp theo.
* Token sẽ làm giảm tầm nhìn, phơi bày password mà nó cần được giấu đi và bảo vệ càng tốt.
* Token có giới hạn thời gian sống và hết hạn nếu chúng bị đánh cắp.
* OpenStack chủ yếu dựa vào token để authentication và các mục đích khác.
* Keystone là một dịch vụ của OpenStack có thể giải quyết vấn đề trên.
* Hiện tại, Keystone được sử dụng `bearer token`. Có nghĩa là bất cứ ai thu được quyền sở hữu token, dù đúng hay sai đều có khả năng sử dụng token để xác thực và truy cập vào tài nguyên. Kết quả là, việc sử dụng Keystone rất quan trọng trong việc bảo vệ token và các thành phần khác.

##1.3 Authorization
* Một user đã được định danh và token đã được tạo và phân bổ, mọi thứ bắt đầu trở nên thú vị. Bởi vì chúng ta có đủ nền tảng và địa điểm để bắt đầu thực hiện authorization.
* Authorization xử lý xác định những tài nguyên nào user được phép truy cập.
* Trong OpenStack cung cấp cho người dùng quyền truy cập vào tài nguyên rất lớn. Ví dụ, có cần cơ chế mà người dùng được phép tạo 1 máy ảo cụ thể, attach hay delete volume of block storage, được phép tạo mạng ảo,...
* Trong openstack, keystone bản đồ user đến Projects hoặc domain bằng cách liên kết role cho user đối với Projects hay domain đó.
* Một số dịch vụ OpenStack khác như Nova, Cinder, Neutron xem xét project của user và role và đánh giá thông tin sử dụng policy.
* Công cụ chính sách xem xét thông tin (đặc biệt là giá trị role) và quyết định hành động của user được phép thực hiện.


**Keystone chủ yếu tập trung vào idenity, authentication và authorization.**
##1.4 Các lợi ích như:
* Xác thực đơn và cấp quyền cho các dịch vụ khác của OpenStack.
* Keystone xử lý các hệ thống xác thực ngoài và cung cấp theo chuẩn cấp quyền cho tất cả các dịch vụ khác của OpenStack: nova, glance, cinder, neutron,... và keystone cô lập tất cả các dịch này.
* Keystone cung cấp project, là những dịch vụ có thể sử dụng tài nguyên riêng.
* Keystone cung cấp domain, được sử dụng để định nghĩa tách rời không gian cho user, groups và project để cho phép tách rời khỏi khách hàng.

* Roles được sử dụng để authorization giữa Keystone vào policy files của mỗi dịch vụ openstack. Phân công user và groups vào project nào, domain nào.
* Lưu trữ catalog cho dịch vụ OpenStack, endpoints, region, cho phép clients khám phá các dịch vụ hoặc endpoints mà họ cần.

#2 Các khái nhiệm cơ bản.
##2.1 Projects
* Trong keystone, projects là khái niệm trừu tượng, sử dụng bởi các dịch vụ khác trong OpenStack.
* Projects có chứa các tài nguyên.
* Tiền thân của Projects là tenants, thay đổi để trực quan hơn.
* Projects không phải là chủ user nhưng user và group có quyền truy cập vào các project, sử dụng các role.
* Các role trên user và group chỉ ra rằng họ có quyền gì để truy cập vào các tài nguyên trong Projects.

##2.2 Domain
* Là khái niệm vừa ra đời ở api v3.
* Không có cơ chế để hạn chế tầm nhìn của project trên các tổ chức khau nhau -> dẫn đến va chạm giữa tên project của các tổ chức khác nhau. username cũng có thể va chạm giữa 2 tổ chức.
* Keystone ra khái nhiệm trừu tượng mới: domain.
* Dùng để cô lạp tầm nhìn, tập hợp các project, user cho 1 tổ chức cụ thể.
* 1 domain có thể bao gồm user, group, project....
* Domain cho phép bạn phân chia các nguồn tài nguyên trong cloud vào các tổ chức cụ thể.

##2.3 Users và Groups
* Mối quan hệ giữa user group và domain.

##2.4 Roles
* Vai trò.
* Mỗi user có thể có vai trò khác nhau đối với từng project.

##2.5 Assignment

##2.6 Targets

##2.7 Token
* Người dùng muốn sử dụng OpenStack API thì cần phải chứng minh mình là ai, và mình nên đưuọc cho phép trong câu hỏi API.
* Cách mà họ lưu trữ là gửi token đến API call và Keystone phản ứng để sinh ra token.
* Người dùng nhận token và xác thực thành công lần nữa ở Keystone.
* Token mang nó để cấp quyền.
* Token chứa cả ID và payload. ID bảo đảm là duy nhất trên mỗi cloud và payload chứa dữ liệu user. payload có thể chứa những dữ liệu dưới: create, expire, authenticated, project, catalog,....

##2.8 Catalog:
* Nó chứa URLs và endpoints của các dịch vụ trong cloud.
* Với catalog, người dùng và ứng dụng có thể biết ở đâu để gửi yêu cầu tạo máy ảo hoặc storage objects.
* Dịch vụ catalog chia thành danh sách các endpoint, mỗi endpoint chi thành các admin URL, internal URL, public URL.
Ví dụ:



#3. Identity
##3.1 SQL
* Keystone hỗ trợ SQL để lưu trữ thông tin users và groups
* Hỗ trợ các database là: MySQL, PostgreSQL và DB2.
* Keystone sẽ lưu thông tin name, password và chi tiết.
* Cài đặt database phải cấu hình trong file cấu hình keystone.
* Chủ yếu, Keystone là Identity Provider, không phải là tốt nhất cho mọi người và cũng ko phải là tốt nhất cho khách hàng doanh nghiệp. 
* Ưu điểm
	* dễ dàng cài đặt.
	* Quản lý users và groups quả OpenStack APIs.

* nhược điểm:
	* Keystone không nên là Identity Provider.
	* Mật khẩu yếu ( không khôi phục mk, không xoay mật khẩu).
	* Các doang nghiệp thường sử dụng LDAP.
	* Phải ghi nhớ username và password.

##3.2 LDAP
* Keystone sẽ truy cập vào LDAP giống như các ứng dụng
khác sử dụng LDAP.
* Cấu hình trong file config của keystone để
sử dụng LDAP.
* LDAP thường được dùng để chỉ đọc, có nghĩa là tìm user và groups (qua search) và authentication (qua bind).
* Nếu sử dụng LDAP chỉ để đọc, keystone sẽ cần tối thiểu các quyền để sử dụng LDAP. Với trường hợp, cần quyền đọc các thuộc tính user và group.
* Thu hồi quyền tài khoản, không yêu cầu quyền truy cập vào mật khẩu. 
* Ưu điểm:
	* không duy trì bản sao của tài khoản người dùng.
	* Keystone không hành động như một nhà cung cấp nhận dạng (identity provider).
* Nhược điểm:
	* dịch vụ tài khoản sẽ lưu ở đâu đó và LDAP không muốn có tài khoản trong LDAP.
	* Keystone có thể thấy mật khẩu người dùng, lúc mật khẩu được yêu cầu authentication.
	* Keystone đơn giản thì chuyển các yêu cầu, nhưng tốt nhất là Keystone không nhìn thấy mật khẩu.

##3.3Multiple Backends
* Từ phiên bản Juno, Keystone hỗ trợ nhiều idenity backend từ phiên bản v3. 

[hình ảnh]
* Identity service có thể có nhiều backend cho mỗi domain.
* Ví dụ: LDAPs for Domain A and B. SQL-based backend for service accounts and Assignment.

* Ưu điểm:
	* Hỗ trợ nhiều backend đồng thời.
	* Sử dụng lại LDAP đã có.
 
* Nhược điểm
	* phức tạp trong cài đặt.
	* Xác thực tài khoản người dùng phải trong miền scoped

##3.4 idenity provider
* Sử dụng các giải pháp thứ ba để có thể xác thực.
##3.5 use cases for idenity backend
| identity source |    uses case|
|:------:|:------:|
|SQL| sử dụng cho testing hoặc developing.
user nhỏ.
openstack-specific accounts.|

|LDAP| sử dụng nếu đã có trước.
chỉ sử dụng mỗi LDAP nếu bạn có khả năng tạo dịch vụ
tài khoản cần thiết trong LDAP.|

|Multiple backend|
trong môi trường doanh nghiệp.
sử dụng nếu dịch vụ người dùng không được phép trong LDAP.|

|idenity provider|
bạn có thể tận dụng lợi thế của cơ chế Federated.
sử dụng nếu indentity provider đã tồn tại.
keystone không thể truy cập vào LDAP.
Non-LDAP idenity source.
sử dụng nếu LDAP tương tác đến underlying platform và web server.|

##3.6. Authentication
###3.6.1 Authentication password
* payload của request phải có đủ thông tin để tìm user nằm ở đâu
* xác nhận user và lấy dịch vụ catalog của user.
* định dang user xác định đến ID, 
 
 
###3.6.2 Authentication token
* user có thể yêu cầu 1 token mới đựa trên token hiện tại.
* tải của yêu cầu POST giảm đáng kê so với password.
* có nhiều lý do để token được sử dụng, như khi refreshing thì token sẽ hết hạn và đổi từ unscoped sang scoped token.

###3.7 Access Management and Authorization
* Keystone tạo ra policy Role-Based Access Controll (RBAC) được thực thi trên mỗi API public endpoint. Những chính sách này được lưu thành 1 file trên đĩa, có tên là policy.json

###3.8 Backends and Services
xanh: thường SQL
tím: LDAP hoặc SQL.
Xanh da trời: SQL hoặc Memcache.
policy: lưu ở file.





