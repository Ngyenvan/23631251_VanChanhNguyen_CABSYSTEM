# Test Case - Notification

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace SRS |
| --- | --- | --- | --- |
| TC-NOT-01 | Booking received, Driver confirmed, no-driver hoặc recovery. | Customer nhận đúng thông báo mốc. | FR-35, FR-36 |
| TC-NOT-02 | Offer/offer expiry/trip changes/payment result. | Driver hoặc Customer đúng recipient nhận thông báo. | FR-37 đến FR-40 |
| TC-NOT-03 | Channel lỗi sau thay đổi nghiệp vụ hợp lệ. | Không rollback Booking, Dispatch, Trip hoặc Payment. | NFR-02, OI-12 |
