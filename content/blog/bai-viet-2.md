---
title: "Bài 2: Port là gì? Tại sao Server hay dùng cổng 8080?"
date: 2025-12-16
weight: 2
draft: false
tags: ["Network", "Theory"]
summary: "Hiểu vai trò của Port trong lập trình mạng và lý do cổng 8080 thường được sử dụng."
---

Trong lập trình mạng, nếu **IP** được xem như địa chỉ nhà của một máy tính trong mạng,  
thì **Port (cổng)** chính là số hiệu dùng để xác định **chương trình hoặc dịch vụ cụ thể** đang chạy trên máy đó.

Một máy tính có thể chạy đồng thời nhiều ứng dụng mạng như Web Server, Database Server, Game Server hoặc Chat Server.  
Mỗi ứng dụng bắt buộc phải gắn với **một Port riêng** để hệ điều hành có thể phân biệt và định tuyến dữ liệu chính xác.

## Port nằm ở đâu trong mô hình TCP/IP?

Trong bộ giao thức **TCP/IP**, Port thuộc về **Transport Layer (Tầng giao vận)** và được sử dụng bởi hai giao thức chính là **TCP** và **UDP**.

- Địa chỉ IP xác định **máy nào** sẽ nhận dữ liệu  
- Port xác định **ứng dụng nào trên máy đó** sẽ xử lý dữ liệu  

Sự kết hợp giữa **IP + Port** tạo thành một **Socket**, là điểm cuối (endpoint) của quá trình giao tiếp mạng.

## Các loại Port phổ biến

Port được đánh số từ **0 đến 65535** và được chia thành ba nhóm chính:
## Internet và các End Devices

Trước khi tìm hiểu về Port, cần hiểu Internet được cấu thành từ hàng tỷ thiết bị đầu cuối (End Devices) như máy tính, điện thoại, máy chủ và các thiết bị IoT.

Các thiết bị này kết nối với nhau thông qua nhiều lớp mạng khác nhau, và mỗi thiết bị khi tham gia giao tiếp mạng đều cần có địa chỉ IP và Port để định danh.
![Sơ đồ Internet và các End Devices](/img/internet-end-devices.png)
*Hình 2.1: Sơ đồ minh họa Internet và các thiết bị đầu cuối (End Devices) kết nối thông qua các mạng truy cập và mạng lõi.*

Trong quá trình giao tiếp mạng, một thiết bị không chỉ cần địa chỉ IP để xác định vị trí trong mạng,
mà còn cần Port để xác định chính xác ứng dụng hoặc dịch vụ đang lắng nghe trên thiết bị đó.

Do đó, Port đóng vai trò như “điểm tiếp nhận” (endpoint) cho các tiến trình mạng chạy trên cùng một máy.


## Mô hình các tầng giao thức TCP/IP

Để hiểu rõ Port hoạt động ở đâu và vì sao Server cần Port,
ta cần nắm được mô hình giao thức TCP/IP – nền tảng của mọi ứng dụng mạng hiện nay.

![Các tầng giao thức TCP/IP](/img/tcp-ip-layers.png)

*Hình 2.2: Mô hình các tầng giao thức TCP/IP và các giao thức tiêu biểu tương ứng với từng tầng.*

Trong mô hình TCP/IP, địa chỉ IP hoạt động ở tầng Internet,
giúp xác định thiết bị trong mạng.

Trong khi đó, Port hoạt động ở tầng Transport (TCP/UDP),
giúp xác định chính xác ứng dụng hoặc dịch vụ đang giao tiếp trên thiết bị đó.



### Well-known Ports (0 – 1023)

Được dùng cho các dịch vụ chuẩn:
- Port 80: HTTP  
- Port 443: HTTPS  
- Port 21: FTP  
- Port 25: SMTP  

### Registered Ports (1024 – 49151)

Thường dùng cho các ứng dụng phổ biến:
- Port 3306: MySQL  
- Port 8080: Web Server thay thế port 80  

### Dynamic / Private Ports (49152 – 65535)

Do hệ điều hành cấp phát tạm thời cho Client khi giao tiếp mạng.

## Vì sao Server thường dùng cổng 8080?

Cổng **8080** rất phổ biến trong quá trình phát triển và học tập vì:

- Tránh xung đột với cổng 80 đang được hệ điều hành chiếm dụng  
- Không cần quyền Administrator khi mở Server  
- Phù hợp cho các Web Server thử nghiệm như:
  - Apache Tomcat  
  - Spring Boot  
  - Node.js  

## Lỗi thường gặp: “Address already in use”

Lỗi này xảy ra khi Server cố gắng mở một Port đã bị chương trình khác chiếm dụng.

Nguyên nhân phổ biến:
- Server cũ chưa được tắt  
- Port đang được dùng bởi dịch vụ khác  
- Chạy nhiều Server cùng lúc  

Cách khắc phục:
- Đổi sang Port khác (ví dụ: 8080 → 9090)  
- Kiểm tra Port đang sử dụng trong hệ điều hành  
- Đảm bảo chỉ có một Server chạy tại một thời điểm  

## Liên hệ với môn Lập trình Mạng máy tính

Trong học phần **Lập trình Mạng máy tính (CMP180)**, khái niệm Port sẽ được sử dụng xuyên suốt trong:

- Lập trình Socket TCP  
- Lập trình Socket UDP  
- Xây dựng mô hình Client – Server  
- Java RMI và Multicast  

Việc hiểu rõ Port là nền tảng bắt buộc trước khi triển khai các ứng dụng mạng thực tế.

## Kết luận

Qua bài học này, ta đã:
- Hiểu được Port là gì và vai trò của Port trong lập trình mạng  
- Nắm được mối quan hệ giữa IP, Port và Socket  
- Giải thích được lý do Server thường sử dụng cổng 8080  
- Chuẩn bị kiến thức nền cho các bài học tiếp theo về TCP và UDP  
