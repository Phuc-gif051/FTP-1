# FTP

## I.Khái niệm 
- File Transfer Protocol ( FTP ) là một chuẩn giao thức mạng sử dụng để chuyển các tập tin máy tính giữa một khách hàng và máy chủ trên một mạng máy tính.

## II.Cơ chế
### 1.Mô hình hoạt động
- FTP là một giao thức dạng client/server truyền thống.
- Ở đây thuật ngữ client thông thường được thay thế bằng thuật ngữ user – người dùng – do thực tế là người sử dụng mới là đối tượng trực tiếp thao tác các lệnh FTP trên máy clients.
- Bộ phần mềm FTP được cài đặt trên một thiết bị được gọi là một tiến trình. Phần mềm FTP được cài đặt trên máy Server được gọi là tiến trình Server-FTP, và phần trên máy client được gọi là tiến trình User-FTP.
- Mô hình FTP chia quá trình truyền thông giữa bộ phận Server với bộ phận Client ra làm hai kênh logic:
  <ul>
  <li>Kênh điều khiển: đây là kênh logic TCP được dùng để khởi tạo một phiên kết nối FTP. Nó được duy trì xuyên suốt phiên kết nối FTP và được sử dụng chỉ để truyền các thông tin điều khiển, như các lệnh và các hồi đáp trong FTP.</li>
  <li>Mỗi khi dữ liệu được truyền từ server tới client, một kênh kết nối TCP nhất định lại được khởi tạo giữa chúng. Dữ liệu được truyền đi qua kênh kết nối này – do đó nó được gọi là kênh dữ liệu. Khi file được truyền xong, kênh này được ngắt.</li>
  </ul>
  
### 2.Các tiến trình FTP
- Các tiến trình phía server: 
  <ul>
  <li>Server Protocol Interpreter (Server-PI): chịu trách nhiệm quản lý kênh điều khiển trên server. Nó lắng nghe yêu cầu kết nối hướng tới từ users trên cổng dành riêng. Khi kết nối đã được thiết lập, nó sẽ nhận lệnh từ phía User-PI, trả lời lại, và quản lý tiến trình truyền dữ liệu trên server.</li>
  <li>Server DataTransfer Process (Server-DTP): làm nhiệm vụ gửi hoặc nhận file từ bộ phận User-DTP. Server-DTP vừa làm nhiệm thiết lập kết nối kênh dữ liệu và lắng nghe một kết nối kênh dữ liệu từ user. Nó tương tác với server file trên hệ thống cục bộ để đọc và chép file.</li>
  </ul>
- Các tiến trình phía client:
  <ul>
  <li>User Protocol Interpreter (User-PI): chịu trách nhiệm quản lý kênh điều khiển phía client. Nó khởi tạo phiên kết nối FTP bằng việc phát ra yêu cầu tới phía Server-PI. Khi kết nối đã được thiết lập, nó xử lý các lệnh nhận được trên giao diện người dùng, gửi chúng tới Server-PI, và nhận phản hồi trở lại. Nó cũng quản lý tiến trình User-DTP.</li>
  <li>User Data Transfer Process (User-DTP): là bộ phận DTP nằm ở phía người dùng, làm nhiệm vụ gửi hoặc nhận dữ liệu từ Server-DTP. User-DTP có thể thiết lập hoặc lắng nghe yêu cầu kết nối kênh dữ liệu trên server. Nó tương tác với thiết bị lưu trữ file phía client.</li>
  <li>User Interface: cung cấp giao diện xử lý cho người dùng. Nó cho phép sử dụng các lệnh đơn giản hướng người dùng, và cho phép người điều khiển phiên FTP theo dõi được các thông tin và kết quả xảy ra trong tiến trình.</li>
  </ul>

### 3.Kênh điều khiển  
#### a.Thiết lập kênh điều khiển
- Server-PI sẽ lắng nghe cổng TCP dành riêng cho kết nối FTP là cổng 21.
- User-PI sẽ tạo kết nối bằng việc sử dụng một cổng bất kỳ làm cổng nguồn trong phiên kết nối TCP mở một kết nối TCP từ thiết bị người dùng tới server trên cổng đó.
- Khi TCP đã cài đặt xong, kênh điều khiển sẽ được thiết lập, cho phép các lệnh từ User-PI tới Server-PI, và Server-PI sẽ đáp trả kết quả là các mã thông báo.
- Bước đầu tiên sau khi kênh đã đi vào hoạt động là bước đăng nhập của người dùng (login sequence).
- Bước này có hai mục đích:
  <ul>
  <li>Điều khiển truy cập: quá trình chứng thực cho phép hạn chế truy cập tới server với những người dùng nhất định. Nó cũng cho phép server điều khiển loại truy cập như thế nào đối với từng người dùng.</li>
  <li>Chọn nguồn cung cấp: Bằng việc nhận dạng người dùng tạo kết nối, FTP server có thể đưa ra quyết định sẽ cung cấp những nguồn nào cho người dùng đã được nhận dạng đó.</li>
  </ul>

#### b.Chứng thực trong FTP
- Chứng thực trong FTP khá đơn giản, chỉ là cung cấp username/password.
- Trình tự:
  <ul>
  <li>Người dùng gửi một username từ User-PI tới Server-PI bằng lệnh USER. Sau đó password của người dùng được gửi đi bằng lệnh PASS.</li>
  <li>Server kiểm tra tên người dùng và password trong database người dùng của nó. Nếu người dùng hợp lệ, server sẽ gửi trả một thông báo tới người dùng rằng phiên kết nối đã được mở. Nếu người dùng không hợp lệ, server yêu cầu người dùng thực hiện lại việc chứng thực. Sau một số lần chứng thực sai nhất định, server sẽ ngắt kết nối.</li>
  </ul>
  
### 4.Kênh dữ liệu:
#### a.Active FTP (Kết nối kênh dữ liệu dạng chủ động)
Cơ chế:
- User-PI thiết lập một kết nối điều khiển từ cổng bất kỳ của nó là 1678 tới cổng điều khiển trên server là cổng 21.
- Server-PI sẽ báo cho Server-DTP khởi tạo một kênh kết nối TCP từ cổng 20 tới cổng 1678 của client.
- Khi client chấp nhận kênh được khởi tạo, dữ liệu sẽ được truyền đi.
- Thực tế, việc sử dụng cùng một cổng cho cả kênh dữ liệu và kênh điều khiển không phải là một ý hay, nó làm cho hoạt động của FTP trở nên phức tạp. Do đó, client nên chỉ định sử dụng một cổng khác bằng việc sử dụng lệnh PORT trước khi truyền dữ liệu.
- Ví dụ: Client chỉ định cổng 1742 với lệnh PORT. Server-DTP sau đó sẽ tạo ra một kết nối từ cổng 20 của nó tới cổng 1742 Client thay vì cổng 1678 như mặc định. Quá trình này được mô tả trong hình dưới đây.

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/7pbeFlM.png">

#### b.Passive FTP (Kết nối kênh dữ liệu dạng bị động)
- Client sẽ nhận server là phía bị động, làm nhiệm vụ chấp nhận một yêu cầu kết nối kênh dữ liệu được khởi tạo từ Client.
- Server trả lời lại Client với địa chỉ IP cũng như địa chỉ cổng mà nó sẽ sử dụng.
- Server-DTP sau đó sẽ lắng nghe một kết nối TCP từ User-DTP trên cổng này.

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/ngN4e7Y.png">

- Client sẽ sử dụng lệnh PASV để yêu cầu Server rằng nó muốn dùng phương thức điều khiển dữ liệu bị động. Server-PI sẽ trả lời lại Client với một giá trị cổng mà Client sẽ sử dụng, từ cổng 2223 trên nó. Sau đó Server PI sẽ hướng cho Server-DTP lắng nghe trên cổng 2223. User-PI cũng sẽ hướng cho User-DTP tạo một phiên kết nối từ cổng 1742 Client tới cổng 2223 Server. Sau khi Server chấp nhận kết nối này, dữ liệu bắt đầu được truyền đi.

#### c.Phương thức truyền dữ liệu
##### c1.Stream mode
- Đây là phương thức được sử dụng nhiều nhất trong triển khai FTP thực tế. Với một số lý do sau:
  <ul>
  <li>Là phương thức mặc định và đơn giản nhất, do đó việc triển khai là dễ dàng nhất.</li>
  <li>Nó xử lý với các file đều đơn thuần như là xử lý dòng byte, mà không để ý tới nội dung của các file.</li>
  <li>Là phương thức hiệu quả nhất vì nó không tốn một lượng byte “overload” để thông báo header.</li>
  </ul>
  
##### c2.Block mode
- Đây là phương thức truyền dữ liệu mang tính quy chuẩn hơn, với việc dữ liệu được chia thành nhiều khối nhỏ và được đóng gói thành các FTP blocks. 
- Mỗi block này có một trường header 3 byte báo hiệu độ dài, và chứa thông tin về các khối dữ liệu đang được gửi.
- Một thuật toán đặc biệt được sử dụng để kiểm tra các dữ liệu đã được truyền đi và để phát hiện, khởi tạo lại đối với một phiên truyền dữ liệu đã bị ngắt.

##### c3.Compressed mode
- Đây là một phương thức truyền sử dụng một kỹ thuật nén khá đơn giản, là “run-length encoding” – có tác dụng phát hiện và xử lý các đoạn lặp trong dữ liệu được truyền đi để giảm chiều dài của toàn bộ thông điệp.
- Thông tin khi đã được nén, sẽ được xử lý như trong block mode, với trường header.
- Trong thực tế, việc nén dữ liệu thường được sử dụng ở những chỗ khác, làm cho phương thức truyền kiểu compressed mode trở nên không cần thiết nữa. 
- Ví dụ: nếu bạn đang truyền đi một file qua internet với modem tương tự, modem của bạn thông thường sẽ thực hiện việc nén ở lớp 1; các file lớn trên FTP server cũng thường được nén sẵn với một số định dạng như ZIP, làm cho việc nén tiếp tục khi truyền dữ liệu trở nên không cần thiết.

## III.Cài đặt FTP server trên Centos 6
### Bước 1: 
- vsftpd là một gói phần mềm máy chủ FTP nhẹ cho CentOS (Linux). Bắt đầu cài đặt gói bằng lệnh sau:

`yum -y install vsftpd`

### Bước 2:
- Sau khi cài đặt, Mở */etc/vsftpd/vsftpd.conf* (tập tin cấu hình cho vsftpd). Thay YES thành NO ở hàng dưới đây:

`anonymous_enable = NO`

- Tìm và bỏ chú thích các dòng dưới đây:

`local_enable = YES`

`write_enable = YES`

`chroot_local_user = YES`

### Bước 3:
- Tạo một thư mục mà bạn muốn để lưu trữ dữ liệu FTP.
- Ví dụ tôi sẽ tạo ra trong /home như dưới đây.

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/gZuuIAL.png">

### Bước 4:
- Tạo tài khoản truy nhập FTP server

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/rUl0G81.png">

### Bước 5:
- Khởi động dịch vụ FTP server

`service vsftpd start`

- Tự động khởi động cùng hệ điều hành

`chkconfig --levels 235 vsftpd on`

### Bước 6:
- Tạo dữ liệu trong thư mục ftp

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/GDvdtpI.png">

### Bước 7:
- Truy nhập FTP server từ trình duyệt với địa chỉ  ftp://ftp-server-IP

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/LW54Llp.png">
- Đăng nhập tài khoản và kiểm tra kết quả

<img src="http://img.prntscr.com/img?url=http://i.imgur.com/kZXb1ou.png">

### Lưu ý:
- Khi không thể kết nối đến FTP server thì có thể do Iptables, SELinux trên server.
- Có nhiều cách để truy cập FTP server như dùng trình duyệt, sử dụng command line, winscp, filezilla ...

## IV. Tham khảo:
- http://sinhvienit.net/forum/tim-hieu-ve-giao-thuc-ftp.28754.html
- http://www.krizna.com/centos/how-to-configure-ftp-server-on-centos-6/


