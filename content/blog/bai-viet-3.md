---
title: "Bài 3: Quản lý luồng nhập xuất trong Java"
date: 2025-12-17
weight: 3
draft: false
tags: ["JavaScript", "Async"]
summary: "Tổng quan về Input/Output Streams trong Java – nền tảng cho xử lý file, console và lập trình mạng."
---

Trong lập trình Java, **luồng nhập xuất (Input / Output Streams)** đóng vai trò trung gian giúp chương trình **đọc và ghi dữ liệu** từ nhiều nguồn khác nhau như bàn phím, file, console, bộ nhớ và sau này là **socket mạng**.

Hiểu rõ cơ chế luồng nhập xuất là **bước nền bắt buộc** trước khi đi vào lập trình Socket TCP, UDP hay Java RMI.

---

## Luồng (Stream) là gì?

> **Luồng (Stream)** là dòng dữ liệu di chuyển  
> từ **nguồn → chương trình** hoặc từ **chương trình → đích**.

Khi Java đọc hoặc ghi dữ liệu, dữ liệu **không đi trực tiếp** mà luôn được **đóng gói và truyền qua luồng**.

---

## Luồng nhập xuất là gì?

Luồng nhập xuất là cơ chế cho phép chương trình:

- Nhận dữ liệu từ bên ngoài (Input)
- Ghi dữ liệu ra bên ngoài (Output)

Luồng có thể kết nối với nhiều nguồn và đích khác nhau:

- File
- Bàn phím
- Màn hình (console)
- Bộ nhớ
- Kết nối mạng (TCP / UDP)

---

## Dữ liệu trong luồng tồn tại dưới những dạng nào?

Trong Java, dữ liệu trong luồng tồn tại dưới **hai dạng chính**:

- **Luồng byte**
- **Luồng ký tự**

Việc chọn đúng loại luồng ảnh hưởng trực tiếp đến **hiệu năng và tính chính xác** của chương trình.

---

## Luồng byte trong Java

### Đặc điểm

- Dữ liệu được xử lý dưới dạng **nhị phân**
- Phù hợp với:
  - File nhị phân
  - Ảnh, video
  - Socket mạng
  - Dữ liệu phức tạp

### Các lớp gốc

- `InputStream` – luồng đọc byte
- `OutputStream` – luồng ghi byte  

> Cả hai đều là **lớp trừu tượng** (abstract).

---

### Các luồng byte đọc phổ biến

- `FileInputStream` – đọc byte từ file
- `ByteArrayInputStream` – đọc từ mảng byte
- `PipedInputStream` – đọc từ luồng pipe

---

### Các luồng byte ghi phổ biến

- `FileOutputStream` – ghi byte vào file
- `ByteArrayOutputStream` – ghi vào mảng byte
- `PipedOutputStream` – ghi vào luồng pipe

---

### Mô hình đọc/ghi dữ liệu với Stream

Quy trình chung khi làm việc với luồng:

1. Tạo đối tượng Stream
2. Gắn Stream với nguồn hoặc đích
3. Đọc/ghi dữ liệu trong vòng lặp
4. Đóng Stream sau khi hoàn tất

---

## Luồng ký tự trong Java

### Đặc điểm

- Xử lý dữ liệu dạng **Unicode**
- Phù hợp với:
  - File văn bản
  - Chuỗi
  - Dữ liệu hiển thị cho người dùng

### Các lớp gốc

- `Reader` – luồng đọc ký tự
- `Writer` – luồng ghi ký tự  

---

### Các luồng ký tự đọc phổ biến

- `FileReader` – đọc file văn bản
- `BufferedReader` – đọc có bộ đệm
- `StringReader` – đọc chuỗi
- `CharArrayReader` – đọc mảng ký tự

---

### Các luồng ký tự ghi phổ biến

- `FileWriter` – ghi vào file văn bản
- `BufferedWriter` – ghi có bộ đệm
- `PrintWriter` – ghi dữ liệu dạng dễ đọc
- `StringWriter` – ghi chuỗi

---

## Luồng lọc dữ liệu (Filtered Streams)

Luồng lọc dùng để **chuyển đổi dữ liệu** từ dạng byte/ký tự sang các kiểu dữ liệu khác.

### Các luồng lọc phổ biến

- `DataInputStream`
- `DataOutputStream`

Cho phép đọc/ghi các kiểu dữ liệu nguyên thủy:

- `int`
- `float`
- `double`
- `boolean`
- `char`

> Đây là cầu nối giữa **luồng thô** và **dữ liệu có kiểu** trong Java.

---

## Lưu trạng thái đối tượng – Serialization

### Serialization là gì?

**Serialization (chuỗi hóa)** là quá trình chuyển đối tượng Java thành luồng byte để:

- Lưu vào file
- Truyền qua mạng
- Phục hồi lại sau này

### Deserialization

Ngược lại, **Deserialization (giải chuỗi)** là quá trình:

- Đọc byte
- Tái tạo lại đối tượng ban đầu

Đây là nền tảng cho:
- Lưu dữ liệu chương trình
- Java RMI
- Truyền object qua mạng

---

## Try-with-resources (TWR)

Java hỗ trợ **Try-with-resources** để:

- Tự động đóng stream
- Tránh rò rỉ tài nguyên
- Code gọn và an toàn hơn

So với `try-catch` truyền thống, TWR:
- Ít lỗi hơn
- Dễ bảo trì hơn
- Được khuyến khích sử dụng

---

## Các lỗi I/O thường gặp

Khi làm việc với luồng, một số exception phổ biến là:

- `FileNotFoundException`
- `IOException`
- `EOFException`
- `UnsupportedEncodingException`
- `FileAlreadyExistsException`
- `AccessDeniedException`
- `SocketException`
- `MalformedURLException`
- `SerializationException`
- `InvalidPathException`

Việc **bắt và xử lý exception đúng cách** là yêu cầu bắt buộc trong lập trình Java.

---

## Liên hệ với môn Lập trình Mạng máy tính

Trong học phần **Lập trình Mạng máy tính (CMP180)**, luồng nhập xuất được sử dụng xuyên suốt trong:

- Lập trình Socket TCP
- Lập trình Socket UDP
- Truyền dữ liệu Client – Server
- Java RMI
- Multicast UDP

> Không hiểu I/O Stream → **không thể làm mạng đúng cách**.

---

## Kết luận

Qua bài học này, ta đã:

- Hiểu khái niệm luồng nhập xuất trong Java
- Phân biệt luồng byte và luồng ký tự
- Nắm được cách đọc/ghi dữ liệu với Stream
- Biết vai trò của Serialization và Filtered Streams
- Chuẩn bị nền tảng cho các bài học Socket TCP/UDP tiếp theo





---
Khi xử lý các tác vụ mạng (như gọi API), người mới học JS hay viết code lồng nhau kiểu này:

```javascript
// Cực hình Callback Hell
getData(function(a) {
    getMoreData(a, function(b) {
        getMoreData(b, function(c) {
            getMoreData(c, function(d) {
                console.log(d);
            });
        });
    });
});