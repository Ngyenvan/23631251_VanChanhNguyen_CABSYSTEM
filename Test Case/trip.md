# Test Case - Trip

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-TRIP-01 | Driver cập nhật arrived, picked up, in progress, completed theo thứ tự. | Trip chuyển mốc hợp lệ và Customer theo dõi được. | FR-23 đến FR-27 |
| TC-TRIP-02 | Customer hủy trước picked up / sau picked up. | Trước được ghi nhận; sau bị từ chối trực tiếp. | FR-60, BRULE-15 |
| TC-TRIP-03 | Driver đã xác nhận báo cannot serve trước pickup. | Một RECOVERY session tự mở, loại Driver này. | FR-59, BRULE-14 |
| TC-TRIP-04 | Recovery đã từng mở và Driver khác cannot serve. | Không mở recovery thứ hai. | FR-59, BRULE-14 |
