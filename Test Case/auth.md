# Test Case - Authentication

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-AUTH-01 | Customer hoặc Driver tự đăng ký hợp lệ. | Account được tạo theo role; dữ liệu đầu vào chi tiết không bị suy diễn. | FR-01, FR-04 |
| TC-AUTH-02 | Actor đăng nhập hợp lệ/không hợp lệ. | Hợp lệ được xác thực; không hợp lệ bị từ chối. | FR-02, FR-56 |
