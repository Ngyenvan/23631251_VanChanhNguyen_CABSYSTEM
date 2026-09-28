# Test Case — Operations Staff

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-OPS-01 | Operations Staff có quyền tạo account Driver. | Tạo được account; người không đủ quyền nhận `403`. | FR-05, FR-57 |
| TC-OPS-02 | Operations Staff có quyền xem Customer, Driver, vehicle và Trip. | Nhận dữ liệu quản trị/tra cứu phù hợp quyền; không suy diễn các thao tác ghi chưa được SRS mô tả. | FR-43–FR-46, FR-57 |
| TC-OPS-03 | Có Trip đang diễn ra/Dispatch Session hoặc Driver vị trí cũ. | Operations Staff xem được Trip/session liên quan, trạng thái Driver và độ mới vị trí. | FR-47, FR-48 |
| TC-OPS-04 | Có Trip lỗi. Operations Staff xem support context và audit logs. | Chỉ hỗ trợ/tra cứu trong quyền; không có hành động gán Driver cho Booking thất bại. | FR-49, FR-58, BRULE-14 |
| TC-OPS-05 | Operations Staff tra cứu giao dịch. | Chỉ nhận lịch sử transaction theo quyền. | FR-50, FR-57 |
