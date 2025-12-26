---
title: "Bài 7: Lập trình đa tuyến và xử lý đồng thời trong Java"
date: 2025-12-21
weight: 7
draft: false
tags: ["JavaScript", "JSON"]
summary: "Giải thích lập trình đa tuyến và xử lý đồng thời trong Java – nền tảng cho các ứng dụng mạng nhiều client và hệ thống phân tán."
---

Trong các ứng dụng mạng thực tế, đặc biệt là **Server**, chương trình thường phải xử lý **nhiều yêu cầu cùng lúc**.  
Nếu chỉ sử dụng một luồng thực thi, server sẽ bị **tắc nghẽn**, các client phải chờ đợi và hiệu năng giảm nghiêm trọng.

Vì vậy, **lập trình đa tuyến (Multithreading)** và **xử lý đồng thời (Concurrency)** là kỹ thuật bắt buộc trong lập trình mạng.

---

## Tuyến (Thread) là gì?

**Thread** là một **dòng điều khiển tuần tự** bên trong một chương trình.

- Một chương trình (process) có thể chứa **nhiều thread**
- Các thread có thể **chạy đồng thời**
- Mỗi thread thực hiện một nhiệm vụ riêng

Thread **không phải** là một chương trình độc lập,  
nó luôn **thuộc về một process**.

---

## Process và Thread

- **Process**:
  - Có không gian bộ nhớ riêng
  - Chạy độc lập với process khác
  - Tạo và chuyển đổi tốn nhiều tài nguyên

- **Thread**:
  - Chia sẻ bộ nhớ với các thread khác trong cùng process
  - Tạo nhanh hơn process
  - Chuyển đổi nhanh hơn

Nhờ thread, chương trình có thể **thực hiện nhiều công việc cùng lúc** trong cùng một ứng dụng.

---

## Vì sao cần lập trình đa tuyến?

Lập trình đa tuyến giúp:

- **Tăng hiệu suất và khả năng đáp ứng**
- **Tận dụng thời gian chờ đợi** (I/O, mạng, tải file)
- **Chia nhỏ tác vụ** và xử lý đồng thời
- **Cải thiện trải nghiệm người dùng**

Trong lập trình mạng, đa tuyến cho phép server **phục vụ nhiều client cùng lúc**.

---

## Thực thi Thread trong Java

Trong Java, một thread bắt đầu chạy khi gọi phương thức `start()`.

- Khi thread chạy, JVM thực thi mã trong phương thức `run()`
- Khi `run()` kết thúc, thread cũng kết thúc
- Một thread **không thể start lại**, muốn chạy lại phải tạo thread mới

Thread cần có **điều kiện dừng rõ ràng** để tránh chạy vô hạn.

---

## Ứng dụng đa tuyến trong lập trình mạng

### Ứng dụng Chat đa tuyến

Một ví dụ tiêu biểu của đa tuyến trong mạng là **ứng dụng chat nhiều client – một server**.

Đặc điểm:

- Server chấp nhận nhiều client kết nối cùng lúc
- Mỗi client được xử lý bởi **một thread riêng**
- Tin nhắn từ một client được gửi tới tất cả client khác
- Server ghi lại nội dung chat vào file log

Thông qua ví dụ này, sinh viên hiểu rõ:
- Cách server xử lý nhiều kết nối đồng thời
- Cách phối hợp giữa socket và thread
- Vai trò của đa tuyến trong ứng dụng mạng

---

## Xử lý đồng thời (Concurrency)

**Xử lý đồng thời** xảy ra khi nhiều thread:

- Chạy cùng lúc
- Truy cập **tài nguyên chung** (biến, file, bộ nhớ)

Nếu không kiểm soát tốt, chương trình có thể gặp các vấn đề nghiêm trọng.

---

## Race Condition

**Race Condition** xảy ra khi:

- Nhiều thread cùng truy cập và thay đổi dữ liệu chung
- Kết quả phụ thuộc vào **thứ tự thực thi không xác định**

Điều này có thể dẫn đến:
- Dữ liệu sai lệch
- Kết quả không nhất quán
- Lỗi khó phát hiện

---

## Đồng bộ hóa (Synchronization)

Để tránh race condition, Java cung cấp cơ chế **đồng bộ hóa**:

- Đảm bảo **chỉ một thread** được truy cập tài nguyên tại một thời điểm
- Giữ cho dữ liệu luôn nhất quán

Đồng bộ hóa là nền tảng để xây dựng các ứng dụng **an toàn và ổn định** khi chạy đa tuyến.

---

## Các vấn đề thường gặp trong xử lý đồng thời

Một số vấn đề phổ biến:

- **Race condition**: tranh chấp dữ liệu
- **Deadlock**: các thread chờ nhau vô hạn
- **Livelock**: các thread liên tục phản ứng nhưng không tiến triển
- **Starvation**: thread không bao giờ được cấp tài nguyên

Những vấn đề này khiến lập trình đa tuyến trở nên **phức tạp và khó debug**.

---

## Lập lịch Thread trong Java

Java sử dụng cơ chế **lập lịch theo độ ưu tiên**:

- Mỗi thread có độ ưu tiên từ `MIN_PRIORITY` đến `MAX_PRIORITY`
- JVM chọn thread runnable có độ ưu tiên cao hơn để chạy
- Thread có thể nhường CPU cho thread khác bằng cách gọi `yield()`

Việc hiểu cơ chế lập lịch giúp lập trình viên **thiết kế thread hợp lý hơn**.

---

## Ý nghĩa của lập trình đa tuyến và xử lý đồng thời

Nắm vững kiến thức bài này giúp bạn:

- Xây dựng server xử lý nhiều client
- Viết ứng dụng mạng hiệu năng cao
- Tránh lỗi tranh chấp dữ liệu
- Chuẩn bị cho các chủ đề nâng cao như:
  - Server đa tuyến
  - Pooling Threads
  - Multicast UDP
  - Java RMI
  - Hệ thống phân tán

---

## Kết luận

Qua bài học này, bạn đã:

- Hiểu khái niệm thread và multithreading
- Phân biệt process và thread
- Biết vai trò của đa tuyến trong lập trình mạng
- Hiểu các vấn đề xử lý đồng thời và race condition
- Chuẩn bị nền tảng cho các bài học nâng cao tiếp theo