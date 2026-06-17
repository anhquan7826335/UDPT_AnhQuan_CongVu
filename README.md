# KeyDB - Bài tập lớn môn Hệ phân tán

## Giới thiệu

Đây là bài tập lớn môn **Hệ phân tán**. Nhóm lựa chọn **KeyDB** làm dự án nghiên cứu nhằm tìm hiểu kiến trúc của hệ quản trị cơ sở dữ liệu dạng Key-Value và phát triển thêm các tính năng liên quan đến xử lý phân tán.

KeyDB là một hệ quản trị cơ sở dữ liệu NoSQL mã nguồn mở, được phát triển từ Redis với khả năng xử lý đa luồng (Multi-threading), hiệu năng cao và tương thích hoàn toàn với giao thức Redis.

---

# Thành viên

| Họ và tên   | Công việc                                                                                           |
| ----------- | --------------------------------------------------------------------------------------------------- |
| Lê Công Vũ  | Nghiên cứu kiến trúc KeyDB, cài đặt môi trường, phát triển tính năng mới, kiểm thử và viết báo cáo. |
| Lê Anh Quân | Nghiên cứu kiến trúc KeyDB, cài đặt môi trường, phát triển tính năng mới, kiểm thử và viết báo cáo. |

---

# Mục tiêu

* Tìm hiểu kiến trúc hoạt động của KeyDB.
* Biên dịch KeyDB từ mã nguồn.
* Chạy thành công KeyDB trên Ubuntu (WSL).
* Hiểu cơ chế lưu trữ dữ liệu và xử lý đồng thời.
* Phát triển thêm hai tính năng mới phục vụ môi trường phân tán.

---

# Công nghệ sử dụng

* C++
* KeyDB
* Ubuntu 22.04 (WSL)
* Docker
* Git & GitHub

---

# Cài đặt

## Clone mã nguồn

```bash
git clone https://github.com/EQ-Alpha/KeyDB.git
cd KeyDB
git submodule update --init
```

## Cài đặt thư viện

```bash
sudo apt update

sudo apt install build-essential \
autoconf \
autotools-dev \
libjemalloc-dev \
tcl \
tcl-dev \
uuid-dev \
libcurl4-openssl-dev \
libbz2-dev \
libzstd-dev \
liblz4-dev \
libsnappy-dev \
libssl-dev \
nasm
```

## Biên dịch

```bash
make -j$(nproc)
```

---

# Chạy KeyDB

Khởi động Server

```bash
cd src

./keydb-server --port 6380
```

Mở Client

```bash
./keydb-cli -p 6380
```

Kiểm tra

```text
127.0.0.1:6380> ping

PONG
```

---

# Tính năng mới

## 1. Distributed Lock

### Mục đích

Cho phép một máy chủ giành quyền sử dụng tài nguyên trước các máy chủ khác.

Nếu tài nguyên đã được khóa thì những server khác sẽ không thể truy cập.

### Cú pháp

```text
acquire_lock <resource> <ttl_ms>
```

Ví dụ

```text
acquire_lock lock1 5000
```

Kết quả

```text
OK
```

Nếu tài nguyên đã được khóa

```text
(error) ERR BUSY:Tài Nguyên này đang bị khóa bởi server khác
```

### Ý nghĩa

* Tránh nhiều server ghi cùng lúc.
* Đồng bộ truy cập tài nguyên.
* Mô phỏng cơ chế Distributed Lock.

---

## 2. Generate Distributed ID

### Mục đích

Sinh ID duy nhất cho các bản ghi trong hệ thống phân tán.

### Cú pháp

```text
generate_id invoice
```

Ví dụ

```text
generate_id invoice
```

Kết quả

```text
ID_1749945123123_1
ID_1749945125189_2
ID_1749945126345_3
```

### Ý nghĩa

* Không bị trùng ID.
* Có thể sử dụng cho hóa đơn, đơn hàng hoặc giao dịch.

---

# Demo

## Kiểm tra Distributed Lock

```text
127.0.0.1:6380> acquire_lock lock1 5000

OK

127.0.0.1:6380> acquire_lock lock1 5000

(error) ERR BUSY:Tài Nguyên này đang bị khóa bởi server khác
```

---

## Kiểm tra Generate ID

```text
127.0.0.1:6380> generate_id invoice

ID_1749945123123_1

127.0.0.1:6380> generate_id invoice

ID_1749945125189_2
```

---

# Kiến thức thu được

Sau khi hoàn thành bài tập lớn, nhóm đã:

* Hiểu kiến trúc của KeyDB.
* Nắm được quá trình biên dịch dự án mã nguồn mở.
* Hiểu cách đăng ký và bổ sung một lệnh mới trong KeyDB.
* Tìm hiểu cơ chế xử lý đa luồng của KeyDB.
* Hiểu nguyên lý Distributed Lock trong hệ phân tán.
* Hiểu cách sinh ID duy nhất trong môi trường phân tán.

---


