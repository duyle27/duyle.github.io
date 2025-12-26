---
title: "Bài 8: Multicast UDP và Java RMI"
date: 2025-12-22
weight: 8
draft: false
tags: ["Java", "Best Practice"]
summary: "Giải thích Multicast UDP (gửi 1-n) và Java RMI (gọi phương thức từ xa) – nền tảng cho ứng dụng nhóm và hệ phân tán."
---

Buổi 8 gồm 2 phần lớn:

1. **Multicast UDP**: gửi dữ liệu từ **một nguồn** đến **nhiều máy nhận** cùng lúc (1 → N).
2. **Java RMI**: gọi phương thức từ xa giữa các ứng dụng Java như gọi hàm bình thường (Remote Method Invocation).

Đây là các kỹ thuật thường gặp trong **streaming**, **trò chuyện nhóm**, **giám sát từ xa** và **hệ thống phân tán**.

---

## 1) UDP Multicast

### Unicast, Broadcast và Multicast

Trong mạng máy tính có 3 kiểu gửi dữ liệu phổ biến:

- **Unicast**: 1 nguồn → 1 đích  
  Ví dụ: bạn mở một trang web, server trả dữ liệu cho riêng bạn.

- **Broadcast**: 1 nguồn → tất cả máy trong mạng nội bộ  
  Ví dụ: một thông báo phát cho toàn bộ LAN.

- **Multicast**: 1 nguồn → một **nhóm** máy đã đăng ký nhận dữ liệu  
  Ví dụ: server phát stream video cho nhóm người xem cùng lúc.

Multicast là cách truyền hiệu quả khi nhiều máy cùng cần nhận **một nội dung giống nhau**.

---

### Tại sao dùng UDP Multicast?

UDP Multicast thường được dùng vì:

- **Hiệu suất cao**: gửi 1 lần nhưng nhiều máy nhận → giảm tải mạng và server.
- **Đơn giản, nhanh**: UDP là kiểu “gửi và quên”, không phải chờ xác nhận.
- **Tối ưu trong LAN**: phù hợp môi trường mạng nội bộ (Local Area Network).
- **Rất hợp streaming**: audio/video cần tốc độ và độ trễ thấp hơn là đảm bảo tuyệt đối.

---

### Ứng dụng của Multicast

Multicast có thể dùng trong:

- Truyền phát multimedia trực tiếp (audio/video streaming)
- Trò chuyện nhóm, hội thoại nhóm
- Quảng bá thông tin cấu hình mạng và cập nhật trong hệ thống mạng
- Game nhiều người chơi (gửi trạng thái game cho nhiều người)
- Phát thông tin trạng thái/cảnh báo cho nhiều thiết bị
- Phát tán cập nhật phần mềm trong mạng nội bộ

---

### Địa chỉ Multicast IPv4

Trong IPv4:

- **Class D** là dải địa chỉ dành cho multicast  
- Dải địa chỉ multicast thường gặp: **224.0.0.0 – 239.255.255.255**

Một số nhóm multicast hay được nhắc:
- `224.0.0.1`: tất cả host trong local subnet

---

### Multicast Group là gì?

Multicast hoạt động theo “nhóm”:

- Một **multicast group** giống như một kênh.
- Máy nào muốn nhận dữ liệu phải **tham gia nhóm (join group)**.
- Khi rời nhóm thì **không nhận dữ liệu nữa**.

---

### Multicast trong Java

Java hỗ trợ multicast qua lớp:

- `java.net.MulticastSocket` (kế thừa từ `DatagramSocket`)

Một số thao tác quan trọng:
- `joinGroup(...)`: tham gia nhóm multicast
- `leaveGroup(...)`: rời nhóm multicast
- `setTimeToLive(ttl)`: đặt TTL (thời gian sống của gói tin, qua bao nhiêu router)

---

### Quy trình gửi dữ liệu Multicast (ý tưởng)

Các bước chung:

1. Tạo `MulticastSocket`
2. Tham gia nhóm multicast
3. Tạo `DatagramPacket` chứa dữ liệu + địa chỉ nhóm + port
4. Gửi packet
5. Rời nhóm

Ví dụ minh họa (ngắn, chỉ để hiểu):

```java
InetAddress group = InetAddress.getByName("230.0.0.1");
MulticastSocket socket = new MulticastSocket();
socket.joinGroup(group);

socket.setTimeToLive(5);

byte[] data = "Hello Multicast".getBytes();
DatagramPacket packet = new DatagramPacket(data, data.length, group, 9876);
socket.send(packet);

socket.leaveGroup(group);
socket.close();
```
## Quy trình nhận dữ liệu Multicast (ý tưởng)

Các bước chung:

1. Tạo `MulticastSocket` lắng nghe trên port
2. Tham gia nhóm multicast
3. Tạo `DatagramPacket` rỗng để nhận dữ liệu
4. Gọi `receive()` để chờ gói tin gửi về
5. Đọc dữ liệu từ packet
6. Rời nhóm và đóng socket

Ví dụ minh họa (ngắn):

```java
InetAddress group = InetAddress.getByName("230.0.0.1");
MulticastSocket socket = new MulticastSocket(9876);
socket.joinGroup(group);

byte[] buf = new byte[1024];
DatagramPacket packet = new DatagramPacket(buf, buf.length);
socket.receive(packet);

String msg = new String(packet.getData(), 0, packet.getLength());
System.out.println("Received: " + msg);

socket.leaveGroup(group);
socket.close();
```
## 2) Java RMI

### Java RMI là gì?

**RMI (Remote Method Invocation)** cho phép chương trình Java:

- Gọi phương thức của một đối tượng nằm trên **máy khác**
- Cảm giác giống như gọi method trong **cùng chương trình**

Nói đơn giản:

- Client gọi: `remoteObject.sum(1,2)`
- Server thực thi thật và trả kết quả về cho client

RMI giúp che giấu toàn bộ chi tiết mạng phía sau, người lập trình chỉ tập trung vào logic.

---

### Vì sao dùng Java RMI?

Java RMI được sử dụng vì:

- **Tích hợp sẵn trong Java**, dễ triển khai nếu cả client và server đều dùng Java
- **Gọi phương thức từ xa đơn giản** như gọi hàm thông thường
- **Hỗ trợ dữ liệu phức tạp**: đối tượng, mảng, class tự định nghĩa
- **Tự động xử lý giao tiếp mạng** thông qua cơ chế stub / skeleton
- Dựa trên **TCP/IP** nên tương đối **ổn định và tin cậy**

---

### Hạn chế của Java RMI

Một số điểm hạn chế cần lưu ý:

- **Phụ thuộc Java**: chủ yếu phù hợp Java ↔ Java
- Thiết kế dịch vụ phức tạp vẫn cần tổ chức tốt (đặc biệt khi dùng callback)
- **Quản lý phiên và kết nối** không mạnh như các giải pháp hiện đại
- **Bảo mật** cần cấu hình cẩn thận trong môi trường thực tế

---

### Ý tưởng kiến trúc RMI (hiểu nhanh)

Trong Java RMI có các thành phần chính:

- **Remote Interface**  
  Khai báo các phương thức cho phép gọi từ xa

- **Server Implementation**  
  Lớp cài đặt interface, chứa logic xử lý thật

- **RMI Registry**  
  Nơi đăng ký service để client tìm thấy server

- **Client**  
  Tìm (lookup) service trong registry và gọi phương thức

---

### Các bước triển khai RMI (tóm tắt)

1. Định nghĩa **Remote Interface**
2. Cài đặt interface bằng một class server (remote object)
3. Viết chương trình **server**: tạo object và bind vào registry
4. Viết chương trình **client**: lookup registry và gọi method
5. Chạy **RMI registry**, chạy server, sau đó chạy client

---

### Ví dụ ứng dụng RMI

Một số ví dụ thường gặp:

- Tính tổng 2 số từ xa
- Dịch vụ quản lý tài khoản ATM
- Dịch vụ giám sát nhiệt độ có **callback** (server gọi ngược lại client)

---

### Bài tập gợi ý theo đúng nội dung buổi 8

- **BT8.01**: Mô phỏng streaming video  
  (1 server phát, nhiều client nhận dữ liệu)

- **BT8.02**: Server quản lý tài khoản ATM bằng RMI  
  (thêm user, đăng nhập, gửi/rút tiền, lưu lịch sử giao dịch)

---

### Kết luận

Qua bài học này, bạn đã:

- Hiểu sự khác nhau giữa **Unicast, Broadcast và Multicast**
- Nắm được vì sao **Multicast UDP** phù hợp cho truyền dữ liệu nhóm và streaming
- Biết cách Java hỗ trợ multicast qua `MulticastSocket`
- Hiểu **Java RMI** và cơ chế gọi phương thức từ xa
- Chuẩn bị nền tảng cho các bài **ứng dụng mạng phân tán** và **đồ án học phần**
