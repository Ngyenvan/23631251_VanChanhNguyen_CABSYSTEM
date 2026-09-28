# Test Case — Authentication

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-AUTH-01 | Gửi đăng ký hợp lệ với role `CUSTOMER`. | Tạo Customer account. | FR-01 |
| TC-AUTH-02 | Gửi đăng ký hợp lệ với role `DRIVER`. | Tạo Driver account tự đăng ký. | FR-04 |
| TC-AUTH-03 | Gửi đăng ký thiếu trường bắt buộc theo schema. | `400`; không tạo account. | FR-01, FR-04 |
| TC-AUTH-04 | Đăng nhập bằng thông tin hợp lệ của Customer hoặc Driver. | Có token và account tương ứng; có thể dùng cho chức năng cần xác thực. | FR-02, FR-56, BRULE-01 |
| TC-AUTH-05 | Gọi chức năng cần account với token thiếu/không hợp lệ. | `401`; không thay đổi nghiệp vụ. | FR-56, BRULE-01 |

Định dạng định danh/mật khẩu, khóa tài khoản và thời hạn token không được kiểm thử vì thuộc OI-18.
