
[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL :: MySQL 5.7 Tài liệu tham khảo :: 4.2.6 Sử dụng các file tùy chọn

### 4.2.6 Sử dụng các file tùy chọn

Hầu hết các chương trình MySQL có thể đọc các tùy chọn khởi động từ các file tùy chọn (đôi khi được gọi là file cấu hình). Các file tùy chọn cung cấp một cách thuận tiện để chỉ định các tùy chọn thường được sử dụng để chúng không được nhập vào dòng lệnh mỗi khi bạn chạy một chương trình.

Để xác định xem một chương trình có đọc các file tùy chọn hay không, hãy gọi nó bằng tùy chọn `\--help` . (Đối với [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].) Nếu chương trình đọc các file tùy chọn, thông báo trợ giúp cho biết file nào sẽ hiển thị và nhóm tùy chọn nào nhận ra.

> Ghi chú 

> Một chương trình MySQL bắt đầu bằng tùy chọn `\--no-defaults` không đọc các file tùy chọn nào ngoài `.mylogin.cnf`. 

Nhiều file tùy chọn là các file văn bản thuần túy, được tạo bằng bất kỳ trình soạn thảo văn bản nào. Ngoại lệ là file `.mylogin.cnf` chứa các tùy chọn đường dẫn đăng nhập. Đây là một file được mã hóa được tạo bởi tiện ích [**mysql_config_editor**][4]. Xem [Mục 4.6.6, "**mysql_config_editor** — Tiện ích cấu hình MySQL"][4]. Một "đường dẫn đăng nhập" là một nhóm tùy chọn chỉ cho phép một số tùy chọn nhất định:  `host`, `user`, `password`, `port` và `socket`.  Chương trình khách hàng chỉ định đường dẫn đăng nhập nào được đọc từ `.mylogin.cnf` bằng cách sử dụng tùy chọn [`\--login-path`][5]. 

Để chỉ định tên file đường dẫn đăng nhập thay thế, hãy đặt biến môi trường  `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích thử nghiệm **mysql-test-run.pl**, nhưng cũng được nhận diện bởi [**mysql_config_editor**][4] và bởi các máy khách MySQL như [**mysql**][6], [**mysqladmin**][7], v.v. 

MySQL tìm kiếm các file tùy chọn theo thứ tự được mô tả trong phần thảo luận sau và đọc bất kỳ file nào tồn tại. Nếu một file tùy chọn bạn muốn sử dụng không tồn tại, hãy tạo nó bằng cách sử dụng phương thức thích hợp, như được thảo luận. 

Trên Windows, các chương trình MySQL đọc các tùy chọn khởi động từ các file được hiển thị trong bảng sau, theo thứ tự được chỉ định (các file được liệt kê đầu tiên được đọc trước tiên, các file được đọc sau được ưu tiên). 

**Bảng 4.1 Các file tùy chọn đọc trên các hệ thống Window**

| Tên file                                                                                  | Mục đích                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Tùy chọn chung                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Tùy chọn chung                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | Tùy chọn chung                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Tùy chọn chung                                                |  
| `defaults-extra-file`                                                                      | File được xác định với [`\--defaults-extra-file`][8], nếu có |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Tùy chọn đường dẫn đăng nhập (chỉ cho các máy khách)                             |  

Trong bảng trước, `%PROGRAMDATA%` đại diện cho thư mục hệ thống file chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ lưu trữ. Đường dẫn này mặc định là `C:ProgramData` trên Microsoft Windows Vista trở lên, và `C:Documents and SettingsAll UsersApplication Data` trên các phiên bản cũ hơn của Microsoft Windows. 

`%WINDIR%` represents the location of your Windows directory. This is commonly `C:WINDOWS`. Use the following command to determine its exact location from the value of the `WINDIR` environment variable: 
    
    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho vị trí của thư mục dữ liệu ứng dụng Windows. Sử dụng lệnh sau đây để xác định vị trí chính xác của nó từ giá trị của biến môi trường  `APPDATA` environment variable: 
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục cài đặt cơ sở MySQL. Khi MySQL 5.7 đã được cài đặt bằng cách sử dụng MySQL Installer, đường dẫn thường là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` trong đó _`PROGRAMDIR`_ đại diện cho thư mục chương trình (thường là`Program Files` trên các phiên bản tiếng Anh của Windows), [Phần 2.3.3, "Trình cài đặt MySQL cho Windows"] [9]. 

Trên các hệ thống giống Unix và Unix, các chương trình MySQL đọc các tùy chọn khởi động từ các file được hiển thị trong bảng sau, theo thứ tự được chỉ định (các file được liệt kê đầu tiên được đọc trước tiên, các file đọc sau được ưu tiên).

> Ghi chú 

> Trên các nền tảng Unix, MySQL bỏ qua các file cấu hình có thể ghi bởi bất kỳ ai. Đây là cố ý như một biện pháp an ninh. 

**Bảng 4.2 Các file tùy chọn đọc trên các hệ điều hành Unix và giống Unix**

| Tên file               | Mục đích                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Các tùy chọn chung                                                |  
| `/etc/mysql/my.cnf`     | Các tùy chọn chung                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Các tùy chọn chung                                                |  
| `$MYSQL_HOME/my.cnf`    | Tùy chọn riêng cho máy chủ (chỉ dành cho máy chủ)                         |  
| `defaults-extra-file`   | file được xác định với [`\ - defaults-extra-file`] [8], nếu có |  
| `~/.my.cnf`             | Tùy chọn dành riêng cho người dùng                                         |  
| `~/.mylogin.cnf`        | Tùy chọn đường dẫn đăng nhập dành riêng cho người dùng (chỉ dành cho khách hàng)               |  

Trong bảng trước, `~` đại diện cho thư mục home của người dùng hiện tại (giá trị của `$ HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được chỉ định với tùy chọn [`SYSCONFDIR`] [10] để ** CMake ** khi MySQL được xây dựng. Theo mặc định, đây là thư mục `etc` nằm trong thư mục cài đặt đã biên dịch. 

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục chứa file `my.cnf` riêng của máy chủ. Nếu `MYSQL_HOME` không được đặt và bạn khởi động máy chủ bằng chương trình [**mysqld_safe**][11] program, [**mysqld_safe**][11] hãy đặt nó thành _`BASEDIR`_, thư mục cài đặt gốc của MySQL. 

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo từng nền tảng hoặc phương pháp cài đặt. Giá trị là vị trí thư mục dữ liệu được xây dựng trong khi MySQL được biên dịch, không phải là vị trí được chỉ định với tùy chọn [`\--datadir`][12] khi [**mysqld**][1] bắt đầu. Sử dụng [`\--datadir`][12] khi chạy không ảnh hưởng đến nơi máy chủ tìm kiếm các file tùy chọn mà nó đọc trước khi xử lý bất kỳ tùy chọn nào.

Nếu tìm thấy nhiều phiên bản của một tùy chọn nhất định, phiên bản cuối cùng được ưu tiên, với một ngoại lệ: Đối với [**mysqld**] [1], phiên bản đầu tiên của tùy chọn `[\ - user`] [13] là được sử dụng như một biện pháp phòng ngừa an ninh, để ngăn người dùng được chỉ định một file tùy chọn bị ghi đè trên dòng lệnh.

Mô tả sau đây về cú pháp file tùy chọn áp dụng cho các file bạn chỉnh sửa theo cách thủ công. Điều này loại trừ `.mylogin.cnf`, được tạo ra bằng cách sử dụng [**mysql_config_editor**] [4] và được mã hóa.

Bất cứ tùy chọn nào có thể được đưa ra trong dòng lệnh khi chạy 1 chương trình MySQL đều có thể được đưa ra trong các file tùy chọn. Để lấy danh sách các tùy chọn khả dụng cho chương trình, chạy nó với tùy chọn `\--help`. (Đối với [**mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].) 

Cú pháp để chỉ định các tùy chọn trong một file tùy chọn tương tự như cú pháp dòng lệnh (xem [Phần 4.2.4, "Sử dụng Tùy chọn trên Dòng Lệnh"] [14]). Tuy nhiên, trong một file tùy chọn, bạn bỏ qua hai dấu gạch ngang hàng đầu từ tên tùy chọn và bạn chỉ định một tùy chọn trên mỗi dòng. Ví dụ, `\ - quick` và`\ - host = localhost` trên dòng lệnh nên được chỉ định là `quick` và`host = localhost` trên các dòng riêng biệt trong một file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu `\ - loose-_`opt_name`_` trong một file tùy chọn, hãy viết nó như là`loose-_`opt_name`_`. 

Các dòng trống trong các file tùy chọn bị bỏ qua. Các dòng không trống có thể có bất kỳ dạng nào sau đây:

* `#_`comment`_`, `;_`comment`_`

Dòng chú thích bắt đầu bằng `#` hoặc `;`. Nhận xét `#` cũng có thể bắt đầu ở giữa một dòng.

* `[_`group`_]`

_`group`_ là tên của chương trình hoặc nhóm mà bạn muốn đặt tùy chọn. Sau một dòng nhóm, mọi dòng thiết lập tùy chọn áp dụng cho nhóm được đặt tên cho đến khi kết thúc file tùy chọn hoặc một nhóm nhóm khác được cung cấp. Tên nhóm tùy chọn không phân biệt chữ hoa chữ thường. 

* `_`opt_name`_`

Điều này tương đương với `\ --_` opt_name`_` trên dòng lệnh.

* `_`opt_name`_=_`value`_`

Điều này tương đương với `\ --_` opt_name`_ = _ `value`_` trên dòng lệnh. Trong một file tùy chọn, bạn có thể có khoảng trống xung quanh ký tự `=`, một cái gì đó không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu ngoặc kép, rất hữu ích nếu giá trị chứa ký tự nhận xét `#`.

Không gian hàng đầu và cuối được tự động xóa khỏi tên và giá trị tùy chọn. 

Bạn có thể sử dụng các chuỗi thoát `b`,`t`, `n`,`r`, `\` và `s` trong các giá trị tùy chọn để biểu diễn backspace, tab, newline, dấu gạch chéo, dấu gạch chéo ngược và ký tự khoảng trắng . Trong các file tùy chọn, các quy tắc thoát này sẽ áp dụng:

* Dấu gạch chéo ngược theo sau là một ký tự chuỗi thoát hợp lệ được chuyển đổi thành ký tự được trình bày theo trình tự. Ví dụ, `s` được chuyển đổi thành một dấu cách.
* Dấu gạch chéo ngược không được theo sau bởi một ký tự chuỗi thoát hợp lệ vẫn không thay đổi. Ví dụ, `S` được giữ nguyên.

Các quy tắc trước có nghĩa là dấu gạch chéo ngược có thể được đưa ra dưới dạng `\\`, hoặc là `\` nếu nó không được theo sau bởi một ký tự chuỗi thoát hợp lệ. 

Các quy tắc cho các chuỗi thoát trong các file tùy chọn khác nhau một chút so với các quy tắc cho các chuỗi thoát trong các chuỗi ký tự trong các câu lệnh SQL. Trong bối cảnh sau, nếu "_`x`_" không phải là ký tự thứ tự thoát hợp lệ, `_`x`_` trở thành" _`x`_ "thay vì`_`x`_`. Xem [Mục 9.1.1, "Chuỗi văn bản"] [15].

Các quy tắc thoát cho các giá trị file tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, sử dụng `\` làm dấu tách tên đường dẫn. Dấu phân cách trong tên đường dẫn Windows phải được viết là `\\` nếu nó được theo sau bởi một ký tự chuỗi thoát. Nó có thể được viết dưới dạng `\\` hoặc `\` nếu không. Ngoài ra, `/` có thể được sử dụng trong tên đường dẫn Windows và sẽ được coi là `\`. Giả sử bạn muốn chỉ định một thư mục cơ sở của `C: Program FilesMySQLMySQL Server 5.7` trong một file tùy chọn. Điều này có thể được thực hiện theo nhiều cách. Vài ví dụ:
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên nhóm tùy chọn giống với tên chương trình, các tùy chọn trong nhóm sẽ áp dụng riêng cho chương trình đó. Ví dụ, các nhóm `[mysqld]` và `[mysql]` áp dụng cho máy chủ [**mysqld**] [1] và chương trình máy khách [**mysql**] [6], tương ứng. 

Nhóm tùy chọn `[client]` được đọc bởi tất cả các chương trình máy khách được cung cấp trong các bản phân phối MySQL (nhưng _không phải_ bởi [**mysqld**] [1]). Để hiểu cách các chương trình khách hàng của bên thứ ba sử dụng API C có thể sử dụng các file tùy chọn, hãy xem tài liệu C API tại [Mục 27.8.7.50, "mysql_options ()"] [16]. 

Nhóm `[client]` cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các máy khách. Ví dụ, `[client]` là nhóm thích hợp để sử dụng để chỉ định mật khẩu để kết nối với máy chủ. (Nhưng hãy chắc chắn rằng tập tin tùy chọn chỉ có thể truy cập được bởi chính bạn, để người khác không thể tìm ra mật khẩu của bạn.) Đảm bảo không đặt tùy chọn trong nhóm `[client]` trừ khi nó nhận ra được _tất cả_ các chương trình khách mà bạn sử dụng . Các chương trình không hiểu tùy chọn thoát sau khi hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng. 

Liệt kê các nhóm tùy chọn chung hơn trước tiên và các nhóm cụ thể hơn sau. Ví dụ, nhóm `[client]` sẽ phổ biến hơn vì nó được đọc bởi tất cả các chương trình khách, trong khi 1 nhóm `[mysqldump]` chỉ được đọc bởi [**mysqldump**][17]. Các tùy chọn đặc thù sau sẽ ghi đè các tùy chọn đặc thù trước đó, vì vậy hay đặt các nhóm tùy chọn theo thứ tự `[client]`, `[mysqldump]` cho phép [**mysqldump**][17]-tùy chọn đặc thù để ghi đè các tùy chọn `[client]`. 

Đây là các file tùy chọn chung điển hình:
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là các file tùy chọn người dùng điển hình:
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo các nhóm tùy chọn chỉ đọc bởi các máy chủ [**mysqld**] [1] từ chuỗi phát hành MySQL cụ thể, hãy sử dụng các nhóm có tên `[mysqld-5.6]`, `[mysqld-5.7]`, v.v. . Nhóm sau chỉ ra rằng cài đặt `[sql_mode`] [18] chỉ nên được sử dụng bởi các máy chủ MySQL có số phiên bản 5.7.x:
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng trực tiếp `! Include` trong các file tùy chọn để thêm các file tùy chọn khác và`! Includeir` để tìm kiếm các thư mục cụ thể cho các file tùy chọn. Ví dụ: để thêm file `/ home / mydir / myopt.cnf`, hãy sử dụng chỉ thị sau: 
    
    
    !include /home/mydir/myopt.cnf

Để tìm kiếm thư mục `/ home / mydir` và đọc các file tùy chọn được tìm thấy ở đó, hãy sử dụng chỉ thị này: 
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự các file tùy chọn trong thư mục sẽ được đọc.

> Ghi chú 

> Bất cứ file nào được tìm thấy và thêm vào bằng cách sử dụng chỉ thị  `!includedir` trong các hệ điều hành _phải_ có tên file kết thục bằng `.cnf`. Trên Windows, chỉ thị này sẽ kiểm tra các file với phần mở rộng là `.ini` hoặc `.cnf`. 

Viết nội dung của các file tùy chọn được thêm giống như bất kỳ file tùy chọn nào khác. Tức là, nó phải chứa các nhóm tùy chọn, mỗi nhóm đứng trước một dòng `[_`group`_]` chỉ ra chương trình mà các tùy chọn áp dụng. 

Trong khi file được thêm đang được xử lý, chỉ những tùy chọn trong nhóm mà chương trình hiện tại đang tìm kiếm được sử dụng. Các nhóm khác bị bỏ qua. Giả sử rằng file `my.cnf` chứa dòng này: 
    
    
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/ home / mydir / myopt.cnf` trông giống như sau:
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi [**mysqld**] [1], chỉ có nhóm`[mysqld] `trong`/ home / mydir / myopt.cnf` được sử dụng. Nếu file được xử lý bởi [**mysqladmin**] [7] thì chỉ sử dụng nhóm `[mysqladmin]`. Nếu file được xử lý bởi bất kỳ chương trình nào khác, không có tùy chọn nào trong `/ home / mydir / myopt.cnf` được sử dụng. 

Chỉ thị `! Includeir` được xử lý tương tự ngoại trừ tất cả các file tùy chọn trong thư mục có tên được đọc.

Nếu một file tùy chọn chứa các chỉ thị `! Include` hoặc`! Includeir`, các file được đặt tên bởi các chỉ thị đó được xử lý bất cứ khi nào file tùy chọn được xử lý, bất kể chúng xuất hiện ở đâu trong file.


  
