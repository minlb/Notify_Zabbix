## Bước 1: Tạo tài khoản Slack
Vào trang chủ Slack: https://slack.com/. Tạo một tài khoản Admin và tạo workspace chỉ có Admin mới có quyền quản trị cả workspace đấy
PS: workspace là một group lớn nhất, thường nó đại diện cho cả công ty và Admin sẽ mời các member qua mail của họ vào workspace đấy. Ví dụ workspace là North-Kdata. Ở đây đặt tên workspace là ABC company

![](/image/s1.PNG)

## Bước 2: Sử dụng ứng dụng Incoming WebHook để push vào Slack

Incoming WebHook: là một ứng dụng trên Slack cho phép push message từ bên ngoài vào Slack thông qua một Webhook URL được tạo liên kết với một channel trong workspace.

### Thực hiện:
1. Vào phần Application và search Incoming WebHook
2. Chọn Add Configure

![](/image/s2.PNG)

3. Chọn Channel sẽ post vào. Sau đó ta sẽ được một webhook url. Cần Copy URL này 

![](/image/s3.PNG)

Ở đây ta chọn là channel là general vì kênh nào là kênh mặc định ban đầu có và các tất cả các thành viên mặc định bao đầu sẽ được add vào trong này

## Bước 3: Tạo file slack.sh trong thư mục: vim /usr/lib/zabbix/alertscripts/slack.sh
 Nội dung: 
 
    #!/bin/sh

    webhook_url=$1
    message=$2

    curl -k -X POST -d "payload={\"username\":\"zabbix\", \"text\":\"$message\"}" $webhook_url
 
 Gán quyền cho nó: chmod 755 /usr/lib/zabbix/alertscripts/slack.sh
 
 ## Bước 4: Cài trên trên Zabbix GUI
 
 * Vào Administration => Media types => Create media type. Cần nhớ webhook url đã tạo ở bước 2 paste vào phần Script parametter.
 
 ![](/image/s4.PNG)
 
* Vào Configuration => Action => Report problems to Zabbix administrators. Config theo hình 

![](/image/s5.PNG)

![](/image/s6.PNG)

![](/image/s7.PNG)

Ở đây chọn trigger Overload_network được tạo trong bài cảnh báo quan Telegram. Với điều kiện trigger là "Overload_network > 1M "
* Vào Administration => User => Admin => Media => Add 

![](/image/s8.PNG)

### Bước 5: Test 
Down một file băng lệnh wget để tạo trigger Overload_Network.
Kết quả:

![](/image/s9.PNG)
