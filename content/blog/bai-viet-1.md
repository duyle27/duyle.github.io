---
title: "Bài 1: Làm sao biết máy mình đang dùng IP gì trong Java?"
date: 2025-12-15
weight: 1
draft: false
tags: ["Java", "Network"]
summary: "Sử dụng class InetAddress để định danh máy tính."
---

Trong lập trình mạng, trước khi kết nối, ta phải biết "địa chỉ nhà" (IP) của mình và đối phương. Java cung cấp class `InetAddress` để làm việc này.

---

## Code lấy IP của máy (Localhost)

```java
import java.net.InetAddress;

public class MyIP {
    public static void main(String[] args) {
        try {
            InetAddress myIP = InetAddress.getLocalHost();
            System.out.println("Tên máy: " + myIP.getHostName());
            System.out.println("Địa chỉ IP: " + myIP.getHostAddress());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
---

## Địa chỉ IP là gì trong lập trình mạng?

**IP (Internet Protocol Address)** là địa chỉ định danh của một thiết bị khi tham gia vào mạng máy tính.  
Trong lập trình mạng, IP đóng vai trò quan trọng trong việc:

- Xác định máy gửi và máy nhận
- Định tuyến dữ liệu trong quá trình truyền thông
- Thiết lập kết nối giữa các tiến trình chạy trên những máy khác nhau

Nếu không có địa chỉ IP, các máy tính sẽ không thể nhận biết và giao tiếp với nhau.

---

## Vai trò của lớp InetAddress trong Java

Lớp `InetAddress` đại diện cho một địa chỉ IP trong Java.  
Thông qua lớp này, lập trình viên có thể:

- Lấy địa chỉ IP của máy đang chạy chương trình
- Lấy tên máy (hostname)
- Phân giải tên miền sang địa chỉ IP
- Chuẩn bị thông tin để tạo Socket hoặc ServerSocket

Hầu hết các chương trình lập trình mạng trong Java đều sử dụng `InetAddress` ở bước khởi tạo.

---

## Phân tích ý nghĩa các lệnh trong chương trình

- `InetAddress.getLocalHost()`  
  → Trả về đối tượng đại diện cho máy cục bộ (localhost).

- `getHostName()`  
  → Lấy tên máy do hệ điều hành hoặc mạng gán.

- `getHostAddress()`  
  → Lấy địa chỉ IP của máy dưới dạng chuỗi.

Việc in ra hostname và IP giúp:
- Kiểm tra cấu hình mạng
- Xác định đúng máy khi kết nối Client – Server
- Debug lỗi mạng

---

## Một số lưu ý khi lấy IP trong Java

- Khi máy không kết nối mạng, IP có thể là `127.0.0.1`
- Khi máy có nhiều card mạng (WiFi, LAN, VPN), IP trả về có thể không phải IP Internet
- Trên Server, hostname và IP thường khác máy cá nhân

---

## Ứng dụng thực tế

Việc xác định IP của máy thường được sử dụng trong:
- Chương trình Client – Server
- Thông báo IP Server cho Client kết nối
- Kiểm tra kết nối mạng LAN
- Ghi log và giám sát hệ thống

---

## Kết luận

Qua bài học này, ta đã:
- Hiểu vai trò của địa chỉ IP trong lập trình mạng
- Biết cách lấy IP bằng lớp `InetAddress`
- Chuẩn bị nền tảng cho các kỹ thuật TCP, UDP và RMI



## Liên hệ với học phần Lập trình Mạng máy tính

Bài học này nằm trong nội dung mở đầu của học phần **Lập trình Mạng máy tính (CMP180)** tại Trường Đại học Công nghệ TP.HCM (HUTECH).

Trong lộ trình môn học, sinh viên sẽ lần lượt tiếp cận các nội dung:

- Kiến thức nền tảng về mạng và lập trình mạng
- Quản lý luồng nhập xuất (I/O)
- Quản lý địa chỉ và kết nối mạng
- Lập trình Socket TCP và UDP
- Xử lý đa tuyến, đa luồng
- Multicast UDP và Java RMI
- Thực hiện đồ án học phần cá nhân
---
## Mục tiêu của bài học

Thông qua Bài 1, sinh viên sẽ:

- Làm quen với môi trường lập trình mạng bằng Java
- Hiểu được vai trò của địa chỉ IP trong giao tiếp mạng
- Biết cách sử dụng lớp `InetAddress` để lấy IP và hostname
- Chuẩn bị nền tảng cho các bài học tiếp theo về Socket và Server – Client


---