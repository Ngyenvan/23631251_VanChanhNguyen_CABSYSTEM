# Test Case - Booking

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-BKG-01 | Customer gửi Booking đủ pickup, destination, vehicle type. | Booking được nhận và PRIMARY session mở. | FR-11, FR-12, FR-35 |
| TC-BKG-02 | No-driver rồi retry trước 10 giây. | Bị từ chối, không mở session mới. | FR-19, BRULE-13 |
| TC-BKG-03 | Sau 10 giây Customer sửa rồi retry. | RETRY session mới thuộc Booking cũ. | FR-19, BRULE-13 |
| TC-BKG-04 | Customer hủy khi đang điều phối. | Offer mở được thu hồi và audit ghi nhận. | FR-60, FR-58 |
