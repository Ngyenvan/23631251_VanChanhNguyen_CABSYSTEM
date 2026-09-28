# Test Case — Reporting

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-REP-01 | Dữ liệu mẫu có Trip và Dispatch Session trong kỳ. | Báo cáo trả số Trip và số session. | FR-51 |
| TC-REP-02 | Dữ liệu mẫu có doanh thu, Trip hoàn thành và hủy. | Báo cáo trả doanh thu, tỷ lệ hoàn thành và tỷ lệ hủy; công thức/kỳ dùng định nghĩa ABC khi có. | FR-52–FR-54, OI-09 |
| TC-REP-03 | Dữ liệu mẫu có kết quả offer/session/retry/recovery. | Có hiệu quả Driver và các chỉ số confirmation, no-driver, retry, recovery. | FR-55 |
| TC-REP-04 | Người không có quyền khai thác báo cáo. | `403`; không lộ dữ liệu báo cáo. | FR-57 |
