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
  
#### a.Các tiến trình FTP
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
  
#### b.Thiết lập kênh điều khiển
- Server-PI sẽ lắng nghe cổng TCP dành riêng cho kết nối FTP là cổng 21.
- User-PI sẽ tạo kết nối bằng việc sử dụng một cổng bất kỳ làm cổng nguồn trong phiên kết nối TCP mở một kết nối TCP từ thiết bị người dùng tới server trên cổng đó.
- Khi TCP đã cài đặt xong, kênh điều khiển sẽ được thiết lập, cho phép các lệnh từ User-PI tới Server-PI, và Server-PI sẽ đáp trả kết quả là các mã thông báo.
- Bước đầu tiên sau khi kênh đã đi vào hoạt động là bước đăng nhập của người dùng (login sequence).
- Bước này có hai mục đích:
  <ul>
  <li>Điều khiển truy cập: quá trình chứng thực cho phép hạn chế truy cập tới server với những người dùng nhất định. Nó cũng cho phép server điều khiển loại truy cập như thế nào đối với từng người dùng.</li>
  <li>Chọn nguồn cung cấp: Bằng việc nhận dạng người dùng tạo kết nối, FTP server có thể đưa ra quyết định sẽ cung cấp những nguồn nào cho người dùng đã được nhận dạng đó.</li>
  </ul>

#### c.Chứng thực trong FTP
- Chứng thực trong FTP khá đơn giản, chỉ là cung cấp username/password.
- Trình tự:
  <ul>
  <li>Người dùng gửi một username từ User-PI tới Server-PI bằng lệnh USER. Sau đó password của người dùng được gửi đi bằng lệnh PASS.</li>
  <li>Server kiểm tra tên người dùng và password trong database người dùng của nó. Nếu người dùng hợp lệ, server sẽ gửi trả một thông báo tới người dùng rằng phiên kết nối đã được mở. Nếu người dùng không hợp lệ, server yêu cầu người dùng thực hiện lại việc chứng thực. Sau một số lần chứng thực sai nhất định, server sẽ ngắt kết nối.</li>
  </ul>
  
#### d.Quản lý kênh dữ liệu:




