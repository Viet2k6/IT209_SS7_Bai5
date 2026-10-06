# Session 07 - Exercise 5: Troubleshoot Port 8082

## 1. Process chiếm port

Lệnh kiểm tra:

```bash
sudo ss -tlnp | grep 8082
```

Kết quả:

```text
LISTEN 0 5 0.0.0.0:8082 0.0.0.0:* users:(("python3",pid=4841,fd=3))
```

Process đang chiếm port `8082`:

* Process: `python3`
* PID: `4841`
* Port: `8082`

## 2. Nguyên nhân

Port `8082` đang bị process `python3` sử dụng nên Spring Boot application không thể bind vào port này.

## 3. Xử lý

Đã xác định PID `4841` bằng lệnh `ss` và sẽ dừng process này bằng `kill`.

## 4. Khởi động lại Spring Boot

Sau khi giải phóng port `8082`, khởi động lại:

```bash
sudo systemctl start spring-app.service
```

Kiểm tra:

```bash
sudo ss -tlnp | grep 8082
```

Kết quả cuối cùng phải là Java process của `spring-app.service`.
## 5. Kết quả sau khi xử lý

Sau khi dừng process `python3` với PID `4841`, Spring Boot service được khởi động lại thành công.

Lệnh kiểm tra:

sudo ss -tlnp | grep 8082
```

Kết quả:

LISTEN 0 100 *:8082 *:* users:(("java",pid=4941,fd=9))
```

Port `8082` đã được giải phóng khỏi process gây xung đột và được Spring Boot application quản lý lại bằng Java.
