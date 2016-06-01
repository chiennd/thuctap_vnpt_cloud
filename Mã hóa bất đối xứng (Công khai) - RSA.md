#Mã hóa bất đối xứng (Công khai) - Asymmetric encryption
#RSA
Thuật toán mã hóa RSA thoả mãn 5 yêu cầu của một hệ mã hiện đại: 
* Độ bảo mật cao (nghĩa là để giải mã được mà không biết khoá mật thì phải tốn hàng triệu năm).
* Thao tác nhanh(thao tác mã hoá và giải mã tốn ít thời gian).
* Dùng chung được.
* Có ứng dụng rộng rãi.
* Có thể dùng để xác định chủ nhân (dùng làm chữ ký điện tử). 

##Thuật toán RSA có hai Khóa: 

- Khóa công khai (Public key): được công bố rộng rãi cho mọi người và được dùng để **mã hóa**.

- Khóa bí mật (Private key): Những thông tin được mã hóa bằng khóa công khai chỉ có thể được **giải mã** bằng khóa bí mật tương ứng.

##Thuật toán
![](http://aita.gov.vn/Uploaded/rsa2.jpg)

Trong đó:
* C là bản mã hóa.
* M là bản rõ.



* Chọn 2 số nguyên tố p và q bất kỳ, p khác q.
* Tính n = p x q
* Tính phi(n) = (p-1) x (q-1)
* Chọn 1 số e bất kỳ sao cho 1<e<phi(n), đồng thời e và phi(n) là số nguyên tố cùng nhau.
* Tính d sao cho de đồng dư với 1 (mod n)

* Khi đó ta có khóa công khai bao gồm: e, n.
* Khóa bí mật bao gồm p,q,d,n

##Cách hoạt động.
###Mã hóa
Giả sử A muốn gửi cho B một thông điệp. 
* 1: A đã có khóa công khai của B. A sẽ chuyển nội dung thông điệp M thành một dãy số m. 
* 2: A sẽ mã hóa bằng cách dùng khóa công khai của B để mã hóa, theo công thức `c = m^e (mod n)`. m là bản rõ, c là bản mã hóa.
* 3: A gửi cho B đoạn mã c.

###Giải mã.
* 1: B sau khi nhận được đoạn mã c của A gửi, sẽ giải mã bằng công thức `m = c^d (mod n).
* 2: Sau đó, B sẽ chuyển nội dung m đã được giải mã thành nội dung thông điệp ban đầu M (theo quy ước của A)
* Trong RSA, thì để chuyển từ M sang m, dùng chuẩn PKCS


		
