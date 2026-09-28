# Test Case — Customer account

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-USR-01 | Customer đã xác thực xem `/users/me`. | Chỉ nhận account của chính Customer. | FR-02, FR-56 |
| TC-USR-02 | Customer đã xác thực cập nhật thông tin được phép. | Dữ liệu cập nhật phản ánh trên account. | FR-03 |
| TC-USR-03 | Driver dùng endpoint cập nhật Customer hoặc Customer dùng account khác. | `403`; không sửa dữ liệu. | FR-03, FR-56 |
