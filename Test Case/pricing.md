# Test Case - Pricing

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-PRICE-01 | Trip COMPLETED. | CAB System mới xác định cước. | FR-27, FR-28 |
| TC-PRICE-02 | Yêu cầu cước trước completed. | Bị từ chối, không tạo Fare. | FR-28 |

Không kiểm tra công thức cước vì OI-01/OI-15.
