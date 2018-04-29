
[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL :: MySQL 5.7 Tài liệu tham khảo :: 4.2.6 Sử dụng các file tùy chọn

### 4.2.6 Sử dụng các file tùy chọn

Hầu hết các chương trình MySQL chỉ có thể đọc các tùy chọn khởi đầu từ các file tùy chọn ( đôi khi nó được gọi là các file cấu hình). Các file tùy chọn cung cấp 1 cách thuận tiện để đặc tả các tùy chọn thường được sử dụng để chúng không cần được nhập vào dòng lệnh mỗi khi bạn chạy chương trình,

Để xác định lúc mà chương trình đọc các file tùy chọn, gọi nó với tùy chọn '\--help`. ( Với [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].)
Nếu chương trình đọc các file tùy chọn, tin nhắn hỗ trợ sẽ chỉ định các file mà nó tìm kiếm và các nhómóm tùy chọn nóó nhận ra.

Ghi chú:

1 chương trình MySQL bắt đầu với tùy chọn `\--no-defaults` sẽ đọc các file không tùy chọn thay vì `.mylogin.cnf`. 

Nhiều file tùy chọn là các file văn bản thần tùy, được tạo bằng cách sử dụng bất cứ trình biên soạn văn bản nào. Có ngoại lệ là khi file `.mylogin.cnf` chưa tùy chọn phần đăng nhập. Đây là  file được mã hóa được tạo bởi tiện ích [**mysql_config_editor**][4]. Xem  [Mục 4.6.6, "**mysql_config_editor** —  Cấu hình tiện tích MySQL"][4]. 1 "phần đăng nhập" là 1 nhóm tùy chọn mà cho phép những tùy chọn nhất đinh: `host`, `user`, `password`, `port` và `socket`. Chương trình khách đặc tả phần đăng nhập nào để đọc từ `.mylogin.cnf` sử dụng tùy chọn [`\--login-path`][5].

Để đặc tả 1 tên file phần đăng nhập thay thế, đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích kiểm thử **mysql-test-run.pl**, nhưng cũng được nhận diện bởi [**mysql_config_editor**][4] và bởi các máy khách  MySQL như [**mysql**][6], [**mysqladmin**][7], và vân vân.

MySQL tìm các file tùy chọn theo thứ tự được mô tả được thảo luận dưới đây và đọc bất cứ thứ gì tồn tại. Nêú 1 file tùy chọn bạn muốn sử dụng không còn tồn tại nữa, tạo nó bằng phương thức thích hợp, như mới thảo luận.

Trên Window, các chương trình MySQL đọc các tùy chọn khởi đầu từ các file được ghi trong bảng dưới đây, theo thứ tự cụ thể (các file được liệt kê trước được đọc trước,  rồi dần đến các file được đọc sau).

**Bảng 4.1 Các file tùy chọn đọc trên các hệ thống Window**

| Tên file                                                                                  | Mục đích                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Các tùy chọn chung                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Các tùy chọn chung                                               |  
| `C:my.ini`, `C:my.cnf`                                                                     | Các tùy chọn chung                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Các tùy chọn chung                                               |  
| `defaults-extra-file`                                                                      | Bất cứ file nào được xác định với [`\--defaults-extra-file`][8] |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Các tùy chọn phần đăng nhập (chỉ cho các máy khách)                             |  

Ở bảng trên, `%PROGRAMDATA%` đại diện cho thử mục hệ thống file chứa các dữ liệu ứng dụng cho tất cả người dùng trên máy chủ. Đường dẫn này mặc định là `C:ProgramData` trên Microsoft Windows Vista và lớn hơn, và `C:Documents and SettingsAll UsersApplication Data` trên các phiên bản cũ hơn của Microsoft Windows.

`%WINDIR%` đại diện cho vị trí của thư mục Windows của ban. Nó thường là `C:WINDOWS`. Sử dụng lệnh sau để xác định chính xác vị trí của nó từ giá trị của biến môi trường `WINDIR`.
    
    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thử mục dữ liệu ứng dụng Windows. Sử dụng câu lệnh sau để xác định chính xác vị trí của nó từ giá trị của biến môi trường `APPDATA`
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thử mục cài đặt cơ sở MySQL. Khi MySQL 5.7 được cài đặt sử dụng MySQL Installer, đường dẫn này thường là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` trong đó _`PROGRAMDIR`_  đại diện cho thư mục các chương trình (thường là `Program Filesiles` trên phiên bản tiếng Anh của Windows). Xem [Phần 2.3.3, "MySQL Installer cho Windows"][9]. 

Trên các hệ điều hành Unix và giống Unix, các chương trình MySQL đọc các tùy chọn khởi đầu từ các file được hiển thị trong bảng dưới đây, theo thứ tự nhất định ( các file được liệt kê trước được đọc trước, rồi tiếp đến các file đằng sau).

Lưu ý

Trên các nền tảng Unix, MySQL loại bỏ các file cấu hình mà có thể được ghi bởi bất kỳ ai. Điều này được dự định như là 1 phép đo bảo mật.


**Bảng 4.2 Các file tùy chọn đọc trên các hệ điều hành Unix và giống Unix**

| Tên file               | Mục đích                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Các tùy chọn chung                                                |  
| `/etc/mysql/my.cnf`     | Các tùy chọn chung                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Các tùy chọn chung                                               |  
| `$MYSQL_HOME/my.cnf`    | Các tùy chọn riêng cho máy chủ (chỉ áp dụng trên máy chủ)                         |  
| `defaults-extra-file`   | Bầy kỳ file nào được đặc tả với [`\--defaults-extra-file`][8] |  
| `~/.my.cnf`             | Các tùy chọn riêng cho người dùng                                        |  
| `~/.mylogin.cnf`        | Các tùy chọn đường dẫn đăng nhập riêng cho người dùng (chỉ cho các máy khách)               |  

Trong bảng trên, `~` đại diện cho thư mục home của người dùng hiện tại(giá trị của `$HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được đặc tả với tùy chọn [`SYSCONFDIR`][10] để **CMake** khi MySQL được xây dựng. Mặc định, đây là thứ mục `etc` được đặt trong thư mục cài đặt được biên dịch.

`MYSQL_HOME` là 1 biến môi trường chưa đường dẫn đến thư mục chứa file `my.cnf` riêng cho máy chủ. Nếu `MYSQL_HOME` không được đặt và bạn bắt đầu máy chủ sử dụng chương trình [**mysqld_safe**][11] , [**mysqld_safe**][11] sẽ đặt nó thành _`BASEDIR`_, thư mục cài đắt gốc của MySQL. 

_`DATADIR`_ thường sẽ là `/usr/local/mysql/data`, mặc dù nó có thể thay đổi với mỗi nền tảng hoặc cách thức cài đặt. Giá trị này là vị trí của thư mục dữ liệu được tạo ra khi MySQL được biên dịch, không phải là vị trí được đặc tả với tùy chọn [`\--datadir`][12]  khi [**mysqld**][1] khởi động. Sử dụng [`\--datadir`][12] trong lúc chạy không có hiệu quả nào vì máy chủ tìm kiếm các file tùy chọn và đọc trước khi thực thi bất cứ tùy chọn nào. 

Nếu nhiều phiên bản của các lựa chọn được tìm thấy, phiên bản cuối cùng sẽ được ưu tiên, với 1 ngoại lệ: Trong  **[mysqld**][1], phiên bản _đầu tiên_ của tùy chọn `[\--user`][13] được sử dụng như là 1 phòng ngữa an ninh, tránh  đặc tả người dùng bên trong file tùy chọn được ghi đè trong dòng lệnh.


Mô tả dưới đây của cú pháp file tùy chọn áp dụng cho các file mà bạn chình sửa bằng tay. Nó bao gồm  `.mylogin.cnf`, được tạo sử dụng  **[mysql_config_editor**][4] và được mã hóa. 

Bất cứ tùy chọn nào có thể được đưa ra trong dòng lệnh khi chạy 1 chương trình MySQL đều có thể được đưa ra trong các file tùy chọn. Để lấy danh sách các tùy chọn khả dụng cho chương trình, chạy nó với tùy chọn `\--help`. (Với **[mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].) 

Cú pháo để đặt tả các tùy chọn trong file tùy chọn tương tự như trong cú pháp dòng lệnh ( xem  [Phần 4.2.4, "Sử dụng các tùy chọn trong dòng lệnh"][14]).
Tuy nhiên, trong 1 file tùy chọn, bạn sẽ bỏ 2 dấu gạch ngang đầu trong tên tùy chọn và bạn đặc tả chỉ 1 tùy chọn cho mỗi dòng mà thôi. Ví dụ`\--quick` và `\--host=localhost` trong dòng lệnh nên được đặc tả thành `quick` và `host=localhost` trên các dòng riêng biệt trong file tùy chọn. Để đặc tả 1 tùy chọn cho định dạng `\--loose-_`tên_opt`_` trong file tùy chọn, hãy viết nó thế này `loose-_`opt_name`_`. 

Các dòng rỗng bên trong các file tùy chọn sẽ bi loại bỏ. Các dòng không rỗng có thể theo bất kỳ định dạng nào sau đây:

* `#_`comment`_`, `;_`comment`_`

Các dòng comment bắt đầu với `#` hoặc `;`. 1 comment `#` cũng có thể bắt đầu ở giữa của dòng.


* `[_`group`_]`

_`group`_ là tên của chương trình hay của nhóm mà bạn muốn cài các tùy chọn. Sau 1 nhóm dòng, bất kỳ dòng cài-đặt-tùy-chọn được áp dụng cho nhóm đã được nêu tên cho tới khi kết thúc của file tùy chọn hoặc 1 dòng nhóm khác được đưa ra. Tên của các nhóm tùy chọn không phân biệt hoa thường.

* `_`opt_name`_`

Cái này tương đương với `\--_`opt_name`_` trong dòng lệnh. 

* `_`opt_name`_=_`value`_`

Cái này tương đương với `\--_`opt_name`_=_`value`_` trong dòng lệnh. Trong file tù chọn, bạn có thể có các khoảng trống xung quanh kí tự `=`, đôi khi điều này lại không đúng trong dòng lệnh. Giá trị tùy chọn có thể được bao quanh bởi các kí tự nháy đơn hoặc nháy kép, sẽ rất hữu dụng nếu giá trị chứa kí tự comment `#`.

Các khoảng trống phía trước và sau sẽ tự động được xóa khỏi tên và các giá trị tùy chọn.


Bạn có thể sử dụng các escape sequence `b`, `t`, `n`, `r`, `\`, vaf `s`  trong các giá trị tùy chọn để đại diện cho các kí tự backspace, tab, newline, carriage return, backslash, và space. Trong các file tùy chọn, những luật escaping sau được áp dụng

* 1 kí tự backslash theo sau bởi 1 kí tự escape sequence hợp lệ sẽ được chuyển thành kí tự đại diện bởi sequence. Ví dụ, `s` sẽ được chuyển thành khoảng trắng.
* 1 kí tự backflash không theo sau bởi 1 kí tự escape sequence hợp lệ  sẽ không vì thay đổi. Ví dụ `S` vẫn giữ nguyên là nó


Các luật trên có nghĩa là 1 kí tự backslash đơn thuần có thể được đưa ra bởi `\`, hoặc như là `` nếu nó không được theo sau bởi kí tự escape sequence hợp lệ.

Các luật cho các escape sequence trong các file tùy chọn sẽ khác 1 chút so với các luật cho escape sequence trong chuỗi thông thường trong câu lệnh SQL. Trong hoàn cảnh này, nếu  "_`x`_" không phải là 1 kí tự escape sequence hợp lệ, `_`x`_` trở thành "_`x`_" thay vì `_`x`_`. Xem [Phần 9.1.1, "String Literals"][15]. 

Các luật escaping cho các giá trị file tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, nơi sử dụng `` để phân chia tên đường dẫn. 1 thành phần phân chia trong tên đường dẫn Windows phải được viết là `\` enếu nó được theo sau bởi kí tự escape sequence. Nó có thể được viết là `\` hoặc nếu không thì là ``. Ngoài ra, `\` có thể được sử dụng trong các tên đường dẫn Windows và có thể được coi như là ``. Giả sử rằng bạn muốn chỉ định thư mục gốc của `C:Program FilesMySQLMySQL Server 5.7` trong 1 file tùy chọn. Điều này có thể được thực hiện bằng 1 vài cách. 1 vài ví dụ sau đây:
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu 1 tên nhóm lựa chọn trùng với tên chương trình, các lựa chọn trong group sẽ áp dụng riêng cho chương trình đó. Ví dụ `[mysqld]`  và nhóm `[mysql]` áp dụng cho lần lượt máy chủ  **[mysqld**][1]  và chương trình khách **[mysql**][6]. 


Nhóm tùy chọn `[client]` sẽ được đọc bởi tất cả các chương trình máy khách cung cấp trong các phân phối của MySQL.(nhưng _không phải_ bởi **[mysqld**][1]). Để hiểu cách mà các chương trình khách bên-thứ-3 sử dụng C API có thể sử dụng các file tùy chọn, xem tài liệu C API tại [Phần 27.8.7.50, "mysql_options()"][16]. 

Nhóm  `[client]` cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các máy khách. Ví dụ,  là 1 nhóm thích hợp để đặc tả mật khẩu để kết nối tới máy chủ. (Nhưng đảm bảo rằng file tùy chọn chỉ có thể truy cập bởi mình bạn, để những người khác không thể tìm được mật khẩu của bạn.) Chắc chắn rằng không đặt 1 tùy chọn nào trong nhóm `[client]`  trừ khi nó được nhận ra bởi _tất cả_ chương trình khách bạn sử dụng. Các chương trình không hiểu các tùy chọn sẽ thoát sau khỉ hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng.


Liệt kê thêm những nhóm lựa chọn chung  và sau đó là các nhóm đặc thù. Ví dụ, 1 nhóm `[client]` sẽ phổ biến hơn vì nó được đọc bởi tất cả các chương trình khách, trong khi 1 nhóm  `[mysqldump]` chỉ được đọc bởi **[mysqldump**][17].
Các tùy chọn đặc thù sau sẽ ghi đè các tùy chọn đặc thù trước đó, vì vậy hay đặt các nhóm tùy chọn theo thứ tự  `[client]`, `[mysqldump]` cho phép **[mysqldump**][17]-tùy chọn đặc thù để ghi đè các tùy chọn `[client]`. 

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

Để tạo nhóm tùy chọn chỉ có thể đọc bởi các máy chủ **[mysqld**][1] từ các chuỗi phát hành MySQL chỉ định, sử dụng các nhóm với các cái tên như `[mysqld-5.6]`, `[mysqld-5.7]`, và vân vân. Các nhóm sau dây chỉ định rằngcài đặt `[sql_mode`][18] chỉ nên được sử dụng bởi các máy chủ MySQL với phiên bản 5.7.x.

    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng trực tiếp `!include`  trong các file tùy chọn để thêm các file tùy chọn khác và  `!includedir`  để tìm kiểm các thư mục chỉ định cho các file tùy chọn. Ví dụ, để thêm file  `/home/mydir/myopt.cnf` , sử dụng chỉ thị sau:
    
    
    !include /home/mydir/myopt.cnf

Để tìm thư mục  `/home/mydir`  và đọc các file tùy chọn được tìm thấy trong đó, sử dụng chỉ thị này

    
    
    !includedir /home/mydir


MySQL không đảm bảo về thứ tự  đọc của các file tùy chọn trong thư mục.

Lưu ý 

Bất cứ file nào được tìm thấy và thêm vào bằng cách sửử dụng chỉ thị `!includedir` trong các hệ điều hành  `phải` có tên file kết thục bằng  `.cnf`. Trên Windows, chỉ thị này sẽ kiểm tra các file với phần mở rộng là `.ini` hoặc `.cnf` . 

Viết nội dung của các file tùy chọn được thêm như các file tùy chọn khác. Có nghĩa là, nó nên bao gồm các nhóm tùy chọn, mỗi nhóm được đứng trước bởi 1 dòng `[_`group`_]`  chỉ định chương trình mà tùy chọn áp dụng.

Trong khi 1 file được thêm đang được xử lý, chỉ những tùy chọn trong các nhóm mà chương trình hiện tại tìm kiếm được sử dụng. Các nhóm khác sẽ bị loại bỏ. Giả sử rằng file  `my.cnf` chứa dòng sau: 
    
    
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/home/mydir/myopt.cnf` trông nhưu thế này: 
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi **[mysqld**][1], chỉ có nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng.  Nếu file được xử lý bởi **[mysqladmin**][7], chỉ nhóm `[mysqladmin]` được sử dụng. Nếu file được xử lý bởi các chương trình khác, không có tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng

Chỉ thị `!includedir`được xử lý tương tự trừ khi tất cả các file tùy chọn trong thư mục được gọi được đọc

Nếu 1 file tùy chọn bao gồm các chỉ thị`!include` hoặc `!includedir`các file được gọi bởi các chỉ thị đó được xử lý mỗi khi file tùy chọn được xử lý, không quan tâm đến việc chúng xuất hiện ở đâu trong file


[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

  
