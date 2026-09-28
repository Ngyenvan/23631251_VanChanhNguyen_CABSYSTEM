# Test Case - Payment

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-PAY-01 | Customer chọn CASH sau Trip hoàn thành. | Chờ Driver xác nhận. | FR-30 |
| TC-PAY-02 | Driver đúng Trip xác nhận CASH. | Kết quả được ghi nhận và Customer nhận thông báo. | FR-30, FR-39 |
| TC-PAY-03 | Provider gửi SUCCESS rồi callback trùng/muộn. | Chỉ một kết quả cuối; callback trùng không tạo kết quả thứ hai. | FR-32 |
| TC-PAY-04 | ELECTRONIC FAILED rồi Customer retry cùng Trip. | Retry được xử lý theo chính sách; không suy diễn giới hạn. | FR-33, FR-34 |
