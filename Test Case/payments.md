# Test Case — Fare and payment

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-PAY-01 | Trip hoàn thành, CAB System xác định cước rồi Customer xem cước. | Có số tiền phải trả gắn đúng Trip. Không kiểm tra công thức cước. | FR-28, FR-29 |
| TC-PAY-02 | Customer chọn CASH cho Trip hoàn thành. | Giao dịch chờ Driver xác nhận; chưa là thanh toán hoàn thành. | FR-30 |
| TC-PAY-03 | Driver đúng của Trip xác nhận đã nhận tiền CASH. | Giao dịch được ghi nhận hoàn thành và Customer nhận thông báo kết quả. | FR-30, FR-39 |
| TC-PAY-04 | Customer chọn ELECTRONIC cho Trip hoàn thành. | Tạo xử lý qua External Payment Provider, không lưu trực tiếp thông tin thẻ/tài khoản nhạy cảm. | FR-31, BRULE-07 |
| TC-PAY-05 | Provider gửi callback SUCCESS hợp lệ rồi gửi trùng cùng callback. | Kết quả cuối cùng chỉ được ghi nhận một lần; callback trùng trả trạng thái đã lưu, không tạo giao dịch/thông báo thứ hai. | FR-32, FR-39 |
| TC-PAY-06 | Provider gửi callback mâu thuẫn hoặc muộn sau khi Payment đã có kết quả cuối cùng. | `409` hoặc trạng thái cuối giữ nguyên; không đảo kết quả đã chốt. | FR-32 |
| TC-PAY-07 | Provider trả FAILED. | Payment thất bại được ghi nhận và Customer nhận thông báo thất bại. | FR-33, FR-39 |
| TC-PAY-08 | Payment điện tử thất bại; Customer bắt đầu xử lý lại trên cùng Trip. | Cho phép tạo lần xử lý mới trên cùng Trip theo chính sách ABC; không khẳng định điều kiện hay số lần retry. | FR-34, BRULE-08 |
| TC-PAY-09 | Payment thất bại nhưng Trip đã hoàn thành. Customer xem lịch sử và gửi đánh giá. | Lịch sử/đánh giá vẫn khả dụng, không phụ thuộc thanh toán thành công. | FR-41, FR-42 |
