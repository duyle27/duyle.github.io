---
title: "Bài 4: Quản lý địa chỉ kết nối mạng trong Java"
date: 2025-12-18
weight: 4
draft: false
tags: ["Java", "Socket", "File Transfer"]
summary: "Tìm hiểu cách Java quản lý địa chỉ mạng thông qua InetAddress, URL, URLConnection và DNS."
---

Trong lập trình mạng, trước khi chương trình có thể gửi hoặc nhận dữ liệu, điều quan trọng nhất là **xác định đúng địa chỉ kết nối mạng**. Nếu xác định sai địa chỉ, chương trình sẽ không thể giao tiếp, dù phần xử lý dữ liệu có đúng đến đâu.

Bài học này giúp bạn hiểu cách Java làm việc với **địa chỉ IP**, **tên miền (DNS)** và **URL**. Đây là nền tảng bắt buộc trước khi đi vào **Socket TCP, UDP, Web Crawler và Java RMI**.

---

## Địa chỉ kết nối mạng là gì?

Địa chỉ kết nối mạng là thông tin dùng để xác định **máy nào** trong mạng cần giao tiếp và **dịch vụ nào** trên máy đó sẽ xử lý dữ liệu.

Trong Internet, địa chỉ kết nối thường bao gồm:

- Địa chỉ IP
- Tên miền (Domain Name)
- Cổng (Port)
- Giao thức (Protocol)

---

## DNS – Domain Name System

Con người dễ nhớ tên miền hơn các dãy số IP.

DNS (Domain Name System) có nhiệm vụ ánh xạ **tên miền sang địa chỉ IP**, giúp client tìm đúng server cần kết nối.

Ví dụ:
- google.com tương ứng với nhiều địa chỉ IP khác nhau
- www.cs.yale.edu tương ứng với 128.36.229.30

Nhờ DNS, người dùng không cần ghi nhớ địa chỉ IP khi truy cập Internet.

---

## Địa chỉ IPv4

IPv4 là dạng địa chỉ phổ biến, gồm 32 bit, thường được viết thành bốn nhóm số.

Ví dụ:
- 192.168.1.1
- 8.8.8.8

Một số dải địa chỉ đặc biệt:
- 127.x.x.x là loopback (localhost)
- 224.0.0.0 – 239.255.255.255 là multicast

---

## Lớp InetAddress trong Java

Java cung cấp lớp InetAddress để làm việc với địa chỉ IP và DNS.

Lớp này thường được dùng để:
- Phân giải tên miền sang IP
- Kiểm tra loại địa chỉ
- Kiểm tra khả năng kết nối mạng

Ví dụ minh họa:

```java
InetAddress address = InetAddress.getByName("google.com");
```
---
## URL – Uniform Resource Locator

URL được dùng để xác định **tài nguyên trên Internet** như trang web, file hoặc dịch vụ.

Một URL đầy đủ thường bao gồm các thành phần sau:

- **Giao thức** (Protocol)
- **Tên miền** (Host)
- **Cổng** (Port)
- **Đường dẫn** (Path)
- **Chuỗi truy vấn** (Query)

Ví dụ một URL đầy đủ: https://www.hutech.edu.vn/admission?id=123

URL cho biết **lấy dữ liệu ở đâu** và **bằng cách nào**.

---

## Lớp URL trong Java

Lớp `URL` cho phép chương trình Java:

- Phân tích các thành phần của URL
- Kết nối tới server
- Đọc dữ liệu từ Internet

Ví dụ minh họa:

```java
URL url = new URL("https://www.example.com");
```
---
## Ý nghĩa của việc quản lý địa chỉ kết nối mạng

Hiểu rõ cách quản lý địa chỉ mạng giúp:

- Tránh kết nối sai server
- Chuẩn bị đúng địa chỉ cho Socket TCP/UDP
- Hiểu cách Web Crawler hoạt động
- Nắm vững mô hình Client – Server

---

## Kết luận

Qua bài học này, bạn đã:

- Hiểu khái niệm địa chỉ kết nối mạng
- Biết vai trò của DNS và IPv4
- Nắm được cách Java làm việc với `InetAddress`
- Hiểu cấu trúc và vai trò của URL
- Chuẩn bị nền tảng cho các bài học Socket TCP/UDP tiếp theo

