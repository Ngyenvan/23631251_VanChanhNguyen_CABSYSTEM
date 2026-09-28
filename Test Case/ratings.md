# Test Case — Rating

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-RATE-01 | Customer của Trip đã COMPLETED gửi đánh giá Driver. | Đánh giá được lưu cho Driver của Trip. | FR-42, BRULE-06 |
| TC-RATE-02 | Customer thử đánh giá Trip chưa COMPLETED. | `409`; không lưu đánh giá. | FR-42 |
| TC-RATE-03 | Customer khác hoặc Driver gửi đánh giá cho Trip. | `403`; không lưu đánh giá. | FR-42, FR-56 |

Không tạo test về thang điểm, đánh giá trùng hoặc thời hạn vì OI-10 chưa chốt.
