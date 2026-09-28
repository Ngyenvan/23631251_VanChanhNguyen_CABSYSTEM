# Test Case — Notification

| ID | Tiền điều kiện và thao tác | Kết quả mong đợi | Trace |
| --- | --- | --- | --- |
| TC-NOTIF-01 | Customer tạo Booking, sau đó Driver được xác nhận. | Customer nhận thông báo Booking received và Driver confirmed đúng Booking/session. | FR-35, FR-36 |
| TC-NOTIF-02 | Session kết thúc no-driver; hoặc CAB System mở RECOVERY Session. | Customer nhận đúng thông báo no-driver hoặc recovery started. | FR-36 |
| TC-NOTIF-03 | Driver nhận offer, offer hết 20 giây hoặc Trip đang thực hiện thay đổi. | Driver nhận thông báo offer, offer expired và active-trip change tương ứng. | FR-40 |
| TC-NOTIF-04 | Driver cập nhật đến pickup/completed; hoặc Payment có kết quả. | Customer nhận thông báo arrived, Trip completed và payment result tương ứng. | FR-37–FR-39 |
| TC-NOTIF-05 | Kênh gửi thông báo lỗi sau khi Booking/Trip/Payment đã đổi trạng thái hợp lệ. | Lỗi được quan sát/lưu theo cơ chế kỹ thuật nhưng không đảo trạng thái nghiệp vụ. Không giả định retry hoặc fallback. | FR-35–FR-40, OI-12 |
