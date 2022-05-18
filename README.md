# Pentmenu

**Một bash menu lựa chọn để khôi phục mạng nhanh chóng và dễ dàng và các cuộc tấn công DOS**

Sudo được thực hiện khi cần thiết.
Đã thử nghiệm trên Debian và Arch.

## Yêu cầu:
* bash
* sudo 
* curl
* netcat (phải hỗ trợ '-k' tùy chọn, biến thể openbsd được đề xuất)
* hping3 (hoặc nping có thể được sử dụng để thay thế cho các cuộc tấn công lũ lụt)
* openssl
* stunnel
* nmap
* whois (không cần thiết nhưng được ưu tiên)
* nslookup (hoặc 'host')
* ike-scan
* bind-tools (thường là một phần của 'bind' gói, cần thiết cho "host" và "nslookup")

## Làm thế nào để sử dụng?

- Tải xuống tập lệnh:

```
bash <(curl -Ls https://raw.githubusercontent.com/DauDau432/pentmenu/main/install-pentmenu)
```

Ngoài ra, hãy tải xuống bản phát hành mới nhất từ https://github.com/GinjaChris/pentmenu/releases, giải nén nó và chạy tập lệnh.

Hoặc sử dụng git clone:

```
git clone https://github.com/GinjaChris/pentmenu
```

## Chi tiết mô-đun

**GHI CHẾ ĐỘ**

* Show IP - sử dụng curl để thực hiện tra cứu IP bên ngoài của bạn. Chạy `ip a` hoặc `ifconfig` (nếu thích hợp) để hiển thị IP của giao diện cục bộ.

* DNS Recon - trinh sát thụ động, thực hiện tra cứu DNS (chuyển tiếp hoặc ngược lại nếu thích hợp với đầu vào mục tiêu) và tra cứu mục tiêu. Nếu không có whois, nó sẽ thực hiện tra cứu đối với ipinfo.io (chỉ hoạt động với IP, không hoạt động với tên máy chủ).

* Ping Sweep - sử dụng nmap để thực hiện phản hồi ICMP (ping) đối với máy chủ hoặc mạng mục tiêu.

* Quét nhanh - Máy quét Cổng TCP sử dụng nmap để quét các cổng đang mở bằng cách quét TCP SYN. Nmap sẽ không thực hiện quét ping trước khi thực hiện quét TCP SYN. Mô-đun này quét 1.000 cổng phổ biến nhất. Tất nhiên, mô-đun này có thể được sử dụng để quét một máy chủ duy nhất hoặc một mạng đầy đủ nhưng thực sự được thiết kế để xác định các mục tiêu trên một loạt địa chỉ IP. Quá trình quét này có thể mất nhiều thời gian để hoàn thành, hãy kiên nhẫn.

* Quét chi tiết - sử dụng nmap để xác định máy chủ lưu trữ trực tiếp, mở cổng, cố gắng xác định hệ điều hành, lấy biểu ngữ / xác định phiên bản phần mềm đang chạy. Nmap sẽ không thực hiện quét ping trước như một phần của quá trình quét này. Chuỗi User-Agent mặc định của Nmap được thay đổi thành chuỗi của trình duyệt Microsoft Edge trong chế độ này, để giúp tránh bị phát hiện qua HTTP. * Tất cả các cổng * TCP trên đích (tên máy chủ / IP / mạng con) đều được quét. Trong khi mô-đun có thể quét một máy chủ duy nhất hoặc nhiều máy chủ, mục đích sử dụng của nó là thực hiện quét thu thập thông tin đối với một hệ thống duy nhất. Quá trình quét này có thể mất nhiều thời gian để hoàn thành, hãy kiên nhẫn.

* Quét UDP - sử dụng nmap để quét các cổng UDP đang mở. * Tất cả các cổng * UDP đều được quét.

* Check Server Uptime - estimates the uptime of the target by querying an open TCP port with hping3. Accuracy of the results varies from one machine to another; this does not work against all servers.

* IPsec Scan - cố gắng xác định sự hiện diện của máy chủ IPsec VPN bằng việc sử dụng ike-scan và các đề xuất Giai đoạn 1 khác nhau. Bất kỳ đầu ra văn bản nào từ mô-đun này, cho dù liên quan đến "bắt tay" hoặc "không có đề xuất nào được chọn", đều cho biết sự hiện diện của máy chủ IPsec VPN. Xem http://nta-monitor.com/wiki/index.php/Ike-scan_User_Guide để có cái nhìn tổng quan tuyệt vời về ike-scan và VPN giai đoạn 1.

**CÁC CHẾ ĐỘ DOS**

* ICMP Echo Flood - sử dụng hping3 để khởi chạy lũ ICMP Echo truyền thống chống lại mục tiêu. Trên một hệ thống hiện đại, bạn có thể không đạt được nhiều thành tựu, nhưng sẽ rất hữu ích nếu bạn kiểm tra tường lửa để quan sát hành vi của chúng. Sử dụng 'Ctrl C' để kết thúc lũ.
Địa chỉ nguồn của các gói lũ có thể định cấu hình. Lưu ý rằng mục tiêu có thể là IP (tức là 127.0.0.1) hoặc tên máy chủ (tức là localhost.localnet.com). KHÔNG bao gồm giao thức như một phần của mục tiêu (tức là http://localhost.localnet.com).


* ICMP Blacknurse Flood - sử dụng hping3 để khởi chạy lũ ICMP chống lại mục tiêu. Các gói ICMP thuộc loại "Không thể truy cập đích, không thể truy cập cổng". Cuộc tấn công này có thể gây ra mức sử dụng CPU cao trên nhiều hệ thống. Sử dụng 'Ctrl C' để kết thúc cuộc tấn công. Xem http://blacknurse.dk/ để biết thêm thông tin. Địa chỉ nguồn của các gói lũ có thể định cấu hình.


* TCP SYN Flood - gửi một loạt các gói TCP SYN bằng cách sử dụng hping3. Nếu không tìm thấy hping3, nó sẽ cố gắng sử dụng tiện ích nmap-nping để thay thế. Hping3 được ưu tiên hơn vì nó gửi các gói nhanh nhất có thể. Các tùy chọn được cung cấp để sử dụng IP nguồn của giao diện của bạn hoặc chỉ định (giả mạo) một IP nguồn, hoặc giả mạo một IP nguồn ngẫu nhiên cho mỗi gói.

Theo tùy chọn, bạn có thể thêm dữ liệu vào gói SYN. Tất cả các gói SYN đều có bộ bit phân mảnh và sử dụng MTU ảo hpings 16 byte, đảm bảo phân mảnh.

Quay lại nmap-nping có nghĩa là gửi X số gói mỗi giây cho đến khi Y số gói được gửi và chỉ cho phép sử dụng IP giao diện hoặc một IP nguồn được chỉ định (giả mạo).

Lũ TCP SYN không có khả năng làm hỏng máy chủ, nhưng là một cách tốt để kiểm tra cơ sở hạ tầng chuyển mạch / bộ định tuyến / tường lửa và các bảng trạng thái.

Lưu ý rằng trong khi hping sẽ báo cáo giao diện gửi đi và IP có thể khiến bạn nghĩ rằng tập lệnh không hoạt động như mong đợi, thì IP nguồn * sẽ * được đặt như đã chỉ định; xem lại một gói tin lưu lượng truy cập nếu nghi ngờ!
Vì nguồn có thể xác định được, nên rất đơn giản để khởi động một cuộc tấn công LAND chẳng hạn (xem https://en.wikipedia.org/wiki/LAND).  Khả năng thiết lập nguồn cũng cho phép, ví dụ, gửi các gói SYN đến một mục tiêu và buộc các phản hồi SYN-ACK đến mục tiêu thứ hai.

* TCP ACK Flood - cung cấp các tùy chọn tương tự như SYN lũ, nhưng thay vào đó đặt cờ ACK (Acknowledgement) TCP.
Một số hệ thống sẽ dành quá nhiều chu kỳ CPU để xử lý các gói như vậy. Nếu IP nguồn được đặt thành IP của một kết nối đã thiết lập, thì có thể một kết nối đã thiết lập có thể bị gián đoạn bởi TCP ACK Flood 'mù' này. Cuộc tấn công này được coi là 'mù' vì nó không tính đến bất kỳ chi tiết nào của bất kỳ kết nối đã thiết lập nào (như số thứ tự hoặc số xác nhận).

* TCP RST Flood - cung cấp các tùy chọn tương tự như SYN lũ, nhưng thay vào đó đặt cờ RST (Đặt lại) TCP.
Một cuộc tấn công như vậy có thể làm gián đoạn các kết nối đã thiết lập nếu IP nguồn được đặt thành IP của kết nối đã thiết lập.
Xem https://en.wikipedia.org/wiki/TCP_reset_attack chẳng hạn.


* TCP XMAS Flood - tương tự như lũ SYN và ACK, với các tùy chọn tương tự, nhưng gửi các gói với tất cả các cờ TCP được đặt (CWR, ECN, URG, ACK, PSH, RST, SYN, FIN). Gói tin được coi là 'sáng lên như một cây christmase'. Về mặt lý thuyết, một gói tin như vậy yêu cầu nhiều tài nguyên hơn cho người nhận để xử lý so với một gói tin tiêu chuẩn.
Tuy nhiên, những gói tin như vậy là dấu hiệu của hành vi bất thường (chẳng hạn như một cuộc tấn công) và thường được IDS / IDP dễ dàng xác định.


* UDP Flood - giống như TCP SYN Flood nhưng thay vào đó sẽ gửi các gói UDP đến máy chủ lưu trữ: cổng được chỉ định. Giống như hàm TCP SYN Flood, hping3 được sử dụng nhưng nếu không tìm thấy nó, nó sẽ cố gắng sử dụng nmap-nping để thay thế. Tất cả các tùy chọn đều giống TCP SYN Flood, ngoại trừ bạn phải chỉ định dữ liệu để gửi trong các gói UDP.

Một lần nữa, đây là một cách tốt để kiểm tra thông lượng của bộ chuyển mạch / bộ định tuyến hoặc để kiểm tra hệ thống VOIP.


* SSL DOS - sử dụng OpenSSL để cố gắng DOS một máy chủ đích: cổng. Nó thực hiện điều này bằng cách mở nhiều kết nối và khiến máy chủ thực hiện các tính toán bắt tay tốn kém. Đây không phải là một đoạn mã đẹp đẽ hay thanh lịch, đừng mong đợi nó dừng lại ngay lập tức khi nhấn 'Ctrl c', nhưng nó có thể có hiệu quả khủng khiếp.

Tùy chọn thương lượng lại của khách hàng được đưa ra; nếu máy chủ đích hỗ trợ thương lượng lại do máy khách khởi xướng, thì tùy chọn này nên được chọn.
Ngay cả khi máy chủ mục tiêu không hỗ trợ máy khách thương lượng lại (ví dụ: CVE-2011-1473), vẫn có thể tác động / DOS máy chủ với cuộc tấn công này.
Sẽ rất hữu ích khi chạy điều này trên bộ cân bằng tải / proxy / máy chủ hỗ trợ SSL (không chỉ HTTPS, mà bất kỳ dịch vụ được mã hóa SSL hoặc TLS nào!) Để xem cách chúng đối phó với căng thẳng.

* Slowloris - sử dụng netcat để từ từ gửi Tiêu đề HTTP đến máy chủ đích: cổng với mục đích làm cạn kiệt tài nguyên của nó. Điều này có hiệu quả đối với nhiều, mặc dù không phải tất cả, máy chủ HTTP, miễn là các kết nối có thể được mở đủ lâu. Do đó, cuộc tấn công này chỉ có hiệu quả nếu máy chủ không giới hạn thời gian có sẵn để gửi một yêu cầu HTTP hoàn chỉnh.

Một số triển khai của cuộc tấn công này sử dụng các tiêu đề có thể xác định rõ ràng, điều này không xảy ra ở đây. Có thể định cấu hình số lượng kết nối để mở tới mục tiêu.

Khoảng thời gian giữa việc gửi mỗi dòng tiêu đề có thể định cấu hình, với giá trị mặc định là ngẫu nhiên từ 5 đến 15 giây. Ý tưởng là gửi tiêu đề chậm, nhưng không chậm đến mức máy chủ hết thời gian chờ đóng kết nối. Ví dụ: nếu chúng tôi gửi một dòng tiêu đề cứ sau 900 giây, thì khả năng là máy chủ sẽ đóng kết nối rất lâu trước khi chúng tôi gửi dòng tiêu đề thứ hai.
Tùy chọn sử dụng SSL (SSL / TLS) được đưa ra, yêu cầu đường hầm và cho phép tấn công được sử dụng chống lại máy chủ HTTPS. Bạn không sử dụng tùy chọn SSL đối với một máy chủ HTTP thuần túy.

Các biện pháp phòng thủ chống lại cuộc tấn công này bao gồm (nhưng không giới hạn):

Giới hạn số lượng kết nối TCP trên mỗi máy khách; điều này sẽ ngăn một máy duy nhất làm cho máy chủ không khả dụng, nhưng không hiệu quả nếu giả sử 10.000 máy khách khởi động cuộc tấn công đồng thời. Ngoài ra, một biện pháp phòng thủ như vậy có thể tác động tiêu cực đến nhiều máy khách (hợp pháp) hoạt động phía sau một máy chủ proxy chuyển tiếp.

Giới hạn thời gian có sẵn để gửi một yêu cầu HTTP hoàn chỉnh; điều này có hiệu quả vì cuộc tấn công dựa vào việc gửi từ từ các tiêu đề đến máy chủ (máy chủ phải đợi tất cả các tiêu đề từ máy khách trước khi phản hồi). Nếu máy chủ giới hạn thời gian nhận tất cả các tiêu đề của một yêu cầu là 10 giây (ví dụ), nó sẽ hạn chế nghiêm trọng hiệu quả của cuộc tấn công. Có thể một biện pháp như vậy sẽ ngăn không cho các khách hàng hợp pháp có kết nối chậm / mất kết nối truy cập vào trang web.


* IPsec DOS - sử dụng ike-scan để cố gắng làm ngập các gói IP được chỉ định với Chế độ chính và Chế độ tích cực Giai đoạn 1 từ các gói IP nguồn ngẫu nhiên. Sử dụng mô-đun IPsec Scan để xác định sự hiện diện của máy chủ IPsec VPN.


* Distraction Scan - đây không thực sự là một cuộc tấn công DOS mà chỉ đơn giản là khởi chạy nhiều lần quét TCP SYN, sử dụng hping3, từ một IP giả mạo mà bạn chọn (chẳng hạn như IP của kẻ thù tồi tệ nhất của bạn). Nó được thiết kế để trở thành một quá trình quét rõ ràng nhằm kích hoạt bất kỳ lDS / IPS nào mà mục tiêu có thể có và do đó hy vọng sẽ che khuất bất kỳ quá trình quét thực tế hoặc hành động nào khác mà bạn có thể đang thực hiện.


* DNS NXDOMAIN Flood - Cuộc tấn công này sử dụng netcat và được thiết kế để kiểm tra căng thẳng máy chủ DNS của bạn bằng cách gửi một loạt các truy vấn DNS cho (hầu hết) các miền không tồn tại cũng như một số yêu cầu DNS không đúng định dạng. Khi chạy với máy chủ DNS đệ quy, nó cố gắng kết nối máy chủ và lấp đầy bộ nhớ cache bằng các phản hồi tiêu cực, làm chậm / ngăn các truy vấn hợp pháp. Hoạt động tốt nhất được khởi chạy từ nhiều khách hàng đang tấn công. Sử dụng 'Ctrl c' để dừng cuộc tấn công.


**CÁC CHẾ ĐỘ CHIẾT XUẤT**

* Send File - Mô-đun này sử dụng netcat để gửi dữ liệu bằng TCP hoặc UDP. Nó có thể cực kỳ hữu ích để trích xuất dữ liệu.
Tổng kiểm tra md5 và sha512 được tính toán và hiển thị trước khi gửi tệp.
Tệp có thể được gửi đến một máy chủ mà bạn chọn; Trình nghe được thiết kế để nhận các tệp này.


* Listener - sử dụng netcat để mở trình nghe trên cổng TCP hoặc UDP có thể định cấu hình. Điều này có thể hữu ích để kiểm tra kết nối nhật ký hệ thống, nhận tệp hoặc kiểm tra quá trình quét đang hoạt động trên mạng. Mọi thứ mà người nghe nhận được sẽ được ghi vào ./pentmenu.listener.out hoặc một tệp bạn chọn.
Khi nhận tệp qua UDP, trình nghe phải được đóng theo cách thủ công bằng 'Ctrl C'. Điều này là do chúng ta phải buộc netcat luôn mở để nhận nhiều gói tin, vì UDP là một giao thức không kết nối.
Khi nhận tệp qua TCP, kết nối tự động đóng sau khi máy khách đóng kết nối của họ (sau khi tệp được chuyển) và tổng kiểm tra md5 và sha512 được tính cho tệp đã nhận.


## Tuyên bố từ chối trách nhiệm

Tập lệnh này chỉ dành cho việc sử dụng có trách nhiệm, được phép. Bạn chịu trách nhiệm cho các hành động của mình và tập lệnh này được cung cấp mà không có bảo hành hoặc đảm bảo dưới bất kỳ hình thức nào. (Các) tác giả không chịu trách nhiệm hoặc nghĩa vụ thay mặt cho bạn.


## Cũng thấy

Pentmenu có sẵn dưới dạng [bưu kiện](https://archstrike.org/packages/pentmenu) trên Arch Linux. Tình yêu lớn [ArchStrike](https://archstrike.org/) và [Parrot linux](https://www.parrotsec.org/).



