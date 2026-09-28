# Test Case — Pricing

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-PRICE-01 | Driver cập nhật Trip COMPLETED. | CAB System bắt đầu xác định cước sau hoàn thành. | FR-27, FR-28 |
| TC-PRICE-02 | Customer xem cước của Trip đã hoàn thành và đã có cước. | Nhận đúng số tiền của Trip đó. | FR-29 |
| TC-PRICE-03 | Yêu cầu xác định/xem cước trước hoàn thành hoặc khi chính sách cước chưa sẵn sàng. | `409`; không tự tạo cước. | FR-28, OI-01, OI-15 |
