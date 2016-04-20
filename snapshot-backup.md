#Snapshot vs Backup

## Snapshot:
* Lưu lại ram, ổ cứng, thông tin máy ảo (cpu, ram, hdd,...)

* Khi thực hiện Snapshot, mỗi disk sẽ sinh ra một file disk mới. Dữ liệu sau này sẽ ghi vào file disk mới này.

* Khi xóa snapshot, lượng data của disk trước và disk sau sẽ merge lại với nhau chứ ko phải
delete snapshot là delete luôn disk. Bởi vì các disk này có mối liên hệ với nhau.

## Backup:
* Full Database Backups: Copy tất cả data files trong một database.
Tất cả những user data và database objects như system tables, indexes, user-defined tables đều được backup.

* Differential Database Backups: Copy những thay đổi trong tất cả data files kể từ lần full backup gần nhất.

* File or File Group Backups: Copy một data file đơn hay một file group.

* Differential File or File Group Backups: Tương tự như differential database backup nhưng chỉ copy những thay đổi
trong data file đơn hay một file group.

* Transaction Log Backups: Ghi nhận một cách thứ tự tất cả các transactions chứa trong transaction log file
kể từ lần transaction log backup gần nhất. Loại backup này cho phép ta phục hồi dữ liệu trở ngược lại
vào một thời điểm nào đó trong quá khứ mà vẫn đảm bảo tính đồng nhất (consistent).

