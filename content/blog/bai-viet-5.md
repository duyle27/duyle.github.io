---
title: "Bài 5: Lập trình Socket TCP trong Java"
date: 2025-12-19
weight: 5
draft: false
tags: ["JavaScript", "Network"]
summary: "Giải thích mô hình Client–Server và cách lập trình Socket TCP trong Java – nền tảng cho giao tiếp mạng tin cậy."
---
Trong lập trình mạng, **TCP Socket** là cơ chế quan trọng nhất để xây dựng các ứng dụng **Client – Server** có độ tin cậy cao.  
Bài học này giúp bạn hiểu **TCP là gì**, **vì sao cần TCP**, và **cách Java triển khai TCP thông qua Socket và ServerSocket**.

Đây là bước chuyển tiếp trực tiếp từ kiến thức **địa chỉ mạng (IP, DNS, URL)** sang **giao tiếp dữ liệu thực tế giữa các chương trình**.

---

## TCP là gì?

**TCP (Transmission Control Protocol)** là giao thức truyền thông **hướng kết nối**, đảm bảo dữ liệu được truyền:

- **Đúng thứ tự**
- **Đầy đủ**
- **Không bị mất mát**
- **Không bị trùng lặp**

Trước khi truyền dữ liệu, TCP yêu cầu **thiết lập kết nối** giữa hai bên, và chỉ khi kết nối thành công thì dữ liệu mới được trao đổi.

---

## Vì sao sử dụng TCP?

TCP được sử dụng khi ứng dụng yêu cầu **độ tin cậy cao**, ví dụ:

- Ứng dụng chat
- Ứng dụng client–server
- Truyền file
- Game online theo lượt
- Hệ thống ngân hàng, thương mại điện tử

TCP cung cấp các cơ chế quan trọng:

- **Đảm bảo dữ liệu**: gói tin bị mất sẽ được gửi lại
- **Kiểm soát lỗi**: phát hiện và sửa lỗi trong quá trình truyền
- **Kiểm soát luồng**: tránh làm quá tải phía nhận
- **Giao tiếp hai chiều**: cho phép gửi và nhận dữ liệu đồng thời

---

## Cơ chế hoạt động của TCP
![Hình 5.1 – Socket TCP và cơ chế bắt tay 3 bước](images/tcp-socket-handshake.png)

**Hình 5.1 – Socket TCP và cơ chế bắt tay 3 bước (Three-way Handshake)**

Hình minh họa quá trình thiết lập kết nối TCP giữa Client và Server.  
Client khởi tạo `client socket` và gửi yêu cầu kết nối.  
Server lắng nghe qua `welcoming socket`, sau đó tạo `connection socket` để giao tiếp riêng với client.

Sau khi kết nối được thiết lập thành công, hai bên trao đổi dữ liệu hai chiều thông qua các luồng byte.


Quá trình giao tiếp TCP gồm ba giai đoạn chính:

1. **Thiết lập kết nối** (Bắt tay 3 bước – Three-way Handshake)
2. **Truyền dữ liệu**
3. **Giải phóng kết nối**

Nhờ cơ chế này, TCP đảm bảo rằng **hai bên luôn sẵn sàng** trước khi truyền và **kết thúc an toàn** sau khi hoàn tất.

---

## Mô hình Client – Server

Các ứng dụng TCP thường hoạt động theo mô hình **Client – Server**.

- **Server**:
  - Chạy trước
  - Lắng nghe yêu cầu kết nối
  - Cung cấp dịch vụ
  - Trả kết quả cho client

- **Client**:
  - Chạy sau
  - Chủ động kết nối tới server
  - Gửi yêu cầu
  - Nhận phản hồi từ server

Quá trình giao tiếp gồm:
1. Client gửi yêu cầu
2. Server xử lý
3. Server trả kết quả cho client

---

## Socket trong Java
![Hình 5.2 – Các bước kết nối TCP Socket giữa Client và Server](images/tcp-steps.png)

**Hình 5.2 – Các bước kết nối TCP Socket giữa Client và Server**

Quy trình kết nối TCP gồm các bước:

1. Server tạo `ServerSocket` và chờ kết nối
2. Client tạo `Socket` và gửi yêu cầu kết nối
3. Server chấp nhận kết nối bằng `accept()`
4. Hai bên trao đổi dữ liệu qua `InputStream` và `OutputStream`
5. Đóng socket sau khi hoàn tất

Mỗi client khi kết nối sẽ có một `Socket` riêng, giúp server xử lý đồng thời nhiều client.



Trong Java, **Socket** đại diện cho **điểm cuối giao tiếp** giữa client và server khi sử dụng TCP.

Socket cho phép:
- Gửi dữ liệu tới server
- Nhận dữ liệu từ server
- Giao tiếp hai chiều thông qua luồng nhập/xuất

Ví dụ minh họa tạo Socket phía client:

```java
Socket socket = new Socket("localhost", 8080);
```
---
## ServerSocket trong Java

ServerSocket được sử dụng ở phía **server** để:

- Mở một cổng lắng nghe
- Chờ client kết nối
- Chấp nhận kết nối và tạo Socket giao tiếp

ServerSocket đóng vai trò như **cửa ngõ**, cho phép các client kết nối vào server.

Ví dụ minh họa:

```java
ServerSocket server = new ServerSocket(8080);
```
## Luồng dữ liệu trong TCP Socket

Sau khi kết nối TCP được thiết lập, client và server trao đổi dữ liệu thông qua các luồng:

- **InputStream**: dùng để đọc dữ liệu từ socket
- **OutputStream**: dùng để ghi dữ liệu vào socket

Luồng dữ liệu này hoạt động tương tự như **I/O Stream** đã học ở bài trước,  
nhưng **nguồn và đích là mạng** thay vì file hay bàn phím.

---

## Ví dụ ứng dụng: Trò chơi Oẳn Tù Tì

Một bài tập tiêu biểu sử dụng TCP Socket là **trò chơi Oẳn Tù Tì** theo mô hình **Client – Server**.

### Đặc điểm

- Server quản lý luật chơi và điểm số
- Hai client gửi lựa chọn (kéo – búa – bao)
- Server xác định kết quả và gửi lại cho client
- Dữ liệu được truyền qua TCP để đảm bảo chính xác

Thông qua bài tập này, sinh viên hiểu rõ:

- Cách thiết lập kết nối TCP
- Cách trao đổi dữ liệu giữa nhiều client
- Cách xử lý lỗi mạng và ngoại lệ

---

## Ý nghĩa của lập trình TCP Socket

Việc nắm vững TCP Socket giúp bạn:

- Hiểu bản chất giao tiếp mạng
- Xây dựng hệ thống Client – Server thực tế
- Chuẩn bị cho các bài nâng cao như:
  - Đa tuyến (Multi-thread)
  - Socket UDP
  - Java RMI
  - Ứng dụng mạng phân tán

---

## Kết luận

Qua bài học này, bạn đã:

- Hiểu TCP là gì và vì sao cần TCP
- Nắm được mô hình Client – Server
- Biết vai trò của Socket và ServerSocket trong Java
- Hiểu cách dữ liệu được truyền qua TCP
- Chuẩn bị nền tảng cho các bài học UDP và xử lý đồng thời tiếp theo