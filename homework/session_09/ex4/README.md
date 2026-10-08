# Bài 4: Lưu trữ dữ liệu đơn giản với Docker Volume

## 1. Mục tiêu
Tạo một Named Volume để lưu trữ tệp dữ liệu bền vững giữa các lần chạy Container, chứng minh dữ liệu lưu trong Volume không bị mất khi container dừng và bị xóa.

---

## 2. Các bước thực hiện & Lệnh đã sử dụng

### Bước 1: Tạo Named Volume mới
Tạo một Docker Volume có tên là `easy-volume`:
```bash
docker volume create easy-volume
```

### Bước 2: Kiểm tra danh sách Volume trên máy
Xác nhận rằng `easy-volume` đã được tạo thành công:
```bash
docker volume ls
```

### Bước 3: Ghi dữ liệu vào Volume bằng Container Alpine thứ nhất
Khởi chạy một container Alpine tạm thời (tự động xóa sau khi chạy xong với flag `--rm`), mount volume `easy-volume` vào thư mục `/data` và ghi nội dung vào file `test.txt`:
```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```

### Bước 4: Đọc lại dữ liệu bằng Container Alpine thứ hai
Khởi chạy một container Alpine hoàn toàn mới, mount volume `easy-volume` vào thư mục `/data` và đọc nội dung file `test.txt`:
```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```

---

## 3. Kết quả kiểm tra

- **Lệnh kiểm tra:**
  ```bash
  docker run --rm -v easy-volume:/data alpine cat /data/test.txt
  ```

- **Kết quả mong đợi (Terminal Output):**
  ```text
  Luu tru du lieu Docker
  ```

---

## 4. Giải thích cơ chế hoạt động
- **Named Volume (`easy-volume`)**: Được Docker quản lý độc lập với vòng đời của container.
- Container thứ nhất sau khi thực thi lệnh `echo` sẽ tự hủy (`--rm`), nhưng dữ liệu trên volume `easy-volume` vẫn được giữ nguyên.
- Container thứ hai khi mount volume `easy-volume` vào đường dẫn `/data` có thể truy cập và đọc tệp `/data/test.txt` đã được tạo trước đó.
