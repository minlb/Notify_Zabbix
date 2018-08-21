Link tham khảo: https://www.youtube.com/watch?v=vVIoC2pFscA

Bước 1: Cài đặt và tải script :

    sudo apt-get update
    sudo apt-get install python-pip -y
    sudo pip install --upgrade pip
    sudo pip install requests

scripts :

    zbxtg.py
    https://drive.google.com/open?id=0B6H...

    zbxtg_settings.py
    https://drive.google.com/open?id=0B6H...

Bước 2: Copy vào thư mục alertscript (thư mục được định nghĩa trong file config zabibx-server)

    sudo cp zbxtg.py /usr/lib/zabbix/alertscripts/
    sudo cp zbxtg_settings.py /usr/libzabbix/alertscripts/
    sudo chmod 755 /usr/local/share/zabbix/alertscripts/zbxtg.py
    sudo chmod 755 /usr/local/share/zabbix/alertscripts/zbxtg_settings.py

Bước 3: Tạo bot trong botFather

Telegram botfather
/start
/newbot

=> Lấy được Token: 485183556:AAED64Qp5qx9bYrF9gHQCgIMMHI94yZ6mpc
 Và vào web https://api.telegram.org/bot485183556:AAED64Qp5qx9bYrF9gHQCgIMMHI94yZ6mpc/getUpdates
 sẽ lấy đượcusername: ở đâu user là: LeMinh95.

Bước 4: Chỉnh file

  - sudo vim /usr/lib/zabbix/alertscripts/zbxtg.py và gõ: set fileformat:unix và lưu lại
  - sudo vim /usr/lib/zabbix/alertscripts/zbxtg-setting.py

Chỉnh sửa các tham số theo hình: 
![](/image/j.png)
- Nhắn tin bất kỳ đến bot
- /usr/lib/zabbix/alertscripts/zbxtg.py "LeMinh95" "hello" "how are you ?"
![](/image/k.png)

Bước 5: Tạo Media Type
![](/image/a.png)


Bước 6: Tạo user trên web zabbix
![](/image/b.png)
![](/image/c.png)


Bước 7: Thêm media type trong user Admin
![](/image/d.png)

Bước 8: Tạo Action 
![](/image/e.png)
![](/image/f.png)
![](/image/g.png)

Bước 9: Kết quả:
![](/image/l.png)

Bước 10: Muốn thêm ảnh vào tin nhắn đẩy vào Tele thêm phần sau vào Defalt Message

    zbxtg;graphs
    zbxtg;graphs_period=10800
    
vào Bước 8 tạo Action 

Extra: Cài đặt Push Telegram vào một Channel
Bước 1: Tạo channel và add bot làm admin channel. Lấy con bot trên vào channel

Bước 2: Đặt tên cho channel. Ở đây channel là: nagios99

Bước 3: Nhâp địa chỉ: https://api.telegram.org/bot485183556:AAED64Qp5qx9bYrF9gHQCgIMMHI94yZ6mpc/sendMessage?chat_id=@nagios99_test&text=Test < Đây là khi channel public>
ta lấy được chatid của channel: -1001264597307

Bước 4: Đặt lại channel là private và test: https://api.telegram.org/bot485183556:AAED64Qp5qx9bYrF9gHQCgIMMHI94yZ6mpc/sendMessage?chat_id=-1001264597307_test&text=Test

Bước 5: Trên server thêm username của channel để zabbix biết đường gửi vào: vim /tmp/uids.txt
nagios99;private;-1001264597307

Bước 6. Thêm vào Media của User Admin với Send to: @nagios99
![](/image/o.png)

Bước 7: Kêt quả 
![](/image/p.png)
